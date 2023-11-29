debugging-snippets
-------------------

- [Network](#network)
- [Processes](#processes)

Useful debugging snippets for everyday operations. ⚙️

----

<a id="network"></a>
# 1. Network

## Show all open TCP connections
Show open TCP connections with source and destination addresses and ports.
ℹ️️ Replace `tcp` with `tcp6` at the end to show IPv6 connections.

```shell

```shell
awk 'function hextodec(str,ret,n,i,k,c){
    ret = 0
    n = length(str)
    for (i = 1; i <= n; i++) {
        c = tolower(substr(str, i, 1))
        k = index("123456789abcdef", c)
        ret = ret * 16 + k
    }
    return ret
}
function getIP(str,ret){
    ret=hextodec(substr(str,index(str,":")-2,2)); 
    for (i=5; i>0; i-=2) {
        ret = ret"."hextodec(substr(str,i,2))
    }
    ret = ret":"hextodec(substr(str,index(str,":")+1,4))
    return ret
} 
NR > 1 {{if(NR==2)print "Local - Remote";local=getIP($2);remote=getIP($3)}{print local" - "remote}}' /proc/net/tcp
```

# Show all HTTP requests on a port
Show the HTTP path of a request on a port.
ℹ️ Replace the port number accordingly.
```shell
sudo tcpdump -s 0 -v -n -l port 8080 | egrep -i "POST /|GET /|HEAD /|PUT /|DELETE /|CONNECT /|OPTIONS /|TRACE /|PATH /"
```

<a id="processes"></a>
# 2. Processes

## Find PID by port
Find a PID which has opened a port.
ℹ️ Replace the port number with the port you want to find.
```shell
sudo ss -lptn 'sport = :8080'
```

## Find port by PID
Find the ports belonging to a PID.
ℹ️ Replace the PID with the PID you want to find.
```shell
sudo netstat -ltnup | grep 'LISTEN' | grep '123/'
```

