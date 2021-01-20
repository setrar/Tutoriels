# D.DHCP

https://www.howtoforge.com/dhcp_server_linux_debian_sarge

## :o: Preliminary Note

This is the current situation:

```
I'm using the network `10.13.237.0`, subnetmask `255.255.255.0`, broadcast address `10.13.237.255`.
My gateway to the internet is `10.13.237.1`; on the gateway there's no DHCP server..
My ISP told me the DNS servers I can use are `149.112.121.20` and `149.112.122.20`.
I have a pool of 64 IP addresses (`10.13.237.128` - `10.13.237.192`) that can be dynamically assigned to client PCs and that are not already in use.
I have an unused Ubuntu 20.04 server with the hostname server1.example.com on the IP address `10.13.237.9` which will act as my DHCP server.
```

```
$ lsb_release --all
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 20.04.1 LTS
Release:	20.04
Codename:	focal
```


:bulb: CIRA DNS
```
En passant, j’utilise maintenant les deux adresses CIRA (gratuite) comme DNS externe en labo.
Ils offrent une protection supplémentaire de « malware & phishing » : 149.112.121.20, 149.112.122.20
 
https://www.cira.ca/cybersecurity-services/canadian-shield/configure
```