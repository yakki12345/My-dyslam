# My-dyslam

My-dyslam is a visual SLAM system that is robust in dynamic scenarios for stereo and RGB-D configurations. Having a static map of the scene allows inpainting the frame background that has been occluded by such dynamic objects.

## Getting Started
- Install ORB-SLAM2 prerequisites: C++11 Compiler, Pangolin0.5, OpenCV3.4.8 and Eigen3.3.1 
- Install boost libraries with the command `sudo apt-get install libboost-all-dev`.
- For RGB-D, The following python environment is required：
- Install python 2.7, keras 1.0.8 and tensorflow-gpu 1.14.0(It should be determined based on the CUDA version. For example,CUDA 10.0 & Cudnn 8.5.0), and download the `mask_rcnn_coco.h5` model from this GitHub repository: https://github.com/matterport/Mask_RCNN/releases.
- For stereo, The following python environment is required：
- install python 3.7, opencv-python 3.4.8, pytorch 1.8.1+cu101, torchvision 0.9.1+cu101, can download cityscapes here:https://www.cityscapes-dataset.com/login/.
- Clone this repo:
chmod +x build.sh
./build.sh
- Place the `mask_rcnn_coco.h5` model in the folder `DynaSLAM_LK/src/python/`.
- For dense mapping modules， installing PCL 1.7.0 and vtk 7.1.1.
- Then check the build.sh.

## RGB-D Example on TUM Dataset
- Download a sequence from http://vision.in.tum.de/data/datasets/rgbd-dataset/download and uncompress it.

- Associate RGB images and depth images executing the python script [associate.py](http://vision.in.tum.de/data/datasets/rgbd-dataset/tools):

  ```
  python associate.py PATH_TO_SEQUENCE/rgb.txt PATH_TO_SEQUENCE/depth.txt > associations.txt
  ```
These associations files are given in the folder `./Examples/RGB-D/associations/` for the TUM dynamic sequences.

- Execute the following command. Change `TUMX.yaml` to TUM1.yaml,TUM2.yaml or TUM3.yaml for freiburg1, freiburg2 and freiburg3 sequences respectively. Change `PATH_TO_SEQUENCE_FOLDER` to the uncompressed sequence folder. Change `ASSOCIATIONS_FILE` to the path to the corresponding associations file. `PATH_TO_MASKS` and `PATH_TO_OUTPUT` are optional parameters.

  ```
  ./Examples/RGB-D/rgbd_tum Vocabulary/ORBvoc.txt Examples/RGB-D/TUMX.yaml PATH_TO_SEQUENCE_FOLDER ASSOCIATIONS_FILE (PATH_TO_MASKS) (PATH_TO_OUTPUT)
  ```
  
If `PATH_TO_MASKS` and `PATH_TO_OUTPUT` are **not** provided, only the geometrical approach is used to detect dynamic objects. 

If `PATH_TO_MASKS` is provided, Mask R-CNN is used to segment the potential dynamic content of every frame. These masks are saved in the provided folder `PATH_TO_MASKS`. If this argument is `no_save`, the masks are used but not saved. If it finds the Mask R-CNN computed dynamic masks in `PATH_TO_MASKS`, it uses them but does not compute them again.

If `PATH_TO_OUTPUT` is provided, the inpainted frames are computed and saved in `PATH_TO_OUTPUT`.

## Stereo Example on KITTI Dataset
- Download the dataset (grayscale images) from http://www.cvlibs.net/datasets/kitti/eval_odometry.php 

- Execute the following command. Change `KITTIX.yaml`to KITTI00-02.yaml, KITTI03.yaml or KITTI04-12.yaml for sequence 0 to 2, 3, and 4 to 12 respectively. Change `PATH_TO_DATASET_FOLDER` to the uncompressed dataset folder. Change `SEQUENCE_NUMBER` to 00, 01, 02,.., 11. By providing the last argument `PATH_TO_MASKS`, dynamic objects are detected with Mask R-CNN.
```
./Examples/Stereo/stereo_kitti Vocabulary/ORBvoc.txt Examples/Stereo/KITTIX.yaml PATH_TO_DATASET_FOLDER/dataset/sequences/SEQUENCE_NUMBER (PATH_TO_MASKS)
```

