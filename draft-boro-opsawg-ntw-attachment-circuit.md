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
    organization: Orange
    email: mohamed.boucadair@orange.com

 -
    fullname: Richard Roberts
    organization: Juniper
    email: rroberts@juniper.net

-
    fullname: Oscar González de Dios
    organization: Telefonica
    email: oscar.gonzalezdedios@telefonica.com

-
    fullname: Samier Barguil Giraldo
    organization: Nokia
    email: samier.barguil_giraldo@nokia.com

normative:

informative:


--- abstract

This document specifies a network model for attachment circuits. The models can be used for the provisioning of attachment circuits prior or during service provisioning (e.g., Network Slice Service).

--- middle

# Introduction

This document specifies a network model for attachment circuits. The models can be used for the provisioning of attachment circuits prior or during service provisioning (e.g., Network Slice Service).

The document leverages {{!RFC9182}} and {{!RFC9291}} by adopting an AC provisioning structure which uses data nodes that were already defined in these RFCs.

The AC network model is designed as an augmnetation to the Service Attachment Points (SAPs) model {{!I-D.ietf-opsawg-sap}}. An AC can be bound to a single or multiple SAPs. Likewise, the model is designed to accomdate deployments where a SAP can be bound to one or multiple ACs.

 The YANG data models in this document conform to the Network Management Datastore Architecture (NMDA) defined in {{!RFC8342}}.


# Conventions and Definitions

{::boilerplate bcp14-tagged}

# Terminology

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

## Separate AC Provisioning vs. Actual Service Provisioning

The procedure to provision a service in a service provider network may depend on the practices adopted by a service provider, including the flow put in place for the provisioning of advanced network services and how they are bound to an attachment circuit. For example, the same attachment circuit may be used to host multiple services. In order to avoid service interference and redundant information in various locations, a service provider may expose an interface to manage ACs network-wide. Customers can the request a base attachment circuit to be put in place, and then refer to that base AC when requesting services that are bound to that AC.



# Description of the Attachment Circuit YANG Module


## Overall Structure of the Module

TBC TBC

~~~~


