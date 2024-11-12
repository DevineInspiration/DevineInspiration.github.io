---
layout: post
title: SDN Performance and Security in Enterprise
date: 2021-12-10 23:58:00 +0000

---

## Introduction

With more and more large organisations relying on handling large amount of data traffic for their business operations, a high performant, resilient and flexible network solution is exactly what these organisations are looking for. A large amount of research has gone into the development and improvement of traditional networks with interest shifting recently into a different networking paradigm, Software Defined Networking.

Software Defined Networks (SDN), a term used to describe a form of network where the portions of the network responsible for the handling of transmitting data over a physical medium and the control of the flow of the data that is being sent over this medium is split away from a per device configuration. These different layers are often described as “planes”. The three main planes in an SDN are the data plane, control plane, application plane. The data plane is made up of vendor non specific switches running an SDN compatible operating system, this plane is where the data that is to be sent over the network is actually sent, this layer is the “outward facing side” of the SDN as the data plane is what interfaces with the devices on the network. The control plane consists of one or more controllers that are configured to make decisions regarding the flow of data traffic over the data plane, orchestrating the functionality of the entire network, carrying out functions such as load balancing, switch configuration and various network security operations. The control plane also communicates with the application plane through APIs which allow for front facing software to be used to control and influence the networks functionality, eg a network switch being configured through a web interface.

Local Area Network (LAN) - Is a defined as a group of end devices connected in a limited geographical area, for example, a network shared by all terminals in the marketing department, is a Local Area Network

Wide Area Network (WAN) - Is defined as a network that covers a large geographical area, often extending outside of the area that premises take up, rather than links between end devices and networking hardware such as ethernet which is often found in a LAN, WANs are often made up of majority networking hardware with little to no end devices connected, connected with mediums such as telephone lines, or fiber optics.

**Background research	**

As part of this report, understanding of how an SDN functions was crucial in being able to make educated analysis and conclusions on the effectiveness of an SDN. To begin, the processes behind how data flow in the network was researched. At a very high level analysis SDNs function with the same base premise as any other network utilizing hop-by-hop routing. However, there is a significant difference in how the packet is handled at each of those hops compared to a Traditional Network. When a Packet arrives at a openflow switch, the switch tries to match the flows with information stored in the first flow table stored (flow table 0), if the packet being received by the switch is following a flow defined in flow table 0, it will carry out the instructions defined in the flow table entry, this may include modifying the packet, updating action instructions or updating the metadata of the packet. As described in Section 2 of the paper written by (Bholebawa et all, 2016)

After this processing is carried out, the switch may then hand the packet over to the next flow table, which is of lower priority. The processing will happen again, until according to table configuration a suitable instruction reached to either drop the packet, forward to the OpenFlow controller - where it will then be the controllers responsibility to decide what to do with the packet - or “execute action set” where the all of the actions written to the set by the previous tables will be carried out, this will often involve the packet will be forwarded on to the next destination this process is described in Chapter 5 of the OpenFlow Specification (Open Networking Foundation, 2012)

For a traditional network switch the MAC address of a device is assigned to the incoming port, and stored in the MAC address table, the packet is then forwarded to the port which has had the packet’s destination MAC address assigned to it (Spurgeon, 2013), these traditional switches are transparent to the devices on the network and are unable to make any flow decisions based on behaviour or make modifications to the Ethernet packet like the OpenFlow Switches. In a traditional network these functions are left to dedicated hardware such as firewalls or routers.

There are two types of SDN deployment categorisation, SD-LAN and SD-WAN. 

SD-LANs are what we will be referring to as SDNs in this report, this is a replacement for traditional networking topologies. SD-WANs are a type of network that is built upon the fundamentals of SDN, but used over a large area, while still providing the features of a traditional SDN, such as the intelligent load balancing, security features, and the dynamic routing enabled by the technology. 

**Enterprise Advantages of using an SDN**

There are many advantages for an enterprise for adopting a software defined network,  better security, flexibility and programmability being some of them. Security is a massive advantage of an SDN due to the centralisation of the control of the network, allowing for better detection of attacks distributed across a network. As traffic monitoring statistics are being supplied from every piece of networking hardware this allows for a SDN to have a form of intrusion detection system at every point compared to a traditional network where IDSs were only active in certain devices in the network (M. Dabbagh et al., 2015). This means that attacks can be spotted faster and availability in the network is better maintained 

