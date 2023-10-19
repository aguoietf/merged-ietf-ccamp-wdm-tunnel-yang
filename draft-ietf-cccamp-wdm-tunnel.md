---
coding: utf-8

title: A YANG Data Model for WDM Tunnels

abbrev: A YANG Data Model for WDM Tunnels
docname: draft-ietf-ccamp-wdm-tunnel-00
workgroup: CCAMP Working Group
category: std
ipr: trust200902

stand_alone: yes
pi: [toc, sortrefs, symrefs, comments]

author:
  -
    name: Aihua Guo
    org: Futurewei Technologies
    email: aihuaguo.ietf@gmail.com
  -
    name: Sergio Belotti
    org: Nokia
    email: Sergio.belotti@nokia.com
  -
    name: G. Galimberti
    org: Individual
    email: ggalimbe56@gmail.com
  -
    name: Daniel King
    org: Old Dog Consulting
    email: daniel@olddog.co.uk
  -
    name: Jorge E. Lopez de Vergara Mendez
    org: Naudit HPCN
    email: jorge.lopez_vergara@uam.es
  -
    name: Daniel Perdices Burrero
    org: Universidad Autonoma de Madrid
    email: daniel.perdices@uam.es

contributor:
  -
    name: Haomian Zheng
    org: Huawei Technologies
    street: H1, Xiliu Beipo Village, Songshan Lake
    city: Dongguan
    country: China
    email: zhenghaomian@huawei.com
  -
    name: Italo Busi
    org: Huawei Technologies
    email: italo.busi@huawei.com
  -
    name: Oscar Gonzalez de Dios
    ins: O. Gonzalez de Dios
    org: Telefonica
    email: oscar.gonzalezdedios@telefonica.com
  -
    name: Victor Lopez
    org: Nokia
    email: victor.lopez@nokia.com
  -
    name: Dieter Beller
    org: Nokia
    email: Dieter.Beller@nokia.com
  -
    name: Ricard Vilalta
    org: CTTC
    email: ricard.vilalta@cttc.es
  -
    name: Young Lee
    org: Samsung
    email: younglee.tx@gmail.com
  -
    name: Bin Yeong Yoon
    org: ETRI
    email: byyun@etri.re.kr
  -
    name: Daniel Michaud Vallinoto
    org: Universidad Autonoma de Madrid
    email: daniel.michaud@estudiante.uam.es
  -
    name: Zafar Ali
    org: Cisco
    email: zali@cisco.com

--- abstract

   This document defines a YANG model for managing flexi-grid optical
   tunnels (media-channels), complementing the information provided by the
   flexi-grid topology model.

   The YANG data model defined in this document conforms to the Network
   Management Datastore Architecture (NMDA).

--- middle

# Introduction
   Transport networks are evolving from current DWDM systems towards
   elastic optical networks, based on flexi-grid transmission and
   switching technologies {{!RFC7698}}. Such technology
   aims at increasing both transport network scalability and flexibility, 
   allowing the optimization of bandwidth usage.

   While {{!I-D.ietf-ccamp-flexigrid-yang}} focuses on flexi-grid
   objects such as nodes, transponders and links, this document presents
   a YANG {{!RFC7950}} model for the flexi-grid tunnel (media-channel). This YANG
   module defines the whole path from a source transponder or node to
   the destination through a number of intermediate nodes in the flexi-grid network.
	 
   This document identifies the flexi-grid tunne components,
   parameters and their values, characterizes the features and the
   performances of the flexi-grid elements. An application example is
   provided towards the end of the document to better understand
   their utility.

## Terminology

   The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
   "SHOULD", "SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and
   "OPTIONAL" in this document are to be interpreted as described in
   BCP 14 {{!RFC2119}} {{!RFC8174}} when, and only when, they appear in all
   capitals, as shown here.

   The terminology for describing YANG data models is found in
   {{!RFC7950}}.

   Refer to {{!RFC7446}} and {{!RFC7699}} for the key terms used in this document.

   The following terms are defined in {{!RFC7950}} and are not redefined here:
   -  client

   -  server

   -  augment

   -  data model

   -  data node

   The following terms are defined in {{!RFC6241}} and are not redefined here:
   -  configuration data

   -  state data

