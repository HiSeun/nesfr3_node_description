# nesfr3_human_detection package.
## 1. image_converter
```image_converter``` node subscribes orginal img data from fisheye camera and publish inference results creating child thread. 
Inference is done by TensorRT optimized SSD in child thread while main thread drawing detection results and displaying video.

### 1.1. Topics
* Subscribes
```
- nesfr3/1/fisheye_camera/image_raw/image_topics
```
```fisheye_camera/image_raw/image_topics``` contains image data obtained by fisheye camera.

* Publishes
```
- nesfr3/1/bounding_boxes
```
### 1.2. Roles
* **class TrtThread**
- It implements the child thread (read images from cam to TRT inferencing). The child thread stores the input image and detectino results (global & condition variable) 
    
* **run()** 
```run()``` runs until running flag is set to False in main thread. It detects human by function trt_ssd.detect(img, self.conf_th). Please refer ```detect.py``` .
      
* **class image_converter** 
This class exists for parsing, and setting robot args. It publishes and subscribes image.

* **callback(self,data)** 
Converting ROS image messages to OpenCV images, put cv_image to img_raw_queue, put bounidng boxes value

* **bbox(box1, box2)** 
```bbox(box1, box2)``` returns the iou of tow bbox.   
get the coordinates of bbox (if x1min>x1max x1min -w, else do nothing), get the coordinates of intersec rec (if x1min>x1max and x2min<x2max calculate iou1 as original(b1_x1 ..) and io2 as previous original box value(b1_x1+w) and set iou as max   
    
* **calculate_iou(coordinates..)** 
```calculate_iou(coordinates..)``` finds intersection and calculate iou by ratio of area        
      
* **duplicate_box_removal(bbox)** 
```duplicate_box_removal(bbox)``` removes bbox for same object
