---
layout: post
title: Zero trust networking
---

Within this blogpost I want to give a brief explaination as to what zero trust networking is and how it compares to traditional networks. 

### [Perimeter networks vs. zero trust networks.](#perimeter-versus-zero-trust)
In traditional networking it is assumed that the DMZ (or Demilitarized Zone) is the untrusted zone that connects to the (public) internet. This is where you would place the external webservers, 
vpn gateways and other servers/services that have to connect to the public internet. This network model is also called the perimeter model.
The original network security architecture places different devices into different zones, which are separated by one or more firewalls. Each zone has its own level of trust, 
and it indicates what resources can be accessed. Furthermore, it allows the resources to be monitored and controlled respondingly. 
Within zero trust networking, the setup of the network is different compared to traditional networks and networking, because it relies on different fundamental assertions.
First of all: Every host, and the network itself are considered hostile. External and internal threats are assumed to exist on the network at all times.

For zero trust networking, network locality or a specific zone is not sufficient for deciding trust in a network. Every device, user, and network flow is authenticated and authorized.
This also means that a VPN is suddenly rendered obsolete, along with several other modern network constructs when looking at them from the traditional perspective. 
This does however mandate pushing the enforcement of Zero trust network as far toward the network edge as possible. 
Modern stateful firewalls in all major operating systems, and network equipment have the advanced capabilities to enable these features at the edge.

Requests for access to protected resources are first made through the controller, where both the device and user must be authenticated and authorized.*
Once the controller has decided that the request will be allowed, it dynamically configures the artifact to accept traffic from that user or device only. 
In addition, the controller can also coordinate the details of an encrypted tunnel between the requestor and the resource. 
This can include temporary one-time-use credentials, keys, and ephemeral port numbers. In order to get correct results, the policies must be dynamic and calculated from as many sources of data as possible.

### [Implementation](#implementation)
Zero trust networks do not require new protocols or libraries, because they build upon existing technologies in novel ways. Automation plays a big part in what allows the zero trust network to operate.
The reason for this is that if the policy enforcement cannot be dynamically updated, zero trust will be unattainable; therefore it is critical that this process be automatic and rapid.


### [Finishing Note](#finishing-note)
For more information, I can highly recommend "Zero Trust Networks: Building secure systems in untrusted networks" by Evan Gilman and Doug Barth.



*When you are applying access control incorrectly or reuse credentials, you lose a lot of the benefits of zero trust networking. This requires a certain level of security already.