# mobman_moveit_config

MoveIt 2 configuration package for the CHARS mobile manipulator (MOBMAN).

This package contains robot planning configuration, kinematics, controllers, SRDF/URDF integration, and launch files needed to run MoveIt planning for the `mobile_manipulator` model.

## What This Package Provides

- MoveIt planning setup for `mobile_manipulator`
- Planning groups, joint limits, kinematics, and controller mappings
- Launch files for:
  - full demo setup
  - move_group node
  - RViz integration
  - robot state publisher and controller spawning
- A Python test utility to send planning goals via MoveGroup action

## Package Structure

```text
mobman_moveit_config/
  config/
    mobile_manipulator.urdf.xacro
    mobile_manipulator.srdf
    kinematics.yaml
    joint_limits.yaml
    moveit_controllers.yaml
    ros2_controllers.yaml
    moveit.rviz
    ...
  launch/
    demo.launch.py
    mobman_moveit.launch.py
    move_group.launch.py
    moveit_rviz.launch.py
    rsp.launch.py
    spawn_controllers.launch.py
    ...
  scripts/
    test_mobman_moveit.py
  package.xml
  CMakeLists.txt
```

## Dependencies

Major runtime dependencies declared in `package.xml`:

- `moveit_ros_move_group`
- `moveit_kinematics`
- `moveit_planners`
- `moveit_simple_controller_manager`
- `moveit_configs_utils`
- `moveit_ros_visualization`
- `moveit_ros_warehouse`
- `robot_state_publisher`
- `rviz2`
- `controller_manager`
- `tf2_ros`
- `xacro`
- `swarm_description`

## Build

From your ROS 2 workspace root:

```bash
colcon build --packages-select mobman_moveit_config
source install/setup.bash
```

## Run

### 1) Standard MoveIt demo launch

```bash
ros2 launch mobman_moveit_config demo.launch.py
```

### 2) Custom MOBMAN launch with namespace support

```bash
# Default namespace: mobman
ros2 launch mobman_moveit_config mobman_moveit.launch.py

# Custom namespace
ros2 launch mobman_moveit_config mobman_moveit.launch.py robot_namespace:=robot1

# Disable RViz (if enabled in your launch variant)
ros2 launch mobman_moveit_config mobman_moveit.launch.py use_rviz:=false
```

## Motion Test Script

Installed executable:

```bash
ros2 run mobman_moveit_config test_mobman_moveit
```

Example usages:

```bash
# Named target pose
ros2 run mobman_moveit_config test_mobman_moveit --ros-args -p target_pose:=up

# Cartesian target
ros2 run mobman_moveit_config test_mobman_moveit --ros-args \
  -p use_cartesian:=true -p target_x:=0.4 -p target_y:=0.0 -p target_z:=0.5

# Plan only (no execution)
ros2 run mobman_moveit_config test_mobman_moveit --ros-args -p execute:=false

# Custom robot namespace
ros2 run mobman_moveit_config test_mobman_moveit --ros-args -p robot_namespace:=robot1
```

## CHARS Integration Notes

In CHARS, this package provides manipulator planning and trajectory execution support for Layer 1 execution nodes.

Typical integration path:

1. Robot description and controllers are launched.
2. MoveIt `move_group` starts for the selected namespace.
3. Higher-level CHARS task logic triggers arm motions through planning goals.

## Known Configuration Note

In `launch/mobman_moveit.launch.py`, RViz config path is currently set to `ur5_moveit_config` package share when building the RViz config file path. If this package is used standalone, update that reference to `mobman_moveit_config`.

## Troubleshooting

- `move_group` not available:
  - Check that launch completed and action endpoint is present.
  - Verify namespace consistency (`mobman` vs custom namespace).
- No robot state / TF issues:
  - Ensure robot_state_publisher and joint states are active.
- Planning fails repeatedly:
  - Verify planning group name and joint limits.
  - Check collision geometry and start state validity.

## License

BSD
