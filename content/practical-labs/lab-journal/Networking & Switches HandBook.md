



### Display Brief

```

```

### Clean Network

```
ena
write erase
delete vlan.dat
reload
```


### ARP
```
arp -d
arp -a
```



### Privilege Escalation


### Set-Up VLAN


### Disable DNS


### Enable SSH
> Note: Disabling Telnet is not required to enable SSH but good practice

#### Disable Telnet Services | 
```
line vty 0 15 
transport input none
no login
end
```



### Virutal Machine Launcher 









### Setting Hostname on a Switch - conf t

```
ena 
conf t
hostname [insert name here]
end
```

### Set MOTD (Message of the Day) on Switches


> [!note] `+` & `#` : can be replaced by anything since it uses the character as a flag to determine the start & end of the MOTD.

```
ena 
conf t
banner motd [insert message here]
commit 
end
```

### Check for MOTD

```
show running-config | include banner [keyword]

keywords 
```

### Setting a password on `ena`

```
ena 
conf t
line console 0 
```