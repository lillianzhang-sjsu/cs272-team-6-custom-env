# CS272-Final Project - Team 6
## Custom Environment - CAR ACCIDENT (AccidentEnv)
## Authors

- [Niyati Aggarwal](https://www.github.com/niyatiagg)
- [Rajshri Ganesh Iyer](https://github.com/GRIYER26)
- [Lillian Zhang](https://github.com/lillianzhang-sjsu)

## Overview
Our custom environment **'accident-v0'** simulates a **Car Accident** scenario built on top of [Highway-Env](https://highway-env.farama.org/environments/highway/) and [Gymnasium](https://gymnasium.farama.org/). The agent controls a vehicle with discrete actions and observes using Lidar sensors. Here are the novel customizations we have introduced into our environment - 

1. A 2-car crash on the highway, placed halfway down the road and spread across 2 lanes (adjacent to each other).   
2. Location of the crash is fixed, but the crash can occur in any 2 random lanes (adjacent to each other).   
3. A modified reward function with new rewards & penalties, suitable for a highway crash scenario.

## Crash Scenario Insertion
During reset(), the environment:
- Calls createroad() to build straight multilane road (4 lanes by default, 1000 meters long).
- Places two crashed vehicles at specific positions in a randomly chosen lane (crashlaneindex), with CrashedVehicle objects at positions 500 and 505 meters. These are static and act as "hazards" that the agent must react to.
- Populates the environment with other moving vehicles.

## Ego Vehicle (Agent) & Traffic Generation
- The agent-controlled vehicle is created at a start position and added to the road.
- Additional vehicles (traffic) are added using standard traffic vehicle logic, with randomized behavior and positions.

## Objective
The main objective of the ego-vehicle is to react to the crash on the highway and respond appropriately in the following ways to ensure safe and efficient driving.

1. Not colliding with other vehicles or the crashed  vehicles.  
2. Moving away from the crash lanes.   
3. Adjusting speed as it approaches crash, but resuming driving at high speed, once away from the crash.
4. Driving on the right-most lane.  
5. Not coming to a complete stop at any point.   
6. Not tailgating any vehicles at any point.   
7. Not driving off the road at any point.
   
## Reward Function
Our reward function retains some rewards from the original highway_env and introduces a couple of additional rewards and penalties, suitable for a crash scenario. The rewards are designed to foster driving at high speed, on the rightmost lanes, and to avoid collisions.


| **Reward Type** | **Description** | 
| :------- | :------ | 
| collision_reward | penalty for colliding with another vehicle| 
| high_speed_reward | reward for driving at posted high speed limit| 
| right_lane_reward | reward for driving on right-most lane|
| on_road_reward | reward for remaning on road |
| reaction_reward | penalty for being in crash lane(s) or close to crash| 
| tailgating_reward | penalty for tailgating |
| job_well_done_reward |reward for safely avoiding crash & driving on right-most lane|

## Termination & Truncation
**The episode terminates if** - 

1. The ego vehicle drives off the road.   
2. The ego vehicle collides with another vehicle.

**The episode truncates if** - 

1. The time limit is exceeded.

## File Descriptions
#### 1. custom_env.py
Defines the AccidentEnv class, extending highway-env abstractions. Implements road creation with crashed vehicles, vehicle spawning, reward functions, and episode termination conditions.
#### 2. run_custom_env.py
Script to run the environment with enhanced step-wise reward printing and optional manual control for interactive testing.
#### 3. README.md
This file, which has the documentation for our **accident_v0** environment


## Usage
### Clone our GitHUb repo
```git clone https://github.com/lillianzhang-sjsu/cs272-team-6-custom-env.git```
### Install depedencies 
```pip install highway-env```

You can experience the environment in manual control mode by running

```python run_custom_env.py```
## Citations
[1] E. Leurent, “An Environment for Autonomous Driving Decision-Making,” GitHub repository. [GitHub, 2018 Available Online](https://github.com/eleurent/highway-env)

[2] M. Towers et al., “Gymnasium.” [Zenodo, Mar. 2023 Available Online](https://zenodo.org/records/8127026)