Ability to detect switch behaviours more accurately, as switches in an SDN are feeding as much information to the controller as any other device, a potentially damaged or compromised switch that is dropping all or some of the packets its receiving can be taken out of the routes distributed around the network, (Gelberger, Yemini and Giladi, 2013) this type of detection making use of the holistic network view is further discussed in referenced paper for more details.

Allows for much faster deployment of new features, software updates can be pushed across the entire network almost instantly, whereas a traditional network may have to wait for new hardware to arrive before implementing the new features (Orange Business, 2016). As the control plane of the switch is taken away from the single piece of networking hardware as in traditional networking hardware deployments, the dependency on hardware performance for processes that would be part of the control plane are no longer as significant of a factor. This means underlying networking hardware such as switches can be used for longer, assuming there is no need for increase in dataplane performance. Traditionally feature upgrades happen more frequently in an enterprise network (Earls, 2020). This increase in the lifespan of the networking hardware could significantly reduce regular costs as to upgrade the hardware in an enterprise's network, a significant amount of time planning the new deployment would be required, with attention also being shown towards the labour that would come with setting up and installing new and updated hardware into the network.

The functionality of an SDN also reduces the costs of setting up and maintaining a network as there is no longer a requirement for hardware dedicated to fill the role of a firewall, or IDS, this functionality can be programmed into the SDN controller and into flow tables of the switches

Allows for more complex control polices, supporting filtration of access by user, device, application, location, available bandwidth, or time of day, metrics that otherwise wouldn't be available to the routing hardware

Allows for Dynamic optimization of the LAN, driven by app priorities

Ability to focus network resources where they serve the organization’s most important needs

Fine-grained application visibility and control at the network edge

However this causes concern as if the centralised controller is attacked, it becomes a single point of failure for the entire network, and if no redundancy is factored into the network design the entire network will stop functioning

Another advantage of an SDN is it prevents vendor lockin, there are no hardware vendor requirements in an SDN, if the switch can be installed with a compatible operating system then the switch can be controlled by a SDN controller regardless of the manufacturer of the hardware

## Performance Testing of SDNs

### Experimentation Methodology

Experimentation was carried out using the mininet network simulator, using miniedit graphical user interface configuration and ONOS operating system for the SDN controller running on a Linux virtual machine

The two types of network were created using the tools mentioned previously, in this report the networks consisted of 6 hosts and 4 switches, see Figure 1 and 2.


![Figure 1](./image1.png "Figure 1: Simulated Topology of Traditional Network in Miniedit")


*Figure 1: Simulated Topology of Traditional Network in Miniedit*

As part of the paper question, performance will be analysed to see if an SDN is suitable for use as a enterprise network, to do this each of the topologies were tested in 2 scenarios, one scenario where a host is communicating to another host connected to the same switch, and one where a host is communicating across multiple switches, in an attempt to measure the impact of proximity on the performance of the networks.


![Figure 2](./image2.png "Figure 2: Simulated Topology of Software Defined Network in Miniedit")


*Figure 2: Simulated Topology of Software Defined Network in Miniedit*

For each topology, and scenario, Bandwidth tests were ran for TCP and UDP traffic 2 times, one with a send rate of 10Gb/s and one with 100Gb/s in order to test performance during a load that would saturate the hardware tested, this could be an indication of the hardware overhead of each type of network or give an idea of the performance during a denial of service attack. Ensuring service availability may be very important to an organisation so this type of testing is reasonable, it may be a significant factor when an organisation chooses whether or not to switch to a software defined network or not. It should be mentioned each of these tests were run 3 times and the average across these runs is the value used for making comparisons later. 

Performance metrics were gathered by running Iperf on the simulated hosts and directing them to the automatically assigned addresses of other hosts on the network. Host 1 was selected to always the server for the Iperf testing application, the server was configured with arguments defining, the role of the host (server), the protocol to be used (TCP or UDP), the port to communicate over and the interval for reporting the datarates produced by the test, as seen in Figure 3


![Figure 3](./image3.png "Figure 3: Configuration of IPerf server for UDP protocol")


*Figure 3: Configuration of IPerf server for UDP protocol*

The other hosts were configured to communicate back to the IPerf server, using arguments to define the server address, protocol to be used, transmission rate, test duration and port to communicate on, as seen in Figure 4


![Figure 4](./image4.png "Figure 4: Configuration of IPerf client for UDP protocol @ 100Gb/s sendrate")