module: ietf-ac-ntw
  augment /nw:networks/nw:network:
    +--rw specific-provisioning-profiles
    |  +--rw valid-provider-identifiers
    |     +--rw external-connectivity-identifier* [id]
    |     |       {external-connectivity}?
    |     |  +--rw id    string
    |     +--rw encryption-profile-identifier* [id]
    |     |  +--rw id    string
    |     +--rw qos-profile-identifier* [id]
    |     |  +--rw id    string
    |     +--rw bfd-profile-identifier* [id]
    |     |  +--rw id    string
    |     +--rw forwarding-profile-identifier* [id]
    |     |  +--rw id    string
    |     +--rw routing-profile-identifier* [id]
    |        +--rw id    string
    +--rw ac-profile* [id]
       +--rw id                   string
       +--rw l2-connection
       |  +--rw encapsulation
       |  |  +--rw encap-type?        identityref
       |  |  +--rw dot1q
       |  |  |  +--rw tag-type?         identityref
       |  |  |  +--rw cvlan-id?         dot1q-types:vlanid
       |  |  |  +--rw tag-operations
       |  |  |     +--rw (op-choice)?
       |  |  |     |  +--:(pop)
       |  |  |     |  |  +--rw pop?         empty
       |  |  |     |  +--:(push)
       |  |  |     |  |  +--rw push?        empty
       |  |  |     |  +--:(translate)
       |  |  |     |     +--rw translate?   empty
       |  |  |     +--rw tag-1?             dot1q-types:vlanid
       |  |  |     +--rw tag-1-type?
       |  |  |     |       dot1q-types:dot1q-tag-type
       |  |  |     +--rw tag-2?             dot1q-types:vlanid
       |  |  |     +--rw tag-2-type?
       |  |  |             dot1q-types:dot1q-tag-type
       |  |  +--rw priority-tagged
       |  |  |  +--rw tag-type?   identityref
       |  |  +--rw qinq
       |  |     +--rw tag-type?         identityref
       |  |     +--rw svlan-id          dot1q-types:vlanid
       |  |     +--rw cvlan-id          dot1q-types:vlanid
       |  |     +--rw tag-operations
       |  |        +--rw (op-choice)?
       |  |        |  +--:(pop)
       |  |        |  |  +--rw pop?         uint8
       |  |        |  +--:(push)
       |  |        |  |  +--rw push?        empty
       |  |        |  +--:(translate)
       |  |        |     +--rw translate?   uint8
       |  |        +--rw tag-1?             dot1q-types:vlanid
       |  |        +--rw tag-1-type?
       |  |        |       dot1q-types:dot1q-tag-type
       |  |        +--rw tag-2?             dot1q-types:vlanid
       |  |        +--rw tag-2-type?
       |  |                dot1q-types:dot1q-tag-type
       |  +--rw (l2-service)?
       |     +--:(l2-tunnel-service)
       |     |  +--rw l2-tunnel-service
       |     |     +--rw type?         identityref
       |     |     +--rw pseudowire
       |     |     |  +--rw vcid?      uint32
       |     |     |  +--rw far-end?   union
       |     |     +--rw vpls
       |     |     |  +--rw vcid?      uint32
       |     |     |  +--rw far-end*   union
       |     |     +--rw vxlan
       |     |        +--rw vni-id             uint32
       |     |        +--rw peer-mode?         identityref
       |     |        +--rw peer-ip-address*   inet:ip-address
       |     +--:(l2vpn)
       |        +--rw l2vpn-id?            vpn-common:vpn-id
       +--rw ip-connection
       +--rw routing-protocols
       |  +--rw routing-protocol* [id]
       |     +--rw id      string
       |     +--rw type?   identityref
       |     +--rw bgp
       |     |  +--rw description?              string
       |     |  +--rw local-as?                 inet:as-number
       |     |  +--rw peer-as                   inet:as-number
       |     |  +--rw address-family?           identityref
       |     |  +--rw multihop?                 uint8
       |     |  +--rw as-override?              boolean
       |     |  +--rw allow-own-as?             uint8
       |     |  +--rw prepend-global-as?        boolean
       |     |  +--rw send-default-route?       boolean
       |     |  +--rw site-of-origin?           rt-types:route-origin
       |     |  +--rw ipv6-site-of-origin?
       |     |  |       rt-types:ipv6-route-origin
       |     |  +--rw redistribute-connected* [address-family]
       |     |  |  +--rw address-family    identityref
       |     |  |  +--rw enable?           boolean
       |     |  +--rw bgp-max-prefix
       |     |  |  +--rw max-prefix?          uint32
       |     |  |  +--rw warning-threshold?   decimal64
       |     |  |  +--rw violate-action?      enumeration
       |     |  |  +--rw restart-timer?       uint32
       |     |  +--rw bgp-timers
       |     |     +--rw keepalive?   uint16
       |     |     +--rw hold-time?   uint16
       |     +--rw ospf
       |     |  +--rw address-family?   identityref
       |     |  +--rw area-id           yang:dotted-quad
       |     |  +--rw metric?           uint16
       |     |  +--rw max-lsa?          uint32
       |     +--rw isis
       |     |  +--rw address-family?   identityref
       |     |  +--rw area-address      l3nm:area-address
       |     |  +--rw level?            identityref
       |     |  +--rw metric?           uint16
       |     |  +--rw mode?             enumeration
       |     +--rw rip
       |     |  +--rw address-family?   identityref
       |     |  +--rw timers
       |     |  |  +--rw update-interval?     uint16
       |     |  |  +--rw invalid-interval?    uint16
       |     |  |  +--rw holddown-interval?   uint16
       |     |  |  +--rw flush-interval?      uint16
       |     |  +--rw default-metric?   uint8
       |     +--rw vrrp
       |        +--rw address-family?   identityref
       |        +--rw ping-reply?       boolean
       +--rw oam
          +--rw bfd {vpn-common:bfd}?
             +--rw session-type?               identityref
             +--rw desired-min-tx-interval?    uint32
             +--rw required-min-rx-interval?   uint32
             +--rw local-multiplier?           uint8
             +--rw holdtime?                   uint32
  augment /nw:networks/nw:network/nw:node/sap:service/sap:sap:
    +--rw ac* [id]
       +--rw id                   string
       +--rw ac-profile* [profile-id]
       |  +--rw profile-id    -> /nw:networks/network/ac-profile/id
       +--rw l2-connection
       |  +--rw encapsulation
       |  |  +--rw encap-type?        identityref
       |  |  +--rw dot1q
       |  |  |  +--rw tag-type?         identityref
       |  |  |  +--rw cvlan-id?         dot1q-types:vlanid
       |  |  |  +--rw tag-operations
       |  |  |     +--rw (op-choice)?
       |  |  |     |  +--:(pop)
       |  |  |     |  |  +--rw pop?         empty
       |  |  |     |  +--:(push)
       |  |  |     |  |  +--rw push?        empty
       |  |  |     |  +--:(translate)
       |  |  |     |     +--rw translate?   empty
       |  |  |     +--rw tag-1?             dot1q-types:vlanid
       |  |  |     +--rw tag-1-type?
       |  |  |     |       dot1q-types:dot1q-tag-type
       |  |  |     +--rw tag-2?             dot1q-types:vlanid
       |  |  |     +--rw tag-2-type?
       |  |  |             dot1q-types:dot1q-tag-type
       |  |  +--rw priority-tagged
       |  |  |  +--rw tag-type?   identityref
       |  |  +--rw qinq
       |  |     +--rw tag-type?         identityref
       |  |     +--rw svlan-id          dot1q-types:vlanid
       |  |     +--rw cvlan-id          dot1q-types:vlanid
       |  |     +--rw tag-operations
       |  |        +--rw (op-choice)?
       |  |        |  +--:(pop)
       |  |        |  |  +--rw pop?         uint8
       |  |        |  +--:(push)
       |  |        |  |  +--rw push?        empty
       |  |        |  +--:(translate)
       |  |        |     +--rw translate?   uint8
       |  |        +--rw tag-1?             dot1q-types:vlanid
       |  |        +--rw tag-1-type?
       |  |        |       dot1q-types:dot1q-tag-type
       |  |        +--rw tag-2?             dot1q-types:vlanid
       |  |        +--rw tag-2-type?
       |  |                dot1q-types:dot1q-tag-type
       |  +--rw (l2-service)?
       |  |  +--:(l2-tunnel-service)
       |  |  |  +--rw l2-tunnel-service
       |  |  |     +--rw type?         identityref
       |  |  |     +--rw pseudowire
       |  |  |     |  +--rw vcid?      uint32
       |  |  |     |  +--rw far-end?   union
       |  |  |     +--rw vpls
       |  |  |     |  +--rw vcid?      uint32
       |  |  |     |  +--rw far-end*   union
       |  |  |     +--rw vxlan
       |  |  |        +--rw vni-id             uint32
       |  |  |        +--rw peer-mode?         identityref
       |  |  |        +--rw peer-ip-address*   inet:ip-address
       |  |  +--:(l2vpn)
       |  |     +--rw l2vpn-id?            vpn-common:vpn-id
       |  +--rw l2-termination-point?      string
       |  +--rw local-bridge-reference?    string
       |  +--rw bearer-reference?          string
       |  |       {vpn-common:bearer-reference}?
       |  +--rw lag-interface {vpn-common:lag-interface}?
       |     +--rw lag-interface-id?   string
       |     +--rw member-link-list
       |        +--rw member-link* [name]
       |           +--rw name    string
       +--rw ip-connection
       |  +--rw l3-termination-point?   string
       |  +--rw ipv4 {vpn-common:ipv4}?
       |  |  +--rw local-address?
       |  |  |       inet:ipv4-address
       |  |  +--rw prefix-length?
       |  |  |       uint8
       |  |  +--rw address-allocation-type?
       |  |  |       identityref
       |  |  +--rw (allocation-type)?
       |  |     +--:(provider-dhcp)
       |  |     |  +--rw dhcp-service-type?
       |  |     |  |       enumeration
       |  |     |  +--rw (service-type)?
       |  |     |     +--:(relay)
       |  |     |     |  +--rw server-ip-address*
       |  |     |     |          inet:ipv4-address
       |  |     |     +--:(server)
       |  |     |        +--rw (address-assign)?
       |  |     |           +--:(number)
       |  |     |           |  +--rw number-of-dynamic-address?
       |  |     |           |          uint16
       |  |     |           +--:(explicit)
       |  |     |              +--rw customer-addresses
       |  |     |                 +--rw address-pool* [pool-id]
       |  |     |                    +--rw pool-id          string
       |  |     |                    +--rw start-address
       |  |     |                    |       inet:ipv4-address
       |  |     |                    +--rw end-address?
       |  |     |                            inet:ipv4-address
       |  |     +--:(dhcp-relay)
       |  |     |  +--rw customer-dhcp-servers
       |  |     |     +--rw server-ip-address*   inet:ipv4-address
       |  |     +--:(static-addresses)
       |  |        +--rw primary-address?
       |  |        |       -> ../address/address-id
       |  |        +--rw address* [address-id]
       |  |           +--rw address-id          string
       |  |           +--rw customer-address?   inet:ipv4-address
       |  +--rw ipv6 {vpn-common:ipv6}?
       |     +--rw local-address?                 inet:ipv6-address
       |     +--rw prefix-length?                 uint8
       |     +--rw address-allocation-type?       identityref
       |     +--rw (allocation-type)?
       |        +--:(provider-dhcp)
       |        |  +--rw provider-dhcp
       |        |     +--rw dhcp-service-type?
       |        |     |       enumeration
       |        |     +--rw (service-type)?
       |        |        +--:(relay)
       |        |        |  +--rw server-ip-address*
       |        |        |          inet:ipv6-address
       |        |        +--:(server)
       |        |           +--rw (address-assign)?
       |        |              +--:(number)
       |        |              |  +--rw number-of-dynamic-address?
       |        |              |          uint16
       |        |              +--:(explicit)
       |        |                 +--rw customer-addresses
       |        |                    +--rw address-pool* [pool-id]
       |        |                       +--rw pool-id          string
       |        |                       +--rw start-address
       |        |                       |       inet:ipv6-address
       |        |                       +--rw end-address?
       |        |                               inet:ipv6-address
       |        +--:(dhcp-relay)
       |        |  +--rw customer-dhcp-servers
       |        |     +--rw server-ip-address*   inet:ipv6-address
       |        +--:(static-addresses)
       |           +--rw primary-address?
       |           |       -> ../address/address-id
       |           +--rw address* [address-id]
       |              +--rw address-id          string
       |              +--rw customer-address?   inet:ipv6-address
       +--rw routing-protocols
       |  +--rw routing-protocol* [id]
       |     +--rw id                  string
       |     +--rw type?               identityref
       |     +--rw routing-profiles* [id]
       |     |  +--rw id      leafref
       |     |  +--rw type?   identityref
       |     +--rw static
       |     |  +--rw cascaded-lan-prefixes
       |     |     +--rw ipv4-lan-prefixes* [lan next-hop]
       |     |     |       {vpn-common:ipv4}?
       |     |     |  +--rw lan           inet:ipv4-prefix
       |     |     |  +--rw lan-tag?      string
       |     |     |  +--rw next-hop      union
       |     |     |  +--rw bfd-enable?   boolean {vpn-common:bfd}?
       |     |     |  +--rw metric?       uint32
       |     |     |  +--rw preference?   uint32
       |     |     |  +--rw status
       |     |     |     +--rw admin-status
       |     |     |     |  +--rw status?        identityref
       |     |     |     |  +--rw last-change?   yang:date-and-time
       |     |     |     +--ro oper-status
       |     |     |        +--ro status?        identityref
       |     |     |        +--ro last-change?   yang:date-and-time
       |     |     +--rw ipv6-lan-prefixes* [lan next-hop]
       |     |             {vpn-common:ipv6}?
       |     |        +--rw lan           inet:ipv6-prefix
       |     |        +--rw lan-tag?      string
       |     |        +--rw next-hop      union
       |     |        +--rw bfd-enable?   boolean {vpn-common:bfd}?
       |     |        +--rw metric?       uint32
       |     |        +--rw preference?   uint32
       |     |        +--rw status
       |     |           +--rw admin-status
       |     |           |  +--rw status?        identityref
       |     |           |  +--rw last-change?   yang:date-and-time
       |     |           +--ro oper-status
       |     |              +--ro status?        identityref
       |     |              +--ro last-change?   yang:date-and-time
       |     +--rw bgp
       |     |  +--rw peer-groups
       |     |  |  +--rw peer-group* [name]
       |     |  |     +--rw name                      string
       |     |  |     +--rw local-address?            union
       |     |  |     +--rw description?              string
       |     |  |     +--rw local-as?                 inet:as-number
       |     |  |     +--rw peer-as                   inet:as-number
       |     |  |     +--rw address-family?           identityref
       |     |  |     +--rw multihop?                 uint8
       |     |  |     +--rw as-override?              boolean
       |     |  |     +--rw allow-own-as?             uint8
       |     |  |     +--rw prepend-global-as?        boolean
       |     |  |     +--rw send-default-route?       boolean
       |     |  |     +--rw site-of-origin?
       |     |  |     |       rt-types:route-origin
       |     |  |     +--rw ipv6-site-of-origin?
       |     |  |     |       rt-types:ipv6-route-origin
       |     |  |     +--rw redistribute-connected* [address-family]
       |     |  |     |  +--rw address-family    identityref
       |     |  |     |  +--rw enable?           boolean
       |     |  |     +--rw bgp-max-prefix
       |     |  |     |  +--rw max-prefix?          uint32
       |     |  |     |  +--rw warning-threshold?   decimal64
       |     |  |     |  +--rw violate-action?      enumeration
       |     |  |     |  +--rw restart-timer?       uint32
       |     |  |     +--rw bgp-timers
       |     |  |     |  +--rw keepalive?   uint16
       |     |  |     |  +--rw hold-time?   uint16
       |     |  |     +--rw authentication
       |     |  |        +--rw enable?            boolean
       |     |  |        +--rw keying-material
       |     |  |           +--rw (option)?
       |     |  |              +--:(ao)
       |     |  |              |  +--rw enable-ao?          boolean
       |     |  |              |  +--rw ao-keychain?
       |     |  |              |          key-chain:key-chain-ref
       |     |  |              +--:(md5)
       |     |  |              |  +--rw md5-keychain?
       |     |  |              |          key-chain:key-chain-ref
       |     |  |              +--:(explicit)
       |     |  |              |  +--rw key-id?             uint32
       |     |  |              |  +--rw key?                string
       |     |  |              |  +--rw crypto-algorithm?
       |     |  |              |          identityref
       |     |  |              +--:(ipsec)
       |     |  |                 +--rw sa?                 string
       |     |  +--rw neighbor* [remote-address]
       |     |     +--rw remote-address            inet:ip-address
       |     |     +--rw local-address?            union
       |     |     +--rw peer-group?
       |     |     |       -> ../../peer-groups/peer-group/name
       |     |     +--rw description?              string
       |     |     +--rw local-as?                 inet:as-number
       |     |     +--rw peer-as                   inet:as-number
       |     |     +--rw address-family?           identityref
       |     |     +--rw multihop?                 uint8
       |     |     +--rw as-override?              boolean
       |     |     +--rw allow-own-as?             uint8
       |     |     +--rw prepend-global-as?        boolean
       |     |     +--rw send-default-route?       boolean
       |     |     +--rw site-of-origin?
       |     |     |       rt-types:route-origin
       |     |     +--rw ipv6-site-of-origin?
       |     |     |       rt-types:ipv6-route-origin
       |     |     +--rw redistribute-connected* [address-family]
       |     |     |  +--rw address-family    identityref
       |     |     |  +--rw enable?           boolean
       |     |     +--rw bgp-max-prefix
       |     |     |  +--rw max-prefix?          uint32
       |     |     |  +--rw warning-threshold?   decimal64
       |     |     |  +--rw violate-action?      enumeration
       |     |     |  +--rw restart-timer?       uint32
       |     |     +--rw bgp-timers
       |     |     |  +--rw keepalive?   uint16
       |     |     |  +--rw hold-time?   uint16
       |     |     +--rw authentication
       |     |     |  +--rw enable?            boolean
       |     |     |  +--rw keying-material
       |     |     |     +--rw (option)?
       |     |     |        +--:(ao)
       |     |     |        |  +--rw enable-ao?          boolean
       |     |     |        |  +--rw ao-keychain?
       |     |     |        |          key-chain:key-chain-ref
       |     |     |        +--:(md5)
       |     |     |        |  +--rw md5-keychain?
       |     |     |        |          key-chain:key-chain-ref
       |     |     |        +--:(explicit)
       |     |     |        |  +--rw key-id?             uint32
       |     |     |        |  +--rw key?                string
       |     |     |        |  +--rw crypto-algorithm?   identityref
       |     |     |        +--:(ipsec)
       |     |     |           +--rw sa?                 string
       |     |     +--rw status
       |     |        +--rw admin-status
       |     |        |  +--rw status?        identityref
       |     |        |  +--rw last-change?   yang:date-and-time
       |     |        +--ro oper-status
       |     |           +--ro status?        identityref
       |     |           +--ro last-change?   yang:date-and-time
       |     +--rw ospf
       |     |  +--rw address-family?   identityref
       |     |  +--rw area-id           yang:dotted-quad
       |     |  +--rw metric?           uint16
       |     |  +--rw sham-links {vpn-common:rtg-ospf-sham-link}?
       |     |  |  +--rw sham-link* [target-site]
       |     |  |     +--rw target-site    string
       |     |  |     +--rw metric?        uint16
       |     |  +--rw max-lsa?          uint32
       |     |  +--rw authentication
       |     |  |  +--rw enable?            boolean
       |     |  |  +--rw keying-material
       |     |  |     +--rw (option)?
       |     |  |        +--:(auth-key-chain)
       |     |  |        |  +--rw key-chain?
       |     |  |        |          key-chain:key-chain-ref
       |     |  |        +--:(auth-key-explicit)
       |     |  |        |  +--rw key-id?             uint32
       |     |  |        |  +--rw key?                string
       |     |  |        |  +--rw crypto-algorithm?   identityref
       |     |  |        +--:(ipsec)
       |     |  |           +--rw sa?                 string
       |     |  +--rw status
       |     |     +--rw admin-status
       |     |     |  +--rw status?        identityref
       |     |     |  +--rw last-change?   yang:date-and-time
       |     |     +--ro oper-status
       |     |        +--ro status?        identityref
       |     |        +--ro last-change?   yang:date-and-time
       |     +--rw isis
       |     |  +--rw address-family?   identityref
       |     |  +--rw area-address      l3nm:area-address
       |     |  +--rw level?            identityref
       |     |  +--rw metric?           uint16
       |     |  +--rw mode?             enumeration
       |     |  +--rw authentication
       |     |  |  +--rw enable?            boolean
       |     |  |  +--rw keying-material
       |     |  |     +--rw (option)?
       |     |  |        +--:(auth-key-chain)
       |     |  |        |  +--rw key-chain?
       |     |  |        |          key-chain:key-chain-ref
       |     |  |        +--:(auth-key-explicit)
       |     |  |           +--rw key-id?             uint32
       |     |  |           +--rw key?                string
       |     |  |           +--rw crypto-algorithm?   identityref
       |     |  +--rw status
       |     |     +--rw admin-status
       |     |     |  +--rw status?        identityref
       |     |     |  +--rw last-change?   yang:date-and-time
       |     |     +--ro oper-status
       |     |        +--ro status?        identityref
       |     |        +--ro last-change?   yang:date-and-time
       |     +--rw rip
       |     |  +--rw address-family?   identityref
       |     |  +--rw timers
       |     |  |  +--rw update-interval?     uint16
       |     |  |  +--rw invalid-interval?    uint16
       |     |  |  +--rw holddown-interval?   uint16
       |     |  |  +--rw flush-interval?      uint16
       |     |  +--rw default-metric?   uint8
       |     |  +--rw authentication
       |     |  |  +--rw enable?            boolean
       |     |  |  +--rw keying-material
       |     |  |     +--rw (option)?
       |     |  |        +--:(auth-key-chain)
       |     |  |        |  +--rw key-chain?
       |     |  |        |          key-chain:key-chain-ref
       |     |  |        +--:(auth-key-explicit)
       |     |  |           +--rw key?                string
       |     |  |           +--rw crypto-algorithm?   identityref
       |     |  +--rw status
       |     |     +--rw admin-status
       |     |     |  +--rw status?        identityref
       |     |     |  +--rw last-change?   yang:date-and-time
       |     |     +--ro oper-status
       |     |        +--ro status?        identityref
       |     |        +--ro last-change?   yang:date-and-time
       |     +--rw vrrp
       |        +--rw address-family?       identityref
       |        +--rw vrrp-group?           uint8
       |        +--rw backup-peer?          inet:ip-address
       |        +--rw virtual-ip-address*   inet:ip-address
       |        +--rw priority?             uint8
       |        +--rw ping-reply?           boolean
       |        +--rw status
       |           +--rw admin-status
       |           |  +--rw status?        identityref
       |           |  +--rw last-change?   yang:date-and-time
       |           +--ro oper-status
       |              +--ro status?        identityref
       |              +--ro last-change?   yang:date-and-time
       +--rw oam
       |  +--rw bfd
       |     +--rw profile?                    leafref
       |     +--rw session-type?               identityref
       |     +--rw desired-min-tx-interval?    uint32
       |     +--rw required-min-rx-interval?   uint32
       |     +--rw local-multiplier?           uint8
       |     +--rw holdtime?                   uint32
       |     +--rw authentication!
       |     |  +--rw key-chain?    key-chain:key-chain-ref
       |     |  +--rw meticulous?   boolean
       |     +--rw status
       |        +--rw admin-status
       |        |  +--rw status?        identityref
       |        |  +--rw last-change?   yang:date-and-time
       |        +--ro oper-status
       |           +--ro status?        identityref
       |           +--ro last-change?   yang:date-and-time
       +--rw security
          +--rw encryption {vpn-common:encryption}?
          |  +--rw enabled?   boolean
          |  +--rw layer?     enumeration
          +--rw encryption-profile
             +--rw (profile)?
                +--:(provider-profile)
                |  +--rw profile-name?         leafref
                +--:(customer-profile)
                   +--rw customer-key-chain?
                           key-chain:key-chain-ref


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

