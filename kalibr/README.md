# Calib Cam-IMU
B0: build dockerfile kalibr, download mau dataset co san, run container:

```
FOLDER=/path/to/your/data/on/host
xhost +local:root
docker run -it -e "DISPLAY" -e "QT_X11_NO_MITSHM=1" \
    -v "/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    -v "$FOLDER:/data" kalibr
```

B1: tao data_dir theo dung dinh dang cua kalibr

B2: tao bag file

```
rosrun kalibr kalibr_bagcreater --folder ../data/00/. --output-bag calib_data.bag
```

B3: calib camera

```
rosrun kalibr kalibr_calibrate_cameras --target ../data/imu_cam_calib/april_6x6_80x80cm.yaml --bag calib_data.bag --models pinhole-equi pinhole-equi --topics /cam0/image_raw /cam1/image_raw
```

B4: Tim thong so nhieu cua imu de dien vao file yaml (neu co)

B5: calib camera-imu

```
rosrun kalibr kalibr_calibrate_imu_camera --target ../data/imu_cam_calib/april_6x6_80x80cm.yaml --bag ../data/imu_cam_calib/calib_data.bag --cam ../data/imu_cam_calib/calib_data-camchain.yaml --imu ../data/imu_cam_calib/imu_adis16448.yaml --timeoffset-padding 0.1
```

# Note: Calib thong so nhieu cua IMU

https://github.com/gaowenliang/imu_utils

B1: Cai thu vien

```
sudo apt-get install libdw-dev
```

B2: download required [`code_utils`](https://github.com/gaowenliang/code_utils "code_utils"); and put the ROS package `imu_utils` and `code_utils` into your workspace, usually named catkin_ws;

B3: build `code_utils`

```
catkin_make -DCATKIN_WHITELIST_PACKAGES="code_utils"
```

B4: build `imu_utils`

```
catkin_make -DCATKIN_WHITELIST_PACKAGES="imu_utils"
```

B5: Run

```
 rosbag play -r 200 imu_A3.bag
```


```
roslaunch imu_utils A3.launch
```

Be careful of your roslaunch file:

```
<launch>
    <node pkg="imu_utils" type="imu_an" name="imu_an" output="screen">
        <param name="imu_topic" type="string" value= "/djiros/imu"/>
        <param name="imu_name" type="string" value= "A3"/>
        <param name="data_save_path" type="string" value= "$(find imu_utils)/data/"/>
        <param name="max_time_min" type="int" value= "120"/>
        <param name="max_cluster" type="int" value= "100"/>
    </node>
</launch>
```
