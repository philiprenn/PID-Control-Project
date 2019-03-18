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

Fellow students have put together a guide to Windows set-up for the project [here](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/files/Kidnapped_Vehicle_Windows_Setup.pdf) if the environment you have set up for the Sensor Fusion projects does not work for this project. There's also an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3).

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## Code Style

[Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Rubric

1. Describe the effect each of the P, I, D components had in your implementation.

P - Proportional component accounts the instantaneuos error. This error the difference between the ideal and actual value. The P gain will help steer the vehicle back in the direction of the ideal position on the track.

I - Integral component is responsible for decreasing the error over time. This integral error accumulates over time to decide how aggresive the correction should be based on the cummulative value of previous errors.

D - Derivative component will account for the future errors. Based on the errors current rate of change, the differential portion of the controller will determine how quickly the vehicle is moving away from or towards the ideal position. This will help the vehicle react more aggressively as the vehicle moves futher from the ideal position and visa-versa.

The proportional component was responsible for keeping the oscillations steady. The differential component helps decrease the oscillations. The integral component

2. Describe how the final hyperparameters were chosen.

I started with the values from the lesson and then chose to manually implement the twiddle algorithm. By adjusting these values "by hand" it ultimately gave me a better understanding of how each component of PID controller affects the output. I first set a limiter on the throttle because I noticed the oscillations increase as the vehicle sped up. This allowed me to tune the controller for a specific speed range. I also decreased the throttle inversely proportionally to the angle. So as the steering angle hit a threshold, the vehicle would slow down to decrease the amplitude of the oscilaltions.

After setting a throttle limiter, I began adjusting the PID parameters. Since I had set a throttle limiter I started from scratch and set all the parameters to zero. I started by increasing the "P" until the response was a relatively steady oscillation around the first turn. Then, I increased the "D" gain until the oscillations got smaller. I introduced the "I" value last as it is used as the fine-tuning parameter. I continued to adjust these values in this order by increasing/decreasing their values by a similar factor of the twiddle algorithm (1.1 or 0.9). I ended up settling on P = 0.08, I = 0.009, and D = 2.2.
