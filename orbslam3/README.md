```bash
XAUTH=/tmp/.docker.xauth
touch $XAUTH
xauth nlist $DISPLAY | sed -e 's/^..../ffff/' | xauth -f $XAUTH nmerge -

XAUTH=/tmp/.docker.xauth
touch $XAUTH
xauth nlist $DISPLAY | sed -e 's/^..../ffff/' | xauth -f $XAUTH nmerge -

docker run -it
  --privileged \
  --net=host \
  --ipc=host \
  --name="supervins" \
  --gpus=all \
  -e "DISPLAY=$DISPLAY" \
  -e "QT_X11_NO_MITSHM=1" \
  -v "/tmp/.X11-unix:/tmp/.X11-unix:rw" \
  -e "XAUTHORITY=$XAUTH" \
  -e ROS_IP=127.0.0.1 \
  --cap-add=SYS_PTRACE \
  --runtime=nvidia \ 
  -v /etc/group:/etc/group:ro \
  -v `pwd`/SuperVINS:/catkin_ws/src/SuperVINS \
  supervins:latest bash

```
