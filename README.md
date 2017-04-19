# ExampleAIClient-EligibilityTemperalDifference
use temperal difference + eligibility trace to learn policy for starcraft combat scenerio

# INTRODUCTION
Now I setup a reinforcement learning framework to teach 3 Goliath to beat 6 Zealot

The RL method I use is temperal difference learning + eligibility trace

I use a network to represent Q functions, and the policy is produced directly calculating the greedy action

## NN INPUT 
The network input is composed of 3 parts

---- 1. Current step agent state informatoin [dim=42]:

-------- own weapon cool down time [0~1], dim=1

-------- own hitpoint [0~1], dim=1

-------- self units distance in 8 directions; multiple distances are SUM in the same direction if existed; if no units in certain direction, it is 0; if unit is out of sight range, it is 0.05; dim=8.

-------- self units distance in 8 directions; multiple distances are MAXIMIZE in the same direction if existed; if no units in certain direction, it is 0; if unit is out of sight range, it is 0.05; dim=8

-------- enemy units distance in 8 directions; multiple distances are SUM in the same direction if existed; blablabla

-------- enemy units distance in 8 directions; multiple distances are MAXIMIZE in the same direction if existed; blablabla

-------- terrain/obstacle distance in 8 direction; if it is out of sight range, 0; dim=8

---- 2. Last step agent state information [dim=42]

-------- same as current step 

---- 3. Last step action [dim=9]

-------- Last step action input node is set to 1, other action input nodes are set to 0; dim=9

## TRAIN/TEST
use train project to train; every 100 episodes, the network parameters are saved in db file;

use test project to test; rename the saved db file to "starcraft_combat_nn_#(num_input)_#(num_hidden)_#(num_output).db" in the project folder, so the project can load the learn network 

# INSTALL
For details, you can refer to UAlbertaBot on how to setup the environment (https://github.com/davechurchill/ualbertabot/wiki) 

## starcraft
you first need to install StarCraft:Broodwar 1.16.1 to run starcraft

## BWAPI
Download BWAPI library in https://github.com/bwapi/bwapi

Add the BWAPI folder directory to Windows system variable BWAPI_DIR (Property of your Computer--Advance System Set--Environment Variable--System Variable--New)

Here we use client mode to connect our program with starcraft

you can also use module mode to connect starcraft, but it is not handy for debug and repeat the game which is highly needed in RL algorithms

## BWTA
Download BWTA library in https://bitbucket.org/auriarte/bwta2/wiki/Home

Add BWTA foler directory to Windows system variable BWTA_DIR 

In this projects, I didn't use BWTA library, since the walkable of terrain can be checked by BWAPI library

If you are designing complicated path finding algorithm, I think BWTA may be useful

## PROJECT
now you can open train/test project and compile

remember to add BWAPI/BWTA to your project: 

---- Project Property->C++>General->Additional Include Directories：$(BWAPI)/include and $(BWTA)/include

---- Project Property->Link->Input->Additional Dependency: 

$(BWAPI_DIR)/lib/BWAPId.lib
$(BWAPI_DIR)/lib/BWAPIClientd.lib
$(BWTA_DIR)/lib/BWTAd.lib
$(BWTA_DIR)/lib/libboost_system-vc120-mt-gd-1_56.lib
$(BWTA_DIR)/lib/libboost_thread-vc120-mt-gd-1_56.lib
$(BWTA_DIR)/lib/libCGAL-vc120-mt-gd-4.4.lib
$(BWTA_DIR)/lib/libgmp-10.lib
$(BWTA_DIR)/lib/libmpfr-4.lib
$(BWTA_DIR)/lib/libboost_filesystem-vc120-mt-gd-1_56.lib

(In this project, except BWAPId.lib and BWAPIClientd.lib, the rest are unneccesary)

(if you compile in RELEASE mode, change libs into release lib, like BWAPId.lib->BWAPI.lib)

## Chaoslauncher 
run Chaoslauncher.exe to start starcraft, check BWAPI Injector [DEBUG], Chaosplugin for 1.16.1, W-MODE 1.02 

config BWAPI Injector [DEBUG], open bwapi.ini, set ai, ai_dbg=null, auto_restart=ON, map=maps/BroodWar/Single player maps/Goliaths3Zealots6.scm, game_type = USE_MAP_SETTINGS, sound = OFF

Now you can see the running

# BLABLABLA
Now agent state surrounding environment representation is divided into 8 directions, it helps to simplify information representation, like convolution kernel in CNN

but I still didn't figure out how to represent multiple units information, for example if 3 enemys are at my top direction, one close, two far away, how I can define a form of information representation that can distinguish this case with anther case that 4 enemys are far from me at my top

if this is solved, then I can add enemies hitpoint information to state representation, then we can select the right enemy to attack it, currently I just select the lowest hitpoint enemy around own weapon attack range

actions between holdposition and attack should be seperated

multiagent cooperation is not considered in current project, and I have no idea use what technique to solve that problem, if that is solved, I can program these 6 zealots to attack Goliath in a high level operation :)

