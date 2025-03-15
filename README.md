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