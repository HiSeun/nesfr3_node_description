# nesfr3_tracking package
This nesfr3_tracking node is not contained in sub packages, but directly in nesfr3_tracking packages.

## 1. nesfr3_tracking
nesfr3_tracking node is for **matching process**. There are several function defined in this ```nesfr3_human_matching.py```. Considering these functions, we can define that the two main job for this node are
```
1. Matching cbox and bbox.
2. Matching hsv histogram and the actor id.
```
### 1.1. Topics
* Subscribes
```
- nesfr3/1/fisheye_camera/image_raw/image_topics (/gazebo)
- nesfr3/1/bounding_boxes (image_converter)
- nesfr3/1/object3d_detector/poses (object3d_detector)
- nesfr3/1/cluster (object3d_detector)
- nesfr3/1/people_tracker/people (bayes_people_tracker)
- nesfr3/1/histogram (bayes_people_tracker)
```

* Publishes
```
- nesfr3/1/pose_with_cluster
```
```nesfr3/1/pose_with_cluster``` contains the ```PoseArrayWithClusters.msg``` file. In the message file, we could find the orientation data of the actors ```poses```, and the pointcloud data ```segments```.
    
### 1.2. Roles
The main 4 functions defined in this node are ```onImage(data_bbox, data_cbox, data_cluster)```, ```scoring(cluster_spec, bbox_spec)```, ``` hist_callback(data_img, data_hist, data_state, data_id)``` and ```state_callback(data_img, data_hist, data_state)```.
Starting from ```onImage()``` function we will discuss about the role of this node.

* onImage(data_bbox, data_cbox, data_cluster)
    - Main role of this function is to comparing the cluster box specification and the boundary box specification to identify whether those two boxes are corresponds to the same person. 
    - Through this process, we can filter out the rest of bboxes except for the human. The method for deciding correspondence comes from the following function ```scoring(cluster_spec, bbox_spec)```. 
    
* scoring(cluster_spec, bbox_spec)
    - If the two boxes(cbox, bbox) show the same cluster(human), then the boxes drawn should have the same dimension in x direction. It is x direction because one image came from the fisheye camera are shown in 2D, while the lidar point cloud data has 3D information. 
    - By comparing the x dimension width between each boxes, we could measure the ```x_score``` value through some simple steps. With the high ```x_score``` value was returned, than it shows the high correspondence. 
    
* hist_callback(data_img, data_hist, data_state, data_id)
    - The above two functions were about the boxes, but now it is about the human_id. ```hist_callback()``` function measures the HSV histogram vaule of each image and then make it save in its own repository list(```hist_list```). 
    - Simply, the size of the list was 10. So if the number of the income HSV histogram's value data is over than 10, the first data in the array would be poped out, so the size of the array is maintained with 10. 
    - Like this, everytime 10 hist value will be used in next function, make sure that whether the human was already detected or not.
    
* state_callback(data_img, data_hist, data_state)
    - It compares the current histogram value data with the previous saved 10 data. 
    - It also use 'scoring' method but this scoring is not the same with the above method. Whether the histogram value data was not in the ```hist_list``` data in the above function, than the nesfr3 identifiest the human with ```new_id```.

* * *
reference : [wom-ai/Point Cloud 3d Clustering](https://github.com/wom-ai/nesfr3_workspace/wiki/Point-Cloud-3d-Clustering, "nesfr3_tracking")
* * *

* * *



## 4. bayes_people_tracker
### 4.1. Package
```bayes_people_tracker``` is contained in the ```bayes_people_tracker``` package. And this package uses the bayes_tracking library developed by Nicola Bellotto (University of Lincoln).

### 4.2. Topics
* Subscribes
```
- nesfr3/1/blobsArray (pcl_transformer_node)
```
From ```pcl_transformer_node```, it receives the ```BlobsArray.msg```, and using ```connectCallback()``` and ```detectCallback()``` function it deals with the tracking ids. (I guess)

* Publishes
```
- nesfr3/1/people_tracker/people
- nesfr3/1/histogram
```
```bayes_people_tracker``` gathers the each people's position and velocity data and passes to the ```nesfr3_tracking``` as topic name ```nesfr3/1/people_tracker/people```.    
Also, it publishes another topic name ```nesfr3/1/histogram``` which contains the data of tracking id(blobsArray indexes).    

### 4.3. Role
This node receives inputs from the user, which ```filter``` they will use and what would be the ```stdlimit``` value, and the constant velocity model noise ```cv_model_noise```. Also you can decide the matching algorithm would be either **NN**(Nearest Neighborhood) or **NNJPDA**(Nearest Neighbor Joint Probabilistic Data Association).    
Depending on the input filter which we would use, this node tracks the pose, and geograhpical position of the people, and then estimates them.   
EKF, UKF, and PF filter can be used.    

- This node is runned by executing ```object3d_detector.launch```. And the **all inputs need for running the ```bayes_people_tracker``` was defined in ```nesfr3/nesfr3_tracking/object3d_detector/config/object3d_detector.yaml``` file**. Therefore if you want to change filter type or the model noise value, simply you can **modify the number or the filter type in the yaml file.**   
   
- Finally you can customize your own filter to track & estimate the human actor position / pose. 
