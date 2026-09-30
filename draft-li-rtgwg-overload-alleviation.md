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
 - bandwidth

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

Network links may become degraded because of physical conditions or owing to partial link failure: this can result in a reduction of available bandwidth. Alternatively, traffic volume may increase significantly and unexpectedly. These circumstances may result in links becoming overloaded which can result in dropped packets or degraded delivery such as increased delay.

Traffic Engineering (TE) techniques can be applied to alleviate these link-overload situations by steering some of the traffic onto other paths that are less loaded and have available bandwidth.

This document examines the scenarios in which links can become overloaded, the requirements for steering traffic to reduce link-overload, and the TE techniques that can be applied to alleviate overloaded links.

--- middle

# Introduction {#sec-intro}

Network links may become degraded because of physical conditions or owing to partial link failure. For example, a microwave link can see a reduction in available bandwidth during inclement weather conditions. Alternatively, a link comprising a bundle of parallel links (often referred to as an a link aggregation group) may suffer the failure of a component link resulting in a decrease in the available bandwidth on the composite link. This may result in the traffic volume on the link exceeding the available bandwidth.

At the same time, traffic volume may increase significantly and unexpectedly above predicted levels such that traffic volume on the link exceeding the available bandwidth.

Any of these circumstances may result in links becoming overloaded which can result in dropped packets or degraded delivery such as increased delay.

Traffic Engineering (TE) techniques {{?RFC9522}} can be applied to alleviate these link-overload situations by steering traffic onto other paths (that is, using other links) that are less loaded and have available bandwidth.

This document examines the scenarios in which links can become overloaded, the requirements for steering some of the traffic to reduce link-overload, and the TE techniques that can be applied to alleviate overloaded links.

# Problem Description {#sec-problem}

## Definition of Link Overload {#sec-overload}

## Required Responsiveness {#sec-required}

## Comparison with Link Failure {#sec-failure}

It is worth noting that link failures are an extreme version of link overload. When a link fails, it is equivalent to the link suddenly having no available bandwidth.

There are plenty of available techniques for mitigating link failure. These include:

- IGP routing convergence {{?RFC4750}}, {{?RFC5340}}, {{ISO10589}}
- End-to-end protection in MPLS-TE networks {{?RFC4427}}
- Segment protection in MPLS-TE networks {{?RFC4427}}
- Fast Reroute (FRR) for IP {{?RFC5741}}, MPLS-TE {{?RFC4090}}, or Segment Routing (SR) {{?RFC9855}}.

Solutions for link failure may provide a basis for, or conceptual input to, solutions for link overload.

## Discussion of Transport-Level Congestion Control {#sec-congestion}

Congestion detection, reporting, control, and mitigation have been the subject of RFCs since the earliest days of the IETF. In general, congestion notification aims to be very responsive or even take place in advance of the congestion actually becoming a problem. Early Congestion Notification (ECN) {{?RFC3180}} aims to provide information to the head end of network flows so that they can moderate the rate at which traffic is presented to the network and so avoid congestion harming traffic flows. The assumption is that notified traffic sources will behave reasonably in their own best interests (reduced throughput is preferable to recovering from dropped packets) and in the best interests of the network as a whole (i.e., not deciding that other sources will back off and increasing traffic to use the capacity they make available).

Over the years, many other TCP/IP congestion notification and remedial techniques have been proposed and some have been standardised. These predominately operate at the transport level, moderating the rate at which traffic is delivered to the network in order to avoid or mitigate congestion. Response time can be as short as detection time plus notification time, plus time for in-transit packets to reach the point of congestion: effectively the detection time plus the round-trip time. 

Active Queue Management (AQM) {{?BCP197}} is a method that allows network devices to control the queue length or the mean time that a packet spends in a queue. By carefully calibrating queuing behaviors, network nodes are able to mitigate high traffic levels (in particular traffic bursts) and so reduce the effects.

The problems discussed in this document (see {{sec-problem}}) are similar to those that have been addressed previously (i.e., link overload is equivalent to congestion on that link). The use cases (see {{sec-usecase}}) are somewhat similar, but the requirements and espescially the responsiveness (see {{sec-required}}) are different.

# Use Cases and Scenarios {#sec-usecase}

# Functional Model {#sec-functional-model}

## Functional Actions {#sec-funct-acts}

## Key Action Points {#sec-funct-points}

# General Approaches {#sec-approaches}

## Distributed Congestion Mitigation {#sec-approach-DCM}

Distributed Congestion Mitigation (DCM) described in {{?I-D.psenak-lsr-igp-dcm}} is a distributed IGP integrated congestion mitigation mechanism. Its primary objective is to dynamically offload traffic from locally congested links onto congestion free alternate paths in Offloading Flex Algo (OFA) topologies.

In DCM, each router continuously monitors local link utilization. When link utilization exceeds the configured Congestion Threshold, the router advertises "Congestion Affinity" via IGP link attributes, which causes the congested link to be excluded from a dedicated OFA topology. Additionally, the router also performs traffic offloading to divert the traffic onto the shortest path that avoids any congested links. A separate IGP link attribute, "High Utilization Affinity" signals routers participating in the IGP to stop sending new offloaded traffic to the link without impacting existing offloaded traffic. DCM leverages Unequal Cost Multipath (UCMP) to divert traffic from the primary to the offload path (i.e., to offload traffic) in progressively periodic iterations. When link utilization on the congested link falls below a configured "Restore Threshold", traffic is gradually moved back (reverted) to the original primary path.

