# orb_slam3_tello_ros
A ROS package for running ORB-SLAM3 on a DJI Tello drone flown using a joystick controller. 

This package uses ```catkin build```. Tested on Ubuntu 20.04.
## 1. Prerequisites
### If you did not do a full noetic install, ensure these is installed.
```
sudo apt install ros-noetic-cv-bridge
sudo apt install ros-noetic-image-transport
sudo apt install ros-noetic-camera-info-manager
sudo apt install ros-noetic-codec-image-transport
```
### Eigen3
```
sudo apt install libeigen3-dev
```
### Pangolin
```
cd ~
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
mkdir build && cd build
cmake ..
make
sudo make install
```
### OpenCV
Check the OpenCV version on your computer (required [at least 3.0](https://github.com/UZ-SLAMLab/ORB_SLAM3)):
```
python3 -c "import cv2; print(cv2.__version__)" 
```
On a freshly installed Ubuntu 20.04.4 LTS with desktop image, OpenCV 4.2.0 is already included. If a newer version is required (>= 3.0), follow [installation instruction](https://docs.opencv.org/4.x/d0/d3d/tutorial_general_install.html) and change the corresponding OpenCV version in `CMakeLists.txt`

### (Optional) `hector-trajectory-server`
Install `hector-trajectory-server` to visualize the real-time trajectory of the camera/imu. Note that this real-time trajectory might not be the same as the keyframes' trajectory.
```
sudo apt install ros-[DISTRO]-hector-trajectory-server
```
## 2. Installation
```
cd ~/catkin_ws/src
git clone git@github.com:jonaug88/orb_slam3_tello_ros.git
cd ../
catkin build
```

## 3. How to use
### Real-time SLAM

In Terminal 1:
```
roscore
```
In Terminal 2:
```
cd ~/catkin_ws
source devel/setup.bash
roslaunch orb_slam3_ros tello.launch
```

In Terminal 3:
```
cd ~/catkin_ws
source devel/setup.bash
roslaunch tello_driver tello_node.launch
```
In Terminal 4:
```
cd ~/catkin_ws
source devel/setup.bash
roslaunch tello_driver joy_teleop.launch
```
### Post-flight SLAM analysis using rosbags
To record the footage do the following: 

In Terminal 1:
```
roscore
```
In Terminal 2:

```
cd ~/catkin_ws
source devel/setup.bash
roslaunch tello_driver tello_node.launch
```
In Terminal 3:
```
cd ~/catkin_ws
source devel/setup.bash
roslaunch tello_driver joy_teleop.launch
```
In Terminal 4:
```
cd ~/catkin_ws/rosbags
source devel/setup.bash
rosbag record -a --clock
```
To perform the SLAM on the recorded footage do the following:

In Terminal 1:
```
roscore
```
In Terminal 2:

```
cd ~/catkin_ws
source devel/setup.bash
roslaunch orb_slam3_ros tello.launch
```
For this you need to change the sim_time parameter to "true" in the tello.launch file:

param name="use_sim_time" value="true"

This way you ensure rosbag plays the video feed correctly according to the timestamps instead of using the computers own clock

In Terminal 3:
```
cd ~/catkin_ws/rosbags
source devel/setup.bash
rosbag play your_recorded_rosbag.bag
```



