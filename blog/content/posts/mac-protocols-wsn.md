---
layout: post
title: Analysis and Comparison of MAC Protocols for use in Wireless Sensor Networks
date: 2021-05-15 15:15:00 +0000

---


In this paper, an investigation will be carried out to discover how different factors affect the energy consumption of a wireless sensor network. Three different MAC communication protocols (SMAC, TMAC and ZigBee) will be tested, with the effects of increased node density and sample rate on power consumption being monitored for analysis. The simulated network will be tested with two different radio hardware models (Texas Instruments CC1000 and CC2420) using the Castelia simulator within the OMNETPP Software Platform

Keywords— WSN; Low Power Sensor Networks; SMAC; TMAC; ZigBee; OMNETPP; Castelia; Wireless Connectivity; Ad-Hoc Networks; Energy Efficiency Testing



## 1. Introduction
Wireless sensor networks are incredibly useful tools for monitoring and data collection especially for environment sensing applications (tectonic plate shifts, pollution level monitoring), continuous tracking applications (road usage monitoring), etc. Fundamentally these networks are extremely flexible as they are self-healing but also extremely low power, which makes them suitable for non-permanent deployment on battery power. These factors reduce the number of physical limitations on the deployments of sensors in a wireless network like this. The aim of this report is to get a greater understanding of the effect design variables such as sample rate and node density have on the power consumption of the network. These tests will be carried out using three different MAC protocols to allow for some conclusions to be made about which protocol is most suitable in which scenario. To carry out this analysis the Castelia Simulator is used with appropriate testing methodologies to ensure accurate results.

WSN (Wireless Sensor Networks) - A type of wireless ad-hoc network, built up of multiple physically spaced low power nodes which are used for data gathering e.g., environment sensors or location tracking devices, which then transmit, receive, or forward this information to other nodes or the sink for data for collection and analysis.

MAC (Medium Access Control) – A sublayer of the data link layer of the OSI model. MAC is responsible for the transmission of data packets to and from the network interface of a node. The main function of MAC is to provide addressing and channel access control mechanisms so that nodes on a network can communicate with other nodes across the network or between networks.

SMAC (Sensor MAC) - A transmission protocol that makes use of a schedule that is shared with neighbouring nodes if the current schedule states that the sensor is not meant to be asleep or listening, a sync packet is sent with precise timings to other nodes If a node has a packet to send then instead of listening for packets, it will send out an RTS (request to send packet), if the destination node, be it the sink or another node along the way. If the receiving node is also available, it will return a CTS (clear to send packet) which will begin the transmission process. As part of the SMAC transmission protocol, the data sent in one go is limited in size to reduce the time required to recover in the case of a lost/corrupted packet. Each packet sent will receive a acknowledge packet in response from the receiver to inform the transmitting node it is safe to send the next packet. If there is no acknowledgement packet sent back to the transmitter the time assigned to the total data transmission is increased by 1 packet, and the dropped/corrupted packet is immediately resent. Once the transmission is complete the synchronised schedule will be checked, if the node is required it will stay awake. Otherwise, it will disable its radio and go to sleep until the next listen period specified in the schedule.

TMAC (Timeout MAC) - A transmission protocol very similar to SMAC, however, there is no fixed duty cycle. This means that the nodes will stay awake until they have completed their transmission and are not forced to sleep at regular times. The active state ends whenever they are not required for transmission or receiving of packets. The same RTS, CTS and ACK packets are used which prevents collision and increases communication reliability. [3]

ZigBee - A transmission protocol based on the IEEE standard 802.15.4, ZigBee supports two modes, beacon-enabled and non-beacon enabled networks which uses slotted and unslotted CSMA/CA respectively as its method for collision detection and avoidance. Slotted

CSMA/CA has fixed transmission periods, where a backoff slot is put in place to prevent overlapping or cutting off of transmissions. Whereas unslotted CSMA/CA does not have this hard limit to the time of transmission. This means the nodes trying to transmit while there are ongoing communications on the medium will have to wait and try sending again in a random amount of time.

CSMA/CA (Carrier-sense multiple access with collision avoidance) - A collision detection and avoision protocol, that listens on the transmission medium to see if it is currently in use. The transmitter will only send when the medium is unused. With options to use RTS and CTS packets to prevent collisions caused by two transmissions being sent to a node that is in range of both transmitting nodes, but these two nodes are out of range of each other.

OMNETPP – A simulation library and framework, its main use is in network simulation, emulating both the networking hardware and the physical properties of transmission media, wireless or wired, this allows prototyping of networks without the requirement of dedicated networking hardware.



## 2. Related Work
The CC1000 and CC2420 radio models are based off transceivers produced by Texas Instruments, The CC1000 [1] was designed to used in frequency ranges of 300-1000Mhz whereas the CC2420 [2] was designed for IEEE 802.15.4 compliance/2400Mhz operation which makes the transceiver “ZigBee Ready”, 



## 3. Simulation Configuration
The simulations in this report will be used to compare the energy consumption of SMAC, TMAC and ZigBee MAC protocol in multiple different environment scenarios. To carry out these simulations Castalia is used which is based on the OMNETPP platform. The only data that is extracted from the different scenarios is the energy usages of each of the different protocol as this is the focus of this report. The nodes in these simulations are static with no mobility simulated, but a randomised arrangement is generated when the simulator is started. Each of the simulations is run for 100s as is the recommended time for accuracy according to the Castalia manual. Each of the scenarios was also run 3 times with the results being averaged to further increase result credibility. The data is then extracted from Castalia and plotted into Microsoft Excel for further analysis and charting.


*Table 1 - Simulation Parameters*


