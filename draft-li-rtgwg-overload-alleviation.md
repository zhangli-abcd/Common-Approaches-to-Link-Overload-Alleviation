---
title: "Common Approaches to Link Overload Alleviation"
abbrev: "Overload Alleviation"
docname: draft-li-rtgwg-overload-alleviation-latest

stand_alone: true
ipr: trust200902
area: "Routing"
workgroup: "Routing Area Working Group"
keyword:
 - congestion
 - congestion mitigation
 - bandwidth
 - link capacity
 - traffic steering
 - segment routing

category: info
submissiontype: IETF
coding: utf-8
pi: [toc, sortrefs, symrefs]

venue:
  group: "Routing Area Working Group"
  type: "Working Group"
  mail: "rtgwg@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/rtgwg/"
  github: "zhangli-abcd/Common-Approaches-to-Link-Overload-Alleviation"

author:

-
  ins: L. Zhang
  name: Li Zhang
  org: Huawei
  country: China
  email: zhangli344@huawei.com

-
  ins: A. Farrel
  name: Adrian Farrel
  org: Old Dog Consulting
  email: adrian@olddog.co.uk

-
  ins: P. Psenak
  name: Peter Psenak
  org: Cisco Systems
  email: ppsenak@cisco.com

informative:

  ISO10589:
    title: Intermediate system to Intermediate system intra-domain routeing information exchange protocol for use in
           conjunction with the protocol for providing the connectionless-mode Network Service (ISO 8473),
           ISO/IEC 10589:2002, Second Edition. <https://www.iso.org/standard/30932.html>
    date: 2002
    author:
      - org: "International Organization for Standardization"

  LCM:
    title: Local Congestion Mitigation. <https://www.cisco.com/c/en/us/products/collateral/cloud-systems-management/crosswork-network-automation/local-congestion-mitigation-wp.html>
    date: 2021
    author:
      - org: "Cisco Systems Inc."

--- abstract

Network links may become degraded because of physical conditions or owing to partial link failure: this can result in a reduction of available bandwidth. Alternatively, traffic volume may increase significantly and unexpectedly. These circumstances may lead to links becoming overloaded which can result in dropped packets or degraded delivery such as increased delay.

Traffic adjustment techniques can be applied to alleviate these link-overload situations by steering some of the traffic onto other paths that are less loaded and have available bandwidth.

This document examines the scenarios in which links can become overloaded, the requirements for steering traffic to reduce link-overload, and the techniques that can be applied to alleviate overloaded links.

--- middle

# Introduction {#sec-intro}

Network links may become degraded because of physical conditions or owing to partial link failure. For example, a microwave link can see a reduction in available bandwidth during inclement weather conditions. Alternatively, changes in the available network resources may directly result in link overload or could cause traffic to be rerouted within the network leading to links becoming overloaded. These situations may result in the traffic volume on the link exceeding the available bandwidth.

At the same time, traffic volume may increase significantly and unexpectedly above predicted levels such that traffic volume on the link exceeds the available bandwidth.

Any of these circumstances may lead to links becoming overloaded which can result in dropped packets or degraded delivery, such as increased delay.

Traffic adjustment techniques (such as Traffic Engineering {{?RFC9522}}, Flex-Algo {{?RFC9350}}, and LFA {{?RFC5286}}) can be applied to alleviate these link-overload situations by steering traffic onto other paths (that is, using other links) that are less loaded and have available bandwidth.

This document examines the scenarios in which links can become overloaded, the requirements for steering some of the traffic to reduce link-overload, and the techniques that can be applied to alleviate overloaded links.

# Problem Description {#sec-problem}

Links may become overloaded when more traffic is dispatched to the link than can be transmitted on the link, or when more traffic arrives at the egress end of the link than can be processed immediately or buffered without the buffers/queues becoming full. If a link is overloaded, traffic (that is packets) will be discarded, and even those packets that are not discarded may be delayed by unacceptable amounts.

