# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program

### Goals

The goal of the project is to safely navigate around a virtual highway with other traffic that is driving +- 10 MPH of the 50 MPH speed limit. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible. The car should avoid hitting other cars at all cost as well as drive inside of the marked road lane lines at all times, unless changing lanes. The car should be able to make one complete loop around the 6946m highway. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

The following paragraphs explain the approach taken to solve the problems posed in the project. For the original README and setup instructions, please see [setup.md](setup.md)

## Trajectory planning

### Trajectory prediction

An important part of autonomous navigation in the complex world of highway driving is to determine the location of other vehicles and try to predict what trajectories they may take in the immediate future. This then becomes an important factor in planning the future behavior of the ego vehicle.

In the project this is done in [main.cpp#L255-306](src/main.cpp#L255-306). This is accomplished by determining if there are other vehicles in the immediate vicinity of our vehicle. The inputs that we consider are telemetry and the sensor fusion data that we receive from the simulator. The flags `car_front_lane`, `car_left_lane` and `car_right_lane` designate if there is a vehicle ahead, left or right of us respectively, within the 30m `buffer_dist`.

### Behavior planning

Once we have determined the positions and trajectories of other vehicles on the road, we can plan for the behavior of the ego vehicle. We have taken a simple approach in this project. If there is a car ahead of us that is driving slowly and there's room on the left or right lanes to merge safely, we merge. If there's no room, then we deaccelerate to keep safe distance from the vehicle ahead of us.

On the other hand, if the there's no car ahead of us, but we are not in the center lane, then we merge into the center lane safely. Alternately, if we are at a speed below the maximum speed limit, then we accelerate. The code for behavior planning is in [main.cpp#L308-332](src/main.cpp#L308-332).

### Trajectory generation

Based on the desired behavior, we generate new waypoints for the vehicle to follow. We use the cubic spline interpolation library to derive a smooth trajectory. To make sure that trajectory follows smoothly from previous waypoints, we select the last two points from the left-over previous trajectory. We then add new three new waypoints ([main.cpp#L370-372](src/main.cpp#L370-372)). These points are corrected so that the origin and heading is congruent to the vehicles center and heading. To generate the new points, we copy over the points remaining from the previous trajectory and append new points from the spline that are 30m apart in the x-coordinate. ([main.cpp#404-429](src/main.cpp#404-429)).

##Demo

A demo of the project can be found [here](https://www.youtube.com/watch?v=8phinZrMJz4)
<div align="center">
  <a href="https://www.youtube.com/watch?v=8phinZrMJz4"><img src="https://img.youtube.com/vi/8phinZrMJz4/0.jpg" alt="IMAGE ALT TEXT"></a>
</div>


