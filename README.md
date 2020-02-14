Intel RealSense SDK 2.0 is a cross-platform library for Intel® RealSense™ T265 tracking camera.

// Please Read full documentation to understand how to run librealsense-odometry because for that you fist need to understand what is RealSense and install all packages.
// BY- Shivani, Raghav 
## Installing the packages:
- Register the server's public key:

        sudo apt-key adv --keyserver keys.gnupg.net --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE In case the public key still cannot be retrieved, check and specify proxy settings: export http_proxy="http://<proxy>:<port>"
, and rerun the command. 

- Add the server to the list of repositories:
Ubuntu 16 LTS:

        sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial main" -u
Ubuntu 18 LTS:

        sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo bionic main" -u

- Install the libraries (see section below if upgrading packages):

        sudo apt-get install librealsense2-dkms
        sudo apt-get install librealsense2-utils

- Optionally install the developer and debug packages:

        sudo apt-get install librealsense2-dev
        sudo apt-get install librealsense2-dbg
With dev package installed, you can compile an application with librealsense using g++ -std=c++11 filename.cpp -lrealsense2 or an IDE of your choice.
Reconnect the Intel RealSense depth camera and run: realsense-viewer to verify the installation.

## Upgrading the Packages:
- Refresh the local packages cache by invoking:

        sudo apt-get update

- Upgrade all the installed packages, including librealsense with:

        sudo apt-get upgrade

- To upgrade selected packages only a more granular approach can be applied:

        sudo apt-get --only-upgrade install <package1 package2 ...>
E.g:
        sudo apt-get --only-upgrade install librealsense2-utils librealsense2-dkms

## Uninstalling the Packages:
- Remove a single package with:

        sudo apt-get purge <package-name>

## Prerequisites

### Make Ubuntu Up-to-date:
- Update Ubuntu distribution, including getting the latest stable kernel:

        sudo apt-get update && sudo apt-get upgrade && sudo apt-get dist-upgrade 
- It is recommended to upgrade the distribution with:

        sudo apt-get install --install-recommends linux-generic-lts-xenial xserver-xorg-core-lts-xenial xserver-xorg-lts-xenial xserver-xorg-video-all-lts-xenial xserver-xorg-input-all-lts-xenial libwayland-egl1-mesa-lts-xenial
- Update OS Boot and reboot to enforce the correct kernel selection with

        sudo update-grub && sudo reboot

### Download/Clone librealsense github repository:
Get librealsense sources in one of the following ways:

- Download the complete source tree with git
git clone https://github.com/IntelRealSense/librealsense.git
- Download and unzip the latest stable version from master branch: https://github.com/IntelRealSense/librealsense/archive/master.zip

### Prepare Linux Backend and the Dev. Environment:

1. Navigate to librealsense root directory to run the following scripts.
   Unplug any connected Intel RealSense camera.

2. Install the core packages required to build librealsense binaries and the affected kernel modules:

        sudo apt-get install git libssl-dev libusb-1.0-0-dev pkg-config libgtk-3-dev

Distribution-specific packages:
- Ubuntu 14 or when running of Ubuntu 16.04 live-disk:

         sudo apt-get install
        ./scripts/install_glfw3.sh

- Ubuntu 16:

        sudo apt-get install libglfw3-dev

- Ubuntu 18:

        sudo apt-get install libglfw3-dev libgl1-mesa-dev libglu1-mesa-dev

3. Navigate to librealsense root directory and run mkdir build && cd build

4. Run CMake:

        cmake ../ - The default build is set to produce the core shared object and unit-tests binaries in Debug mode. Use -DCMAKE_BUILD_TYPE=Release to build with optimizations.
        cmake ../ -DBUILD_EXAMPLES=true - Builds librealsense along with the demos and tutorials
        cmake ../ -DBUILD_EXAMPLES=true -DBUILD_GRAPHICAL_EXAMPLES=false - For systems without OpenGL or X11 build only textual examples

5. Recompile and install librealsense binaries:

        sudo make uninstall && make clean && make && sudo make install

-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



## How to run librealsense-odometry:

1. Download the above RealSense SDK

2. Git clone: https://github.com/IntelRealSense/librealsense.git 

3. If you want to save images in rs-save-to-disk code then refer : https://github.com/Shivani1796/librealsense-odometry/tree/master/rs-savetodisk-sequenceimage, copy this code in rs-save-to-disk directory 
// Remember to keep the .cpp file name as it is given in the program otherwise it won't run.

4. If you want to run rs-trajectory and want to use your .bag file and calculate matrix then refer : https://github.com/Shivani1796/librealsense-odometry/tree/master/rs-trajectory-bag2matrix
// Remember to keep the .cpp file name as it is given in the program otherwise it won't run.

5. If you want to run rs-trajectory and also at the same time want to save pose file (matrix - rotation , translation) then refer : https://github.com/Shivani1796/librealsense-odometry/tree/master/rs-trajectory-matrix.
// Remember to keep the .cpp file name as it is given in the program otherwise it won't run.

6. Now, if you want to save your matrix file (rotation , translation) which is also called as pose data, also want to get images in sequence refer : https://github.com/Shivani1796/librealsense-odometry/tree/master/rs-trajectory-savetodisk_merge




 



























