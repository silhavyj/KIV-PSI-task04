# Setting up a simple home network

<img src="img/01.png">

- [Router R1](#router-r1)
  * [Assigning an IP address to interface gigabitEthernet 1/0](#assigning-an-ip-address-to-interface-gigabitethernet-1-0)
  * [Setting up a DHCP pool](#setting-up-a-dhcp-pool)
  * [Configure the second interface of the router](#configure-the-second-interface-of-the-router)
  * [Setting up RIP](#setting-up-rip)
- [Router R2](#router-r2)
  * [Assigning an IP address to interface gigabitEthernet 1/0](#assigning-an-ip-address-to-interface-gigabitethernet-1-0-1)
  * [Setting up RIP](#setting-up-rip-1)
  * [Getting an IP address from the ISP](#getting-an-ip-address-from-the-isp)
  * [Setting up NAT](#setting-up-nat)
## Router R1

### Assigning an IP address to interface gigabitEthernet 1/0

Getting into the configuration mode.

```
config term
```

Selecting the interface to be configured.

```
interface gigabitEthernet 1/0
```

Assigning an IP of `10.10.2.1` to the interface. The mask was chosen to be `/27`. It allows as many as 30 hosts to be connected to this network.

```
ip address 10.10.2.1 255.255.255.224
```

Turning on the interface.

```
no shutdown
```

Returning from the configuration mode.
```
end
```

Saving the configuration of the router.
```
copy running-config startup-config
```

---

### Setting up a DHCP pool

Getting into the configuration mode.

```
config term
```

Creating a new DHCP pool named home.

```
ip dhcp pool home
```

Selecting a network for the DHCP pool
```
network 10.10.2.0 255.255.255.224
```

Setting up the default router (gateway).
```
default-router 10.10.2.1
```

Adding default DNS server for resolving addresses.
```
dns-server 8.8.8.8 4.4.4.4
```

Returning from the configuration mode.
```
end
```

Saving the configuration of the router.

```
copy running-config startup-config
```

---

### Configure the second interface of the router

Getting into the configuration mode.

```
config term
```

Selecting the interface to be configured.
```
interface gigabitEthernet 0/0
```

Assigning an IP of `192.168.1.1` to the interface. The mask is set to `/30` as there are only two nodes needed.

```
ip address 192.168.1.1 255.255.255.252
```


Turning on the interface.

```
no shutdown
```

Returning from the configuration mode.
```
end
```

Saving the configuration of the router.
```
copy running-config startup-config
```

---

### Setting up RIP

Getting into the configuration mode.

```
config term
```

Getting into RIP configuration.
```
router rip
```

Setting up version 2.
```
version 2
```

Adding network `10.10.2.0` into RIP.
```
network 10.10.2.0
```


Adding network `192.168.1.0` into RIP.
```
network 192.168.1.0
```

Returning from the configuration mode.
```
end
```

Saving the configuration of the router.
```
copy running-config startup-config
```

## Router R2

### Assigning an IP address to interface gigabitEthernet 1/0

Getting into the configuration mode.

```
config term
```

Selecting the interface to be configured.

```
interface gigabitEthernet 1/0
```

Assigning an IP of `192.168.1.2` to the interface. The mask is set to `/30` as there are only two nodes needed.

```
ip address 192.168.1.2 255.255.255.252
```


Turning on the interface.

```
no shutdown
```

Returning from the configuration mode.
```
end
```

Saving the configuration of the router.
```
copy running-config startup-config
```

---

### Setting up RIP

Getting into the configuration mode.

```
config term
```

Getting into RIP configuration.
```
router rip
```

Setting up version 2.
```
version 2
```

Adding network `192.168.1.0` into RIP.
```
network 192.168.1.0
```

Returning from the configuration mode.
```
end
```

Saving the configuration of the router.
```
copy running-config startup-config
```

---

### Getting an IP address from the ISP

Getting into the configuration mode.

```
config term
```

Selecting the interface to be configured.

```
interface gigabitEthernet 0/0
```

Obtaining an IP address on the interface through DHCP
```
ip address dhcp
```

Turning on the interface.

```
no shutdown
```

Returning from the configuration mode.
```
end
```

Saving the configuration of the router.
```
copy running-config startup-config
```

---

### Setting up NAT