# Teensy-YOLO 
Fast object recognition (15-18 FPS) on Raspberry Pi 3, YOLO (v2) / Darknet with NNPack on a custom dataset, using a three object classes.

Use-Case
--------
YOLO/Darknet may be used as a location tool (left/right/up/down from camera center) on a multi-core SBC (like Raspberry Pi 3), with high FPS and lowered, but sufficient, accuracy.  This specific use was built as part of a Hackaday open-source project(https://hackaday.io/project/26863-visioneer).  A wearable, used by a visually impaired person, to detect the presence and direction of a pedestrian button near a crosswalk.

Hardware (as shown:  )
--------
Raspberry Pi 3 Model B (not overclocking, but fan will still be needed)  
![Raspberry Pi 3](https://www.raspberrypi.org/app/uploads/2017/05/Raspberry-Pi-3-1-1619x1080.jpg{:height="10%" width="10%"})     
Raspberry Pi Camera Module V2 - 8 Megapixel,1080p     
![Raspberry Pi Camera](https://www.raspberrypi.org/app/uploads/2016/04/camera_v2.jpg{:height="10%" width="10%"})     
(Optional) Kuman 3.5 Inch TFT LCD Display Monitor    
40x40mm Fan (required to prevent CPU throttling due to overheating)   
Training Workstation: Ubuntu 16.04 / GTX 1060 (6G) / CUDA 8.0.61.2 / CuDNN 5.1    

Software
--------
Raspbian Stretch - https://www.raspberrypi.org/downloads/raspbian/    
Darknet for NNPack - https://github.com/digitalbrain79/darknet-nnpack    
OpenCV 3.3.1 (2017.10.23) - https://opencv.org/releases.html    
Changes to demo.c - default demo will not run NNPack multi-threading, without these modifications    
Changes to image.c - added simple equation to output whether bbox is left, right or center of screen center    
Changes to detector.c - YOU WILL NEED TO CUSTOMIZE THIS with the path to your names file and number of classes   

Teensy-YOLO  (teensy-yolo.cfg)
-----------
Reduced Input Width/Height = 108x108    
Maintained YOLO Output = 13x13    
(12) Convolutional Layers (3x1), all Batch Normalized and Padded    
(3) Max Pools (2x1)    
(2) Custom anchors based on width/height ratio of bounding box best describing the object shape    
(1) Final Linear Convolutional Layer - Filters = (#classes + #coords(4) + 1)*(NUM)    
[Ex. 3 classes, 2 anchor(NUM) -> (3 + 4 + 1) * 2 = 16  Filters for last layer]    

Training (~15000 iterations needed to achieve average IOU 80%)
--------  
Pre-trained weight file: darknet19_448.conv.32    
learning_rate=0.001    
policy=steps    
steps=10000,20000,30000    
scales=.1,.1,.1    

Dataset (very small, as could be the case for many custom uses)    
-------
(0) stopsign - 270 stop signs (from Guanghan Ning - http://guanghan.info/blog/en/my-works/train-yolo/)    
(1) yieldsign - 284 yield signs (from Guanghan Ning - http://guanghan.info/blog/en/my-works/train-yolo/)    
(2) button - 29 images of a one type of pedestrian button extracted from video    
Bounding Box Tool:  https://github.com/puzzledqs/BBox-Label-Tool    
Converted (python ./convert.py) labels to YOLO format    
Create train.txt and test.txt (python ./process.py)    
Helpful generic tutorial by Nils Tijtgat: https://timebutt.github.io/static/how-to-train-yolov2-to-detect-custom-objects/    

Results
-------
Validtion set (10%) avg accuracy: ~70%    
Low false positives during live street corner scenario with -threshold 0.3 (30%)    
Pi3: Avg 15-18 FPS    
NanoPi NEO Air: ~10 FPS
