# OSPF 
- Link state routing prototcol

### Informatie wordt uitgewisseld aan dehand van 5 packets
deze packetten worden uitgebruikt voor het vinden van neighbor routers en voor routing informatie uit te wisselen voor up-to-date te zijn met het netwerk.

- *Hello packet*  
- *Database description packet (DBU)*  
- *link-state request packet (LSR)* 
- *link-state update packet (LSU)*
- *link-state acknowledgement packet (LSAck)"
 
 
Extra** LSU wordt ook gebruikt voor het verzenden van OSPF routing updates. LSU packets bevatten LSA(Linke state advertisements) messages.

### Hello packet

wordt gebruikt voor het zoeken van neighbors en gaan een neighbor connectie maken. De parameters worden geadvertised en beide neighbor routers moeten het hierover eens worden. Er wordt een **Designated router (DR)** en een **Back up designated router (BDR)** verkozen. Dit is niet nodig bij point to point links.

 # OSPF operation states
 
 **1. Down state** 
- er worden geen Hello packets ontvangent.
- Router verstuurt wel Hello packets.
-  Router kan overschakelen naar de Init state.  

**2. Init state (initiatie fase)**  
 - gaat naar de state als er een Hello packet ontvangen is van de neighbor
 - Het packet bevat het router ID van de neightbor
 - Router schakeld over naar de TWO-way state  

**3. Two-Way state**  
 - Tijdens deze fase is de communicatie van beide routers biderectioneel.
 - De routers gaan stemmen voor een DR(Designated router) en BDR( back up DR), dit gebeurt enkel bij multiacces links. Niet bij point to point links.
 - Router schakeld over naar de ExStart state.
 
 
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
Er wordt gebruikt gemaakt van meerdere area's. Al deze area's moeten wel verbonden zijn met de *backbone area (area 0)*. De routers die de connecties maken tussen de verschillende areas worden ook wel *Area Border Routers (ABRs)* genoemd.

**Voordelen:**
- Kleinere routing tables: de tables zijn kleiner omdat er minder routing entries zijn. Dit komt omdat de netwerk addressen samengenomen worden tussen de area's. (route summarization is niet enables by default)
- Kleinere link-state update overhead: Het maken van kleinere OSPF areas (ipv 1 grote single area OSPF) zorgt ervoor dat er minder processing en memory requirements nodig zijn.
- Verminderd de frequentie aan SPF berekeningen: Als er een verandering is in de status van een router dan worden de router status doorgegeven aan alle routers. Dit zorgt voor heel veel LSA (Link-state advertisement) verkeer
maar deze zal dan enkel maar binnen de huideige area blijven. want dit verkeer stopt aan de area boundaries.  







