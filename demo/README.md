# OceanX Showcase Demo

This folder contains repeatable presentation scripts for OceanX.

## Run

From a Windows `cmd.exe` at the repository root:

```bat
demo_showcase.bat
```

Optional arguments:

```bat
demo_showcase.bat demo\showcases\ocean_fleet.json 240 scenarios\vehicle_test_3domain.json
```

Argument order:

1. showcase JSON path
2. UE auto-exit duration in simulation seconds
3. scenario JSON path

The BAT starts the UE presentation window, loads the ROS overlay, starts
`oceanx_bringup`, runs `tools/demo_showcase.py`, and records a ROS bag for
`/(hydrox|oceanx)/.*` topics.

## Operator Cues

The first version is an external director. It sends ROS tasks and prints cues,
while the operator still controls UE viewport features:

- `2`: Chase view
- `P`: multi-view overlay
- `I`: telemetry HUD
- `M`: top-down trajectory map
- `F10`: UE authoritative Replay start/stop
- `Ctrl+F9`: top-down and per-vehicle capture boards

## Artifacts

Each run writes a timestamped folder under `demo/artifacts/`:

- `showcase_events.jsonl`
- `run_summary.md`
- `ros_bringup.log`
- `rosbag/`
- `snapshot_*.txt`

UE screenshots are written by OceanX to the configured user capture directory.
