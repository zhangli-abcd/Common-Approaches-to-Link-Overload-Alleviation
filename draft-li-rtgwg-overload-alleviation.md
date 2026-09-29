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

Distributed Congestion Mitigation (DCM) is described in {{!I-D.psenak-lsr-igp-dcm}}.

## Elastic Bandwidth-aware Routing {#sec-approach-EBR} 

Elastic Bandwidth-aware Routing (EBR) is described in {{!I-D.czz-rtgwg-elastic-bandwidth-routing}}.

## Local Congestion Management and Tactical Traffic Engineering {#sec-approach-LCM-TTE}

Local Congestion Management (LCM) is a proprietary mechanism defined by Cisco and documented at {{LCM}}. Tactical Traffic Engineering (TTE_
is a proprietary mechanism defined by HPE and documented at {{TTE}}.

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
