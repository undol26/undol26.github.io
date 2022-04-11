---
layout: post
title:  "[opencv] opencv install"
date:   2021-08-28
category: opencv
---

## Intro.
ubuntu 18.04에서 opencv 3.4.5를 설치하는 방식이다.

나는 ROS 를 쓰는데, 여기에 기본으로 <span style="color:#f92672">opencv 3.4.2</span>버전이 들어있다. 하지만 <span style="color:#f92672">opencv 3.4.5</span>버전 이상을 써야 할 때 강제로 이를 설치하는 방법이다.

## Install
* warning 뜨는 것을 신경 쓰지 않는 방법을 추천함.
* ROS Melodic full package 설치 시, <span style="color:#f92672">opencv 3.2.0</span>이 기본적으로 함께 설치됨.

```bash
# update
sudo apt-get update && sudo apt-get upgrade

# install extra package
sudo apt install -y build-essential cmake pkg-config git
sudo apt install -y libjpeg-dev libtiff5-dev libpng-dev
sudo apt install -y libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev libxvidcore-dev libx264-dev libxine2-dev libv4l-dev v4l-utils libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
sudo apt install -y libgtk-3-dev libatlas-base-dev libeigen3-dev gfortran
sudo apt-get -y install qt5-default

# OpenCV source code download
cd ~
mkdir opencv && cd opencv
wget -O opencv-3.4.5.zip https://github.com/opencv/opencv/archive/3.4.5.zip
wget -O opencv_contrib-3.4.5.zip https://github.com/opencv/opencv_contrib/archive/3.4.5.zip

# After downloading opencv-3.4.5 zip files
unzip opencv-3.4.5.zip
unzip opencv_contrib-3.4.5.zip
cd opencv-3.4.5
mkdir build && cd build

# CMAKE
cmake \
-D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D WITH_TBB=OFF \
-D WITH_IPP=OFF \
-D WITH_1394=OFF \
-D BUILD_WITH_DEBUG_INFO=OFF \
-D BUILD_DOCS=OFF \
-D INSTALL_C_EXAMPLES=ON \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D BUILD_EXAMPLES=OFF \
-D BUILD_TESTS=OFF \
-D BUILD_PERF_TESTS=OFF \
-D WITH_QT=ON \
-D WITH_GTK=OFF \
-D WITH_OPENGL=ON \
-D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-3.4.5/modules \
-D WITH_V4L=ON \
-D WITH_FFMPEG=ON \
-D WITH_XINE=ON \
-D BUILD_NEW_PYTHON_SUPPORT=ON \
-D OPENCV_GENERATE_PKGCONFIG=ON ../

# Build & Install (8: number of cores)
make -j8
sudo make install

# Confirm the OpenCV version
pkg-config --modversion opencv
```

## 사용법.
사용하려는 패키지의 CMAKELIST에 <span style="color:#f92672">find_package(OpenCV 3.4 REQUIRED)</span>를 명시함.

ex. 
```cmake
find_package(catkin REQUIRED COMPONENTS
  cv_bridge
  geometry_msgs
  roscpp
  roslib
  sensor_msgs
  std_msgs
)

find_package(OpenCV REQUIRED)

###################################
## catkin specific configuration ##
###################################
catkin_package(
 INCLUDE_DIRS include ${OpenCV_INCLUDE_DIRS}
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


## Problem1. 만약 stitching 쪽에서 error가 뜬다면?
opencv3.4.5 폴더에서

```bash
rm -rf modules/stitching/
```

부분을 삭제

## Problem2. undefined reference ~~ aruco ~~~
이런식으로 aruco 관련 패키지를 못찾는다면

<span style="color:#f92672">opencv3.4.5/build/modules/</span>에 aruco 폴더가 존재하는지 확인.

이 부분이 없다면 <span style="color:#f92672">build</span> 부분을 지우고, 위의 CMAKE를 다시 해서 생길 때까지 해야 한다.

만약 계속 해도 안된다면 아래 커맨드로 해볼 것.

```bash
cmake \ -D CMAKE_BUILD_TYPE=RELEASE \
 -D CMAKE_INSTALL_PREFIX=/usr/local \
-D BUILD_TESTS=OFF \
-D BUILD_PERF_TESTS=OFF \
-D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-3.4.5/modules ..
```