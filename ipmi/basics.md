IPMI basic commands
===================

For all the following examples, the selected channel is 1. This may change
depending of your system.

Print LAN configuration
-----------------------

`ipmitool lan print`

Set static IP
-------------

```
ipmitool lan set 1 ipsrc static
ipmitool lan set 1 ipaddr 192.168.1.12
ipmitool lan set 1 netmask 255.255.255.0
ipmitool lan set 1 defgw ipaddr 192.168.1.1
```

Set dynamic IP
--------------

`ipmitool lan set 1 ipsrc DHCP`

Set VLAN
--------

`ipmitool lan set 1 vlan id 12`

My change is done but the IPMI is not applying it!
--------------------------------------------------

On old systems, a reboot of the management controller (abreviated `mc`) needs
to be rebooted in order to apply the new configuration.

`ipmitool mc reset cold`

Also, if the modification only impact VLAN settings, a simple raw command can
do the trick ([source](http://www.mail-archive.com/ipmitool-devel@lists.sourceforge.net/msg00095.html)).

`ipmitool raw 0x0c 1 1 0 0`

Set a user/pass
---------------

```
ipmitool user set name $USER_ID $USERNAME
ipmitool user set password $USER_ID $PASSWORD
```

Use the IPMI remotely
---------------------

Just replace the `ipmitool` command with:
`ipmitool -I lanplus -H $HOST_IP -U $USERNAME -P $PASSWORD`

---

References:
* [Thomas Krenn's website](https://www.thomas-krenn.com/en/wiki/Configuring_IPMI_under_Linux_using_ipmitool)
* [IBM documentation](https://www.ibm.com/support/knowledgecenter/en/linuxonibm/liabw/liabwresetDHCP.htm)
* [Computer cheese's blog](https://computercheese.blogspot.com/2013/12/ipmi-vlan-configuration.html)
