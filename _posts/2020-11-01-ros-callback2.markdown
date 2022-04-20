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

## Async spinner v.s. Callback queue
Asynscpinner는 두 가지 형태로 선언 한다.

```cpp
ros::AsyncSpinner::AsyncSpinner(uint32_t thread_count)
ros::AsyncSpinner::AsyncSpinner(uint32_t thread_count, CallbackQueue * queue)
```

이 두 방식의 차이는 multi-thread를 관리하는 Asynscpinner의 `queue`를 설정 할지 (`2번 방법`), 말지 (`1번 방법`)이다. 

`1번 방법`은 각 객체에서 생성된 thread를 `ros master`를 통해 `global queue`로 관리한다. 

반대로 `2번 방법`은 `local queue`로 관리한다.

웬만하면 `local queue` 방식인 `2번 방법`, `CallbackQueue` 사용을 권한다. 이는 코딩할 때 `global` 변수를 잘 사용하지 않는 것과 같다.

`thread_count`는 사용할 thread의 개수이다. [1번 포스팅](https://undol26.github.io/ros/2020/10/30/ros-callback1.html)에 관련 내용을 언급하였으니 참조하면 된다.

<br>
Asyncspinner의 주요 api는 `start()`와 `stop()`이 있다. 당연히 `start()`를 하면 `multi-thread` 기능이 시작되고, 흔히 말하는 `subbscriber`, `publisher` 등을 사용할 수 있다. 

반대로 `stop()`은 `multi-thread`기능을 멈춘다. 하지만 특수한 경우를 제외하고 프로그램이 종료될 때 까지 명시적으로 `subscriber`, `publisher`를 끌 일은 없기 때문에, 굳이 `stop()`을 할 필요는 없다. 프로그램이 종료될 때 ROS가 소멸자에서 자동으로 `stop()`을 부른다.

## Thread의 소멸
ROS에서 생성된 thread들은 class를 소멸할 때 자동으로 소멸한다. 즉 이 부분은 사용자가 신경 쓸 필요 없다.

만일 소멸자를 불렀을 때, `queue`에 데이터가 쌓여 있는 경우에는 (ex. subscirber에서 세마포어를 걸고 delay), 이 데이터를 다 처리하고 소멸한다. 

즉 소멸할 때 delay가 발생한다.
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