This module uses types defined in XXX.

~~~~
<CODE BEGINS> file "ietf-ac-ntw@2022-11-30.yang"
module ietf-ac-ntw {
  yang-version 1.1;
  namespace "urn:ietf:params:xml:ns:yang:ietf-ac-ntw";
  prefix ac-ntw;

  import ietf-vpn-common {
    prefix vpn-common;
    reference
      "RFC 9181: A Common YANG Data Model for Layer 2 and Layer 3
                 VPNs";
  }
  import ietf-l3vpn-ntw {
    prefix l3nm;
    reference
      "RFC 9182: A YANG Network Data Model for Layer 3 VPNs";
  }
  import ietf-inet-types {
    prefix inet;
    reference
      "RFC 6991: Common YANG Data Types, Section 4";
  }
  import ietf-yang-types {
    prefix yang;
    reference
      "RFC 6991: Common YANG Data Types, Section 3";
  }
  import ietf-key-chain {
    prefix key-chain;
    reference
      "RFC 8177: YANG Data Model for Key Chains";
  }
  import ietf-routing-types {
    prefix rt-types;
    reference
      "RFC 8294: Common YANG Data Types for the Routing Area";
  }
  import ietf-interfaces {
    prefix if;
    reference
      "RFC 8343: A YANG Data Model for Interface Management";
  }
  import ieee802-dot1q-types {
    prefix dot1q-types;
    reference
      "IEEE Std 802.1Qcp: Bridges and Bridged Networks--
                          Amendment 30: YANG Data Model";
  }
  import ietf-network {
    prefix nw;
    reference
      "RFC 8345: A YANG Data Model for Network
                 Topologies, Section 6.1";
  }
  import ietf-sap-ntw {
    prefix sap;
    reference
      "RFC SSSS: A YANG Network Model for Service Attachment
                 Points (SAPs)";
  }

  organization
    "IETF OPSAWG (Operations and Management Area Working Group)";
  contact
    "WG Web:   <https://datatracker.ietf.org/wg/opsawg/>
     WG List:  <mailto:opsawg@ietf.org>

     Author:   Mohamed Boucadair
               <mailto:mohamed.boucadair@orange.com>
     Author:   Richard Roberts
               <mailto:rroberts@juniper.net>";
  description
    "This YANG module defines a generic YANG model for
     the configuration of attachment circuits.

     Copyright (c) 2022 IETF Trust and the persons identified as
     authors of the code.  All rights reserved.

