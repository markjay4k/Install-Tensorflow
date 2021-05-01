# Install Tensorflow 2.4

[Github pages version](https://markjay4k.github.io/Install-Tensorflow/)
We're going for Tensorflow 2.4.1 with GPU support on an Ubuntu 20.04 system.

- GPU: GTX 1070
- OS: Ubuntu 20.04

Sorry, but we won't use the [Tensorflow Docker Image with GPU Support](https://www.tensorflow.org/install/docker).

## Step 1 - Update Nvidia GPU Driver

We will install using the PPA Repository. This will install the latest driver. Run the following command to install the `graphics-driver/ppa` repository to your system

```shell
sudo add-apt-repository ppa:graphics-drivers/ppa
```
Then install the latest driver. Though we call out ```440```, it should install the latest (```450``` in my case)

```shell
sudo apt install nvidia-driver-440
```
you need to reboot

```shell
sudo reboot now
```
check that the driver is Install
```shell
nvidia-smi
```
## Step 2 - Install CUDA Toolkit

Download [CUDA Toolkit 11.0](https://developer.nvidia.com/cuda-11.0-download-archive)
- Linux, x86_64, Ubuntu, 20.04, deb (local)

run the commands provided.
NOTE: The toolkit about 2.4GB

```shell
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget http://developer.download.nvidia.com/compute/cuda/11.0.2/local_installers/cuda-repo-ubuntu2004-11-0-local_11.0.2-450.51.05-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-11-0-local_11.0.2-450.51.05-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu2004-11-0-local/7fa2af80.pub
sudo apt update
sudo apt install cuda
```

## Step 3 - Install CUPTI

It comes with the Toolkit

## Step 4 - Install CUDNN 8.0.4

You will need an Nvidia account so sign up for one. Download cuDNN v8.0.4 Library for Linux (x86_64) from the **cuDNN Archives**.

run the following commands

```shell
tar -xzvf cudnn-11.0-linux-x64-v8.0.4.30.tgs
sudo cp cuda/lib64/cudnn.h /usr/local/cuda/include
sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64
sudo chmod a+r /usr/local/cuda/include/cudnn.h /usr/local/cuda/lib64/libcudnn*
```

Next, we need to update our ```.bashrc``` by adding the path to a few cuda folders to ```LD_LIBRARY_PATH```. Add the following lines to the end of our ```.bashrc``` file. Or to ```.zshrc``` if you're using zsh.

```shell
export LD_LIBRARY_PATH=/usr/local/cuda-11.0/lib64:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH

```

## Step 5 - PIP Install

Make sure you have pip3. If not, install with apt

```shell
sudo apt update
sudo apt install python3-pip
```

We will install Tensorflow to a Virtual Environment with venv. So make sure venv is installed

```shell
sudo apt install python3-venv
```

I like to name my virtual environments `venv` and place them in a directory with a name more relavent to its use. for example

```shell
mkdir tensorflow
cd tensorflow
python3 -m venv venv
```

then activate the environment

```shell
source venv/bin/activate
```

Now install tensorflow with pip.

```shell
pip install tensorflow
```

## Step 6 - Test It!

We should be good to go at this point. Let's activate our environment, start python and import tensorflow.

```shell
source venv/bin/activate
python
```

```python
import tensorflow as tf
tf.config.list_physical_devices('GPU')
```

you should see a bunch of output lines and finally a list of your devices. In my case, I get

```python
[PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
```

that's it! Enjoy!!
