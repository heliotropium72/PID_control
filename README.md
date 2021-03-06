# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Project description
In this project a PID control for a car's steering angle is implemented.
The main task is choosing the the parameterization.
steering angle = Kp * CTE + Ki * CTE_total + Kd * CTE_diff
where Kp, Ki and Kd are the values for the proportional, integral and differential part and CTE stands for cross track error

Kp controls how strong the PID reacts, Ki can balance biases in the measurements and Kd dampens the oscillation of the steering angle around the optimal value.
The values were chosen empirically by testing different combinations. In theory, it is more efficient to use optimization algorithms like Twiggle.

| Kp | Ki | Kd | throttle | Observation |
|:--:|:--:|:--:|:--:|:----:|
-1 | 0 | 0 | 0.1 |drives generally correctly, but strong oscillation and eventual overshooting
-0.8 | 0 | 0 | 0.1 |
-1 | -0.01 | 0 | 0.1 | overshoots immediately
-1 | -0.001 | 0 | 0.1 | smoother
-3 | 0 | -0.5 | 0.1 | smooth, also in the curves
| | |0.2| fails track
-3 | 0 | -1 | 0.2 | finishes track but slight overshooting
-2 | 0 | -1 | 0.2 | finishes track inside lines
-2 | -0.05 | -1 | 0.2 | immediately off-track
-2 | -0.01 | -1 | 0.2 | immediately off-track
-2 | -0.001 | -1 | 0.2 | almost finishes track; strong oscillation in the curves; more stable on straight lines
-1.5 | 0 | -1 | 0.2 | finishes track inside lines, more oscillation
-2.25 | 0 | -1 | 0.2 | finishes track
-2.25 | 0 | -1.5 | 0.2 | finishes track inside lines
-2.25 | 0 | -2 | 0.2 | finishes track inside lines
-2.25 | 0 | -3 | 0.2 | finishes track inside lines
-2.25 | 0 | -4 | 0.2 | finishes track inside lines
-2.25 | 0 | -5 | 0.2 | finishes track inside lines
-2.25 | 0 | -10 | 0.2 | finishes track inside lines
-3 | -0.001 | -10 | 0.2 | finishes track inside lines
-3 | -0.001 | -10 | 0.3 | falls of bridge after oscillation
-3 | -0.001 | -30 | 0.3 | finishes track (but overshoots)
-2 | 0 | -40 | 0.3 | finishes tracks inside lines

The idealy values depend strongly on the throttle.
For a throttle of 0.3, the selected values are Kp=-2, Ki=0, Kd=-40