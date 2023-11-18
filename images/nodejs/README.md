
## savatin/nodejs

Run this [nodejs](https://nodejs.org/) image with:

    $ docker run -it --rm savatin/nodejs

Includes [npm](https://docs.npmjs.com/) and [yarn](https://yarnpkg.com/en/)

The image comes with a `nodejs` user for unprivileged container usage. To ease development pains
you can use the provided [ONBUILD](https://docs.docker.com/engine/reference/builder/#onbuild) instructions. Docker-Compose example:

```
version: '2.2'
services:
  app:
    build:
      dockerfile: ${PWD}/docker/Dockerfile
      context: ./docker
      args:
        - NODEJS_UID=${UID}
        - NODEJS_GID=${GID}
    user: nodejs
    working_dir: ${PWD}
    volumes:
      - ~/.npm:/home/nodejs/.npm
      - ${PWD}:${PWD}
```

The referenced Dockerfile:

```
FROM savatin/nodejs
```

The `ONBUILD` instructions are triggered by setting the `NODEJS_UID` and `NODEJS_GID` docker build args
and will set the image's `nodejs` user uid/gid to the passed values. Then we just mount the local `.npm` folder and
the project source for a close to native development environment.

Note: The example above expects the environment variables `UID` and `GID`, on Bash `UID` is an internal
variable and not exported by default! You may want to add the following to your `~/.bashrc`:

```
export UID
export GID="$(id -g $(whoami))"
```

---

![PACKAGES](PACKAGES.md)