     Redistribution and use in source and binary forms, with or
     without modification, is permitted pursuant to, and subject
     to the license terms contained in, the Revised BSD License
     set forth in Section 4.c of the IETF Trust's Legal Provisions
     Relating to IETF Documents
     (https://trustee.ietf.org/license-info).

     This version of this YANG module is part of RFC xxx; see the
     RFC itself for full legal notices.";

  revision 2022-11-30 {
    description
      "Initial revision.";
    reference
      "RFC xxxx: A YANG Data Model for Attachment Circuits";
  }

  // L2 conenction

  grouping l2-connection {
    description
      "Defines Layer 2 protocols and parameters that
       are required to enable AC connectivity.";
    container encapsulation {
      description
        "Container for Layer 2 encapsulation.";
      leaf encap-type {
        type identityref {
          base vpn-common:encapsulation-type;
        }
        default "vpn-common:priority-tagged";
        description
          "Tagged interface type.  By default, the
           type of the tagged interface is
           'priority-tagged'.";
      }
      container dot1q {
        when "derived-from-or-self(../encap-type, "
           + "'vpn-common:dot1q')" {
          description
            "Only applies when the type of the
             tagged interface is 'dot1q'.";
        }
        description
          "Tagged interface.";
        leaf tag-type {
          type identityref {
            base vpn-common:tag-type;
          }
          default "vpn-common:c-vlan";
          description
            "Tag type.  By default, the tag type is
             'c-vlan'.";
        }
        leaf cvlan-id {
          type dot1q-types:vlanid;
          description
            "VLAN identifier.";
        }
        container tag-operations {
          description
            "Sets the tag manipulation policy for this
             VPN network access.  It defines a set of
             tag manipulations that allow for the
             insertion, removal, or rewriting
             of 802.1Q VLAN tags.  These operations are
             indicated for the CE-PE direction.
             By default, tag operations are symmetric.
             As such, the reverse tag operation is
             assumed on the PE-CE direction.";
          choice op-choice {
            description
              "Selects the tag rewriting policy for a
               VPN network access.";
            leaf pop {
              type empty;
              description
                "Pop the outer tag.";
            }
            leaf push {
              type empty;
              description
                "Pushes one or two tags defined by the
                 tag-1 and tag-2 leaves.  It is
                 assumed that, absent any policy, the
                 default value of 0 will be used for
                 the PCP setting.";
            }
            leaf translate {
              type empty;
              description
                "Translates the outer tag to one or two
                 tags.  PCP bits are preserved.";
            }
          }
          leaf tag-1 {
            when 'not(../pop)';
            type dot1q-types:vlanid;
            description
              "A first tag to be used for push or
               translate operations.  This tag will be
               used as the outermost tag as a result
               of the tag operation.";
          }
          leaf tag-1-type {
            type dot1q-types:dot1q-tag-type;
            default "dot1q-types:s-vlan";
            description
              "Specifies a specific 802.1Q tag type
               of tag-1.";
          }
          leaf tag-2 {
            when '(../translate)';
            type dot1q-types:vlanid;
            description
              "A second tag to be used for
               translation.";
          }
          leaf tag-2-type {
            type dot1q-types:dot1q-tag-type;
            default "dot1q-types:c-vlan";
            description
              "Specifies a specific 802.1Q tag type
               of tag-2.";
          }
        }
      }
      container priority-tagged {
        when "derived-from-or-self(../encap-type, "
           + "'vpn-common:priority-tagged')" {
          description
            "Only applies when the type of the
             tagged interface is 'priority-tagged'.";
        }
        description
          "Priority tagged container.";
        leaf tag-type {
          type identityref {
            base vpn-common:tag-type;
          }
          default "vpn-common:c-vlan";
          description
            "Tag type.  By default, the tag type is
             'c-vlan'.";
        }
      }
      container qinq {
        when "derived-from-or-self(../encap-type, "
           + "'vpn-common:qinq')" {
          description
            "Only applies when the type of the tagged
             interface is 'QinQ'.";
        }
        description
          "Includes QinQ parameters.";
        leaf tag-type {
          type identityref {
            base vpn-common:tag-type;
          }
          default "vpn-common:s-c-vlan";
          description
            "Tag type.  By default, the tag type is
             's-c-vlan'.";
        }
        leaf svlan-id {
          type dot1q-types:vlanid;
          mandatory true;
          description
            "S-VLAN identifier.";
        }
        leaf cvlan-id {
          type dot1q-types:vlanid;
          mandatory true;
          description
            "C-VLAN identifier.";
        }
        container tag-operations {
          description
            "Sets the tag manipulation policy for this
             VPN network access.  It defines a set of
             tag manipulations that allow for the
             insertion, removal, or rewriting
             of 802.1Q VLAN tags.  These operations are
             indicated for the CE-PE direction.
             By default, tag operations are symmetric.
             As such, the reverse tag operation is
             assumed on the PE-CE direction.";
          choice op-choice {
            description
              "Selects the tag rewriting policy for a
               VPN network access.";
            leaf pop {
              type uint8 {
                range "1|2";
              }
              description
                "Pops one or two tags as a function
                 of the indicated pop value.";
            }
            leaf push {
              type empty;
              description
                "Pushes one or two tags defined by the
                 tag-1 and tag-2 leaves.  It is
                 assumed that, absent any policy, the
                 default value of 0 will be used for
                 PCP setting.";
            }
            leaf translate {
              type uint8 {
                range "1|2";
              }
              description
                "Translates one or two outer tags.  PCP
                 bits are preserved.

                 The following operations are
                 supported:

                 - translate 1 with tag-1 leaf is
                   provided: only the outermost tag is
                   translated to the value in tag-1.

                 - translate 2 with both tag-1 and
                   tag-2 leaves are provided: both
                   outer and inner tags are translated
                   to the values in tag-1 and tag-2,
                   respectively.

                 - translate 2 with tag-1 leaf is
                   provided: the outer tag is popped
                   while the inner tag is translated
                   to the value in tag-1.";
            }
          }
          leaf tag-1 {
            when 'not(../pop)';
            type dot1q-types:vlanid;
            description
              "A first tag to be used for push or
               translate operations.  This tag will be
               used as the outermost tag as a result
               of the tag operation.";
          }
          leaf tag-1-type {
            type dot1q-types:dot1q-tag-type;
            default "dot1q-types:s-vlan";
            description
              "Specifies a specific 802.1Q tag type
               of tag-1.";
          }
          leaf tag-2 {
            when 'not(../pop)';
            type dot1q-types:vlanid;
            description
              "A second tag to be used for push or
               translate operations.";
          }
          leaf tag-2-type {
            type dot1q-types:dot1q-tag-type;
            default "dot1q-types:c-vlan";
            description
              "Specifies a specific 802.1Q tag type
               of tag-2.";
          }
        }
      }
    }
    choice l2-service {
      description
        "The Layer 2 connectivity service can be
         provided by indicating a pointer to an
         L2VPN or by specifying a Layer 2 tunnel
         service.";
      container l2-tunnel-service {
        description
          "Defines a Layer 2 tunnel termination.
           It is only applicable when a tunnel is
           required.  The supported values are
           'pseudowire', 'vpls', and 'vxlan'.  Other
           values may be defined, if needed.";
        leaf type {
          type identityref {
            base l3nm:l2-tunnel-type;
          }
          description
            "Selects the tunnel termination option
             for each AC Endpoint.";
        }
        container pseudowire {
          when "derived-from-or-self(../type, "
             + "'pseudowire')" {
            description
              "Only applies when the Layer 2 service
               type is 'pseudowire'.";
          }
          description
            "Includes pseudowire termination
             parameters.";
          leaf vcid {
            type uint32;
            description
              "Indicates a pseudowire (PW) or
               virtual circuit (VC) identifier.";
          }
          leaf far-end {
            type union {
              type uint32;
              type inet:ip-address;
            }
            description
              "Neighbor reference.";
            reference
              "RFC 8077: Pseudowire Setup and
                         Maintenance Using the Label
                         Distribution Protocol
                         (LDP), Section 6.1";
          }
        }
        container vpls {
          when "derived-from-or-self(../type, "
             + "'vpls')" {
            description
              "Only applies when the Layer 2 service
               type is 'vpls'.";
          }
          description
            "VPLS termination parameters.";
          leaf vcid {
            type uint32;
            description
              "VC identifier.";
          }
          leaf-list far-end {
            type union {
              type uint32;
              type inet:ip-address;
            }
            description
              "Neighbor reference.";
          }
        }
        container vxlan {
          when "derived-from-or-self(../type, "
             + "'vxlan')" {
            description
              "Only applies when the Layer 2 service
               type is 'vxlan'.";
          }
          description
            "VXLAN termination parameters.";
          leaf vni-id {
            type uint32;
            mandatory true;
            description
              "VXLAN Network Identifier (VNI).";
          }
          leaf peer-mode {
            type identityref {
              base vpn-common:vxlan-peer-mode;
            }
            default "vpn-common:static-mode";
            description
              "Specifies the VXLAN access mode.  By
               default, the peer mode is set to
               'static-mode'.";
          }
          leaf-list peer-ip-address {
            type inet:ip-address;
            description
              "List of a peer's IP addresses.";
          }
        }
      }
      case l2vpn {
        leaf l2vpn-id {
          type vpn-common:vpn-id;
          description
            "Indicates the L2VPN service associated
             with an Integrated Routing and Bridging
             (IRB) interface.";
        }
      }
    }
  }

  grouping l2-connection-if-ref {
    description
      "Defines Layer 2 protocols and parameters that
       are required to enable AC connectivity.";
    uses l2-connection;
    leaf l2-termination-point {
      type string;
      description
        "Specifies a reference to a local Layer 2
         termination point, such as a Layer 2
         sub-interface.";
    }
    leaf local-bridge-reference {
      type string;
      description
        "Specifies a local bridge reference to
         accommodate, for example, implementations
         that require internal bridging.
         A reference may be a local bridge domain.";
    }
    leaf bearer-reference {
      if-feature "vpn-common:bearer-reference";
      type string;
      description
        "This is an internal reference for the
         service provider to identify the bearer
         associated with this VPN.";
    }
    container lag-interface {
      if-feature "vpn-common:lag-interface";
      description
        "Container for configuration of Link
         Aggregation Group (LAG) interface
         attributes.";
      leaf lag-interface-id {
        type string;
        description
          "LAG interface identifier.";
      }
      container member-link-list {
        description
          "Container for the member link list.";
        list member-link {
          key "name";
          description
            "Member link.";
          leaf name {
            type string;
            description
              "Member link name.";
          }
        }
      }
    }
  }

  grouping ip-connection {
    description
      "Defines IP connection parameters.";
    leaf l3-termination-point {
      type string;
      description
        "Specifies a reference to a local Layer 3
         termination point, such as a bridge domain
         interface.";
    }
    container ipv4 {
      if-feature "vpn-common:ipv4";
      description
        "IPv4-specific parameters.";
      leaf local-address {
        type inet:ipv4-address;
        description
          "The IP address used at the provider's
           interface.";
      }
      leaf prefix-length {
        type uint8 {
          range "0..32";
        }
        description
          "Subnet prefix length expressed in bits.
           It is applied to both local and customer
           addresses.";
      }
      leaf address-allocation-type {
        type identityref {
          base l3nm:address-allocation-type;
        }
        must "not(derived-from-or-self(current(), "
           + "'slaac') or "
           + "derived-from-or-self(current(), "
           + "'provider-dhcp-slaac'))" {
          error-message "SLAAC is only applicable "
                      + "to IPv6.";
        }
        description
          "Defines how addresses are allocated to
           the peer site.

           If there is no value for the address
           allocation type, then IPv4 addressing
           is not enabled.";
      }
      choice allocation-type {
        description
          "Choice of the IPv4 address allocation.";
        case provider-dhcp {
          description
            "Parameters related to DHCP-allocated
             addresses.  IP addresses are allocated
             by DHCP, which is provided by the
             operator.";
          leaf dhcp-service-type {
            type enumeration {
              enum server {
                description
                  "Local DHCP server.";
              }
              enum relay {
                description
                  "Local DHCP relay.  DHCP requests
                   are relayed to a provider's
                   server.";
              }
            }
            description
              "Indicates the type of DHCP service to
               be enabled on this access.";
          }
          choice service-type {
            description
              "Choice based on the DHCP service
               type.";
            case relay {
              description
                "Container for a list of the
                 provider's DHCP servers (i.e.,
                 'dhcp-service-type' is set to
                 'relay').";
              leaf-list server-ip-address {
                type inet:ipv4-address;
                description
                  "IPv4 addresses of the provider's
                   DHCP server, for use by the local
                   DHCP relay.";
              }
            }
            case server {
              description
                "A choice for how addresses are
                 assigned when a local DHCP server
                 is enabled.";
              choice address-assign {
                default "number";
                description
                  "A choice for how IPv4 addresses
                   are assigned.";
                case number {
                  leaf number-of-dynamic-address {
                    type uint16;
                    default "1";
                    description
                      "Specifies the number of IP
                       addresses to be assigned to
                       the customer on this
                       access.";
                  }
                }
                case explicit {
                  container customer-addresses {
                    description
                      "Container for customer
                       addresses to be allocated
                       using DHCP.";
                    list address-pool {
                      key "pool-id";
                      description
                        "Describes IP addresses to
                         be allocated by DHCP.

                         When only 'start-address'
                         is present, it represents a
                         single address.

                         When both 'start-address'
                         and 'end-address' are
                         specified, it implies a
                         range inclusive of both
                         addresses.";
                      leaf pool-id {
                        type string;
                        description
                          "A pool identifier for the
                           address range from
                           'start-address' to
                           'end-address'.";
                      }
                      leaf start-address {
                        type inet:ipv4-address;
                        mandatory true;
                        description
                          "Indicates the first
                           address in the pool.";
                      }
                      leaf end-address {
                        type inet:ipv4-address;
                        description
                          "Indicates the last
                           address in the pool.";
                      }
                    }
                  }
                }
              }
            }
          }
        }
        case dhcp-relay {
          description
            "The DHCP relay is provided by the
             operator.";
          container customer-dhcp-servers {
            description
              "Container for a list of the
               customer's DHCP servers.";
            leaf-list server-ip-address {
              type inet:ipv4-address;
              description
                "IPv4 addresses of the customer's
                 DHCP server.";
            }
          }
        }
        case static-addresses {
          description
            "Lists the IPv4 addresses that are
             used.";
          leaf primary-address {
            type leafref {
              path "../address/address-id";
            }
            description
              "Primary address of the connection.";
          }
          list address {
            key "address-id";
            description
              "Lists the IPv4 addresses that are
               used.";
            leaf address-id {
              type string;
              description
                "An identifier of the static IPv4
                 address.";
            }
            leaf customer-address {
              type inet:ipv4-address;
              description
                "IPv4 address of the customer
                 side.";
            }
          }
        }
      }
    }
    container ipv6 {
      if-feature "vpn-common:ipv6";
      description
        "IPv6-specific parameters.";
      leaf local-address {
        type inet:ipv6-address;
        description
          "IPv6 address of the provider side.";
      }
      leaf prefix-length {
        type uint8 {
          range "0..128";
        }
        description
          "Subnet prefix length expressed in bits.
           It is applied to both local and customer
           addresses.";
      }
      leaf address-allocation-type {
        type identityref {
          base l3nm:address-allocation-type;
        }
        description
          "Defines how addresses are allocated.
           If there is no value for the address
           allocation type, then IPv6 addressing is
           disabled.";
      }
      choice allocation-type {
        description
          "A choice based on the IPv6 allocation
           type.";
        container provider-dhcp {
          when "derived-from-or-self(../address-allo"
             + "cation-type, 'provider-dhcp') or "
             + "derived-from-or-self(../address-allo"
             + "cation-type, 'provider-dhcp-slaac')" {
            description
              "Only applies when addresses are
               allocated by DHCPv6 as provided by
               the operator.";
          }
          description
            "Parameters related to DHCP-allocated
             addresses.";
          leaf dhcp-service-type {
            type enumeration {
              enum server {
                description
                  "Local DHCPv6 server.";
              }
              enum relay {
                description
                  "DHCPv6 relay.";
              }
            }
            description
              "Indicates the type of the DHCPv6
               service to be enabled on this
               access.";
          }
          choice service-type {
            description
              "Choice based on the DHCPv6 service
               type.";
            case relay {
              leaf-list server-ip-address {
                type inet:ipv6-address;
                description
                  "IPv6 addresses of the provider's
                   DHCPv6 server.";
              }
            }
            case server {
              choice address-assign {
                default "number";
                description
                  "Choice for how IPv6 prefixes are
                   assigned by the DHCPv6 server.";
                case number {
                  leaf number-of-dynamic-address {
                    type uint16;
                    default "1";
                    description
                      "Describes the number of IPv6
                       prefixes that are allocated
                       to the customer on this
                       access.";
                  }
                }
                case explicit {
                  container customer-addresses {
                    description
                      "Container for customer IPv6
                       addresses allocated by
                       DHCPv6.";
                    list address-pool {
                      key "pool-id";
                      description
                        "Describes IPv6 addresses
                         allocated by DHCPv6.

                         When only 'start-address'
                         is present, it represents a
                         single address.

                         When both 'start-address'
                         and 'end-address' are
                         specified, it implies a
                         range inclusive of both
                         addresses.";
                      leaf pool-id {
                        type string;
                        description
                          "A pool identifier for the
                           address range from
                           'start-address' to
                           'end-address'.";
                      }
                      leaf start-address {
                        type inet:ipv6-address;
                        mandatory true;
                        description
                          "Indicates the first
                           address.";
                      }
                      leaf end-address {
                        type inet:ipv6-address;
                        description
                          "Indicates the last
                           address.";
                      }
                    }
                  }
                }
              }
            }
          }
        }
        case dhcp-relay {
          description
            "DHCPv6 relay provided by the
             operator.";
          container customer-dhcp-servers {
            description
              "Container for a list of the
               customer's DHCP servers.";
            leaf-list server-ip-address {
              type inet:ipv6-address;
              description
                "Contains the IP addresses of the
                 customer's DHCPv6 server.";
            }
          }
        }
        case static-addresses {
          description
            "IPv6-specific parameters for static
             allocation.";
          leaf primary-address {
            type leafref {
              path "../address/address-id";
            }
            description
              "Principal address of the
               connection.";
          }
          list address {
            key "address-id";
            description
              "Describes IPv6 addresses that are
               used.";
            leaf address-id {
              type string;
              description
                "An identifier of an IPv6 address.";
            }
            leaf customer-address {
              type inet:ipv6-address;
              description
                "An IPv6 address of the customer
                 side.";
            }
          }
        }
      }
    }
  }

