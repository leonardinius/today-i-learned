# Shell: check tcp port

Periodically I find myself in the need to check for TCP port availability; and
no convenient tools available and not possible to install one.

Usually I use `netcat -z -v cidrc port` or telnet.

Lately I've found quite useful some under-appreciated feature of `bash` shell.
See [/dev/tcp][dev-tcp].

## Example

**Source:** http://stackoverflow.com/a/19866239

```console
# Connection successful:
$ timeout 1 bash -c 'cat < /dev/null > /dev/tcp/google.com/80' && echo ping || echo fail
ping
```

<!-- References -->
[dev-tcp]: http://www.tldp.org/LDP/abs/html/devref1.html#DEVTCP "/dev/tcp"
