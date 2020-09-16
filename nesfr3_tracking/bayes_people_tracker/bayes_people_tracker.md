# nesfr3_tracking/bayes_people_tracker package
## 1. bayes_people_tracker
This package uses the bayes_tracking library developed by Nicola Bellotto (University of Lincoln).

### 1.1. Topics
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

### 1.2. Role
This node receives inputs from the user, which ```filter``` they will use and what would be the ```stdlimit``` value, and the constant velocity model noise ```cv_model_noise```. Also you can decide the matching algorithm would be either **NN**(Nearest Neighborhood) or **NNJPDA**(Nearest Neighbor Joint Probabilistic Data Association).    
Depending on the input filter which we would use, this node tracks the pose, and geograhpical position of the people, and then estimates them.   
EKF, UKF, and PF filter can be used.    

- This node is runned by executing ```object3d_detector.launch```. And the **all inputs need for running the ```bayes_people_tracker``` was defined in ```nesfr3/nesfr3_tracking/object3d_detector/config/object3d_detector.yaml``` file**. Therefore if you want to change filter type or the model noise value, simply you can **modify the number or the filter type in the yaml file.**   
   
- Finally you can customize your own filter to track & estimate the human actor position / pose. 
