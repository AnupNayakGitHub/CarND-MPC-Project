# CarND-Controls-MPC
Udacity Self-Driving Car Nanodegree Model Pedictive Control Project.

In this project, the vehicle needs to be driven using the Model Predictive Control (MPC) technique. Additionally, the communicaiton latency of 100 miliseconds should be taken care.

## Conceptual Steps
* Identify the polynomial that fits the waypoints
* Based on the current state predict the state after the delay/latency
* Use the polynomial to minimize the cost funciton satrting at the predicted state
* Use the computed actuators - steering angle (δ) and throttle(a) - to conrol the vehicle

## Fitting the points with Polynomial
As suggested in the lecture, the waypoints from the simulator fit into a 3rd order polynomial. Since the simulator provides global coordinates and for visualization purpose we need to tranform into the vehicle's cordinate system, the global coordiates are transformed into car's coordinate system and used as reference throughout.

The CppAD library is used to compute the 3rd order polynomial.

## Compute the initial state 
As the actuator will take effect after a delay of 100 miliseconds, the initial state is the predicted state of the vehicle after 100 ms. The formula for each dimension of the predicated state is in `main.cpp` file.

## Minimizing the cost function
The MPC uses the ipopt library to compute actuators by minimizing cost. The lecture suggested techniques were used, however, few critical points are mentioned below:

* After trial and error, a value 10 is choosen for N and 100 ms for time deference. Coincidentally the observerved value for time difference matches with the delayed time which also eliminates the need for extra computation. Some of the tried value pairs are 20/0.1s, 20/0.05s, 10/0.05s, 50/0.1s. 
* After an extensive web search and manual tuning, the cost multiplication factors are choosen as in `MPC.cpp\h` files.

## Using the computed actuators
The cost minimization function returns both the actuation values- δ, and a. These values are fed to the simulator. To mimic the latency/delay, the current thread delays the 100ms before sending the actuator signals to the simulator.

## Visualization
* As suggested by the boilerplate code, the the reference waypoints wer fed to be displayed yellow in the simulator. Also, the points for the predicted path were fed to be shown in green in the simulator.
* A sort video is capture and can be seen here.

## Possible Improvements
* The computation time supposed to be part of the latency. This can be factored into the latency.
* Instead of sleeping the thread for 100ms, it can hold a stream of actuator values and it should feed the value calculated 100ms before.
* The tuning parmaters should be automatically tuned. The values may work for the simulator but is not appropriate for real vehicle.

## End node
Please look into the code for additional information.
