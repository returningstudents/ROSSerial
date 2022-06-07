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
