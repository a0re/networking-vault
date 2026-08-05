--- 
Set User Mode Password
```
conf t 
line console 0
password ccna
login 
end
exit
```

Delete User Mode Password
```
conf t 
line console 0
no password 
no login 
end 
exit
```
```
___ 

Set Administrator Mode password
```
> conf t
> enable secret cisco
```

Delete Administrator mode password
```
conf t
no enable secret
```


Shutdown ports
```
conf t
interface [range] gigabitEthernet 1/0/1
shutdown
exit
```


```
conf t
interface [range] gigabitEthernet 1/0/1
no shutdown
exit
```

### `show ip interface brief`

Press Enter: Shows the remaining switches 

