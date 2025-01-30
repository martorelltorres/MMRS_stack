# MMRS_stack

## Installation

Clone this repository and execute the script `install_first_time.sh`

    (MMRS_stack)$ ./install_first_time.sh

Wait for the code to download.

Install dependencies
        sudo apt install ros-noetic-rosbridge-server ros-noetic-joy lm-sensors lcov python3-ruamel.yaml python-is-python3 python3-pip 
        pip3 install GitPython==3.1.42

Install cola2_lib:
        git clone https://bitbucket.org/iquarobotics/cola2_lib.git
        cd cola2_lib
        # check the required dependencies and then
        mkdir build
        cd build
        cmake ..
        make
        sudo make install


Install other dependencies:
        pip install geopandas scipy gitpython shapely scikit-learn
