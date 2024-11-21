---
layout: post
title: Routing Protocol Attacks in IoT Networks
date: 2022-12-02 10:38:00 +0000

---

## Chapter 1: Testing Decreased Rank Attacks on IoT Networks

 


## Introduction

 

In this report a simulated IoT network will be analysed under 5 different scenarios. In order to determine the effect of a “Decreased Rank Attack” on the Power Consumption and PDR of the network. These results will then be used to conclude the effectiveness of this type of attack on a network of this type. The simulations will be carried out in the Cooja simulator, available in InstantContiki operating system.

Decreased Rank Attack – In an RPL network an attacker may advertise a false rank to attract nearby nodes, nodes within connection range with the same or higher rank than the attacker are classified as potential children as in [1]. These potential children then attempt to connect through the attacker to the root node in an attempt to follow the lowest metric path which is defined by the objective function used by RPL even if it may not be the true optimal route. With nodes routing through the attacker, a black-hole, grey-hole, or information siphoning attack may be carried out potentially disturbing the packet delivery ratio of a network. 

 


## Simulation parameters

 

Default Cooja parameters are used in this report unless stated otherwise. Each of the simulations will be carried out with the specifications seen in Table 1

 
|Parameter|Value|
|--|--|
|Number of Nodes|21 (1 Sink included)|
|Simulation Time|10 minutes|
|Transmission Range|30 meters|
|Interference Range|35 meters|
|Data Rate|1 packet per minute|
|Attack Delay|After 3 DIO messages|




*Table 1: General Simulation Specifications*

 

The IoT network will consist of 1 sink node and 20 remote nodes in differing topologies, linked to each simulated scenario as described in Table 2.

 
|Scenario No.|Attacker Node Rank|Attacker Node Mobility|Attacker Node Location| 
|--|--|--|--|
|Scenario 1|No Attack|None|Natural Position (Figure 1)|
|Scenario 2|0|None|Attack Position (Figure 1)|
|Scenario 3|257|None|Attack Position (Figure 1)|
|Scenario 4|257|None|Attack Position (Figure 2)|
|Scenario 5|257|Yes|--|

*Table 2: Simulation Scenario Specifications*


RPL is used as the routing protocol by the network with MRHOF set as the objective function for deciding the routes to be utilised. MRHOF allows for metric-optimised routes while also utilising hysteresis to prevent neighbours with similar metrics from causing churn in the network. The following 3 figures represent the attacker node locations specified in Table 2.
 

*Figure 1: Healthy IoT Network Topology with no attacker node*

![Figure 1](./image1.png "Figure 1: Healthy IoT Network Topology with no attacker node")

*Figure 2: Infiltrated IoT Network Topology with Attacker Node 21 at Position 1*

![Figure 2](./image2.png "Figure 2: Infiltrated IoT Network Topology with Attacker Node 21 at Position 1")

*Figure 3: Infiltrated IoT Network Topology with Attacker Node 21 at Position 2*

![Figure 3](./image3.png "Figure 3: Infiltrated IoT Network Topology with Attacker Node 21 at Position 2")

## Implementation of the attack

 

A Reduced Rank Attack is a type of Denial-Of-Service attack utilising a modified (lowered) rank value in DIO messages sent by a node which is used by RPL to calculate the best routes for nodes to communicate with the sink and parent nodes. An attacker broadcasting a lower rank than true will cause other nodes to prioritise routing through itself, rather than along the real best route.

 

The Reduced Rank Attack was carried out on the simulated network for this report by inserting some logic into the nodes routing programming. This logic contains a flag which is used for delaying the start of the attack and a logical comparison to ensure only the behaviour of the attacker node is affected.

 

If both of the conditions are met the rank value is manually set to the value specified in the Scenario Specification, which would otherwise be assigned by the routing protocol as seen in Figure 4

 


*Figure 4: Reduced Rank Attack Logic*

 ![Figure 4](./image4.png "Figure 4: Reduced Rank Attack Logic")

