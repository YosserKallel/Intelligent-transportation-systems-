# Data model

This project has no database/ORM model. Its persistent and exchanged data is simulation state carried through ROS messages, replay files, map resources, and simulator APIs.

## Key exchanged models

- `VehicleModelOutput` — ego pose, velocity, yaw/acceleration, engine/gear metrics, fuel metrics, and corresponding input.
- `VehicleModelInput` — throttle, brake, steering, gear, road grade, and control frame ID.
- `PlanningResult` — planner status, emergency-stop flag, and an ordered `Waypoint[]`.
- `Waypoint` — 3-D center point and target speed.
- `PredictionResult` — a sequence of future `PerceptionResult` frames.
- `PerceptionResult` — an obstacle array for one simulation frame.
- `Obstacle` — traffic-agent identity, position, orientation, and speed.
- `LaneChangeMsgs` — lane-change operation (`-1`, `0`, `1`) and episode completion flag.
- `SimTick` — frame/timestamp heartbeat plus optional ego center/orientation.

## External data

- OpenDRIVE/AD Map configuration and binary map resources under `hdmap/ad_map/ad_map_server/resource`.
- Replay traffic traces under `scenarios/traffic_sim/resource`.
- Vehicle-model parameter and Simulink resources under `vehicle_model/cnhtc_pre_a/resource` and `vehicle_model/simulink`.
