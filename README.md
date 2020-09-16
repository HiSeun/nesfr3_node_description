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
cartographer ros packages are containing 

### 3.2. nesfr3_tracking (pkg)

### 3.3. bayes_people_tracker_(/nesfr3_tracking) (pkg)

### 3.4. pcl_transformer_(/nesfr3_tracking) (pkg)

### 3.5. object3d_detector_(/nesfr3_tracking) (pkg)

### 3.6. nesfr3_services (pkg)


## 4. Operation Pipeline
### 4.1. Detection

### 4.2. Identifying

### 4.3. Tracking
