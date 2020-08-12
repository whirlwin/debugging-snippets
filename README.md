# shell-snippets
-------------------

Useful shell snippets

----

## Show all running processes by TCP ports

```shell
lsof -i -P -n \
    | grep LISTEN \
    | awk '{print $9}' \
    | sed 's/.*://' \
    | xargs -I {} lsof -n -i :{} \
    | grep LISTEN \
    | awk '{print $2}' \
    | xargs -I {} sh -c 'cat /proc/{}/cmdline; echo "";'
```

