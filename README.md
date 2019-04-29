# YOLO-darknet-on-Jetson-TX2 and on-Jetson-TX1

Yolo darknet is an amazing algorithm that uses deep learning for real-time object detection but needs a good GPU, many CUDA cores. For **Jetson TX2 and TX1 I would like to recommend to you use this repository if you want to achieve better performance, more fps, and detect more objects [real-time object detection on Jetson TX2](https://github.com/Alro10/realtime_object_detection)**

<p align="center">
<img src="https://github.com/Alro10/YOLO-darknet-on-Jetson-TX2/blob/master/jetsontx2.jpg" alt="alt text" width="80%" height="60%">
</p>


## How to run YOLO on Jetson TX2

After boot (Jetpack 3.1) and install OPENCV...

Copy original Yolo repository:

$ git clone https://github.com/pjreddie/darknet.git

$ cd darknet

$ sudo sed -i 's/GPU=0/GPU=1/g' Makefile

$ sudo sed -i 's/CUDNN=0/CUDNN=1/g' Makefile

$ sudo sed -i 's/OPENCV=0/OPENCV=1/g' Makefile

$ make -j4

You will have to download the pre-trained weight file yolo.weights or tiny-yolo but this is much faster but less accurate than the normal YOLO model.

$ wget https://pjreddie.com/media/files/yolo.weights

$ wget https://pjreddie.com/media/files/tiny-yolo-voc.weights

For TX1 and change the batch size and subdivisions if you run out od memory:

$ sudo nano cfg/yolov3.cfg
 
increase the batch size and reduce the subdivisions:
 
 #batch=64
 batch=32
 #subdvisions=16
 subdivisions=32

### How to run YOLO using onboard camara Jetson TX2? It's a really hard question, I needed to find many sites but I found the right solution:

*overclock
```
$ sudo ./jetson_clocks.sh

$ ./darknet detector demo cfg/coco.data cfg/yolo.cfg yolo.weights "nvcamerasrc ! video/x-raw(memory:NVMM), width=(int)1280, height=(int)720,format=(string)I420, framerate=(fraction)30/1 ! nvvidconv flip-method=0 ! video/x-raw, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink"
```
Or if you wan to run using tiny-yolo only need to change

```
$ ./darknet detector test cfg/voc.data cfg/tiny-yolo-voc.cfg tiny-yolo-voc.weights 

```

Run in videos

```

$ ./darknet detector demo cfg/coco.data cfg/yolo.cfg yolo.weights data/<file-name>

```

Run in image

```

$ ./darknet detect cfg/yolo.cfg yolo.weights data/<file-name>

```

I recommend to take a look...https://pjreddie.com/darknet/yolo/ for more details of YOLO! 

I think it is important to install a SSD and setup to work as the root directory. Also build a kernel and extra modules, you can do the last recommendation after o before build and run YOLO. Jetson only has 32gb.
See this videos:

https://www.youtube.com/watch?v=ZpQgRdg8RmA&t=4s


# YOLOV3 on Jetson TX2 (last update)

<p align="center">
<img src="https://github.com/Alro10/YOLO-darknet-on-Jetson-TX2/blob/master/output1.jpg" alt="alt text" width="90%" height="70%">
</p>


After boot Jetson TX2 with Jetpack 3.2 (CUDA 9 and cuDNN 7) and install openCV (https://github.com/AlexanderRobles21/OpenCVTX2)

## Build darknet:

```

$ git clone https://github.com/pjreddie/darknet.git

$ cd darknet

$ sudo sed -i 's/GPU=0/GPU=1/g' Makefile

$ sudo sed -i 's/CUDNN=0/CUDNN=1/g' Makefile

$ sudo sed -i 's/OPENCV=0/OPENCV=1/g' Makefile

$ make -j4

```

## Download weights 

```

$ wget https://pjreddie.com/media/files/yolov3.weights

$ wget https://pjreddie.com/media/files/yolov3-tiny.weights

```

## Run on JETSON TX2 using onboard cam 

### For yolov3:

```

$ ./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights "nvcamerasrc ! video/x-raw(memory:NVMM), width=(int)1280, height=(int)720,format=(string)I420, framerate=(fraction)30/1 ! nvvidconv flip-method=0 ! video/x-raw, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink"

```

**Performance: 2-4fps**


### For tiny-yolov3:

```

$ ./darknet detector demo cfg/coco.data cfg/yolov3-tiny.cfg yolov3-tiny.weights "nvcamerasrc ! video/x-raw(memory:NVMM), width=(int)1280, height=(int)720,format=(string)I420, framerate=(fraction)30/1 ! nvvidconv flip-method=0 ! video/x-raw, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink"

```

You are able to change the resolution just modify this part: **width=(int)1280, height=(int)720**. 

**Performance: 12fps**

### Using usb webcam:

```

$ ./darknet detector demo cfg/coco.data cfg/yolov3.cfg yolov3.weights /dev/video1

```

*This information was useful for your project? Consider to cite my repository! 

