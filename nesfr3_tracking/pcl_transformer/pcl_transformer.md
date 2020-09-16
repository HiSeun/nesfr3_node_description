# nesfr3_tracking/pcl_transformer package

## 1. pcl_transformer_node
### 1.1. Topics
* Subscribes
```
- nesfr3/1/fisheye_camera/image_raw/image_topics (/gazebo)
- nesfr3/1/pose_with_cluster (/nesfr3_tracking)
```

* Publishes
```
- nesfr3/1/blobsarray`(/nesfr3/1/bayes_people_tracker)
```
### 1.2. Roles
* how it works
This node gets image from fisheye_camera and pose_with_cluster message from nesfr3_tracking.    
This message contains orientation data of each actors detected, and point cloud cluster information, and then this node projects point cloud information into the camera image, by coordinate transition from 3-dimensional cartesian coordinate to polar coordinate and 2-dimensional cartesian coordinate on the image.

**Important functions defined are followed:**

* **dataCallback(const nesfr3_msgs::PoseArrayWithClusters::ConstPtr &clusters_msg, const sensor_msgs::ImageConstPtr &img_msg, ros::Publisher blobs_array_pub_, bool verbose)   **
- This function callbacks required messages which are ```clusters_msg```(point cloud clsuter contains actors' orientation) and ```img_msg```(fisheye camera image) and publishes blobsArray message. This message contains poses, tracking_id of each cluster converted into image blob.

* **cart2img**
- 'Image blob' described on the above line is shown on this function. This function calls `xyz2rt2image()` function to convert point cloud cluster into image blob, means rectangular zone has identical center with point cloud. Then it publishes `blobsArray` which is array that contains each blob's center coordinate, width and height.

* **xyz2rt2image**
- This function calculates coordinate of center point of transformed image blob. Mathematical calculation is included, further analysis is needed for better understanding.
