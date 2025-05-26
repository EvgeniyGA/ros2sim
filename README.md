# ros2sim
Create package:
```
mkdir src && cd src
ros2 pkg create --build-type ament_cmake my_two_wheel_robot
```
build:
```
cd ..
colcon build
source install/setup.bash
```
after build
```
ros2 launch my_two_wheel_robot two_wheel_robot.launch.py
```
Message for run:
```
ros2 topic pub /diff_drive_controller/cmd_vel geometry_msgs/msg/TwistStamped "
{
  header: {frame_id: 'base_link'},
  twist: {linear: {x: 0.1}}
}"
```
Regular Twist not fitted:
```
ros2 topic pub /cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.5}, angular: {z: 0.1}}"
```
Diff drive controoler getting messages to: diff_drive_controller хочет получать сообщения в /diff_drive_controller/cmd_vel

### Control TWIST->TWIST_STAMPED in ROS2 and RViz
for check controller state:
```
ros2 control list_controllers
```
run teleop twist keyboard with remap:
```
ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args --remap /cmd_vel:=/cmd_vel_unstamped
```
for check topics:
```
ros2 topic list
```
- /cmd_vel_unstamped (куда отправляет команды teleop_twist_keyboard)
- /diff_drive_controller/cmd_vel (куда должна поступать команда TwistStamped)
### Create node converter for convert Twist to TwistStamped
from src create package:
```
ros2 pkg create --build-type ament_python twist_converter
```
```
colcon build
```
```
source install/setup.bash
```

