---
share: true
catagory: Networks/Lap01
tags:
  - network
---
# Problem

Consider we having this network [[NETWORK LAP 1 - VPN route.canvas|NETWORK LAP 1 - VPN route]], route the Admin to access the Private LAN (the Router can't not access and change route table)

The 192.168.1.254 gateway have 2 network card to make route traffic between 192.168.1.0/24 and 192.168.2.0/24

Jump server also have two network card too. But all other Private Lan 2 client only have 192.168.1.254 as default gateway


![[Pasted image 20230818184045.png|Pasted image 20230818184045.png]]

# Solve

To get to the [[LAP 1 - Answer.canvas|Answer]]
![[Pasted image 20230818191304.png|Pasted image 20230818191304.png]] 

We mush make sure we have the packet can be route forward and backward. In this problem, we have 2 new hop gateway (with 3 manual route) that need manual config.

## Pre require

The network card in Jump server make the routing too complex thus being remove and use 192.168.1.254 as gateway to access the Private Lan 2 client instead. 

```bash
sudo nwtui
```

Then process to deactivate and disable Lan 2 network interface on the Jump server 
## Hop 1: 

### Route Admin PC to Jump server 

This is needed so that Admin PC can understand and find **192.168.2.0/24** network

This require we add to our gateway routing table (which is on VPN server)

![[Pasted image 20230818185311.png|Pasted image 20230818185311.png]]

```r
route 192.168.2.0/24 via 10.243.143.44
```

or

```bash
sudo route add -net 192.168.2.0/24 gw 10.243.143.44
```

or (for windows reference)

```
route add 192.168.1.0 mask 255.255.255.0 10.243.143.44 IF 10.243.143.47
```

Result route table on Admin PC after update should look like this

```r
→ route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         net.xxx         0.0.0.0         UG    600    0        0 tun0
10.243.0.0      0.0.0.0         255.255.0.0     U     0      0        0 <VPN_inf>
link-local      0.0.0.0         255.255.0.0     U     1000   0        0 br-1da3c1d75167
192.168.2.0     10.243.143.44   255.255.255.0   UG    0      0        0 <VPN_inf>
```

### Adding forwarding request on Jump server 

To route the packet back, Jump server have to have IPv4 forwarding capacity to allow traffic to flow through as local VPN client (.44) is also acting as a router for other devices (.43)

```bash
sudo sysctl -w net.ipv4.ip_forward=1
```

#### Or by config file

```bash
sudo nano /etc/sysctl.conf
```

Add or uncomment the following line:

```toml
net.ipv4.ip_forward=1
```

And then apply

```bash
sudo sysctl -p
```

## Hop 2: Route packet between Jump server and Gateway

### Gateway route traffic over VPN network

On the gateway, we need to enable IP forwarding so that it can forward traffic from the 192.168.2.0/24 subnet to the VPN client:

**Via GUI**

![[Pasted image 20230818192221.png|Pasted image 20230818192221.png]]

**Check the Available Network Interfaces:**

To find out the name of the network interface you want to route traffic through, run the following command:

```
netsh interface show interface
```

Identify the name of the interface you want to use (e.g., `Lan1_network_interface`). And add route

```
route add 10.243.0.0 mask 255.255.255.0 192.168.1.98 IF Lan1_network_interface
```

> You may want to use metric so our manual route have higher priority

### Jump route traffic over Private Lan2 Network

On the Jump server, we need to enable IP forwarding so that it can forward traffic from the VPN client to the 192.168.2.0/24 subnet:

```sh
sudo route add -net 192.168.2.0/24 gw 192.168.1.254
```

## Hop 3: Gateway to Private LAN2

Just use default network interface, Lan2 client only know to send the packet back to our gateway (via `192.168.2.254`)

Our gateway `192.168.1.254` to `192.168.2.254` have 2 network interface, so there is no need for further config.