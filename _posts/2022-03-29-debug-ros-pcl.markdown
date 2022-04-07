---
layout: post
title:  "[Debug] PCL - undefined reference to symbol '_ZN2tf17TransformListenerD1Ev']?"
date:   2022-03-29
category: Debug
---

undefined reference to symbol 은 cmake에서 뭔가를 missing 했을 때 일어나는 에러메시지이다.

{%- highlight ruby -%}
/usr/bin/ld: CMakeFiles/_test_node.dir/src/Test/Test.cpp.o: undefined reference to symbol '_ZN2tf17TransformListenerD1Ev'
//opt/ros/melodic/lib/libtf.so: error adding symbols: DSO missing from command line
{%- endhighlight -%}

CMake를 봐도 특별히 없는 게 없었다. 
한 줄씩 지워가면서 확인을 해봤는데. 원인은 <span style="color:#f92672">PCL library</span> 이었다.

내가 지금까지 관성적으로 사용하던 PCL은 <span style="color:#f92672">`find_package(PCL 1.2 REQUIRED)`</span> 이었는데, 이를 그냥 <span style="color:#f92672">pcl-ros</span>로 변경하니 해결되었다.


### 변경 전
```cmake
find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  geometry_msgs
  roscpp
  roslib
  sensor_msgs
  std_msgs
)

find_package(PCL 1.2 REQUIRED)
find_package(Boost REQUIRED COMPONENTS system)
find_package(Eigen3 REQUIRED)
find_package(OpenCV REQUIRED)

###################################
## catkin specific configuration ##
###################################
catkin_package(
 INCLUDE_DIRS include ${PCL_INCLUDE_DIRS} ${Boost_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIR} ${OpenCV_INCLUDE_DIRS}
 LIBRARIES ${PROJECT_NAME}
 CATKIN_DEPENDS 
  cv_bridge 
  geometry_msgs 
  roscpp 
  roslib 
  sensor_msgs 
  std_msgs
 DEPENDS system_lib
)
```
### 변경 후
```cmake
find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  geometry_msgs
  roscpp
  roslib
  sensor_msgs
  std_msgs
  pcl_ros
)

find_package(Boost REQUIRED COMPONENTS system)
find_package(Eigen3 REQUIRED)
find_package(OpenCV REQUIRED)

###################################
## catkin specific configuration ##
###################################
catkin_package(
 INCLUDE_DIRS include ${Boost_INCLUDE_DIRS} ${EIGEN3_INCLUDE_DIR} ${OpenCV_INCLUDE_DIRS}
 LIBRARIES ${PROJECT_NAME}
 CATKIN_DEPENDS 
  cv_bridge 
  geometry_msgs 
  roscpp 
  roslib 
  sensor_msgs 
  std_msgs
  pcl_ros
 DEPENDS system_lib
)
```