![Table 1](./table1.png "Table 1 - Simulation Parameters")


An example of the simulated network topology and visualization of the network transmissions can be seen below:


*Figure 1 - Example Network Topology (All 19 nodes, including sink)*


![Figure 1](./image1.png "Figure 1 - Example Network Topology - All 19 nodes, including sink")




## 4. Analysis
Initially looking at the energy consumption of the three models with the older CC1000 radio model, it is clear that TMAC suffers the most from high frequency/high sample rates. With TMAC drawing over 180% the amount of energy compared to SMAC when the sample interval is set to 10 ms. Another comparison to be made is the difference in power consumption between the different sample intervals, the variance in TMACs power consumption from 10 ms to 50 ms sample intervals was 1.792J whereas SMAC had a consumption variance of just 0.381J showing much higher energy losses in scenarios with higher sample frequency when using the TMAC protocol.


*Figure 2 – Energy Consumption vs Sample Rate using the CC1000 Radio model*
        
![Figure 2](./image2.png "Figure - Energy Consumption vs Sample Rate using the CC1000 Radio model")

Focusing on ZigBee we can see that it is extremely consistent in its power consumption, varying by only 0.019J across the different sample rates tested. However, in all scenarios, SMAC proved to be more efficient with the difference in efficiency increasing as the sample rate increased. 


*Table 2 – Energy Consumption in Joules of MAC protocols using CC1000 radio (variable sample rate)*


![Table 2](./table2.png "Table 2 – Energy Consumption in Joules of MAC protocols using CC1000 radio - variable sample rate")


Moving our attention to using the CC2420 model results the first result we can see is the significant increase in energy consumption when using the ZigBee protocol. Again, the same extreme consistency is shown by ZigBee, with a variance of just 0.009J. However, the average power consumption across all 5 sample rates tested increased by 1.71x. TMACs power consumption fared much better with this radio model, reducing its power consumption by 7.3% and the SMAC protocol having a small increase in consumption at 5.5%. SMAC is still the most energy-efficient protocol in this scenario.


*Figure 3 – Energy Consumption vs Sample Rate using the CC2420 Radio model*


![Figure 3](./image3.png "Figure 3 – Energy Consumption vs Sample Rate using the CC2420 Radio model")


With the extremely stable energy consumption of ZigBee, it can be predicted that for super low sample intervals (1-2 ms) it can be estimated to be much more energy-efficient compared to the other protocols assuming that the energy usage stays at a similar level to the results gathered 


*Table 3 – Energy Consumption in Joules of MAC protocols using CC2420 radio (variable sample rate)*


![Table 3](./table3.png "Table 3 – Energy Consumption in Joules of MAC protocols using CC2420 radio - variable sample rate")



*Figure 4 – Energy Consumption vs Node Density using the CC1000 Radio model*


![Figure 4](./image4.png "Figure 4 – Energy Consumption vs Node Density using the CC1000 Radio mode")


As seen by the graph all of the protocols scale relatively linearly with an increase in node density. The best performer overall being SMAC, with an energy consumption increase of 6.71x while the number of nodes is increased by 6x which is extremely good scaling. TMAC presented fairly good results in this test across a wide range of node densities even posting as the most energy-efficient protocol for very low node density sensor networks. ZigBee is the most linear of the protocols and while being the least efficient in these tests, still proving to be a pretty good option.


   *Table 4 – Energy Consumption in Joules of MAC protocols using CC1000 radio (variable node density)*


![Table 4](./table4.png "Table 4 – Energy Consumption in Joules of MAC protocols using CC1000 radio - variable node density")



*Table 5 – Energy Consumption in Joules of MAC protocols using CC2420 radio (variable node density)*


![Table 5](./table5.png "Table 5 – Energy Consumption in Joules of MAC protocols using CC2420 radio - variable node density")

A very similar story can be said when using the CC2420 model, very linear scaling with both SMAC and TMAC, however, the energy consumption again is significantly raised when using the ZigBee protocol with an increased number of nodes. This may be accredited to the non-fixed transmission timings which will cause the collision avoidance to initiate a resend which costs energy, with collision happening more often, the more nodes there are in the network, which lines up with the data gathered. 



##  Conclusion
To conclude, the SMAC protocol is seen in multiple different tests as being overall the most energy-efficient protocol for this type of wireless sensor network, with TMAC having equal or slightly better performance in low sample-rate, low node density deployments but suffering in scenarios with short sample intervals. Looking at the data ZigBee seems relatively inefficient for deployment in this scenario, however looking at the technical data of each of these protocols, we can see that ZigBee has a much higher maximum bandwidth [4] partially due to the fact it is based on 2.4 GHz frequency bands, with the other two option being based on sub 1 GHz bands. while not tested, the sample rate tests show ZigBee’s advantage in extremely high frequency/high bandwidth scenarios. But for wireless sensor networks, the data rate will be relatively low, meaning this protocol may not be the best choice for a network designer.  


## References 



1. [https://www.ti.com/product/CC1000](https://www.ti.com/product/CC1000), “CC1000 Data Sheet”, Texas Instruments, 2007
2. [https://www.ti.com/product/CC2420](https://www.ti.com/product/CC2420), “CC2420 Data Sheet”, Texas Instruments, 2013
3. Samant, Tapaswini & Datta, Amlan. (2016). Analysis and Comparison of SMAC and TMAC Protocol for Energy Efficient Dynamic Topology in Sensor Network. International Journal of Electrical and Computer Engineering (IJECE) pp 2331-2337
4. [https://zigbeealliance.org/zigbee-faq/](https://zigbeealliance.org/zigbee-faq/) “ZigBee FAQ”, ZigBee Alliance, 2021