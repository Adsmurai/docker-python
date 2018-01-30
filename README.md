# Adsmurai's Dockerized Python

You can find the published images in
[our own docker repository](https://hub.docker.com/r/adsmurai/python/).

## Usage instructions

For example, if you want to execute the build task
```bash
docker run                                                  \
       --rm                                                 \
       -it                                                  \
       -v /home/user/sourcedir/:/home/apprunner/app         \
       adsmurai/python:3.6.3-alpine
```
