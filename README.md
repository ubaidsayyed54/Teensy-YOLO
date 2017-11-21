# YOLO-Pi
Fast object recognition (~15 FPS) on Raspberry Pi 3, using YOLO (v2)/Darknet with NNPack on a custom dataset (a single class) and scaled down version of Tiny-YOLO-VOC v2 (Teensy-YOLO?) with custom anchor.

Use-Case
--------
YOLO/Darknet may be used as a custom location detector for specific objects (invariant) on a multi-core SBC (like Raspberry Pi 3), with high FPS and lowered, but sufficient, accuracy.  This specific use was for a Hackaday wearable (https://hackaday.io/project/26863-visioneer), worn by a blind person to detect the presence and location of a pedestrian button near a crosswalk.

Hardware for Inference (as shown:  )
--------
Raspberry Pi 3 Model B (no overclocking)    
Raspberry Pi Camera Module V2 - 8 Megapixel,1080p     
Kuman 3.5 Inch TFT LCD Display Monitor    
40x40mm Fan (required to prevent CPU throttling due to overheating)    

Software
--------
Raspbian Stretch - https://www.raspberrypi.org/downloads/raspbian/    
Darknet for NNPack - https://github.com/digitalbrain79/darknet-nnpack    
OpenCV 3.3.1 (2017.10.23) - https://opencv.org/releases.html    
Changes to demo.c - default demo will not run NNPack multi-threading    
Changes to image.c - added simple equation to output whether bbox is left, right or center    
Changes to detector.c - custom location of names file and number of classes    

Teensy-YOLO
-----------
Reduced Input Width/Height = 108x108     
(12) Convolutional Layers (3x1), all Batch Normalized and Padded    
(3) Max Pools (2x1)    
(1) Custom anchor based on width/height ratio of bounding box best describing the object shape    
(1) Final Linear Convolutional Layer - Filters = (#classes + #coords(4) + 1)*(NUM)    
[Ex. 1 class, 1 anchor(NUM) -> (1 + 4 + 1) * 1 = 6  Filters for last layer]    

Training
--------
Ubuntu 16.04 and GTX 1060 (6G Memory)    
~15000 iterations    
Pre-trained weights: darknet19_448.conv.32    
learning_rate=0.001    
policy=steps    
steps=10000,20000,30000    
scales=.1,.1,.1    

Dataset
-------
29 images of a one pedestrian button at various distances    
Extracted (ffmpeg) from a 720p video (Samsung Galaxy 8 phone) at eye-height to 640x480 images    
Converted (mogrify) from PNG to JPEGs    
Resized (convert) to 208x208    
Bounding Box Tool:  https://github.com/puzzledqs/BBox-Label-Tool    
Converted (python ./convert.py) labels to YOLO format    
Create train.txt and test.txt (python ./process.py)    
Tutorial: https://timebutt.github.io/static/how-to-train-yolov2-to-detect-custom-objects/    

Results
-------
Validtion set (10%) avg accuracy: ~70%    
Low false positives during live street corner scenario with -threshold 0.3 (30%)    
Pi3: ~15 FPS    
NanoPi NEO Air: ~10 FPS    
