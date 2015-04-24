# UR5
This document is intented to have a quick and easy setup for using the UR5 robot in ROS.

This readme assumes you have ros-indigo-desktop-full pre-installed on your laptop/PC.

(1) Start with building a catkin workspace with a name you prefer, in this case I use "ur5_ws".

(2) Clone the UR5 repository into your workspace src folder using the following commands:
$ cd ~/ur5_ws/src/
$ git clone https://github.com/ros-industrial/universal_robot.git

(3) To use moveIt I needed to install a few extra packages, using the following command:
$ sudo apt-get update
$ sudo apt-get install ros-indigo-moveit-commander ros-indigo-moveit-planners ros-indigo-moveit-plugins ros-indigo-moveit-ros ros-indigo-moveit-setup-assistant

(4) Start a new terminal and build your workspace using the following commands:
$ cd ~/ur5_ws/
$ source devel/setup.bash
$ catkin_make

