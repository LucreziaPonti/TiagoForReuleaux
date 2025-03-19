# **TIAGo packages**

This folder contains all the packages provided by Pal Robotics used to simulate the robot TIAGo. 

This repo has been used for the work done on the repository Reuleaux_new (https://github.com/LucreziaPonti/Reuleaux_new), therefore it contains some files that have been slightly modified from the original to better fit the work done. 
For a clean installation follow the instructions and tutorials at: https://wiki.ros.org/Robots/TIAGo/Tutorials  

### Installation
To use TIAGo (with reuleaux - make sure to have done all the installation from the README) you can clone this repo, and install these additional packages that are required:
```
sudo apt install ros-noetic-four-wheel-steering-msgs ros-noetic-ddynamic-reconfigure ros-noetic-people-msgs ros-noetic-urdf-geometry-parser ros-noetic-twist-mux ros-noetic-teb-local-planner
```


### Changes from original 
- *tiago* : added metapackage useful for building - this repo contains many pkgs so sometimes it is convenient to use separate metapackages for the different parts of the ws so to build only the necessary ones (using catin build)
- *tiago_ikfast_arm_plugin*: added package that contains the plugin for the IKFast kinematic solver developed for the planning group containing the arm (not the torso!);
- *tiago_robot>tiago_description>urdf>torso.urdf.xacro* : added a frame (and tf) *torso_footprint* useful in the *reuleaux_bp_to_nav* package (see end of file);
- *tiago_robot>tiago_description>robot>tiago.urdf.xacro* : added a plugin for the grasp (see end of file) - see the Grasp (**still needs to be setup**) repo for more info;
- *tiago_moveit_config>cofig>kinematics_ik_fast.yaml* : added the config file to use the IKFast solver with moveit. Since IKFast is only for the arm group here is also added the KDL solver for the arm_torso group;
- *tiago_moveit_config>config>srfd>tiago_pal-gripper.srdf and tiago_pal-gripper_schunk-ft.srdf*: added a tf to connect odom and base_footprint and changed the parent group of the end-effector to arm;
- *tiago_moveit_config>launch>demo.launch* : added the arg *use_rviz* ;
- *tiago_moveit_config>launch>move_group.launch* : changed the default of *fake_execution* to true;
- *tiago_moveit_config>launch>moveit_rviz.launch and planning_context.launch* : changed the default for Kinematics to ik_fast;


## Useful commands: 
Simple simulation launch:
```
roslaunch tiago_gazebo tiago_gazebo.launch public_sim:=true end_effector:=pal-gripper
```
Tiago with moveit launch:
```
roslaunch tiago_moveit_config demo.launch
```
Tiago with navigation stack (use the rviz button) [for more detailed instructions see tutorial in ros wiki and raccolta_comandi_navigazione_tiago.txt in "useful stuff" folder]: 
```
roslaunch tiago_2dnav_gazebo tiago_navigation.launch public_sim:=true lost:=true map:=*path to map folder*
```

### Control of tiago (to use with simulation already started):
Key teleop: 
```
rosrun key_teleop key_teleop.py
```

PLAY MOTIONS:
```
rosparam list | grep  "play_motion/motions" | grep "meta/name" | cut -d '/' -f 4
rosrun play_motion run_motion *motion name*
```
Joint control: 
```
rosrun rqt_joint_trajectory_controller rqt_joint_trajectory_controller
```