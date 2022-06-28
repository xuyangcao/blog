---
layout: post
---

## 卸载原有驱动

```bash
sudo apt-get remove –purge nvidia*
```

## 禁用nouneau

在 `/etc/modprobe.d/blacklist.conf` 下添加内容：

```bash
sudo vim sudo  /etc/modprobe.d/blacklist.conf
```

在文件结尾添加如下：

```bash
blacklist nouveau
options nouveau modeset=0
```

保存退出，执行下面命令：

```bash
sudo update-initramfs -u
```

重启电脑后输入：

```bash
lsmod | grep nouveau
```

如果没有输出说明禁用成功

## 安装NVIDIA驱动

1. ctrl+alt+F1进入命令行
2. 禁用X服务
	```
	sudo service lightdm stop
	```
3. 安装驱动
	```
	cd ～/Download/ 
	sudo chmod a+x NVIDIA-Linux-x86_64-384.130.run
	sudo ./NVIDIA-Linux-x86_64-375.20.run –no-opengl-files
	```
	
	* –no-opengl-files 只安装驱动文件，不安装OpenGL文件。这个参数很重要
	* –no-x-check 安装驱动时不检查X服务
	* –no-nouveau-check 安装驱动时不检查nouveau 

	后面两个参数可不加。

4. 打开X服务
	```
	sudo service lightdm start
	```
	
5. 成功

	打开terminal，输入`nvidia-smi`发现显示了显卡情况

# 安装CUDA
 

 1.  打开terminal
		```
		cd ~/Download/
		sudo chmod a+x cuda_8.0.61_375.26_linux
		sudo ./cuda_8.0.61_375.26_linux.run --no-opengl-libs
		```
	
 		–no-opengl-files 只安装驱动文件，不安装OpenGL文件。这和驱动安装时的参数不同。

	

		下面是一些推荐的选择参数：
		```
		Do you accept the previously read EULA?
		accept/decline/quit: accept
		 
		Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 361.62?
		(y)es/(n)o/(q)uit: n
		 
		Install the CUDA 8.0 Toolkit?
		(y)es/(n)o/(q)uit: y
		 
		Enter Toolkit Location
		[ default is /usr/local/cuda-8.0 ]:
		 
		Do you want to install a symbolic link at /usr/local/cuda?
		(y)es/(n)o/(q)uit: y
		 
		Install the CUDA 8.0 Samples?
		(y)es/(n)o/(q)uit: y
		 
		Enter CUDA Samples Location
		```

2. 重启电脑 （不重启可能也行）
3. 配置cuda环境变量，可以配置在bashrc中
	```
	sudo vim ~/.bashrc
	```
	在文件最后加上以下内容并保存：
	```
	export PATH=/usr/local/cuda-8.0/bin:$PATH
	export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
	```

	然后运行下面命令使之生效：
	```
		source ~/.bashrc
	```

4. 验证是否安装成功：
	```
	cd /usr/local/cuda-8.0/samples/1_Utilities/deviceQuery
	sudo make
	./deviceQuery
	```

## 安装cudnn

 1. 下载对应版本
 2. 解压下载的文件，可以看到cuda文件夹，在当前目录打开终端，执行如下命令：

	```
		sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
		 
		sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
		 
		sudo chmod a+r /usr/local/cuda/include/cudnn.h
		 
		sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
	```

 3. 成功
