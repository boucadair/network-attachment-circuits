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

contributor:
  -
    name: Victor Lopez
    org: Nokia
    email: victor.lopez@nokia.com

  -
    fullname: Ivan Bykov
    organization: Ribbon Communications
    email: Ivan.Bykov@rbbn.com

  -
    fullname: Qin Wu
    organization: Huawei
    email: bill.wu@huawei.com

  -
    fullname: Ogaki Kenichi
    organization: KDDI
    email: ke-oogaki@kddi.com

  -
    fullname: Luis Angel Munoz
    organization: Vodafone
    email: luis-angel.munoz@vodafone.com

normative:

informative:
  AC-Ntw-Tree:
    title: Full Network Attachment Circuit Tree Structure
    date: 2023
    target: https://github.com/boucadair/network-attachment-circuits/blob/main/yang/ac-ntw-without-groupings.txt

--- abstract

This document specifies a network model for attachment circuits. The model can be used for the provisioning of attachment circuits prior or during service provisioning (e.g., Network Slice Service). A companion service model is specified in {{!I-D.boro-opsawg-teas-attachment-circuit}}.

The module augments the Service Attachment Point (SAP) model with the detailed information for the provisioning of attachment circuits in Provider Edges (PEs).

--- middle

# Introduction

Connectivity services are provided by networks to customers via
   dedicated terminating points, such as Service Functions {{?RFC7665}},
   customer edges (CEs), peer Autonomous System Border Routers (ASBRs),
   data centers gateways, or Internet Exchange Points.

The procedure to provision a service in a service provider network may depend on the practices adopted by a service provider, including the flow put in place for the provisioning of advanced network services and how they are bound to an Attachment Circuit (AC). For example, the same AC may host multiple services (e.g., Layer 2 VPN, Slice Service, or Layer 3 VPN). In order to avoid service interference and redundant information in various locations, a service provider may expose an interface to manage ACs network-wide. Customers can then request a base AC to be put in place, and then refer to that AC when requesting services to be bound to that AC. {{!I-D.boro-opsawg-teas-attachment-circuit}} specifies a data model for managing attachment circuits as a service.

This document specifies a network model for ACs ("ietf-ac-ntw"). The model can be used for the provisioning of ACs prior or during service provisioning.

The document leverages {{!RFC9182}} and {{!RFC9291}} by adopting an AC provisioning structure that uses data nodes that are defined in these RFCs. Some refinements were introduced to cover, not only conventional service provider networks, but also specifics of other target deployments (cloud, for example).

The AC network model is designed as an augmnetation to the Service Attachment Point (SAP) model {{!RFC9408}}. An AC can be bound to a single or multiple SAPs. Likewise, the model is designed to accomdate deployments where a SAP can be bound to one or multiple ACs.

~~~~
                    .---.
                    |CE6|
                    '-+-'
                   ac |        .---.                    .---.
                      |        |CE5+----------+---------+CE2|
               .------+-----.  '---'          |         '---'
               |            |                 |ac
               |            |                 |
             .-+-.       .-+-.              .-+-.
           .-+sap+-------+sap+-.          .-+sap+-------------.
           | '---'       '---' |          | '---'             |
.---.    .-+-.                 |          |                   |
|CE1+----+sap|      PE1        |          |         PE2       |
'---' ac '-+-'                 |          |                   |
           '-------------------'          '-------------------'

           .-------------------.          .-------------------.
           |                   |          |                 .-+-. ac .---.
           |         PE3       |          |        PE4      |sap+----+CE5|
           |                   |          |                 '---'    '---'
           |             .---. |          | .---. .---. .---. |
           '-------------+sap+-'          '-+sap+-+sap+-+sap+-'
                         '-+-'              '-+-' '-+-' '-+-'
                           |ac                |     |ac   |ac
                         .-+-.                |   .-+-.   |
                         |CE3+--------ac------'   |CE4+---'
                         '---'                    '---'
