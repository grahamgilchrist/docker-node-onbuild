# docker-node-onbuild

Dockerfile repo for nodeJS onbuild docker container. 

Available from the the docker hub regsitry as `grahamgilchrist\node-onbuild`

The default [official NodeJS docker hub repo](https://registry.hub.docker.com/_/node/) offers an 'onbuild' option, which copies and installs packages from a `package.json` folder in the project. 

This works great with containers which are meant to be built once, but for development work, we frequently want to mount a project directory as a volume inside our docker container to allow us to instantly see modifications to the source code without rebuilding. Sadly this isn't possible with the official docker hub repo, as mounting the project directory to `/usr/src/app` will overwrite the `node_modules` directory from the build step.

This container image solves that by installing all `node_modules` into a directory one level above the mounted folder, since node will recursively check up the directory tree for modules. The modules are thus still entirely contained inside the docker container and not the mounted directory.
This also has the side benefit that node modules can also be temporarily loaded into the `node_modules` directory of the mounted folder for quick testing, as they will take priority over those one directory up inside the container.
