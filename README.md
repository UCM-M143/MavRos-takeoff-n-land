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

```
sudo apt install ros-kinetic-mavros ros-kinetic-mavros-extras
sudo /opt/ros/kinetic/lib/mavros/install_geographiclib_datasets.sh
```

## Download and run PX4 Gazebo simulator
Prepare tools for building PX4
```
sudo apt-get update
sudo apt-get install python-argparse git-core wget zip python-empy qtcreator cmake build-essential genromfs -y
sudo apt-get install ant protobuf-compiler libeigen3-dev libopencv-dev openjdk-8-jdk openjdk-8-jre clang-3.5 lldb-3.5 apt install python-toml python-numpy -y
sudo apt-get install python-pip -y
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

![Alt text](/px4_gazebo.png?raw=true "Screenshot of successful run")

## Create ROS work space, make and run our ROS node
Open a new Terminal and run:
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
### Run roscore, mavros and this ros nodde

In the terminal you have, run
```
roscore
```
Open another two terminals, inside one you run
```
roslaunch mavros px4.launch fcu_url:="udp://:14540@127.0.0.1:14540"
```
inside another, run the ROS node you just built
```
cd ~/catkin_ws
source ./devel/setup.bash 
rosrun ex1 takeoff_n_land 
```
![Alt text](/final_res.png?raw=true "Screenshot of successful run")

[1] http://wiki.ros.org/kinetic/Installation/Ubuntu

[2] https://dev.px4.io/en/simulation/ros_interface.html

[3] https://dev.px4.io/en/simulation/gazebo.html
