# Sử dụng Ubuntu 22.04 làm base image
FROM ubuntu:22.04

# Thiết lập các biến môi trường
ENV DEBIAN_FRONTEND=noninteractive

# Cập nhật hệ thống và cài đặt các công cụ cơ bản
RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    wget \
    unzip \
    libgtk2.0-dev \
    pkg-config \
    libflann-dev \
    libavcodec-dev \
    libavformat-dev \
    libswscale-dev \
    libtbb2 \
    libtbb-dev \
    libjpeg-dev \
    libpng-dev \
    libtiff-dev \
    libopenexr-dev \
    libeigen3-dev \
    libsuitesparse-dev \
    libboost-all-dev \
    libgl1-mesa-dev \
    libglew-dev \
    libglfw3-dev \
    libglm-dev \
    libpython3-dev \
    python3-pip \
    libblas-dev \
    liblapack-dev \
    libopenblas-dev \
    libopenmpi-dev \
    libhdf5-dev \
    && rm -rf /var/lib/apt/lists/*

# Cài đặt Eigen (3.4.0)
WORKDIR /tmp
RUN wget https://gitlab.com/libeigen/eigen/-/archive/3.4.0/eigen-3.4.0.tar.gz && \
    tar -xvzf eigen-3.4.0.tar.gz && \
    cd eigen-3.4.0 && \
    mkdir build && cd build && \
    cmake .. && make install && \
    cd /tmp && rm -rf eigen-3.4.0*

# Cài đặt Sophus
WORKDIR /tmp
RUN wget https://github.com/strasdat/Sophus/archive/refs/tags/1.22.10.tar.gz && \
    tar -xvzf 1.22.10.tar.gz && \
    cd Sophus-1.22.10 && \
    mkdir build && cd build && \
    cmake .. && make install && \
    cd /tmp && rm -rf Sophus-1.22.10*

# Cài đặt SuiteSparse (7.7.0)
WORKDIR /tmp
RUN wget https://github.com/DrTimothyAldenDavis/SuiteSparse/archive/refs/tags/v7.7.0.tar.gz && \
    tar -xvzf v7.7.0.tar.gz && \
    cd SuiteSparse-7.7.0 && \
    cd build && \
    cmake .. -DSUITESPARSE_ENABLE_PROJECTS="cholmod;cxsparse" && \
    make -j$(nproc) && make install INSTALL=/usr/local && \
    cd /tmp && rm -rf SuiteSparse-7.7.0*

# Cài đặt G2O (0.1)
WORKDIR /tmp
RUN git clone https://github.com/RainerKuemmerle/g2o.git && \
    cd g2o && \
    git checkout 9b41a4ea5ade8e1250b9c1b279f3a9c098811b5a && \
    mkdir build && cd build && \
    cmake .. && make -j$(nproc) && make install && \
    cd /tmp && rm -rf g2o

# Cài đặt PCL (1.12)
WORKDIR /tmp
RUN git clone https://github.com/PointCloudLibrary/pcl.git && \
    cd pcl && \
    git checkout pcl-1.12.1 && \
    mkdir build && cd build && \
    cmake .. && make -j$(nproc) && make install && \
    cd /tmp && rm -rf pcl

# Cài đặt OpenCV (4.5.4)
WORKDIR /tmp
RUN wget -O opencv.zip https://github.com/opencv/opencv/archive/4.5.4.zip && \
    wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/4.5.4.zip && \
    unzip opencv.zip && \
    unzip opencv_contrib.zip && \
    cd opencv-4.5.4 && \
    mkdir build && cd build && \
    cmake -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.5.4/modules .. && \
    make -j$(nproc) && make install && \
    cd /tmp && rm -rf opencv-4.5.4* opencv_contrib-4.5.4*

# Cài đặt Rerun Viewer và Rerun (0.16.1)
WORKDIR /tmp
RUN pip install rerun-sdk==0.16.1

# Thiết lập thư mục làm việc
WORKDIR /workspace

# copy code vao docker container
COPY . /workspace

# Thiết lập biến môi trường cho C++ 17
ENV CXXFLAGS="-std=c++17"

# Lệnh mặc định khi container được khởi động
CMD ["bash"]
