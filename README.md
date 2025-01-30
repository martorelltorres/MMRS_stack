# MMRS_stack
## System prerequisites
The recommended setup to run the current version of the [COLA2](https://iquarobotics.com/cola2) architecture is Ubuntu 20.04 LTS and [ROS Noetic](https://wiki.ros.org/noetic).
We recommend to install the ROS desktop-full version (ros-noetic-desktop-full).

## Installation
Create a [catkin_ws](https://wiki.ros.org/catkin/Tutorials/create_a_workspace), and run the following commands:
``` cd catkin_ws/src
    git clone
    cd MMRS_stack/
    chmod +x install_first_time.sh
    ./install_first_time.sh
```

Wait for the code to download.

Install [cola2 architecture](https://iquarobotics.com/cola2) dependencies:
```
    sudo apt install ros-noetic-rosbridge-server ros-noetic-joy lm-sensors lcov python3-ruamel.yaml python-is-python3 python3-pip 
    pip3 install GitPython==3.1.42
```
    

Install cola2_lib:
```
        git clone https://bitbucket.org/iquarobotics/cola2_lib.git
        cd cola2_lib
        # check the required dependencies and then
        mkdir build
        cd build
        cmake ..
        make
        sudo make install
```


Install multi_robot_system package dependencies:
```
        pip install geopandas scipy gitpython shapely scikit-learn
```
Install [Iquaview](https://bitbucket.org/iquarobotics/iquaview/src/master/) following the installation procedure detailed in the link.

## MMRS workflow
### 1-Define the area exploration
To define the area exploration you should run Iquaview, choose a waypoints mission template and draw the area tha you would like to explore. Save the mission in the ~/MMRS_stack/multi_robot_system/missions
### 2-Divide the area exploration
Once the exploration area has been defined, it must be divided into as many parts as explorer robots you wish to use in the system.
### 3-Run the exploration process
Once the exploration area has been defined, it must be divided into as many parts as explorer robots you wish to use in the system.

