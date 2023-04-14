# Docker containers 

## April 14th 2023 
- Today i created a container based from an ubuntu image, with this container i was able to run a javascript website on my local network by exposing and mapping
internal ports to the external ports by using: `-p 8000:80` when running and `EXPOSE 80` from within the `Dockerfile`
- Then i could access the server through my laptops ip address and port number from any device connected to the same network.
- I also learnt how to run a docker container as an interactive environment for debugging by using `docker exec <container name> -it bash` 
- Then i created a react-app container based of **node:alpine** to run a react app from github in a peer programming setup.
- I researched volumes and bindings to keep my data persisent across restarts. 

- [docker run](https://docs.docker.com/engine/reference/commandline/run/#publish) to see all of the flag options 
