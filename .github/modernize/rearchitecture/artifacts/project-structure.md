# Project structure

## Classification

Full-stack robotics/simulation workspace implemented primarily as ROS 2 packages. It is not a web application. The runtime is a distributed autonomous-driving simulation composed of Python and C++ ROS nodes communicating through topics and services.

## Top-level areas

- `interface/` — ROS 2 message and service definitions (`fead_interfaces`).
- `hdmap/` — AD Map/OpenDRIVE access, map matching, routing, lane queries, and map test utilities.
- `planning/trajectory_planner/` — map-based waypoint generation, speed profiles, collision response, and trajectory planning.
- `planning/rl_decision_making/` — reinforcement-learning decision experiments plus training/test helpers.
- `control/pid_controller/` — lateral/longitudinal PID control and fuel/vehicle-control calculations.
- `scenarios/scenario_runner/` — simulation clock/scenario orchestration and episode reset handling.
- `scenarios/traffic_sim/` — replay-based traffic agents and predicted obstacle trajectories.
- `vehicle_model/` — generated Simulink vehicle model bridge and related vehicle-model packages.
- `visualization/carla/` — CARLA bridge, vehicle/traffic actor spawning, rendering, and control visualization.
- `utils/` — small fuel-model and utility modules.
- `hdmap/ad_map/GeographicLib/` and `spdlog/` — bundled third-party libraries; not core IITS business logic.

## Runtime layers

1. **Orchestration**: `scenario_runner` emits simulation ticks and reset signals.
2. **World/traffic**: `traffic_sim` replays traffic files and publishes predicted obstacles.
3. **Decision and planning**: greedy/RL decision modules select lane changes; `trajectory_planner` converts map and obstacle state into timed waypoints.
4. **Control**: `pid_controller` follows waypoints and sends vehicle-control requests.
5. **Plant/simulator adapters**: TruckSim, Simulink, and CARLA bridges provide vehicle state, apply controls, and visualize the run.
6. **Map services**: AD Map services provide lane identity, routing, geometry, elevation, and lane-neighbor queries.

## Primary runtime entry point

`scenarios/scenario_runner/launch/run_jinan.py` launches the Jinan scenario, map server, traffic nodes, planner, greedy decision maker, TruckSim bridge, PID controller, and CARLA bridge.
