Enable IPMI kernel modules
==========================

Once the `ipmitool` package is installed, it may still be unusable because
of missing kernel modules. In order to load them, run:

```
modprobe ipmi_devintf
modprobe ipmi_si
```

---

Source: [serverfault](https://serverfault.com/questions/480371/ipmitool-cant-find-dev-ipmi0-or-dev-ipmidev-0)
