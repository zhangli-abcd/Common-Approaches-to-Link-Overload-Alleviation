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

  LCM:
    title: Local Congestion Manageent
    date: 2026
    target: https://www.cisco.com/c/en/us/products/collateral/cloud-systems-management/crosswork-network-automation/local-congestion-mitigation-wp.html

   TTE:
     title: Tactical Traffic Engineering
     date: 2025
     target: https://community.juniper.net/blogs/moshiko-nayman/2025/02/19/sr-tactical-traffic-engineering-in-junos

--- abstract

Network links may become degraded because of physical conditions or owing to partial link failure: this can result in a reduction of available
bandwidth. Alternatively, traffic volume may increase significantly and unexpectedly. These circumstances may result in links becoming overloaded which can result in dropped packets or degraded delivery such as increased delay.

Traffic Engineering (TE) techniques can be applied to alleviate these link-overload situations by steering traffic onto other paths that are less loaded and have available bandwidth.

This document examines the scenarios in which links can become overloaded, the requirements for steering traffic to reduce link-overload, and the TE techniques that can be applied to alleviate overloaded links.

--- middle

# Introduction {#sec-intro}

TODO Introduction


# Conventions and Definitions

{::boilerplate bcp14-tagged}


# Problem Description {#sec-problem}

## Definition of Link Overload {#sec-overload}

## Required Responsiveness 

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
