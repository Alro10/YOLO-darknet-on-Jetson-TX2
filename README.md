# YOLO-darknet-on-Jetson-TX2
How to run YOLO on Jetson TX2

After boot (Jetpack 3.1) and install OPENCV...

Copy original Yolo repository:

$ git clone https://github.com/pjreddie/darknet.git

$ cd darknet

$ sudo sed 's/GPU=0/GPU=1/g' Makefile

$ sudo sed 's/CUDNN=0/CUDNN=1/g' Makefile

$ sudo sed 's/OPENCV=0/OPENCV=1/g' Makefile

$ make -j4

You will have to download the pre-trained weight file yolo.weights or tiny-yolo but this is much faster but less accurate than the normal YOLO model.

$ wget https://pjreddie.com/media/files/yolo.weights

$ wget https://pjreddie.com/media/files/tiny-yolo-voc.weights

How to run YOLO using onboard camara Jetson TX2? It's a really hard question, I needed to find many sites but I found the true solution:

$ ./darknet detector demo cfg/coco.data cfg/yolo.cfg yolo.weights "nvcamerasrc ! video/x-raw(memory:NVMM), width=(int)1280, height=(int)720,format=(string)I420, framerate=(fraction)30/1 ! nvvidconv flip-method=0 ! video/x-raw, format=(string)BGRx ! videoconvert ! video/x-raw, format=(string)BGR ! appsink"

Or if you wan to run using tiny-yolo only need to change

$ ./darknet detector test cfg/voc.data cfg/tiny-yolo-voc.cfg tiny-yolo-voc.weights 

Run in videos

$ ./darknet detector demo cfg/coco.data cfg/yolo.cfg yolo.weights data/<file-name>

Run in photos or image

$ ./darknet detect cfg/yolo.cfg yolo.weights data/<file-name>

I recommend to take a look...https://pjreddie.com/darknet/yolo/ for more detalis of YOLO! 

I think it is important to install a SSD and setup to work as the root directory. Also build a kernel and extra modules, you can do the last recommendation after o before build and run YOLO. Jetson only has 32gb.
See this videos:

https://www.youtube.com/watch?v=ZpQgRdg8RmA&t=4s

This information was useful for your project? Consider to cite my repository! 

