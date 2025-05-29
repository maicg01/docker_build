```bash
XAUTH=/tmp/.docker.xauth
touch $XAUTH
xauth nlist $DISPLAY | sed -e 's/^..../ffff/' | xauth -f $XAUTH nmerge -

docker run -it --rm \
  --name supervins_v2 \
  --privileged \
  --network=host \
  --env="DISPLAY=$DISPLAY" \
  --env="QT_X11_NO_MITSHM=1" \
  --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
  --env="XAUTHORITY=$XAUTH" \
  --volume="$XAUTH:$XAUTH" \
  --runtime=nvidia \
  --gpus all \
  -v `pwd`/SuperVINS:/catkin_ws/src/SuperVINS \
  supervins_v2:latest bash
```

- Run  debug ros

```
catkin_make -DCMAKE_BUILD_TYPE=Debug
rosrun --prefix 'gdb -ex run --args' supervins supervins_node src/SuperVINS/config/euroc/stereo_cam_setup.yaml
```
