# Distributed Stream Processing on Edge-Cloud

We used [our fork of Apache Storm](https://github.com/Sefik-Palazoglu/storm) distributed stream processing engine to allow us to enforce placements of bolts to physical hardware.
[Core Network Emulator](http://coreemu.github.io/core/) was used to easily create cloud-edge computing with easily 
configurable scenarios.
We employed [Riot Bench](https://github.com/dream-lab/riot-bench) for running actual benchmarked topologies in our experiments.
[ETL Taxi](https://github.com/dream-lab/riot-bench/blob/master/modules/storm/src/main/java/in/dream_lab/bm/stream_iot/storm/topo/apps/ETLTopology.java) Topology was used particularly.


Here is the ETLTopology DSP Application diagram. This is implemented by folks at [Riot Bench](https://github.com/dream-lab/riot-bench)

![ETL TAXI Topology](https://github.com/DSPoEC/Cmpe492/blob/main/Topology/Research%20and%20Design%20-%20Page%201%20-%20v2.png)

The top stack is sent to only edge hardware and bottom stack is sent only to the cloud harware. This achieved by our fork of [Apache Storm](https://github.com/Sefik-Palazoglu/storm) which enables "Placement Enforcement".

## Placement Enforcement
"Placement Enforcement" is when you force a class of Apache Storm bolts (logical executor objects) to a class of hardware.
"Placement Enforcement" is not possible in default Storm.

## Core Setups

Our experiments were run by changing cloud-to-source latency value. Experiments were run with 5ms, 40ms, 80ms values. You can see the setup files in [xmls](https://github.com/DSPoEC/Cmpe492/tree/main/xmls) file.

Here is how experiment setups look like in Core:

![40ms](https://github.com/DSPoEC/Cmpe492/blob/main/xmls/40ms.png)

We have bandwidth, latency, and even loss/duplication configurations in Core Network Emulator. This is why we chose this emulator to be our testbed in our DSP applications which are run on edge-cloud hardware.

## Test Setups
We run our experiments with 5, 40, and 80ms and with edge-cloud placement and only-cloud placement.

## Results
Green graph is edge-Cloud, Blue graph is cloud-only

5ms_Cloud-only vs. Edge-Cloud:  
![5ms_Cloud-only vs. Edge-Cloud](https://github.com/DSPoEC/Cmpe492/blob/main/Graphs/5ms-comparison.png)

40ms_Cloud-only vs. Edge-Cloud:  
![40ms_Cloud-only vs. Edge-Cloud](https://github.com/DSPoEC/Cmpe492/blob/main/Graphs/40ms-comparison.png)

80ms_Cloud-only vs. Edge-Cloud:  
![80ms_Cloud-only vs. Edge-Cloud](https://github.com/DSPoEC/Cmpe492/blob/main/Graphs/80ms-comparison.png)

# How to run your own Experiments
You first need to get our fork of Apache Storm. Compiling from source code is described in [DEVELOPER.md](https://github.com/apache/storm/blob/master/DEVELOPER.md) of Apache Storm. When building, just keep in mind how you run the distribution maven command. You may need to run it like this:  
`mvn package -DskipTests=true -Dgpg.skip=true`

Then you need to set up a core session with the provided session files in xmls folder.
After this, It is as straightforward as running your Storm daemons. Then you submit your topology as described in the `Resources/How to Build and Submit Topologies In Apache Storm.pdf` document.

## Caveats
### File system conflicts
We actually run these daemons in the same machine, so we need to seperate their dedicated files where they write log files and state etc. This is done by two different .yaml files provided in src/ folder.
Run an edge supervisor like this: `storm supervisor --config /path/to/edgeSupervisor.yaml`

### Cpu limiting
We used linux's own cpu limiting tool to limin edge and cloud nodes's cpu power.
Limit edge node's cpu core limit to 1 and cpulimit to 8%: `cpulimit --pid=34 --cpu=1 --limit=8`
This is assuming edgeSupervisor node is at pid=34.