*Figure 4: Configuration of IPerf client for UDP protocol @ 100Gb/s sendrate*

IPerf also provides metrics on the number of datagrams that arrived out of order and that were dropped, these values were also recorded for a comparison between the topologies when it comes to the reliability of the transmission over the two types of network.

Latency testing was carried out using a ping test, each scenario being tested 3 times, with each test containing 10 readings, the average of these 10 readings used for comparisons. See Figure 5.


![Figure 5](./image5.png "Figure 5: Latency Test Example")


*Figure 5: Latency Test Example*

### TCP Throughput Results

The first set of tests carried out was measuring the maximum throughput of both types of networks while communicating using the TCP transmission protocol. Results showed that in every scenario where the send rate was limited to 10Gb/s the Measured throughput was 10.7Gb/s. This result was hyper consistent. Every single one of the results for both the Traditional Topology and SDN, When sending from Host 1 to Host 2 (short distance) or Host 1 to Host 6 (long distance) every test returned that same 10.7Gb/s. In all of these tests there were 0 reported dropped or out of order packets which demonstrates that both network topologies are comfortably capable of handling this send rate, and the results were ultimately limited by the sendrate.

The second set of tests was measuring the throughput of the networks with a send rate of 100Gb/s coming from the IPerf client, also communicating using the TCP Protocol. Results in this scenario are more interesting, after running the 3 tests The Traditional network had an average maximum throughput when communicating across a short distance (Host 1 to Host 2) of 14.2Gb/s and a long distance (Host 1 to Host 6) throughput of 13.75Gb/s. The SDN fared better in this test, Achieving a average maximum of 16.65Gb/s over a short distance, and 16.75Gb/s over a long distance. With these results plotted, Figure 6, These results demonstrate that the performance of the SDN is higher in this particular traffic load, this is likely due to there being a lower CPU overhead for the simulated SDN, as the throughput was directly limited to the amount of processing power available to the simulation. This means that the SDN is more efficient with hardware cycles and will be more performant on fixed hardware.


![Figure 6](./image6.png "Figure 6: TCP Throughput @ 100Gb sendrate")


*Figure 6: TCP Throughput @ 100Gb sendrate*

Just as before there were no dropped or out of order were reported by IPerf, however this posed a question, what was happening to the rest of the data coming from the client. Initially the results were justified by TCPs ability to detect saturated connections and throttling the speeds. However this may not be the case.

### UDP Throughput Results

The first set of tests returned results severely different from the previous set of TCP Throughput results, As with before the first set was conducted with a 10Gb/s send rate from the client, which after 3 runs, the Traditional Network returned a short distance average maximum throughput of 875Mb/s and Long distance throughput of 712Mb/s.The SDN returned results of 881Mb/s for the short distance test and long distance throughput of 780Mb/s. This shows that for this scenario the SDN is once again more performant when it comes to throughput, with less degradation in speed observed as distance increases. However, there was a noticeable increase in the percentage of datagrams lost in transmission as reported by IPerf. The traditional network topology had measurably lower datagram loss rates of more than 0.48% in a best case scenario, See Figure 7. This brings up the dilemma if delivery rates or raw throughput are more important in the network deployment. SDN is significantly inferior in this case

. 
![Figure 7](./image7.png "Figure 7: UDP Datagram loss percentages")


*Figure 7: UDP Datagram loss percentages*

The second set of results were carried out with a send rate of 100Gb/s from the client. initially the Throughput results are extremely similar, With the Traditional Network achieving an average maximum throughput of 888Mb/s over a short distance and 792Mb/s over a long distance, compared to the SDNs results of 880Mb/s over short distance and 785Mb/s over a long distance. Looking at the datagram loss statistics as shown previously in Figure 7, the decrease in the SDNs performance lead over the Traditional Network can be partially attributed to the increase in data loss rates, but looking at the consistency of the throughputs between the two send rates, it would appear there is a bottle neck elsewhere in the network limiting the throughput, looking deeper into the default mininet configuration, this may be a data link limit. Overall, the two networks performed fairly similarly, but the Traditional Network seemed to be better suited to more scenarios. The simulated traditional network was relatively equal in throughput in the long distance transmission compared to the SDN, however the loss percentages were significantly lower.

### Network Latency Results

