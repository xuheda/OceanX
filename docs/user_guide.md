# OceanX 用户使用指南

更新日期：2026-07-24

[返回文档首页](index.md)

本文面向使用 OceanX 进行场景仿真、车辆控制、数据采集和二次配置的用户。内部动力学、线程和通信实现见 [技术文档索引](index.md#核心文档)。

## 1. OceanX 运行时由什么组成

OceanX 的正常闭环由三个运行部分组成：

```text
ROS 2 任务/控制节点
  <-> MicroXRCEAgent（主机共享，一个）
  <-> HydroX SITL（每辆车一个）
  <-> MAVLink HIL
  <-> Unreal Engine / OceanX（场景、动力学、传感器和显示）
```

- Unreal Engine 负责场景、车辆生成、固定步长动力学、传感器、显示和外部进程编排。
- HydroX SITL 负责导航估计、制导、控制和执行器分配。
- MicroXRCEAgent 负责 HydroX 与 ROS 2 DDS 网络之间的桥接。
- ROS 2 负责任务、航点、编队、追踪、外部策略和工具，不直接修改 UE Actor。

Editor Play、Headless 和独立打包版使用同一套运行逻辑：UE 启动或复用共享 Agent，再为场景中的每辆车启动一个 HydroX SITL。仅打开 ROS 2 节点不能代替 UE 仿真。

## 2. 使用范围与目录

本页以**已打包运行程序**的使用和配置为主。当前发布目标为 Win64：解压完整发布目录后即可运行，不要求安装 Unreal Engine、Visual Studio 或 CMake。请保留程序目录、`scenarios/`、`configs/vehicle_profiles/` 及随包许可证文件之间的相对位置；不要只复制 `OceanX.exe`。

普通使用者通常只编辑以下松散配置：

```text
<OceanXRoot>/
├─ scenarios/                      场景和车辆实例
├─ configs/vehicle_profiles/       车辆、传感器和执行器 Profile
└─ <UE ProjectDir>/                打包后的 Unreal 程序目录
```

不要把运行生成物、构建缓存或日志写回 `scenarios/`、`configs/vehicle_profiles/`。其它源码目录、构建工具链和 ROS overlay 属于开发者工作区；需要从源码构建、在 Linux 上构建或调整 UE 插件时，请使用 [构建与运行](internal/build_and_run.md)。

## 3. 运行前准备

### 3.1 Windows 打包程序

- 解压整个发布目录，不要单独移动可执行文件、`Engine/`、项目目录或外置配置。
- 从发布目录启动程序；需要指定配置根目录时，设置 `OCEANX_ROOT`，并通过 `-OceanXScenario` 指定场景，见第 7 节。
- 只有需要任务、控制服务或 ROS 工具时才配置 ROS 2；纯本地场景仿真不以启动 ROS 节点代替 UE 程序。
- 建议使用系统 `cmd.exe` 或 Windows PowerShell 5.1 启动带有 BAT 包装器的开发者工具。环境变量语法不要混用：`cmd.exe` 使用 `set ROS_DOMAIN_ID=0`，PowerShell 使用 `$env:ROS_DOMAIN_ID="0"`。

### 3.2 从源码运行（开发者）

从源码启动需要 Unreal Engine 5.3、C++ 工具链、CMake，以及在使用 ROS/HydroX 时匹配的 ROS 2 Humble 环境。Windows 和 Linux 的构建顺序、第三方 Agent 版本和 Headless 入口由 [构建与运行](internal/build_and_run.md) 维护；本页不重复这些命令，避免运行说明和构建说明漂移。

## 4. 在 UE Editor 中运行（开发者）

1. 打开 [engine/OceanX.uproject](../engine/OceanX.uproject)。
2. 确认地图为 `OceanExampleLevel`；它也是当前 Editor 和 Game 的默认地图。发布包会同时包含
   `OceanExampleLevel`、`OceanSandHeightmap` 和 `OceanSeaSurface` 三个可选关卡。
3. 按 **Play**。
4. 等待场景车辆出现，UE 会自动启动或复用 MicroXRCEAgent，并为每辆车启动 HydroX SITL。

默认场景为：

```text
scenarios/vehicle_test_4types.json
```

包含四辆位于 NED 深度 200 m 的车辆：

| 车辆名 | Profile | 初始位置 `[N,E,D]` m |
|---|---|---|
| `ecaa9_test` | `EcaA9` | `[-300, 0, 200]` |
| `lauv_test` | `LAUV` | `[-100, 0, 200]` |
| `desistek_test` | `DesistekSaga` | `[100, 0, 200]` |
| `rexrov2_test` | `RexROV2` | `[300, 0, 200]` |

如果需要在打开 Editor 前指定 ROS domain，可从同一个 cmd.exe 启动：

```bat
set ROS_DOMAIN_ID=0
"C:\Program Files\Epic Games\UE_5.3\Engine\Binaries\Win64\UnrealEditor.exe" "%CD%\engine\OceanX.uproject"
```

停止 Play 时，本次场景拥有的 HydroX SITL 会退出；共享 MicroXRCEAgent 保持运行，供下次 Play 复用。UE 异常退出时，HydroX 通过父进程守护自行退出。

### 4.1 紧急停机

在 UE 控制台输入以下命令可立即锁存执行器为零；HydroX 随后发来的 actuator 包会被拒绝，心跳和传感器链路仍保持运行：

```text
OceanX.EmergencyStop
OceanX.EmergencyStop 2
```

第一条命令作用于全部车辆，第二条只作用于 vehicle index `2`。停机是锁存的，必须显式解除：

```text
OceanX.ClearEmergencyStop
OceanX.ClearEmergencyStop 2
```

若全局停机仍处于锁存状态，不能只解除单艇；应先使用 `OceanX.ClearEmergencyStop` 解除全局锁存。解除后控制器需发送新的 HIL actuator 包，车辆才会再次受控。

## 5. Headless 运行（开发者）

Windows 默认运行 120 个仿真秒：

```bat
run_ue_headless_sitl.bat
```

指定场景和仿真时长：

```bat
run_ue_headless_sitl.bat scenarios\vehicle_test_4types.json 180
```

如果 UE 不在脚本已知位置：

```bat
set "UE_EDITOR_EXE=D:\UE_5.3\Engine\Binaries\Win64\UnrealEditor.exe"
run_ue_headless_sitl.bat scenarios\vehicle_test_4types.json 180
```

Headless 使用 `-game -nullrhi -unattended`，但仍运行完整的 UE、动力学、传感器、CommBridge 和 HydroX 链路。日志写入：

```text
.ros/log/ue_headless_sitl.log
```

时长参数是 OceanX 仿真时间，不是墙钟时间。机器无法保持实时运行时，现实等待时间可能更长。

## 6. 独立打包版

### 6.1 外置配置是发布契约

`scenarios/` 和 `configs/vehicle_profiles/` 有意保持为用户可编辑的松散文件，不应封入 PAK。发布目录必须保留以下关系：

```text
<OceanXRoot>/
├─ <UE ProjectDir>/               UE 打包项目目录
├─ scenarios/
│  └─ vehicle_test_4types.json
└─ configs/
   └─ vehicle_profiles/
      ├─ EcaA9.json
      ├─ LAUV.json
      └─ ...
```

默认情况下，OceanX 从 UE `ProjectDir` 的上一级寻找这两个目录。制作发行包时，需要把它们作为松散文件复制到发布根目录；标准 UE cook 不会替发行流程完成这一步。

推荐显式设置根目录并显式传入场景，避免安装位置变化造成歧义：

```bat
set "OCEANX_ROOT=D:\OceanXRuntime"
OceanX.exe -OceanXScenario="D:\OceanXRuntime\scenarios\vehicle_test_4types.json"
```

### HIL TCP 安全与远程飞控

默认情况下，OceanX 仅在 `127.0.0.1` 监听 HydroX HIL TCP 端口，因此本机
自动启动的 HydroX SITL 无需额外配置。不要将此端口暴露到局域网。

若 UE 与飞控确实运行在不同机器，必须同时显式指定 UE 的绑定地址、可连接
飞控的 IP 白名单、**32 字节 MAVLink 2 签名密钥文件**和链路 ID 基数；缺少任一
参数时场景启动会失败。密钥文件是仅含 64 个十六进制字符的文本文件，不要把密钥
写进命令行、场景文件或日志。

```powershell
OceanX.exe -OceanXHilRemote `
  -OceanXHilBindHost=192.168.10.20 `
  -OceanXHilAllowedPeers=192.168.10.31 `
  -OceanXHilSigningKeyFile=D:\OceanXSecrets\hil_signing.key `
  -OceanXHilSigningLinkIdBase=16
```

`-OceanXHilRemote` 不会自动启动本机 HydroX。把同一密钥通过受控的带外方式放到
白名单飞控机器（限制该文件的读取权限），再手动启动对应车辆的 HydroX：

```powershell
hydrox_sitl --ue5-host 192.168.10.20 --ue5-port 14600 `
  --mavlink-signing-key-file D:\OceanXSecrets\hil_signing.key `
  --mavlink-signing-link-id 16
```

每辆车使用 `link_id = OceanXHilSigningLinkIdBase + VehicleIndex`，因此第二辆车的
端口和 ID 依次为 `14601`、`17`。远程端点会拒绝未签名、签名错误、链路 ID 不符或
已重放的 MAVLink 包；错误包不会触发可被攻击者滥用的 failsafe。多个受信任飞控
地址仍用逗号分隔。

MAVLink 签名提供来源认证、完整性和重放防护，但**不加密** TCP/HIL 载荷，也不能
消除网络层拒绝服务风险。跨不可信网络、跨公网或无法信任二层局域网时，仍必须在
防火墙基础上使用 WireGuard/VPN 或 mTLS 隧道；不要直接暴露 HIL TCP 端口。

Linux：

```bash
export OCEANX_ROOT=/opt/oceanx
./OceanX -OceanXScenario=/opt/oceanx/scenarios/vehicle_test_4types.json
```

`OCEANX_ROOT` 用于解析 `configs/vehicle_profiles/`；`-OceanXScenario` 明确指定本次场景。不要把 Profile 文件名写成绝对路径，场景中只写 Profile 简单名称。

### 6.2 流场开关与 DVL 跟踪

流场默认**关闭**，因此不改变旧场景的动力学或传感器结果。打开时，场景会在启动或
Soft Reset 前加载一份只读流场，并共享给所有 Fossen 载具。

源码工作区在未传入 `-OceanXCurrentField` 时使用
`engine/Content/OceanX/Environment/default_current.json`。当前 Win64 打包候选包**不**把
该 JSON 作为松散运行时文件暂存；因此只传 `-OceanXCurrentFieldEnabled=1` 的打包程序会因
找不到默认配置而拒绝启动。要在打包程序启用流场，必须由打包者另外提供完整的外置 JSON
及其引用的 OCF1 文件，并使用绝对路径传入。

```powershell
# 源码工作区：使用 ProjectContentDir 下的内置示例
OceanX.exe -OceanXCurrentFieldEnabled=1

# 打包程序：使用发行方明确附带的外置配置
OceanX.exe -OceanXCurrentFieldEnabled=1 `
  -OceanXCurrentField="D:\OceanXRuntime\currents\my_current.json"

# 强制关闭（即使命令行或配置包装器同时传入了开启项）
OceanX.exe -OceanXCurrentFieldEnabled=0
```

JSON 使用 NED：位置 `[north, east, down]` 单位 m，速度 `[north, east, down]` 单位 m/s，
时间是 OceanX 仿真时间。它可组合 `base_velocity_ned_mps`、深度 `layer`、周期 `tide`、
局部 `vortices` 和可选规则 `grid`。配置加载失败会阻止场景启动，避免只部分车辆受流。

`grid.file` 使用二进制 `OCF1`，运行时进行空间三线性和时间线性插值；范围外策略为
`clamp` 或 `zero`。把规则 CSV（列为 `time_s,north_m,east_m,depth_m,u_mps,v_mps,w_mps`）
转为 OCF1（仅源码开发者或打包者）：

```bat
python tools\compile_ocean_current_grid.py measurements.csv engine\Content\OceanX\Environment\survey.ocf
```

开启流场后，Fossen 水动力使用载具位置和当前仿真时间的水速，物理快照同时发布水相对
速度给 DVL。DVL Profile 的 `configuration.mode` 支持：

- `bottom_track`：原有海床波束跟踪；
- `water_track`：无底锁时也可测水相对速度，但流场关闭或配置禁用时输出无效；
- `auto`：先尝试底跟踪，底锁不足时自动回退水体跟踪。

`HIL_DVL`（自定义 MAVLink 消息 `11060`）使用固定 26 字节协议：时间戳、FRD 速度、
`altitude_m`、`tracking_mode` 和有效位。`tracking_mode` 为 `0=unavailable`、
`1=bottom_track`、`2=water_track`；有效位分别表示速度和高度。水体跟踪的
`altitude_m` 固定为 `NaN`，且高度有效位为 0。HydroX 只把底跟踪速度融合进现有 EKF；
水体速度与 GPS 速度或近时刻底跟踪共同约束对地速度和 NED 流速；只有水体 DVL 而没有
这些参考时，流速与对地速度不可完全观测，`current_valid` 会反映其可信度。完整融合规则见
[HydroX 控制架构](internal/hydrox_control_architecture.md)。

该报文已原地升级，不兼容旧的 25 字节 `HIL_DVL` 负载；OceanX 与 HydroX 必须一起更新。
当前运行时 allowlist 只承诺场景、Vehicle Profile 和许可证等松散文件；要把流场作为正式
用户功能发布，打包规则必须另行纳入 JSON 与 OCF1，并在干净机器上验证其绝对路径。

#### 流场可视化

可视化是独立的渲染诊断层，默认关闭，不影响动力学、DVL 或仿真确定性。它必须与流场
一起开启：

```powershell
OceanX.exe -OceanXCurrentFieldEnabled=1 -OceanXCurrentFieldVisualize -OceanXCurrentFieldVisualizeParticles
```

此时会围绕第一艘载具，在指定 NED 深度绘制流向箭头（青色慢、红色快）、随流移动的粒子点，
并显示该艇当前位置的 NED 水速和 DVL 跟踪来源。可选参数：

```text
-OceanXCurrentFieldVisualizeDepthM=5
-OceanXCurrentFieldVisualizeSpacingM=15
-OceanXCurrentFieldVisualizeRadius=4
```

`Radius=4` 表示 `9 x 9` 个采样箭头；粒子只有在
`-OceanXCurrentFieldVisualizeParticles` 存在时绘制。运行中可在 UE 控制台输入
`OceanX.CurrentFieldVisualization on|off|toggle`，或
`OceanX.CurrentFieldVisualizationDepth <depth_m>` 改变深度。该诊断绘制面向 Editor 和
Development 构建，正式 Shipping 版不应依赖它作为用户功能。

### 6.3 包内运行时（打包者）

打包前必须先构建对应平台的 HydroX 和 MicroXRCEAgent。UE 打包规则会把以下平台运行时加入包：

```text
ThirdParty/HydroX/hydrox_sitl[.exe]
ThirdParty/DDS/MicroXRCEAgent[.exe]
ThirdParty/DDS/<平台 DDS 动态库>
```

缺少这些文件时，打包会直接失败并提示应执行的构建脚本。

## 7. 启动 ROS 2

ROS 2 与 UE/HydroX 必须使用同一个 `ROS_DOMAIN_ID`。默认值为 `0`。

打包版的 `T` ROS Topic Monitor 会自行发现本机/安装包中的 ROS overlay，
并启动一个常驻、只读的图查询 helper；直接双击 OceanX.exe 时不需要先在
终端执行 `setup.bat`。本节的环境加载仍是启动外部 `ros2 launch`、`ros2 topic`
或其他 ROS 工具时所必需的步骤。

Windows 源码工作区：

```bat
call ros\install\setup.bat
set ROS_DOMAIN_ID=0
```

当前统一启动入口是 `oceanx_bringup`。默认场景的车辆名不是 `auv0`，因此必须显式传入实际车辆名：

```bat
ros2 launch oceanx_bringup oceanx_bringup.launch.py ^
  vehicle_names:=ecaa9_test,lauv_test,desistek_test,rexrov2_test ^
  start_fleet_manager:=false ^
  start_formation:=false ^
  start_pursuit:=false
```

确认 HydroX 状态已经进入 ROS 2 网络：

```bat
ros2 topic list | findstr /I /C:"/hydrox/" /C:"/oceanx/"
ros2 topic echo --qos-reliability best_effort /hydrox/ecaa9_test/out/auv_state
```

给 `ecaa9_test` 发送一个向 North 正方向移动 50 m、保持 200 m 深度的单航点任务：

```bat
ros2 action send_goal /oceanx/ecaa9_test/mission/follow_waypoints ^
  oceanx_interfaces/action/FollowWaypoints ^
  "{vehicle_name: 'ecaa9_test', waypoints: [{x: -250.0, y: 0.0, z: 200.0}], speed: 2.0, radius: 5.0, loop: false}" ^
  --feedback
```

控制链路必须保持单写者：

```text
/oceanx/{vehicle}/control/setpoint_sources/{source}
  -> setpoint_mux
  -> /hydrox/{vehicle}/in/setpoint
```

不要让任务节点、手动节点或策略节点绕过 `setpoint_mux`，直接同时写入 HydroX 最终 setpoint。

当前 ROS workspace 包含：

- `oceanx_interfaces`
- `oceanx_ros_support`
- `hydrox_xlog`
- `hydrox_ros2`
- `oceanx_control`
- `oceanx_coordination`
- `oceanx_bringup`

打包程序不自带可供 `ros2 launch` 使用的 ROS overlay。若要使用外部 ROS 工具，请先安装 ROS 2 Humble，并从兼容的 OceanX ROS 工作区加载 overlay；开发者工作区可使用上面的 `ros\\install\\setup.bat`。完整 topic、service、action 和 QoS 契约见 [HydroX 控制架构与话题契约](internal/hydrox_control_architecture.md)。

## 8. 修改场景

建议复制现有场景后修改，不直接覆盖示例文件。最小场景：

```json
{
  "schema_version": 2,
  "comment": "My first OceanX scenario",
  "vehicles": [
    {
      "name": "auv_demo",
      "vehicle_profile": "EcaA9",
      "location": [0.0, 0.0, 165.0],
      "rotation": [0.0, 0.0, 170.0]
    }
  ]
}
```

规则：

- `name` 在同一场景中必须唯一，也是 ROS topic 中的 `{vehicle}`。
- `vehicle_profile` 只写文件基本名，例如 `EcaA9`，不要写路径或 `.json`。
- `location` 使用 NED `[north, east, down]`，单位 m；水下深度为正数。
- `rotation` 使用 `[roll, pitch, yaw]`，单位 deg。
- 场景只负责车辆实例和初始位姿，不在里面配置传感器、端口、控制模式或训练参数。

当前示例还包括空中车辆场景：

```text
scenarios/vehicle_test_air_3types.json
```

其中 `D=-30 m` 表示平均海平面上方 30 m。

## 9. 修改 Vehicle Profile

Vehicle Profile 位于：

```text
configs/vehicle_profiles/*.json
```

建议创建新文件，例如 `MyEcaA9.json`：

```json
{
  "schema_version": 1,
  "vehicle_profile": "MyEcaA9",
  "vehicle_type": "EcaA9",
  "sensor_suite": [
    {
      "sensor_type": "ImuSensor",
      "sensor_name": "imu",
      "mount_frd": {
        "location_m": [0.0, 0.0, 0.0],
        "rotation_rpy_deg": [0.0, 0.0, 0.0]
      },
      "configuration": {
        "sample_rate_hz": 100
      }
    }
  ]
}
```

必须满足：

- `vehicle_profile` 与文件名一致。
- `vehicle_type` 对应 UE 已实现车型。
- `sensor_suite` 非空，传感器名称在本车内保持唯一。
- `mount_frd` 使用机体系 FRD：X 前、Y 右、Z 下。
- 支持的固定采样率为 `100, 50, 25, 20, 10, 5, 4, 2, 1 Hz`。

当前内置 Profile：

| 水面/水下 | 空中 |
|---|---|
| `EcaA9`, `EcaA9_Obstacle` | `X500` |
| `LAUV`, `LAUV_Target` | `StandardVTOL` |
| `DesistekSaga`, `RexROV2` | `RCCessna` |
| `VRX_WAMV` | |

传感器类型和配置字段见 [传感器文档](internal/sensors.md)。Fossen/Aero 质量、惯量和执行器等高级物理参数分别位于 `engine/Content/Fossen/` 和 `engine/Content/Aero/`；修改前应保留原文件并进行动力学回归测试。

## 10. 视角、截图和回放

常用快捷键：

| 快捷键 | 功能 |
|---|---|
| `V` | 在 Free、Chase 和 Sensor POV 间切换 |
| `1` / `2` / `3` | 直接选择 Free / Chase / Sensor POV |
| `Tab` / `Shift+Tab` | 切换车辆 |
| `Space` | 暂停/继续仿真；回放中暂停/继续播放 |
| `P` | Chase 多视角 |
| `I` | 遥测仪表 |
| `M` | 顶视轨迹图 |
| `N` | 轨迹显示 |
| `F9` | 保存当前 viewport 截图 |
| `Ctrl+F9` | 保存顶视图和各车视角 |
| `F10` | 开始/停止 Replay 录制和 ROS 接口 rosbag |
| `F12` | 播放最新 Replay |
| `H` | 显示帮助 |

截图默认写入用户目录下的 `OceanXCaptures/`。Replay 默认写入：

```text
<UserHome>/Documents/OceanX/Replays/
```

Replay、rosbag 和 XLog 用途不同：Replay 恢复 UE 权威状态，rosbag 保存 ROS 接口消息，XLog 保存 HydroX 内部状态和控制时间序列。详见 [回放、rosbag 与 XLog](internal/replay_xlog.md)。

## 11. 进程生命周期

| 操作 | MicroXRCEAgent | HydroX SITL |
|---|---|---|
| 第一次 Play/启动 | UE 启动或复用 | UE 按车辆启动 |
| Stop Play | 保持运行 | 当前场景实例退出 |
| 再次 Play | 复用 | 重新按车辆启动 |
| UE 正常退出 | 默认保持共享服务 | UE 停止 |
| UE 崩溃 | 保持共享服务 | 父进程守护检测后退出 |

不要在场景、ROS launch 和外部脚本中各自再启动一份 Agent。一个主机、一个 DDS UDP 端口通常只运行一个共享 Agent。

## 12. 常见问题

### Play 后车辆不动

- HydroX 默认从 `DISABLED` 启动；没有 ROS setpoint 时不会自动执行任务。
- 确认已启动 `oceanx_bringup`，且 `vehicle_names` 与场景中的 `name` 完全一致。
- 确认 `/hydrox/<vehicle>/out/auv_state` 持续更新。

### ROS 看不到 HydroX topic

- 检查 UE、HydroX 和 ROS 终端的 `ROS_DOMAIN_ID` 是否一致。
- 检查 `MicroXRCEAgent` 是否运行，以及 UDP 8888 是否被其他程序占用。
- 当前 UE 的 Agent 探测以 UDP 端口占用为依据，因此不要让无关程序占用同一端口。

Windows 检查命令：

```bat
tasklist | findstr /I "MicroXRCEAgent hydrox_sitl"
netstat -ano | findstr ":8888"
```

### Stop Play 后下一次无法启动

- 正常情况下 HydroX 会由 UE 停止；UE 崩溃后由父进程守护退出。
- 使用 `tasklist` 确认是否仍有异常残留的 `hydrox_sitl.exe`。
- 不要同时打开多个使用相同场景端口的 UE 实例。

### MicroXRCEAgent 出现“损坏的映像”或 `0xc0e90002`

- 不要混用 Agent 3.0、Fast DDS 3.x 或旧的 `microxrcedds_agent-3.0.dll`。
- 使用当前 ROS 2 Humble/Conda 环境重新运行 `build_microxrce_agent.bat`。
- 当前正式组合是 Micro XRCE-DDS Agent 2.4.2、Fast DDS 2.6、Fast CDR 1.0。
- 受管 Windows 使用仓库 BAT、`cmd.exe` 或系统 PowerShell 5.1 启动。

### 报 Scenario 或 Profile 找不到

- 确认 `scenarios/` 和 `configs/vehicle_profiles/` 位于发布根目录。
- 独立包建议同时设置 `OCEANX_ROOT` 和 `-OceanXScenario=<绝对路径>`。
- 检查 Profile 文件名是否与 JSON 中的 `vehicle_profile` 一致。
- 查看 UE 日志中的 `OceanXScenarioSpawner` 错误。

### 日志在哪里

- Editor：`engine/Saved/Logs/OceanX.log`
- Headless：`.ros/log/ue_headless_sitl.log`
- UE Replay：`<UserHome>/Documents/OceanX/Replays/`
- HydroX XLog：运行工作目录下的 `log/xlog_<vehicle>_<timestamp>.xlog`

## 13. 哪些内容允许修改

| 内容 | 是否建议用户修改 | 说明 |
|---|---|---|
| `scenarios/*.json` | 是 | 场景车辆和初始位姿 |
| `configs/vehicle_profiles/*.json` | 是 | 车型和传感器构型 |
| 打包程序目录、`Engine/` 和第三方运行时 | 否 | 运行时完整性依赖这些文件，不支持手工替换 |
| `engine/Content/Fossen/*.json`、`engine/Content/Aero/*.json` | 仅源码开发者 | 修改物理模型后需要重新 cook、回归测试和重新打包 |
| `hydrox_autopilot/`、`ros/`、`third_party/` | 仅源码开发者 | 属于源码、构建或 ROS overlay，不是打包程序可编辑配置 |

## 14. 进一步阅读

- [构建与运行细节](internal/build_and_run.md)
- [场景与 Vehicle Profile 契约](internal/scenario_vehicle_profiles.md)
- [系统架构](internal/architecture.md)
- [坐标系](internal/coordinate_systems.md)
- [传感器](internal/sensors.md)
- [HydroX 控制架构](internal/hydrox_control_architecture.md)
- [视角、截图与采集](internal/view_capture.md)
- [回放、rosbag 与 XLog](internal/replay_xlog.md)
