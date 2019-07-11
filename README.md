# OpenVINO - Intel® Neural Compute Stick 2
 
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

# GUI version installer
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

3.Additional installation steps
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
```

5.【Optional execution】Change Virtual Machines USB Settings (Only for Virtual Box)
```
Add two new USB Filters:
Name: USB2 | Vector ID: 03e7
Name: USB3 | Vector ID: 040e
```

### Configure Neural Compute Stick USB Drive
```bash
source /opt/intel/computer_vision_sdk/bin/setupvars.sh
cd /opt/intel/computer_vision_sdk/install_dependencies/
./install_NCS_udev_rules.sh 
```

### Test the installation
NOW Plug in the Neural Compute Stick to a USB port on your computer
```bash
cd /opt/intel/computer_vision_sdk/deployment_tools/model_optimizer/install_prerequisites/
./install_prerequisites.sh
```

### Run Demos
```bash
#Demo No1
cd /opt/intel/computer_vision_sdk/deployment_tools/demo/
./demo_squeezenet_download_convert_run.sh -d MYRIAD
```
```
#Demo No2 - Traffic Camera (Object Detection)
cd /opt/intel/computer_vision_sdk/deployment_tools/demo/
./demo_security_barrier_camera.sh -d MYRIAD
```

### References
* **https://docs.openvinotoolkit.org/latest/_docs_install_guides_installing_openvino_linux.html**
* **https://software.intel.com/en-us/openvino-toolkit/choose-download?_ga=2.209152978.1128788212.1560861573-1590426351.1560682722**
* **https://github.com/PINTO0309/OpenVINO-YoloV3**

