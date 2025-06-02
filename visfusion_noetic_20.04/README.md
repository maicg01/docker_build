- fix bux khong chay duoc rviz do key ros bi cu qua

  ```
  sudo rm /etc/apt/sources.list.d/ros-latest.list
  sudo rm /etc/apt/sources.list.d/ros1-latest.list 2>/dev/null
  
  sudo apt install curl gnupg2 -y
  curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc \
      | sudo gpg --dearmor -o /usr/share/keyrings/ros-archive-keyring.gpg
  
  
  echo "deb [signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros/ubuntu focal main" \
      | sudo tee /etc/apt/sources.list.d/ros-latest.list > /dev/null
  ```

- Cai lai rviz

  ```
  sudo apt install ros-noetic-desktop-full
  ```
