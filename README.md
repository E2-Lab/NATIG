**N**etwork **A**ttack **T**estbed **I**n [Power] **G**rid (**NATI[P]G**), a co-simulation environment for distribution power grid network using state-of-the-art simulators.

It is a standalone, containerized, and reusable environment to enable cyber analysts and researchers to run different cyber security and performance scenarios on powergrid and generate benchmark datasets. It supports IEEE123 and IEEE9500 as the reference powergrid models with communication model using mesh/ring/star topologies. The communication model supports CSMA, WiFi, 4G, and 5G (optional) protocols. 

We demonstrate few attack scenarios in the default framework and provide ways to design additional scenarios using configuration files.

## Table of contents
[How to get started](https://github.com/pnnl/NATIG/tree/master/README.md#how-to-get-started)

[Out of the box examples (4G)](https://github.com/pnnl/NATIG/tree/master/README.md#out-of-the-box-examples)

[5G configuration](https://github.com/pnnl/NATIG/tree/master/README.md#5G-configuration)

[Out of the box examples (5G)](https://github.com/pnnl/NATIG/tree/master/README.md#5G-out-of-the-box-example)

[How to check if the code is running](https://github.com/pnnl/NATIG/tree/master/README.md#Is-the-code-running)

[How to stop the code](https://github.com/pnnl/NATIG/tree/master/README.md#How-to-stop-the-run)

[How to update code](https://github.com/pnnl/NATIG/tree/master/README.md#Getting-an-updated-version-of-the-code)

[Citation](https://github.com/pnnl/NATIG/tree/master/README.md#Reference)

## How to get started
1. Have docker installed
2. clone the NATIG repo ```git clone https://github.com/pnnl/NATIG.git```
3. run the following commands:
   ```
   - cd NATIG
   - bash make_run_docker.sh
   ```
   - If you are using MINGW64 (such as gitbash) on windows, please edit the rundocker.sh: ```the input device is not a TTY.  If you are using mintty, try prefixing the command with 'winpty' ```
     
**NOTE: The default docker container does not come with 5G enabled**

Once the setup is done you can run the out of the box examples
1. go to the control folder inside the integration folder that is located in the home rd2c folder (full path: _/rd2c/integration/control_)
2. make sure that all TP*.txt files are removed by running ` rm -r TP*.txt `
3. run sudo bash run.sh _Full path of work directory (example for docker: /rd2c/)_  _[3G|4G|5G]_ _[RC|emptyString]_ _[9500|123]_ _[conf|noconf]_

**Note: example command for docker to run 4G example: ` sudo bash run.sh /rd2c/ 4G "" 9500 conf ` . Change the 3rd parameter to RC when running of docker in a unix cluster that uses slurm. The 4th parameter is the model number that will be used by gridlabd for the simulation.**

When refering to the 3G example, we are talking about topologies that just use a combination of point to point connections, CSMA connections and wifi connections. There is no 4G or 5G in these examples. 

Finally, when collecting data from recorders in gridlabd, with the IEEE 9500 bus model the files get fully populated at the end of the run. _currently under investigation_

## Labels used to describe experiments

### Execution environmnent

**RC**: This label is used to signal the run script that the slurm system will be used so the main run file needs to sleep instead of ending right after the start of the run. Use this label when you want to use the slurm system.

**_Empty string_**: This label is used to signal the run script that docker is used and therefore the main script can stop after the subscripts have started.

### Communication types labels

Non-cellular Networks:

**3G**: This label is used to describe directly connected networks. These networks are networks that make use of ethernet and fiber connections to connect individual nodes in a network to one another. We used the 3G label for this example to keep the labeling consistent with the other experiments.  

Cellular Networks:

**4G**: These are cellular networks that use LTE architecture to transfer data between the control center and the microgrids.

**5G**: This cellular network example leverages the New Radio core, developed by the LENA group, to simulate a simplified version of a non-standalone 5G network as described in research literature with minimal use of the LTE framework.

### IEEE 9500 bus model

The IEEE 9500 bus model simulates a medium-voltage distribution system. An example of such a system is a suburban neighborhood with a mix of houses, apartment buildings, and a small shopping center. 

Composition of a medium-voltage distribution system: 

The power for such a system originates at a high-voltage substation, likely several miles away. The voltage at this substation could be anywhere from 34.5 kV to 138 kV, depending on the local grid configuration. A large transformer located at the edge of the neighborhood steps down the high voltage to a medium voltage level, typically between 12.4 kV and 25 kV. Underground cables, insulated for the medium voltage level, run along main roads or easements throughout the neighborhood.  These cables are the "backbone" of the system, delivering power to various points. At regular intervals along these main feeders, there will be pad-mounted transformers. These transformers further step down the voltage to a low-voltage level (usually 480 V or 240 V) suitable for powering homes and businesses. From the pad-mounted transformers, overhead or underground lines (depending on local regulations and aesthetics) distribute the low voltage power to individual buildings. These lines connect to transformers on utility poles outside each building, which may further reduce the voltage to levels usable by appliances (typically 120 V).

### IEEE 123 bus model

The IEEE 123 bus model simulates a radial distribution system. An example of such a system is a power grid covering a rural town with a mix of farms, houses, and a small school.

Composition of a radial distribution system:

This is a simplified type of power grid where electricity flows from a single source (like a substation) outwards to various consumers like homes and businesses. Unlike a meshed network, there are no redundant paths for electricity to flow. While the specific voltage might vary depending on the model variation, it typically represents a medium-voltage distribution system. This means the voltage level is likely in the range of a few kilovolts (kV) to tens of kV. The IEEE 123 bus model is known for being unbalanced, meaning the loads on each of the three phases (conductors) can be unequal. This is a more realistic representation of real-world distribution systems where loads can vary depending on the types of consumers connected.

Similar to the IEEE 9500 bus model, the power originates at a substation several miles away, likely stepping down the voltage from a high-voltage transmission grid (around 34.5 kV to 138 kV) to a medium voltage level (around 12.4 kV to 25 kV). Once the power has been generated, it is sent through a single overhead power line, insulated for medium voltage, that runs from the substation towards the town, acting as the backbone of the system. At key points along the main feeder, transformers are located on utility poles. These transformers step down the voltage again to a low voltage level (typically 480 V or 240 V) suitable for powering homes and businesses. From these transformers, smaller overhead or underground lines branch out, reaching individual buildings. These lines connect to transformers on poles outside each building, further reducing the voltage to levels usable by appliances (typically 120 V).

IEEE 123 vs IEEE 9500 bus models:

Compared to the more complex IEEE 9500 bus model, the 123 bus system represents a smaller and less geographically expansive area. It's often used for studying specific aspects of distribution system behavior, such as voltage regulation, power losses, or protection schemes.

## Performance of out of the box examples

For the following 3 tables the poll request is sent every 15ms. The experiments were ran in docker on a computer running a 2.6 GHz 6-Core Intel Core i7 processor with 16 GB 2667 MHz DDR4 memory. 

The following time data was collected for a simulation of 60 simulated seconds and with the debug print statements active
| topology tested | IEEE model | Number of Nodes | Number of Paths | Attack? | Time (s) |
|---|---|---|---|---|---|
| 4G LTE | 9500 | 45 | 121 | No attacks | 34369.21 |
| 4G LTE | 9500 | 45 | 121 | DDoS with 2 attackers | 41737.71 |
| 4G LTE | 9500 | 45 | 121 | MIM with 2 attacker | 35085.39 |  
| 4G LTE | 123 | 17 | 16 | No attack | 4372.48 |
| 4G LTE | 123 | 17 | 16 | DDoS with 1 attackers | 14684.11 |
| 4G LTE | 123 | 17 | 16 | MIM with 3 attackers | 5796.20 |

The following data is the same amount of simulated seconds but with no print lines but data collection is still active
| topology tested | IEEE model | Number of Nodes | Number of Paths | Attack? | Time (s) |
|---|---|---|---|---|---|
| 5G | 9500 | 45 | 121 | no attack | 28885.06 | 
| 5G | 9500 | 45 | 121 | DDoS with 2 attacker | 85648.90 |
| 5G | 9500 | 45 | 121 | MIM with 2 attackers | 30580.91 | 
| 5G | 123 | 17 | 16 | no attack | 5824.01 |
| 5G | 123 | 17 | 16 | DDoS with 1 attacker | 17698.04 | 
| 5G | 123 | 17 | 16 | MIM with 3 attackers | 5351.68 |
| 4G LTE | 9500 | 45 | 121 | no attacker | 30048.81 | 
| 4G LTE | 9500 | 45 | 121 | DDoS with 2 attackers | 24697.42 |
| 4G LTE | 9500 | 45 | 121 | MIM with 2 attackers | 36660.07 |
| 4G LTE | 123 | 17 | 16 | no attack | 4868.21 |
| 4G LTE | 123 | 17 | 16 | DDoS with 1 attacker | 19900.36 |
| 4G LTE | 123 | 17 | 16 | MIM with 3 attackers | 2794.67 |
| (3G) Mesh | 9500 | 23 | 121 | no attack | 21423.28 |
| (3G) Mesh | 9500 | 23 | 121 | DDoS with 2 attacker | 15199.12 |
| (3G) Mesh | 9500 | 23 | 121 | MIM 2 attackers | 19013.93 |
| (3G) Mesh | 123 | 9 | 16 | no attack | 4399.30 |
| (3G) Mesh | 123 | 9 | 16 | DDoS with 1 attacker | 3064.72 |
| (3G) Mesh | 123 | 9 | 16 | MIM with 3 attackers | 3056.50 |
| (3G) Star | 9500 | 23 | 11 | no attack | 18629.31 |
| (3G) Star | 9500 | 23 | 11 | DDoS with 2 attackers | 15350.38 | 
| (3G) Star | 9500 | 23 | 11 | MIM with 2 attackers | 14262.83 | 
| (3G) Star | 123 | 9 | 4 | no attack | 3534.34 |
| (3G) Star | 123 | 9 | 4 | DDoS with 1 attacker | 5194.93 |
| (3G) Star | 123 | 9 | 4 | MIM with 3 attackers | 3629.42 | 

The following data is the same amount of simulated seconds but with no print statement or data collection
| topology tested | IEEE model | Number of Nodes | Number of Paths | Attack? | Time (s) |
|---|---|---|---|---|---|
| 5G | 9500 | 45 | 121 | no attack | 45732.18 |
| 5G | 9500 | 45 | 121 | DDoS with 2 attackers | 74192.72| 
| 5G | 123 | 17 | 16 | no attack | 5795.19 |
| 5G | 123 | 17 | 16 | DDoS with 1 attacker | 10591.63 |
| 5G | 123 | 17 | 16 | MIM with 3 attackers | 7997.78 |
| 4G LTE | 9500 | 45 | 121 | no attack | 26244.69 |
| 4G LTE | 9500 | 45 | 121 | DDoS with 2 attackers | 36607.44 |
| 4G LTE | 9500 | 45 | 121 | MIM with 2 attackers | 24107.69 | 
| 4G LTE | 123 | 17 | 16 | no attack | 4098.09 |
| 4G LTE | 123 | 17 | 16 | DDoS with 1 attacker | 8354.13 | 
| 4G LTE | 123 | 17 | 16 | MIM with 3 attackers | 2683.45 |
| (3G) Mesh | 9500 | 23 | 121 | no attack | 20765.89 |
| (3G) Mesh | 9500 | 23 | 121 | DDoS with 2 attackers | 16061.02 |
| (3G) Mesh | 9500 | 23 | 121 | MIM with 2 attackers | 22001.69 | 
| (3G) Mesh | 123 | 9 | 16 | no attack | 3416.46 |
| (3G) Mesh | 123 | 9 | 16 | DDoS with 1 attacker | 4091.37 |
| (3G) Mesh | 123 | 9 | 16 | MIM with 3 attackers | 3579.23 |  
| (3G) Star | 9500 | 23 | 11 | no attack | 26689.90 | 
| (3G) Star | 9500 | 23 | 11 | DDoS with 2 attackers | 26023.97 |
| (3G) Star | 9500 | 23 | 11 | MIM with 2 attacker | 22749.53 | 
| (3G) Star | 123 | 9 | 4 | no attack | 3338.92 |
| (3G) Star | 123 | 9 | 4 | DDoS with 1 attacker | 5982.22 |
| (3G) Star | 123 | 9 | 4 | MIM with 3 attacker | 3198.54 |  

The following 5G run was run with 2X higher bandwidth. We went from sending a packet every 15 ms to every 7ms

| topology tested | IEEE model | Number of Nodes | Number of Paths | Data collection? | Attack? | Time (s) |
|---|---|---|---|---|---|---|
| 5G | 123 | 17 | 16 | no attack | yes | 8460.03 |
| 5G | 123 | 17 | 16 | DDoS with 1 attacker | yes | 19895.78 |
| 5G | 123 | 17 | 16 | MIM with 3 attacker | yes | 5715.21 | 
| 5G | 123 | 17 | 16 | no attack | no |4321.34 |
| 5G | 123 | 17 | 16 | DDoS with 1 attacker | no | 8121.72 |
| 5G | 123 | 17 | 16 | MIM with 3 attackers | no | 7420.40 |

Running the 5G example with the IEEE 9500 bus model and debug statements activated on a shared computing system using slurm
| topology tested | IEEE model | Number of Nodes | Number of Paths | Attack? | Time (s) |
|---|---|---|---|---|---|
| 5G | 9500 | 45 | 121 | no attack | 6736.61 |
| 5G | 9500 | 45 | 121 | DDoS with 2 attackers | 8269.92 |
| 5G | 9500 | 45 | 121 | MIM with 2 attackers | 7376.86 |

Running the 5G example with the 9500 bus model and the debug statements turned off and the data collection still running on a shared computing system using slurm

## Out of the box examples

The following table is for both the 9500 and 123 bus models:

| Toplogy Type | Development Stage |
|---|---|
| 3G Star | Works for DDos and MIM
| 3G Mesh | Works for MIM and DDoS
| 3G Ring | Works for MIM and DDoS
| 4G Mesh and Star hybrid | Works for DDoS and MIM
| 5G Mesh and Star hybrid | Works for DDoS and MIM

NOTE: Mesh and Start hybrid means that there is a Mesh topology connecting the micrigrids to the 4G/5G network in a all to all connection and a Star topology connecting the control center to the 4G/5G network. 

Table containing the status for the MIM examples:

| Example | Description | Development Stage | 
|---|---|---|
| 3G 123 IEEE bus | connects the microgrids of the IEEE 123 bus model using directly connected network | Works
| 3G 9500 IEEE bus | connects the microgrids of the IEEE 9500 bus model using directly connected network | Works
| 4G 123 IEEE bus | connects the microgrids of the IEEE 123 bus model using 4G network | Works
| 4G 9500 IEEE bus | connects the microgrids of the IEEE 9500 bus model using 4G network | Works
| 5G 123 IEEE bus | connects the microgrids of the IEEE 123 bus model using 5G network | Works
| 5G 9500 IEEE bus | connects the microgrids of the IEEE 9500 bus model using 5G network | Works


Table containing the status for the DDoS examples:

| Example | Description | Development Stage | 
|---|---|---|
| 3G 123 IEEE bus | connects the microgrids of the IEEE 123 bus model using directly connected network | Works
| 3G 9500 IEEE bus | connects the microgrids of the IEEE 9500 bus model using directly connected network | Works
| 4G 123 IEEE bus | connects the microgrids of the IEEE 123 bus model using 4G network | Works
| 4G 9500 IEEE bus | connects the microgrids of the IEEE 9500 bus model using 4G network | Works
| 5G 123 IEEE bus | connects the microgrids of the IEEE 123 bus model using 5G network | Works
| 5G 9500 IEEE bus | connects the microgrids of the IEEE 9500 bus model using 5G network | Works

4G or point to point example using a star topology.

For 4G network:

When using the IEEE 9500 bus model, the topology contains 10 substation, 10 middle nodes, 10 user equipments (UE) connected to 10 4G relay antennas (EnB nodes), and one control center.

When using the IEEE 123 bus model, the topology contains 4 substation, 4 middle nodes, 4 user equipments (UE) connected to 4 4G relay antennas (EnB nodes), and one control center.

For base point to point connected star topology:

When using the IEEE 9500 bus model, the topology contains 10 substation, 10 middle nodes, and one control center.

When using the IEEE 123 bus model, the topology contains 4 substation, 4 middle nodes, and one control center.

DDoS enabled and running between 10 and 35 simulated seconds (simulated seconds refers to the time that ns3 tracks and not the wall time). This attack is trying to flood the link between the UE and the Middle node with several junk packets with the goal to slow down and increase packet loss. 

When refering to MIM nodes for DDoS attack scenario: These nodes are simple hops in the network between the substation and the UE nodes for 4G network, or the Control center for the base point to point connected network.

DDoS default parameters in grid.json inside /rd2c/integration/control/config/:
```
NumberOfBots: 4,

threadsPerAttacker: 4,

Active: 1 ( 1 means that the DDoS is active and 0 means that the DDoS is inactive)

Start: 10,

End: 35,

PacketSize: 1500,

Rate: "40480kb/s",

usePing: 1,

legitNodeUsedByBots: UE, (This is the node that the bots conducting the DDoS attack connect to)

endPoint: MIM (Refers to the end point of the attack. Usefull if you want to attack multiple links)
```

MIM --> Middle node between the UE and the substation/Control center

Using 4G as the communication protocol:

To run this example in docker and overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 4G "" 9500 conf `

To run this example in docker and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 4G "" 9500 noconf `

To run this example in docker with the IEEE 123 model and overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 4G "" 123 conf `

To run this example in docker with the IEEE 123 model and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 4G "" 123 noconf `

To run this example in a unix cluster using slurm and overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 4G RC 9500 conf `

To run this example in a unix cluster using slurm and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 4G RC 9500 noconf `

To run this example in a unix cluster using slurm with the IEEE 123 model and overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 4G RC 123 conf `

To run this example in a unix cluster using slurm with the IEEE 123 model and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 4G RC 123 noconf `

Using base point to point connected star topology:

To run this example in docker and overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 3G "" 9500 conf `

To run this example in docker and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 3G "" 9500 noconf `

To run this example in docker with the IEEE 123 model and overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 3G "" 123 conf `

To run this example in docker with the IEEE 123 model and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 3G "" 123 noconf `

To run this example in a unix cluster using slurm and overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 3G RC 9500 conf `

To run this example in a unix cluster using slurm and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 3G RC 9500 noconf `

To run this example in a unix cluster using slurm with the IEEE 123 model and overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 3G RC 123 conf `

To run this example in a unix cluster using slurm with the IEEE 123 model and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 3G RC 123 noconf `

Interesting outputted data:

1. TP.txt this file contains the performance of each path full path between the control center and the substation. To read this file only take the 20 last inputs per timesteps. if the number of substation changes, only read the last 2 X the number of substations rows per timesteps.

  - column IDs in the file: Timesteps,  path ID , ( sourceAddress / sourcePort --> destinationAddress / destinationPort ) , Throughput of path, lostPackets, Total received bytes since the start of the simulation , Total transmitted bytes since the start of the simulation, loss packet rate, delay per received packets, total transmitted packets since the start of the simulation ,total received packets since the start of the simulation, jitter per received packet

## DDoS applications that the attacker can use:

When setting usePing to 1, the attacker is sending numerous ping packets to the victim node using the ipv4 protocol. When using that option the attacker does not rely on a specific port being opened. In this case the attacker is conducting what is called a ping attack (https://www.researchgate.net/publication/222619629_PING_attack_-_How_bad_is_it)

When setting usePing to 0, the attacker is sending numerous packets filled with random data to the target node. The attacker in this case is more visible since the target victim has a port that is used by the attacker to receive the data. 

The threadsPerAttacker parameter in the DDoS section controls how many concurrent application the attacker starts to flood the target node. Ex: if the attack does a ping attack with 4 threadsPerAttacker, this means it will instantiate 4 concurrent ping applications pinging the same node. This parameter enables us to simulate different intensities of attacks. 

## How to stop the run?

If running using Docker, to stop the run run ` bash killall.sh ` to make sure most threads stop

  - Once script has finished runing run ` ps aux | grep root ` to check that no other thread related to ns3 is still running

  - If some threads have been found run ` kill -9 \<ID number\> ` to kill the remaining thread

## Is the code running?

Once the code has started there are several ways to track the progress:

1. run ` ps aux | grep root ` to see if you can see ns3, helics and gridlabd threads

2. run ` tail -\< Number of lines chosen \> output/ns3-helics-grid-dnp3-\< chosen example ID\>.log ` to see the most recent outputed lines from the run

3. run ` cat TP.txt ` to see the throughput monitoring file being populated

## Getting an updated version of the code

If you want to get an updated version of the NATIG repository run the following instructions (__Currently being tested__):
NOTE: Make sure that there are no un-commited changes in the PUSH/NATIG folder. If you do have some changes just run ` git stash `
NOTE2: before running ` git stash ` make sure that you have a copy of the config files that you changed somewhere else.
```
cd /rd2c/PUSH/NATIG/
# 5G disabled
./update_workstation.sh 4G
# 5G enabled
./update_workstation.sh 5G
```

## 5G configuration

To enable 5G capabilities:
1. request access to https://gitlab.com/cttc-lena/nr
2. run ``` ./build_ns3.sh 5G /<root folder that the simulation is run on>/ ``` from the NATIG folder in the PUSH folder
  - in the case of Docker run ``` ./build_ns3.sh 5G /rd2c/ ```

## 5G out of the box example

5G example using a star topology.

When using the IEEE 9500 bus model, the topology contains 10 substation, 10 middle nodes, 10 user equipments (UE) connected to 10 5G relay antennas (GnB nodes), and one control center.
When using the IEEE 123 bus model, the topology contains 4 substation, 4 middle nodes, 4 user equipments (UE) connected to 4 5G relay antennas (GnB nodes), and one control center.

DDoS enabled and running between 10 and 35 simulated seconds (simulated seconds refers to the time that ns3 tracks and not the wall time). This attack is trying to flood the link between the UE and the Middle node with several junk packets with the goal to slow down and increase packet loss. 

DDoS default parameters in grid.json inside /rd2c/integration/control/config/:
```
NumberOfBots: 4,

threadsPerAttacker: 4,

Active: 1 ( 1 means that the DDoS is active and 0 means that the DDoS is inactive)

Start: 10,

End: 35,

PacketSize: 1500,

Rate: "40480kb/s",

usePing: 1,

legitNodeUsedByBots: UE, (This is the node that the bots conducting the DDoS attack connect to)

endPoint: MIM (Refers to the end point of the attack. Usefull if you want to attack multiple links)
```
MIM --> Middle node between the UE and the substation

To run this example in docker and overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 5G "" 9500 conf `

To run this example in docker and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 5G "" 9500 noconf `

To run this example in docker with the IEEE 123 model and overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 5G "" 123 conf `

To run this example in docker with the IEEE 123 model and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sudo bash run.sh /rd2c/ 5G "" 123 noconf `

To run this example in a unix cluster using slurm and overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 5G RC 9500 conf `

To run this example in a unix cluster using slurm and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 5G RC 9500 noconf `

To run this example in a unix cluster using slurm with the IEEE 123 model and overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 5G RC 123 conf `

To run this example in a unix cluster using slurm with the IEEE 123 model and __NOT__ overwrite the config files using the ones in the NATIG repo: ` sbatch run.sh /rd2c/ 5G RC 123 noconf `

Interesting outputted data:

1. TP.txt this file contains the performance of each path full path between the control center and the substation. To read this file only take the 20 last inputs per timesteps. if the number of substation changes, only read the last 2 X the number of substations rows per timesteps.

  - column IDs in the file: Timesteps,  path ID , ( sourceAddress / sourcePort --> destinationAddress / destinationPort ) , Throughput of path, lostPackets, Total received bytes since the start of the simulation , Total transmitted bytes since the start of the simulation, loss packet rate, delay per received packets, total transmitted packets since the start of the simulation ,total received packets since the start of the simulation, jitter per received packet

## Example output data

NS3 example output data:

Code generates a file containing the performance of paths between the Substations and the Control center

Location: File called TP.txt is located in integration/control/ folder

Column IDs: Timesteps,  path ID , ( sourceAddress / sourcePort --> destinationAddress / destinationPort ) , Throughput of path, lostPackets, Total received bytes since the start of the simulation , Total transmitted bytes since the start of the simulation, loss packet rate, delay per received packets, total transmitted packets since the start of the simulation ,total received packets since the start of the simulation, jitter per received packet

Example output for DDoS out of the box example on 5G network:

```
1.05 20000 (UDP 1.0.0.2 / 20000 --> 172.17.0.3 / 20000) 62.4727 0 343 343 0 0.0027614 7 7 0.00143351
1.05 40000 (UDP 172.107.0.3 / 20000 --> 1.0.0.2 / 40000) 2409.84 0 9918 21028 0.528571 0.00702642 70 33 0.000376239
1.05 40001 (UDP 172.108.0.3 / 20000 --> 1.0.0.2 / 40001) 3654.13 0 15578 25676 0.395604 0.00445615 91 55 0.000375062
1.05 40002 (UDP 172.109.0.3 / 20000 --> 1.0.0.2 / 40002) 5027.92 0 24951 33663 0.258929 0.00431755 112 83 0.000253965
1.05 40003 (UDP 172.110.0.3 / 20000 --> 1.0.0.2 / 40003) 5094.66 0 21982 36883 0.404762 0.00457615 126 75 0.000253877
1.05 40004 (UDP 172.111.0.3 / 20000 --> 1.0.0.2 / 40004) 5634.63 0 26154 44226 0.408163 0.00504666 147 87 0.000363581
1.05 40005 (UDP 172.112.0.3 / 20000 --> 1.0.0.2 / 40005) 4569.61 0 16290 53844 0.697802 0.00578028 182 55 0.00045446
1.05 40006 (UDP 172.113.0.3 / 20000 --> 1.0.0.2 / 40006) 12935.8 0 50112 59521 0.157635 0.00483181 203 171 0.000196482
1.05 40007 (UDP 172.114.0.3 / 20000 --> 1.0.0.2 / 40007) 7145.32 0 31503 71393 0.558824 0.00595131 238 105 0.000302485
1.05 40008 (UDP 172.115.0.3 / 20000 --> 1.0.0.2 / 40008) 10829.2 0 49198 84511 0.417857 0.00569313 280 163 0.000209076
1.05 40009 (UDP 172.116.0.3 / 20000 --> 1.0.0.2 / 40009) 13533.5 0 58298 100436 0.419643 0.00586743 336 195 0.000153357
1.05 49153 (UDP 1.0.0.2 / 49153 --> 172.116.0.3 / 20000) 62.474 0 343 343 0 0.00247497 7 7 0.00129082
1.05 49154 (UDP 1.0.0.2 / 49154 --> 172.105.0.3 / 20000) 62.4741 0 343 343 0 0.00190355 7 7 0.00100508
1.05 49155 (UDP 1.0.0.2 / 49155 --> 172.83.0.3 / 20000) 62.4741 0 343 343 0 0.00333212 7 7 0.00171937
1.05 49156 (UDP 1.0.0.2 / 49156 --> 172.61.0.3 / 20000) 62.4741 0 343 343 0 0.00161216 7 7 0.000719382
1.05 49157 (UDP 1.0.0.2 / 49157 --> 172.50.0.3 / 20000) 62.4741 0 343 343 0 0.00190357 7 7 0.00100508
1.05 49158 (UDP 1.0.0.2 / 49158 --> 172.28.0.3 / 20000) 62.4741 0 343 343 0 0.00190355 7 7 0.0010051
1.05 49159 (UDP 1.0.0.2 / 49159 --> 172.72.0.3 / 20000) 62.4741 0 343 343 0 0.00390983 7 7 0.00200508
1.05 49160 (UDP 1.0.0.2 / 49160 --> 172.39.0.3 / 20000) 62.4741 0 343 343 0 0.0011836 7 7 0.000290798
1.05 49161 (UDP 1.0.0.2 / 49161 --> 172.94.0.3 / 20000) 62.4741 0 343 343 0 0.00218926 7 7 0.00114794
1.1 20000 (UDP 1.0.0.2 / 20000 --> 172.17.0.3 / 20000) 58.3214 0 686 686 0 0.0018276 14 14 0.000716774
1.1 40000 (UDP 172.107.0.3 / 20000 --> 1.0.0.2 / 40000) 2944.77 0 31550 42056 0.25 0.00614299 140 105 0.000432111
...
...
1.1 49156 (UDP 1.0.0.2 / 49156 --> 172.61.0.3 / 20000) 58.322 0 686 686 0 0.00125248 14 14 0.000359708
1.1 49157 (UDP 1.0.0.2 / 49157 --> 172.50.0.3 / 20000) 58.322 0 686 686 0 0.00139818 14 14 0.00050255
1.1 49158 (UDP 1.0.0.2 / 49158 --> 172.28.0.3 / 20000) 58.322 0 686 686 0 0.00139817 14 14 0.000502563
1.1 49159 (UDP 1.0.0.2 / 49159 --> 172.72.0.3 / 20000) 58.322 0 686 686 0 0.00240132 14 14 0.00100255
1.1 49160 (UDP 1.0.0.2 / 49160 --> 172.39.0.3 / 20000) 58.322 0 686 686 0 0.0010382 14 14 0.00014542
1.1 49161 (UDP 1.0.0.2 / 49161 --> 172.94.0.3 / 20000) 58.3221 0 686 686 0 0.00154102 14 14 0.000573984
```


Gridlabd example output data:

TBD

## How to run it on a unix cluster

Step 1: move the RC folder outside of the repository folder. 

Step 2: go to the RC folder and run ./make.sh

Step 3: WIP note make sure that all hard coded links are changed to the location of your code. _Will be fixed soon_ 

Step 4: run the make.sh bash script located in the ns-3-dev folder with the location of the RC folder as an input: ` ./make.sh _location of RC folder_ `

If your system has slurm nodes:

Command to run the simulation: ` sbatch --exclusive run.sh _location of RC folder_ _[3G|4G|5G]_ _[RC|emptyString]_ _[9500|123]_ _[conf|noconf]_ `

If your system does not have slurm nodes:

Command to run the simulation: ` ./run.sh _location of RC folder_  _[3G|4G|5G]_ _[RC|emptyString]_ _[9500|123]_ _[conf|noconf]_ `

## Available configurations
Using the configuration files, a user can create a ring topology that uses wifi as a connection type, for example. Currently with the exception of 5G and 4G a user can mix and match any connection types with any topologies.

Existing error: When using a mesh topology with wifi, we do run into the ressource issue with the docker container. If a user want to run such a topology please limit the number of connections per nodes at 2 to 3 connections per nodes. *currently under investigation*

### Topologies (Only with 3G example)
1. Ring
2. Mesh (partial when using wifi)
3. Star

### Connection types:
1. point to point connections (p2p)
2. CSMA 
3. Wifi (only on 3G example)
5. 4G
6. 5G

### IEEE models (working)
1. 123-node bus model
2. 9500-node bus model (Current it is called using ieee8500.glm, but it is the ieee9500 model)

### Additional tools
To automatically generate the input json files use the get\_config.py from the graph folder. If you want to change the glm file that is used either replace the content of ieee8500.glm with the content of your glm file or replace in the python from the name of the glm file that is passed in by the name of the glm file that you want to use

#### How to generate config json file
1. go to graph folder
2. run ``` python get_config.py <Number of Microgrids> ```
3. the number of Microgrids is the number of Microgrids you want your model to have. Currently only tested even number of Microgrids. 

## Configuration files

1. Location: integration/control/config
2. Available config files:
   - **topology.json**: Used to control the topology of the ns3 network
   - **grid.json**: Used to relate nodes of glm files to ns3 nodes. This file is also used to control the attack parameters.
   - **ns_config.json**: Used as configuration file for Helics

### topology.json
1. Channel parameter: settings for the communication parameter for point to point, csma and wifi networks
2. Gridlayout parameter: settings for 2D layout for wifi networks. This will be enabled in the future for other types of networks. 
3. 5GSetup parameter: settings used in both 5G and 4G networks.
4. Node parameter: settings for each node of the network. The configuration file contains default values for each of the nodes. 

### grid.json
1. Microgrids: connects the gridlabd components from glm with the NS3 nodes. Ex: mg1 is 1 NS3 node.
2. MIM: parameter settings for the man-in-the-middle attacks (injection and parameter changes) 
3. DDoS: parameters for the DDoS attacker (ex: number of bots)
4. Simulation: general parameters for the simulation (ex: the start and end time of the simulation)

### gridlabd\_config.json
1. Helics parameters: Helics broker setup parameters (ex: IP address and port number for helics setup)
2. Endpoint: Gridlabd endpoint

NOTE: to change the helics broker's port the gridlabd\_config.json needs to be updated. All 3 of the files have a reference to the port used by the helics broker. 

## Simulated attacks

### Currently working
1. Injection attacks (Man-in-the-middle): The attacker trips relays connecting the microgrids to one another and to the grid causing islanding of the microgrid 
2. Parameter changes (Man-in-the-middle): The attacker modifies the Pref and Qref values of two inverters in Microgrid 1 while the microgrids are islanded
3. DDoS: The attacker generates a number of bots that will flood 1 to n nodes in the network to slow down the network performance

### In progress
1. MPI capabilities for the code
2. Examples for Man-In-The-Middle attacks


### Reference
```
@article{bel2023co,
  title={Co-Simulation Framework For Network Attack Generation and Monitoring},
  author={Bel, Oceane and Kim, Joonseok and Hofer, William J and Maharjan, Manisha and Purohit, Sumit and Niddodi, Shwetha},
  journal={arXiv preprint arXiv:2307.09633},
  year={2023}
}
```
