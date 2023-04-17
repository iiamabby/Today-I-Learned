# Starting NOVNC in docker container 

I tried starting the NOVNC server as the `CMD` says `x11vnc not found`

[novnc commands](https://command-not-found.com/x11vnc)

> Fixed this issue by viewing the file structure from within the container when running. Found the path was not correct and there was a spelling mistake!

Also ports need to be mapped

`-p 5900:5900`
