# Overview

This project was created to better understand the usage of NGINX helper when architecting microfrontends following [this](https://www.youtube.com/watch?v=5_Ie7ykJ9Iw&ab_channel=MatheusCastiglioni) tutorial.

# Architecture Overview

The NGINX orchestrator its done via build-code (after run `npm run build` in the projects).

When API is triggerd, it receives and manages the response managing the routes.

It decides to which microfrontend it should load at which moment.

Its worth also to remember that our orchestrator works like a reverse-proxy.

Which means that using the nginx reverse proxy, it provides one more abstraction level which assures the traffic flow from the client to the server.

It means that wheneaver we send a request, our reverse-proxy double checks wheather we have to send to which docker (microfrontend01, 02, or 3...).

# Commands

- `npm i`: to install the dependencies and to generate the `lock` file so the `build` docker step, works.
- `npm ci`: cleans the `node_module` folder if exists and assures that the dependencies are sync with the `package.json` specs.

# Docker

The idea is to have one docker with the `nginx` image for each microfrontend.

Then, once each each micrfrontend is deployed on each docker, on each container, on each nginx, we will create the orchestrator to communicate on each of these nginx microfrontends.

- `docker ps`: list the containers.
- `docker images`: list the images.
- `docker build -t app-react01 .`: build a new image from where the Dockerfile its accessing (`./`).
- `docker run --name orchestrator -d -p 80:80 nginx`: runs a docker with the `orchestrator` name, binding to the port `80`, using the `nginx` image.
- `docker run --name app-react01 -d -p 80:80 app-react01`: runs a docker with the `app-react01` name, binding to the port `80`, using the `app-react1` image.
- `docker rm -f app-react01 app-react02 app-react03`: stops several images w/ 1 command.
- `docker rm -f orchestrator`: stops the `orchestrator` docker image using the --force arg.
- `docker rm orchestrator`: deletes the `orchestrator` docker image.

- `docker-compose build orchestrator app-react01 app-react02 app-react03`: builds all the apps after some change.
- `docker-compose up`: recreates the containers and runs them.

![image](https://user-images.githubusercontent.com/8363610/100921873-e7f42d00-34bb-11eb-9313-68cfe75d39a6.png)