  /* Routing */
  //BGP-generic
  //BGP-extended

  grouping bgp-base {
    description
      "Configuration specific to BGP.";
    leaf description {
      type string;
      description
        "Includes a description of the BGP
         session.

         This description is meant to be used
         for diagnostic purposes.  The semantic
         of the description is local to an
         implementation.";
    }
    leaf local-as {
      type inet:as-number;
      description
        "Indicates a local AS Number (ASN), if
         an ASN distinct from the ASN configured
         at the VPN node level is needed.";
    }
    leaf peer-as {
      type inet:as-number;
      mandatory true;
      description
        "Indicates the customer's ASN when
         the customer requests BGP routing.";
    }
    leaf address-family {
      type identityref {
        base vpn-common:address-family;
      }
      description
        "This node contains the address families
         to be activated.  'dual-stack' means
         that both IPv4 and IPv6 will be
         activated.";
    }
    leaf multihop {
      type uint8;
      description
        "Describes the number of IP hops allowed
         between a given BGP neighbor and
         the PE.";
    }
    leaf as-override {
      type boolean;
      default "false";
      description
        "Defines whether ASN override is
         enabled, i.e., replacing the ASN of
         the customer specified in the AS_PATH
         attribute with the local ASN.";
    }
    leaf allow-own-as {
      type uint8;
      default "0";
      description
        "If set, specifies the maximum number of
         occurrences of the provider's ASN that
         are permitted within the AS_PATH
         before it is rejected.";
    }
    leaf prepend-global-as {
      type boolean;
      default "false";
      description
        "In some situations, the ASN that is
         provided at the VPN node level may be
         distinct from the ASN configured at the
         VPN network access level.  When such
         ASNs are provided, they are both
         prepended to the BGP route updates
         for this access.  To disable that
         behavior, 'prepend-global-as'
         must be set to 'false'.  In such a
         case, the ASN that is provided at
         the VPN node level is not prepended
         to the BGP route updates for
         this access.";
    }
    leaf send-default-route {
      type boolean;
      default "false";
      description
        "Defines whether default routes can be
         advertised to a peer.  If set, the
         default routes are advertised to a
         peer.";
    }
    leaf site-of-origin {
      when "../address-family = 'vpn-common:ipv4' "
         + "or 'vpn-common:dual-stack'" {
        description
          "Only applies if IPv4 is activated.";
      }
      type rt-types:route-origin;
      description
        "The Site of Origin attribute is encoded
         as a Route Origin Extended Community.
         It is meant to uniquely identify the
         set of routes learned from a site via a
         particular CE-PE connection and is used
         to prevent routing loops.";
      reference
        "RFC 4364: BGP/MPLS IP Virtual Private
                   Networks (VPNs), Section 7";
    }
    leaf ipv6-site-of-origin {
      when "../address-family = 'vpn-common:ipv6' "
         + "or 'vpn-common:dual-stack'" {
        description
          "Only applies if IPv6 is activated.";
      }
      type rt-types:ipv6-route-origin;
      description
        "The IPv6 Site of Origin attribute is
         encoded as an IPv6 Route Origin
         Extended Community.  It is meant to
         uniquely identify the set of routes
         learned from a site via VRF
         information.";
      reference
        "RFC 5701: IPv6 Address Specific BGP
                   Extended Community
                   Attribute";
    }
    list redistribute-connected {
      key "address-family";
      description
        "Indicates, per address family, the
         policy to follow for connected
         routes.";
      leaf address-family {
        type identityref {
          base vpn-common:address-family;
        }
        description
          "Indicates the address family.";
      }
      leaf enable {
        type boolean;
        description
          "Enables the redistribution of
           connected routes.";
      }
    }
    container bgp-max-prefix {
      description
        "Controls the behavior when a prefix
         maximum is reached.";
      leaf max-prefix {
        type uint32;
        default "5000";
        description
          "Indicates the maximum number of BGP
           prefixes allowed in the BGP session.

           It allows control of how many
           prefixes can be received from a
           neighbor.

           If the limit is exceeded, the action
           indicated in 'violate-action' will be
           followed.";
        reference
          "RFC 4271: A Border Gateway Protocol 4
                     (BGP-4), Section 8.2.2";
      }
      leaf warning-threshold {
        type decimal64 {
          fraction-digits 5;
          range "0..100";
        }
        units "percent";
        default "75";
        description
          "When this value is reached, a warning
           notification will be triggered.";
      }
      leaf violate-action {
        type enumeration {
          enum warning {
            description
              "Only a warning message is sent to
               the peer when the limit is
               exceeded.";
          }
          enum discard-extra-paths {
            description
              "Discards extra paths when the
               limit is exceeded.";
          }
          enum restart {
            description
              "The BGP session restarts after
               the indicated time interval.";
          }
        }
        description
          "If the BGP neighbor 'max-prefix'
           limit is reached, the action
           indicated in 'violate-action'
           will be followed.";
      }
      leaf restart-timer {
        type uint32;
        units "seconds";
        description
          "Time interval after which the BGP
           session will be reestablished.";
      }
    }
    container bgp-timers {
      description
        "Includes two BGP timers that can be
         customized when building a VPN service
         with BGP used as the CE-PE routing
         protocol.";
      leaf keepalive {
        type uint16 {
          range "0..21845";
        }
        units "seconds";
        default "30";
        description
          "This timer indicates the KEEPALIVE
           messages' frequency between a PE
           and a BGP peer.

           If set to '0', it indicates that
           KEEPALIVE messages are disabled.

           It is suggested that the maximum
           time between KEEPALIVE messages be
           one-third of the Hold Time
           interval.";
        reference
          "RFC 4271: A Border Gateway Protocol 4
                     (BGP-4), Section 4.4";
      }
      leaf hold-time {
        type uint16 {
          range "0 | 3..65535";
        }
        units "seconds";
        default "90";
        description
          "Indicates the maximum number of
           seconds that may elapse between the
           receipt of successive KEEPALIVE
           and/or UPDATE messages from the peer.

           The Hold Time must be either zero or
           at least three seconds.";
        reference
          "RFC 4271: A Border Gateway Protocol 4
                     (BGP-4), Section 4.2";
      }
    }
  }

