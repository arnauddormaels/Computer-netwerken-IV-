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
|toont lijst van neighbor routers (neighbor table), uniek voor elke router|`show ip ospf neighbor`|
|toont de lijst van alle routers in het netwerk, is hetzelfde voor alle routers in dezelfde area en LSDB(link-state database)| `show ip osp database`|
|toont de routing table|`show ip route`|
