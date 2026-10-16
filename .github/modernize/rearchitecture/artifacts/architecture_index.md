# Architecture implementation guide

This index is not the full contract. Do not implement from this file alone; follow the artifact paths below.

## Global artifacts

- `unit_graph.yaml` — entry points, dependencies, exported ROS contracts, and dynamic entry points.
- `wire_contracts.yaml` — filter by `unit` for topic/service payload and error behavior.
- `shared_modules.yaml` — filter `used_by_units` for shared ROS interfaces and coordinate transforms.
- `cross_unit_state.yaml` — filter writer/reader units for frame correlation and readiness behavior.
- `project-structure.md` — module and runtime layering.
- `tech-stack.md` — existing frameworks and environment assumptions.
- `data-model.md` — message and replay-file model; there is no ORM/database.

## Implementation units

Each unit must read its `behavior.yaml`, `bindings.yaml`, and `unit_decomposition.yaml`. The decomposition file contains advisory candidates only and has `commit: false`.

- `scenario-runner`: `units/scenario-runner/*`; preserve tick sequencing, timeout heartbeat, and reset behavior.
- `traffic-sim`: `units/traffic-sim/*`; preserve replay loading, prediction horizon, and speed calculation.
- `traffic-list`: `units/traffic-list/*`; preserve selected traffic-agent IDs and tick-driven publication.
- `map-server`: `units/map-server/*`; preserve AD Map coordinate transforms and lane/routing service behavior.
- `lane-change-decision`: `units/lane-change-decision/*`; preserve hazard detection, safe-neighbor checks, and `-1/0/1` output.
- `trajectory-planner`: `units/trajectory-planner/*`; preserve emergency braking, lane-change response, speed profile, and waypoint output.
- `pid-controller`: `units/pid-controller/*`; preserve waypoint following, control timing, service calls, and vehicle metrics.
- `vehicle-model`: `units/vehicle-model/*`; preserve input-to-model-step and output-field mapping.
- `carla-bridge`: `units/carla-bridge/*`; preserve simulator connection, actor lifecycle, and visualization updates.

Before DONE, report the artifacts read, every `must_preserve` behavior implemented, unresolved runtime checks, and build/runtime evidence.
