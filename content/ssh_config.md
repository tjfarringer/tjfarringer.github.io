Title: SSH Config
Date: 2015-03-27
Category: tutorial
Tags: ssh, tutorial

cription
In our current setup, to access a machine, we need to go through a jump server.
Without an ~/.sssh/config file, the step involves multiple ssh commands

This guide shows how to ssh to remote servers with a single ssh command


# Without ssh config
a) Connect to host "server" using host "jump"
```
ssh -v jump -t ssh -v server
```
or
```
ssh -o ProxyJump=jump server
```

b) Connect to host "server" using host "jump" with port forwarding for port 8082
```
ssh -v -L8082:localhost:8082 jump  -t ssh -v -L8082:localhost:8082 server
```

# With ssh config
a) Connect to host "server" using host "jump"
```
ssh -v server
```

b) Connect to host "server" using host "jump" with port forwarding for port 8082
```
ssh -v -L8082:localhost:8082 luigi
```


# Steps
1. edit ~/.ssh/config on your local machine
```
nano ~/.ssh/config
``` 
2. Copy the attached ssh_config file

# Tests
1. ssh to our etl machine (luigi)
```
ssh -v luigi 
```
2. ssh to our etl machine (luigi) with port forwarding
```
ssh -v -L8082:localhost:8082 luigi
```
3. copy local file to etl machine 
```
scp -r data/sample.txt luigi:/tmp
```



# Notes
1. Old Version of ssh (openss)
If you have an older version of ssh (<7.3), the directive ProxyJump won't work. 
You have to replace the line 
```
ProxyJump jump
```
with
```
ProxyCommand ssh -q -W %h:%p jump
```

2. Lingering ssh connections
Sometimes, when you close the ssh connection, the ssh tunnel might still be running. To force kill the tunnel, kill all process running on the opened port

all process running on port 8082 
```
lsof -i :8082

```

kill process (PID)
```
kill -9 <Process ID>
```
