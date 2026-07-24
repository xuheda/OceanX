# Installation and runtime guide

## Requirements

- Windows 10/11 64-bit
- A DirectX 12 compatible GPU
- ROS 2 (only required for ROS 2 mission/task interfaces)
- Micro XRCE-DDS (only required for the HydroX DDS bridge)

## Download

Download the latest packaged release from the [Releases](../../releases) page:

```text
OceanX-<version>-win64.zip
```

Extract the archive to a short path without spaces, for example:

```text
C:\OceanX\
```

## Running the simulator

After extracting, the package contains the cooked Unreal runtime and default content.

### Run the main simulator

Navigate to the extracted folder and run:

```bat
OceanX.exe
```

### Run a headless SITL scenario

If you have the HydroX SITL binary and ROS 2 environment configured, use the launch scripts provided in the release package or follow the scenario documentation in `scenarios/`.

## Customizing scenarios and vehicle profiles

You can edit the JSON files under `configs/vehicle_profiles/` and `scenarios/` to define your own vehicle instances and initial poses. Keep the `schema_version` field unchanged unless the release notes say otherwise.

## Getting help

- Read [`docs/user_guide.md`](docs/user_guide.md) for detailed usage.
- Open an [Issue](../../issues) for bugs or questions about this public distribution.