Link overload may arise when traffic flows that comprise more bits per second than the link has capacity for are routed or steered onto a link. This may happen because of normal shortest path routing, errors in traffic engineering planning, flexible bandwidth mechanisms, poor policing of flows at the network edge, or recovery from network failure conditions. Such overload may be short-term (for example, quick bursts of traffic) or may be longer-lasting. Short-term overload may be detected, notified, and rectified by congestion notification and mitigation mechanisms (see {{sec-congestion}}), but more permanent overload situations need more strategic solutions.

Various solutions (see {{sec-approaches}}) provide mechanisms to detect and alleviate link overload. The objectives are to determine when traffic load reaches a threshold, to notify the situation, and to steer traffic so that it takes other acceptable paths (that is, not resulting in excessive path cost, delay, etc.), but balances the traffic load in the network so that no link is overloaded and that traffic load remains below loading thresholds on all links where that is possible.

Ideally, when the traffic load on any previously overloaded link drops below a second threshold, traffic will revert to the originally preferred path.

## Definition of Link Overload {#sec-overload}

A link is considered to be overloaded when the amount of traffic (measured in bits per second) has reached a configured threshold on the link. The traffic is usually measured over a sample period that allows short bursts to not trigger the link to be considered as overloaded. The threshold can be set as a percentage of the capacity of the link or as an absolute value. In the absence of a configured threshold, a link will be considered overloaded when the link is full, i.e., when the amount of traffic is equal to the capacity of the link. When a link is full it is likely that traffic will be dropped, and that transmitted traffic may be delayed.

By setting the configured threshold appropriately, a router may detect an increase in traffic levels before the link is full and may take action to alleviate the link overload, thus preventing any impact on the traffic.

Except when referring to congestion as defined in {{sec-congestion}}, this document uses the term "link overload". Note that "Distributed Congestion Mitigation" (DCM) discussed in {sec-approach-DCM} is a prior term, but is described in this document in terms of link overload.

## Link Overload Alleviation {#sec-alleviate}

Link overload alleviation involves redirecting (steering) traffic so that it takes another path that avoids the overloaded link. It should be careful to not cause secondary overload on links of the other paths.

## Required Responsiveness {#sec-required}

While, "As soon as possible," is always a good target for alleviating network overload, the situation is not regarded as highly urgent partly because the threshold triggering action can be set to rectify the situation before serious situations occur, and because it can be that only "best-effort" traffic will be affected.

Link overload alleviation can be considered as a traffic planning or optimization activity, and the target response times are in the order of thirty seconds. This should be factored into how the overload threshold is configured so that there is no immediate urgency to alleviating the overload.

## Traffic Reversion {#sec-reversion}

It may be assumed that the path originally taken by the traffic was preferred because it was shorter, more cost-effective, better protected, lower delay, etc. Thus, in order to alleviate link overload, traffic has been steered onto an equally or less preferred path. It follows that, if the link overload situation has been alleviated, it may be desirable to revert traffic back to its original path.

In order to avoid flip-flop of traffic from one path to another, it is important that the link-no-longer-overloaded state involves a threshold markedly lower than the link-overloaded threshold. Further, reversion of traffic to its original path should be subject to local and network-wide policies such as delays (hold-off timers).

Note that switching traffic from one path to another may introduce some disruption (for example, out of order packet delivery, or jitter) and even risks delivery failure.

## Comparison with Link Failure {#sec-failure}

It is worth noting that link failures are an extreme version of link overload. When a link fails, it is equivalent to the link suddenly having no available bandwidth.

There are plenty of available techniques for mitigating link failure. These include:

- IGP routing convergence {{?RFC4750}}, {{?RFC5340}}, {{ISO10589}}
- End-to-end protection in MPLS-TE networks {{?RFC4427}}
- Segment protection in MPLS-TE networks {{?RFC4427}}
- Fast Reroute (FRR) for IP {{?RFC7841}}, MPLS-TE {{?RFC4090}}, or Segment Routing (SR) {{?RFC9855}}.

