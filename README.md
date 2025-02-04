# :ocean::robot::ocean: MMRS_stack :ocean::robot::ocean: 
## MMRS definition
This stack presents a Marine Multi-Robot System (MMRS) composed by a number of Autonomous Underwater Vehicles (AUVs) exploring the sea floor and an Autonomous Surface Vehicle (ASV) acting as a communication relay point and as a central coordinator of the system. Our work focuses on the coordination algorithm for such an heterogeneous team of marine robots. The main goal of the system is the reduction of the communication latency between the acquisition and transmission of data from randomly scattered objects of interest on the seafloor. In the context of this research,latency refers to the time elapsed between the detection of objects of interest scattered in the environment and stored in
the AUVs memory and the moment when they are received by the ASV.
![Alt text](https://github.com/martorelltorres/MMRS_stack/blob/main/images/comm_squeme.svg)
Our approach presents an MMRS coordination strategy in which multiple explorer AUVs acoustically report their status to an ASV coordinator, which manages the decision-making process. The ASV uses information that characterises the communication with each AUV to decide which AUV should go to collect information from the explored environment. In order to achieve the objective, this paper proposes a new multi-robot task allocation (MRTA) based on Adaptive Response Threshold Method (ARTM) and Ordered Weighted Average (OWA) aggregation methods.

## System prerequisites
The recommended setup to run the current version of the MMRS_stack include the [COLA2](https://iquarobotics.com/cola2) architecture, Ubuntu 20.04 LTS and [ROS Noetic](https://wiki.ros.org/noetic).
We recommend to install the ROS desktop-full version (ros-noetic-desktop-full).
![Alt text](https://github.com/martorelltorres/MMRS_stack/blob/main/images/system%20(1).jpg)

## Stack installation
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

## Working with MMRS_stack
### 1- Define and divide the area exploration
To define the area exploration you should run Iquaview as detailed [here](https://bitbucket.org/iquarobotics/iquaview/src/master/). Choose a waypoints mission template, draw the area tha you would like to explore and save the mission.xml in the multi_robot_system/missions.
Make sure that you set the:
```
number_of_robots: 6
pickle_path:'~/catkin_ws/src/multi_robot_system/missions/pickle/6AUVs/20000.pickle'
exploration_area: 20000
```
 params located into the ~/catkin_ws/src/MMRS_stack/multi_robot_system/config/data_extraction.yaml.
```
    cd ~/catkin_ws/src/MMRS_stack/multi_robot_system/src
    python polygon_division.py
```
As a result you will obtain something like this:

![Alt text](https://github.com/martorelltorres/MMRS_stack/blob/main/images/area_partition%20(1).jpg)

Where the exploration area will have been divided into as many sub regions as AUVs have been defined. This pre-process system divides the area of interest into sub-regions that will be assigned to each of the explorer AUV, and generates a series of objects of interest (priority and regular) within the region.

### 2- Define the aggregation method and set the weights of each stimulus
The data_extraction.yaml file,located in multi_robot_system/config folder, contains all the parameters involved in the coordination process of the multi-robot system. The system implements two aggregation methods, ARTM and OWA. These methods are used by the ASV to decide to which AUV it should go to collect information.  
```
aggregation_model: Set to 1 if you want to use ARTM or 2 if you want o use OWA.
alpha: 5 #[0-10] data stimulus
beta: 5 #[0-10] distance stimulus
w1: 3 #[0-1]
w2: 3 # [0-1]
w3: 4 #[0-1]
```
  
### 3- Define the tracking strategy and area coverage parameters
Once the ASV has decided which AUV to go to in order to collect the information through the acoustic channel, a tracking strategy detailed in this [article](https://www.mdpi.com/1424-8220/23/1/109) will be applied. This strategy is defined by three parameters, defined in the multi_robot_system/config/multi_robot_ystem.yaml file:
```
repulsion_radius: 10 #delineates the area in which the ASV implements a repulsion strategy to prevent potential collisions with the AUV.
adrift_radius: 20 #indicates the optimal communication distance at which the acoustic signal is most effective.
tracking_radius: 30 #defines the area within which the ASV must track an AUV.
```
As far as the area coverage is concerned you must define the following parameters:
```
# NED origin definition, the latitude and longitude point of reference
ned_origin_lat: 39.543330
ned_origin_lon: 2.377940
offset_coverage_distance: 5 #sets the distance between the coverage lines path.
```
> [!CAUTION]
> It is essential to establish a NED origin near the designated exploration area; otherwise, the COLA2 security mechanisms will prevent the activation of the robot architectures.

The AUVs follow a back-and-forth coverage pattern defined [here](https://www.sciencedirect.com/science/article/pii/S0141118723003899) with the objective of explore their assigned sub-area.

![Alt text](https://github.com/martorelltorres/MMRS_stack/blob/main/images/coverage%20.jpg)

### 4- Run the exploration process
In order to launch the multi robot system yo should run the following command:

```
roslaunch multi_robot_system mrs.launch

```
## Results
In this scene, 6 AUVs are seen performing an exploration of a predefined area, while the ASV applies the coordination mechanism outlined before. The ASV follows the vehicles and receives information about the objects of interest present in the environment. Regular objects are depicted as green balls, while objects of interest are shown as red balls. The circumscribed circles highlighted in red, green, and blue represent the different zones of the AUV tracking strategy: the repulsion area, the resting area, and the tracking area, respectively. 
![Alt text](https://github.com/martorelltorres/MMRS_stack/blob/main/images/simulation%20.jpg)

## Related publications
1. [Xiroi II, an Evolved ASV Platform for Marine Multirobot Operations](https://www.mdpi.com/1424-8220/23/1/109)
2. [Coordination of marine multi robot systems with communication constraints](https://www.sciencedirect.com/science/article/pii/S0141118723003899)
3. [Exploring Communication in Marine Multi-robot Systems
Using an Adaptive Response Threshold Model](https://www.researchgate.net/profile/Sergey-Yurish/publication/378144846_Proceedings_of_the_4th_IFSA_Winter_Conference_on_Automation_Robotics_and_Communications_for_Industry_4050_ARCI_2024/links/65c9fe381bed776ae34ac345/Proceedings-of-the-4th-IFSA-Winter-Conference-on-Automation-Robotics-and-Communications-for-Industry-40-50-ARCI-2024.pdf?__cf_chl_tk=TdFtB_kYlMr5Gx7LRoRz7DGK3_asGLqNv_eRmqHMq88-1738583599-1.0.1.1-BEJoQ3PcasJPl4rSHCN.zqZou_.sh5Vm0W0MKcG6L90#page=302)



