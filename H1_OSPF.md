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
||`show ip ospf interface `|
||`show ip protocols`|
||`show ip ospf`|

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


## Designated Router en Back up Designated Router
De DR word bepaald aan de hand van een electie. (BDR wordt bepaald aan de hand van de router dat op de 2e plaats komt)
- de router met het hoogste Router ID `Router-rid [rid] `
- De router met heet hoogste loopback-ip address.
- De router met het hoogste actieve IP address  

 Het is mogelijk om ook de electie te beïnvloeden door de prioriteitsvalue op een interface zelf aan te passen met dit commando `ip ospf priority [0-255]`, default is 0.

 ### Hello en dead interval
 
Elk pakket dat de DR of BDR moet bereiken worden verzonden naar het **multiacces address (224.0.0.6)**. De BDR luistert passief naar deze packetten en gaat enkel de rol de de DR overnemen wanneer deze geen hello packets meet stuurt. Deze worden verzonden om de 10 seconden **(Hello interval)**.  
Een router kan 4 intervallen (40s **Dead interval**) wachten voordat die een neighbor als down verklaart. Daarna wordt deze router uit de LSDB (Link-state Database) verwijderd en deze informatie wordt dan gefloodt/verspreidt naar alle routers. Deze 'Timer intervals' kan je zien bij `show ip ospf interface [interface-id]` en de intervals moeten gelijk zijn aan beide van kanten van een link anders gaat deze link falen.
Je kan deze intervallen aanpassen per interface met de commando's `ip ospf hello-interval [1-...seconds]` en `ip ospf dead-interval [1-... seconds]` (het dead interval wordt automatisch naar 4x het hello-interval.



## Bandwith metric cost
Aan de hand van het bandwith commando te gebruiken ga je niet de bandwith aanpassen van de link maar de metric cost aanpassen. Dit doe je omdat OSPF het beste path wilt berkenen en je wilt zelf bepalen welk path de beste metric heeft. Dit doe je aan de hand van 1 van volgende commando's `auto-cost reference-bandwith [0-... in Mbps]` of `ip ospf cost [metric cost]`deze is voor de cost op enkel 1 interface aan te passen. Dit kan je checken via `show ip ospf interface` of `show ip route`

## Default route configureren

dit commando gaat je default route aanpassen`ip route 0.0.0.0 0.0.0.0.0 [next-hop-address]` 
Dit commando gaat aan iedereen zeggen dat jij de default router bent, voor bv naar het internet te gaan `default-information originate`

