# Package : nesfr3_services

## 1. shot_controller_node
### 1.1. Topics
**Follow(Followactor::request &req, followactor::response &res)**
It requests parameter and topic messages required.
In more specific way, this function subscribes actor id and wheel odometry information and then publishes linear/angular velocity and camera tilt/pan.

* Parameters
```
- req.actor_id
- req.robot_id
- req.distance
- req.angle- req.shot_size
- req.use_gt
```
* Subscribes
```
- nesfr3/robot_id/Actors`(if `use_gt` =/= 1)
- gazebo/Actor/1`(if `use_gt` == 1)
- nesfr3/1/new_id
- nesfr3/1/wheel_odom
```
* Publishes
```
- nesfr3/1/gimbal/cmd_pos(geometry_msgs::twist)
- nesfr3/1/main_camera/set_hfov(std_msgs::Float64)
- nesfr3/1/state(std_msgs::Int32)
- nesfr3/1/id(std_msgs::Int32)
```

### 1.2. Roles
* how it works

shot_controller_node makes nesfr3 enable to track & film certain actor. It requests id of the actor, id of the robot, desired distance & angle between robot and actor, shot size and use_gt(). If use_gt is 1, nesfr3 follows human based on ground truth position of human.

Important functions defined in this node are followed:

#### ShotController(uint32_t robot_id)
 - advertises `("shot_cmd", &shotcontroller::Follow, this)` to make robot follow the actor.

#### NewIdCallback(std_msgs::Int32::ConstPtr &msg)

This function callbacks id of the robot? actor? by `this->actor_id = uint32_t(msg->data)`

#### ActorCallback(nesfr3_msgs::Actor::ConstPtr &msg)

This function gets message of position and orientation of the certain actor(when tracking occurs on the ground truth position of human). And then publishes `cmd_vel` and `cmd_hfov` which refers to robot's angular/linear velocity and camera tilt/pan.
NESFR3 tracks actor with some desired parameters : distance, angle and velocity(to be done), and this function includes how to track actor in efficient way.
If NESFR3 and robot is far away, this function finds a shortest arc route heads for desired position. And then when distance is short enough, robot maintains its velocity and continuously follows target actor.


#### ActorsCallback
This function activates when `use_gt()` isn't equal to 1. It seems to act similar way compared to ActorCallback, but not analyzed yet enough.

#### OdomCallback
This functinon gets robot's current position and orientation information with odometry message received.

#### GetYaw(double x, y, z, w)
This function gets yaw value of the certain actor, by get quaternion value of the orientation and transfer it into roll, pitch and yaw.

* * *
