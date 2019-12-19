Table of Contents
=================

* [Table of Contents](#table-of-contents)
   * [1. Introduction](#1-introduction)
   * [2. Prerequisite](#2-prerequisite)
   * [3. Installation](#3-installation)
         * [3.1 Binary](#31-binary)
         * [3.2 Building from Source](#32-building-from-source)
            * [3.2.1 Building](#321-building)
            * [3.2.2 Installation](#322-installation)
         * [3.3 Docker Container](#33-docker-container)
   * [4. Test Your installation](#4-test-your-installation)

## 1. Introduction

Intel® Edge AI Scholarship from Udaciy is based on The [OpenVINO™](https://software.intel.com/en-us/openvino-toolkit) toolkit quickly deploys applications and solutions that emulate human vision. Based on Convolutional Neural Networks (CNN), the Toolkit extends computer vision (CV) workloads across Intel® hardware, maximizing performance.

<p align="center">
  <img src="https://user-images.githubusercontent.com/16764177/71119146-30023d00-21e2-11ea-9cdb-5787cc86a44f.png">
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/16764177/71123218-dbaf8b00-21ea-11ea-8644-0663f336c386.png">
</p>


> **Note:** I if you have access to the Udacity class workspaces you don't need to install it locally but for further local developing find the installation guid below.



## 2. Prerequisite
- An x86_64 computer running Ubuntu 16.04. Below processors are supported:
	* 6th-8th Generation Intel® Core™
	* Intel® Xeon® v5 family
	* Intel®  Xeon® v6 family

- OpenVINO™ Toolkit Open Source
  	* The [Deep Learning Deployment Toolkit](https://github.com/opencv/dldt) that helps to enable fast, heterogeneous deep learning inferencing for Intel® processors (CPU and GPU/Intel® Processor Graphics), and supports more than 100 public and custom models.
	* [Open Model Zoo](https://github.com/opencv/open_model_zoo) includes 20+ pre-trained deep learning models to expedite development and improve deep learning inference on Intel® processors (CPU, Intel Processor Graphics, FPGA, VPU), along with many samples to easily get started.


- Graphics are required only if you use a GPU. The official system requirements for GPU are:
	* 6th to 8th generation Intel® Core™ processors with Iris® Pro graphics and Intel® HD Graphics
	* 6th to 8th generation Intel® Xeon® processors with Iris Pro graphics and Intel HD Graphics (excluding the e5 product family, which does not have graphics)
	* Intel® Pentium® processors N4200/5, N3350/5, N3450/5 with Intel HD Graphics

- Use one of the following methods to determine the GPU on your hardware:
	* [lspci] command: GPU info may lie in the [VGA compatible controller] line.
	* Ubuntu system: Menu [System Settings] --> [Details] may help you find the graphics information.
	* Openvino: Download the install package, install_GUI.sh inside will check the GPU information before installation.


## 3. Installation 

#### 3.1 Binary

Install Intel® Distribution of OpenVINO™ toolkit for Linux using [official documentation](https://docs.openvinotoolkit.org/latest/_docs_install_guides_installing_openvino_linux.html) also they provide GUI based installation.

![image](https://user-images.githubusercontent.com/16764177/71124989-ba509e00-21ee-11ea-8632-44487f4e91e7.png)

> **Note**: always source your openvino installation directory when you open a new shell (refer to the doc) to update several environment variables before you can compile and run OpenVINO™ applications. 
> 
> Run the following script to temporarily set your environment variables:
> `$ source /opt/intel/openvino/bin/setupvars.sh`

#### 3.2 Building from Source

##### 3.2.1 Building

- Install OpenVINO™ Toolkit Open Source
	* Create directory for all source files
    ```
    mkdir ~/code
    ```
    * Install [OpenCV 3.x: 3.4 or later](https://docs.opencv.org/master/d9/df8/tutorial_root.html)([guide](https://docs.opencv.org/master/d7/d9f/tutorial_linux_install.html))
	```
	[compiler] sudo apt-get install build-essential
	[required] sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
	[optional] sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev
	cd ~/code
	git clone https://github.com/opencv/opencv.git
	git clone https://github.com/opencv/opencv_contrib.git
	cd opencv && git checkout 3.4.2 && cd ..
	cd opencv_contrib && git checkout 3.4.2 && cd ..
	cd opencv
	mkdir build && cd build
	cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=$HOME/code/opencv_contrib/modules/ ..
	make -j8
	sudo make install
	```

	* Install OpenCL Driver for GPU([guide](http://registrationcenter-download.intel.com/akdlm/irc_nas/11396/intel-opencl-4.1-installation.pdf))<br>
	```bash
	cd ~/code
	wget http://registrationcenter-download.intel.com/akdlm/irc_nas/11396/SRB5.0_linux64.zip
	unzip SRB5.0_linux64.zip -d SRB5.0_linux64
	cd SRB5.0_linux64
	sudo apt-get install xz-utils
	mkdir intel-opencl
	tar -C intel-opencl -Jxf intel-opencl-r5.0-63503.x86_64.tar.xz
	tar -C intel-opencl -Jxf intel-opencl-devel-r5.0-63503.x86_64.tar.xz
	tar -C intel-opencl -Jxf intel-opencl-cpu-r5.0-63503.x86_64.tar.xz
	sudo cp -R intel-opencl/* /
	sudo ldconfig
	```
	* Install [Deep Learning Deployment Toolkit](https://github.com/opencv/dldt)([guide](https://github.com/opencv/dldt/tree/2018/inference-engine))<br>
	```bash
	cd ~/code
	git clone https://github.com/opencv/dldt.git
	cd dldt/inference-engine/
	git checkout 2018_R4
	./install_dependencies.sh
	mkdir build && cd build
	cmake -DCMAKE_BUILD_TYPE=Release ..
	make -j8
	sudo mkdir -p /opt/openvino_toolkit
	sudo ln -s ~/code/dldt /opt/openvino_toolkit/dldt
	```
	* Install [Open Model Zoo](https://github.com/opencv/open_model_zoo)([guide](https://github.com/opencv/open_model_zoo/tree/2018/demos))<br>
	```bash
	cd ~/code
	git clone https://github.com/opencv/open_model_zoo.git
	cd open_model_zoo/demos/
	git checkout 2018_R4
	mkdir build && cd build
	cmake -DCMAKE_BUILD_TYPE=Release /opt/openvino_toolkit/dldt/inference-engine
	make -j8
	sudo mkdir -p /opt/openvino_toolkit
	sudo ln -s ~/code/open_model_zoo /opt/openvino_toolkit/open_model_zoo
	```

- Other Dependencies
	```bash
	# numpy and networkx
	pip3 install numpy
	pip3 install networkx
	# libboost
	sudo apt-get install -y --no-install-recommends libboost-all-dev
	cd /usr/lib/x86_64-linux-gnu
	sudo ln -s libboost_python-py35.so libboost_python3.so

##### 3.2.2 Installation

* set ENV InferenceEngine_DIR, CPU_EXTENSION_LIB and GFLAGS_LIB
	```bash
	export InferenceEngine_DIR=/opt/openvino_toolkit/dldt/inference-engine/build/
	export CPU_EXTENSION_LIB=/opt/openvino_toolkit/dldt/inference-engine/bin/intel64/Release/lib/libcpu_extension.so
	export GFLAGS_LIB=/opt/openvino_toolkit/dldt/inference-engine/bin/intel64/Release/lib/libgflags_nothreads.a
	```

#### 3.3 Docker Container

- Follow this [Guide](https://github.com/vuiseng9/openvino-ubuntu) to set up and run OpenVINO in Docker Ubuntu Environment on Intel CPU with Integrated Graphics.

- Also give a look to the [official documentation for Docker from Intel](https://docs.openvinotoolkit.org/latest/_docs_install_guides_installing_openvino_docker_linux.html) 


## 4. Test Your installation 

Go through the official documentation from Intel to [Run the Verification Scripts to Verify Installation](https://docs.openvinotoolkit.org/latest/_docs_install_guides_installing_openvino_linux.html).


![image](https://user-images.githubusercontent.com/16764177/71127248-63999300-21f3-11ea-9bdb-13275828affc.png)
