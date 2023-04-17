# Update state of container when modifying src code

> How do I keep live reload active while running the app in a container and modifying the source code from my IDE on the host machine?

- The below command mounts the project directory in the container and passes env variable to be able to use chokidar to track changes of the `npm` project using [CHOKIDAR](https://github.com/paulmillr/chokidar)
`docker run -e CHOKIDAR_USEPOLLING=true  -v ${PWD}/src/:/code/src/ -p 3000:3000 repository/image_name`

- [Docker watchdog](https://betterprogramming.pub/live-reloading-with-docker-compose-for-efficient-development-356d50e91e39)

A docker-compose file that contains a live reload image to keep track of changes, git repo is available for this.

