---
title: "A Network YANG Data Model for Attachment Circuits"
abbrev: "A YANG Network Model for ACs"
category: std

docname: draft-boro-opsawg-ntw-attachment-circuit-latest
submissiontype: IETF
number:
date:
consensus: true
v: 3
area: "Operations and Management"
workgroup: "Operations and Management Area Working Group"
keyword:
 - Slice Service
 - L3VPN
 - L2VPN

author:
 -
    fullname: Mohamed Boucadair
    role: editor
    organization: Orange
    email: mohamed.boucadair@orange.com

 -
    fullname: Richard Roberts
    organization: Juniper
    email: rroberts@juniper.net

 -
    fullname: Oscar Gonzalez de Dios
    organization: Telefonica
    email: oscar.gonzalezdedios@telefonica.com

 -
    fullname: Samier Barguil Giraldo
    organization: Nokia
    email: samier.barguil_giraldo@nokia.com

 -
    fullname: Bo Wu
    organization: Huawei Technologies
    email: lana.wubo@huawei.com

normative:

informative:


--- abstract

This document specifies a network model for attachment circuits. The model can be used for the provisioning of attachment circuits prior or during service provisioning (e.g., Network Slice Service).

The module augments the SAP model with the detailed information for the provisioning of an AC in a PE.

--- middle

# Introduction

The procedure to provision a service in a service provider network may depend on the practices adopted by a service provider, including the flow put in place for the provisioning of advanced network services and how they are bound to an attachment circuit. For example, the same attachment circuit may be used to host multiple services. In order to avoid service interference and redundant information in various locations, a service provider may expose an interface to manage ACs network-wide. Customers can then request a base attachment circuit to be put in place, and then refer to that base AC when requesting services that are bound to that AC.

This document specifies a network model for attachment circuits. The model can be used for the provisioning of attachment circuits prior or during service provisioning (e.g., Network Slice Service).

The document leverages {{!RFC9182}} and {{!RFC9291}} by adopting an AC provisioning structure that uses data nodes that were already defined in these RFCs.

The AC network model is designed as an augmnetation to the Service Attachment Points (SAPs) model {{!I-D.ietf-opsawg-sap}}. An AC can be bound to a single or multiple SAPs. Likewise, the model is designed to accomdate deployments where a SAP can be bound to one or multiple ACs.

 The YANG data models in this document conform to the Network Management Datastore Architecture (NMDA) defined in {{!RFC8342}}.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

The reader should be familiar with the terms defined in {{Section 2 of !I-D.ietf-opsawg-sap}}.

This document uses the term "network model" as defined in {{Section 2.1 of ?RFC8969}}.

The meanings of the symbols in the YANG tree diagrams are defined in {{?RFC8340}}.

This document uses the following terms:

Network controller:
: Denotes a functional entity responsible for the management of the service provider network.

Service orchestrator:
: Refers to a functional entity that interacts with the customer of a network service. The service orchestrator is typically responsible for the attachment circuits, the Provider Edge (PE) selection, and requesting the activation of the requested service to a network controller.

Service provider network:
: A network that is able to provide network services (e.g., Network Slice Services).

Service provider:
: A service provider that offers network services (e.g., Network Slice Services).


# Sample Uses of the Attachment Circuit Data Models

TBC.

# Description of the Attachment Circuit YANG Module


## Overall Structure of the Module

TBC TBC

~~~~
xxx
~~~~
{: #o-ntw-tree title="Overall Tree Structure"}

## AC Profiles

## SAPs and ACs

## L2 Connection

## IP Connection

## Routing

## OAM

## Security



#  YANG Module

This module uses types defined in {{!RFC6991}, {{!RFC8177}}, {{!RFC8294}}, {{!RFC8343}}, {{!RFC9181}}, and IEEE Std 802.1Qcp.

~~~~
<CODE BEGINS> file "ietf-ac-ntw@2022-11-30.yang"
{::include ./yang/ietf-ac-ntw.yang}
<CODE ENDS>
~~~~


# Security Considerations

   The YANG module specified in this document defines a schema for data
   that is designed to be accessed via network management protocols such
   as NETCONF {{!RFC6241}} or RESTCONF {{!RFC8040}}.  The lowest NETCONF layer
   is the secure transport layer, and the mandatory-to-implement secure
   transport is Secure Shell (SSH) {{!RFC6242}}.  The lowest RESTCONF layer
   is HTTPS, and the mandatory-to-implement secure transport is TLS
   {{!RFC8446}}.

   The Network Configuration Access Control Model (NACM) {{!RFC8341}}
   provides the means to restrict access for particular NETCONF or
   RESTCONF users to a preconfigured subset of all available NETCONF or
   RESTCONF protocol operations and content.

   There are a number of data nodes defined in this YANG module that are
   writable/creatable/deletable (i.e., config true, which is the
   default).  These data nodes may be considered sensitive or vulnerable
   in some network environments.  Write operations (e.g., edit-config)
   and delete operations to these data nodes without proper protection
   or authentication can have a negative effect on network operations.
   These are the subtrees and data nodes and their sensitivity/
   vulnerability in the "ietf-ac-ntw" module:

   * TBC
   * TBC

   Some of the readable data nodes in this YANG module may be considered
   sensitive or vulnerable in some network environments.  It is thus
   important to control read access (e.g., via get, get-config, or
   notification) to these data nodes.  These are the subtrees and data
   nodes and their sensitivity/vulnerability in the "ietf-ac-svc" module:

   * TBC
   * TBC


# IANA Considerations

   IANA is requested to register the following URI in the "ns" subregistry within
   the "IETF XML Registry" {{!RFC3688}}:

~~~~
   URI:  urn:ietf:params:xml:ns:yang:ietf-ac-ntw
   Registrant Contact:  The IESG.
   XML:  N/A; the requested URI is an XML namespace.
~~~~

   IANA is requested to register the following YANG module in the "YANG Module
   Names" subregistry {{!RFC6020}} within the "YANG Parameters" registry.

~~~~
   Name:  ietf-ac-ntw
   Maintained by IANA?  N
   Namespace:  urn:ietf:params:xml:ns:yang:ietf-ac-ntw
   Prefix:  ac
   Reference:  RFC xxxx
~~~~

--- back

# Full Tree

~~~~
{::include ./yang/ac-ntw-without-groupings.txt}
~~~~

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
