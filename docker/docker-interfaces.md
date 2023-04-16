# GUI in docker 

Whilst running the container as an interactive environment, i ran the command `./gradlew runClient` from within the bash shell, i noticed that all of the graphics output properties where set to unkown or 0, the build failed due to these reasons. 

Docker does not provide a GUI to execute applications 
You need to tell docker how and what it should use to display the GUI. 

## X11
You can do this by using flags on the `run` command, the below command uses the host's machine graphics card (x11).

`-–env=”DISPLAY” –net=”host” <tag-name>`

or 

Setting the variables in the Dockerfile

`ENV DISPLAY=${DISPLAY}`
`ENV network_mode = "host"`
`VOLUME [ "/tmp/.X11-unix:/tmp/.X11-unix" ]`

> X11 is not a secure way of doing this as it gives the container access to the gpu to be able to draw on the screen but for testing purposes its fine.