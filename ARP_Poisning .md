learn about what arp posing is???


# what is ARP poising


## what is ARP ???
#whatis it is address resolution protocol
 - discover mac addresses 
 - map them to asscoisated IP address
### how  does it work
- the connecting device wants to know the default gate way
- host will ask the on a broadcast which is 127.0.0.1 and the mac address of the hosts
- the other host will think it is not for them 
- but the router will reply because it has the gate way and will give up the mac address
-  the asker will store the mac address on the arp cache
#### the attack 
- attacker will impersonate being the router 
- send the asker as an router on a specifically crafted message  
- the attacker will send his ip as the router ip
```
this is called man in the middle attack!!
```
### tools used 
we use ettercap = have gui 
	this tools will make the main attack
	we have to have 
		the target as target one 
		and the route as target two
we use wireshark to capture the traffic
