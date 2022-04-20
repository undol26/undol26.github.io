---
layout: post
title:  "[ROS] callback - 1. Callbacks and Spinning"
date:   2020-10-30
category: ROS
---

## Intro.
ros callback에 대한 정리이다.

자세한 튜토리얼은 [reference](https://wiki.ros.org/roscpp/Overview/Callbacks%20and%20Spinning)에 소개되어 있고, 개념만 정리하려 한다.

## roscpp single thread - spin
```cpp
ros::spin()
```

`ros::spin()`을 쓰면, single thread로 동작하는 문제가 있다.

single thread이기 때문에 여러 callback 함수가 있을 때, 한 callback 함수가 완료될 때 까지 다른 callback 함수가 실행되지 않는다. 

<br>
즉, 처음 실행된 callback 함수의 작업이 완료가 되지 않는다면, 다른 callback 함수는 영원히 시작되지 않는다. 이는 매우 `치명적인 문제`이다.

단, `rospy spinner`는 애초에 `multi-thread` 이기 때문에 위의 문제가 생기지 않는다.

<br>
**이를 해결 하려면? `asyncspinner`를 사용해야 한다.**

## roscpp multi threads - AsyncSpinner
ROS는 기본적으로 `ros::MultiThreadedSpinner` 또는 `ros::AsyncSpinner`와 같은 `multi-thread`를 지원한다.

[reference](https://wiki.ros.org/roscpp/Overview/Callbacks%20and%20Spinning)에서 ROS에서는 `AsyncSpinner`가 더 유용하며 이를 사용하는 것을 권한다.

ROS에서 사용되는 thread로는 `subscriber`, `timer`, `service server`등 이 있다. (단 `publisher`, `service client` 들은 그냥 객체이다.)

<br>
우리는 ROS의 기본 기능을 사용함으로써 (`subscriber` 등) 자연스럽게 `multi-thread` 프로그래밍을 하고 있었다!

<br>
asyncspinner는 `threaded spinner`이다. 다시말해, 각각의 callback 함수가 하나의 thread를 가지는 객체이다. ([reference](http://docs.ros.org/en/api/roscpp/html/classros_1_1AsyncSpinner.html) 이를 또 다시 말하면, `single thread`에서와 달리, 한 콜백 함수가 실행되고 있어도, 다른 쓰레드에서의 콜백 함수는 실행이 가능하다.

이를 사용하려면, 
```cpp
int main(int argc, char **argv)
{
	​​​​ros::init(argc, argv, "talker_subscribers");
	​​​​ros::NodeHandle nh;
	​​​​ros::AsyncSpinner spinner(0);
	​​​​spinner.start();
	​​​​​​​​
	​​​​ros::Subscriber counter1_sub = nh.subscribe("talker1", 10, callbackTalker1);
	​​​​ros::Subscriber counter2_sub = nh.subscribe("talker2", 10, callbackTalker2);
	​​​​
	​​​​ros::waitForShutdown();
	​​​​// ros::spin(); --> Do not use that anymore!
}
```

`ros::AsyncSpinner` 를 선언해주고, spinner에 원하는 thread의 개수를 적어준다. 0은 `as many threads as` 라고 하니 생각 없이 할 때 좋은 숫자 같다.

마지막에 `ros::spin` 대신에 `ros::waitForShutdown()`을 써주면 된다.

그러면 노드가 제거(kill)되기 전까지 계속 asyncspinner로서 동작하게 된다.

<br>
다음 포스팅은 `custom callback queue`를 이용해서 `asyncspinner`를 적용하는 글을 포스팅 할 예정이다.

## 출처.
- [https://wiki.ros.org/roscpp/Overview/Callbacks%20and%20Spinning](https://wiki.ros.org/roscpp/Overview/Callbacks%20and%20Spinning)
- [http://docs.ros.org/en/api/roscpp/html/classros_1_1AsyncSpinner.html](http://docs.ros.org/en/api/roscpp/html/classros_1_1AsyncSpinner.html)

---
## 관련글.
1. [[ROS] callback - 1. Callbacks and Spinning](https://undol26.github.io/ros/2020/10/30/ros-callback1.html)
2. [[ROS] callback - 2. Custom Callback Queue](https://undol26.github.io/ros/2020/11/01/ros-callback2.html)