Solutions for link failure cannot be directly applied to link overload scenarios since they switch all the traffic to the backup paths, whereas link overload does not generally require redirecting all of the traffic. However, it may provide a basis for, or conceptual input to, solutions for link overload.

## Discussion of Transport-Level Congestion Control {#sec-congestion}

Congestion detection, reporting, control, and mitigation have been the subject of RFCs since the earliest days of the IETF. In general, congestion notification aims to be very responsive to, or even take place in advance of, the congestion actually becoming a problem. Early Congestion Notification (ECN) {{?RFC3180}} aims to provide information to the head end of network flows so that they can moderate the rate at which traffic is presented to the network and so avoid congestion harming those traffic flows. The assumption is that notified traffic sources will behave reasonably in their own best interests (reduced throughput is preferable to recovering from dropped packets) and in the best interests of the network as a whole (i.e., not deciding that other sources will back off and increasing traffic to use the capacity they make available).

Over the years, many other TCP/IP congestion notification and remedial techniques have been proposed and some have been standardised. These predominately operate at the transport level, moderating the rate at which traffic is delivered to the network in order to avoid or mitigate congestion. Response time can be as short as detection time plus notification time, plus time for in-transit packets to reach the point of congestion: effectively the detection time plus the round-trip time.

Active Queue Management (AQM) {{?BCP197}} is a method that allows network devices to control the queue length or the mean time that a packet spends in a queue. By carefully calibrating queuing behaviors, network nodes are able to mitigate high traffic levels (in particular traffic bursts) and so reduce the effects.

The problems discussed in this document (see {{sec-problem}}) are similar to those that have been addressed previously (i.e., link overload is equivalent to congestion on that link). The use cases (see {{sec-applicability}}) are somewhat similar, but the requirements, solutions, and espescially the responsiveness (see {{sec-required}}) are different.

# Functional Model {#sec-functional-model}

The functional model is split into abstract functional actions and key action points that realise those actions. It may be observed that the different approaches described in {{sec-approaches}} take link overload alleviation measures at different places in the network and so might not require all of the functional actions to be performed.

## Functional Actions {#sec-funct-acts}

Configuration:
: The capacity of each link may be known a priori or configured at the link ends. The overload and reversion thresholds need to be configured to the link ends.

Capacity-Aware Topology Database Construction:
: The network topology is supplemented by the capacities of all of the links. This may be distributed across the network in the TE-enhanced IGP or collected to a centralized controller using management protocols.

Measurement:
: In order to determine the load on any link, the link ends measure the traffic over a period of time designed to dampen any peaks caused by short bursts of traffic.

Capacity-Aware topology Database Construction:
: To correctly balance traffic and avoid overloading other links, it is important that the load on other links be known across the network. Depending on the solution approach taken, this information may be needed just locally within the network (for example, a router may need to know the loads on links one or two hops away) or may be needed with wider vision to enable network-wide load distribution. As with the construction of the capacity-aware topology database, this may be achieved by leveraging TE-enhanced IGPs, link affinities of Flex-Algo, or through reporting to a central controller.

Overload Trigger:
: The overload condition is triggered when when the load on a link crosses a configured threshold. This may be an absolute value or a percentage of the link capacity.

Overload Notification:
: Depending on where the overload alleviation is to be performed and where the overload condition is triggered, it may be necessary to send an overload notification so that remedial action can be taken. For example, in controller-based overload alleviation scenario, the centralized controller performs the alleviation process. In this case, the node with an overloaded link needs to send an overload notification to the controller.

Traffic Steering:
: To alleviate link overload, traffic is steered away from the overloaded link onto links that have less load. This is achieved by using tunnelling techniques that place the traffic onto a path that it would not normally follow or by modifying the next hop routing/forwarding decisions. Only a part of the total traffic is steered onto an alternate path so as to keep the overloaded link in use while reducing the load it carries.

## Key Action Points {#sec-funct-points}

The functional actions described in the previous section can be enacted at key points in the network.

