# Distributed Stream Processing on Edge-Cloud

We used [our fork of Apache Storm](https://github.com/Sefik-Palazoglu/storm) distributed stream processing engine to allow us
to enforce placements of bolts to physical hardware.

[Core Network Emulator](http://coreemu.github.io/core/) was used to easily create cloud-edge computing with easily 
configurable scenarios.

We employed [Riot Bench](https://github.com/dream-lab/riot-bench) for running actual benchmarked topologies in our experiments.
[ETL Taxi](https://github.com/dream-lab/riot-bench/blob/master/modules/storm/src/main/java/in/dream_lab/bm/stream_iot/storm/topo/apps/ETLTopology.java) Topology was used particularly.

Our experiments were run by changing cloud-to-source latency value. Experiments were run with 5ms, 40ms, 80ms values.
You can see the setup files in [xmls](https://github.com/DSPoEC/Cmpe492/tree/main/xmls) file.

Here is the ETLTopology DSP Application diagram. This is implemented by folks at [Riot Bench](https://github.com/dream-lab/riot-bench)

![ETL TAXI Topology](https://github.com/DSPoEC/Cmpe492/blob/main/Topology/Research%20and%20Design%20-%20Page%201%20-%20v2.png)

The top stack is sent to only edge hardware and bottom stack is sent only to the cloud harware.
This achieved by our fork of [Apache Storm](https://github.com/Sefik-Palazoglu/storm) which enables "Placement Enforcement".

"Placement Enforcement" is when you force a class of Apache Storm bolts (logical executor objects) to a class of hardware.
"Placement Enforcement" is not possible in default Storm.

Here is how experiment setups look like in Core:

40ms: ![alt text](https://github.com/DSPoEC/Cmpe492/blob/main/xmls/40ms.png)
