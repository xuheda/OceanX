# OceanX third-party notices

> Generated from `packaging/manifests/third_party.json`; do not edit manually.
> This inventory is an engineering compliance record, not legal advice.

| Component | Version | License | Review |
|---|---|---|---|
| Unreal Engine | 5.3 | LicenseRef-Epic-Unreal-Engine-EULA | legal_review_required |
| Oceanology | 5.6.0 for Unreal Engine 5.3 | LicenseRef-Fab-or-Marketplace-Content-License | entitlement_evidence_required |
| Micro XRCE-DDS Agent | 2.4.2 | Apache-2.0 | engineering_verified |
| eProsima Fast DDS (Fast RTPS ABI) | 2.6.10 | Apache-2.0 | engineering_verified |
| eProsima Fast CDR | 1.0.24 | Apache-2.0 | engineering_verified |
| foonathan/memory | 0.7.3 | Zlib | engineering_verified |
| TinyXML-2 | 11.0.0 | Zlib | engineering_verified |
| OpenSSL | 3.6.3 | Apache-2.0 | engineering_verified |
| Micro XRCE-DDS Client | 2.4.2 | Apache-2.0 | engineering_verified |
| Micro CDR | 2.0.1 | Apache-2.0 | engineering_verified |
| Eigen | 3.5.0-development | MPL-2.0 AND BSD-3-Clause AND Apache-2.0 | covered_source_offer_required |
| UUV Simulator vehicle assets | Desistek SAGA 0.3.2; ECA A9 0.1.6; LAUV 0.1.6; RexROV2 0.1.3 | Apache-2.0 | engineering_verified |
| UUV Simulator sand heightmap assets | sand_heightmap source snapshot (2018-12-09) | Apache-2.0 AND LicenseRef-TextureLib-Integral-Product | engineering_verified |
| VRX WAM-V assets | jazzy | Apache-2.0 | engineering_verified |
| VRX RobotX light buoy assets | jazzy | Apache-2.0 | engineering_verified |
| PX4 Gazebo model assets | pinned checkout | BSD-3-Clause | engineering_verified |
| OceanX ROS 2 interfaces and support packages | 0.1.0-development | MIT | public_source_url_required |

## Detailed notices

### Unreal Engine

- Component id: unreal-engine
- Copyright: Epic Games, Inc. and its licensors
- License: LicenseRef-Epic-Unreal-Engine-EULA
- Source: https://www.unrealengine.com/eula/unreal
- Distribution form: object code and cooked content incorporated into the packaged product
- OceanX changes: OceanX is built as an Unreal Engine packaged product; Epic source and Engine Tools are excluded.
- Review status: legal_review_required
Terms are maintained by the third-party publisher at the source URL; OceanX does not reproduce or relicense those commercial terms here.

### Oceanology

- Component id: oceanology
- Copyright: Galidar and applicable licensors
- License: LicenseRef-Fab-or-Marketplace-Content-License
- Source: https://www.fab.com/eula
- Distribution form: incorporated cooked product content only
- OceanX changes: Integrated as the visual water and underwater-rendering system; original plugin source is not a runtime deliverable.
- Review status: entitlement_evidence_required
Terms are maintained by the third-party publisher at the source URL; OceanX does not reproduce or relicense those commercial terms here.

### Micro XRCE-DDS Agent

- Component id: micro-xrce-dds-agent
- Copyright: eProsima and contributors
- License: Apache-2.0
- Source: https://github.com/eProsima/Micro-XRCE-DDS-Agent
- Distribution form: separate executable
- OceanX changes: Unmodified pinned upstream source; OceanX provides external build and deployment scripts.
- Review status: engineering_verified
- License text inputs: third_party/Micro-XRCE-DDS-Agent/LICENSE

### eProsima Fast DDS (Fast RTPS ABI)

- Component id: fast-dds
- Copyright: eProsima and contributors
- License: Apache-2.0
- Source: https://github.com/eProsima/Fast-DDS/tree/v2.6.10
- Distribution form: shared runtime library
- OceanX changes: No source modifications recorded; binary is copied from the locked build environment.
- Review status: engineering_verified
- License text inputs: third_party/Micro-XRCE-DDS-Agent/LICENSE

