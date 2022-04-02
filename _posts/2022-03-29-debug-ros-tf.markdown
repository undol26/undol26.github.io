---
layout: post
title:  "[ROS] pcl debug. undefined reference to symbol '_ZN2tf17TransformListenerD1Ev']?"
date:   2022-03-29
category: ROS
---

<p class="intro">undefined reference to symbol 은 cmake에서 뭔가를 missing 했을 때 일어나는 에러메시지이다. </p>

<img src="/public/img/debug/debug-ros-tf1.png" alt=""/> 
{%- highlight ruby -%}
/usr/bin/ld: CMakeFiles/_test_node.dir/src/Test/Test.cpp.o: undefined reference to symbol '_ZN2tf17TransformListenerD1Ev'
//opt/ros/melodic/lib/libtf.so: error adding symbols: DSO missing from command line
{%- endhighlight -%}

CMake를 봐도 특별히 없는 게 없었다. 
한 줄씩 지워가면서 확인을 해봤는데. 원인은 PCL 이었다.

내가 지금까지 관성적으로 사용하던 PCL은 `find_package(PCL 1.2 REQUIRED)` 이었는데, 이를 그냥 pcl-ros로 변경하니 해결되었다.


| 변경전      | 변경후 |
| ----------- | ----------- |
| <img src="/public/img/debug/debug-ros-tf2.png" alt=""/>     | <img src="/public/img/debug/debug-ros-tf3.png" alt=""/>       |


