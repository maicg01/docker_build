### Build Docker

```bash
docker build -t example_file -f docker/Dockerfile .
```

### Usage

```bash
XAUTH=/tmp/.docker.xauth
touch $XAUTH
xauth nlist $DISPLAY | sed -e 's/^..../ffff/' | xauth -f $XAUTH nmerge -

docker run -it --rm \
  --privileged \
  --network=host \
  --env="DISPLAY=$DISPLAY" \
  --env="QT_X11_NO_MITSHM=1" \
  --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
  --env="XAUTHORITY=$XAUTH" \
  --volume="$XAUTH:$XAUTH" \
  --runtime=nvidia \
  image_docker \
  file_code_run \
```

```bash
docker run -it --privileged --net=host --ipc=host     --name="orbslam3"     --gpus=all     -e "DISPLAY=$DISPLAY"     -e "QT_X11_NO_MITSHM=1"     -v "/tmp/.X11-unix:/tmp/.X11-unix:rw"     -e "XAUTHORITY=$XAUTH"     -e ROS_IP=127.0.0.1     --cap-add=SYS_PTRACE     -v /etc/group:/etc/group:ro     -v `pwd`/SuperVINS:/SuperVINS     orbslam3:latest bash
echo 'ROS_PACKAGE_PATH=/opt/ros/noetic/share:/ORB_SLAM3/Examples/ROS'>>~/.bashrc && source ~/.bashrc
```
Note: -v -> mount duong dan pwd/SuperVins vao folder SuperVins
