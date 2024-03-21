# AS21859 Routing Policies and BGP Communities

## Disclaimer

The content of document may change without notice, please make sure to refer to the latest version of the document.We will do our best to provide BGP community support, but we do not provide an SLA commitment for this feature.

## Document Version

v20250321_1

## Routing Policy Description

AS21859 implements a single AS multiple network design, which means each AS21859 PoP is independent, uses a completely different address space and rely on transit AS to achieve network intercommunication between PoPs.

## Route Damping

AS21859 does not use route damping.

## Graceful Shutdown

AS21859 does not support graceful shutdown.

## MED

AS21859 does not accept MED from its customers.


## EBGP route filter policy

AS21859 rejects to receive routes displaying an invalid RPKI validation state from all peers or customers.  
AS21859 rejects to receive or announce routes within reserved address ranges on all EBGP neighbors.  
AS21859 rejects to receive or announce IPv4 routes with a /24 or longer mask on all EBGP neighbors.  
AS21859 rejects to receive or announce IPv6 routes with a /48 or longer mask on all EBGP neighbors.  
AS21859 deploys a maximum received route limit on all peers or customers based on PeeringDB data or the actual number of received routes.


## BGP community for customers supported at all AS21859 PoPs

### Allowed BGP communities received from customers

#### Blackhole

21859:666 - Black hole traffic  
Note: Only /32 or /128 prefixes with this community will be accepted.

## BGP community for customers supported only at specific AS21859 PoPs

The TE functionality mentioned in this section is only supported on the following PoPs:

[List of PoPs Supporting TE Functionality](./list_of_pops_supporting_te_functionality.md)

### Allowed BGP communities sent to customers

#### Route origin type

65502:1 - On net route or from customer AS  
65502:3 - From peer AS  
65502:6 - From transit AS

### Allowed BGP communities received from customers

#### Perpend or do not announce based on neighbor type

The format of a community follows 652A0:C where:  

C - Neighbor Type

3 - Peer  
6 - Transit  

A - Action

1 - Prepend 21859 x 1 when announcing to neighbors of type C  
2 - Prepend 21859 x 2 when announcing to neighbors of type C  
3 - Prepend 21859 x 3 when announcing to neighbors of type C  
9 - Suppress the announcing to neighbors of type C  

For example, "65210:6" implies that when announcing to neighbors
of type transit within the current AS21859 network prepend 21859 x 1.

#### Perpend or do not announce based on neighbor ASN

The TE functionality for neighbor ASN is only supported on the following neighbors:

[List of Neighbors Supporting TE Functionality](./list_of_neighbors_supporting_te_functionality.md)



The format of a standard community follows 653A0:C for 2Byte ASN where:

C - Neighbor ASN

2Byte public ASN public ASN

A - Action

1 - Prepend 21859 x 1 when announcing to neighbors of ASN C  
2 - Prepend 21859 x 2 when announcing to neighbors of ASN C  
3 - Prepend 21859 x 3 when announcing to neighbors of ASN C  
9 - Suppress the announcing to neighbors of ASN C

For example, the standard community "65310:4229" implies that when announcing
to neighbors of AS4229 within the current AS21859 network prepend 21859 x 1.

If the neighbor is an Internet Exchange (IX), using the route server's ASN can control all neighbors on that IX, however, currently, it is not able to control a specific peer within an IX.
