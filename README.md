**N**etwork **A**ttack **T**estbed **I**n [Power] **G
r**rid (**NATI[P]G**), a co-simulation environment for distribution power grid network using state-of-the-art simulators.

It is a standalone, containerized, and reusable environment to enable cyber analysts and researchers to run different cyber security and performance scenarios on powergrid.

## How to get started
1. Have docker installed
2. clone the NATIG repo
3. Make sure you have access to the NS3 stash repo at: https://stash.pnnl.gov/projects/HAGEN/repos/ns-3-dev/browse
4. run the following commands:
   - cd NATIG
   - ./buildimage.sh 
   - Once the docker finishes building successfully, run: ./rundocker.sh
NOTE: The default docker container does not come with 5G enabled
   

## Available configurations

NOTE: using the configuration files, a user can create a ring topology that uses wifi as a connection type, for example. Currently with th eexception of 5G and 4G a user can mix and match any connection types with any topologies.

Existing error: When using a mesh topology with wifi, we do run into the ressource issue with the docker container. If a user want to run such a topology please limit the number of connections per nodes at 2 to 3 connections per nodes. *currently under investigation*

### Topologies
1. Ring
2. Mesh
3. Star
4. 5G
#### Future topologies
1. 4G

### Connection types:
1. point to point connections (p2p)
2. CSMA
3. Wifi

## Configuration files

1. Location: integration/control/config
2. Available config files:
   - **topology.json**: Used to control the topology of the ns3 network
   - **grid.json**: Used to relate nodes of glm files to ns3 nodes. This file is also used to control the attack parameters.
   - **ns_config.json**: Used as configuration file for Helics

## Simulated attacks

### Currently working
1. Injection attacks: The attacker trips relays connecting the microgrids to one another and to the grid causing islanding of the microgrid 
2. Man-in-the-middle: The attacker modifies the Pref and Qref values of two inverters in Microgrid 1 while the microgrids are islanded

### Comming soon
1. Insider attack
2. DDoS attack
3. 5G attack vectors, example attack at the SDN level, attacks on slice configurations, etc.
