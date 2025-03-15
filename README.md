# ros-llm-demo
A ROS LLM Demo, for funzies


## Software versions

The considered software supports ROS2:

| Software    | Humble | Iron | Jazzy |
|------------|--------|------|-------|
| Turtlebot 3 | ✅    | ❌   | ❌    |
| ROSA        | ✅    | ✅   | ✅    |
| ROS-LLM     | ✅    | ❌   | ❌    |

Needs ROS2 Humble Hawksbill, and therefore Ubuntu 22.04. Jazzy and 24.04 would be great, but no.

## Installation

### Virtual Machine
VM is nice for easy fun. Used VMWare Workstation Pro and made an Ubuntu 22.04 VM.

- 8GB RAM
- 50 GB storage
- 2 CPU cores

### ROS2 install steps
History of commands for install of ROS2 Humble in VM. Resulting steps from https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debs.html
```bash
sudo apt-get update
sudo apt-get upgrade

sudo apt update && sudo apt install curl -y
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

sudo apt update
sudo apt upgrade

sudo apt install ros-humble-desktop
sudo apt install ros-dev-tools

echo "source /opt/ros/humble/setup.bash" >> ~/.bashrc
```

### Turtlebot 3 install steps
History of commands for install of Turtlebot 3 in VM. Resulting steps from https://emanual.robotis.com/docs/en/platform/turtlebot3/quick-start/

```bash
sudo apt install ros-humble-gazebo-*
sudo apt install ros-humble-cartographer
sudo apt install ros-humble-cartographer-ros
sudo apt install ros-humble-navigation2
sudo apt install ros-humble-nav2-bringup

mkdir -p ~/turtlebot3_ws/src
cd ~/turtlebot3_ws/src/
git clone -b humble https://github.com/ROBOTIS-GIT/DynamixelSDK.git
git clone -b humble https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
git clone -b humble https://github.com/ROBOTIS-GIT/turtlebot3.git
sudo apt install python3-colcon-common-extensions
cd ~/turtlebot3_ws
colcon build --symlink-install

echo 'source ~/turtlebot3_ws/install/setup.bash' >> ~/.bashrc
echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
echo 'source /usr/share/gazebo/setup.sh' >> ~/.bashrc

cd ~/turtlebot3_ws/src/
git clone -b humble https://github.com/ROBOTIS-GIT/turtlebot3_simulations.git
cd ~/turtlebot3_ws && colcon build --symlink-install

echo 'export TURTLEBOT3_MODEL=waffle_pi' >> ~/.bashrc
```
Note: got an error on dynamixel and gazebo during build, don't care at this point though.

### Verify `.bashrc`

After install, the following 4 lines should be at then end of `.bashrc`
```bash
source /opt/ros/humble/setup.bash
source ~/turtlebot3_ws/install/setup.bash
export ROS_DOMAIN_ID=30 #TURTLEBOT3
source /usr/share/gazebo/setup.sh
export TURTLEBOT3_MODEL=waffle_pi
```
verify using `gedit ~/.bashrc` and scroll to bottom.

## Test Turtlebot 3 simulation
Use https://emanual.robotis.com/docs/en/platform/turtlebot3/nav_simulation/

### Make a map

Terminal 1
```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

Terminal 2
```bash
ros2 launch turtlebot3_cartographer cartographer.launch.py use_sim_time:=True
```

Terminal 3
```bash
ros2 run turtlebot3_teleop teleop_keyboard
```

used terminal 3 to drive the turtlebot around to make a map. Then

Terminal 4
```bash
ros2 run nav2_map_server map_saver_cli -f ~/map
```

## Demo

Terminal 1
```bash
ros2 launch turtlebot3_gazebo turtlebot3_world.launch.py
```

Terminal 2
```bash
ros2 launch turtlebot3_navigation2 navigation2.launch.py use_sim_time:=True map:=$HOME/map.yaml
```