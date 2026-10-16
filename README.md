# Autonomous Truck Simulation Project

This is a personal project for learning about self-driving truck software.

The project uses ROS 2 to connect several parts of a driving simulation:

```text
simulation clock
    -> traffic data
    -> lane-change decision
    -> route and waypoint planning
    -> steering and speed control
    -> vehicle simulator
    -> vehicle position and speed
```

The project can use a road map, saved traffic data, and simulator tools such as TruckSim, Simulink, and CARLA. It is made for learning and testing in simulation. It is not ready to control a real vehicle.


## What it does

The main example is a two-lane road scenario based on the Jinan map. The different parts work together like this:

1. **Scenario runner** starts the simulation clock and handles resets.
2. **Traffic simulator** replays saved data for nearby vehicles.
3. **Lane-change module** decides whether the truck should go left, stay, or go right.
4. **Trajectory planner** uses the map and traffic data to create waypoints and speeds.
5. **PID controller** follows the waypoints by calculating steering, throttle, and brake values.
6. **Vehicle model bridge** sends these values to TruckSim or the Simulink vehicle model.
7. **CARLA bridge** can show the truck and other vehicles in a 3D scene.

The messages shared between these parts are in [`interface/`](interface/). Some of the main messages are:

- `SimTick`
- `PredictionResult`
- `LaneChangeMsgs`
- `PlanningResult`
- `VehicleModelInput`
- `VehicleModelOutput`
- `Control`

## Main folders

| Folder | What it contains |
| --- | --- |
| `interface/` | ROS messages and services shared by the nodes |
| `hdmap/` | Road-map files and map tools |
| `planning/trajectory_planner/` | Route, waypoint, speed, and obstacle planning |
| `planning/rl_decision_making/` | Experimental reinforcement-learning code |
| `control/pid_controller/` | Steering, speed, brake, and throttle control |
| `scenarios/scenario_runner/` | Simulation timing and scenario setup |
| `scenarios/traffic_sim/` | Saved traffic playback and traffic prediction |
| `vehicle_model/` | Simulink vehicle model and simulator bridges |
| `visualization/carla/` | Optional CARLA connection and display |
| `utils/` | Small shared helper modules |

## Software needed

The original project was made for:

- Ubuntu 18.04
- ROS 2 Eloquent
- Python 3
- `colcon`
- A C++ compiler

Ubuntu 18.04 and ROS 2 Eloquent are old and are no longer supported. The easiest way to run this version is probably with an old Ubuntu virtual machine or container.

Depending on what you want to run, you may also need:

- ROS 2 Eloquent Desktop
- `colcon-common-extensions`
- Python packages such as NumPy and SciPy
- AD Map dependencies such as Boost, PROJ, GTest, yaml-cpp, and LAPACKE
- **Optional:** CARLA and its Python API
- **Optional:** TruckSim and its ROS bridge
- **Optional:** rosbag2 for recording and replaying data

## Build the project

Create a ROS workspace and put this project inside its `src` folder:

```bash
mkdir -p ~/autonomous-truck-ws/src
cd ~/autonomous-truck-ws/src
git clone <your-repository-url> personal-truck-sim
cd ..
```

Load ROS 2 and build:

```bash
source /opt/ros/eloquent/setup.bash
colcon build --symlink-install
source install/setup.bash
```

You can also build only the packages needed for the main planning example:

```bash
colcon build --symlink-install \
  --packages-select \
  fead_interfaces \
  trajectory_planner \
  pid_controller \
  traffic_sim \
  scenario_runner
```

## Run the Jinan scenario

After building and sourcing the workspace, run:

```bash
source /opt/ros/eloquent/setup.bash
source ~/autonomous-truck-ws/install/setup.bash
ros2 launch scenario_runner run_jinan.py
```

The scenario also needs a working map and a vehicle-model bridge. Those parts depend on the simulator installed on your computer.

## Use CARLA

Start CARLA in one terminal:

```bash
cd /path/to/CARLA
./CarlaUE4.sh
```

Then start the ROS bridge in another terminal:

```bash
source /opt/ros/eloquent/setup.bash
source ~/autonomous-truck-ws/install/setup.bash
ros2 run carla_bridge run_carla_bridge
```

The CARLA settings are in [`visualization/carla/carla_bridge/config/settings.yaml`](visualization/carla/carla_bridge/config/settings.yaml). The default connection is `127.0.0.1:2000`.

## Vehicle models

The project includes a generated Simulink bridge in [`vehicle_model/simulink/simulink_bridge/`](vehicle_model/simulink/simulink_bridge/).

TruckSim is also mentioned in the launch files, but it needs a separate TruckSim installation and bridge. The exact command may be different on each computer.

Before starting the full system, these commands can help check whether the ROS nodes and services are available:

```bash
ros2 topic list
ros2 service list
```

## Useful ROS topics

| Topic or service | Purpose |
| --- | --- |
| `SimTick` | Keeps the simulation frames moving |
| `PredictionResult` | Contains predicted positions of nearby vehicles |
| `LaneChangeMsgs` | Contains the lane-change decision |
| `planning_result` | Contains the planned waypoints and speeds |
| `VehicleModelOutput` | Contains the truck's position, speed, and other values |
| `Control` | Sends control values to the vehicle model |

## Record and replay data

If rosbag2 is installed, you can record the ROS messages:

```bash
sudo apt-get install ros-eloquent-ros2bag ros-eloquent-rosbag2-storage-default-plugins
ros2 bag record -a -o truck-simulation-run
ros2 bag play truck-simulation-run
```

Recording only the planning and control topics can make the saved file smaller.

## Ideas for future changes

Some simple ideas for improving the project later:

- Add another road or traffic scenario.
- Save planner and fuel results in a clearer format.
- Add charts for speed, braking, and lane changes.
- Move the project to a newer ROS 2 version.
- Add a small test that checks whether the main nodes can communicate.

## Safety

This project is for simulation and learning only. It has not been tested for use on public roads and should not be connected to a real vehicle.

## Credits and licenses

This repository contains project code and third-party code. The original copyright and license information in the source files should stay in place. Check each package's `package.xml`, README, and license files before sharing the project.
