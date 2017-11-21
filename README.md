# YOLO-Pi
Fast object recognition (~15 FPS) on Raspberry Pi 3, using YOLO (v2)/Darknet with NNPack on a custom dataset (a single class) and scaled down version of Tiny-YOLO-VOC v2 (Teensy-YOLO?) with custom anchor.

|Teensy-YOLO (Scaled down Tiny-YOLO-VOC v2)
|Reduced Input Width/Height = 108x108
|(12) Convolutional Layers (3x1), all Batch Normalized and Padded
|(3) Max Pools (2x1)
|(1) Custom anchor based on width/height ratio of bounding box best describing the object shape (Ex. Pedestrian Button/Pole)
|(1) Final Linear Convolutional Layer - Filters = (#classes + #coords(4) + 1)*(NUM) 
|[Ex. 1 class, 1 anchor(NUM) -> (1 + 4 + 1) * 1 = 6  Filters for last layer]

|Training (~15000 iterations on GTX 1060 (6G)
|Pre-trained weights used: darknet19_448.conv.32
|learning_rate=0.001
|policy=steps
|steps=10000,20000,30000
|scales=.1,.1,.1

Dataset: 29 images of a particular pedestrian button at various distances extracted from a 720p video at eye-height, then converted to 208x208 JPEGs.

|Validtion set (10%) avg accuracy: ~70% 
|Low false positives on street corner with -threshold 0.3 (30%)
|Avg FPS on Pi3 (not-overclocked): 15
