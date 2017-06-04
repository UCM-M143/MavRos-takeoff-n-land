# MavRos-takeoff-n-land
## How to establish the environment?
https://docs.google.com/document/d/1j-0oNPsMVonIDlRqUEddmXbyBflnM9hX7zPVfSpe64U/edit?usp=sharing

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