  grouping bgp-base-with-auth {
    description
      "Configuration specific to BGP.";
    uses bgp-base;
    container authentication {
      description
        "Container for BGP authentication
         parameters between a PE and a CE.";
      leaf enable {
        type boolean;
        default "false";
        description
          "Enables or disables authentication.";
      }
      container keying-material {
        when "../enable = 'true'";
        description
          "Container for describing how a BGP
           routing session is to be secured
           between a PE and a CE.";
        choice option {
          description
            "Choice of authentication options.";
          case ao {
            description
              "Uses the TCP Authentication
               Option (TCP-AO).";
            reference
              "RFC 5925: The TCP Authentication
                         Option";
            leaf enable-ao {
              type boolean;
              description
                "Enables the TCP-AO.";
            }
            leaf ao-keychain {
              type key-chain:key-chain-ref;
              description
                "Reference to the TCP-AO key
                 chain.";
              reference
                "RFC 8177: YANG Data Model for
                           Key Chains";
            }
          }
          case md5 {
            description
              "Uses MD5 to secure the session.";
            reference
              "RFC 4364: BGP/MPLS IP Virtual
                         Private Networks
                         (VPNs), Section 13.2";
            leaf md5-keychain {
              type key-chain:key-chain-ref;
              description
                "Reference to the MD5 key
                 chain.";
              reference
                "RFC 8177: YANG Data Model for
                           Key Chains";
            }
          }
          case explicit {
            leaf key-id {
              type uint32;
              description
                "Key identifier.";
            }
            leaf key {
              type string;
              description
                "BGP authentication key.
                 This model only supports the
                 subset of keys that are
                 representable as ASCII
                 strings.";
            }
            leaf crypto-algorithm {
              type identityref {
                base key-chain:crypto-algorithm;
              }
              description
                "Indicates the cryptographic
                 algorithm associated with the
                 key.";
            }
          }
          case ipsec {
            description
              "Specifies a reference to an
               Internet Key Exchange Protocol
               (IKE) Security Association
               (SA).";
            leaf sa {
              type string;
              description
                "Indicates the
                 administrator-assigned name
                 of the SA.";
            }
          }
        }
      }
    }
  }

  //routing profile

  grouping routing-profile {
    description
      "Defines routing protocols.";
    list routing-protocol {
      key "id";
      description
        "List of routing protocols used on the
         CE-PE link.  This list can be augmented.";
      leaf id {
        type string;
        description
          "Unique identifier for the routing
           protocol.";
      }
      leaf type {
        type identityref {
          base vpn-common:routing-protocol-type;
        }
        description
          "Type of routing protocol.";
      }
      container bgp {
        when "derived-from-or-self(../type, "
           + "'vpn-common:bgp-routing')" {
          description
            "Only applies when the protocol is
             BGP.";
        }
        description
          "Configuration specific to BGP.";
        uses bgp-base;
      }
      container ospf {
        when "derived-from-or-self(../type, "
           + "'vpn-common:ospf-routing')" {
          description
            "Only applies when the protocol is
             OSPF.";
        }
        description
          "Configuration specific to OSPF.";
        leaf address-family {
          type identityref {
            base vpn-common:address-family;
          }
          description
            "Indicates whether IPv4, IPv6, or
             both are to be activated.";
        }
        leaf area-id {
          type yang:dotted-quad;
          mandatory true;
          description
            "Area ID.";
          reference
            "RFC 4577: OSPF as the Provider/Customer
                       Edge Protocol for BGP/MPLS IP
                       Virtual Private Networks
                       (VPNs), Section 4.2.3
             RFC 6565: OSPFv3 as a Provider Edge to
                       Customer Edge (PE-CE) Routing
                       Protocol, Section 4.2";
        }
        leaf metric {
          type uint16;
          default "1";
          description
            "Metric of the PE-CE link.  It is used
             in the routing state calculation and
             path selection.";
        }
        leaf max-lsa {
          type uint32 {
            range "1..4294967294";
          }
          description
            "Maximum number of allowed Link State
             Advertisements (LSAs) that the OSPF
             instance will accept.";
        }
      }
      container isis {
        when "derived-from-or-self(../type, "
           + "'vpn-common:isis-routing')" {
          description
            "Only applies when the protocol is
             IS-IS.";
        }
        description
          "Configuration specific to IS-IS.";
        leaf address-family {
          type identityref {
            base vpn-common:address-family;
          }
          description
            "Indicates whether IPv4, IPv6, or both
             are to be activated.";
        }
        leaf area-address {
          type l3nm:area-address;
          mandatory true;
          description
            "Area address.";
        }
        leaf level {
          type identityref {
            base vpn-common:isis-level;
          }
          description
            "Can be 'level-1', 'level-2', or
             'level-1-2'.";
          reference
            "RFC 9181: A Common YANG Data Model for
                       Layer 2 and Layer 3 VPNs";
        }
        leaf metric {
          type uint16;
          default "1";
          description
            "Metric of the PE-CE link.  It is used
             in the routing state calculation and
             path selection.";
        }
        leaf mode {
          type enumeration {
            enum active {
              description
                "The interface sends or receives
                 IS-IS protocol control packets.";
            }
            enum passive {
              description
                "Suppresses the sending of IS-IS
                 updates through the specified
                 interface.";
            }
          }
          default "active";
          description
            "IS-IS interface mode type.";
        }
      }
      container rip {
        when "derived-from-or-self(../type, "
           + "'vpn-common:rip-routing')" {
          description
            "Only applies when the protocol is RIP.
             For IPv4, the model assumes that RIP
             version 2 is used.";
        }
        description
          "Configuration specific to RIP routing.";
        leaf address-family {
          type identityref {
            base vpn-common:address-family;
          }
          description
            "Indicates whether IPv4, IPv6, or both
             address families are to be activated.";
        }
        container timers {
          description
            "Indicates the RIP timers.";
          reference
            "RFC 2453: RIP Version 2";
          leaf update-interval {
            type uint16 {
              range "1..32767";
            }
            units "seconds";
            default "30";
            description
              "Indicates the RIP update time, i.e.,
               the amount of time for which RIP
               updates are sent.";
          }
          leaf invalid-interval {
            type uint16 {
              range "1..32767";
            }
            units "seconds";
            default "180";
            description
              "The interval before a route is
               declared invalid after no updates are
               received.  This value is at least
               three times the value for the
               'update-interval' argument.";
          }
          leaf holddown-interval {
            type uint16 {
              range "1..32767";
            }
            units "seconds";
            default "180";
            description
              "Specifies the interval before better
               routes are released.";
          }
          leaf flush-interval {
            type uint16 {
              range "1..32767";
            }
            units "seconds";
            default "240";
            description
              "Indicates the RIP flush timer, i.e.,
               the amount of time that must elapse
               before a route is removed from the
               routing table.";
          }
        }
        leaf default-metric {
          type uint8 {
            range "0..16";
          }
          default "1";
          description
            "Sets the default metric.";
        }
      }
      container vrrp {
        when "derived-from-or-self(../type, "
           + "'vpn-common:vrrp-routing')" {
          description
            "Only applies when the protocol is the
             Virtual Router Redundancy Protocol
             (VRRP).";
        }
        description
          "Configuration specific to VRRP.";
        reference
          "RFC 5798: Virtual Router Redundancy
                     Protocol (VRRP) Version 3 for
                     IPv4 and IPv6";
        leaf address-family {
          type identityref {
            base vpn-common:address-family;
          }
          description
            "Indicates whether IPv4, IPv6, or both
             address families are to be enabled.";
        }
        leaf ping-reply {
          type boolean;
          default "false";
          description
            "Controls whether the VRRP speaker
             should reply to ping requests.";
        }
      }
    }
  }

