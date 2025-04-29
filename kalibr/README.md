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
rosrun kalibr kalibr_calibrate_cameras --target ../data/imu_cam_calib/april_6x6_80x80cm.yaml --bag calib_data.bag --models pinhole-equi --topics /cam0/image_raw
```

B4: Tim thong so nhieu cua imu de dien vao file yaml (neu co)

B5: calib camera-imu

```
rosrun kalibr kalibr_calibrate_imu_camera --target ../data/imu_cam_calib/april_6x6_80x80cm.yaml --bag ../data/imu_cam_calib/calib_data.bag --cam ../data/imu_cam_calib/calib_data-camchain.yaml --imu ../data/imu_cam_calib/imu_adis16448.yaml
```
