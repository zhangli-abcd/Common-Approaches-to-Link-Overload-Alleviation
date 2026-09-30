---
title: "Common Approaches to Link Overload Alleviation"
abbrev: "Overload Alleviation"
docname: draft-li-rtgwg-overload-alleviation

stand_alone: true
ipr: trust200902
area: RTG
workgroup: WG Working Group
keyword:
 - congestion
 - bandwidth

category: info
submissiontype: IETF
coding: utf-8
pi: [toc, sortrefs, symrefs]

venue:
  group: RTGWG
  type: Working Group
  mail: rtgwg@ietf.org
  arch: https://ietf.org/rtgwg
  github: zhangli-abcd/Common-Approaches-to-Overload-Alleviation
  
author:
 -
    fullname: Li Zhang
    organization: Huawei
    email: zhangli344@huawei.com

    fullname: Adrian Farrel
    organization: Old Dog Consulting
    email: adrian@olddog.co.uk

informative:

  ISO10589:
     title: Intermediate system to Intermediate system intra-domain routeing information exchange protocol for use in
            conjunction with the protocol for providing the connectionless-mode Network Service (ISO 8473),
            ISO/IEC 10589:2002, Second Edition.
     date: 2002
     author: International Organization for Standardization
     target: https://www.iso.org/standard/30932.html
     
  LCM:
    title: Local Congestion Manageent
    date: 2026
    author: Cisco Systems Inc.
    target: https://www.cisco.com/c/en/us/products/collateral/cloud-systems-management/crosswork-network-automation/local-congestion-mitigation-wp.html

   TTE:
     title: Tactical Traffic Engineering
     date: 2025
     author: Hewlett Packard Enterprise
     target: https://community.juniper.net/blogs/moshiko-nayman/2025/02/19/sr-tactical-traffic-engineering-in-junos

--- abstract

Network links may become degraded because of physical conditions or owing to partial link failure: this can result in a reduction of available
bandwidth. Alternatively, traffic volume may increase significantly and unexpectedly. These circumstances may result in links becoming overloaded which can result in dropped packets or degraded delivery such as increased delay.

Traffic Engineering (TE) techniques can be applied to alleviate these link-overload situations by steering traffic onto other paths that are less loaded and have available bandwidth.

This document examines the scenarios in which links can become overloaded, the requirements for steering traffic to reduce link-overload, and the TE techniques that can be applied to alleviate overloaded links.

--- middle

# Introduction {#sec-intro}

Network links may become degraded because of physical conditions or owing to partial link failure. For example, a microwave link can see a reduction in available bandwidth during inclement weather conditions. Alternatively, a link comprising a bundle of parallel links (often referred to as an a link aggregation group) may suffer the failure of a component link resulting in a decrease in the available bandwidth on the composite link. This may result in the traffic volume on the link exceeding the available bandwidth.

At the same time, traffic volume may increase significantly and unexpectedly above predicted levels such that traffic volume on the link exceeding the available bandwidth.

Any of these circumstances may result in links becoming overloaded which can result in dropped packets or degraded delivery such as increased delay.

Traffic Engineering (TE) techniques {{!RFC9522}} can be applied to alleviate these link-overload situations by steering traffic onto other paths (that is, using other links) that are less loaded and have available bandwidth.

This document examines the scenarios in which links can become overloaded, the requirements for steering traffic to reduce link-overload, and the TE techniques that can be applied to alleviate overloaded links.

# Problem Description {#sec-problem}

## Definition of Link Overload {#sec-overload}

## Required Responsiveness {#sec-required}

## Comparison with Link Failure {#sec-failure}

It is worth noting that link failures are an extreme version of link overload. When a link fails, it is equivalent to the link suddenly having no available bandwidth.

There are plenty of available techniques for mitigating link failure. These include:

- IGP routing convergence {{!RFC4750}}, {{!RFC5340}}, {{ISO10589}}
- End-to-end protection in MPLS-TE networks {{!RFC4427}}
- Segment protection in MPLS-TE networks {{!RFC4427}}
- Fast Reroute (FRR) for IP {{!}}, MPLS-TE {{!RFC4090}}, or Segment Routing (SR) {{!}}. 

Solutions for link failure may provide a basis for, or conceptual input to, solutions for link overload.

# Use Cases and Scenarios {#sec-usecase}

# Functional Model {#sec-functional-model}

## Functional Actions {#sec-funct-acts}

## Key Action Points {#sec-funct-points}

# General Approaches {#sec-approaches}

## Distributed Congestion Mitigation {#sec-approach-DCM}

Distributed Congestion Mitigation (DCM) described in {{!I-D.psenak-lsr-igp-dcm}} is a distributed IGP integrated congestion mitigation mechanism. Its primary objective is to dynamically offload traffic from locally congested links onto congestion free alternate paths in Offloading Flex Algo (OFA) topologies. 

