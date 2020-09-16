# nesfr3_tracking/object3d_detector package
## 1. object3d_detecter 
## 1.1. Topic
* Subscribes
```
- nesfr3/1/lidar/points
```
lidar/points contains points data from lidar sensor.

* Publishes
```
- nesfr3/1/object3d_detector/poses
- nesfr3/1/cluster
```
```nesfr3/1/object3d_detector/poses``` contains information of points such as position(centroid).
```nesfr3/1/cluster``` contains cluster property such as min, max, centroid.

## 1.2. Role
* **class Object3dDetector** 
```class Object3dDetector``` contains publisher / subscriber, arguments and functions             

* **Object3dDetector::pointCloudCallback**
It calls function extractCluster, and classify.     

* **extractCluster**
It compare distances of pc_indices(from lidar) with range. If distance is in range, then push it to indices_array. (do space divde)       
And set cluster_size min, max value. Also it conducts clustering, set(calculate) width, height, centroid. And limit size(not accurate).

* **classify**
marking (color)
