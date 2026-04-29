<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

This Solution reads input from the BFD1000 5-channel IR to control the robot motor wheels.
The sensor has 5 inputs, S1, S2, S3, S4, S5, giving a 1 output for blanc color and 1 for white color. The CLP pin outputs a 1 when a collision occurs, and the near pin outputs a 1 when an obstacle is detected.
The outputs are the left and right motors' ON/OFF signal, and the direction control. We also add velocity since we are doing PWM directly from the clock pin.
+----+----+----+----+----+-----+------++-----+------+-----+------+----+----+---------------------+<br>
| S1 | S2 | S3 | S4 | S5 | CLP | Near || EnL | DirL | EnR | DirR | VL | VR |     Observation     |<br>
+----+----+----+----+----+-----+------++-----+------+-----+------+----+----+---------------------+<br>
| x  | x  | x  | x  | x  |  1  |   x  ||  0  |  x   |  0  |   x  | x  |  x | Stop                |<br>
+----+----+----+----+----+-----+------++-----+------+-----+------+----+----+---------------------+<br>
| x  | x  | x  | x  | x  |  x  |   1  ||  0  |  x   |  0  |   x  | x  |  x | Stop                |<br>
+----+----+----+----+----+-----+------++-----+------+-----+------+----+----+---------------------+<br>
| 1  | 1  | 0  | 1  | 1  |  0  |   0  ||  1  |  1   |  1  |   1  | 1  |  1 | Forward full speed  |<br>
+----+----+----+----+----+-----+------++-----+------+-----+------+----+----+---------------------+<br>
| 1  | 0  | 0  | 1  | 1  |  0  |   0  ||  0  |  x   |  1  |   1  | x  |  0 | Low correction left |<br>
+----+----+----+----+----+-----+------++-----+------+-----+------+----+----+---------------------+<br>
| 1  | 0  | 1  | 1  | 1  |  0  |   0  ||  0  |  x   |  1  |   1  | x  |  1 | High correction left|<br>
+----+----+----+----+----+-----+------++-----+------+-----+------+----+----+---------------------+<br>
| 0  | 0  | 1  | 1  | 1  |  0  |   0  ||  1  |  0   |  1  |   1  | 0  |  1 | rotate left         |<br>
+----+----+----+----+----+-----+------++-----+------+-----+------+----+----+---------------------+<br>
| 0  | 1  | 1  | 1  | 1  |  0  |   0  ||  1  |  0   |  1  |   1  | 1  |  1 | full rotate left    |<br>
+----+----+----+----+----+-----+------++-----+------+-----+------+----+----+---------------------+<br>
| 1  | 1  | 0  | 0  | 1  |  0  |   0  ||  1  |  1   |  0  |   x  | 0  |  x | Low correction right|<br>
+----+----+----+----+----+-----+------++-----+------+-----+------+----+----+---------------------+<br>
<br>
The logical equations are then :<br>
Left motor control:<br>
EnL = (s1&s2&s3&~s4)|(s1&s2&s3&~s5)|(s1&s2&~s3&s5)|(s1&s2&~s4&s5)|(~s1&~s2&~s3&~s4&~s5)|(~s1&s3&s4&s5)<br>
DirL = (s1&s2&s3&~s4)|(s1&s2&s3&~s5)|(s1&s2&~s3&s5)|(s1&s2&~s4&s5)|(~s1&~s2&~s3&~s4&~s5)<br>
VL = (s1&s2&s3&s4&~s5)|(s1&s2&s3&~s4&s5)|(s1&s2&~s3&s4&s5)|(~s1&s2&s3&s4&s5)<br><br>
Right motor control:<br>
EnR = (s1&s2&s3&~s5)|(s1&~s2&s4&s5)|(s1&~s3&s4&s5)|(~s1&~s2&~s3&~s4&~s5)|(~s1&s3&s4&s5)|(~s2&s3&s4&s5)<br>
DirR = (s1&~s2&s4&s5)|(s1&~s3&s4&s5)|(~s1&~s2&~s3&~s4&~s5)|(~s1&s3&s4&s5)|(~s2&s3&s4&s5)<br>
VR = (s1&s2&s3&s4&~s5)|(s1&s2&~s3&s4&s5)|(s1&~s2&s3&s4&s5)|(~s1&s2&s3&s4&s5)<br>
<br><br>
## How to test

Explain how to use your project

## External hardware

connect the BFD1000 sensor to the appropriate pins and the motor drivers to the out puts