### eProsima Fast CDR

- Component id: fast-cdr
- Copyright: eProsima and contributors
- License: Apache-2.0
- Source: https://github.com/eProsima/Fast-CDR/tree/v1.0.24
- Distribution form: shared runtime library
- OceanX changes: No source modifications recorded; binary is copied from the locked build environment.
- Review status: engineering_verified
- License text inputs: third_party/Micro-XRCE-DDS-Agent/LICENSE

### foonathan/memory

- Component id: foonathan-memory
- Copyright: 2015-2023 Jonathan Müller and foonathan/memory contributors
- License: Zlib
- Source: https://github.com/foonathan/memory/tree/v0.7-3
- Distribution form: shared runtime library
- OceanX changes: No source modifications recorded; binary is copied from the locked build environment.
- Review status: engineering_verified
- License text inputs: packaging/licenses/third_party/FOONATHAN_MEMORY_ZLIB.txt

### TinyXML-2

- Component id: tinyxml2
- Copyright: Lee Thomason and contributors
- License: Zlib
- Source: https://github.com/leethomason/tinyxml2/tree/11.0.0
- Distribution form: shared runtime library
- OceanX changes: No source modifications recorded; binary is copied from the locked build environment.
- Review status: engineering_verified
- License text inputs: packaging/licenses/third_party/TINYXML2_ZLIB.txt

### OpenSSL

- Component id: openssl
- Copyright: The OpenSSL Project Authors
- License: Apache-2.0
- Source: https://github.com/openssl/openssl/tree/openssl-3.6.3
- Distribution form: shared runtime libraries
- OceanX changes: No source modifications recorded; binaries are copied from the locked build environment.
- Review status: engineering_verified
- License text inputs: third_party/Micro-XRCE-DDS-Agent/LICENSE

### Micro XRCE-DDS Client

- Component id: micro-xrce-dds-client
- Copyright: eProsima and contributors
- License: Apache-2.0
- Source: https://github.com/eProsima/Micro-XRCE-DDS-Client/tree/v2.4.2
- Distribution form: statically linked into hydrox_sitl
- OceanX changes: No source modifications; transport profiles are selected by OceanX build options.
- Review status: engineering_verified
- License text inputs: third_party/Micro-XRCE-DDS-Agent/LICENSE

### Micro CDR

- Component id: micro-cdr
- Copyright: eProsima and contributors
- License: Apache-2.0
- Source: https://github.com/eProsima/Micro-CDR/tree/v2.0.1
- Distribution form: statically linked into hydrox_sitl
- OceanX changes: No source modifications recorded.
- Review status: engineering_verified
- License text inputs: third_party/Micro-XRCE-DDS-Agent/LICENSE

### Eigen

- Component id: eigen
- Copyright: The Eigen Authors and file-level contributors
- License: MPL-2.0 AND BSD-3-Clause AND Apache-2.0
- Source: https://gitlab.com/libeigen/eigen
- Distribution form: header implementations compiled into hydrox_sitl
- OceanX changes: No Eigen source modifications recorded; OceanX includes Eigen headers from the pinned checkout.
- Review status: covered_source_offer_required
- License text inputs: hydrox_autopilot/third_party/eigen/LICENSES/MPL-2.0.txt, hydrox_autopilot/third_party/eigen/LICENSES/BSD-3-Clause.txt, hydrox_autopilot/third_party/eigen/LICENSES/Apache-2.0.txt

### UUV Simulator vehicle assets

- Component id: uuv-simulator-vehicle-assets
- Copyright: UUV Simulator contributors and the authors identified in the upstream packages
- License: Apache-2.0
- Source: https://github.com/uuvsimulator
- Distribution form: converted and cooked Unreal vehicle assets
- OceanX changes: The imported DAE inputs were verified against the listed commits. ECA A9 body texture-coordinate annotations, mesh conversion, naming, materials, collision, and OceanX vehicle integration are recorded in engine/Import/MODEL_MODIFICATION_NOTICES.md.
- Review status: engineering_verified
- License text inputs: engine/Import/desistek_saga-master/LICENSE, engine/Import/eca_a9-master/LICENSE, engine/Import/lauv_gazebo-master/LICENSE, engine/Import/rexrov2-master/LICENSE
- Upstream notice inputs: engine/Import/desistek_saga-master/NOTICE, engine/Import/eca_a9-master/NOTICE, engine/Import/lauv_gazebo-master/NOTICE, engine/Import/rexrov2-master/NOTICE

