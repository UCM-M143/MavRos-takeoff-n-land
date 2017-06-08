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
# prepare tools for building PX4
```
sudo apt-get update
sudo apt-get install python-argparse git-core wget zip python-empy qtcreator cmake build-essential genromfs -y
sudo apt-get install ant protobuf-compiler libeigen3-dev libopencv-dev openjdk-8-jdk openjdk-8-jre clang-3.5 lldb-3.5 -y
sudo apt-get install python-pip
sudo -H pip install pandas jinja2
```

```
cd /tmp
mkdir me190
git clone https://github.com/PX4/Firmware.git
cd Firmware
git submodule update --init --recursive
```
Wait until it download finish correctly, then run
```
make posix_sitl_default gazebo
```
## Create works space for ROS
```
cd ~
mkdir catkin_ws
cd  catkin_ws
mkdir src
cd src
git clone
cd ..
```
build this ROS node
```
catkin_make
```
run following command so that rosrun can find our new node
```
source ./devel/setup.bash 
```

run ROS node
```
rosrun ex1 takeoff_n_land 
```


## How to run the code?

The Gazebo should be the one with ROS Kinetic, which is version 7.7.
```
cd ~
mkdir catkin
cd  catkin

mkdir src

cd src/
catkin_create_pkg ex1 mavros

cd ..
catkin_make
cd src/ex1/
wget https://github.com/UCM-ME190/MavRos-takeoff-n-land/raw/master/takeoff_n_land.cpp

gedit CMakeLists.txt
```
Put those two lines at the end of the CMakeLists.txt file.
```
add_executable(takeoff_n_land takeoff_n_land.cpp)
target_link_libraries(takeoff_n_land ${catkin_LIBRARIES})
```

```
cd ~/catkin/
catkin_make
source ./devel/setup.bash 

rosrun ex1 takeoff_n_land 

```
[1] http://wiki.ros.org/kinetic/Installation/Ubuntu
