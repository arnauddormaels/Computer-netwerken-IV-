# ACL (Acces Control List)

### 2 types ACLs
- Standard ACL: Filter op laag 3 (Neterk laag), gaan enkel filter op Source Ipv4 address
- Extended ACL: Filter op laag 3 en 4(Transport laag), gaat filteren op Source en/of destination address op laag 3 en filteren op TCP en UDP ports op laag 4
 
## WilcardMask 
The wildcard gaat zeggen welke bit je mag negeren. Als de wildcard mask 0.0.0.0 is dan moet alles gelijk zijn aan het meegegeven IP address zodat de regel zou werken.    
  
`acces list 10 permit 192.168.0.1 0.0.0.0` -> 192.168.0.1 is het enige dat toegestaan wordt  
  
`acces list 10 deny 192.168.0.1 0.0.0.255` -> alles van 192.168.0.0 tot en met 192.168.168.255 wordt afgeblockt  