~~~~
{: #sap-ac-ntw title="Attachment Circuits Examples" artwork-align="center"}

The AC network model uses the AC common model defined in {{!I-D.boro-opsawg-teas-common-ac}}.

 The YANG data model in this document conforms to the Network Management Datastore Architecture (NMDA) defined in {{!RFC8342}}.

# Conventions and Definitions

{::boilerplate bcp14-tagged}

The reader should be familiar with the terms defined in {{Section 2 of !RFC9408}}.

This document uses the term "network model" as defined in {{Section 2.1 of ?RFC8969}}.

The meanings of the symbols in the YANG tree diagrams are defined in {{?RFC8340}}.

This document uses the following terms:

Bearer:
: A physical or logical link that connects a customer node (or site) to a provider network. A bearer can be a wireless or wired link. One or multiple technologies can be used to build a bearer. The bearer type can be specified by a customer.
: The operator allocates a unique bearer reference to identify a bearer within its network (e.g., customer line identifier). Such a reference can be retrieved by a customer and used in subsequent service placement requests to unambiguously identify where a service is to be bound.
: The concept of bearer can be generalized to refer to the required underlying connection for the provisioning of an attachment circuit. One or multiple attachment circuits may be hosted over the same bearer (e.g., multiple VLANs on the same bearer that is provided by a physical link).

Network controller:
: Denotes a functional entity responsible for the management of the service provider network.

Service orchestrator:
: Refers to a functional entity that interacts with the customer of a network service. The service orchestrator is typically responsible for the attachment circuits, the Provider Edge (PE) selection, and requesting the activation of the requested service to a network controller.

Service provider network:
: A network that is able to provide network services (e.g., Network Slice Services).

Service provider:
: A service provider that offers network services (e.g., Network Slice Services).

# Sample Uses of the Attachment Circuit Data Models

{{u-ex}} shows the positioning of the AC network model in the overall service delivery process.

~~~~
                          +---------------+
                          |   Customer    |
                          +-------+-------+
          Customer Service Model  |
        e.g., slice-svc, ac-svc   |
                          +-------+-------+
                          |    Service    |
                          | Orchestration |
                          +-------+-------+
           Network Model          |
     e.g., l3vpn-ntw, sap, ac-ntw |
                          +-------+-------+
                          |   Network     |
                          | Orchestration |
                          +-------+-------+
    Network Configuration Model   |
                      +-----------+-----------+
                      |                       |
             +--------+------+       +--------+------+
             |    Domain     |       |     Domain    |
             | Orchestration |       | Orchestration |
             +---+-----------+       +--------+------+
  Device         |        |                   |
  Configuration  |        |                   |
  Model          |        |                   |
            +----+----+   |                   |
            | Config  |   |                   |
            | Manager |   |                   |
            +----+----+   |                   |
                 |        |                   |
                 | NETCONF/CLI..................
                 |        |                   |
                 +--------------------------------+
   +----+ Bearer |                                | Bearer +----+
   |CE#1+--------+            Network             +--------+CE#2|
   +----+        |                                |        +----+
                 +--------------------------------+
   Site A                                                  Site B
~~~~
{: #u-ex title="An Example of the Network AC Model Usage" artwork-align="center"}

# Description of the Attachment Circuit YANG Module

## Overall Structure of the Module

The overall tree structure of the module is shown in {{o-ntw-tree}}. A node can host one or more SAPs as per {{!RFC9408}}. Each SAP terminate one or multiple ACs. The SAP model in {{!RFC9408}} is thus augmented with required AC-related information. Also, in order to ease the correlation between the AC exposed at the service layer and the one that is actually provisioned in the network operation, a reference to the AC exposed to the customer  ('ac-ref') is stored in the 'ac-ntw' module.

Unlike the AC service model, an AC is uniquely identified within the scope of a node, not a network. An AC can be characterized using Layer 2 connectivity, Layer 3 connectivity, routing protocols, OAM, and security considerations. In order to factorize a set of data that is provisioned for a set of ACs, a set of profiles can be defined at the network level, and then called under the node level. The information contained in a profile is thus inherited, unless the corresponding data node is refined at the AC level. In such as case, the value provided at the AC level takes precedence over the global one.

~~~~
  augment /nw:networks/nw:network:
    +--rw ac-profile* [name]
       ...
  augment /nw:networks/nw:network/nw:node/sap:service/sap:sap:
    +--rw ac* [name]
       +--rw name                 string
       +--rw ac-ref?              ac-svc:attachment-circuit-reference
       +--rw ac-profile* [profile-id]
       |  +--rw profile-id    -> /nw:networks/network/ac-profile/name
       +--rw peer-sap-id*         string
       +--rw group* [group-id]
       |  +--rw group-id      string
       |  +--rw precedence?   identityref
       +--rw status
       |  +--rw admin-status
       |  |  +--rw status?        identityref
       |  |  +--rw last-change?   yang:date-and-time
       |  +--ro oper-status
       |     +--ro status?        identityref
       |     +--ro last-change?   yang:date-and-time
       +--rw description?         string
       +--rw l2-connection
       |  ...
       +--rw ip-connection
       |  ...
       +--rw routing-protocols
       |  ...
       +--rw oam
       |  ...
       +--rw security
          ...
~~~~
{: #o-ntw-tree title="Overall Tree Structure"}

The full tree of the 'ac-ntw' is provided in {{AC-Ntw-Tree}}.

## L2 Connection

The  Layer 2 connection tree structure is shown in {{l2-tree}}.

~~~~
{::include ./yang/l2-tree.txt}
~~~~
{: #l2-tree title="Layer 2 Connection Tree Structure"}

## IP Connection

The  Layer 3 connection tree structure is shown in {{l3-tree}}.

~~~~
{::include ./yang/l3-tree.txt}
~~~~
{: #l3-tree title="IP Connection Tree Structure"}

## Routing

The routing tree structure is shown in {{rtg-tree}}.

~~~~
{::include ./yang/rtg-tree.txt}
~~~~
{: #rtg-tree title="Rotuing Tree Structure"}

## OAM

The OAM tree structure is shown in {{oam-tree}}.

~~~~
{::include ./yang/oam-tree.txt}
~~~~
{: #oam-tree title="OAM Tree Structure"}

## Security

The security tree structure is shown in {{sec-tree}}.

~~~~
{::include ./yang/security-tree.txt}
~~~~
{: #sec-tree title="Security Tree Structure"}

#  YANG Module

This module uses types defined in {{!RFC6991}}, {{!RFC8177}}, {{!RFC8294}}, {{!RFC8343}}, {{!RFC9181}}, {{!I-D.boro-opsawg-teas-common-ac}}, and IEEE Std 802.1Qcp.

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

Several data nodes ('bgp', 'ospf', 'isis', and 'rip') rely upon {{!RFC8177}} for authentication purposes. As such, the AC network module inherits the security considerations discussed in Section 5 of {{!RFC8177}}. Also, these data nodes support supplying explicit keys as strings in ASCII format. The use of keys in hexadecimal string format would afford greater key entropy with the same number of key-string octets. However, such a format is not included in this version of the AC network model, because it is not supported by the underlying device modules (e.g., {{?RFC8695}}).

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
   Prefix:  ac-ntw
   Reference:  RFC xxxx
~~~~

--- back

# Acknowledgments
{:numbered="false"}

Thanks to Moti Morgenstern for the review and comments.
