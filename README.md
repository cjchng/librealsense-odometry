Intel RealSense SDK 2.0 is a cross-platform library for Intel® RealSense™ T265 tracking camera.

**Please read full documentation to understand how to run librealsense-odometry because for that you first need to understand what is RealSense and install all packages.**

**BY- Shivani, Raghav**

## Installing the packages:
- Register the server's public key:

        sudo apt-key adv --keyserver keys.gnupg.net --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE || sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-key F6E65AC044F831AC80A06380C8B3A55A6F3EFCDE In case the public key still cannot be retrieved, check and specify proxy settings: export http_proxy="http://<proxy>:<port>"
, and re-run the command. 

- Add the server to the list of repositories:
1. Ubuntu 16 LTS:

        sudo add-apt-repository "deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial main" -u
2. Ubuntu 18 LTS:

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

--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## Explaination between original and modified code.

1. rs-trajectory (Original) 

The code rs-trajectory is the code developed by Intel and available on Intel website on github "https://github.com/IntelRealSense/librealsense/tree/master/examples/trajectory". Now, what this code do so , **this is a sample code which demonstrates how to draw the trajectory of device movement based on pose data.**

This should open a window split into 4 viewports:top (upper left viewport), front (lower left viewport), side (lower right viewport) and 3D (upper right viewport).In each viewport, a 3D model of the camera and the corresponding 2D trajectory are rendered. In the 3D view, you should be able to interact with the camera using your mouse, for rotating, zooming and panning.


2. rs-savetodisk (Original)

The code rs-save-to-disk is the code developed by Intel and available on Intel website on github "https://github.com/IntelRealSense/librealsense/tree/master/examples/save-to-disk". Now, what this code can do!! 
This sample demonstrates how to configure the camera for streaming in a textual environment and save depth and color data to PNG format. In addition, it touches on the subject of per-frame metadata.When we run this sample code it opens for an second an click two pictures from camera and than shut down. 


### Our Modification-
We modified the above code in suh a way that we get the dataset for odometry. Now, modified the rs-trajectory and rs-save-to-disk as:
	1) bag-to-matrix by rs-trajectory 
	2) save-to-disk sequence images
	

1. bag to matrix rs-trajectory Modified: 

Here we use .bag file as the device and modified the original code in such a way that it will store all the pose (6DOF) data of the trajectory into a text file. We arranged that matrix in a way of 3x4 matrix  which is written by the horizontal concatenation of the matrix R and matrix t (rotational and the translation vector). This generated text file is the required ground truth values necesary in odometry. 

*In this code you not need the camera to run the program and store the matrix, you just need to create the .bag file with the camera and use the path of .bag file to run the program and it with show the trajectory according to that bag file and create no. of matrix for all the frames in video file.

2. save-to-disk sequence images:

As explained above what the original sample code rs-save-to-disk do we modified it in a way that before it only start for a second and gives output only single frame but now in the *modified version* the program not stops and runs continously till user wants and gives the output frames continously, user need to terminate the program to stop.

## Final Code to get the dataset with Intel t265(Tracking camera):

### rs-trajectory-savetodisk_merge(matrix with images):
Here we ned camera to run the program . In this code we merge the above modified code (1) bag to matrix by rs-trajectory and (2) save-to-disk sequence images together. By combining the above we are able to get the matrix text file and the frames for every matrix or we can say that by this code we get many frames according to camera movement and get the matrix for every frame which is stored or saved.

**Now the question is what type data the matrix text file contains?**
 
The text file contains the ground truth poses (trajectory) for the sequences recorded. The txt file contains a N x 12 table, where N is the number of frames of this sequence. It contains the 3x4 transformation matrix. The matrix is stored in row aligned order. This is the most complicated part and time consuming part of the dataset because there is no any pre-defined tool or API to get this ground truth for every image you must use your own method and create a one to get this data and save it to the text file.

Basically, the data stored in text file of kitti dataset is the 3x4 matrix which is written by the horizontal concatenation of matrix R (3x3 rotation matrix) and translation vector t (3x1 vector). Every row of the txt file has 12 elements. These 12 elements correspond to elements of 3x4 matrix (written row wise). And this is it after creating these files the dataset for Odometry is ready to go. The sequence of frames are alreay recorded in the folder.


-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
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



-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

 



























