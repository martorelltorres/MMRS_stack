# :ocean::robot::ocean: MMRS_stack :ocean::robot::ocean: 
## MMRS definition
This stack presents a Marine Multi-Robot System (MMRS) composed by a number of Autonomous Underwater Vehicles (AUVs) exploring the sea floor and an Autonomous Surface Vehicle (ASV) acting as a communication relay point and as a central coordinator of the system. Our work focuses on the coordination algorithm for such an heterogeneous team of marine robots. Real-world applications of such systems include deploying multiple marine vehicles to detect and assess the abundance of commercially valuable species on the seabed or to identify potential safety hazards in a given area. The main goal of the system is the reduction of the communication latency between the acquisition and transmission of data from randomly scattered objects of interest on the seafloor.

## System prerequisites
The recommended setup to run the current version of the MMRS_stack include the [COLA2](https://iquarobotics.com/cola2) architecture, Ubuntu 20.04 LTS and [ROS Noetic](https://wiki.ros.org/noetic).
We recommend to install the ROS desktop-full version (ros-noetic-desktop-full).

## Installation
Create a [catkin_ws](https://wiki.ros.org/catkin/Tutorials/create_a_workspace), and run the following commands:
```
    cd catkin_ws/src
    git clone https://github.com/martorelltorres/MMRS_stack.git
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
Install [Iquaview](https://bitbucket.org/iquarobotics/iquaview/src/master/) into the MMRS_stack using the installation procedure detailed in the link.

Finally compile the MMRS_stack:
```
    cd catkin_ws
    catkin build
```

## MMRS workflow
### 1-Define the area exploration
To define the area exploration you should run Iquaview, choose a waypoints mission template and draw the area tha you would like to explore. Save the mission in the ~/MMRS_stack/multi_robot_system/missions
### 2-Divide the area exploration
Once the exploration area has been defined, it must be divided into as many parts as explorer robots you wish to use in the system.
### 3-Run the exploration process
Once the exploration area has been defined, it must be divided into as many parts as explorer robots you wish to use in the system.