## Overview

   The present model defines a flexi-grid tunnel mainly composed of:
   -  source address

   -  source flexi-grid port

   -  source flexi-grid transponder

   -  destination address

   -  destination flexi-grid port

   -  destination flexi-grid transponder

   -  list of links that defines the path

   -  other optical attributes

   Each path can be a tunnel (only defined by source and
   destination node) or a network tunnel (additionally needs
   source and destination transponders). Therefore, all the attributes
   are optional to support both situations.

   This is achieved by a combination of the traffic engineering tunnel
   attributes explained in {{?I-D.ietf-teas-yang-te"}} and augments
   when necessary. For instance, source address, source flexi-grid
   transponder, destination address and destination flexi-grid
   transponder attributes are directly taken from tunnel, whereas other
   attributes such as source flexi-grid port, destination flexi-grid
   port are defined, as they are specific for flexi-grid.
	  
## Prefixes in Data Node Names

   In this document, names of data nodes and other data model objects
   are prefixed using the standard prefix associated with the
   corresponding YANG imported modules, as shown in {{tab-prefixes}}.

   | Prefix   | YANG Module                  | Reference         |
   |----------|------------------------------|-------------------|
   | yang     | ietf-yang-types              | {{!RFC6991}}      |
   | inet     | ietf-inet-types              | {{!RFC6991}}      |
   | nt       | ietf-network-topology        | {{!RFC8345}}      |
   | nw       | ietf-network-topology        | {{!RFC8345}}      |
   | tet      | ietf-te-topology             | {{!RFC8795}}      |
   | ietf-nss | ietf-network-slice-service   | \[RFCVVVV]        |
   | te-types | ietf-te-types                | \[RFCWWWW]        |
   | otnt     | ietf-otn-topology            | \[RFCYYYY]        |
   | l1-types | ietf-layer1-types            | \[RFCZZZZ]        |
   | otns     | ietf-otn-slice               | \[RFCXXXX]        |
   | otns-mpi | ietf-otn-slice-mpi           | \[RFCXXXX]        |
{: #tab-prefixes title="Prefixes and Corresponding YANG Modules"}

RFC Editor Note:
Please replace VVVV with the RFC number assigned to {{?I-D.ietf-teas-ietf-network-slice-nbi-yang}}.
Please replace WWWW with the RFC number assigned to {{?I-D.ietf-teas-rfc8776-update}}.
Please replace XXXX with the RFC number assigned to this document.
Please replace YYYY with the RFC number assigned to {{!I-D.ietf-ccamp-otn-topo-yang}}.
Please replace ZZZZ with the RFC number assigned to {{!I-D.ietf-ccamp-layer1-types}}.
Please remove this note.

## Definition of OTN Slice
   An OTN slice is an OTN virtual network topology connecting a number
   of OTN endpoints using a set of shared or dedicated OTN network resources to
   satisfy specific service level objectives (SLOs).

   An OTN slice is a technology-specific realization of an IETF network slice 
   {{!I-D.ietf-teas-ietf-network-slices}} in the OTN domain, with the
   capability of configuring slice resources in the term of OTN technologies. 
   Therefore, all the terms and definitions concerning network slicing as 
   defined in {{!I-D.ietf-teas-ietf-network-slices}} apply to OTN slicing.
   
   An OTN slice can span multiple OTN administrative domains, encompassing 
   access links, intra-domain paths, and inter-domain links. 
   An OTN slice may include multiple endpoints, each associated with a set of physical
   or logical resources, e.g. optical port or time slots, at the termination point (TP) of 
   an access link or inter-domain link at an OTN provider edge (PE) equipment. 

   An end-to-end OTN slice may be composed of multiple OTN segment slices in
   a hierarchical or sequential (or stitched) combination. 

   {{fig-otn-slice}} illustrates the scope of OTN slices in multi-domain 
   environment.

~~~~
      <------------------End-to-end OTN Slice---------------->

      <- OTN Segment Slice 1 --->  <-- OTN Segment Slice 2 -->


       +-------------------------+  +-----------------------+
       | +-----+      +-------+  |  | +-------+      +-----+|
+----+ | | OTN |      | OTN   |  |  | | OTN   |      | OTN ||  +----+
| CE +-+-o PE  +-...--+ Borde o--+--+-o Borde +-...--+ PE  o+--+ CE |
+----+||/|     |      | Node  |\ || | | Node  |      |     || |+----+
      |||+-----+      +-------+| || | +-------+      +-----+| |
      |||    OTN Domain 1      | || |      OTN Domain 2     | |
      |++----------------------+-+| +-----------------------+ |
      | |                      |  |                           |
      | +-----+    +-----------+  |                           |
      |       |    |              |                           |
      V       V    V              V                           V
   Access    OTN Slice        Inter-domain                  Access
   Link      Endpoint         Link                          Link

~~~~
{: #fig-otn-slice title="OTN Slice"}

   OTN slices may be pre-configured by the management plane and presented to 
   the customer via the northbound interface (NBI), or be dynamically 
   provisioned by a higher layer slice controller, e.g., an IETF network slice 
   controller (IETF NSC) through the NBI. The OTN slice is 
   provided by a service provider to a customer to be used as though it was part
   of the customer's own networks.
         
# Use Cases for OTN Network Slicing

## Leased Line Services with OTN

   For end business customers (like OTT or enterprises), leased lines
   have the advantage of providing high-speed connections with low
   costs. On the other hand, the traffic control of leased lines is very
   challenging due to rapid changes in service demands. Carriers are
   recommended to provide network-level slicing capabilities to meet
   this demand. Based on such capabilities, private network users have
   full control over the sliced resources which have been allocated to them
   and which could be used to support their leased lines, when needed.
   Users may formulate policies based on the demand for services and
   time to schedule the resources from the entire network's perspective
   flexibly. For example, the bandwidth between any two points may be
   established or released based on the time or monitored traffic
   characteristics. The routing and bandwidth may be adjusted at a
   specific time interval to maximize network resource utilization
   efficiency.

## Co-construction and Sharing

   Co-construction and sharing of a network are becoming a popular means
   among service providers to reduce networking building CAPEX. For Co-
   construction and sharing case, there are typically multiple co-
   founders for the same network. For example, one founder may provide
   optical fibres and another founder may provide OTN equipment, while
   each occupies a certain percentage of the usage rights of the network
   resources. In this scenario, the network O&M is performed by a
   certain founder in each region, where the same founder usually
   deploys an independent management and control system. The other
   founders of the network use each other's management and control
   system to provision services remotely. In this scenario, different
   founders' network resources need to be automatically (associated)
   divided, isolated, and visualized. All founders may share or have
   independent O&M capabilities, and should be able to perform service-
   level provisioning in their respective slices.

## Wholesale of optical resources

   In the optical resource wholesale market, smaller, local carriers and
   wireless carriers may rent resources from larger carriers, or
   infrastructure carriers instead of building their networks. Likewise,
   international carriers may rent resources from respective local
   carriers and local carriers may lease their owned networks to each
   other to achieve better network utilization efficiency.
   From the perspective of a resource provider, it is crucial that a
   network slice is timely configured to meet traffic matrix
   requirements requested by its tenants. The support for multi-tenancy
   within the resource provider's network demands that the network
   slices are qualitatively isolated from each other to meet the
   requirements for transparency, non-interference, and security.
   Typically, a resource purchaser expects to use the leased network
   resources flexibly, just like they are self-constructed. Therefore,
   the purchaser is not only provided with a network slice, but also the
   full set of functionalities for operating and maintaining the network
   slice.  The purchaser also expects to, flexibly and independently, 
   schedule and maintain physical resources to support their own
   end-to-end automation using both leased and self-constructed network
   resources.

## Vertical dedicated network with OTN

   Vertical industry slicing is an emerging category of network slicing
   due to the high demand for private high-speed network interconnects
   for industrial applications.
   In this scenario, the biggest challenge is to implement
   differentiated optical network slices based on the requirements from
   different industries. For example, in the financial industry, to
   support high-frequency transactions, the slice must ensure to provide
   the minimum latency along with the mechanism for latency management.
   For the healthcare industry, online diagnosis network and software
   capabilities to ensure the delivery of HD video without frame loss.
   For bulk data migration in data centers, the network needs to support
   on-demand, large-bandwidth allocation. In each of the aforementioned
   vertical industry scenarios, the bandwidth shall be adjusted as
   required to ensure flexible and efficient network resource usage.

## End-to-end network slicing

   In an end-to-end network slicing scenario such as 5G network slicing
   {{TS.28.530-3GPP}}, an IETF network slice {{?I-D.ietf-teas-ietf-network-slices}}
   provides the required connectivity between other different segments 
   of an end-to-end network slice, such as the Radio Access Network 
   (RAN) and the Core Network (CN) segments, with a specific 
   performance commitment. An IETF network slice could be composed of 
   network slices from multiple technological and administrative 
   domains. An IETF network slice can be realized by using or combining
   multiple underlying OTN slices with OTN resources, e.g., ODU time 
   slots or ODU containers, to achieve end-to-end slicing across the transport
   domain.

# Framework for OTN slicing

   OTN slices may be abstracted differently depending on the requirement contained
   in the configuration provided by the slice customer. Whereas the customer requests
   an OTN slice to provide connectivity between specified endpoints, an OTN slice 
   can be abstracted as a set of endpoint-to-endpoint links, with each link formed 
   by an end-to-end tunnel across the underlying OTN networks. The resources
   associated with each link of the slice is reserved and commissioned in the underlying
   physical network upon the completion of configuring the OTN slice and all the 
   links are active. 
   
   An OTN slice can also be abstracted as an abstract topology when the customer requests
   the slice to share resources between multiple endpoints and to use the resources on demand.
   The abstract topology may consist of virtual nodes and virtual links, and their associated
   resources are reserved but not commissioned across the underlying OTN networks. The 
   customer can later commission resources within the slice dynamically using the NBI provided
   by the service provider. An OTN slice could use abstract topology to connect endpoints with 
   shared resources to optimize the resource utilization, and connections can be activated 
   within the slice as needed.
    
   It is worth noting that those means to abstract an OTN slice are similar to the Virtual 
   Network (VN) abstraction defined for higher-level interfaces in {{!RFC8453}}, in which context
   a connectivity-based slice corresponds to Type 1 VN and a resource-based slice corresponds to 
   Type 2 VN, respectively.

   A particular resource in an OTN network, such as a port or link, may be
   sliced with one of the two granularity levels:

   -  Link-based slicing, in which a link and its associated link
      termination points (LTPs) are dedicatedly allocated to a
      particular OTN slice.

   -  Tributary-slot based slicing, in which multiple OTN slices
      share the same link by allocating different OTN tributary slots in
      different granularities.

   Furthermore, an OTN switch is typically fully non-blocking switching 
   at the lowest ODU container granularity, it is
desirable to specify just the total number of ODU containers in the
lowest granularity (e.g. ODU0), when configuring tributary-slot based
slicing on links and ports internal to an OTN network. In multi-domain
OTN network scenarios where separate OTN slices are created on
each of the OTN networks and are stitched at inter-domain OTN links, it
is necessary to specify matching tributary slots at the endpoints of the
inter-domain links. In some real network scenarios, OTN network resources
including tributary slots are managed explicitly by network operators for
network maintenance considerations. Therefore, an OTN slice controller
shall support configuring an OTN slice with both options.

   An OTN slice controller (OTN-SC) is a logical function responsible for
   the life-cycle management of OTN slices instantiated within the 
   corresponding OTN network domains. The OTN-SC provides technology-specific 
   interfaces at its northbound (OTN-SC NBI) to allow a higher-layer slice 
   controller, such as an IETF network slice controller (NSC) or an orchestrator, 
   to request OTN slices with OTN-specific 
   requirements. The OTN-SC interfaces at the southbound using the MDSC-to-PNC 
   interface (MPI) with a Physical Network Controller (PNC) or Multi-Domain Service Orchestrator (MDSC),
   as defined in the ACTN control framework {{!RFC8453}}. The logical function 
   within the OTN-SC is responsible for translating the OTN slice requests 
   into concrete slice realization which can be understood and 
   provisioned at the southbound by the PNC or MDSC.
   
   The presence of OTN-SC provides multiple options for a high-level slice controller 
   or an orchestrator to configure and realize slicing in OTN networks, depending on
   whether a customer's slice request is technology agnostic or technology specific:
    
   Option 1\[opt.1]: An IETF NSC receives a technology-agnostic slice request from the IETF NSC NBI and
   realizes full or part of the slice in OTN networks directly through MPI provided by
   the PNC or MDSC. The IETF NSC is responsible for mapping a technology-agnostic slicing request 
   into an OTN technology-specific realization. In this option, the OTN-SC is not used.
   
   Option 2\[opt.2]: An IETF NSC receives a technology-agnostic slice request from the IETF NSC NBI and delegates the
   request to the OTN-SC through the OTN-SC NBI, which is OTN technology specific. The OTN-SC in turn realizes the slice in single or multi domain OTN networks by working with the underlying PNC or MDSC. In this option, the OTN-SC is considered as a realization of IETF NSC, i.e.,
   an NS realizer as per {{!I-D.draft-contreras-teas-slice-controller-models}},
   when the underlying network is OTN. The OTN-SC is also a subordinate slice controller of the IETF NSC, which 
   is consistent with the hierarchical control of slices defined by the IETF network slice framework.
   
   Option 3\[opt.3]: An OTN-aware orchestrator may request an OTN technology-specific slice with OTN-specific SLOs through the 
   OTN-SC NBI to the OTN-SC. The OTN-SC in turn realizes the slice in single or multi domain OTN networks by working with the underlying PNC or MDSC
   
   An OTN slice may be realized by using standard MPI interfaces, control plane, network management system (NMS) or any other proprietary interfaces as needed. Examples of such interfaces include the abstract TE topology {{!RFC8795}}, TE tunnel {{!I-D.ietf-teas-yang-te}},L1VPN{{!RFC4847}}, or Netconf/YANG based interfaces such as OpenConfig. Some of these interfaces, such as the TE tunnel model, are suitable for creating connectivity-based OTN slices which represent a slice as a set of TE tunnels, while other interfaces such as the TE topology model are more suitable for creating resource-based OTN slices which represent a slice as a topology.
   
   The OTN-SC NBI is a technology-specific interface that augments the IETF NSC NBI, which is technology-
   agnostic. 
   
   {{fig-slice-interfaces}} illustrates the OTN slicing control hierarchy 
   , the positioning of the OTN slicing interfaces as well as the options for OTN slice configuration.


~~~~
                      +--------------------+
                      | Provider's User    |
                      +--------|-----------+
                               | CMI
       +-----------------------+----------------------------+
       |          Orchestrator / E2E Slice Controller       | 
       +------------+-----------------------------+---------+
                    |                             | NSC-NBI
                    |       +---------------------+---------+
                    |       | IETF Network Slice Controller |
                    |       +-----+---------------+---------+
                    | opt.3       | opt.2         | opt.1
                    | OTN-SC NBI  |OTN-SC NBI     |               
       +------------+-------------+--------+      |
       |               OTN-SC              |      |
       +--------------------------+--------+      |      
                                  | MPI           | MPI                           
       +--------------------------+---------------+---------+ 
       |                         PNC                        | 
       +--------------------------+-------------------------+ 
                                  | SBI
                      +-----------+----------+
                      |OTN Physical Network  |
                      +----------------------+

~~~~
{: #fig-slice-interfaces title="Positioning of OTN Slicing Interfaces"}

   OTN-SC functionalities may be recursive such that a higher-level
   OTN-SC may designate the creation of OTN slices to a lower-level
   OTN-SC in a recursive manner. This scenario may apply to the
   creation of OTN slices in multi-domain OTN networks, where 
   multiple domain-wide OTN slices provisioned by lower-layer
   OTN-SCs are stitched to support a multi-domain OTN slice
   provisioned by the higher-level OTN-SC.  Alternatively, the OTN-SC
   may interface with an MDSC, which in turn interfaces with multiple 
   PNCs through the MPI to realize OTN slices in multi-domain OTN networks 
   without OTN-SC recursion. 
   {{fig-otn-sc-recursion}} illustrates both options for OTN slicing
   in multi-domain.

~~~~
    +-------------------+                    +-------------------+
    |      OTN-SC       |                    |      OTN-SC       |
    +--------|----------+                    +---|----------|----+
             |MPI                                |OTN-SC NBI|
    +--------|----------+                    +---|----+ +---|----+
    |      MDSC         |                    | OTN-SC | | OTN-SC |
    +---|----------|----+                    +---|----+ +---|----+
        |MPI       |MPI                          |MPI       |MPI
    +---|----+ +---|----+                    +---|----+ +---|----+
    |   PNC  | |   PNC  |                    |   PNC  | |   PNC  |
    +--------+ +--------+                    +--------+ +--------+
    Multi-domain Option 1                    Multi-domain Option 2
~~~~
{: #fig-otn-sc-recursion title="OTN-SC for multi-domain"}

   OTN-SC functionalities are logically independent and may be deployed in 
   different combinations to cater to the realization needs. In reference to the 
   ACTN control framework {{!RFC8453}}, an OTN-SC may be deployed

   - as an independent network function;

   - together with a Physical Network Controller (PNC) for single-domain
      or with a Multi-Domain Service Orchestrator (MDSC)for multi-domain;

   - together with a higher-level network slice controller to support 
      end-to-end network slicing;

# Realizing OTN Slices

{{!I-D.ietf-teas-ietf-network-slices}} introduces a mechanism for an IETF network slice controller to realize network slices by constructing Network Resource Partitions (NRP). A NRP is a collection of resources identified in the underlay network to facilitate the mapping of network slices onto available network resources. An NRP is a scope view of a topology and may be considered as a topology in its own right. Thus, in traffic-engineered (TE) networks including OTN, an NRP may be simply represented as an abstract TE topology defined by {{!RFC8795}}. For OTN networks, An NRP may be represented as an abstract OTN topology defined by {{!I-D.ietf-ccamp-otn-topo-yang}}.

The NRP may be used to address the scalability issues where there may be considerable numbers of control and data plane states required to be stored and programmed if network slices are mapped directly to the underlay topology. NRP is internal to a network slice controller, and use of NRPs is optional yet could benefit a network slice realization in large-scale networks, including OTN networks. 

For connectivity-based OTN slices, a connection within an OTN slice is typically realized by an OTN tunnel in the underlay topology and resources are reserved by the tunnel, thus use of NRP is optional in this case.

For resource-based OTN slices, the OTN-SC may map an OTN slice directly onto the underlay TE topology presented by the subtended network controller (MDSC or PNC) without creating NRP topologies. Due to the need for reserving resources, the OTN-SC needs to color corresponding link resources of the underlay topology with a slice identifier and maintain the coloring to keep track of the mapping of OTN slices. The OTN-SC may push the colored topology to the subtended MDSC or PNC using the MPI model defined in this draft.

Alternatively, an OTN slice may be mapped to a NRP as an overlay abstract OTN TE topology on top of the underlay topology. The corresponding link resources allocated to the slice is encapsulated in and tracked by the abstract topology, and a given link or port in the NRP topology represents resources that are reserved in the underlay topology. Multiple OTN slices may be mapped to the same NRP, and a single connectivity construct of the slice may be mapped to only one NRP, as per {{!I-D.ietf-teas-ietf-network-slices}}. The resources of an NRP topology are reserved and shared by all the OTN slices mapped to this NRP, and the NRP topology may be pushed directly to the subtended MDSC or PNC, thus eliminating the need for link coloring if using the underlay topology.

{{fig-otn-sc-nrp}} illustrates the relationship between OTN slices and NRP.

~~~~
        /---------------/      |            /---------------/
       /  --     --    /       |           /  --     --    /
      /  |N1|---|N3|  /---/    |          /  |N2|   |N3|  /
     /    --\    --  /   /     |         /    --     --  /
    /        \--    /   /      |        /       \ --/   /
   /         |N2|  /   /       |       /         |N4|  /
  / Slice 1   --  /   /        |      / Slice 2   --  /
 /------------<--/   /         |     /-----------<---/
    / Slice 3 <     /          |                 <
   /--------- <-<--/           |                 <
+-------------<-<--------------V-----------------<------------+
|          /--<-<------------/             /-----<-----------/|
|         / /--\     /--\   /             /          /--\   / |
|        / |NE1 |---|NE2 | /             /          |NE2 | /  |
|       /   \--/\  . \--/ /             /            \--/ /   |
|      /       ..\ ........            /              /. /    |
|     /        . /--\   / .           / /--\     /--\/ ./     |
|    /         .|NE4 | /  .          / |NE3 |---|NE4 | .      |
|   /          . \--/ /   .         /   \--/  .  \--/ /.      |
|  /  NRP Topology 1 /    .        /  NRP Topology 2 / .      |
| /------------.----/     .       /-----------.-----/  .      |
|              .......    .                   .        .      |
|             /------.----.-----------------/ .        .      |
|            / /--\  .    .     /--\       /  .        .      |
|           / |NE1 |-.----v----|NE2 |     /   .        .      |
|          /   ---/\ .         /\--/     /    .        .      |
|         /   /     \v        /<........................      |
|        / /-/\      \ /--\  /         /      .               |
|       / |NE3 |------|NE4 |/         /       .               |
|      /   \--/  ^     \--/          /        .               |
|     /  Underlay.OTN TE Topology   /         .               |
|    /-----------.-----------------/          .               |
|                ..............................     OTN-SC    |
+-------------------------------------------------------------+
                 |                         ^
                 |MPI                      |MPI
+----------------V--------------------------------------------+
|                                                             |
|                       OTN MDSC/PNC                          |
+-------------------------------------------------------------+
~~~~
{: #fig-otn-sc-nrp title="Mapping OTN Slices to NRP"}

# YANG Data Model for OTN Slicing Configuration

## OTN Slicing YANG Model for MPI

### MPI YANG Model Overview

   For the configuration of connectivity-based OTN slices, existing models such as 
   the TE tunnel interface {{!I-D.ietf-teas-yang-te}} may be used and no addition is 
   needed. This model is addressing the case for configuring resource-based OTN slices,
   where the model permits to reserve resources exploiting the common knowledge of an underlying 
   virtual topology between the OTN-SC and the subtended network controller (MDSC or PNC). The slice
   is configured by marking corresponding link resources on the TE topology received from the 
   underlying MDSC or PNC with a slice identifier and OTN-specific resource requirements, 
   e.g. the number of ODU time slots or the type/number of ODU containers. The MDSC or PNC, based on the 
   marked resources by the OTN-SC, will update the underlying TE topology with new TE link for each of 
   the colored links to keep booked the reserved OTN resources e.g. time slots or ODU containers.

### MPI YANG Model Tree

~~~~
{::include ./ietf-otn-slice-mpi.tree}
~~~~
{: #fig-otn-slice-mpi-tree title="OTN slicing MPI tree diagram"}

### MPI YANG Code

~~~~
   <CODE BEGINS> file "ietf-otn-slice-mpi@2022-10-12.yang"
{::include ./ietf-otn-slice-mpi.yang}
   <CODE ENDS>
~~~~
{: #fig-otn-slice-mpi-yang title="OTN slicing MPI YANG model"}

## OTN Slicing YANG Model for OTN-SC NBI

### NBI YANG Model Overview

   The YANG model for OTN-SC NBI is OTN-technology specific, but shares many
   common constructs and attributes with the common network slicing YANG model
   defined in {{?I-D.ietf-teas-ietf-network-slice-nbi-yang}}. Furthermore, the 
   OTN-SC NBI YANG is expected to support both connectivity-based
   and resource-based slice configuration, which is likely a common requirement for
   supporting slicing at other transport network layers, e.g. WDM or MPLS(-TP).
   
   The OTN slicing model augments the common network slicing YANG model by extending
   OTN technology-specific SLO and SLE attributes which can be requested by OTN-aware
   customers and allows the customer to specify desired OTN signal quality. 
   These attributes include:
   
   - The performance objective for Optical Data Unit (ODU) containers as defined in
     ITU-T-G.8201-Amd.1.
   
   - Bandwidth specification in the type and number of ODU containers.

### NBI YANG Model Tree for OTN slice

~~~~
{::include ./ietf-otn-slice.tree}
~~~~
{: #fig-ietf-otn-slice title="Tree diagram for OTN slice"}

### NBI YANG Code for OTN Slice

~~~~
   <CODE BEGINS> file "ietf-otn-slice@2023-07-06.yang"
{::include ./ietf-otn-slice.yang}
   <CODE ENDS>
~~~~
{: #fig-ietf-otn-slice-yang title="YANG model for transport network slice"}   
  
# Manageability Considerations

   To ensure the security and controllability of physical resource
   isolation, slice-based independent operation and management are
   required to achieve management isolation.
   Each optical slice typically requires dedicated accounts,
   permissions, and resources for independent access and O&M. This
   mechanism is to guarantee the information isolation among slice
   tenants and to avoid resource conflicts. The access to slice
   management functions will only be permitted after successful security
   checks.

# Security Considerations

   The YANG module specified in this document defines a schema for data
   that is designed to be accessed via network management protocols such
   as NETCONF {{!RFC6241}} or RESTCONF {{!RFC8040}}.  The lowest NETCONF layer
   is the secure transport layer, and the mandatory-to-implement secure
   transport is Secure Shell (SSH) {{!RFC6242}}.  The lowest RESTCONF layer
   is HTTPS, and the mandatory-to-implement secure transport is TLS
   {{!RFC8446}}.

   The NETCONF access control model {{!RFC8341}} provides the means to
   restrict access for particular NETCONF or RESTCONF users to a
   preconfigured subset of all available NETCONF or RESTCONF protocol
   operations and content.

   There are a number of data nodes defined in this YANG module that are
   writable/creatable/deletable (i.e., config true, which is the
   default).  These data nodes may be considered sensitive or vulnerable
   in some network environments.  Write operations (e.g., edit-config)
   to these data nodes without proper protection can have a negative
   effect on network operations.  Considerations in Section 8 of
   {{!RFC8795}} are also applicable to their subtrees in the module defined
   in this document.

   Some of the readable data nodes in this YANG module may be considered
   sensitive or vulnerable in some network environments.  It is thus
   important to control read access (e.g., via get, get-config, or
   notification) to these data nodes.  Considerations in Section 8 of
   {{!RFC8795}} are also applicable to their subtrees in the module defined
   in this document.

# IANA Considerations

   It is proposed to IANA to assign new URIs from the "IETF XML
   Registry" {{!RFC3688}} as follows:

~~~~
   URI: urn:ietf:params:xml:ns:yang:ietf-otn-slice
   Registrant Contact: The IESG
   XML: N/A; the requested URI is an XML namespace.

   URI: urn:ietf:params:xml:ns:yang:ietf-otn-slice-mpi
   Registrant Contact: The IESG
   XML: N/A; the requested URI is an XML namespace.
~~~~

   This document registers two YANG modules in the YANG Module Names
   registry {{!RFC6020}}.

~~~~
   name: ietf-otn-slice
   namespace: urn:ietf:params:xml:ns:yang:ietf-otn-slice
   prefix: otns
   reference: RFC XXXX

   name: ietf-otn-slice-mpi
   namespace: urn:ietf:params:xml:ns:yang:ietf-otn-slice-mpi
   prefix: otns-mpi
   reference: RFC XXXX
~~~~

--- back

{: numbered="false"}

# Acknowledgments

   This document was prepared using kramdown.

   Previous versions of this document were prepared using 2-Word-v2.0.template.dot.

   The authors would like to thank Adrian Farrel, Danielle Ceccarelli,
   Igor Bryskin, Bo Wu, Gyan Mishra, Joel M. Halpen, Dhruv Dhoddy and Loa Andersson
   for providing valuable insights.
