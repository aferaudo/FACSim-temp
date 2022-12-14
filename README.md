# FAr-edge Cloud Simulator: simulating edge applications deployment on far-edge resources in beyond 5G networks

FACSim is an [Omnet++](https://omnetpp.org/) simulator modeling  distribution of edge applications on far-edge resources using ETSI MEC-compliant procedures.

The simualtor relies on the well-known 5G simulation model ([Simu5G](https://github.com/Unipisa/Simu5G)) to support edge applications deployment in beyond 5G networks.

As this project depends on Simu5G, its implementation is part of a [Simu5G fork](https://github.com/aferaudo/Simu5G/tree/feat/vim-extension).

Simulations and examples of FACSim will be provided soon in this repo.

## General Architecture
The idea behind FACSim is to leverage far-edge device computational power to form Cloud of resources at far-edge layer. To cope with challenges involving these environments, like device and network heterogeneity, integration with cloud infrastructure etc., the simulator extends the [ETSI MEC architecture](https://www.etsi.org/deliver/etsi_gs/MEC/001_099/003/03.01.01_60/gs_MEC003v030101p.pdf) for distributing standard MEC apps at far-edge level. The adoption of a well-known standard allows to solve various challenges such as:
* **technology coexistence**

* **cloud integration** for delay-tolerant services: ETSI MEC supports the opportunity to deploy requested services on both Cloud and edge resources

* **easy-to-access** services: services accessibility and positioning occur through standard procedure

* **app migration throught standard API**: devices leaving the resource pool can use [ETSI MEC API](https://www.etsi.org/deliver/etsi_gs/MEC/001_099/021/02.01.01_60/gs_MEC021v020101p.pdf) to trigger migration of MEC compliant application deployed on them.


As shown in the figure below, the simulation model extends the concept of MEC-Host by supporting the acquisition of dynamic resources provided by far-edge devices. In particular, each MEC Host defines an Area of Interest within which far-edge nodes can decide whether to provide their onboard resources. The devices entering this area can decide to expose their resources based on rewards proposed by the MEC Host regulating that area. We used a publish-subscribe model, where a Broker handles resource pooling of multiple MEC Hosts. It collects MEC-Host AoI subscription and manage notifications whenever new devices enter that area.

![missing architecture image](./images/graphs-Architecture.png)

Once device resource registration is completed, the MEC Host selects the most suitable host based on the requested MEC App characteristics.

## Implementation
As previously described, the simulation model involves several changes in the ETSI MEC traditional architecture. First of all, the Virtualization Infrastructure Manager (VIM), which is in charge of administering host resources, is aware of the single contributions that each far-edge host brings in terms of capacity. Thus, during startup it subscribes itself to the MEC Host AoI and waits for notifications for each device inside that area. The component in charge of managing and virtualizing internal resources, namely Virtualization Infrastructure (VI), supports the opportunity to run on several hosts regarderless of the underlying layers.

*Do we need other stuff here?*

To allows the deployment of MEC components on different locations in the 5G network, FACSim models them as simple module [Inet Applications](https://doc.omnetpp.org/inet/api-current/neddoc/inet.applications.contract.IApp.html). A potential deployment is shown below.

![missing building block image](./images/implementation_blocks.png)

The figure shows an example of MECHost layer connected to an internal UPF (UPF-MEC in the figure) as described in [this document](https://www.etsi.org/images/files/ETSIWhitePapers/etsi_wp28_mec_in_5G_FINAL.pdf). The UPF  is then connected to a gNB charcterising the 5G Radio Access Network(RAN). 
In FACSim MEC components like VIM, VI, and MECPlatform manager, can be deployed in standalone Standard Hosts ...

## Simulator Features
* Decoupled model for resources subscription
* extension of ETSI MEC components  to support deployment of MEC app on static far-edge nodes (cars)
* Application Mobility Service APIs extended to support migration among far-edge hosts
 

## Known Limitations
The simulator currently does not support the following:
* emulation
* computational time simulation
* mobile nodes
* energy consumption

## Future goals
* Enabling emulation of far-edge nodes
* Including mobile far-edge nodes for MEC app deployment
* Adding additional metrics in far-edge resource selection
* Implementing a case-study tailored to this kind of scenario
* ...


## Contributors
* [Angelo Feraudo](mailto:angelo.feraudo@unibo.it)
* [Alessandro Calvio](mailto:alessandro.calvio@unibo.it)