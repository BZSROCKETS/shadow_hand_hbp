# shadow_hand_hbp

Will edit it later.

MoveIt parts:
1. start moveit_setup_assistant:
  roslaunch moveit_setup_assistant setup_assistant.launch

  1.1 create new moveit configuration package, choose the urdf file in folder "sr_description/urdf"
  1.2 self collision checking, select to "high", then generate collision matrix
  1.3 no virtual joints
  1.4 define planning group, name it "sr_moveit", choose kinematic solver "KDLKinematicsPlugin";
      then add all the joints, except the "rh_world_joint"
  1.5 robot poses
  1.6 no end effector
  1.7 no passive joints
  1.8 ROS control
      1.8.1 add "FollowJointTrajectory" controller, add all the joints
      1.8.2 add "position_controllers/JointTrajectoryController", add all the joints
  1.9 save package in path "sr_moveit"

2. launch everything
   roslaunch sr_moveit demo_gazebo.launch

   error:
        RLException: unused args [config] for include of [/home/bing/catkin_ws/src/sr_moveit/launch/moveit_rviz.launch] The traceback for the exception was written to the log file

   solution:
        delete this line <arg name="config" value="true"/>, within <include file="$(find sr_moveit)/launch/moveit_rviz.launch">

   try again:
   error:
        Action client not connected: sr_position_controller/follow_joint_trajectory
   solution:
        rename the controller in "sr_moveit/config/ros_controllers.yaml"
        rename the controller in "sr_moveit/launch/ros_controllers.launch"
        add the controller "joint_state_controller" in "sr_moveit/launch/ros_controllers.launch"

   "Fixed Frame" error in RVIZ, can be solved by adding two lines in the demo_gazebo.launch file, under comment "If needed, broadcast static tf for robot root"
      <node pkg="tf" type="static_transform_publisher" name="world_broadcaster" args = "0 0 0 0 0 0 world base_link 10" />
      <node pkg="tf" type="static_transform_publisher" name="map_broadcaster" args = "0 0 0 0 0 0 world map 10" />