  grouping routing {
    description
      "Defines routing protocols.";
    list routing-protocol {
      key "id";
      description
        "List of routing protocols used on the
         CE-PE link.  This list can be augmented.";
      leaf id {
        type string;
        description
          "Unique identifier for the routing
           protocol.";
      }
      leaf type {
        type identityref {
          base vpn-common:routing-protocol-type;
        }
        description
          "Type of routing protocol.";
      }
      list routing-profiles {
        key "id";
        description
          "Routing profiles.";
        leaf id {
          type leafref {
            path "/nw:networks/nw:network"
               + "/ac-ntw:specific-provisioning-profiles/valid-provider-identifiers"
               + "/routing-profile-identifier/id";
          }
          description
            "Routing profile to be used.";
        }
        leaf type {
          type identityref {
            base vpn-common:ie-type;
          }
          description
            "Import, export, or both.";
        }
      }
      container static {
        when "derived-from-or-self(../type, "
           + "'vpn-common:static-routing')" {
          description
            "Only applies when the protocol is a
             static routing protocol.";
        }
        description
          "Configuration specific to static
           routing.";
        container cascaded-lan-prefixes {
          description
            "LAN prefixes from the customer.";
          list ipv4-lan-prefixes {
            if-feature "vpn-common:ipv4";
            key "lan next-hop";
            description
              "List of LAN prefixes for the site.";
            leaf lan {
              type inet:ipv4-prefix;
              description
                "LAN prefixes.";
            }
            leaf lan-tag {
              type string;
              description
                "Internal tag to be used in VPN
                 policies.";
            }
            leaf next-hop {
              type union {
                type inet:ip-address;
                type l3nm:predefined-next-hop;
              }
              description
                "The next hop that is to be used
                 for the static route.  This may be
                 specified as an IP address or a
                 predefined next-hop type (e.g.,
                 'discard' or 'local-link').";
            }
            leaf bfd-enable {
              if-feature "vpn-common:bfd";
              type boolean;
              description
                "Enables Bidirectional Forwarding
                 Detection (BFD).";
            }
            leaf metric {
              type uint32;
              description
                "Indicates the metric associated
                 with the static route.";
            }
            leaf preference {
              type uint32;
              description
                "Indicates the preference associated
                 with the static route.";
            }
            uses vpn-common:service-status;
          }
          list ipv6-lan-prefixes {
            if-feature "vpn-common:ipv6";
            key "lan next-hop";
            description
              "List of LAN prefixes for the site.";
            leaf lan {
              type inet:ipv6-prefix;
              description
                "LAN prefixes.";
            }
            leaf lan-tag {
              type string;
              description
                "Internal tag to be used in VPN
                 policies.";
            }
            leaf next-hop {
              type union {
                type inet:ip-address;
                type l3nm:predefined-next-hop;
              }
              description
                "The next hop that is to be used for
                 the static route.  This may be
                 specified as an IP address or a
                 predefined next-hop type (e.g.,
                 'discard' or 'local-link').";
            }
            leaf bfd-enable {
              if-feature "vpn-common:bfd";
              type boolean;
              description
                "Enables BFD.";
            }
            leaf metric {
              type uint32;
              description
                "Indicates the metric associated
                 with the static route.";
            }
            leaf preference {
              type uint32;
              description
                "Indicates the preference associated
                 with the static route.";
            }
            uses vpn-common:service-status;
          }
        }
      }
      container bgp {
        when "derived-from-or-self(../type, "
           + "'vpn-common:bgp-routing')" {
          description
            "Only applies when the protocol is
             BGP.";
        }
        description
          "Configuration specific to BGP.";
        container peer-groups {
          description
            "Configuration for BGP peer-groups";
          list peer-group {
            key "name";
            description
              "List of BGP peer-groups configured on the local system -
               uniquely identified by peer-group name";
            leaf name {
              type string;
              description
                "Name of the BGP peer-group";
            }
            leaf local-address {
              type union {
                type inet:ip-address;
                type if:interface-ref;
              }
              description
                "Sets the local IP address to use for
                 the BGP transport session.  This may be
                 expressed as either an IP address or a
                 reference to an interface.";
            }
            uses bgp-base-with-auth;
          }
        }
        list neighbor {
          key "remote-address";
          description
            "List of BGP neighbors.";
          leaf remote-address {
            type inet:ip-address;
            description
              "The remote IP address of this entry's BGP peer.";
          }
          leaf local-address {
            type union {
              type inet:ip-address;
              type if:interface-ref;
            }
            description
              "Sets the local IP address to use for
               the BGP transport session.  This may be
               expressed as either an IP address or a
               reference to an interface.";
          }
          leaf peer-group {
            type leafref {
              path "../../peer-groups/peer-group/name";
            }
            description
              "The peer-group with which this neighbor is
               associated.";
          }
          uses bgp-base-with-auth;
          uses vpn-common:service-status;
        }
      }
      container ospf {
        when "derived-from-or-self(../type, "
           + "'vpn-common:ospf-routing')" {
          description
            "Only applies when the protocol is
             OSPF.";
        }
        description
          "Configuration specific to OSPF.";
        leaf address-family {
          type identityref {
            base vpn-common:address-family;
          }
          description
            "Indicates whether IPv4, IPv6, or
             both are to be activated.";
        }
        leaf area-id {
          type yang:dotted-quad;
          mandatory true;
          description
            "Area ID.";
          reference
            "RFC 4577: OSPF as the Provider/Customer
                       Edge Protocol for BGP/MPLS IP
                       Virtual Private Networks
                       (VPNs), Section 4.2.3
             RFC 6565: OSPFv3 as a Provider Edge to
                       Customer Edge (PE-CE) Routing
                       Protocol, Section 4.2";
        }
        leaf metric {
          type uint16;
          default "1";
          description
            "Metric of the PE-CE link.  It is used
             in the routing state calculation and
             path selection.";
        }
        container sham-links {
          if-feature "vpn-common:rtg-ospf-sham-link";
          description
            "List of sham links.";
          reference
            "RFC 4577: OSPF as the Provider/Customer
                       Edge Protocol for BGP/MPLS IP
                       Virtual Private Networks
                       (VPNs), Section 4.2.7
             RFC 6565: OSPFv3 as a Provider Edge to
                       Customer Edge (PE-CE) Routing
                       Protocol, Section 5";
          list sham-link {
            key "target-site";
            description
              "Creates a sham link with another
               site.";
            leaf target-site {
              type string;
              description
                "Target site for the sham link
                 connection.  The site is referred
                 to by its identifier.";
            }
            leaf metric {
              type uint16;
              default "1";
              description
                "Metric of the sham link.  It is
                 used in the routing state
                 calculation and path selection.
                 The default value is set to '1'.";
              reference
                "RFC 4577: OSPF as the
                           Provider/Customer Edge
                           Protocol for BGP/MPLS IP
                           Virtual Private Networks
                           (VPNs), Section 4.2.7.3
                 RFC 6565: OSPFv3 as a Provider Edge
                           to Customer Edge (PE-CE)
                           Routing Protocol,
                           Section 5.2";
            }
          }
        }
        leaf max-lsa {
          type uint32 {
            range "1..4294967294";
          }
          description
            "Maximum number of allowed Link State
             Advertisements (LSAs) that the OSPF
             instance will accept.";
        }
        container authentication {
          description
            "Authentication configuration.";
          leaf enable {
            type boolean;
            default "false";
            description
              "Enables or disables authentication.";
          }
          container keying-material {
            when "../enable = 'true'";
            description
              "Container for describing how an OSPF
               session is to be secured between a CE
               and a PE.";
            choice option {
              description
                "Options for OSPF authentication.";
              case auth-key-chain {
                leaf key-chain {
                  type key-chain:key-chain-ref;
                  description
                    "Name of the key chain.";
                }
              }
              case auth-key-explicit {
                leaf key-id {
                  type uint32;
                  description
                    "Key identifier.";
                }
                leaf key {
                  type string;
                  description
                    "OSPF authentication key.
                     This model only supports the
                     subset of keys that are
                     representable as ASCII
                     strings.";
                }
                leaf crypto-algorithm {
                  type identityref {
                    base key-chain:crypto-algorithm;
                  }
                  description
                    "Indicates the cryptographic
                     algorithm associated with the
                     key.";
                }
              }
              case ipsec {
                leaf sa {
                  type string;
                  description
                    "Indicates the
                     administrator-assigned name
                     of the SA.";
                  reference
                    "RFC 4552: Authentication/
                               Confidentiality for
                               OSPFv3";
                }
              }
            }
          }
        }
        uses vpn-common:service-status;
      }
      container isis {
        when "derived-from-or-self(../type, "
           + "'vpn-common:isis-routing')" {
          description
            "Only applies when the protocol is
             IS-IS.";
        }
        description
          "Configuration specific to IS-IS.";
        leaf address-family {
          type identityref {
            base vpn-common:address-family;
          }
          description
            "Indicates whether IPv4, IPv6, or both
             are to be activated.";
        }
        leaf area-address {
          type l3nm:area-address;
          mandatory true;
          description
            "Area address.";
        }
        leaf level {
          type identityref {
            base vpn-common:isis-level;
          }
          description
            "Can be 'level-1', 'level-2', or
             'level-1-2'.";
          reference
            "RFC 9181: A Common YANG Data Model for
                       Layer 2 and Layer 3 VPNs";
        }
        leaf metric {
          type uint16;
          default "1";
          description
            "Metric of the PE-CE link.  It is used
             in the routing state calculation and
             path selection.";
        }
        leaf mode {
          type enumeration {
            enum active {
              description
                "The interface sends or receives
                 IS-IS protocol control packets.";
            }
            enum passive {
              description
                "Suppresses the sending of IS-IS
                 updates through the specified
                 interface.";
            }
          }
          default "active";
          description
            "IS-IS interface mode type.";
        }
        container authentication {
          description
            "Authentication configuration.";
          leaf enable {
            type boolean;
            default "false";
            description
              "Enables or disables authentication.";
          }
          container keying-material {
            when "../enable = 'true'";
            description
              "Container for describing how an IS-IS
               session is to be secured between a CE
               and a PE.";
            choice option {
              description
                "Options for IS-IS authentication.";
              case auth-key-chain {
                leaf key-chain {
                  type key-chain:key-chain-ref;
                  description
                    "Name of the key chain.";
                }
              }
              case auth-key-explicit {
                leaf key-id {
                  type uint32;
                  description
                    "Key identifier.";
                }
                leaf key {
                  type string;
                  description
                    "IS-IS authentication key.
                     This model only supports the
                     subset of keys that are
                     representable as ASCII
                     strings.";
                }
                leaf crypto-algorithm {
                  type identityref {
                    base key-chain:crypto-algorithm;
                  }
                  description
                    "Indicates the cryptographic
                     algorithm associated with the
                     key.";
                }
              }
            }
          }
        }
        uses vpn-common:service-status;
      }
      container rip {
        when "derived-from-or-self(../type, "
           + "'vpn-common:rip-routing')" {
          description
            "Only applies when the protocol is RIP.
             For IPv4, the model assumes that RIP
             version 2 is used.";
        }
        description
          "Configuration specific to RIP routing.";
        leaf address-family {
          type identityref {
            base vpn-common:address-family;
          }
          description
            "Indicates whether IPv4, IPv6, or both
             address families are to be activated.";
        }
        container timers {
          description
            "Indicates the RIP timers.";
          reference
            "RFC 2453: RIP Version 2";
          leaf update-interval {
            type uint16 {
              range "1..32767";
            }
            units "seconds";
            default "30";
            description
              "Indicates the RIP update time, i.e.,
               the amount of time for which RIP
               updates are sent.";
          }
          leaf invalid-interval {
            type uint16 {
              range "1..32767";
            }
            units "seconds";
            default "180";
            description
              "The interval before a route is
               declared invalid after no updates are
               received.  This value is at least
               three times the value for the
               'update-interval' argument.";
          }
          leaf holddown-interval {
            type uint16 {
              range "1..32767";
            }
            units "seconds";
            default "180";
            description
              "Specifies the interval before better
               routes are released.";
          }
          leaf flush-interval {
            type uint16 {
              range "1..32767";
            }
            units "seconds";
            default "240";
            description
              "Indicates the RIP flush timer, i.e.,
               the amount of time that must elapse
               before a route is removed from the
               routing table.";
          }
        }
        leaf default-metric {
          type uint8 {
            range "0..16";
          }
          default "1";
          description
            "Sets the default metric.";
        }
        container authentication {
          description
            "Authentication configuration.";
          leaf enable {
            type boolean;
            default "false";
            description
              "Enables or disables authentication.";
          }
          container keying-material {
            when "../enable = 'true'";
            description
              "Container for describing how a RIP
               session is to be secured between a CE
               and a PE.";
            choice option {
              description
                "Specifies the authentication
                 scheme.";
              case auth-key-chain {
                leaf key-chain {
                  type key-chain:key-chain-ref;
                  description
                    "Name of the key chain.";
                }
              }
              case auth-key-explicit {
                leaf key {
                  type string;
                  description
                    "RIP authentication key.
                     This model only supports the
                     subset of keys that are
                     representable as ASCII
                     strings.";
                }
                leaf crypto-algorithm {
                  type identityref {
                    base key-chain:crypto-algorithm;
                  }
                  description
                    "Indicates the cryptographic
                     algorithm associated with the
                     key.";
                }
              }
            }
          }
        }
        uses vpn-common:service-status;
      }
      container vrrp {
        when "derived-from-or-self(../type, "
           + "'vpn-common:vrrp-routing')" {
          description
            "Only applies when the protocol is the
             Virtual Router Redundancy Protocol
             (VRRP).";
        }
        description
          "Configuration specific to VRRP.";
        reference
          "RFC 5798: Virtual Router Redundancy
                     Protocol (VRRP) Version 3 for
                     IPv4 and IPv6";
        leaf address-family {
          type identityref {
            base vpn-common:address-family;
          }
          description
            "Indicates whether IPv4, IPv6, or both
             address families are to be enabled.";
        }
        leaf vrrp-group {
          type uint8 {
            range "1..255";
          }
          description
            "Includes the VRRP group identifier.";
        }
        leaf backup-peer {
          type inet:ip-address;
          description
            "Indicates the IP address of the peer.";
        }
        leaf-list virtual-ip-address {
          type inet:ip-address;
          description
            "Virtual IP addresses for a single VRRP
             group.";
          reference
            "RFC 5798: Virtual Router Redundancy
                       Protocol (VRRP) Version 3 for
                       IPv4 and IPv6,
                       Sections 1.2 and 1.3";
        }
        leaf priority {
          type uint8 {
            range "1..254";
          }
          default "100";
          description
            "Sets the local priority of the VRRP
             speaker.";
        }
        leaf ping-reply {
          type boolean;
          default "false";
          description
            "Controls whether the VRRP speaker
             should reply to ping requests.";
        }
        uses vpn-common:service-status;
      }
    }
  }

