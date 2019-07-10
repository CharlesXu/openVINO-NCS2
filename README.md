# OpenVINO-NCS2
 
### Inspired and based on
* **https://github.com/gplast/OpenVINO-YOLO**
* **https://software.intel.com/en-us/articles/get-started-with-neural-compute-stick**

### Software Requirements
A Linux (Ubuntu 18.04) build environment needs these components:
* [`OpenCV 3.4`](https://docs.opencv.org/3.4.2/d7/d9f/tutorial_linux_install.html) or higher
* [`GNU Compiler Collection (GCC) 3.4`](https://linuxconfig.org/how-to-install-gcc-the-c-compiler-on-ubuntu-18-04-bionic-beaver-linux) or higher
* [`CMake 2.8`](https://cmake.org/install/) or higher
* [`Python 3.6`](https://www.python.org/downloads/) or higher

### Environment construction procedure (Work with LaptopPC)
1.OpenVINO R5 Full-Install. Execute the following commands:
```bash
cd ~
curl -sc /tmp/cookie "https://drive.google.com/uc?export=download&id=1tlDW_kDOchWbkZbfy5WfbsW-b_GpXgr7" > /dev/null
CODE="$(awk '/_warning_/ {print $NF}' /tmp/cookie)"
curl -Lb /tmp/cookie "https://drive.google.com/uc?export=download&confirm=${CODE}&id=1tlDW_kDOchWbkZbfy5WfbsW-b_GpXgr7" -o l_openvino_toolkit_p_2018.5.445.tgz
tar -zxf l_openvino_toolkit_p_2018.5.445.tgz
rm l_openvino_toolkit_p_2018.5.445.tgz
cd l_openvino_toolkit_p_2018.5.445
sudo -E ./install_cv_sdk_dependencies.sh

## GUI version installer
sudo ./install_GUI.sh

```
2.Configure the Model Optimizer. Execute the following commands:
```bash
cd /opt/intel/computer_vision_sdk/install_dependencies
sudo -E ./install_cv_sdk_dependencies.sh
nano ~/.bashrc
source /opt/intel/computer_vision_sdk/bin/setupvars.sh

source ~/.bashrc
cd /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/install_prerequisites
sudo ./install_prerequisites.sh
```
3.【Optional execution】 Additional installation steps for the Intel® Movidius™ Neural Compute Stick v1 and Intel® Neural Compute Stick v2
```bash
sudo usermod -a -G users "$(whoami)"
cat <<EOF > 97-usbboot.rules
SUBSYSTEM=="usb", ATTRS{idProduct}=="2150", ATTRS{idVendor}=="03e7", GROUP="users", MODE="0666", ENV{ID_MM_DEVICE_IGNORE}="1"
SUBSYSTEM=="usb", ATTRS{idProduct}=="2485", ATTRS{idVendor}=="03e7", GROUP="users", MODE="0666", ENV{ID_MM_DEVICE_IGNORE}="1"
SUBSYSTEM=="usb", ATTRS{idProduct}=="f63b", ATTRS{idVendor}=="03e7", GROUP="users", MODE="0666", ENV{ID_MM_DEVICE_IGNORE}="1"
EOF

sudo cp 97-usbboot.rules /etc/udev/rules.d/
sudo udevadm control --reload-rules
sudo udevadm trigger
sudo ldconfig
rm 97-usbboot.rules
```
4.【Optional execution】 Additional installation steps for processor graphics (GPU)
```bash
cd /opt/intel/computer_vision_sdk/install_dependencies/
sudo -E su
uname -r
4.15.0-42-generic #<--- display kernel version sample

### Execute only when the kernel version is older than 4.14
./install_4_14_kernel.sh

./install_NEO_OCL_driver.sh
sudo reboot
```

### 2. Work with RaspberryPi (Raspbian Stretch)
**[Note] Only the execution environment is introduced.**  
  
1.Execute the following command.
```bash
sudo apt update
sudo apt upgrade
curl -sc /tmp/cookie "https://drive.google.com/uc?export=download&id=1rBl_3kU4gsx-x2NG2I5uIhvA3fPqm8uE" > /dev/null
CODE="$(awk '/_warning_/ {print $NF}' /tmp/cookie)"
curl -Lb /tmp/cookie "https://drive.google.com/uc?export=download&confirm=${CODE}&id=1rBl_3kU4gsx-x2NG2I5uIhvA3fPqm8uE" -o l_openvino_toolkit_ie_p_2018.5.445.tgz
tar -zxvf l_openvino_toolkit_ie_p_2018.5.445.tgz
rm l_openvino_toolkit_ie_p_2018.5.445.tgz
sed -i "s|<INSTALLDIR>|$(pwd)/inference_engine_vpu_arm|" inference_engine_vpu_arm/bin/setupvars.sh
```
2.Execute the following command.
```bash
nano ~/.bashrc
### Add 1 row below
source /home/pi/inference_engine_vpu_arm/bin/setupvars.sh

source ~/.bashrc
### Successful if displayed as below
[setupvars.sh] OpenVINO environment initialized

sudo usermod -a -G users "$(whoami)"
sudo reboot
```
3.Update USB rule.
```bash
sh inference_engine_vpu_arm/install_dependencies/install_NCS_udev_rules.sh
### It is displayed as follows
Update udev rules so that the toolkit can communicate with your neural compute stick
[install_NCS_udev_rules.sh] udev rules installed
```
**[Note] OpenCV 4.0.1 will be installed without permission when the work is finished.
If you do not want to affect other environments, please edit environment variables after installation is completed.**
<br>
<br>
<br>
<br>

# Neural Compute Stick 2
**https://ncsforum.movidius.com/discussion/1302/intel-neural-compute-stick-2-information**

# Issue
**[OpenVINO failing on YoloV3's YoloRegion, only one working on FP16, all working on FP32](https://software.intel.com/en-us/forums/computer-vision/topic/804019)**  
**[Regarding YOLO family networks on NCS2. Possibly a work-around](https://software.intel.com/en-us/forums/computer-vision/topic/805425)**  
**[Convert YOLOv3 Model to IR](https://software.intel.com/en-us/forums/computer-vision/topic/805370)**  

# Reference
**https://github.com/opencv/opencv/wiki/Intel%27s-Deep-Learning-Inference-Engine-backend**
**https://github.com/opencv/opencv/wiki/Intel%27s-Deep-Learning-Inference-Engine-backend#raspbian-stretch**
