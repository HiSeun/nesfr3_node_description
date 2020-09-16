# Nesfr3 Overall Summary 
This document was made for arranging our overall concepts about nesfr3. We will discuss about the robot itself's composition, and discuss about each packages and nodes roles. 
* * *

## 1. Nesfr3 model composition
### 1.1. Sensors
* LiDAR (Ouster Lidar)
* Fisheye Camera
* Main Camera
* Encoder

### 1.2. Controllers
* Joystick
* 


## 2. Structural Diagram
Above figure shows the overall structural diagram of the nesfr3_workspace. It contains (*&(*&)^^ packages, and the topic message files are contained in ```nesfr3_msgs``` package.   
(*Other ```sensor_msgs```, ```geometry_msgs``` and etc. are not include in this diagram.)


## 3. Package Description
In this section we will discuss only about the main important packages and nodes. Their main roles are detecting, identifying, and tracking human.   

### 3.1. Cartograhper_ros (pkg)
### 3.1.1. Cartographer_node
### 3.1.2. Cartographer_occupancy_grid_node
Cartographer_node and Cartographer_occupancy_grid_node are the Google open source libraries. Cartographer is a system that provides real-time SLAM in 2D and 3D across multiple platforms and sensor configurations. Anyone can approach to its sources through [cartographer](https://github.com/cartographer-project/cartographer, "ROS_Wiki").  

It's detail explanation is arranged in [cartographer_ros.md](,"cartographer_ros").

### 3.2. nesfr3_human_detection (pkg)
### 3.2.1. image_converter node
image_converter node subscribes orginal img data from fisheye camera and publish inference results creating child thread.    
Inference is done by TensorRT optimized SSD in child thread while main thread drawing detection results and displaying video.   

It's detail is arranged in [nesfr3_human_detection.md](, "image_converter node").

### 3.3. nesfr3_tracking (pkg)
The ```nesfr3_tracking``` node is not contained in sub packages, but directly in nesfr3_tracking packages.
### 3.3.1. nesfr3_tracking
nesfr3_tracking node is for **matching process**. There are several function defined in this ```nesfr3_human_matching.py```. Considering these functions, we can define that the two main job for this node are
```
1. Matching cbox and bbox.
2. Matching hsv histogram and the actor id.
```

It's detail explanation is arranged in this [nesfr3_tracking.md](,"nesfr3_tracking").

### 3.4. bayes_people_tracker_(/nesfr3_tracking) (pkg)
It's detail explanation is arranged in this [people_tracker.md](,"people_tracker").
### 3.5. pcl_transformer_(/nesfr3_tracking) (pkg)
It's detail explanation is arranged in this [pcl_transfer_node.md](,"pcl_transfer_node").
### 3.6. object3d_detector_(/nesfr3_tracking) (pkg)
It's detail explanation is arranged in this [object3d_detector.md](,"object3d_detector").
### 3.7. nesfr3_services (pkg)
It's detail explanation is arranged in this [shotcontroller_node.md](,"shotcontroller_node").


## 4. Operation Pipeline
### 4.1. Detection

### 4.2. Identifying

### 4.3. Tracking
