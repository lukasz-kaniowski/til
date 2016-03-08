## Running development environment inside of docker container

Quickly run any development environment with docker, ie. nodejs:

1. Create workspace ```$ mkdir ~/my-new-project && cd ~/my-new-project && echo "console.log('test')" >> app.js```
2. Run node docker interactive environment ```$ docker run -it --rm -v $(pwd):/src -w /src node:4 /bin/bash```
3. Inside of container execute ```$ node app.js```

Docker command maps current directory to ```/src``` inside of the container and then uses it as working directory. 

## Sources:


[http://blog.benhall.me.uk/2016/03/docker-as-an-alternative-to-runtime-version-managers/](http://blog.benhall.me.uk/2016/03/docker-as-an-alternative-to-runtime-version-managers/)
