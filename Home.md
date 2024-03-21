# Lab 9: Take Control - The PID Feedback Controller
* Alexis Puckett
* Hannah Markwell

March 21, 2024

## Project Summary:

For this lab we are planning to combine the previous lab with the Arduino robocar and pair it with a control system that allows the car to keep a certain distance away from an object or wall that may get in the car’s pathway. The goal is to add a proportional-integral-derivative (PID) controller on the Arduino robocar. A PID is commonly use by engineers. It calculates the error between the setpoint (SP) and the measured value. The error is then fed into the amplifier, integrator, and differentiator which are summed and create a parameter that changes the effect of the overall process. In addition to a PID, we will use the ultrasonic distance as a sensor to detect objects and keep the car away.  

## Design/Methods:

For this lab we need:
* A computer running Arduino IDE: MacBook Air
* RedBoard, ultrasonic sensor, two motors, motor driver, and a battery pack: SparkFun Inventor's Kit

**Part One: PID Use**

We used the robocar that we assembled in Lab 8 using the RedBoard, ultrasonic sensor, two motors, motor driver, and the wheels from the SparkFun Inventor's Kit.

We first installed the PID_v2 library onto Arduino IDE running on the computer and opened the code that we used in Lab 8 to run the robocar by entering a direction and speed when prompted. We then needed to edit the code to include PID control. The complete code is labeled as lab9part1_copy_20240321175156.ino in the files of this repository. 

We began by including the PID_v2 library. 
```c++
#include <PID_v2.h>
```
We then, defined the setpoint, measurement, output, Kp, Ki, and Kd variables. We also created the PID instance. This code is all before the setup function in the code to run the robocar.
```c++
double MEAS = 0;
double OUT = 0;
double setpoint = 0;
double Kp = 0;
double Ki = 0;
double Kd = 0;
PID myPID(&MEAS, &OUT, &setpoint, Kp, Ki, Kd, DIRECT);
```
Within the setup function, we initialized the PID.
```c++
myPID.SetTunings(Kp, Ki, Kd);
myPID.SetMode(AUTOMATIC);
```
In the loop function, we ran the PID after the Arduino obtains the distance from the ultrasonic sensor. 
```c++
myPID.Compute();// new
```
We then printed the setpoint, measurement, and output to the serial port so that we could see that the PID was working.
```c++
 Serial.print(setpoint);
 Serial.print(",");
 Serial.print(MEAS);
 Serial.print(",");
 Serial.println(OUT);
```
We then connected the Arduino RedBoard and the robocar to the computer to make sure the PID control worked properly. 

**Part Two: Keep your Distance**

We then added to the code we worked on in Part One to make it so that the robocar will move forward until an object is within 15cm of the car, which in that case it will move backwards until the object is over 15cm away again. Our code for this can be found in the files of this repository under lab9part2_copy_20240306160801.ino.

To do this, we began by changing the values of setpoint, Kp, Kd, and Ki.
```c++
double MEAS = 0;
double OUT = 0;
double setpoint = 15;
double Kp = 10;
double Ki = 1;
double Kd = 1;
``` 
We then used the map function in Arduino IDE to map the values from the PID to the speed of the motors and set the measurement value equal to the distance the ultrasonic sensor measures. We put this within the loop function of our previous code, after the ultrasonic sensor gets the distance value. 

```c++
motorSpeed = map(OUT, 0, 225, 150, 255);
MEAS = distance;
``` 
Within the loop function, we also added an if-else statement that will make the car move forward when the measurement is greater than the setpoint of 15cm, and make it move backwards if the measurement is less than 15cm.      
```c++                 
 if (MEAS > setpoint)    
  { 
   botDirection = "f";
  }
   else
  {
   botDirection = "b";
  }
``` 
We then connected the RedBoard and the robocar to the computer running Arduino IDE and tested our code. 

## Results/Conclusions:
