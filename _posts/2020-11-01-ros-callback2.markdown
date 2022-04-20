---
layout: post
title:  "[ROS] callback - 2. Custom Callback Queue"
date:   2020-11-01
category: ROS
---

## Intro.
ros callback에 대한 정리이다.

이전 [포스팅](https://undol26.github.io/ros/2020/10/30/ros-callback1.html)에서 `single thread`가 아닌 `multi thread`를 사용해야 하는 이유를 언급하였다.

이번 포스팅에서는 `callback queue`를 사용한 `async spinner`의 사용 방법에 대해 설명한다.

`queue`의 개념처럼 queue에 쌓인 callback 값을 내보내는 느낌으로 접근하면 된다.

<br> 
역시 자세한 튜토리얼은 [reference](https://wiki.ros.org/roscpp/Overview/Callbacks%20and%20Spinning)에 소개되어 있고, 개념만 정리하려 한다.

## callAvailable()  v.s. callbackOne()
callbackqueue에서 사용할 수 있는 method는 크게 2개로 정리할 수 있다.

`callAvailable()`  v.s. `callbackOne()`

[함수에 대한 설명](https://docs.ros.org/en/api/roscpp/html/classros_1_1CallbackQueue.html)을 보면 

* `callAvailable()`: 현재 대기열에 있는 모든 콜백을 호출한다.
* `callbackOne()`: 대기열 앞에 단일 콜백을 팝하고 호출한다.

다시말해 queue에 쭉 쌓아두고, 한 번에 내보낼 것인가(`callAvailable()`) v.s. 한 개씩 내보낼 것인가(`callbackOne()`)에 대한 선택으로 정리할 수 있다.

<br>
이 callbackqueue를 이용해서 사용하는 방식을 wiki ros에서는 4가지 정도로 소개하는데, 나는 3번째 방식을 선호해서 사용한다.
```cpp
#include <ros/callback_queue.h>
	// ...
	// 중략
	// ...
	ros::CallbackQueue my_callback_queue;

	ros::AsyncSpinner spinner(0, &my_callback_queue);
	spinner.start();
```

## 출처.
- [wiki.ros.org/roscpp/Overview/Callbacks%20and%20Spinning](https://wiki.ros.org/roscpp/Overview/Callbacks%20and%20Spinning)
- [docs.ros.org/en/api/roscpp/html/classros_1_1CallbackQueue.html](https://docs.ros.org/en/api/roscpp/html/classros_1_1CallbackQueue.html)
- [levelup.gitconnected.com/ros-spinning-threading-queuing-aac9c0a793f](https://levelup.gitconnected.com/ros-spinning-threading-queuing-aac9c0a793f)

---
## 관련글.
1. [[ROS] callback - 1. Callbacks and Spinning](https://undol26.github.io/ros/2020/10/30/ros-callback1.html)
2. [[ROS] callback - 2. Custom Callback Queue](https://undol26.github.io/ros/2020/11/01/ros-callback2.html)