Traffic source:
: This is where traffic for a particular flow or set of flows enters the network. Flows are routed or steered from the traffic source to the traffic sink.

Traffic sink:
: This is where traffic for a particular flow or set of flows leaves the network.

Link head end:
: In the direction of traffic flow, the link head end is where traffic enters the link. In the context of IP networks, the link head end is a router or a host.

Link tail end:
: In the direction of traffic flow, the link tail end is where traffic exits the link. In the context of IP networks, the link tail end is a router or a host.

Point of local alleviation:
: A point of local alleviation is a router that steers traffic onto an alternate path to avoid an overloaded link. Such a router is (of course) on the path of the traffic that will be steered. Further, the router is upstream of the overloaded link.
: Where tunnelling is used to steer the traffic onto the alternate path, the point of local alleviation is the head end of the tunnel.

Merge point:
: The merge point is a router or host where traffic that has been steered to avoid the overloaded link re-joins the original path.
: Where tunneling is used to steer the traffic onto the alternate path, the merge point is the tail end of the tunnel.

Central controller:
: A central controller, such as a Path Computation Element (PCE) {{?RFC4655}}, is responsible for determining optimal paths for traffic within the network. Where traffic is to be steered onto alternate paths to alleviate link overload, the central controller may be used to compute those alternate paths.

# General Approaches {#sec-approaches}

This section outlines five approaches to link overload alleviation that have been proposed and experimentally implemented. While the overall objectives are the same in each case, the approaches vary somewhat.

## Distributed Congestion Mitigation {#sec-approach-DCM}

Distributed Congestion Mitigation (DCM) described in {{?I-D.psenak-lsr-igp-dcm}} is a distributed and integrated IGP mechanism to mitigate link overlaod. Its primary objective is to dynamically offload traffic from local overloaded links onto less loaded alternate paths in Offloading Flex Algo (OFA) topologies.

In DCM, each router continuously monitors local link utilization. When link utilization exceeds the configured "Congestion Threshold", the router advertises "Congestion Affinity" via IGP link attributes, which causes the overloaded link to be excluded from a dedicated OFA topology. Additionally, the router also performs traffic offloading to divert the traffic onto the shortest path that avoids any overloaded links. A separate IGP link attribute, "High Utilization Affinity" signals routers participating in the IGP to stop sending new offloaded traffic to the link without impacting existing offloaded traffic. DCM leverages Unequal Cost Multipath (UCMP) to divert traffic from the primary to the offload path (i.e., to offload traffic) in progressively periodic iterations. When link utilization on the overloaded link falls below a configured "Restore Threshold", traffic is gradually moved back (reverted) to the original primary path.

DCM depends on IGP (IS-IS or OSPF) for affinity advertisement, and IGP Flexible Algorithm ({{?RFC9350}}) to construct the OFA topology. Participating nodes must implement threshold based link state signaling and obey IGP LSP/LSA update throttling rules.

## Elastic Bandwidth-aware Routing {#sec-approach-EBR}

Elastic Bandwidth aware Routing (EBR) specified in {{?I-D.czz-rtgwg-elastic-bandwidth-routing}}, is a distributed dynamic mechanism to alleviate link overload that responds rapidly to unexpectedly overloaded links in the network before centralized TE completes global re-optimization. Its core goal is to mitigate in a timely way link overload that was triggered by unexpected reasons by distributing traffic among the shortest paths and load-balancing alternate paths through Segment Routing Traffic Engineering (SR-TE) {{?RFC9256}}.

In EBR, each router monitors local link bandwidth usage and advertises available bandwidth and utilization metrics via extensions to IGP-TE when specific conditions and thresholds are met. Then, each router pre-computes loop-free, preferably disjoint, load-balancing alternate paths for each destination. When the link utilization exceeds the configured "Congestion Threshold", the router uses UCMP to divert specific flows to alternate paths. The diverted traffic is encapsulated using an SR-TE strict path for loop-free forwarding. Path weights are derived from the bottleneck link on each alternate path. Traffic fallback (reversion) is performed when bandwidth utilization on the originally overloaded link drops below a dynamic "Restore Threshold" to restore flows to primary paths.

