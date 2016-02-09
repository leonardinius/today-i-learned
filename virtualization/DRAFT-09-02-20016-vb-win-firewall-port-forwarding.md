```console
netsh interface portproxy delete v4tov4 listenport=1111 listenaddress=192.168.2.1
netsh interface portproxy add v4tov4 listenport=1111 listenaddress=192.168.2.1 connectport=1111 connectaddress=my.host.com
```
