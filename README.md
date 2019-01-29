# rxr-01_ros

## Configuring Raspbery-PI 3B+ onborad of the Turtlebot3:

1) Download an image from [Ubiquity Robotics](https://downloads.ubiquityrobotics.com/pi.html)
   
   *Use instructions on how to deploy it onto Raspberry (trick: to have internet connection while using ssh to set all stuff, plug Ethernet cable into Raspberry)*
   
- On the first launch use `sudo raspi-config` to turn camera support on and resize OS to the whole memory card. Then reboot.
Then use instructions onto deployment of Turtlebot3 packages

**NB: Avoid " `wget https://raw.githubusercontent.com/ROBOTIS-GIT/robotis_tools/master/install_ros_kinetic_rp3.sh && chmod 755 ./install_ros_kinetic_rp3.sh && bash ./install_ros_kinetic_rp3.sh `" – this will introduce disorder into preconfigured by Ubiquity packages!**

Avoid *"size optimization"* step with packages deleting. In general those packages are used on a PC part in PC+Tb3 setting, but we tun but for current stage we'd decided to leave all code as it is. 
`sudo rm -r turtlebot3_description/ turtlebot3_teleop/ turtlebot3_navigation/ turtlebot3_slam/ turtlebot3_example/`
 

## Steps for onboard camera installation
USB-camera software can be installed via `sudo apt-get install ros-kinetic-usb-cam`
and `sudo apt-get install ros-kinetic-libuvc` -- we use this one because 'vanilla' usb-cam 
also `rosdep install libuvc_camera libuvc_ros`
NB: when running example like 'roslaunch usb_cam usb_cam-test.launch', go to launch file itself and set appropriate camera device (if you have several of them, set like <param name="video_device" value="/dev/video1" /> )

## udev 
- to get info on your vebcams:
`lsusb -v` -- you need some properties to identify them further in udev rules.
udev rules needed to grant access to camera resources:

at dir : `/etc/udev/rules.d` create file like ours: `99-uvc.rules`
change file to add access to your cameras like this, aim is to change access-mode:
```
# UVC cameras
SUBSYSTEMS=="usb", ENV{DEVTYPE}=="usb_device", ATTRS{idVendor}=="046d", MODE="0666"
# ^ Change the vendor and product IDs to match your camera.
```


## packages for speech-control
# TODO

## upstart configure 
example: 

`rosrun robot_upstart install turtlebot3_bringup/launch/turtlebot3_robot.launch --logdir /home/ubuntu/logs/tb3_bringup --user ubuntu`

# TODO make it all as install script
# websocket:
```sudo apt install ros-kinetic-rosbridge-server ros-kinetic-rosbridge-suite```

# services daemons autorun
