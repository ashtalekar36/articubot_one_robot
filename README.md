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
![structure](https://github.com/user-attachments/assets/79f499cf-0600-435e-af0f-e99960693835)
