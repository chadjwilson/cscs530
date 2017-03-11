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

**Micro-level Process**: Each military system will be interconnected to a command system and its peer systems. Each military system will have a specified level of survivability (robustness). When a military system's survivability level is exceeded, it will disperse its functionality to its neighboring systems and then undergo maintenance for a period of time and be offline. Once repaired, this system will regain its functionality from its neighbors and become fully operational. As these systems move across the environment. particular system functions will be impacted by disparate environmental conditions causing a degradation in performance level of individual functions. Concurrently, these individual systems will be randomly attacked by enemy threats. The threats will attack specific functions and degrade them or render them inoperative which will require maintenance and downtime to repair.

**Overall objective**: for the formation to maintain a defined capability level throughout its mission.

&nbsp; 


## Model Outline
****
&nbsp; 
### 1) Environment

**Description**: the environment will represent a military operational environment and subdivided into sections to which military systems will interact directly with conditions and threats of their occupied section

**Boundary conditions**:  the environment will be bounded within a finite, symmetric space 

**Dimensionality**:  the environment will be constructed as a 250x250 2-dimensional array consisting of 62,500 total positions (sections)

**Variables**: 
- coverage: an array, (-1...-5), signifying the amount of foliage or other physical structures within a section; least to most
- terrain: an array, (-1...-5), signifying the ground condition and elevation of a section; mild to extreme 
- hazard: an array, (-1...-5), signifying the amount of hazards within a section; negligible to catastrophic
- threats: an array of 4 objects of each distinct threat type
 - mortars: an array, (-1...-5), signifying the impact level of a mortar attack at this sections on all system functions; negligible to catastrophic
 - ieds:  an array, (-1...-5), signifying the impact level of an IED (Improvised Explosive Device) attack at this section on all system functions (less than mortar attack); has less impact negligible to catastrophic
 - jamming:  an array, (-1...-5), signifying the impact level of a mortar attack at this section on C2 system function; negligible to catastrophic
 - nbc:  an array, (-1...-5), signifying the impact level of a mortar attack at this section on personnel; negligible to catastrophic
                  
**Methods**:
1) set_env_conditions: normal distribution of environmental conditions levels across all sections of the environment
2) set_env_threats: random distribution of threats across 10% of the environment

```
#Initialize environment
 env = create array(250x250)
 set_env_conditions()
 set_env_threats()
 
#Set Environment Conditions method
 for each array position in env
      set foilage_level = normal_distribution(-1...-5)
      set terrain_level = normal_distribution(-1...-5) 
      set hazard_level = normal_distribution(-1...-5) 
      
#Set Environment Threats method
 for a random 10% set of all array positions in env    
      set mortar_impact = -4 #impacts all system functions
      set ied_impact = -2 #impacts all system functions
      set jamming_impact = -3 #impacts C2 (Command & Control) system functions
      set nbc_impact = -3 #impacts personnel
```

&nbsp; 

### 2) Agents

**Description**:  the agents will represent an individual military system of type motorized on non-motorized (dismounted squad/team); agents will be networked together into a military unit topology analogous with an Army Company structure; agents will interact with one another and their local environment

**Variables**:
 - agent_role: a string that identifies the role of the military system within the unit structure; different roles have different specializations and, thus, different functional performance levels (leader, soldier or support)
 - agent_type: a string that identifies the type of the military system (motorized/non-motorized); impacts are increased by 1 for non-motorized types
 - agent_functions:  an array objects that identifies the functions of a military system and their specific performance levels (1...5)
 - agent_transfer: an integer that identifies amount of functional performance value transferred to neighboring military systems; to be returned upon repair
 - agent_failures: identifies number of active failures for each military system
 - agent_personnel:  an integer that identifies the overall performance of personnel within a military system; level depends on role and type 
 - agent_location: an array, (x,y), that identifies the location/section of the military system within the environment 
 - agent_inop: an integer representing number of days military system is to remain inoperable and under repair
 
