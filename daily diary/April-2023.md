# Docker containers 

## April 14th 2023 
- Today i created a container based from an ubuntu image, with this container i was able to run a javascript website on my local network by exposing and mapping internal ports to the external ports by using: `-p 8000:80` flag when running the container and `EXPOSE 80` from within the `Dockerfile` using [`nginx`](https://www.nginx.com/) as the web server
- Then i could access the server through my laptops ip address and port number from any device connected to the same network.
- I also learnt how to run a docker container as an interactive environment for debugging by using `docker exec <container name> -it bash` 
- Then i created a react-app container based of **node:alpine** to run a react app from github in a peer programming setup.
- I researched volumes and bindings to keep my data persisent across restarts. 
- [docker run](https://docs.docker.com/engine/reference/commandline/run/#publish) to see all of the flag options 

## April 17th 2023

- Created an image to run Minecraft client JDK in a containerized environment
- Created a git repo to store the code for the minecraft project 
- Researched minecraft-launcher and how this works in the CLI 
- Researched ./gradlew commands to help with the automation process
- Practiced using docker flags `-it` & `bash` to debug running containers
- Continously worked on the TIL repo. 
### Issues 

- To be able to run the interface of the project some variables need to be declared in the Dockerfile see `docker/docker-interfaces.md`. 
- Container runs `-it` but is not able to keep its state due to the GUI not been set correctly.

## April 18th 2023
- Fixed minecraft-client issue of novnc not found
- Researched docker best practices, cache managment, live reload.
- Learnt about heredocs see `best-practices.md` 
- Learnt about docker watchdog see `live-reload.md`
- Updated TIL repo 


# April 19th 2023

- Paired with alex
- Attempted Nvidia cuda implementation on windows AMD build with wsl
- Researched tigerVNC and NoVNC, I think this is going to be the way 
- Created a gitlab for this project with advice from Zach 

