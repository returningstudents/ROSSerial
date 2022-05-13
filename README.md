# Servo motor

## Servo motor 제
젯슨나노(publish) 입력한 각도를 rosserial을 이용하여 아두이노 나노(subscribe)로 전송하여 servo motor의 각도를 조절한다

### 아두이노
```c
#if (ARDUINO >= 100)
 #include <Arduino.h>
#else
 #include <WProgram.h>
#endif

#include <Servo.h> 
#include <ros.h>
#include <std_msgs/UInt16.h>

ros::NodeHandle  nh;

Servo servo;
//서보 변수 선언

void servo_cb( const std_msgs::UInt16& cmd_msg){
  servo.write(cmd_msg.data); //set servo angle, should be from 0-180  
  digitalWrite(13, HIGH-digitalRead(13));  //toggle led  
}


ros::Subscriber<std_msgs::UInt16> sub("servo", servo_cb);

void setup(){
  pinMode(13, OUTPUT);

  nh.initNode();
  nh.subscribe(sub);
  
  servo.attach(9); //attach it to pin 9
}

void loop(){
  nh.spinOnce();
  delay(1);
}

### 젯슨 나노
```
ROS를 실행하기 위해 터미널창에 아래 코드를 실행시킨다.
```
$ roscore
```
새로운 터미널 창에 아래 코드를 작성한다.
```
$ rosrun rosserial_python serial_node.py _port:=/dev/ttyACM0
```
새로운 터미널 창을 열고 각도 조절하는 코드를 작성한다.
```
$ rostopic pub /servo std_msgs/UInt16 90 --once  // UInt16 xx <- 각도 조절, --once 한번만 실행
```

<img src="https://user-images.githubusercontent.com/65072588/168052111-50469276-72cb-4750-bd87-a6a73a738647.gif">


## 다중 Servo motor 제어

젯슨나노(publish) 입력한 각도를 rosserial을 이용하여 아두이노 나노(subscribe)로 전송하여 3개의 servo motor의 각도를 조절한다

### 아두이노
```c
#if (ARDUINO >= 100)
 #include <Arduino.h>
#else
 #include <WProgram.h>
#endif

#include <Servo.h> 
#include <ros.h>
#include "std_msgs/MultiArrayLayout.h"
#include "std_msgs/MultiArrayDimension.h"

#include "std_msgs/UInt16MultiArray.h"

ros::NodeHandle  nh;

Servo servo1;
Servo servo2;
Servo servo3;

void servo_cb( const std_msgs::UInt16MultiArray&  cmd_msg){
  servo1.write(cmd_msg.data[0]); //set servo angle, should be from 0-180  
  servo2.write(cmd_msg.data[1]); 
  servo3.write(cmd_msg.data[2]);
  digitalWrite(13, HIGH-digitalRead(13));  //toggle led  
}


ros::Subscriber<std_msgs::UInt16MultiArray> sub("servo", servo_cb);

void setup(){
  pinMode(13, OUTPUT);

  nh.initNode();
  nh.subscribe(sub);


  servo1.attach(7); //attach it to pin7
  servo2.attach(8);//attach it to pin8
  servo2.attach(9);//attach it to pin9
}

void loop(){
  nh.spinOnce();
  delay(1);
}
```

### 젯슨나노
ROS를 실행하기 위해 터미널창에 아래 코드를 실행시킨다.
```
$ roscore
```
새로운 터미널 창에 아래 코드를 작성한다.
```
$ rosrun rosserial_python serial_node.py _port:=/dev/ttyACM0
```
새로운 터미널 창을 열고 각도 조절하는 코드를 작성한다.
```
$ rostopic pub servo std_msgs/UInt16MultiArray '{data: [<angle1>, <angle2>]}'  // UInt16 xx <- 각도 조절, --once 한번만 실행
```

<img src="https://user-images.githubusercontent.com/65072588/168216663-8aeeaef2-1a92-4fe0-93be-b0c7245faa9a.gif">
