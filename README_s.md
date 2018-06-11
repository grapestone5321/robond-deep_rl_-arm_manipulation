# robond-deep_rl_arm_manipulation
Udacity robond-deep_rl_ arm_manipulation project


## Project Overivew

Welcome to the Deep RL Arm Manipulation project! For this project, your goal is to create a DQN agent and define reward functions to teach a robotic arm to carry out two primary objectives:

1. Have any part of the robot arm touch the object of interest, with at least a 90% accuracy.

2. Have only the gripper base of the robot arm touch the object, with at least a 80% accuracy.

Both of these objectives, have associated tasks that you will complete while working on this project. The upcoming sections will cover these tasks in detail.

## Workspaces

We will be providing you with a digital workspace to work on this project. The instructions to using the workspace are provided here. Follow the instructions in Booting the Desktop to launch your workspace's desktop gui.

The project folder, RoboND-DeepRL-Project, is already loaded into the project workspace for you and can be found in the /home/workspace directory. 

To launch the project for the first time, run the following:

``` bash
$ cd /home/workspace/RoboND-DeepRL-Project/build/x86_64/bin
$ ./gazebo-arm.sh
``` 


Note: Gazebo can often take time to load. As a byproduct of that, you might see errors similar to the following -

[Err] [Scene.cc:2927] Light [sun] not found. Use topic ~/factory/light to spawn a new light.

These errors should go away once Gazebo loads up completely, and can be ignored.

Once the gazebo environment loads up, you will observe the robotic arm, a camera sensor, and an object in the environment. The gazebo arm will fall to the ground after a short while, and the terminal will continuously display the following message:

ArmPlugin - failed to create DQN agent


Since no DQN agent has been defined as of now, the arm isn't learning anything and has no input to control it. In the next section, you will go through a list of tasks that will step you through the project!

Note: The gazebo environment can take some time to load when launching for the first time. If it's taking too long, or if it throws errors and doesn't completely launch, then stop the script and launch it again.

### Jetson TX2

The instructions to build and run the project on the TX2 can be found in the project repo.