The results for the network latency results were very clear, 2 topologies were tested again with the two scenarios, short and long distance communications. The Traditional Network presented an average latency of 103ms for short distance transmissions, and an average of 132ms for long distance. The SDN showed performance improvements in this area, with an average short distance communication latency of just 93ms and long distance of 127ms. After being plotted, Figure 8, it is clear that for time sensitive applications the SDN is the more performant network, this can be attributed to the lower hardware overhead as alluded to earlier


![Figure 8](./image8.png "Figure 8: Average Latency Measurements")


*Figure 8: Average Latency Measurements *

## Conclusion

SDNs are definitely a suitable option for large scale networking operations within an enterprise, the improvements when it comes to security are clear, and through experimentation performance is promising too.  At regular data rates, the performance is extremely similar when using the network, with a slight latency advantage in favour of using an SDN in the scenarios tested.

At extremely high data rates, switch throughput performance was increased slightly with the simulated Software Defined Network, This is likely due to decreased CPU overhead in the simulation, which means performance would be improved in a hardware limited environment.

## Evaluation

This report was relatively thorough in the discussion of SDN deployments in an enterprise scenario, However I feel as if it could be more focused on one of the main attracting features, either security or performance, whereas both are discussed to limited degrees of detail.

Strange behaviour in IPerf was also recorded, with Higher Throughput For TCP traffic using this testing methodology, this is against what would be expected due to the fact that TCP is the more complex protocol, UDP is a connectionless protocol therefore should have more raw throughput. I investigated deeply into this behavior, discovering that the Default dataframe size is 8 KB for TCP, 1470 bytes for UDP using Iperf. this leads me to believe multiple things are happening here:

 Switches are likely being overloaded with the number of dataframes being sent when using UDP, TCP also supports 

Speeds improved when increasing the datagram size significantly when using TCP, datagram size… 

Discovered that transmission speed was limited by CPU performance entirely, Tests on both protocols maxing out CPU…

Possible explanation, could be there is a form of TCP segmentation acceleration happening, TSO (TCP Segmentation Offload) allows for CPU load to be reduced by offloading TCP/IP operations to the network interface card. This is also possible in virtualised machines as seen here:

[https://kb.vmware.com/s/article/2055140](https://kb.vmware.com/s/article/2055140)

## References


Bholebawa, I.Z., Jha, R.K. & Dalal, U.D. (2016). Performance Analysis of Proposed OpenFlow-Based Network Architecture Using Mininet. *Wireless Pers Commun 86,* 943–958. 
  [https://doi.org/10.1007/s11277-015-2963-4](https://doi.org/10.1007/s11277-015-2963-4)


 Open Networking Foundation, (2012). OpenFlow Switch Specification V1.3.0. [online] Opennetworking.org. Available at: [https://opennetworking.org/wp-content/uploads/2014/10/openflow-spec-v1.3.0.pdf](https://opennetworking.org/wp-content/uploads/2014/10/openflow-spec-v1.3.0.pdf)

Spurgeon, C., (2013). Ethernet Switches. *O'Reilly Media*.
[https://learning.oreilly.com/library/view/ethernet-switches/9781449367299/](https://learning.oreilly.com/library/view/ethernet-switches/9781449367299/)

M. Dabbagh, B. Hamdaoui, M. Guizani and A. Rayes. (2015). Software-defined networking security: pros and cons. *IEEE Communications Magazine. vol. 53, no. 6, pp*. 73-79.
[https://ieeexplore.ieee.org/abstract/document/7120048](https://ieeexplore.ieee.org/abstract/document/7120048)

A. Gelberger, N. Yemini and R. Giladi. (2013). Performance Analysis of Software-Defined Networking (SDN),* 2013 IEEE 21st International Symposium on Modelling, Analysis and Simulation of Computer and Telecommunication Systems, pp. 389-393*,
[https://ieeexplore.ieee.org/abstract/document/6730793](https://ieeexplore.ieee.org/abstract/document/6730793)

Orange Business, (2016). Five business benefits of SDN and NFV.. [online] Orange Business Services. Available at: 
[https://www.orange-business.com/en/magazine/five-business-benefits-of-sdn-and-nfv](https://www.orange-business.com/en/magazine/five-business-benefits-of-sdn-and-nfv)

Earls, A, (2020). A look at software-defined networking upgrade guidelines. [online] SearchDataCenter. Available at:
   [https://searchdatacenter.techtarget.com/tip/A-look-at-software-defined-networking-upgrade-guidelines](https://searchdatacenter.techtarget.com/tip/A-look-at-software-defined-networking-upgrade-guidelines)