EBR depends on IGP (IS-IS or OSPF) with existing TE metric extensions ({{?RFC8570}} for IS-IS, {{?RFC7471}} for OSPF) for bandwidth information dissemination. SR-TE data plane capability is mandatory for steering offloaded traffic.

## Tactical Traffic Engineering {#sec-approach-TTE}

Tactical Traffic Engineering (TTE), described in {{?I-D.li-rtgwg-tte}}, is a distributed, reactive traffic engineering mechanism designed to rapidly alleviate unexpected link overload. TTE operates locally, adjusting the forwarding of selected traffic flows in response to actual link utilization, without requiring immediate changes to the underlying routing topology or traffic engineering paths. It is complementary to conventional bandwidth engineering mechanisms, separating tactical mitigation from strategic optimization to allow networks to react quickly to unforeseen traffic demands without replacing their existing bandwidth management architecture.

When overload is anticipated or detected, TTE selectively distributes traffic across primary and alternate forwarding paths. TTE is independent of the underlying traffic engineering architecture, and alternate paths may be derived from existing protection mechanisms, including Loop-Free Alternates (LFA) {{?RFC5286}}, Topology Independent Loop-Free Alternates (TI-LFA) {{?RFC9855}}, RSVP-TE Fast Reroute {{?RFC4090}}, or paths computed by a Path Computation Element (PCE) {{?RFC4655}}. TTE activates these alternatives through modifications to Forwarding Information Base (FIB) or MPLS Label Forwarding Information Base (LFIB) entries, allowing traffic to be redistributed without requiring network-wide routing convergence or global traffic engineering reoptimization.

TTE must avoid overloading alternate paths, to maintain network and forwarding stability. When utilization returns to acceptable levels, or when longer-term traffic engineering optimization has corrected the overload condition, TTE can deactivate the alternate paths and restore normal forwarding.

## Congestion Avoidance using Segment Routing {#sec-CA-SR}

While many approaches to overload alleviation may make use of Segment Routing to tunnel traffic around overloaded links, {{?I-D.he-rtgwg-sr-ca}} specifically utilizes this mechanism and is applicable only in an SR-enabled network.

Based on the concept of Topology Independent Loop-Free Alternate (TI-LFA) Fast Reroute (FRR) (see {{sec-failure}}), the traffic can be steered away from an overloaded link while avoiding the formation of micro-loops by moving the traffic to the bypass paths that have been predetermined for link-failure protection.

This approach requires no new protocol extensions, and switches traffic based on its destination address so that it is possible that large amounts of traffic are steered away from an overloaded link in one action. As with normal FRR procedures, this mechanism is applicable both to immediate adjacency overload and to remote adjacency overload.

## Local Congestion Management {#sec-approach-LCM}

Local Congestion Mitigation (LCM) specified in {{LCM}} is a controller-based tactical solution to mitigate link overload, designed to handle short-lived transient link overload while complementing capacity planning. LCM mitigates link overload by diverting the minimal necessary amount of traffic away from the overloaded interface to bring it out of overload state. LCM acts as a tactical complement to capacity planning and network engineering. It supports monitor-only and human-in-the-loop workflows for operational safety.

LCM operates as a closed-loop controller-driven workflow. Crosswork Data Gateway (CDG) collects interface-level statistics from network elements. The controller maintains real-time network state and detects when a link is overloaded when measured utilization exceeds user configured thresholds. It computes how much optimizable traffic volume needs to be offloaded to relieve the link overload. A Segment Routing Path Computation Element (SR PCE) calculates suitable alternate paths and deploys temporary tactical SR-TE policies on headend nodes. ECMP splits traffic across multiple parallel tactical SR-TE policies to achieve the target offloaded volume. After the link has become less loaded and once hold-margin conditions are satisfied, the controller removes temporary tactical policies and reverts traffic back to native IGP forwarding to avoid persistent policy churn.

