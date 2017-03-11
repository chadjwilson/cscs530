# Model Proposal for Resilience in Engineered Military Systems

Chad Wilson

* Course ID: CMPLXSYS 530,
* Course Title: Computer Modeling of Complex Systems
* Term: Winter, 2017



&nbsp; 

### Goal 
*****

The overall goal of this project is to create an agent-based model (ABM) of interconnected engineered, military systems that interact with one another in addition to a heterogeneous environment and an unpredictable enemy threat. Furthermore, I seek to gain a normative understanding through evaluation of various network configurations, functional allocations, and system behaviors that best lead to the desired emergence of resilience in this system-of-systems (SoS).

&nbsp;  
### Justification
****

ABM is the modeling approach for this system under study. Each node in the network represents an individual, intelligent military system and acts independently and collectively to achieve a desired outcome. The environment in which these systems operate is often unknown and heterogeneous. The threats that these systems encounter are often unknown and heterogeneous as well. Based on these system characteristics and the desire to evaluate the emergence of higher level behavior forms a strong case for choosing ABM as the modeling approach.




&nbsp; 
### Main Micro-level Processes and Macro-level Dynamics of Interest
****

**Macro-level Process**:  The military systems will be interconnected into a network arrangement analogous to a military unit structure. This formation will be given a mission with a performance objective. The mission will direct the formation to traverse the environment and interact with the enemy. The formation will become degraded by various environmental conditions and compromised by enemy attacks. The overall performance objective of the formation will be to withstand these negative impacts while maintaining the required capability level.

**Micro-level Proces**s: Each military system will be interconnected to a command system and its peer systems. Each military system will have a specified level of survivability (robustness). When a military system's survivability level is exceeded, it will disperse its functionality to its neighboring systems and then undergo maintenance for a period of time and be offline. Once repaired, this system will regain its functionality from its neighbors and become fully operational. As these systems move across the environment. particular system functions will be impacted by disparate environmental conditions causing a degradation in performance level of individual functions. Concurrently, these individual systems will be randomly attacked by enemy threats. The threats will attack specific functions and degrade them or render them inoperative which will require maintenance and downtime to repair.

**Overal objective**: for the formation to maintain a defined capability level throughout its mission.

&nbsp; 


## Model Outline
****
&nbsp; 
### 1) Environment

**Boundary conditions**:  the environment will be bounded within a finite, symmetric space

**Dimensionality**:  the environment will be constructed as a 250x250 2-dimensional array consisting of 62,500 total positions (sections)

**Environment variables**: 
- coverage: an array, (1...5), signifying the amount of foilage or other physical structures within a section; least to most
- terrain: an array, (1...5), signifying the ground condition and elevation of a section; mild to extreme 
- hazard: an array, (1...5), signifying the amound of hazards within a section; negligible to catastrophic
- threats: a nested array of 4 distinct threat types
 - mortars: an array, (1...5), signifying the impact level of a mortar attack at this sections on all system functions; negligible to catastrophic
 - ieds:  an array, (1...5), signifying the impact level of an ied attack at this section on all system functions (less than mortar attack); has less impact negligible to catastrophic
 - jamming:  an array, (1...5), signifying the impact level of a mortar attack at this section on C2 system function; negligible to catastrophic
 - nbc:  an array, (1...5), signifying the impact level of a mortar attack at this section on personnel; negligible to catastrophic
                  
 **Environment methods**:         
 
```python
# Include first pass of the code you are thinking of using to construct your environment
# This may be a set of "patches-own" variables and a command in the "setup" procedure, a list, an array, or Class constructor
# Feel free to include any patch methods/procedures you have. Filling in with pseudocode is ok! 
# NOTE: If using Netlogo, remove "python" from the markdown at the top of this section to get a generic code block
```

&nbsp; 

### 2) Agents
 
 _Description of the "agents" in the system. Things to specify *if they apply*:_
 
* _List of agent-owned variables (e.g. age, heading, ID, etc.)_
* _List of agent-owned methods/procedures (e.g. move, consume, reproduce, die, etc.)_


```python
# Include first pass of the code you are thinking of using to construct your agents
# This may be a set of "turtle-own" variables and a command in the "setup" procedure, a list, an array, or Class constructor
# Feel free to include any agent methods/procedures you have so far. Filling in with pseudocode is ok! 
# NOTE: If using Netlogo, remove "python" from the markdown at the top of this section to get a generic code block
```

&nbsp; 

### 3) Action and Interaction 
 
**_Interaction Topology_**

_Description of the topology of who interacts with whom in the system. Perfectly mixed? Spatial proximity? Along a network? CA neighborhood?_
 
**_Action Sequence_**

_What does an agent, cell, etc. do on a given turn? Provide a step-by-step description of what happens on a given turn for each part of your model_

1. Step 1
2. Step 2
3. Etc...

&nbsp; 
### 4) Model Parameters and Initialization

_Describe and list any global parameters you will be applying in your model._

_Describe how your model will be initialized_

_Provide a high level, step-by-step description of your schedule during each "tick" of the model_

&nbsp; 

### 5) Assessment and Outcome Measures

_What quantitative metrics and/or qualitative features will you use to assess your model outcomes?_

&nbsp; 

### 6) Parameter Sweep

_What parameters are you most interested in sweeping through? What value ranges do you expect to look at for your analysis?_
