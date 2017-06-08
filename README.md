# MavRos takeoff and land
## Install ROS [1]
Open a terminal and then copy and paste the following commands into it. (each block can be copied and pasted at once)
```
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
```
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116
sudo apt-get update
sudo apt-get install ros-kinetic-desktop-full
```
```
sudo rosdep init
rosdep update
```
```
echo "source /opt/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
## Download and run PX4 Gazebo simulator
Prepare tools for building PX4
```
sudo apt-get update
sudo apt-get install python-argparse git-core wget zip python-empy qtcreator cmake build-essential genromfs -y
sudo apt-get install ant protobuf-compiler libeigen3-dev libopencv-dev openjdk-8-jdk openjdk-8-jre clang-3.5 lldb-3.5 -y
sudo apt-get install python-pip
sudo -H pip install pandas jinja2
```
Download source files from Github
```
cd /tmp
mkdir me190
cd me190
git clone https://github.com/PX4/Firmware.git
cd Firmware
git submodule update --init --recursive
```
Wait until downloading finish correctly, then run
```
make posix_sitl_default gazebo
```
A window will pop up shows a quadcoter
![Alt text](MavRos-takeoff-n-land/px4_gazebo.png?raw=true "Screenshot of successful run")

## Create ROS works space, make and run our ROS node
```
cd ~
mkdir catkin_ws
cd  catkin_ws
mkdir src
cd src
```
Create an empty ROS package called ```ex1``` node which depends on ```mavros```. It will create CMakeLists.txt and packages.xml for you.
```
catkin_create_pkg ex1 mavros
```
Then you can pull the given c++ source code to your ```ex1``` package folder
```
cd ex1/
wget https://github.com/UCM-ME190/MavRos-takeoff-n-land/raw/master/takeoff_n_land.cpp
```
After that, let's change the generated CMakeLists.txt file so that ros knows how to build this package
```
gedit CMakeLists.txt
```
Put those two lines at the end of the CMakeLists.txt file.
```
add_executable(takeoff_n_land takeoff_n_land.cpp)
target_link_libraries(takeoff_n_land ${catkin_LIBRARIES})
```
Here you have finished modifing files. Build this ROS package by
```
cd ~/catkin_ws
catkin_make
```
run following command so that rosrun can find our new nodes in your ```ex1``` package
```
source ./devel/setup.bash 
```
Open another two terminals, inside one you run
```
roscore
```
inside another, run the ROS node you just built
```
rosrun ex1 takeoff_n_land 
```

[1] http://wiki.ros.org/kinetic/Installation/Ubuntu
