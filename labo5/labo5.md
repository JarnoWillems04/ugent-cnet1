# Computernetwerken I - cnet1
# Lab IP routing
> (base 02509654) - complete your personal information below

date:

name:
year: 

## Design

### IP addresses
> These are the assigned IP address for the different hosts in your network.

| Host                         | IP address                                 |
| ---------------------------- | ------------------------------------------ |
| workstation1                 | 172.20.33.76                      |
| workstation2                 | 172.20.33.87                      |
| webserver                    | 172.20.33.42                           |
| ftpserver                    | 172.20.33.44                           |
| Host in 12-host network      | 172.20.33.163                         |
| Uplink address on ISP router | 172.20.33.254/<Smallest SN possible> |


### Subnetworks calculated
> Fill in the resulting network information in the table below; use the format as in the theoretical exercises (Linux differs on some points in it’s display).

| Network (logical)  | Network address | Netmask | Broadcast | Router Interface (lowest usable) | Usable Host Range | Interface name |
| ------------------- | --------------- | -------- | ---------- | -------------------------------- | ----------------- | --------------- |
| Workstation subnet  | 172.20.33.64/27 | 255.255.255.224 | 172.20.33.95  | 172.20.33.65 | 172.20.33.66 - 172.20.33.94 | ws |
| Server park         | 172.20.33.40/29 | 255.255.255.248 | 172.20.33.47  | 172.20.33.41 | 172.20.33.42 - 172.20.33.46 | serv |
| Visitor subnet      | 172.20.33.160/28 | 255.255.255.240 | 172.20.33.175 | 172.20.33.161 | 172.20.33.162 - 172.20.33.174 | test |
| ISP uplink (p2p)    | 172.20.33.252/30 | 255.255.255.252 | 172.20.33.255 | 172.20.33.253 | 172.20.33.253 - 172.20.33.254 | up |

### Setting up ip addresses

#### Workstations
In workstation1:
```
root@ws1:/# ip address add 172.20.33.76/27 dev eth0
root@ws1:/# ip route add default via 172.20.33.65
root@ws1:/# ip route
default via 172.20.33.65 dev eth0 
172.20.33.64/27 dev eth0 proto kernel scope link src 172.20.33.76 
```

In workstation2:
```
root@ws2:/# ip address add 172.20.33.87/27 dev eth0
root@ws2:/# ip route add default via 172.20.33.65
```

In router:
```
root@rout:/# ip address add 172.20.33.65/27 dev eth0
```

Erna testen:
```
root@ws1:/# ping 172.20.33.65
PING 172.20.33.65 (172.20.33.65) 56(84) bytes of data.
64 bytes from 172.20.33.65: icmp_seq=1 ttl=64 time=0.553 ms
64 bytes from 172.20.33.65: icmp_seq=2 ttl=64 time=0.980 ms
64 bytes from 172.20.33.65: icmp_seq=3 ttl=64 time=1.12 ms
```

#### Server park

In router
```
root@rout:/# ip a a 172.20.33.41/29 dev eth1
```

In web (ook hier eth0 zie ip link show command)
```
root@web:/# ip link show
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
13: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 82:65:f4:4f:02:02 brd ff:ff:ff:ff:ff:ff
root@web:/# ip a a 172.20.33.42/29 dev eth0
root@web:/# ip route add default via 172.20.33.41
```

In FTP
```
root@ftp:/# ip a a 172.20.33.44/29 dev eth0
root@ftp:/# ip r a default via 172.20.33.41
```

## Configure the network

### ... the router
> screenshot of the router pinging external node


### ... the hosts
> What do you need to add on the host to make this work? 

> screenshot of the workstation pinging the webserver 

> Your built-up routing table of the company router (ip route output):


### ... the ISP router
> Which networks are we missing?

> screenshot of the workstation pinging external node

> Explain what general rule make the entry match.