DCM depends on IGP (IS-IS or OSPF) for affinity advertisement, and IGP Flexible Algorithm ({{?RFC9350}}) to construct the OFA topology. Participating nodes must implement threshold based link state signaling and obey IGP LSP/LSA update throttling rules.

## Elastic Bandwidth-aware Routing {#sec-approach-EBR}

Elastic Bandwidth aware Routing (EBR) specified in {{?I-D.czz-rtgwg-elastic-bandwidth-routing}}, is a distributed dynamic congestion alleviation mechanism that responds rapidly to unexpected network congestion before centralized TE completes global optimization. Its core goal is to mitigate congestion triggered by unexpected reasons timely by distributing traffic among the shortest paths and load-balancing alternate paths through Segment Routing Traffic Engineering (SR-TE) {{?RFC9256}}.

In EBR, each router monitors local link bandwidth usage and advertises available bandwidth and utilization metrics via extensions to IGP-TE when specific conditions and thresholds are met. Then, each router pre-computes loop-free, preferably disjoint, load-balancing alternate paths for each destination. When the link utilization exceeds the configured "Congestion Threshold", the router uses UCMP to divert specific flows to alternate paths. The diverted traffic is encapsulated using an SR-TE strict path for loop-free forwarding. Path weights are derived from the bottleneck link on each alternate path. Traffic fallback (reversion) is performed when bandwidth utilization on the originally overloaded link drops below a dynamic "Restore Threshold" to restore flows to primary paths.

EBR depends on IGP (IS-IS or OSPF) with existing TE metric extensions ({{?RFC8570}} for IS-IS, {{?RFC7471}} for OSPF) for bandwidth information dissemination. SR-TE data plane capability is mandatory for steering offloaded traffic.

## Tactical Traffic Engineering{#sec-approach-TTE}

Tactical Traffic Engineering (TTE) is a distributed real time congestion mitigation mechanism defined in {{?I-D.li-rtgwg-tte}}. It works in conjunction with previous bandwidth-oriented traffic engineering techniques to mitigate transient congestion while optimal traffic assignment is being recomputed. TTE dynamically distributes load if congestion is anticipated, shifts traffic load away from congested links, and reverts traffic back to original paths once congestion abates.

TTE leverages pre-computed backup paths such as Loop-Free Alternates (LFAs) {{?RFC5286}} or Topology Independent Loop-Free Alternates (TI-LFAs) {{?RFC9855}}. When utilization on a link exceeds a configured congestion threshold, the corresponding router converts these standby backup paths into active parallel paths alongside primary paths to form Equal Cost Multipath (ECMP) multipath groups. TTE manipulates Forwarding Information Base (FIB) or MPLS Label FIB (LFIB) entries to achieve flow-level load distribution.

TTE builds upon existing loop-free backup path computation capabilities such as LFA and TI-LFA. Forwarding planes must support FIB/LFIB modification for dynamic multipath group adjustment.

## Local Congestion Management  {#sec-approach-LCM}

Local Congestion Mitigation (LCM) specified in {{LCM}} is a controller-based tactical congestion mitigation solution, designed to handle short-lived transient congestion while complementing capacity planning. LCM mitigates congestion by diverting the minimal necessary amount of traffic away from the congested interface to bring it out of congestion. LCM acts as a tactical complement to capacity planning and network engineering. It supports monitor-only and human-in-the-loop workflows for operational safety.

LCM operates as a closed-loop controller-driven workflow. Crosswork Data Gateway (CDG) collects interface-level statistics from network elements. The controller maintains real-time network state and detects congestion when measured utilization exceeds user configured thresholds. It computes how much optimizable traffic volume needs to be offloaded to relieve congestion. A Segment Routing Path Computation Element (SR PCE) calculates suitable alternate paths and deploys temporary tactical SR-TE policies on headend nodes. ECMP splits traffic across multiple parallel tactical SR-TE policies to achieve the target offloaded volume. After congestion subsides and hold-margin conditions are satisfied, the controller removes temporary tactical policies and reverts traffic back to native IGP forwarding to avoid persistent policy churn.

LCM relies on BGP-LS {{?RFC9552}} or the active IGP to collect real-time topology information. PCEP {{?RFC5440}} is required between the SR PCE and PCC routers for installation and removal of PCE-initiated SR-TE policies. gRPC or SNMP is used for statistics telemetry collection. Headend routers must support PCE-initiated SR-TE policies with autoroute steering and ECMP over multiple parallel SR-TE policies.

# Applicability to Different Scenarios {#sec-applicability}

## Scenario 1

## Scenario 2

# Combined Approaches {#sec-symbiosis}

I am not sure whether this section is necessary, maybe we can leave it blank for now.

# Deployment and Implementation Experience {#sec-deployment}

# Security Considerations {#sec-security}

# Operational Considerations {#sec-operational}

# IANA Considerations {#sec-iana}

This document makes no requests for IANA action.

--- back

# Acknowledgments
{:numbered="false"}
