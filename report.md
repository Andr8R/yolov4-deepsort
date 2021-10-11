# This is short report about car tracking task.

Original repository (https://github.com/theAIGuysCode/yolov4-deepsort.git) allows to use pretrained YOLO object detector together with DeepSORT tracking. In addition to that, optical flow tracking was added. 
All testing videos, together with outputs, can be found on my Google Drive: https://drive.google.com/drive/folders/1pSz4EOAML0Z0eK-JtM0flXgCYV-c0ru2?usp=sharing
After installation and model convertation, you can simply run the code with next command:

```
python object_tracker_new.py --video ./videos/cut3.mp4 --output ./outputs/car_cut3_of_mult_25.avi --optical_flow True
```

Last argument is needed if you wand to see optical flow motion visualisation.

YOLO object detection is the main part for car detection, and only when object bounding boxes from it are available, the object tracking process can be started.

DeepSORT algorithm combines Kalman Filter technics together with metrica that includes Mahalanobis distance and cosine distance (that corresponds to object similarity on frame sequence). Moreover, importance of cosine distance sometimes is much more than Maha. In simple words, we are tracking similarity distance between consecutive frames. Kalman Filter helps to estimate and minimize uncertainties of the process.

On the other side, optical flow allows to visualize and estimate object motion, based on Lucas–Kanade method. Actually, optical flow is a relative motion between objects and observer. For tracking, this method uses Shi-Tomasi corner detection as the key points of corners of objects. So, first of all algorithm detects object, then finds its key points, and after all finds correspondent key points on consecutive frame and calculates optical flow.

To compare, let's notice one of the key drawbacks of DeepSORT algorithm: there should be no ego-motion, what means that camera should be stationary. On the other hand, optical flow is a good way to track *relative* motion. The Lucas-Kanade method assumes that the intensity of a chosen point does not change from one frame to the next and that the point and its neighbors have similar motion. 

Deep learning models are usually the bottleneck in DeepSORT, which makes DeepSORT unscalable for real-time applications. So it happens that tracking includes both method: optical flow is a good approach to fill the gaps of DeepSORT. For optical flow is important that the motion is limited.

Here were used sparse optical flow approach, however, dense approach is very interesting in semantic segmentation tasks: if the objects are placed separately, the distinct motions of the objects may be highly helpful where discontinuity in the dense optical flow field correspond to boundaries between objects.




