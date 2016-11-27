# Kinect installation for Ubuntu&ROS
This tutorial will teach you how to install kinect drivers on Ubuntu 14.04 and it is also helpful for install kinects driver for ROS.
Although you install ROS on ubuntu, you still have to install kinect drive in order to use in ROS.

## Install 3rd party lib
````
sudo apt-get install libopenni-dev libopenni-sensor-primesense-dev
sudo apt-get install python libusb-1.0-0-dev freeglut3-dev doxygen graphviz
````

## 1. JVM
Download jdk-8u73-linux-x64.tar on java official website or download from our drive.

````
sudo mkdir -p /usr/lib/jvm/
sudo cp -r jdk1.8.0_73/ /usr/lib/jvm/

sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_73/bin/java" 1
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0_73/bin/javac" 1
sudo update-alternatives --install "/usr/bin/jar" "jar" "/usr/lib/jvm/jdk1.8.0_73/bin/jar" 1

sudo update-alternatives --config java
sudo update-alternatives --config javac
sudo update-alternatives --config jar
````


## 2.Install OpenNI from source
This is required for the kinect interface

	sudo apt-get install git-core cmake freeglut3-dev pkg-config build-essential libxmu-dev libxi-dev libusb-1.0-0-dev doxygen graphviz mono-complete

Now clone the code and set it up

```` 
mkdir ~/kinect
cd ~/kinect
git clone https://github.com/OpenNI/OpenNI.git
````

This thing has a bizarre install scheme. Do the following:
````
cd OpenNI/Platform/Linux/CreateRedist/
chmod +x RedistMaker
./RedistMaker
````
Now this creates some distribution. One of the two following cases should work. Else just look for a damn compiled binary, extract it and install it.
````
Case 1:
cd Final
tar -xjf OpenNI-Bin-Dev-Linux*bz2
cd OpenNI- ...
sudo ./install.sh
````
````
Case 2:
cd ../Redist/OpenNI-Bin-Dev-Linux-x64-v1.5.2.23/
sudo ./install.sh
````

## 3. Install Sensor Kinect
Yet another library for the Kinect.. (Why can't anyone package all this into one distribution)..
There are two versions of this. One should work.. (i.e., if you get some weird error with one, use the second one

	mkdir SensorKinect
	cd SensorKinect 

Version 1 : (this one did not work for Samir)

	git clone git://github.com/avin2/SensorKinect.git

Version 2 : (this one worked for Samir) This one work for me

	git clone git://github.com/ph4m/SensorKinect.git

Once you have the lib, go ahead and compile it in the same bizarre manner as OpenNI (well atleast they are consistent).
 ````
 cd SensorKinect/Platform/Linux/CreateRedist/
 chmod +x RedistMaker
 ./RedistMaker
````
Done compiling. Now install this.
````
cd Final
tar -xjf Sensor ...
cd Sensor ...
sudo ./install.sh
````
Test and Verifyvation If ROS is installed


**In Jade:**

	sudo apt-get install ros-jade-openni-launch

**In Indigo:**
	
	sudo apt-get install ros-indigo-openni-launch

**Test**
	
	roslaunch openni_launch openni.launch
	rosrun rviz rviz

**If ROS is not installed**
	
	NiViewer


## 4. Trouble shooting
Kinect driver and debug 
http://mitchtech.net/ubuntu-kinect-openni-primesense/
if you get the error:

	InitFromXml failed: Failed to set USB interface!  

the solution is to remove the gspca_linect kernel module:
	
	sudo rmmod gspca_kinect

## 5. Reference : 
http://mitchtech.net/ubuntu-kinect-openni-primesense/
https://bitbucket.org/samirmenon/scl-manips-v2/wiki/vision/kinect
