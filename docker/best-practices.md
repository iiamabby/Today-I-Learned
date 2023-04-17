# Collection of helpful links to writing efficient Dockerfile

> Frustrating waiting for the dockerfile to rebuild everytime i make a change in the Dockerfile. Here are some tips and links to speeding up the process.

- [Speed Up Your Development Flow With These Dockerfile Best Practices ](https://www.docker.com/blog/speed-up-your-development-flow-with-these-dockerfile-best-practices/)

## Key points 

- Pay mind to the layers, keep them small and organised 
- Layers like `COPY` should be near the end of the `Dockerfile` if they can be 
- Any layer prone to change should be at the bottom of the `Dockerfile`
- Be considerate of the files you add to the image, dont copy the entire folder if you only need 1 file from it
- Use the `RUN ` command, has a built in cache but to fine tune to only run when there is changes in the package you can do:

    RUN \
    --mount=type=cache,target=/var/cache/apt \
    apt-get update && apt-get install -y git

- Minimize number of layers by coupling commands using `&&` etc 
- Use multi stage builds: 

        # syntax=docker/dockerfile:1

        # stage 1
        FROM alpine as git
        RUN apk add git

        # stage 2
        FROM git as fetch
        WORKDIR /repo
        RUN git clone https://github.com/your/repository.git .

        # stage 3
        FROM nginx as site
        COPY --from=fetch /repo/docs/ /usr/share/nginx/html

- Use [heredocs](https://en.wikipedia.org/wiki/Here_document) to neatify your code blocks.

            RUN <<EOF
            set -e
            echo "the first command"
            echo "the second command"
            EOF
> File literal that uses string indentation to run code blocks or files.