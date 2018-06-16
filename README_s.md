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

Note: Gazebo can often take time to load. As a byproduct of that, you might see errors similar to the following 

``` bash
[Err] [Scene.cc:2927] Light [sun] not found. Use topic ~/factory/light to spawn a new light.
``` 

These errors should go away once Gazebo loads up completely, and can be ignored.

Once the gazebo environment loads up, you will observe the robotic arm, a camera sensor, and an object in the environment. The gazebo arm will fall to the ground after a short while, and the terminal will continuously display the following message:

ArmPlugin - failed to create DQN agent


Since no DQN agent has been defined as of now, the arm isn't learning anything and has no input to control it. In the next section, you will go through a list of tasks that will step you through the project!

Note: The gazebo environment can take some time to load when launching for the first time. If it's taking too long, or if it throws errors and doesn't completely launch, then stop the script and launch it again.

### Jetson TX2

The instructions to build and run the project on the TX2 can be found in the project repo.

## Tasks

In the previous lesson, we discussed the code for the ArmPlugin.cpp file. As you might have noticed, the file has specific TODOs or tasks listed out for the Project. You have to complete each of those tasks in the following order - 

### 1. Subscribe to camera and collision topics.
The nodes corresponding to each of the subscribers have already been defined and initialized. You will have to create the subscribers in the ArmPlugin::Load() function , as covered in the previous lesson, based on the following specifications - 

For the camera node - 
- Node - cameraNode
- Topic Name - /gazebo/arm_world/camera/link/camera/image
- Callback Function - ArmPlugin::onCameraMsg (this should be a reference parameter)
- Class Instance - refer to the same class, using the “this” pointer or keyword.

For the contact/collision node -
- Node - collisionNode
- Topic Name - /gazebo/arm_world/tube/tube_link/my_contact
- Callback Function - ArmPlugin::onCollisionMsg (this should be a reference parameter)
- Class Instance - refer to the same class, using the “this” pointer or keyword.

### 2. Create the DQN Agent

Refer to the API instructions to create the agent using the Create() function from the dqnAgent Class, in ArmPlugin::createAgent(). You will pass the variable names defined at the top of the file to this function call. You can also refer to the constructor in project_folder/c/dqnAgent.cpp for the same.

### 3. Velocity or position based control of arm joints

Previously, we discussed how the DQN output is mapped to a particular action, which, for this project, is the control of each joint for the robotic arm. In ArmPlugin::updateAgent(), there are two existing approaches to control the joint movements. You can either use one of these approaches, or you can come up with your own solution.

- Velocity Control - The current value for a joint’s velocity is stored in the array vel with the array lengths based on the number of degrees of freedom for the arm. If you choose to select this control strategy, modify the following line of code to assign a new value to the variable velocity based on the current joint velocity and the associated delta value, actionVelDelta.

``` bash
float velocity = 0.0;
``` 

- Position Control - The current value for a joint’s position is stored in the array ref with the array lengths based on the number of degrees of freedom for the arm. If you choose to select this control strategy, modify the following line of code to assign a new value to the variable joint based on the current joint velocity and the associated delta value, actionJointDelta.

``` bash
float joint = 0.0;
``` 

TIP: To find out which joint’s value to change (corresponding index to either of the arrays, ref or vel) you can define the index as a function of the variable action. This is helpful if you want to have a more complicated (more degrees of freedom) arm without having to define new set of conditions. The default number of actions, defined in DQN.py, is 3; and there are two outputs for every action - increase or decrease in the joint angles.

The next set of tasks are based on creating and assigning reward functions based on the required goals. There are a few important variables in relation to rewards - 

rewardHistory - Value of the previous reward, you can set this to either a positive or a negative value.

REWARD_WIN or REWARD_LOSS - The values for positive or negative rewards, respectively.

newReward - If a reward has been issued or not.

endEpisode - If the episode is over or not.

Note - For each of the following, you have to decide what rewards to use, and whether or not it’s a new reward and whether or not to trigger an end of episode.

### 4. Reward for robot gripper hitting the ground

In Gazebo’s API, there is a function called GetBoundingBox() which returns the minimum and maximum values of a box that defines that particular object/model corresponding to the x, y, and z axes. 

For example, for the bounding box defined for the robot’s base, you will have the attributes box.max.x and box.min.x to obtain the minimum and maximum values of the box in reference to the x-axis. 

Using the above, you can check if the gripper is hitting the ground or not, and assign an appropriate reward. The bounding box for the gripper, and a threshold value have already been defined for you in the ArmPlugin::OnUpdate() method.

### 5. Issue an interim reward based on the distance to the object
In ArmPlugin.cpp a function called “BoxDistance()” calculates the distance between two bounding boxes. Using this function, calculate the distance between the arm and the object. Then, use this distance to calculate an appropriate reward as well.

TIP: One recommended reward is a smoothed moving average of the delta of the distance to the goal. It can be calculated as - 
average_delta  = (average_delta * alpha) + (dist * (1 - alpha));

As we covered in the previous section, there are two primary objectives to the project - 

Any part of the robot arm should touch the object with atleast an accuracy of 90%.

Only the gripper base of the robot arm should touch the object with at least an accuracy of 80%.

The following reward function will help you with the first part of the objectives.

### 6. Issue a reward based on collision between the arm and the object.

In the callback function onCollisionMsg, you can check for certain collisions. Specifically, you will define a check condition to compare if particular links of the arm with their defined collision elements are colliding with the COLLISION_ITEM or not. The function already contains some code that can help you with this task.

You can then assign the appropriate rewards.

TIP: The name of the collision elements are defined in the gazebo-arm.world file. For example, for link 2, the collision element is 
called collision2.


At this stage, it’s recommended that you test your project so far. Once in your project folder - 

$ cd build
$ make
$ cd x86_64/bin
$ ./gazebo-arm.sh

Note: Gazebo can often take time to load. As a byproduct of that, you might see errors similar to the following -

[Err] [Scene.cc:2927] Light [sun] not found. Use topic ~/factory/light to spawn a new light.

These errors should go away once Gazebo loads up completely, and can be ignored.
Once the environment/gazebo is loaded, you should notice the arm trying to move but it might not be learning. This brings us to the next task for the project.

### 7. Tuning the hyperparameters
The list of hyperparameters that you can tune is provided in ArmPlugin.cpp file, at the top. They are also listed below:
#define INPUT_WIDTH   512
#define INPUT_HEIGHT  512
#define OPTIMIZER "None"
#define LEARNING_RATE 0.0f
#define REPLAY_MEMORY 10000
#define BATCH_SIZE 8
#define USE_LSTM false
#define LSTM_SIZE 32

Tune the above hyperparameters to obtain the required accuracy. You might also have to revisit your reward functions and tune or modify them as needed.

Once you obtain the desired accuracy percentage, take a screenshot of your terminal or record a video of both your terminal and your arm touching the object. You can then move on to the second objective for the project.

### 8. Issue a reward based on collision between the arm’s gripper base and the object.
For this task of the project, modify the collision check defined in task 6, to check for collision between the gripper base and the object. And then, assign the appropriate reward.

Run your project again, and observe how the agent performs. Tune the rewards or the hyperparameters to obtain the required accuracy, if required. 

