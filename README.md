# ubuntu_tensorflowGPU_CPU_without_AVX_wheel
Build tensorflow-gpu 1.12.0 wheel (Ubuntu 18.10) which CPU does not support AVX (Intel Pentium GOLD G5400)<br>

    It took me around 6 hours to successfully build this wheel !

some details of my environment:<br>

	Intel® Pentium® Gold G5400 Processor 4M Cache, 3.70 GHz
	GeForce GTX 1060/ 6GB
	Ubuntu 18.10
	tensorflow-gpu 1.12.0
	CUDA 10.0
	cuDNN 7
	The latest version of Anaconda with python 3.7.1

Building Steps:<br>
1. <br>

		sudo apt-get update
		sudo apt-get upgrade

2. GPU supports CUDA<br>

		lspci | grep -i nvidia

3. install dependencies and repository<br>

		sudo apt-get install build-essential
		sudo apt-get install cmake git unzip zip
		sudo add-apt-repository ppa:deadsnakes/ppa
		sudo apt-get update
		python -> I use Anaconda ENV

4. install Nvidia CUDA 10.0<br>

		I use the way of cuda_10.0.130_410.48_linux.run for installation

5. cofigure your building environments<br>

		echo 'export PATH=/usr/local/cuda-10.0/bin${PATH:+:${PATH}}' >> ~/.bashrc
		echo 'export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashrc
		source ~/.bashrc
		sudo ldconfig
		nvidia-smi (if you concern...)

6. install cuDNN 7 with the lazy way<br>

		libcudnn7_7.4.1.5-1+cuda10.0_amd64.deb
		libcudnn7-dev_7.4.1.5-1+cuda10.0_amd64.deb
		libcudnn7-doc_7.4.1.5-1+cuda10.0_amd64.deb
		sudo apt-get install libcupti-dev
		echo 'export LD_LIBRARY_PATH=/usr/local/cuda/extras/CUPTI/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
		(in Anaconda ENV) sudo apt-get install python3-numpy python3-dev python3-pip python3-wheel

7.  official building style of tensorflow<br>

		wget https://github.com/bazelbuild/bazel/releases/download/0.2X.0/bazel-0.2X.0-installer-linux-x86_64.sh
		chmod +x bazel-0.2X.0-installer-linux-x86_64.sh
		./bazel-0.2X.0-installer-linux-x86_64.sh --user
		echo 'export PATH="$PATH:$HOME/bin"' >> ~/.bashrc
		source ~/.bashrc
		git clone https://github.com/tensorflow/tensorflow.git
		cd tensorflow
		./configure
		
8. selections of configurations (follow your heart !)<br>

		You have bazel 0.17.2 installed.
		Please specify the location of python. [Default is /usr/bin/python]: (Anaconda-env-path)
		
		Found possible Python library paths:
		/...
		/...
		Please input the desired Python library path to use.  Default is [/usr/local/lib/python3.5/dist-packages]

		Do you wish to build TensorFlow with Apache Ignite support? [Y/n]: n

		Do you wish to build TensorFlow with XLA JIT support? [Y/n]: n
	
		Do you wish to build TensorFlow with OpenCL SYCL support? [y/N]: n

		Do you wish to build TensorFlow with ROCm support? [y/N]: n

		Do you wish to build TensorFlow with CUDA support? [y/N]: y

		Please specify the CUDA SDK version you want to use. [Leave empty to default to CUDA 9.0]:  10.0

		Please specify the location where CUDA 10.0 toolkit is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 
		
		Please specify the cuDNN version you want to use. [Leave empty to default to cuDNN 7]: 
		
		Please specify the location where cuDNN 7 library is installed. Refer to README.md for more details. [Default is /usr/local/cuda]: 

		Do you wish to build TensorFlow with TensorRT support? [y/N]: n
		
		Please specify the locally installed NCCL version you want to use. [Default is to use https://github.com/nvidia/nccl]: 

		Please note that each additional compute capability significantly increases your build time and binary size. [Default is: 6.1]: 
		
		Do you want to use clang as CUDA compiler? [y/N]: n
			
		Please specify which gcc should be used by nvcc as the host compiler. [Default is /usr/bin/gcc]:  gcc-7
		
		Do you wish to build TensorFlow with MPI support? [y/N]: n
		
		Please specify optimization flags to use during compilation when bazel option "--config=opt" is specified [Default is -march=native]: 
		
		Would you like to interactively configure ./WORKSPACE for Android builds? [y/N]: n
		...
		Configuration finished

9. build pip package (it will take around 2.5~3.5 hours!)<br>

		bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package
		...
		INFO: Build completed successfully, 8036 total actions
		
10. build pip package<br>

		bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package

11. build the invincible WHL file (you should prey for your GOD now)

		bazel-bin/tensorflow/tools/pip_package/build_pip_package tensorflow_pkg

12. find the invincible WHL file and install it ! (you can drink your whisky now)

		...
		...
		CST 2018 : === Output wheel file is in:
		/.../tensorflow/tensorflow_pkg
		
#### pip install tensorflow-1.12.0-cp36-cp36m-linux_x86_64.whl

#### Find your favorite tensorflow scripts for testing this whl~~~

#### ^_^

#### Author<br>
kao.kuntai@GMAIL

#### License<br>
This WHL is totally free for the installation of tensorflow-gpu which has no AVX support of Intel CPU serious.