LCM relies on BGP-LS {{?RFC9552}} or the active IGP to collect real-time topology information. PCEP {{?RFC5440}} is required between the SR PCE and PCC routers for installation and removal of PCE-initiated SR-TE policies. gRPC or SNMP is used for statistics telemetry collection. Headend routers must support PCE-initiated SR-TE policies with autoroute steering and ECMP over multiple parallel SR-TE policies.

# Applicability to Different Scenarios {#sec-applicability}

The approaches described in {{sec-approaches}} share the common objective of alleviating link overload, but they differ significantly in their architectural assumptions, deployment requirements, and operational characteristics. This section identifies the scenarios in which each approach is most applicable.

## Scenario 1: Distributed Alleviation with Flex-Algo Support {#sec-scenario-dcm}

Characteristics: The network operates a single IGP domain (IS-IS or OSPF) with Flexible Algorithm capabilities already deployed or easily enabled. The operator requires a fully distributed solution that does not depend on a central controller or external TE policy management.  The network can tolerate the overhead of a dedicated offload topology.

Applicable approach: Distributed Congestion Mitigation (DCM) {{sec-approach-DCM}}.

DCM is the natural fit when the network has already adopted IGP Flexible Algorithm {{?RFC9350}}. DCM provisions a dedicated Offloading Flex-Algo (OFA) topology that excludes congested links via affinity advertisements, allowing offloaded traffic to be natively routed around congestion without explicit TE tunnels. This makes DCM attractive for operators who want to avoid the configuration and state overhead of SR-TE policies on every headend.

Key deployment considerations: Requires IGP-wide support for Flex-Algo and extended administrative groups.

The OFA topology is shared across participating routers; scaling may be a concern if many distinct Flex-Algo typologies already exist.

## Scenario 2: Distributed Alleviation with SR-TE Capability {#sec-scenario-ebr}

Characteristics: The network operates a single IGP domain (IS-IS or OSPF) with Segment Routing (SR-MPLS or SRv6) capabilities already deployed or easily enabled.The operator requires a fully distributed solution that does not depend on a central controller.

Applicable approach: Elastic Bandwidth-aware Routing (EBR) {{sec-approach-EBR}}.

EBR is designed for scenarios where the network has already adopted IGP TE extensions and Segment Routing. Unlike DCM, which relies on topology exclusion, EBR computes alternate paths with explicit consideration of available bandwidth on each path, using UCMP weights derived from the bottleneck link on each alternate path. This makes EBR react quickly once the overload is detected. It leverages existing IGP TE extensions (RFC 8570 for IS-IS, RFC 7471 for OSPF) for bandwidth advertisement, meaning it can be deployed incrementally as IGP implementations support these extensions.

Key deployment considerations:

- SR-TE data plane capability is mandatory for steering offloaded traffic.

- The accuracy of bandwidth information (depends on advertisement frequency) and the pressure on the control plane are a trade-off.

## Scenario 3: Controller-Based Alleviation with Human Oversight {#sec-scenario-lcm}

Characteristics: The network is managed by a central controller and the operator requires visibility, auditability, and control over any traffic steering actions. Regulatory or operational policy requires human-in-the-loop approval for traffic engineering changes. The overload events are transient and localized, and the operator wants to offload the minimal necessary traffic volume to restore the link to acceptable utilization. Existing SR-TE policy infrastructure is already deployed or can be deployed.

Applicable approach: Local Congestion Mitigation (LCM) {{sec-approach-LCM}}.

LCM is the appropriate choice when operational safety and human oversight are paramount. It operates as a closed-loop controller-driven workflow: the controller collects interface statistics, detects overload against user-configured thresholds, computes the minimal offload volume, deploys temporary SR-TE policies, and later removes them once hold-margin conditions are satisfied. The monitor-only and human-in-the-loop workflows allow operators to preview recommendations before committing them.

