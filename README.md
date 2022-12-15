# FAr-edge Cloud Simulator: simulating edge applications deployment on far-edge resources in beyond 5G networks

FACSim is an [Omnet++](https://omnetpp.org/) simulator modeling the distribution of edge applications on far-edge resources using ETSI MEC-compliant procedures.

The simualtor relies on the well-known 5G simulation model ([Simu5G](https://github.com/Unipisa/Simu5G)) to support edge applications deployment in beyond 5G networks.

As this project depends on Simu5G, its implementation is part of a [Simu5G fork](https://github.com/aferaudo/Simu5G/tree/feat/vim-extension).


## General Architecture
The idea behind FACSim is to leverage far-edge device computational power to form Cloud of resources at far-edge layer. To cope with challenges involving these environments, like device and network heterogeneity and integration with cloud infrastructure, the simulator extends the [ETSI MEC architecture](https://www.etsi.org/deliver/etsi_gs/MEC/001_099/003/03.01.01_60/gs_MEC003v030101p.pdf) for distributing standard MEC apps at far-edge level. 

The adoption of a well-known standard allows to solve various challenges such as:
* **technology coexistence**

* **cloud integration** for delay-tolerant services: ETSI MEC supports the deployment of requested services on both Cloud and edge resources through orchestration methods

* **easy-to-access** services: services accessibility and positioning occur through standard procedure

* **app migration throught standard API**: devices leaving the resource pool can use [ETSI MEC API](https://www.etsi.org/deliver/etsi_gs/MEC/001_099/021/02.01.01_60/gs_MEC021v020101p.pdf) to trigger migration of MEC compliant application deployed on them.


As shown in figure 1, the simulation model extends the concept of MEC-Host by supporting the acquisition of dynamic resources provided by far-edge devices. In particular, each MEC Host defines an Area of Interest within which far-edge nodes can decide whether to provide their onboard resources. In this context, devices entering the AoI choose to expose their resources based on some rewards proposed by the MEC Host regulating that area. We used a publish-subscribe model, where a Broker handles resource pooling of multiple MEC Hosts. It collects MEC-Host AoI subscriptions and manage notifications whenever new devices enter that area.

Once device resource registration is completed, the MEC Host selects the most suitable host based on the requested MEC App characteristics.

<p align="center">
    <img src="./images/graphs-Architecture.png" alt="missing architecture image">
    <em>Figure 1 FACSim general Architecture</em>
</p>

## Implementation
As showed in Figure 1, FACSim acts at MEC Host layer in order to make MEC Hosts ablet to acquire dynamic resources from far-edge devices. The main ETSI MEC comoponent to be affect is the Virtualization Infrastructure Manager (VIM) that is in charge of administering host resources. In sucha a scenarion, it should be aware of the single contributions that each far-edge host brings in terms of capacity. Thus, during startup it subscribes itself to the MEC Host AoI and waits for notifications for each device inside that area. In FACSim each MEC component has been modeled as a simple module [Inet Application](https://doc.omnetpp.org/inet/api-current/neddoc/inet.applications.contract.IApp.html), to allow  their deployment on different kind of devices (e.g. Standard host, UE) and locations in the 5G network. The same appllies on components that helps in resource registration.

<p align="center">
    <img src="./images/implementation_blocks.png" alt="missing building block image">
    <em>Figure 2 FACSim MECHost level deployment example</em>
</p>

Figure 2 shows an example of MECHost layer connected to an internal UPF (UPF-MEC in the figure) as described in [this document](https://www.etsi.org/images/files/ETSIWhitePapers/etsi_wp28_mec_in_5G_FINAL.pdf). The UPF  is then connected to a gNB charcterising the 5G Radio Access Network(RAN). 

Since MEC components are modeled as applications, their deployment might occur on different devices. For example, as showed in the figure below, we choose to deploy FACSim MECHost within three devices: one representing Internal Resources, one for MEC Services registration and deployment and finally a device hosting the MEC Platform Manager. 

On the other side, FACSim requires 5G User Equipments (UEs), modeled by Simu5G, to demonstrate their interest in providing their onboard resources. To do that the simulation model introduces a **car** module as an extension of 5G UE, as present-day cars carry a considerable amount of on-board resources. 

<p align="center">
    <img src="./images/car.png" alt="missing car image" width="300"height="300"><br/>
    <em>Figure 3 FACSim Car Module</em>
</p>

Figure 3 illustrates the Car module provided by FACSim. It runs three kind of applications. The first two represents ETSI MEC components, where the former is the Virtualization Infrastructure (VI) in charge of managing and virtualizing internal resources, while the latter represents application compliant with the ETSI MEC Standard. The last application *Client Resource App* edeal with  reward discovery and resource provisioning.

In the current version of the simulation module, we expect that far-edge devices (i.e. cars) remain fixed in a location for a certain amount of time (i.e. cars in a parking lot). Additionally, it should be noted that the *FACSim car* module  has been defined as "car", however it can be any kind of module which is able to communicate in 5G networks.


<p align="center">
    <img src="./images/simulation.png" alt="missing simulation image" width="350"height="400"><br/>
    <em>Figure 4 FACSim Simulation Scenario</em>
</p>

In figure 4 we illustrated an example of FACSim scenario, where UEs request for MEC Apps execution, and MEC APP deployment occur on FACSim Car module inside a parking lot.

## Simulator Features
* Decoupled model for resources subscription
* extension of ETSI MEC components to support deployment of MEC app on static far-edge nodes (cars)
* Application Mobility Service APIs extended to support migration among far-edge hosts
 

## Known Limitations
The simulator currently does not support the following:
* algorithm for resource scheduling - so far only Round Robin and Best First are supported
* emulation
* computational time simulation
* mobile nodes (e.g. SUMO and VEINS)
* energy consumption metric for resource allocation

## Future goals
* Enabling emulation of far-edge nodes
* Including mobile far-edge nodes for MEC app deployment
* Adding additional metrics in far-edge resource selection
* Implementing a case-study tailored to this kind of scenario
* Add How-to in this repo
* multi-access far-edge devices


## Contributors
* [Angelo Feraudo](mailto:angelo.feraudo@unibo.it)
* [Alessandro Calvio](mailto:alessandro.calvio@unibo.it)