# Darknet with NNPACK
NNPACK was used to optimize [Darknet](https://github.com/pjreddie/darknet) without using a GPU. It is useful for embedded devices using ARM CPUs.

## Build from Raspberry Pi
Log in to Raspberry Pi using SSH.<br/>
Install [PeachPy](https://github.com/Maratyszcza/PeachPy) and [confu](https://github.com/Maratyszcza/confu)
```
sudo pip install --upgrade git+https://github.com/Maratyszcza/PeachPy
sudo pip install --upgrade git+https://github.com/Maratyszcza/confu
```
Install [Ninja](https://ninja-build.org/)
```
git clone https://github.com/ninja-build/ninja.git
cd ninja
git checkout release
./configure.py --bootstrap
export NINJA_PATH=$PWD
```
Install clang
```
sudo apt-get install clang
```
Install [NNPACK-darknet](https://github.com/digitalbrain79/NNPACK-darknet.git)
```
git clone https://github.com/digitalbrain79/NNPACK-darknet.git
cd NNPACK-darknet
confu setup
python ./configure.py --backend auto
$NINJA_PATH/ninja
sudo cp -a lib/* /usr/local/lib/
sudo cp include/nnpack.h /usr/local/include/
sudo cp deps/pthreadpool/include/pthreadpool.h /usr/local/include/
```
Build darknet-nnpack
```
git clone https://github.com/digitalbrain79/darknet-nnpack.git
cd darknet-nnpack
make
```

## Test
The weight files can be downloaded from the [YOLO homepage](https://pjreddie.com/darknet/yolo/).
```
YOLOv2-tiny
./darknet detector test cfg/coco.data cfg/yolov2-tiny.cfg yolov2-tiny.weights data/person.jpg
YOLOv3-tiny
./darknet detector test cfg/coco.data cfg/yolov3-tiny.cfg yolov3-tiny.weights data/person.jpg
```
## Result
Device | Model | Build Options | Prediction Time (seconds)
:-:|:-:|:-:|:-:
Raspberry Pi 3 | YOLOv2-tiny | NNPACK=1,ARM_NEON=1 | 1.8
Raspberry Pi 3 | YOLOv2-tiny | NNPACK=0,ARM_NEON=0 | 31
Raspberry Pi 3 | YOLOv3-tiny | NNPACK=1,ARM_NEON=1 | 2.0
Raspberry Pi 3 | YOLOv3-tiny | NNPACK=0,ARM_NEON=0 | 32
Raspberry Pi 4 | YOLOv3-tiny | NNPACK=1,ARM_NEON=1 | 1.4