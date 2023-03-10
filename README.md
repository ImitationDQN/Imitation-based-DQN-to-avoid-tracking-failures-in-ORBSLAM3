# Imitation-based-DQN-to-avoid-tracking-failures-in-ORBSLAM3

Imitation DQN aims to bring a supervised learned agent which can navigate in any indoor evironment using SLAM without having to face the tracking failures. 

We launch our training files as well as the pre-trained agent for the research community to test our agent and reuse the agent for further improvements.

## 1. Pre-requisites

* Ubuntu 16.04, 180.04 or 20.04
* C++ 11 Compiler
* Pangolin 
https://github.com/stevenlovegrove/Pangolin.
* OpenCV 3.4 or higher
* Eigen3
Required by g2o (see below). Download and install instructions can be found at: http://eigen.tuxfamily.org. Required at least 3.1.0.
* MINOS simulator or any preferred simulator
https://github.com/minosworld/minos
* ORB-SLAM3
https://github.com/UZ-SLAMLab/ORB_SLAM3
* ROS

## 2. Environemnt Setup

For agent training and testing, we are using MINOS as simulator, localization and mapping is done with the help of ORB-SLAM3 and the environment control is achieved via ROS topics. After successful installation of all prerequisites, follow these simple steps to set up the complete environment. 

### Camera Calliberation

Copy the MINOS camera calliberation file (MINOS.yaml) into the folder ORBSLAM3/Examples. 

### ROS NODE: 

This ROS node is important, simply paste the attached ROS node (merger) in catkin_ws/src and do necessary changes to CmakeList.txt by Uncommenting following:

*add_compile_options(-std=c++11)*

*add_executable(${PROJECT_NAME}_node src/merger.cpp)*

*target_link_libraries(${PROJECT_NAME}_node*
  
  *${catkin_LIBRARIES})*

*Add find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
image_transport
std_msgs
cv_bridge
)*


Later, catkin_make and test by running following command:
 
*rosrun merger merger_node*

### Changes in MINOS simulator:

replace lines 98-102 with

if 'logdir' in params:

            self._logdir = '/home/frames'
            
        else:
            
            self._logdir = '/home/frames'


and replace line 422-428 with

*if self.params.get(???save_png???):*

  *if image is None:*

  *image = Image.frombytes(mode,(data.shape[0], data.shape[1]),data)*

  *time.sleep(0.06)*

  *image.save(???/home/frames/color_.jpg???)*

and save Simulator.py

### Bash File

Simply download the given bash file and from terminal run *bash start.sh*

This will start the SLAM3 and MINOS simulator. A rostopic will also be initiated in parallel by this file which is responsible for publishing images over SLAM3. 


## 3. Train and Test the agent

* Download the agent.pt and Imitation_agent.py and place in these files catkin_ws/src/merger_node/scripts
* Now from the scripts of merger_node provide in terminal the following command for testing and training. 

*python3 imitation_agent.py --dqn_test*

*python3 imitation_agent.py --dqn_train*



