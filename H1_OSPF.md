# OSPF 
- Link state routing prototcol

### Informatie wordt uitgewisseld aan dehand van 5 packets
deze packetten worden uitgebruikt voor het vinden van neighbor routers en voor routing informatie uit te wisselen voor up-to-date te zijn met het netwerk.
- *Hello packet*  
- *Database description packet*  
- *link-state request packet* 
- *link-state update packet*
- *link-state acknowledgement packet"
 
|beschrijving| commando|
|---|---|
|toont lijst van alle neighbor routers (neighbor table), is uniek voor elke router|`show ip ospf neighbor`|
|toont de lijst van alle routers in het netwerk, is hetzelfde voor alle routers in dezelfde area en LSDB(link-state database)| `show ip osp database`|
|toont de routing table|`show ip route`|

## Basis van OSPF
ospf is gebasseerd op het algoritme van dijkstra Shortest-Path-First(SPF) voor het bepalen van het korste pad.  
OSPF gaat een OSPF tree maken met elke router, en gaat voor elke router het kortste pad berekenen voor elke node. Het kortste pad wordt want elke keer weggeschreven naar de routing table.

### Single-area OSPF
alle routers bevinden zich in dezelfde area. Best practice is om area 0 te gebruiken.

### multiarea OSPF
Er wordt gebruikt gemaakt van meerdere area's. Al deze area's moeten wel verbonden zijn met de *backbone area 0 (area 0)*. De routers die de connecties maken tussen de verschillende areas worden ook wel *Area Border Routers (ABRs)* genoemd.

**Voordelen:**
- Kleinere routing tables: de tables zijn kleiner omdat er minder routing entries zijn. Dit komt omdat de netwerk addressen samengenomen worden tussen de area's. (route summarization is niet enables by default)
- Kleinere link-state update overhead: Het maken van kleinere OSPF areas (ipv 1 grote single area OSPF) zorgt ervoor dat er minder processing en memory requirements nodig zijn.
- Verminderd de frequentie aan SPF berekeningen: Als er een verandering is in de status van een router dan worden de router status doorgegeven aan alle routers. Dit zorgt voor heel veel LSA (Link-state advertisement) verkeer
maar deze zal dan enkel maar binnen de huideige area blijven. want dit verkeer stopt aan de area boundaries.  