## Power Consumption Calculations

 

The power consumption of each of the nodes was calculated locally on each of said nodes using a function added to their programming. This was utilised to allow for accurate measurements of the power consumption even if communication to the sink node is not possible. The function carries out some fundamental calculations utilising predefined values for the power consumption of each part of the node, per unit of time. These values are shown in Figure 5.


*Figure 5: Predefined Power Consumption of IoT node functions*

![Figure 5](./image5.png "Figure 5: Predefined Power Consumption of IoT node functions")

These predefined values are then multiplied by the time each function is active in the node, to provide a total energy usage per function. To calculate the total energy consumption of the node these different function powers are combined as seen in Figure 6, printed to the user in (mJ) alongside an average power usage in (mW) and timestamp for the calculation as seen in Figure 7.

*Figure 6: Power Consumption Calculations*

![Figure 6](./image6.png Figure 6: Power Consumption Calculations")

*Figure 7: Power Calculation function output*

![Figure 7](./image7.png "Figure 7: Power Calculation function output")

In order to compare the Energy usage of each of the nodes, final average power usage readout was multiplied by the full 600 seconds simulated to estimate the total energy usage of each node. This estimated total power was compared against the presented Energy usage value on every calculation to provide a method of checking the rough accuracy of these results.

 

 


## PDR Calculations

 

The Packet Delivery Ratio of the network was calculated using the “CollectView” tool in the Cooja simulator, which allows metrics to be gathered by the sink node about the nodes on the network. One of the metrics provided by this tool is a count of how many packets were received by the sink. Then using the packet rate and the duration of the simulation, the expected received packet count can be calculated.

 

As the simulation runs for 10 minutes, and the nodes take 1 minute to start broadcasting, 9 packets should be received by each node. As there are 20 transmitting nodes in the network, the sink should receive 180 packets if the PDR is 1

 

The calculation used for PDR was:


*PDR = (Number of Packets Received)/(Number of Packets Sent)*

## Results

### Scenario 1: No Attack


Scenario 1 is run as a baseline, providing a reference for the power consumption and the packet delivery ratio of this type of IoT network containing this number of nodes, routing protocol and packet rate. In this scenario, the average Energy consumption across all nodes at 600 seconds elapsed was 912.81mJ and had a PDR of 1.0. As seen in Figure 9 every node has a secure and solid route to the sink.

*Figure 8: Scenario 1 Final Routing Graph*

![Figure 8](./image8.png "Figure 8: Scenario 1 Final Routing Graph")

The two nodes with the highest consumption in this scenario, were Node 11 with an estimated usage of 1530.0mJ, and Node 7 with 1420.8mJ. Node 3 used the least amount of energy, using 688.2mJ average. The standard deviation of results is 220.54mJ. These values show that there is a very close correlation between the energy usage of a node and both the distance to the sink as well as the number of nodes selecting it as its parent as presented in Figure 9.


*Figure 9: Scenario 1 Energy Usage Heatmap*

![Figure 9](./image9.png "Figure 9: Scenario 1 Energy Usage Heatmap")

### Scenario 2: Position 1, Rank 0

 

As part of Scenario 2, the attacker's rank is set to 0, which seems to disrupt the DODAG construction of the network slightly, setting the Parent of the nodes who request routing information from the attacker node to “NULL”. It would appear this is the result of the requesting node removing Node 21 as its parent after another DIS message is received. Interestingly, neither the PDR nor Power consumption is affected considerably and is within the margin of error/change caused by the relocation of the attacker node. The average energy consumption across all nodes was 906.09mJ and the network had a PDR of 1.0.
 

*Figure 10: Scenario 2 Final Routing Graph*

![Figure 10](./image10.png "Figure 10: Scenario 2 Final Routing Graph")

The two most energy-consuming nodes were Node 12, with 1308.6mJ, and Node 10, with 1148.4mJ. Like before, Node 3 has the lowest usage with 703.8mJ. The standard deviation for these results was 198.87mJ. The nodes closest to the attacking node seem to have elevated power consumption, likely because of the resetting of the stored routing information, Node 12 also being a key node for the routes of lower branches in the network (See Figure 10).

 

 

 

 

 

 

 

 

 

 

 


### Scenario 3: Position 1, Rank 257

 

Next, Scenario 3 is simulated. In this configuration of the network, the sink defaults to a rank of 256. Setting the rank to 257 makes the Attacker node appear as close to the Sink as possible. This change will cause nodes to choose the Attacker node as their parent, even if there is an alternative valid route to the sink available. The average energy usage in this scenario was 945.3mJ which is slightly higher than the previous two results, most of the increase in energy usage comes from nodes connected to the attacker node, which may be a result of the nodes switching route to the attacker node. The PDR of the network in this scenario was 1.0. Even after nodes switched to using Node 21 as their parent, there is still a link to the sink, meaning packets are not lost, just taking a nonideal route as shown in Figure 11.

 

 

*Figure 11: Scenario 3 Final Routing Graph*

![Figure 11](./image11.png "Figure 11: Scenario 3 Final Routing Graph") 

The two highest energy uses recorded in this scenario were from Node 21, with 1473.6mJ, and Node 12, with 1255.2mJ, where the standard deviation was 186.60mJ. As with the previous scenario, the nodes connecting to the attacker have elevated power consumption compared to Scenario 1 (See Figure 12).


*Figure 12: Scenario 3 Energy Usage Heatmap*

![Figure 12](./image12.png "Figure 12: Scenario 3 Energy Usage Heatmap") 

 

 

 

 

 


### Scenario 4: Position 2, Rank 257

 

As in the previous scenario, the rank of the attacker is set to 257, as close to the sink's rank as possible. In Scenario 4 as there are no nodes with a lower rank between the attacker and the sink, AND there is no route between the attacker and the sink. The attacking node exhibits blackhole attack behaviour, forcing a significant increase in energy consumption and reduction in the PDR of the network. The average energy consumption for this network was 2725.32mJ and had a PDR of 0.467. As seen in Figure 13, there are nodes that never registered with the sink and two isolated nodes, this causes a significant decrease in the PDR as there is no route for the packets to take to reach the sink. The power consumption of nodes connected to the sink (Nodes 2 - 5) have a fairly nominal usage relative to Scenario 1, However, every other node in the network has dramatically higher reported usage. This is likely because of the retransmission required with the blackhole attack launched by Node 21, as seen in Figure 4, the power required to operate the wireless radio is significant, and is likely the cause of the increased power usage.

*Figure 13: Scenario 4 Final Routing Graph*



The two highest nodes in terms of energy consumption in this scenario were Node 21 with 6891.0mJ and Node 15 with 4658.4mJ and the standard deviation was 1676.45mJ. Node 21 likely has increased energy usage from the increased radio usage caused by nodes trying to send packets to the sink through it with no route available. Node 15 was likely also using a significant amount of radio trying to rebroadcast/find a valid route. Node 2 had the lowest usage of 703.2mJ due to the simplicity of the route and proximity to the sink.

 

 

 

 

 

 

 


### Scenario 5: Mobile, Rank 257

 


####  Mobility Methodology

 

For Scenario 5 mobility is introduced for the attacker node, which will allow it to have an effect on a wider range of nodes throughout the network. In order to make use of Mobility in the simulator, the mobility plugin for Cooja was downloaded and extracted into:

	contiki/tools/cooja/apps/

The plugin jar was built using ant build tool inside the mobility plugin folder:

	ant jar

To enable the plugin in Cooja, open The “Settings” drop-down in the simulator and open the “External tools paths” settings menu, as shown in Figure 13:

*Figure 13: External tools paths option*

 

The plugin is then added to the DEFAULT_PROJECTDIRS field to link the plugin to the Cooja simulator like so:

	[CONTIKI_DIR]/tools/cooja/apps/mobility

 

After restarting the Simulator, the plugin can now be activated for the session using the “Cooja Extensions” option under the “Settings” drop-down.

 

The Mobility option will now appear under the “Tools” drop-down, opening this option will ask you to provide a Positions.dat file.

 

The Positions.dat file is used to define the coordinates for the nodes specified to follow. In this report, the coordinates used can be seen in Figure 14.

*Figure 14: Attacking Node Mobility Positions File*
 

Once the positions.dat file has been loaded, the node will be relocated in the topology at the times specified, simulating the effect of mobility.

 


####  Scenario 5 Results

 

Scenario 5 presented a fairly effective scenario for deploying a Decreased Rank Attack with an average energy consumption for the nodes on the network of 1561.23mJ and the PDR is 0.83. The coordinates defined above cover multiple areas over and around the static topology of the network as represented by the red dots in Figure 15. The increased Energy consumption is likely caused by new routes having to be discovered as the attacking node moves out of range. The PDR drop may be caused by packets which have been sent having their route broken by the mobility during transmission.


*Figure 15: Visualisation of Mobility Positions of Attacker Node*

 

Two highest recorded energy usages for nodes were, Node 21 with usage of 2705.4mJ 4 and Node 11 with 2104.2mJ, the lowest recorded usage was Node 5 with 765mJ. The Standard Deviation of all usages is 459.4mJ, which shows a relatively uniform increase in power usage over that of a network with no attacks taking place. Node 5’s result is very similar to the power level of the nodes in a non-attacked network, which likely means the chosen coordinates do not cause Node 5 to be affected by the attacking node at all. As shown by Figure 16 this is also a scenario where the Decreased Rank Attack is effective in reducing the efficiency and reliability of an IoT network.



*Figure 16: Comparison of Energy Consumptions across tested Scenarios*



## Conclusion

 

Decreased rank attacks are not effective at decreasing PDR or increasing power consumption if the attacker node is broadcasting a rank lower than that of the sink’s or is close in proximity to the sink node as demonstrated by the results of Scenario 2 and 3 simulations. However, they are extremely detrimental to both power consumption and PDR if placed further away from the sink as demonstrated by Scenario 4. Mobility is another way of increasing the effectiveness of this type of attack, moderately decreasing the PDR of the network and increasing the power consumption, with the benefit of affecting more nodes in the network and reducing the power requirements of the attacking node as seen in the results of Scenario 5 simulations.

 


## Evaluation

 

Overall, a very effective, informative set of experiments, however, there is room to improve the experimentation, for example, the slight inaccuracy of the energy usage values at 600 simulation seconds elapsed. This could possibly be improved with a more complex power calculation function, that takes into account the remaining idle time when calculating the final usage reading. Mobility could happen more frequently to get a better idea of how a “continually” moving attacker would affect the network.

 

 

 


# Chapter 2: Decreased Rank Attack Mitigation Techniques

 

Mitigation Techniques Analysis: In this chapter, you are required to produce a critical analysis of THREE security mitigation techniques to ONE of the IoT attacks from the following list: Version Number Attack, Decreased Rank Attack, or DIS Attack. In addition to describing the particulars of the mitigation techniques, the chapter should also produce a table highlighting the pros and cons of each technique and final recommendation of the most suitable approach based on your analysis.

 


## Introduction

 

In this chapter of the report, mitigation techniques that can be used against Decreased Rank Attack will be explored and compared. With an overall recommendation on which of the studied methods are suitable for reducing the attack impact seen in Chapter 1

 


## Related Research and Definitions

 

6LoWPAN - IPv6 over Low Power Wireless Personal Area Networks protocol. Due to the restricted resources of IoT nodes a full IPv6 stack is not suitable for deployment in this type of IoT network [5]. To allow for an IoT network such as this to be connected to outside networks running on IPv6, a cut down networking stack was proposed in 2007 by Mulligan [6] named 6LoWPAN. Utilising a compressed IP header, with a storing only needed fields attitude, as well as other methods.

 

IDS – Intrusion Detection System. An Intrusion Detection System is a set of either software or hardware systems that are put in place in order to activate security protocol [7] if behaviour of devices inside or outside of a network are deemed as potentially dangerous. There are two types of system used, a signature and anomaly-based system. Signature-based systems operate by comparing network behaviour to a set of rules listed as potentially harmful and activating relevant security protocol. These are simple and very accurate systems, however, do not trigger with unknown attack behaviours [8]. Anomaly-based systems are based on the opposite, a model/ruleset is provided to the IDS of how a device would behave on the network. If a device behaves unlike it is expected, it will be flagged as an intruder.

 


## RPL Security Mechanisms

 

The RPL standard states three different modes of operation for securing routing of the Network, unsecured mode, pre-installed mode, and authenticated mode [9]. Unsecured Mode relies purely on the link-layer security of the network and does not add any kind of encryption to the control messages sent throughout of the network. Pre-installed mode works by making use of preinstalled symmetrical encryption keys on each of the nodes in the network to encrypt the control traffic. Authenticated mode makes use of preinstalled encryption keys for control messages used for nodes wanting to join the network, after which nodes get new keys from a trusted authority [10].

 


## Possible Mitigation Methods

 


VeRA – Version Number and Rank Authentication or VeRA was described initially in 2011 by Dvir et al. [11] as a method verifying the version number and rank of a node by creating a hash chain of the rank stemming from the sink. This allows for a method of proving if a rank is invalid or not as the rank of nodes lower in the tree systemically must be lower than that of its parent. This logic means that it is impossible for an attacker to employ a decreased rank attack unless they prevent the full propagation of the new version number throughout the network. Once the version number increase has been attempted twice, an attacker can use this information to figure out the random number used in the hash chain. This allows the attacker to display a valid decreased rank to initiate the attack.

 
|Pros|Cons|
|--|--|
|Allows for an extremely simple and robust method of proving a node’s rank in the network.|VeRA can be exploited and surpassed, if the attacker acquires enough information to calculate the root rank hash.|
|VeRA also provides a method of protecting against version number attacks using the same hash.|Depending on the maximum rank value set, the hash calculation can be resource intensive.|
 

*Figure 1: Features of utilising VeRA for Rank Attack Mitigation*

 


 SVELTE – The security concept named after a word describing something is streamlined and clean was first described in 2013 by Tolsma, P. et al [3] as an Intrusion Detection System (IDS) developed for use in 6LoWPAN. This IDS is based on 2 fundamental methods and a mini firewall system. SVETLE is proposed as using both the signature based and anomaly-based IDS methods [4] to accurately detect attacks on the network. SVELTE utilises a locally stored table of each of the routes of the network on the sink node, this information includes the advertised rank of the nodes. This information is then used to generate a topology graph of the children. The signature-based method looks for known behaviours of an attacking node [7], such as a node changing rank without changing in proximity to the sink node. This makes SVELTE very suitable as a mitigation method for Decreased Rank Attacks. The anomaly-based method is mainly utilised in detecting attacker behaviour not defined in the signature-based IDS method, such as attacks from outside of the IoT network from public network acting as a “mini-firewall”.

 
|Pros|Cons|
|--|--|
|SVELTE can be updated with new and updated attack definitions, which allows for the best security against newer attack vectors.|Due to SVELTE’s dependency of the root node of a network having a wholistic image of the network and all its nodes, a large amount of traffic is generated throughout the network during initial discovery|
|SVELTE also protects the network from the public domain, which may open of the possibility of a secured WAN connected IoT network|SVELTE is only defined as an intrusion detection system, so it may detect a decreased rank attack but does not have its own method of isolating the attacker|



## Conclusion

While there are many more mitigation methods other than the ones discussed in this report, it remains that there is no one best choice, and it depends on each network environment, use case and scale which mitigation which suits best. This is why constant research of new contributions to network security is always important. 


## References

 


[1]     Almusaylim, Z. et al. (2020) Detection and Mitigation of RPL Rank and Version Number Attacks in the Internet of Things: SRPL-RP. Sensors. [Online] 20 (21), 5997. [online]. Available from:[ http://dx.doi.org/10.3390/s20215997](http://dx.doi.org/10.3390/s20215997)

 


[2]     Tolsma, P.  et al (2021) T-TRAIL: Preventing Decreased Rank Attacks in RPL-based IoT Networks. thesis. Delft University Student Theses. Available online at:[ http://resolver.tudelft.nl/uuid:eb373e00-ce69-4bd7-8329-68f4d4e0bd2f](http://resolver.tudelft.nl/uuid:eb373e00-ce69-4bd7-8329-68f4d4e0bd2f)

 


[3] 	S. Raza, L. Wallgren, and T. Voigt (2013), Svelte: Real-time intrusion detection in the internet of things, Ad Hoc Networks, vol. 11, pp. 2661–2674. Available from:[ https://dl.acm.org/doi/10.1016/j.adhoc.2013.04.014](https://dl.acm.org/doi/10.1016/j.adhoc.2013.04.014)

 


[4] 	Napiah, M.N., Idris, M.Y.I.B., Ramli, R. and Ahmedy, I., 2018. Compression header analyzer intrusion detection system (CHA-IDS) for 6LoWPAN communication protocol. IEEE Access, 6, pp.16623-16638. Available from:[ https://doi.org/10.1109/ACCESS.2018.2798626](https://doi.org/10.1109/ACCESS.2018.2798626)

 


[5] 	Shreenivas, D., Raza, S., & Voigt, T. (2017). Intrusion Detection in the RPL-Connected 6LoWPAN Networks. Proceedings of the 3rd ACM International Workshop on IoT Privacy, Trust, and Security, 31–38.[ https://doi.org/10.1145/3055245.3055252](https://doi.org/10.1145/3055245.3055252)

 


[6] 	Mulligan, G. (2007). The 6LoWPAN Architecture. Proceedings of the 4th Workshop on Embedded Networked Sensors, 78–82.[ https://doi.org/10.1145/1278972.1278992](https://doi.org/10.1145/1278972.1278992)

 


[7] 	Khraisat, A., Gondal, I., Vamplew, P., & Kamruzzaman, J. (2019). Survey of intrusion detection systems: techniques, datasets and challenges. Cybersecurity, 2(1), 20.[ https://doi.org/10.1186/s42400-019-0038-7](https://doi.org/10.1186/s42400-019-0038-7)

 


[8] 	Liao, H.-J., Richard Lin, C.-H., Lin, Y.-C., & Tung, K.-Y. (2013). Intrusion detection system: A comprehensive review. Journal of Network and Computer Applications, 36(1), 16–24.[ https://doi.org/https://doi.org/10.1016/j.jnca.2012.09.004](https://doi.org/https:/doi.org/10.1016/j.jnca.2012.09.004)

 


[9] 	Simoglou, G., Violettas, G., Petridou, S., & Mamatas, L. (2021). Intrusion detection systems for RPL security: A comparative analysis. Computers & Security, 104, 102219.[ https://doi.org/https://doi.org/10.1016/j.cose.2021.102219](https://doi.org/https:/doi.org/10.1016/j.cose.2021.102219)

 


[10]  Ordu, A. B., Bayar, M., & Örs, B. (2022). RPL Authenticated Mode Evaluation: Authenticated Key Exchange and Network Behavioral. 2022 Thirteenth International Conference on Ubiquitous and Future Networks (ICUFN), 167–173.[ https://doi.org/10.1109/ICUFN55119.2022.9829601](https://doi.org/10.1109/ICUFN55119.2022.9829601)

 



[11]  Dvir, A., Holczer, T., & Buttyan, L. (2011). VeRA - Version Number and Rank Authentication in RPL. 2011 IEEE Eighth International Conference on Mobile Ad-Hoc and Sensor Systems, 709–714.[ https://doi.org/10.1109/MASS.2011.76](https://doi.org/10.1109/MASS.2011.76)

 