### UUV Simulator sand heightmap assets

- Component id: uuv-simulator-sand-heightmap-assets
- Copyright: UUV Simulator Authors; Texture Library © 2013 Dmitriy Chugai
- License: Apache-2.0 AND LicenseRef-TextureLib-Integral-Product
- Source: https://github.com/uuvsimulator/uuv_simulator/tree/9baba956936ca6b2ad83493fc7651fc49752766d/uuv_gazebo_worlds/models/sand_heightmap
- Distribution form: cooked Unreal environment mesh, material, and texture embedded in the packaged product
- OceanX changes: OceanX reproducibly converts the fixed UUV heightmap DAE to heightmap.fbx for existing static-mesh editor metadata. The existing Unreal static mesh, material, and texture are placed in the environment. The imported texture MD5 exactly matches UUV's soil_sand_0045_01.jpg.
- Review status: engineering_verified
- License text inputs: engine/Import/eca_a9-master/LICENSE, packaging/licenses/third_party/TEXTURELIB_SOIL_SAND_0045_01.txt
- Upstream notice inputs: packaging/licenses/third_party/UUV_SIMULATOR_NOTICE.txt

### VRX WAM-V assets

- Component id: vrx-wamv-assets
- Copyright: Open Source Robotics Foundation and VRX contributors
- License: Apache-2.0
- Source: https://github.com/osrf/vrx
- Distribution form: converted and cooked Unreal vehicle assets
- OceanX changes: Mesh and texture conversion, Unreal materials, collision, and OceanX vehicle integration are recorded in engine/Import/MODEL_MODIFICATION_NOTICES.md.
- Review status: engineering_verified
- License text inputs: engine/Import/vrx-jazzy/LICENSE

### VRX RobotX light buoy assets

- Component id: vrx-robotx-light-buoy-assets
- Copyright: Open Source Robotics Foundation and VRX contributors
- License: Apache-2.0
- Source: https://github.com/osrf/vrx
- Distribution form: converted and cooked Unreal prop assets
- OceanX changes: Mesh and texture conversion, Unreal material creation, collision, and OceanX simple-buoyancy integration are recorded in engine/Import/MODEL_MODIFICATION_NOTICES.md.
- Review status: engineering_verified
- License text inputs: engine/Import/vrx-jazzy/LICENSE

### PX4 Gazebo model assets

- Component id: px4-gazebo-model-assets
- Copyright: PX4 Autopilot for Drones, Rudis Laboratories, and named model contributors
- License: BSD-3-Clause
- Source: https://github.com/PX4/PX4-gazebo-models
- Distribution form: converted and cooked Unreal X500, Standard VTOL, and RC Cessna assets
- OceanX changes: Mesh assembly and conversion, naming, materials, collision, and OceanX vehicle integration are recorded in engine/Import/MODEL_MODIFICATION_NOTICES.md.
- Review status: engineering_verified
- License text inputs: engine/Import/px4-gazebo-models/LICENSE, engine/Import/px4-gazebo-models/models/x500/LICENSE

### OceanX ROS 2 interfaces and support packages

- Component id: oceanx-ros2-sdk
- Copyright: 2026 OceanX contributors
- License: MIT
- Source: https://example.invalid/oceanx-sdk-source-location-to-be-published
- Distribution form: separate SDK source and/or binaries, not the proprietary core runtime
- OceanX changes: OceanX-authored interface and support packages.
- Review status: public_source_url_required
- License text inputs: ros/LICENSE

The complete license texts included in a runtime distribution are in
`packaging/licenses/THIRD_PARTY_NOTICES.txt`.
