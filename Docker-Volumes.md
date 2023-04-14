
# Docker volumes: 

Volumes and mount binds are a way to persist data across versions of your container, when you stop a container and re-start the data is lost as it is static within
the container, using volumes you can persist the data across all instances of the container. 

`docker volume ls`
- lists the volumes that have been created

`docker volume create <name>`
  - creates a new named volume

 `docker volume rm <name>`
  -removes the volume aslong as it not connected to a container
  

`docker volume inspect <name>`
  - inspect volume to view the state 
  
`docker prune` 
  - prune a volume with confirmation 

 
> State data lives on the host machine 
