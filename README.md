# FLOAM 
## Fast LOAM (Lidar Odometry And Mapping)

This work is an optimized version of A-LOAM and LOAM with the computational cost reduced by up to 3 times.
This code is modified from [LOAM](https://github.com/laboshinl/loam_velodyne) and [A-LOAM](https://github.com/HKUST-Aerial-Robotics/A-LOAM) .

**Modifier:** [Wang Han](http://wanghan.pro), Nanyang Technological University, Singapore

## 1. Evaluation
### 1.1. Computational efficiency evaluation
Computational efficiency evaluation (based on KITTI dataset):
Platform: Intel® Core™ i7-8700 CPU @ 3.20GHz 
| Dataset                                      | ALOAM                      | FLOAM                  |
|----------------------------------------------|----------------------------|------------------------|
| `KITTI`                                      | 151ms                      | 59ms                   |

Localization error:
| Dataset                                      | ALOAM                      | FLOAM                  |
|----------------------------------------------|----------------------------|------------------------|
| `KITTI sequence 00`                          | 0.55%                      | 0.51%                  |
| `KITTI sequence 02`                          | 3.93%                      | 1.25%                  |
| `KITTI sequence 05`                          | 1.28%                      | 0.93%                  |

### 1.2. localization result
<p align='center'>
<img width="65%" src="/img/kitti_example.gif"/>
</p>

### 1.3. mapping result
<p align='center'>
<a href="https://youtu.be/w_R0JAymOSs">
<img width="65%" src="/img/floam_mapping.gif"/>
</a>
</p>

## 2. Prerequisites
### 2.1 **Ubuntu** and **ROS**
Ubuntu 64-bit 18.04.

ROS Melodic. [ROS Installation](http://wiki.ros.org/ROS/Installation)

### 2.2. **Ceres Solver**
Follow [Ceres Installation](http://ceres-solver.org/installation.html).

### 2.3. **PCL**
Follow [PCL Installation](http://www.pointclouds.org/downloads/linux.html).

### 2.4. **Trajectory visualization**
For visualization purpose, this package uses hector trajectory sever, you may install the package by 
```
sudo apt-get install ros-melodic-hector-trajectory-server
```
Alternatively, you may remove the hector trajectory server node if trajectory visualization is not needed

## 3. Build 
### 3.1 Clone repository:
```
    cd ~/catkin_ws/src
    git clone https://github.com/wh200720041/floam.git
    cd ..
    catkin_make
    source ~/catkin_ws/devel/setup.bash
```
### 3.2 Download test rosbag
Download [KITTI sequence 05](https://drive.google.com/open?id=18ilF7GZDg2tmT6sD5pd1RjqO0XJLn9Mv) or [KITTI sequence 07](https://drive.google.com/open?id=1VpoKm7f4es4ISQ-psp4CV3iylcA4eu0-)

Unzip compressed file 2011_09_30_0018.zip. If your system does not have unzip. please install unzip by 
```
sudo apt-get install unzip 
```

And then copy the file 2011_09_30_0018.bag into ~/catkin_ws/src/floam/dataset/ (this may take a few minutes to unzip the file)
```
	cd ~/catkin_ws/src/floam/dataset/
	unzip ~/Downloads/2011_09_30_0018.zip
```

### 3.3 Launch ROS
```
    roslaunch floam floam.launch
```
if you would like to create the map at the same time, you can run (more cpu cost)
```
    roslaunch floam floam_mapping.launch
```
If the mapping process is slow, you may wish to change the rosbag speed by replacing "--clock -r 0.5" with "--clock -r 0.2" in your launch file, or you can change the map publish frequency manually (default is 10 Hz)


## 4. Test on other sequence
To generate rosbag file of kitti dataset, you may use the tools provided by 
[kitti_to_rosbag](https://github.com/ethz-asl/kitti_to_rosbag) or [kitti2bag](https://github.com/tomas789/kitti2bag) 

## 5. Test on Velodyne VLP-16 or HDL-32
You may wish to test FLOAM on your own platform and sensor such as VLP-16
You can install the velodyne sensor driver by 
```
sudo apt-get install ros-melodic-velodyne-pointcloud
```
launch floam for your own velodyne sensor
```
    roslaunch floam floam_velodyne.launch
```
If you are using HDL-32 or other sensor, please change the scan_line in the launch file 

## 6. Solid state lidar
If you would like to use solid-state lidar such as intel L515, you may want to try floam-ssl
https://github.com/wh200720041/floam_ssl.git

## 7.Acknowledgements
Thanks for [A-LOAM](https://github.com/HKUST-Aerial-Robotics/A-LOAM) and LOAM(J. Zhang and S. Singh. LOAM: Lidar Odometry and Mapping in Real-time) and [LOAM_NOTED](https://github.com/cuitaixiang/LOAM_NOTED).