**Methods**:
 - init_unit: set network topology and assign initial agent variable
 - move_agent:  move agent to random neighbor location
 - assess_env_impact:  update functional performance levels based on environmental impacts
 - assess_threat_impact:  update functional performance levels based on threat impacts
 - transfer_agent_function: transfer agent functions if degraded below threshold (add remaining performance level to random neighbor)
 - repair_agent: decrements inop time by 1 for each time step
 
```
#Initialize agents
 unit = init_unit()
 
#Initialize Unit method
 create unit as graph
 create agents as nodes
 connect agents with edges
 assign initial variable values to agents
 exit
 
#Move Agent method
 for each agent check if not inop
  move to random neighbor section if not occupied
  else don't move
 exit
 
#Assess Environmental Impact method
 for each agent check if not inop
  update agent functional performance level due to environmental conditions
  #coverage (-1...-5) impacts survivability (inverse: if -1 then -5 impact) and lethality (direct: if -1 then -1 impact)
  #terrain (-1...-5) impacts mobility and C2 (direct: if -1 then -1 impact)
  #hazard (-1...-5) impacts on personnel (direct: if -1 then -1 impact)
 exit
 
 #Assess Threat Impact method
 for each agent check if not inop
  update agent functional performance level due to environmental threats
  #mortars (-4) impacts all functions and personnel performance goes to 0
  #ieds (-2) impacts all functions and personnel performance goes in half
  #jamming (-3) impacts C2 functions only
  #nbc (-3) impacts personnel only
 exit
 
 #Transfer Agent Functionality method
  for each agent if not inop
   for each function
    if personnel performance level is below performance threshold
     then agent is inop and set agent repair time
    if check functional performance level is less than performance threshold #system function degraded or failed
     then transfer remaining performance value to random neighbor
     update transfer account with amount of transferred performance value
   if more than 2 system functions degraded or failed
    then agent is inop and set agent repair time
  exit
    
  #Repair Agent method
  for each agent if inop
   decrement inop time by 1
   if agent inop time = 0 
    then reset functional and personnel performance levels 
    #functional performance is restored by neighbor agents where functional performance is not degraded
    # personnel performance is reset to initial value
  exit
```

&nbsp; 

### 3) Action and Interaction 
 
**_Interaction Topology_**
 
The agents of the ABM will be connected in a network topology analogous to the structure depicted in the image below. The network will be a fixed structure and will not be adaptive at this time.

![Army Company Structure](http://www.globalsecurity.org/military/library/policy/army/fm/3-21-11/image624.jpg)
 
**_Action Sequence_**

During each time step each active agent of the network will move to a neighboring section and interact with its environment, take on impact from local threats and interact with neighboring agents if necessary.

Here is the activity flow for each time step:
 1. move agents that are active
 2. assess impacts of active agents
 3. if too many failures then go inop and get repaired
 4. if personnel performance is too low go inop and heal

&nbsp; 
### 4) Model Parameters and Initialization

**Global Variables**:  
 - capability_level:  identifies the cumulative functional performance of all agents and their functions
 - effectiveness_level:  identifies the acceptable level of capability throughout a mission to deem it a success
 - performance_threshold:  identifies the level when functional performances are considered degraded or failed
 - repair_time:  identifies the amount of time (in steps) an inop agent will be in maintenance and not active
 - mission_duration: identifies the number of time steps to execute the model
 - threat_level (future use):  identifies the cumulative threat level of all environment sections

**Simulation Procedure**:  
  1. initialize global variables
  2. initialize environment
  3. initialize agents
  4. until mission_duration not met:
  -- execute the model
  -- draw model
  -- update global variables (capability_level)
 5. plot global variables (capability_level against effectiveness_level)

&nbsp; 

### 5) Assessment and Outcome Measures

The model will be assessed by evaluating how well various unit structures and functional performance allocations are able to uphold the capability_level of the overall unit to a desired effectiveness_level for the duration of a mission.

&nbsp; 

### 6) Parameter Sweep

In order to determine which unit structures and functional performance allocations perform the best (smallest variance between capability_level and effectiveness_level over duration of mission) relative to one another, a parameter sweep will be conducted against the functional performance levels in addition to random structuring of the unit topology.
