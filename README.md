# articubot_one_robot


### Creating a Workspace 

Creating the  workspace 
```bash
mkdir -p ~/ros2_ws/src
```
Navigate to the Workspace: Change to the workspace directory.
```bash
cd ~/ros2_ws/
```
Build the Workspace: If you have packages in the src folder, you can build the workspace using colcon.
```bash
colcon build
```
Source the Workspace: After building, source the workspace to make the packages available.
```bash
source install/setup.bash
```

### Installin the Packages 
```bash
sudo apt install ros-humble-xacro ros-humble-joint-state-publisher-gui
```

![structure1](https://github.com/user-attachments/assets/cfb1d85b-888d-4aad-9a2b-fb9e043e434a)

#### No need to rebuild colcon
```bash
colcon build --symlink-install
```

###  Enter your workspace 
```bash
cd ros2_ws/src
```
### Clone the Github repo
```bash
git clone https://github.com/joshnewans/my_bot.git
```
In src change the name from my_robot to articubot_one

### Source the worksapce 
```bash
colcon build --packages-select articubot_one 
```
### Source the Packages 
```bash
source install/setup.bash
```
### Launch the file 
```bash
ros2 launch articubot_one rsp.launch.py
```

### If u found eror 
You need to add these things in ur package.xml , cmakelist.txt , rsp.launch.py ( U need to add ur bot name and github usename and gmail )
```bash
import os

from ament_index_python.packages import get_package_share_directory

from launch import LaunchDescription
from launch.substitutions import LaunchConfiguration
from launch.actions import DeclareLaunchArgument
from launch_ros.actions import Node

import xacro


def generate_launch_description():

    # Check if we're told to use sim time
    use_sim_time = LaunchConfiguration('use_sim_time')

    # Process the URDF file
    pkg_path = os.path.join(get_package_share_directory('articubot_one'))
    xacro_file = os.path.join(pkg_path,'description','robot.urdf.xacro')
    robot_description_config = xacro.process_file(xacro_file)
    
    # Create a robot_state_publisher node
    params = {'robot_description': robot_description_config.toxml(), 'use_sim_time': use_sim_time}
    node_robot_state_publisher = Node(
        package='robot_state_publisher',
        executable='robot_state_publisher',
        output='screen',
        parameters=[params]
    )


    # Launch!
    return LaunchDescription([
        DeclareLaunchArgument(
            'use_sim_time',
            default_value='false',
            description='Use sim time if true'),

        node_robot_state_publisher
    ])

```
### In the package.xml file 
```bash
<?xml version="1.0"?>
<?xml-model href="http://download.ros.org/schema/package_format3.xsd" schematypens="http://www.w3.org/2001/XMLSchema"?>
<package format="3">
  <name>articubot_one</name>
  <version>0.0.0</version>
  <description>TODO: Package description</description>
  <maintainer email="my_email@email.com">ashishtalekar36@gmail.com</maintainer>
  <license>TODO: License declaration</license>

  <buildtool_depend>ament_cmake</buildtool_depend>

  <test_depend>ament_lint_auto</test_depend>
  <test_depend>ament_lint_common</test_depend>

  <export>
    <build_type>ament_cmake</build_type>
  </export>
</package>

```
In Cmakelist.txt 
```bash
cmake_minimum_required(VERSION 3.5)
project(articubot_one)

# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

install(
  DIRECTORY config description launch worlds
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
```
 
### Launch it again 

```bash
ros2 launch articubot_one rsp.launch.py
```
### To add material to ur urdf 
```bash
<material name="white">
        <color rgba="1.0 1.0 1.0 1.0"/>
    </material>

    <material name="orange">
        <color rgba="1.0 0.3 0.1 1.0"/>
    </material>

    <material name="blue">
        <color rgba="0.2 0.2 1.0 1.0"/>
    </material>

    <material name="black">
        <color rgba="0.0 0.0 0.0 1.0"/>
    </material>
```

### To add link to ur urdf 
```bash
<link name="base_link">
    
    </link>
```
### Paste this code in the robot_core.xacro
```bash
<?xml version="1.0"?>
<robot xmlns:xacro="http://www.ros.org/wiki/xacro">

    <!-- Include inertial m<xacro:include filename="path/to/inertial_macros.xacro"/>acros -->
    <xacro:include filename="inertial_macros.xacro"/>

 <!-- Ensure the correct path or update it if needed -->

    <!-- Define Materials -->
    <material name="white">
        <color rgba="1 1 1 1"/>
    </material>

    <material name="orange"> <!-- Changed name from 'white' to 'orange' to avoid duplicate material names -->
        <color rgba="1 0.3 0.1 1"/>
    </material>

    <material name="blue">
        <color rgba="0.2 0.2 1 1"/>
    </material>

    <material name="black">
        <color rgba="0 0 0 1"/>
    </material>

    <!-- Base Link -->
    <link name="base_link"/>

    <!-- Chassis Link -->
    <joint name="chassis_joint" type="fixed">
        <parent link="base_link"/>
        <child link="chassis"/>
        <origin xyz="-0.1 0 0"/>
    </joint>

    <link name="chassis">
        <visual>
            <origin xyz="0.15 0 0.075"/>
            <geometry>
                <box size="0.3 0.3 0.15"/>
            </geometry>
            <material name="white"/>
        </visual>
        <collision>
            <geometry>
                <sphere radius="0.05"/>
            </geometry>
        </collision>
        <xacro:inertial_box mass="0.5" x="0.3" y="0.3" z="0.15">
            <origin xyz="0.15 0.0 0.075" rpy="0.0 0.0 0.0"/>
        </xacro:inertial_box> <!-- Closing tag for xacro macro -->
    </link>

    <!-- Left Wheel Link -->
    <joint name="left_wheel_joint" type="continuous">
        <parent link="base_link"/>
        <child link="left_wheel"/>
        <origin xyz="0.0 0.175 0.0" rpy="-${pi/2} 0.0 0.0"/>
        <axis xyz="0.0 0.0 1.0"/>
    </joint>

    <link name="left_wheel">
        <visual>
            <geometry>
                <cylinder radius="0.05" length="0.04"/>
            </geometry>
            <material name="blue"/>
        </visual>
        <collision>
            <geometry>
                <cylinder radius="0.05" length="0.04"/>
            </geometry>
        </collision>
        <xacro:inertial_cylinder mass="0.1" length="0.04" radius="0.05">
            <origin xyz="0 0.0 0.0" rpy="0.0 0.0 0.0"/>
        </xacro:inertial_cylinder> <!-- Closing tag for xacro macro -->
    </link>

    <!-- Right Wheel Link -->
    <joint name="right_wheel_joint" type="continuous">
        <parent link="base_link"/>
        <child link="right_wheel"/>
        <origin xyz="0.0 -0.175 0.0" rpy="${pi/2} 0.0 0.0"/>
        <axis xyz="0.0 0.0 -1.0"/>
    </joint>

    <link name="right_wheel">
        <visual>
            <geometry>
                <cylinder radius="0.05" length="0.04"/>
            </geometry>
            <material name="blue"/>
        </visual>
        <collision>
            <geometry>
                <cylinder radius="0.05" length="0.04"/>
            </geometry>
        </collision>
        <xacro:inertial_cylinder mass="0.1" length="0.04" radius="0.05">
            <origin xyz="0 0.0 0.0" rpy="0.0 0.0 0.0"/>
        </xacro:inertial_cylinder> <!-- Closing tag for xacro macro -->
    </link>

    <!-- Caster Wheel Link -->
    <joint name="caster_wheel_joint" type="fixed">
        <parent link="chassis"/>
        <child link="caster_wheel"/>
        <origin xyz="0.24 0.0 0.0"/>
    </joint>

    <link name="caster_wheel">
        <visual>
            <geometry>
                <sphere radius="0.05"/>
            </geometry>
            <material name="black"/>
        </visual>
        <collision>
            <geometry>
                <sphere radius="0.05"/>
            </geometry>
        </collision>
        <xacro:inertial_sphere mass="0.1" radius="0.05">
            <origin xyz="0.0 0.0 0.0" rpy="0.0 0.0 0.0"/>
        </xacro:inertial_sphere> <!-- Closing tag for xacro macro -->
    </link>

</robot>
```
### Run the File 
```bash
ros2 run joint_state_publisher_gui joint_state_publisher_gui
```


