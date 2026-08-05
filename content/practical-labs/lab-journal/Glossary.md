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

| Interface          | IP Address  | OK? | Method | Status                | Protocol |
| ------------------ | ----------- | --- | ------ | --------------------- | -------- |
| GigabitEthernet0/0 | 192.168.1.1 | YES | manual | up                    | up       |
| GigabitEthernet0/1 | unassigned  | YES | unset  | administratively down | down     |








