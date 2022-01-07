# ACL (Acces Control List)

### 2 types ACLs
- Standard ACL: Filter op laag 3 (Neterk laag), gaan enkel filter op Source Ipv4 address. (Moet zo dicht mogelijk bij de Destination zich bevinden)
- Extended ACL: Filter op laag 3 en 4(Transport laag), gaat filteren op Source en/of destination address op laag 3 en filteren op TCP en UDP ports op laag 4. (Moet zich zo dicht mogelijk bij de source bevinden)
 

## WilcardMask 
The wildcard gaat zeggen welke bit je mag negeren. Als de wildcard mask 0.0.0.0 is dan moet alles gelijk zijn aan het meegegeven IP address zodat de regel zou werken.    
  
`acces list 10 permit 192.168.0.1 0.0.0.0` -> 192.168.0.1 is het enige dat toegestaan wordt  
  
`acces list 10 deny 192.168.0.1 0.0.0.255` -> alles van 192.168.0.0 tot en met 192.168.168.255 wordt afgeblockt  

**Host en any**  
Het host keyword is hetzelfde als 0.0.0.0, deze zegt gewoon dat het ip address gelijk moet zijn aan het aangegeven ip address
`acces list 10 permit 192.168.0.1 0.0.0.0` is hetzelfde als `acces list 10 permit host 192.168.0.1`  

any voegt de regelt toe aan alle ip addressen dat nog geen Regel hebben gekregen
`acces list 10 permit 0.0.0.0 255.255.255.255` is hetzelfde als `acces list 10 permit any`  
  
 EXTRA** Elke ACL bevatten een impliciete deny any op het einde  

## Numbered and Named ACLs

**Numbered ACL**  
`acces-list [1-99 standard ACL / 1300-1999 Extended ACL`  

**Named ACL **   
`ip acces-list extended FTP-FILTER`  
`Permit tcp 192.168.10.0 0.0.0.255 any eq ftp`  
`permit tcp 192.168.10.0 0.0.0.255 any eq ftp-data`
 EXTRA ** Extended named filters kunnen ook filteren op IP, TCP, UDP, ICMP  en port numbers  

 # Show commands  
 |Beschrijving|Commando|
 |---|---|
 |toont de acces-lists met de permits en deny's|`R1(config)# do show acces-list`|
 |Toont de acces-list configuratie| `R1# show run \| section acces-list`|
 |Toont de ACL op de interface| `R# show ip int s0/0/0 \| include acces list`|
 |Toont de acces-lists| `R1# show acces-lists`
 
 # Excercise 
 
 |Beschrijving | Commando|
 |---|---| 
 | wijzig een ACE/regel van de ACL| `R1(config)# ip acces-list standard 1` <br> `R1(config-std-nacl)# no 10`: 10 is het standard ip van de regel dit kan je vinden door (`R1# show acces-lists`) <br> `R1(config-std-nacl)# 10 deny host 192.168.10.10`|
