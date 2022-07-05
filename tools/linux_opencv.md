---
layout: post
---

## Installation of OpenCV using the Ubuntu repository

Please refer to [1].

## Install OpenCV through the source

### Step 1: Install build tools

```bash
sudo apt install build-essential cmake git pkg-config libgtk-3-dev \
libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
libxvidcore-dev libx264-dev libjpeg-dev libpng-dev libtiff-dev \
gfortran openexr libatlas-base-dev \
libtbb2 libtbb-dev libdc1394-22-dev libopenexr-dev \
libgstreamer-plugins-base1.0-dev libgstreamer1.0-dev
```

### Step 2: Clone OpenCV’s repositories

```bash
mkdir ~/opencv_build && cd ~/opencv_build
git clone https://github.com/opencv/opencv.git
# optional
git clone https://github.com/opencv/opencv_contrib.git
```

### Step 3: Setup OpenCV build

```bash
cd ~/opencv_build/opencv
mkdir -p build && cd build
```

```bash
# remove some options according to your personal favor
cmake -D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local \
-D INSTALL_C_EXAMPLES=ON \
-D INSTALL_PYTHON_EXAMPLES=ON \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D OPENCV_EXTRA_MODULES_PATH=~/opencv_build/opencv_contrib/modules \
-D BUILD_EXAMPLES=ON ..
```

### Step 4: Start a compilation

```bash
make -j 8
```

### Step 5:  Install OpenCV

```bash
# install
sudo make install
```

```bash
# verify if success
pkg-config --modversion opencv4
```

## Reference

- [https://vitux.com/opencv_ubuntu/](https://vitux.com/opencv_ubuntu/)

- [Ubuntu下OpenCV的安装及实例](https://blog.csdn.net/weixin_45654230/article/details/120734932)

- [Using OpenCV with gcc and CMake](https://docs.opencv.org/2.4/doc/tutorials/introduction/linux_gcc_cmake/linux_gcc_cmake.html#linux-gcc-usage)

- [Installation in Linux](https://docs.opencv.org/2.4/doc/tutorials/introduction/linux_install/linux_install.html#linux-installation)
