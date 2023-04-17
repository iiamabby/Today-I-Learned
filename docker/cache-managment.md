# Optimizing Cache Managment 

> Once a layer changes, then all downstream layers need to be rebuilt as well. Even if they wouldn’t build anything differently, they still need to re-run. 

 
##  Using the cache efficiently 
 Refactor and optimize the docker file by organizing your layers 

Inefficent example: The Copy files are prone to changes, meaning the installs will have to re-run after each change.

        # syntax=docker/dockerfile:1
        FROM
        Learn more about the "FROM" Dockerfile command.
        node
        WORKDIR /app
        COPY . .          # Copy over all files in the current directory
        RUN npm install   # Install dependencies
        RUN npm build     # Run build 


- Efficent example: Splitting the copy in to 2 iterations, the first been needed for the commands that follow and the last been the src directory that is prone to change 

            # syntax=docker/dockerfile:1
            FROM node
            WORKDIR
            Learn more about the "WORKDIR" Dockerfile command.
            /app
            COPY package.json yarn.lock .    # Copy package management files
            RUN npm install                  # Install dependencies
            COPY . .                         # Copy over project files
            RUN npm build                    # Run build  ```
