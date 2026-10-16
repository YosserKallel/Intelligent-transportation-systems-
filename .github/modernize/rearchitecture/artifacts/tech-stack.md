# Technology stack

- ROS 2 Eloquent conventions (`rclpy`, `rclcpp`, `ament_python`, `ament_cmake`, `colcon`).
- Python 3 nodes using NumPy, SciPy, setuptools, and ROS Python message bindings.
- C++ ROS nodes for generated Simulink vehicle-model integration.
- AD Map / `ad_map_access`, `ad_physics`, OpenDRIVE map resources, and GeographicLib.
- CARLA Python API plus Pygame for visualization and simulator interaction.
- TruckSim integration is referenced by launch/build scripts but its bridge source is not present in this checkout.
- Simulink-generated C++ model is included under `vehicle_model/simulink/simulink_bridge/src`.
- ROS interfaces are defined as `.msg` and `.srv` files in `interface/`.
- Configuration is primarily ROS YAML parameter files under each package's `config/` directory.
- README targets Ubuntu 18.04 and ROS 2 Eloquent; this is an old/EOL environment assumption.
- Package versions are mostly `0.0.0`/`0.0.1`; dependency versions are generally delegated to the ROS distribution rather than pinned in this repository.
