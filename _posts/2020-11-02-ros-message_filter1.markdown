---
layout: post
title:  "[ROS] message filter"
date:   2020-11-02
category: ROS
---

## Intro.
이번 포스팅은 ros message filter에 대한 글이다.

`ROS`를 이용해서 두 개 이상의 같은/다른 종류의 센서를 사용할 때 `synchronize` 작업이 필요할 때가 있다. 

이 때 사용할 수 있는 것이 바로 `ros::message_filter` 이다.

<br> 
역시 자세한 튜토리얼은 [reference](https://wiki.ros.org/message_filters)에 소개되어 있고, 개념만 정리하려 한다.


## 기본 문법

```cpp
#include <message_filters/subscriber.h>
#include <message_filters/time_synchronizer.h>
#include <sensor_msgs/Image.h>
#include <sensor_msgs/CameraInfo.h>
 
using namespace sensor_msgs;
using namespace message_filters;

void callback(const ImageConstPtr& image, const CameraInfoConstPtr& cam_info)
{
	// Solve all of perception here...
}
 
int main(int argc, char** argv)
{
	ros::init(argc, argv, "vision_node");

	ros::NodeHandle nh;

	message_filters::Subscriber<Image> image_sub(nh, "image", 1);
	message_filters::Subscriber<CameraInfo> info_sub(nh, "camera_info", 1);
	TimeSynchronizer<Image, CameraInfo> sync(image_sub, info_sub, 10);
	sync.registerCallback(boost::bind(&callback, _1, _2));

	ros::spin();

	return 0;
 }
```

기본 문법은 바로 알아볼 수 있다. 이 예제는 `Image`와 `CameraInfo`를 `synchronize`하는 예제이다.

이 때 `image_sub()`, `info_sub()`의 `queue size`는 $1$로 해서, 바로 내보낼 수 있게 했고, 두 개를 합치는 `sync()`의 `queue size`는 $10$으로 넉넉한 값을 설정한다.

그리고 두 센서 메시지를 실제로 동작시키는 함수를 `callback()` 함수에 구현한다.

## time synchronize exact v.s. approx.
`ExactTime Policy`는 정확한 동기화를 해서, 콜백은 정확한 타임 스탬프를 사용하여 지정된 모든 채널에서 메시지가 수신 된 경우에만 호출된다.

```cpp
...
typedef sync_policies::ExactTime<Image, CameraInfo> MySyncPolicy;
// ExactTime takes a queue size as its constructor argument, hence MySyncPolicy(10)
Synchronizer<MySyncPolicy> sync(MySyncPolicy(10), image_sub, info_sub);
sync.registerCallback(boost::bind(&callback, _1, _2));
...
```

<br>
반면에, `approximate time policy`는 적응형 알고리즘을 사용하여 타임 스탬프를 기반으로 메시지를 동기화시킨다.
```cpp
...
typedef sync_policies::ApproximateTime<Image, Image> MySyncPolicy;
// ApproximateTime takes a queue size as its constructor argument, hence MySyncPolicy(10)
Synchronizer<MySyncPolicy> sync(MySyncPolicy(10), image1_sub, image2_sub);
sync.registerCallback(boost::bind(&callback, _1, _2));
...
```

## 실제 코드 사용 방법
하지만 검색되는 대부분의 예제는 주로 `main`에서 노드핸들러를 만들고, `message_filter`를 선언하고 `callback` 함수를 부르는데, 실제로 우리가 사용할 프로젝트 등에서는, 어떻게 main에다가 다 할수 있겠는가..

<br>
세 가지 센서(`image`, `lds`, `odometry`)의 값을 융합한다고 한다. 앞의 두 센서는 `sensor_msgs` 타입이고, 마지막은 `nav_msgs` 타입이다.

이 때 `header`와 `source file` 분리는 다음과 같이 한다.

### header file
```cpp
#include <message_filters/subscriber.h>
#include <message_filters/sync_policies/approximate_time.h>
#include <nav_msgs/Odometry.h>
#include <sensor_msgs/Image.h>
#include <sensor_msgs/LaserScan.h>
#include <ros/ros.h>

...
ros::NodeHandle m_nodeHandler;
message_filters::Subscriber<sensor_msgs::Image> cameraSub;
message_filters::Subscriber<sensor_msgs::LaserScan> ldsSub;
message_filters::Subscriber<nav_msgs::Odometry> odomSub;

typedef message_filters::sync_policies::ApproximateTime<sensor_msgs::Image, sensor_msgs::LaserScan, nav_msgs::Odometry> syncPolicy;

typedef message_filters::Synchronizer<syncPolicy> sensorSync;
boost::shared_ptr<sensorSync> m_pSensorSync;

std::string m_cameraTopicName;
std::string m_ldsTopicName;
std::string m_odomTopicName;
```

### source file
```cpp
void MyClass::registerMyCallback()
{
	m_cameraTopicName = "YOUR_CAMERA_TOPIC_NAME";
	m_ldsTopicName = "YOUR_LDS_TOPIC_NAME";
	m_odomTopicName = "YOUR_ODOM_TOPIC_NAME";
    cameraSub.subscribe(m_nodeHandler, m_cameraTopicName, 10);
    ldsSub.subscribe(m_nodeHandler, m_ldsTopicName, 10);
    odomSub.subscribe(m_nodeHandler, m_odomTopicName, 10);

    m_pSensorSync.reset(new sensorSync(syncPolicy(10), cameraSub, ldsSub, odomSub));
    m_pSensorSync->registerCallback(&DockingStationRecognizer::callback, this);
}
```

## 출처.
- [https://wiki.ros.org/message_filters](https://wiki.ros.org/message_filters)

