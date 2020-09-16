# Nesfr3 Overall Summary 
This document was made for arranging our overall concepts about nesfr3. We will discuss about the robot itself's composition, and discuss about each packages and nodes roles. 
* * *

## 1. Nesfr3 model composition
### 1.1. Sensors
* **LiDAR (Ouster Lidar)**   
This ouster lidar is used for gathering point cloud information for clustering them and compare with human detection output in fisheye camera.     
On the gazebo simulation environment, lidar message is received as `/nesfr3/1/lidar/points`.    

* **Fisheye Camera**    
This camera produces 2d images of 360 degree, which has resolution of 4400*1100.            
On the gazebo simulation environment, fisheye camera message is received as `/nesfr3/1/fisheye_camera/image_raw/image_topics`.      
    - (please add model name of the fisheye camera!)
 
* **Main Camera**    
This is main camera(BFS-U3-70S7C), which has resolution of 7.10 megapixels(3,208*2,200) and 51 fps.     
Please refer [FLIR Blackfly® BFS-U3-70S7C-C USB 3.1](https://www.edmundoptics.co.kr/p/BFS-U3-70S7C-C-USB3-BlackflyR-S-Color-Camera/41900) for further specifications.

* **Encoder**    
Encoder on the wheel axis gives wheel odometry information for estimation of current NESFR3 position, via topic `nesfr3/1/wheel_odom` from `/gazebo` to the `/nesfr3/cartographer_node`.     
**On the gazebo simulation environment, ouster lidar is located 80cm from the ground while fisheye camera is located 25cm from the ground.**
 
 
### 1.2. Controllers    
* Joystick controller(not equipped yet)      
With `joy_node`, which is ROS driver for generic Linux joystick, this controller publishes `/joy` message to the NESFR3 for teleoperation.      
With this message, `nesfr_teleop_node` receives lidar point message and wheel odometry message so publish linear/angular velocity and camera tilt/pan to track the target actor.       

### 1.3. Computer
* Nvidia Xavier
Nvidia Xavier is computer expected to be equipped on the NESFR3, which enables gpu-based computation within the robot.     
Please refer [Jetson AGX Xavier Developer Kit](https://developer.nvidia.com/embedded/jetson-agx-xavier-developer-kit) for further specifications.

## 2. Structural Diagram
Above figure shows the overall structural diagram of the nesfr3_workspace. It contains (*&(*&)^^ packages, and the topic message files are contained in ```nesfr3_msgs``` package.   
(*Other ```sensor_msgs```, ```geometry_msgs``` and etc. are not include in this diagram.)


## 3. Package Description
In this section we will discuss only about the main important packages and nodes. Their main roles are detecting, identifying, and tracking human.   

### 3.1. Cartograhper_ros (pkg)
* **Cartographer_node**
* **Cartographer_occupancy_grid_node**
Cartographer_node and Cartographer_occupancy_grid_node are the Google open source libraries. Cartographer is a system that provides real-time SLAM in 2D and 3D across multiple platforms and sensor configurations. Anyone can approach to its sources through [cartographer](https://github.com/cartographer-project/cartographer, "ROS_Wiki").  

It's detail explanation is arranged in [cartographer_ros.md](https://github.com/HiSeun/nesfr3_pkg_description/blob/master/cartographer_ros/cartograhper_ros.md,"cartographer_ros").

### 3.2. nesfr3_human_detection (pkg)
* **image_converter**
```image_converter```node subscribes orginal img data from fisheye camera and publish inference results creating child thread.    
Inference is done by TensorRT optimized SSD in child thread while main thread drawing detection results and displaying video.   

It's detail is arranged in [nesfr3_human_detection.md](https://github.com/HiSeun/nesfr3_pkg_description/blob/master/nesfr3_human_detection/nesfr3_human_detection.md, "image_converter node").

### 3.3. nesfr3_tracking (pkg)
The ```nesfr3_tracking``` node is not contained in sub packages, but directly in nesfr3_tracking packages.
* **nesfr3_tracking**
nesfr3_tracking node is for **matching process**. There are several function defined in this ```nesfr3_human_matching.py```. Considering these functions, we can define that the two main job for this node are
```
1. Matching cbox and bbox.
2. Matching hsv histogram and the actor id.
```

It's detail explanation is arranged in [nesfr3_tracking.md](https://github.com/HiSeun/nesfr3_pkg_description/blob/master/nesfr3_tracking/nesfr3_tracking.md,"nesfr3_tracking").

### 3.4. bayes_people_tracker_(/nesfr3_tracking) (pkg)
* **people_tracker**
This node receives inputs from the user, which ```filter``` they will use and what would be the ```stdlimit``` value, and the constant velocity model noise ```cv_model_noise```. Also you can decide the matching algorithm would be either **NN**(Nearest Neighborhood) or **NNJPDA**(Nearest Neighbor Joint Probabilistic Data Association).    
Depending on the input filter which we would use, this node tracks the pose, and geograhpical position of the people, and then estimates them.   
EKF, UKF, and PF filter can be used.       
   
It's detail explanation is arranged in [people_tracker.md](https://github.com/HiSeun/nesfr3_pkg_description/blob/master/nesfr3_tracking/bayes_people_tracker/bayes_people_tracker.md,"people_tracker").   

### 3.5. pcl_transformer_(/nesfr3_tracking) (pkg)
* **pcl_transfer_node** 
This node gets image from fisheye_camera and pose_with_cluster message from nesfr3_tracking.       
This message contains orientation data of each actors detected, and point cloud cluster information, and then this node projects point cloud information into the camera image, by coordinate transition from 3-dimensional cartesian coordinate to polar coordinate and 2-dimensional cartesian coordinate on the image.      
      
It's detail explanation is arranged in [pcl_transfer_node.md](https://github.com/HiSeun/nesfr3_pkg_description/blob/master/nesfr3_tracking/pcl_transformer/pcl_transformer.md,"pcl_transfer_node").       
   
### 3.6. object3d_detector_(/nesfr3_tracking) (pkg)
* **object3d_detector**
object3d_detecter conducts clustering points from lidar. Group of points turns into cuboid by clustering.         
It also calculate centroid, and min & max value of x,y, z postion and publish it.
It's detail explanation is arranged in [object3d_detector.md](,"object3d_detector").   
   
### 3.7. nesfr3_services (pkg)
* **shot_controller_node**
```shot_controller_node``` makes nesfr3 enable to track & film certain actor. It requests id of the actor, id of the robot, desired distance & angle between robot and actor, shot size and use_gt(). If use_gt is 1, nesfr3 follows human based on ground truth position of human.    

It's detail explanation is arranged in [shot_controller_node.md](https://github.com/HiSeun/nesfr3_pkg_description/blob/master/nesfr3_services/nesfr3_services.md,"shotcontroller_node").


## 4. Operation Process
### 4.1. Detection
1.
2.
3.
### 4.2. Matching
1. Subscribe cbox data(size, dimension, index) and bbox data(size, dimension, index). 
2. Compare the width (x dimension) between each boxes and filter out the corresponding human bbox. 
3. Calcuate the blob array's hsv histogram value and check whether the ```hist_list``` is full or not.         
   Then, append or delete the ```hist``` so that the array size does not exceed the prescribed value(10). 
4. Check the blob image's hsv histogram value and get the correspondence score.  
   If the value is not contained in the ```hist_list```, than add new ```tracking_id```. 
5. 

### 4.3. Tracking
1. User ```nesfr3/nesfr3_tracking/object3d_detector/config/object3d_detector.yaml``` modifies the matching algorithm, observation model, noise_params, and etc. to proper values.  
2. According to those parameters, track the pose, and geograhpical position of the people, and then estimate them.   
   EKF, UKF, and PF filter can be used.
3. 
