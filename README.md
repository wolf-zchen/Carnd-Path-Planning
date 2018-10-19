# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program

## Introduction
In this project a car is safely navigated around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. I am provided with the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car drives as close as possible to the 50 MPH speed limit, which passes slower traffic when possible, note that other cars will try to change lanes too. The car avoids hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. Also the car will not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

   
### Simulator.
You can download the Term3 Simulator which contains the Path Planning Project from the [releases tab (https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2).

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./path_planning`.

## Dependencies

* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```

## Rubric Point

###Compilation

####The code compiles correctly.

No change was made to CMakeLists.txt, code compiles without errors with cmake and make.

###Valid Trajectories

####The car is able to drive at least 4.32 miles without incident..

As shown in the following image, the car ran 15 miles and 19 min without incident.

![image](/img_videos/car_no_incident.png)


####The car drives according to the speed limit.

The reference speed is set to 49.5 mph, as the speed limit is 50 mph. No speed limit violation message seen.

####Max Acceleration and Jerk are not Exceeded.

The acceleration and deceleration is set to around 5 $m/s^2$. Max jerk red message was not seen.

####Car does not have collisions.

The car did not come into contact with any of the other cars on the road.

####The car stays in its lane, except for the time between changing lanes.

The car try to keep in center lane whenever possible, and didn't drive off road.

####The car is able to change lanes

The car change lanes when the there is a slow car in front of it, and it is safe to change lanes (no other cars within 30 $m$ ahead and/or 5 $m$ right/left). Also the car will try to change back to center lane when it is safe.

###Reflection

####Car Identification line 251 to line 319

This part of code is used to identify surrounding cars from sensor fusion data. The code mainly classify the following conditions:

* If there is a car ahead of us which is too slow.
* If there is a car within 30 m ahead and/or 5 m behind us on the left side.
* If there is a car within 30 m ahead and/or 5 m behind us on the right side.

#### Corresponding Strategy line 322 to line 354
After identified the three cases above, what behavior do we choose to perform?

* If there is a car in front of us blocking the traffic:
	* if there is no other cars on the left side and the car is not on the left lane, then it is safe to change lane to the left. MAKE LEFT LANE CHANGE.
	* if there is no other cars on the right side and the car is not on the right lane, then it is safe to change lane to the right. MAKE RIGHT LANE CHANGE.
	* if it is not safe to perform either right lane change or left lane change, then stay in the lane and slow down.

* After the lane change:
	* if no car ahead, and the speed is slower than 49.5 mph, then speed up.
	* if it is safe to change lane back to center, then change back to center lane, which will allow us to either make left or right lane changes if needed.

#### Trajectory line 357 to line 465

This code (mainly referenced the class Q&A) does the calculation of the trajectory based on the speed and lane output from the behavior, car coordinates and past path points.

First, the last two points of the previous trajectory, or the car position if there are no previous trajectory are used in conjunction three points at a far distance to initialize the spline calculation. The coordinates are transformed (shift and rotation) to local car coordinates.

To get a smooth trajectory, the rest of the points are calculated by evaluating the spline and transforming the output coordinates to not local coordinates.


