# Virtualbox / Win / Port forwarding

Recently I helped out a team which used Win + Virtualbox to manage certain
legacy system in isolation.

This is where I've discovered a windows `iptables` alternative how to
effectively perform port forwarding between Win7 Host and Virtualbox
host-bridge network.

It relies on [netsh][1] utility availability in order to perform that.

```console
netsh interface portproxy delete v4tov4 listenport=1111 listenaddress=192.168.2.1
netsh interface portproxy add v4tov4 listenport=1111 listenaddress=192.168.2.1 connectport=2222 connectaddress=my.host.com
```

Notes:
- `1111` - port to listen to on Virtualbox host-bridge interface
- `192.168.2.1` - Netsh commands for Interface IP address
- `2222` - port to redirect to
- `my.host.com` - the fqdn address to redirect traffic to

_Vuala._
<!-- References -->

[1]: https://technet.microsoft.com/en-us/library/bb490939.aspx "Netsh commands for Interface IP"