  // OAM

  grouping bfd {
    description
      "Grouping for BFD.";
    leaf session-type {
      type identityref {
        base vpn-common:bfd-session-type;
      }
      default "vpn-common:classic-bfd";
      description
        "Specifies the BFD session type.";
    }
    leaf desired-min-tx-interval {
      type uint32;
      units "microseconds";
      default "1000000";
      description
        "The minimum interval between
         transmissions of BFD Control packets, as
         desired by the operator.";
      reference
        "RFC 5880: Bidirectional Forwarding
                   Detection (BFD),
                   Section 6.8.7";
    }
    leaf required-min-rx-interval {
      type uint32;
      units "microseconds";
      default "1000000";
      description
        "The minimum interval between received BFD
         Control packets that the PE should
         support.";
      reference
        "RFC 5880: Bidirectional Forwarding
                   Detection (BFD),
                   Section 6.8.7";
    }
    leaf local-multiplier {
      type uint8 {
        range "1..255";
      }
      default "3";
      description
        "Specifies the detection multiplier that
         is transmitted to a BFD peer.

         The detection interval for the receiving
         BFD peer is calculated by multiplying the
         value of the negotiated transmission
         interval by the received detection
         multiplier value.";
      reference
        "RFC 5880: Bidirectional Forwarding
                   Detection (BFD),
                   Section 6.8.7";
    }
    leaf holdtime {
      type uint32;
      units "milliseconds";
      description
        "Expected BFD holdtime.

         The customer may impose some fixed
         values for the holdtime period if the
         provider allows the customer to use
         this function.

         If the provider doesn't allow the
         customer to use this function,
         fixed values will not be set.";
      reference
        "RFC 5880: Bidirectional Forwarding
                   Detection (BFD),
                   Section 6.8.18";
    }
  }

  // AC profile

  grouping ac-profile {
    description
      "Grouping for an attachment circuits.";
    // Layer 2
    container l2-connection {
      description
        "Defines Layer 2 protocols and parameters that
         are required to enable AC connectivity.";
      uses l2-connection;
    }
    // Layer 3
    container ip-connection {
      description
        "Defines IP connection parameters.";
    }
    // Routing
    container routing-protocols {
      description
        "Defines routing protocols.";
      uses routing-profile;
    }
    // OAM
    container oam {
      description
        "Defines the Operations, Administration,
         and Maintenance (OAM) mechanisms used.";
      container bfd {
        if-feature "vpn-common:bfd";
        description
          "Container for BFD.";
        uses bfd;
      }
    }
  }

  //AC network provisioning

  grouping ac {
    description
      "Grouping for an attachment circuits.";
    // Layer 2
    container l2-connection {
      description
        "Defines Layer 2 protocols and parameters that
         are required to enable AC connectivity.";
      uses l2-connection-if-ref;
    }
    // Layer 3
    container ip-connection {
      description
        "Defines IP connection parameters.";
      uses ip-connection;
    }
    // Routing
    container routing-protocols {
      description
        "Defines routing protocols.";
      uses routing;
    }
    // OAM
    container oam {
      description
        "Defines the Operations, Administration,
         and Maintenance (OAM) mechanisms used.";
      container bfd {
        description
          "Container for BFD.";
        leaf profile {
          type leafref {
            path "/nw:networks/nw:network"
               + "/ac-ntw:specific-provisioning-profiles"
               + "/valid-provider-identifiers"
               + "/bfd-profile-identifier/id";
          }
          description
            "Well-known service provider profile name.

             The provider can propose some profiles
             to the customer, depending on the
             service level the customer wants to
             achieve.";
        }
        uses bfd;
        container authentication {
          presence "Enables BFD authentication";
          description
            "Parameters for BFD authentication.";
          leaf key-chain {
            type key-chain:key-chain-ref;
            description
              "Name of the key chain.";
          }
          leaf meticulous {
            type boolean;
            description
              "Enables meticulous mode.";
            reference
              "RFC 5880: Bidirectional Forwarding
                         Detection (BFD),
                         Section 6.7";
          }
        }
        uses vpn-common:service-status;
      }
    }
    // Security
    container security {
      description
        "Site-specific security parameters.";
      container encryption {
        if-feature "vpn-common:encryption";
        description
          "Container for AC security encryption.";
        leaf enabled {
          type boolean;
          default "false";
          description
            "If set to 'true', traffic encryption on
             the connection is required.  Otherwise,
             it is disabled.";
        }
        leaf layer {
          when "../enabled = 'true'" {
            description
              "Included only when encryption
               is enabled.";
          }
          type enumeration {
            enum layer2 {
              description
                "Encryption occurs at Layer 2.";
            }
            enum layer3 {
              description
                "Encryption occurs at Layer 3.
                 For example, IPsec may be used when
                 a customer requests Layer 3
                 encryption.";
            }
          }
          description
            "Indicates the layer on which encryption
             is applied.";
        }
      }
      container encryption-profile {
        when "../encryption/enabled = 'true'" {
          description
            "Indicates the layer on which encryption
             is enabled.";
        }
        description
          "Container for the encryption profile.";
        choice profile {
          description
            "Choice for the encryption profile.";
          case provider-profile {
            leaf profile-name {
              type leafref {
                path "/nw:networks/nw:network"
                   + "/ac-ntw:specific-provisioning-profiles"
                   + "/valid-provider-identifiers"
                   + "/encryption-profile-identifier/id";
              }
              description
                "Name of the service provider's
                 profile to be applied.";
            }
          }
          case customer-profile {
            leaf customer-key-chain {
              type key-chain:key-chain-ref;
              description
                "Customer-supplied key chain.";
            }
          }
        }
      }
    }
  }

  augment "/nw:networks/nw:network" {
    description
      "Add a list of profiles.";
    container specific-provisioning-profiles {
      description
        "Contains a set of valid profiles to reference
         in the AC activation.";
      uses vpn-common:vpn-profile-cfg;
    }
    list ac-profile {
      key "id";
      description
        "Maintains a list of AC profiles.";
      leaf id {
        type string;
        description
          "An identifier of the AC.";
      }
      uses ac-ntw:ac-profile;
    }
  }

  augment "/nw:networks/nw:network/nw:node"
        + "/sap:service/sap:sap" {
    when '../../../nw:network-types/sap:sap-network' {
      description
        "Augmentation parameters apply only for SAP
         networks.";
    }
    description
      "Augments SAPs with AC provisioning details.";
    list ac {
      key "id";
      description
        "List of AC profiles.";
      leaf id {
        type string;
        description
          "A local AC identifier.";
      }
      list ac-profile {
        key "profile-id";
        description
          "List of AC profiles.";
        leaf profile-id {
          type leafref {
            path "/nw:networks/nw:network/ac-profile/id";
          }
          description
            "A reference to an AC profile.";
        }
      }
      uses ac-ntw:ac;
    }
  }
}
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

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