Key deployment considerations:

- Requires BGP-LS or IGP for topology collection;

- Requires SR-TE policies and ECMP over multiple parallel policies.

## Scenario 4: Distributed Alleviation with Existing Alternate Paths (TTE) {sec-scenario-tte}

Characteristics: The network requires rapid, distributed mitigation of unexpected link overload and has suitable alternate forwarding paths available. The operator wishes to minimize additional control-plane activity and avoid dependence on network-wide routing convergence or centralized path reoptimization for the immediate corrective action. The network may use conventional IP routing, RSVP-TE, or Segment Routing.

Applicable approach: Tactical Traffic Engineering (TTE) {{sec-approach-tte}}.

TTE is particularly applicable when existing loop-free alternate paths and forwarding-plane capabilities can be leveraged to redistribute selected traffic locally. It provides a tactical response to overload without requiring changes to the underlying routing or traffic engineering architecture.

In RSVP-TE networks, TTE complements existing bandwidth-engineering mechanisms, including bandwidth reservations, admission control, Auto-Bandwidth, and CSPF-based path selection. It can provide localized overload relief while these mechanisms perform bandwidth adjustments or global LSP reoptimization.

Key deployment considerations: Alternate paths must be suitable for carrying additional traffic, and offloading must avoid creating secondary overload. Forwarding-plane capabilities must support the required traffic distribution, and appropriate hysteresis and reversion policies are necessary to prevent instability. Where RSVP-TE protection paths are reused, the impact on reserved bandwidth and failure-protection capacity must also be considered.

## Summary {#sec-scenario-summary}

| Scenario | Primary Criterion | Recommended Approach | Key Prerequisite |
|----------|-------------------|----------------------|------------------|
| Scenario1 | Distributed, Flex-Algo available | DCM | Flex-Algo + extended admin groups |
| Scenario2 | Distributed, SR-TE available | EBR	| SR-TE data plane + IGP TE metrics (bandwidth related metrics) |
| Scenario3 | Controller-based, human oversight required | LCM |	Controller + SR-TE policy support |
| Scenario4 | Distributed Alleviation with Existing Alternate Paths | TTE | Existing bandwidth-engineering and protection mechanisms |

These scenarios are not mutually exclusive. A network may deploy LCM for policy-controlled offload while also relying on DCM or EBR for autonomous distributed reaction to sudden events. The choice of approach should be guided by the operator's existing infrastructure, operational model, and the specific characteristics of the overload events that need to be mitigated.

# Deployment and Implementation Experience {#sec-deployment}

TBD

# Security Considerations {#sec-security}

There are two main security exposures introduced by link overload alleviation.

Firstly, the network can be destabilised causing traffic to be steered off the optimal path resulting in degradation of traffic delivery and potential packet loss. For example, if the configuration of overload and reversion thresholds can be compromised, then traffic steering can be induced. Further, a flip-flop with reversion can be forced by setting the reversion threshold higher than the overload threshold, resulting in interruption of traffic flows.

A more complex attack may cause traffic be sent to an authorised network exit where it can be exfiltrated. This attack requires a tunnel to be set up to that exit point, and then overload steering to be triggered so that traffic is placed on the tunnel.

The most important protection against these attacks is security applied to the configuration/management protocols.

It may be possible to force link overload alleviation by introducing bogus, high-volume traffic flows to the network. Such an attack, however, is actually mitigated by link overload alleviation because otherwise it would simply result in overload with delay to or loss of other, legitimate traffic flows. In any case, this sort of attack should be adequately protected against by appropriate traffic policing at the network edge.

# Operational Considerations {#sec-operational}

TBD

- configuration of thresholds
- monitoring traffic flows (where is my packet?)
- reporting traffic steering
- complexity and instability versus alleviating overload

# IANA Considerations {#sec-iana}

This document makes no requests for IANA action.

--- back

# Acknowledgments
{:numbered="false"}

The authors would like to thank Daniel King and Colby Barth for input to this document.
