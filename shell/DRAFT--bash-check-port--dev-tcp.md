http://stackoverflow.com/a/19866239

```
# Connection successful:
$ timeout 1 bash -c 'cat < /dev/null > /dev/tcp/google.com/80'
$ echo $?
0
```
