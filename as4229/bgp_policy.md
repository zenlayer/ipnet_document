# AS4229 Routing Policies and BGP Communities

## Disclaimer

The content of document may change without notice, please make sure to refer to the latest version of the document.
We will do our best to provide BGP community support, but we do not provide an SLA commitment for this feature.

## Document Version

v20250321_1

## Routing Policy Description

AS4229 implements a single AS single network design, employing backbone connections between all PoPs. All customer routes are synchronized across the entire network, while peer routes are synchronized to the maximum extent possible, aligning with the capabilities of our backbone infrastructure.

## Route Damping

AS4229 does not use route damping.

## Graceful Shutdown

AS4229 does not support graceful shutdown.

## MED

AS4229 accepts MED from its customers.

## EBGP route filter policy

AS4229 rejects to receive routes displaying an invalid RPKI validation statefrom all peers or customers.  
AS4229 rejects to receive or announce routes within reserved address ranges on all EBGP neighbors.  
AS4229 rejects to receive or announce IPv4 routes with a /24 or longer mask on all EBGP neighbors.  
AS4229 rejects to receive or announce IPv6 routes with a /48 or longer mask on all EBGP neighbors.  
AS4229 deploys a maximum received route limit on all peers or customers based on PeeringDB data or the actual number of received routes.

## BGP community for customers

### Allowed BGP communities sent to customers

#### Route origin site

4229:1101 - US, Los Angeles
4229:1102 - US, Ashburn
4229:1103 - US, Miami
4229:1104 - US, San Jose
4229:1201 - SG, Singapore
4229:1202 - HK, Hong Kong
4229:1203 - JP, Tokyo
4229:1301 - DE, Frankfurt
4229:1302 - FR, Marseille

#### Route origin region

4229:3001 - North America  
4229:3002 - Asia  
4229:3003 - Europe

#### Route origin type

65502:1 - On net route or from customer AS  
65502:3 - From peer AS  
65502:6 - From transit AS

### Allowed BGP communities received from customers

#### Blackhole

4229:666 - Black hole traffic

Note: Only /32 or /128 prefixes with this community will be accepted.

#### Perpend or do not announce based on neighbor type

The format of a community follows 652AB:C where:

C - Neighbor Type

1 - Customer  
3 - Peer  
6 - Transit

B - Scope

0 - Entire network  
1 - North America  
2 - Asia  
3 - Europe

A - Action

1 - Prepend 4229 x 1 when announcing to neighbors of type C within scope B  
2 - Prepend 4229 x 2 when announcing to neighbors of type C within scope B  
3 - Prepend 4229 x 3 when announcing to neighbors of type C within scope B  
9 - Suppress the announcing to neighbors of type C within scope B

For example, "65211:6" implies that when announcing to neighbors of type transit within the North America scope prepend 4229 x 1.

#### Perpend or do not announce based on neighbor ASN

The format of a standard community follows 653AB:C for 2Byte ASN where:

C - Neighbor ASN

2Byte public ASN public ASN

B - Scope

0 - Entire network  
1 - North America  
2 - Asia  
3 - Europe

A - Action

1 - Prepend 4229 x 1 when announcing to neighbors of ASN C within scope B  
2 - Prepend 4229 x 2 when announcing to neighbors of ASN C within scope B  
3 - Prepend 4229 x 3 when announcing to neighbors of ASN C within scope B  
9 - Suppress the announcing to neighbors of ASN C within scope B

For example, the standard community "65311:21859" implies that when announcing to neighbors of AS21859 within the North America scope prepend 4229 x 1.

If the neighbor is an Internet Exchange (IX), using the route server's ASN can control all neighbors on that IX, however, currently, it is not able to control a specific peer within an IX.


#### Local preference adjustment

The format of a standard community follows 649AB:0 where:

B - Scope

0 - Entire network  
1 - North America  
2 - Asia  
3 - Europe

A - Action

2 - Set the local preference to 950 (lower than customer defaults) within scope B  
9 - Set the local preference to 450 (lower than all routes) within scope B

Limitation:

If A = 2, B must not equals to 0.  
If A = 9, B must equals to 0.

Note:

Local preference in AS4229  
Customer route: 1000  
Peer route: 800  
Transit route: 500
