# docker-node-onbuild

Dockerfile repo for nodeJS onbuild docker container. Based on a basic node environment with a volume compatible onbuild step

Available from the the docker hub regsitry as `grahamgilchrist\node-onbuild`

## Background
The default [official NodeJS docker hub repo](https://registry.hub.docker.com/_/node/) offers an 'onbuild' option, which copies and installs packages from a `package.json` folder in the project. 

This works great with containers which are meant to be built once, but for development work, we frequently want to mount a project directory as a volume inside our docker container to allow us to instantly see modifications to the source code without rebuilding. Sadly this isn't possible with the official docker hub repo, as mounting the project directory to `/usr/src/app` will overwrite the `node_modules` directory from the build step.

This container image solves that by installing all `node_modules` into a directory one level above the mounted folder, since node will recursively check up the directory tree for modules. The modules are thus still entirely contained inside the docker container and not the mounted directory.
This also has the side benefit that node modules can also be temporarily loaded into the `node_modules` directory of the mounted folder for quick testing, as they will take priority over those one directory up inside the container.

Repo structure is based on the same version folder convention as official docker hub repos (e.g. python: https://github.com/docker-library/python)

# Usage
* Mount a directory with a `package.json` file and anything else you want to be available (e.g. node project files) to `/usr/src/app`
* The docker build step will copy `package.json` into the container and install the packages one directory above the project
* `docker run -v ./:/usr/src/app grahamgilchrist/node-onbuild:0.12 node-command`

# Use with docker compose:
`docker-compose.yaml`:
```
node:
  image: grahamgilchrist/node-onbuild:0.12
  volumes:
    - ./:/usr/src/app
```
* `docker-compose run node node-command`
