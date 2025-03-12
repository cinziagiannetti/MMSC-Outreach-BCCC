Bottle Can or Coffee Cup

This project has been developed by the Materials Made Smarter Centre at Swansea University in collaboration with the Sustain Manufacturing Research Hub and Discover Materials to demonstrate how Computer Vision and Machine Learning can be used to recognise different objects to help with the sorting of materials for recycling.

The platform this project is built on is the Seeed Studio reComputer J1010 NVIDIA Jetson Nano 2GB Platform with the Arm Cortex A57 CPU and NVIDIA Maxwell GPU and it has been developed by Dr R. Gibbs and Prof. C. Giannetti based upon the NVIDIA DLI "Getting Started with AI on Jetson Nano” course.

These instructions document the original steps to set up each reComputer unit making use of the instructions on the Seeed computer support website and the NVIDIA DLI course website. Some of these websites have been altered since the equipment was originally set up in 2022/2023 and so the Wayback machine internet archive is referenced and pdf printouts of the webpages are also provided.

TO SAVE HAVING TO SET UP THE MACHINE FROM SCRATCH FOR AN SD CARD FAILURE THE .IMG IMAGES OF THE TWO SD CARDS ALREADY SETUP WITH ALL OF THIS ARE AVAILBLE AND ARE A MUCH PREFEREABLE WAY OF RESTORING THE SYSTEM

However, if a completely fresh rebuild is necessary the following are the original steps from 2022.

The 4 steps used to set up the system from scratch are detailed below, with the supporting files already downloaded and avaialbe in the 7zip arxives;

01 Flashing emmc.7z
02 Formatting SD card.7z
03 SDK manager installer.7z
04 NVIDIA course.7z
Wayback_machine archive.7z

due to the size of these files they are stored in the attached zenodo repository only, the DOI for the zenodo repository is found in the file
~/zenodo_DOI_for_large_7z_files.txt

# are notes of physical actions
>> 
indicate a set of instructions to be entered at command line
>>


## 01 Flash the OS to the reComputer using command line instructions on the host computer:

https://wiki.seeedstudio.com/reComputer_J1010_J101_Flash_Jetpack/

# jump pins 2 and 3 and connect reComputer with a USB cable
# Download 32_7_3-JP4_6_3 files Jetson-210_Linux_R32.7.3_aarch64.tbz2 and Tegra_Linux_Sample-Root-Filesystem_R32.7.3_aarch64.tbz2

>>
tar xf Jetson-210_Linux_R32.7.3_aarch64.tbz2
cd Linux_for_Tegra/rootfs/
sudo tar xpf ../../Tegra_Linux_Sample-Root-Filesystem_R32.7.3_aarch64.tbz2
cd ..
sudo ./apply_binaries.sh

sudo ./flash.sh jetson-nano-devkit-emmc mmcblk0p1
>>

# power off and unjump the pins
# Setup and update through the GUI



## 02 Transfer OS to SD card and change boot to SD card on the recomputer

https://wiki.seeedstudio.com/Flash_System_on_SD_card/
https://web.archive.org/web/20221002022034/https://wiki.seeedstudio.com/Flash_System_on_SD_card/

>>
git clone https://github.com/Seeed-Studio/seeed-linux-dtoverlays.git
cd seeed-linux-dtoverlays
sed -i '17s#JETSON_COMPATIBLE#\"nvidia,p3449-0000-b00+p3448-0002-b00\"\, \"nvidia\,jetson-nano\"\, \"nvidia\,tegra210\"#' overlays/jetsonnano/jetson-sdmmc-overlay.dts
make overlays/jetsonnano/jetson-sdmmc-overlay.dtbo

sudo cp overlays/jetsonnano/jetson-sdmmc-overlay.dtbo /boot/
cd /boot/
sudo /opt/nvidia/jetson-io/config-by-hardware.py -l

sudo /opt/nvidia/jetson-io/config-by-hardware.py -n "reComputer sdmmc"

git clone https://github.com/limengdu/bootFromUSB
cd bootFromUSB
./copyRootToUSB.sh -p /dev/mmcblk1p1

lsblk
vi /boot/extlinux/extlinux.conf
>>

# press i for insert mode
# change root=/dev/mmcblk0p1 to root=/dev/mmscblk1p1 at top and bottom of document
# press :wq to save and exit

# reboot the computer



## 03 Using an Ubuntu 18 host computer, install NVIDIA SDK manager and install SDK packages
      onto reComputer connected with USB cable (skip reflashing of the reComputer)

https://wiki.seeedstudio.com/reComputer_J1010_J101_Flash_Jetpack/
https://developer.nvidia.com/drive/sdk-manager

# download sdkmanager_1.9.1-10844_amd64.deb onto a host Ubuntu 18 machine (must be an Ubuntu 18 machine)

>>
sudo apt install ./sdkmanager_1.9.1-10844_amd64.deb
sdkmanager
>>



#04a To install original dli course docker

>>
mkdir ~/nvdli-data

echo "sudo docker run --runtime nvidia -it --rm --network host --volume ~/nvdli-data:/nvdli-nano/data --device /dev/video0 nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1" > docker_dli_run.sh  
chmod +x docker_dli_run.sh
./docker_dli_run.sh
>>


#04b To adapt for the MMSC_BCCC project

>>
mkdir ~/mmsc-data

echo "sudo docker run --runtime nvidia -it --rm --network host --volume ~/mmsc-data:/nvdli-nano/data --device /dev/video0 nvcr.io/nvidia/dli/dli-nano-ai:v2.0.2-r32.7.1" > MMSC_BCCC_start.sh  
chmod +x MMSC_BCCC_start.sh
./MMSC_BCCC_start.sh
>>




The documentation supporting the demonstration of this project to ages 11+ is provided on the website

https://discovermaterials.co.uk/resource/bottle-can-or-coffee-cup/
 
Professor C. Giannetti would like to acknowledge the support of the EPSRC (EP/V061798/1) in this Materials Made Smarter Project.
