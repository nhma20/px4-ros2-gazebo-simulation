# PX4 ROS2 Gazebo Simulation
A guide on how to fly ROS2-based multi-rotors manually and autonomously in Gazebo with PX4 in the loop

(March 20th 2023) Currently under construction.

Derived from: 
- https://docs.px4.io/main/en/ros/ros2_comm.html
- https://github.com/PX4/PX4-user_guide/blob/main/en/middleware/xrce_dds.md
- https://github.com/nhma20/mmWave_ROS2_PX4_Gazebo
- https://github.com/nhma20/radar_cable_follower_HW

Tested with:
- Ubuntu 22.XX.X
- ROS2 Humble
- Gazebo version #
- PX4-msgs commit #
- PX4 commit #
- Micro-XRCE-DDS-Agent commit #

## Installation

Setup XRCE-DDS Agent and Client:
  - `git clone https://github.com/eProsima/Micro-XRCE-DDS-Agent.git`
  - `cd Micro-XRCE-DDS-Agent`
  - `mkdir build`
  - `cd build`
  - `cmake ..`
  - `make`
  - `sudo make install`
  - `sudo ldconfig /usr/local/lib/`
     
     
Install ROS2:
  - `sudo apt install software-properties-common`
  - `sudo add-apt-repository universe`
  - `sudo apt update && sudo apt install curl`
  - `sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg`
  - `echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null`
  - `sudo apt update`
  - `sudo apt upgrade`
  - `sudo apt install ros-humble-desktop`
  - `sudo apt install ros-dev-tools`
  - `pip3 install --user -U empy pyros-genmsg setuptools`
  - Create ROS2 workspace:
  `mkdir -p ros2_ws/src`
  - Get message definitions and example project:
  `cd ros2_ws/src`
  `git clone https://github.com/PX4/px4_msgs.git`
  `git clone https://github.com/PX4/px4_ros_com.git`
  - Build workspace:
  `source /opt/ros/humble/setup.bash`
  `colcon build`
  
  
Install PX4: 
  - PX4:
    - ```sh
      git clone https://github.com/PX4/PX4-Autopilot.git --recursive
      bash ./PX4-Autopilot/Tools/setup/ubuntu.sh
      ```
  
  
Install QGroundControl:
https://docs.qgroundcontrol.com/master/en/getting_started/download_and_install.html#ubuntu
```sh
sudo usermod -a -G dialout $USER
sudo apt-get remove modemmanager -y
sudo apt install gstreamer1.0-plugins-bad gstreamer1.0-libav gstreamer1.0-gl -y
```
- Download: https://s3-us-west-2.amazonaws.com/qgroundcontrol/latest/QGroundControl.AppImage
```sh
cd ~/Downloads/
chmod +x ./QGroundControl.AppImage
   ```
- Start QGroundControl
`./QGroundControl.AppImage  (or double click app icon)`
- Go to Application Settings (Press the Q in the upper left corner then "Application Settings"), and tick the "Virtual Joystick" box.

  
## Simulation
  
- Start QGroundControl
`./QGroundControl.AppImage  (or double click app icon)`
- Start simulation:
  - Source ROS2 and workspace:
  `source /opt/ros/humble/setup.bash`
  `source install/local_setup.bash`
  - Start agent with:
  `MicroXRCEAgent udp4 -p 8888`
  - Start a PX4 Gazebo Classic simulation using:
  `make px4_sitl gazebo-classic`
  - 
  
  
## Customization

### Manual flight (through QGroundControl)
- With QGroundControl and the simulation running, perform a takeoff through QGroundControl (click "takeoff" and accept).
- Fly drone either with virtual joystick or connected transmitter.

### Autonomous flight (with ROS2 nodes and simulated sensors)
- 

### Simulated sensors (e.g. camera or LiDAR)
A variety of sensors can be added to the simulation and customized to fit various requirements. If a sensor is not available, it is possible to create your own sensor plugins or base it on an existing plugin.
Existing plugins are added to the model .sdf file (see here (link) for example on 2D LiDAR and camera). Based on the customization in the .sdf file, the data from the sensors can now be subscribed to via the relevant ROS2 topics.

### Simulated disturbances (e.g. wind)
Wind can be added to increase the realism of the simulation. It is added as a plugin to the world file (see here (link)). Modify velocity and direction values as needed.

### Custom drone frame
A custom drone frame can be added to the simulation. 
- Visual model from CAD
- Collision model
- Frame dynamics

## TODO
- Add custom HCAA world
- Port cable alignment example to Ubuntu 22/ROS2 Humble workspace
- Fill out customization section
