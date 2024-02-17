# Install CUDA-11.8 with cuDNN-8.7 for ubuntu(20.04) server A30 GPU
---
* Steps:
	1. verify the system has a cuda-capable gpu

	2. download and install the nvidia cuda toolkit and cudnn.

	3. setup environmental variables

	4. verify the installation
	
* to verify your gpu is cuda enable check
  `>> lspci | grep -i nvidia`
* If you have previous installation remove it first.
	```sh
	>> sudo apt purge nvidia* -y

	>> sudo apt remove nvidia-* -y

	>> sudo rm /etc/apt/sources.list.d/cuda*

	>> sudo apt autoremove -y && sudo apt autoclean -y

	>> sudo rm -rf /usr/local/cuda*
	```
*  install other import packages
  `>> sudo apt install g++ freeglut3-dev build-essential libx11-dev 	libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev`

*  first get the PPA repository driver
	```sh
	>> sudo add-apt-repository ppa:graphics-drivers/ppa

	>> sudo apt update
	```
* install the nvidia driver with dependencies
	```sh
	>> sudo apt install nvidia-utils-525-server nvidia-driver-525-server
	```
*  verify that the nvidia driver installation is successful if error occurs reboot the system and try again this command
	`>> nvidia-smi`
* install CUDA toolkit
	```sh
	>> wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
	
  >> sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
  
  >> wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-ubuntu2004-11-8-local_11.8.0-520.61.05-1_amd64.deb
  
  >> sudo dpkg -i cuda-repo-ubuntu2004-11-8-local_11.8.0-520.61.05-1_amd64.deb
  
  >> sudo cp /var/cuda-repo-ubuntu2004-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
  
  ### Update and upgrade

	>> sudo apt update && sudo apt upgrade -y

	### installing CUDA-11.8

	>> sudo apt install cuda-11-8 -y
	```
*  setup your env paths variables
	```
	>> echo 'export PATH=/usr/local/cuda-11.8/bin:$PATH' >> ~/.bashrc

	>> echo 'export LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc

	>> source ~/.bashrc
	```
*  install cuDNN v11.8
	First register here: https://developer.nvidia.com/developer-program/signup
	```sh
	>> CUDNN_TAR_FILE="cudnn-linux-x86_64-8.7.0.84_cuda11-archive.tar.xz"

	>> sudo wget https://developer.download.nvidia.com/compute/redist/cudnn/v8.7.0/local_installers/11.8/cudnn-linux-x86_64-8.7.0.84_cuda11-archive.tar.xz

	>> sudo tar -xvf ${CUDNN_TAR_FILE}

	>> sudo mv cudnn-linux-x86_64-8.7.0.84_cuda11-archive cuda
	```
* copy the following files into the cuda toolkit directory.
	```sh
	>> sudo cp -P cuda/include/cudnn.h /usr/local/cuda-11.8/include

	>> sudo cp -P cuda/lib/libcudnn* /usr/local/cuda-11.8/lib64/

	>> sudo chmod a+r /usr/local/cuda-11.8/lib64/libcudnn*
	```
* Finally, to verify the installation, check
	```sh
	>> nvidia-smi

	>> nvcc -V
	```
### Install ONNX Runtime (ORT)

	
|ONNX Runtime version  |CUDA  |cuDNN  |[ONNX version](https://github.com/onnx/onnx/blob/master/docs/Versioning.md)  |
|--|--|--|--|
| **1.17** | The default CUDA version for ORT 1.17 is CUDA 11.8. To install CUDA 12 package please look at [Install ORT](https://onnxruntime.ai/docs/install). | cuDNN from 8.8.1 up to 8.9.x |**1.15** |
| **1.15**, **1.16**, **1.17** | CUDA versions from 11.6 up to 11.8 | cuDNN from 8.2.4 up to 8.7.0 | **1.14**, **1.14.1**, **1.15** |


##### [](https://onnxruntime.ai/docs/install/#install-onnx-runtime-cpu)INSTALL ONNX RUNTIME CPU

```
pip install onnxruntime

```

##### [](https://onnxruntime.ai/docs/install/#install-onnx-runtime-gpu-cuda-11x)INSTALL ONNX RUNTIME GPU (CUDA 11.X)

The default CUDA version for ORT is 11.8

```
pip install onnxruntime-gpu

```


##### INSTALL ONNX RUNTIME GPU (CUDA 12.X)

For Cuda 12.x, please use the following instructions to install from  [ORT Azure Devops Feed](https://aiinfra.visualstudio.com/PublicPackages/_artifacts/feed/onnxruntime-cuda-12/PyPI/onnxruntime-gpu/overview)

```
pip install onnxruntime-gpu --extra-index-url https://aiinfra.pkgs.visualstudio.com/PublicPackages/_packaging/onnxruntime-cuda-12/pypi/simple/

```
