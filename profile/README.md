# MIRMI ERL Simulation and Deployment Architecture

Research software for the ecoBay project at TUM MIRMI: a modular ROS 2 architecture that lets the same control, estimation and evaluation code run against several simulators and, later, real aquatic vehicles. The work connects the Hamburg aquatic robotics stack with the research needs in Munich.

## The architecture under test

Three blocks, one rule: reusable code never contains simulator-specific knowledge.

| Block | Owns | Examples |
| --- | --- | --- |
| Canonical Components | endpoint-independent algorithms and the authoritative vehicle and environment definitions | tracking controller, BlueROV vehicle profile, HESS basin geometry |
| Interface | the shared messages, topics, frames, units, timing and reset semantics | the Toy 1 contract: eight normalized thruster commands in, odometry in ENU out, authoritative simulation clock |
| Endpoints | translation of a canonical definition into one simulator or one physical system | Gazebo Harmonic endpoint, Stonefish 1.6 endpoint |

A toy, or later a research application, composes components from the three blocks. It is not a fourth reusable block.

## Staged experiments

1. **Toy 1, bounded trajectory tracking.** One BlueROV, one controller, one basin, run unchanged through Gazebo and Stonefish. The decisive result is whether the controller runs through both endpoints without source changes, and how large the plant differences are. In progress.
2. **Toy 2, interaction and hydrodynamics.** Currents, thruster response, external wrench estimation. Not started.
3. **Toy 3, sensing.** Camera first, acoustics when representative tank data exist. Not started.

## Repositories

| Repository | Block | Content |
| --- | --- | --- |
| toy1_ws | application | the Toy 1 colcon workspace: launch composition, experiment configuration, the Interface contract file, the import list of all dependencies |
| mirmi_motion_control | Canonical Components | pose and body-velocity tracking cascade, trajectory definitions, mixer configuration |
| mirmi_bluerov_canons | Canonical Components | canonical BlueROV vehicle profiles with parameter provenance, and their schema |
| mirmi_translation_core | Endpoints, shared | the four operations every endpoint translator uses: load, check support, render, write the build record |
| mirmi_gazebo_endpoint | Endpoints | canonical profile to Gazebo SDF, Hippo plugin bridge, HESS basin world |
| mirmi_stonefish_endpoint | Endpoints | canonical profile to Stonefish scenario, runtime adapter for frames, pressure, buoyancy and damping |
| stonefish_vendor | third party | ROS 2 vendor package that builds the pinned Stonefish 1.6 core inside a workspace |
| stonefish_ros2 | third party, fork | the Stonefish ROS 2 wrapper with a branch that depends on the vendor package |

Getting started: clone toy1_ws, run `vcs import src < toy1.repos`, then `colcon build`. Platform: Ubuntu 24.04, ROS 2 Jazzy, Gazebo Harmonic, Stonefish 1.6.

## Status

Prototype. No simulator has been selected; the comparison is the point. Results and decisions are recorded in the project documentation, not here.
