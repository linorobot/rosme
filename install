#!/usr/bin/env bash

set -e

ARCH=$(uname -i)
RELEASE=$(lsb_release -c -s)

if [ $RELEASE == "trusty" ]
    then
        ROSDISTRO=indigo

elif [ $RELEASE == "xenial" ]
    then
        ROSDISTRO=kinetic
       
elif [ $RELEASE == "bionic" ]
    then
        ROSDISTRO=melodic
elif [ $RELEASE == "focal" ]
    then
        ROSDISTRO=noetic
else
    echo "There's no ROS Distro compatible for your platform"
    exit 1
fi

echo Installing ros-$ROSDISTRO

sudo apt update
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
sudo apt -y update
sudo apt -y install dpkg

if [ $ARCH == "x86_64" ]
    then
        sudo apt -y install ros-$ROSDISTRO-desktop-full
        echo "Installing ROS-$ROSDISTRO Full Desktop Version"

else  
    sudo apt -y install ros-$ROSDISTRO-ros-base
    echo "Installing ROS-$ROSDISTRO Barebones"
fi

source /opt/ros/$ROSDISTRO/setup.bash
echo "source /opt/ros/$ROSDISTRO/setup.bash" >> ~/.bashrc
source ~/.bashrc 

if [ $ROSDISTRO == "noetic" ]
    then
        sudo apt install -y python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
else
    sudo apt install -y python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
    sudo `which rosdep` init
fi

rosdep update

rosdep install --default-yes --from-paths . --ignore-src --rosdistro $ROSDISTRO

echo ""
echo "ROS $(rosversion -d) Installation Done!"
echo "You can create your catkin workspace now. https://wiki.ros.org/catkin/Tutorials/create_a_workspace"

#rosdep fail
#https://github.com/ros-infrastructure/rosdep/issues/322
