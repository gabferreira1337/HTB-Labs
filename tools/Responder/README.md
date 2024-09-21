#
***
#### Poisoning on LLMNR, NBT-NS, and MDNS protocols. It's commonly used to capture NTLMv2 hashes by tricking network devices into sending authentication requests to our machine (sniffing).
* Link-Local Multicast Name Resolution (**LLMNR**) , protocol based on DNS packet format 
* Net BIOS Name Service (**NBT-NS**) , protocol used to translate NetBIOS names to IP addresses. We can get name service under NetBT by a broadcast mechanism in which each machine keeps a database , and a unicast mechanism in which there is a designated server. Both LLMNR and NBT-NS provide identification of hostname
* Multicast Domain Name Service (**MDNS**),is a protocol communication designed in a small network , when all the users in the network are addressed, and it provides a naming service system.
#### Pentest on the infrastructure of the corporate network

#### Example
```bash
$ responder -I eth0 -wf
```


### Useful flags
* -I = Specify network interface to use
* -i  = local ip
* -e  = external ip
* -w, --wpad = WPAD rogue proxy server
* -f = fingerprint a host that issued an NBT-NS or LLMNR query.