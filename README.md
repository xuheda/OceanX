# OceanX

OceanX is an Unreal Engine 5.3 robotics simulator for marine and cross-domain autonomy.

This is the **public distribution repository**. It contains user-facing documentation, example scenarios, vehicle profiles, and binary release downloads. The full engine source and core autopilot implementation are maintained separately under a private source repository.

## What is included here

- **Vehicle profiles** (`configs/vehicle_profiles/`): parameter files for AUV, ROV, surface vessels, and air vehicles.
- **Scenarios** (`scenarios/`): example scenario definitions for fleet simulations.
- **Demo configurations** (`demo/`): showcase setups and baseline mission examples.
- **User documentation** (`docs/user_guide.md`): getting started and runtime guide.
- **Release downloads**: packaged Win64 runtime is available from the [Releases](../../releases) page.

## Quick start

1. Download the latest release package from the [Releases page](../../releases).
2. Extract the archive to a local directory.
3. Follow the instructions in [`install.md`](install.md) to run the simulator.

## Runtime overview

OceanX combines Unreal Engine 5 rendering and world simulation with Fossen-style marine dynamics, HydroX software-in-the-loop flight control, MAVLink HIL transport, and ROS 2 task interfaces.

See [`docs/user_guide.md`](docs/user_guide.md) for the full user guide.

## Licensing

OceanX-owned runtime source and core assets are proprietary. The ROS 2 interface packages that carry their own MIT license retain those terms. Unreal Engine, imported vehicle models, and other third-party material retain their own terms.

See [`LICENSE`](LICENSE) and [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) for details.

## Source access

Access to the private source repository is granted on a case-by-case basis. Public contributions to this distribution repository (scenarios, vehicle profiles, documentation improvements) are welcome through Issues and Pull Requests.