In DCM, each router continuously monitors local link utilization. When link utilization exceeds the configured Congestion Threshold, the router advertises **Congestion Affinity** via IGP link attributes, which excludes the congested link from a dedicated Offloading Flex Algo (OFA) topology. Additionally, the router also performs traffic offloading to divert the traffic onto the shortest path that avoids any congested links. A separate **High Utilization Affinity** signals routers in the area to stop sending new offload traffic to the link without impacting existing offloaded traffic. DCM leverages UCMP to divert traffic across the primary and offload path in progressively in periodic iterations. When link utilization falls below restore threshold, traffic is gradually moved back to the original primary path.

DCM depends on IGP(IS IS or OSPF) for affinity advertisement and IGP Flexible Algorithm ({{!RFC9350}}) to construct the OFA topology. Participating nodes must implement threshold based link state signaling and obey IGP LSP/LSA update throttling rules. 

## Elastic Bandwidth-aware Routing {#sec-approach-EBR} 

Elastic Bandwidth aware Routing (EBR) specified in {{!I-D.czz-rtgwg-elastic-bandwidth-routing}}, is a distributed dynamic congestion alleviation mechanism that responds rapidly to unexpected network congestion before centralized TE completes global optimization. Its core goal is to mitigate congestion triggered by unexpected reasons timely by distributing traffic among the shortest paths and load-balancing alternate paths through Segment Routing Traffic Engineering (SR-TE). 

In EBR, each router monitors local link bandwidth usage and advertises available bandwidth and utilization metrics via IGP TE extensions when specific conditions are meet. Then, each router pre computes loop free, preferably disjoint load balancing alternate paths for each destination. When the link utilization exceeds the configured **Congestion Threshold**, the router uses UCMP to divert specific flows to alternate paths. The diverted traffic is encapsulated with SR TE strict path for loop free forwarding. Path weights are derived from the bottleneck link on each alternate path. Traffic fallback is performed when link bandwidth utilization drops below a dynamic **Restore Threshold** to restore flows to primary paths.

EBR depends on IGP (IS IS or OSPF) with existing TE metric extensions ({{!RFC8570}} for IS IS, {{!RFC7471}} for OSPF) for bandwidth information dissemination. SR TE data plane capability is mandatory for steering offloaded traffic.

## Tactical Traffic Engineering{#sec-approach-TTE}

Tactical Traffic Engineering (TTE) is a distributed real time congestion mitigation mechanism defined in {{!I-D.li-rtgwg-tte}}. It works in conjunction with traditional bandwidth oriented traffic engineering techniques to mitigate transient congestion while optimal traffic assignment is being recomputed. TTE dynamically distributes load if congestion is anticipated, shifts traffic load away from congested links, and reverts traffic back to original paths once congestion abates. 

TTE leverages pre computed backup paths such as LFA or TI LFA. When a link utilization exceeds congestion thresholds, the corresponding router converts these standby backup paths into active parallel paths alongside primary paths to form ECMP multi path groups. TTE manipulates FIB or LFIB entries to achieve flow level load distribution.

TTE builds upon existing loop free backup path computation capabilities such as LFA and TI LFA. Forwarding planes must support FIB/LFIB modification for dynamic multi path group adjustment. 

## Local Congestion Management  {#sec-approach-LCM}

Local Congestion Mitigation (LCM) specified in {{LCM}} is a controller based tactical congestion mitigation solution, designed to handle short lived transient congestion while complementing capacity planning. LCM mitigates congestion by diverting the minimal amount of traffic away from the congested interface to bring it out of congestion. LCM acts as a tactical complement to capacity planning and network engineering. It supports monitor only mode and human in the loop workflow for operational safety.

LCM operates as a closed loop controller driven workflow. Crosswork Data Gateway (CDG) collects interface level statistics from network elements. The controller maintains real time network state and detects congestion when measured utilization exceeds user configured thresholds. It computes how much optimizable traffic volume needs to be offloaded to relieve congestion. Segment Routing Path Computation Element (SR PCE) calculates suitable alternate paths and deploys temporary tactical SR TE policies on headend nodes. ECMP splits traffic across multiple parallel tactical SR TE policies to achieve the target offloaded volume. After congestion subsides and hold margin conditions are satisfied, the controller removes temporary tactical policies and reverts traffic back to native IGP forwarding to avoid persistent policy churn.

LCM relies on BGP LS or IGP to collect real-time topology. PCEP is required between SR PCE and PCC routers for installation and removal of PCE initiated SR TE policies. gRPC or SNMP is used for statistics telemetry collection. Headend routers must support PCE-initiated SR-TE policies with autoroute steering and ECMP over multiple parallel SR TE policies. 

# Applicability to Different Scenarios {#sec-applicability}

## Scenario 1

## Scenario 2

# Combined Approaches {#sec-symbiosis}

# Deployment and Implementation Experience {#sec-deployment}

# Security Considerations {#sec-security}

# Operational Considerations {#sec-operational}

# IANA Considerations {#sec-iana}

This document makes no requests for IANA action.

--- back

# Acknowledgments
{:numbered="false"}


