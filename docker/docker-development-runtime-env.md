# Running development environment inside of docker container

Quickly run any development environment with docker

## Nodejs

1. Create workspace ```$ mkdir ~/my-new-project && cd ~/my-new-project```
2. Create file ```app.js``` with content: 

      ```javascript
      console.log('test');
      ```
3. Run node docker interactive environment ```$ docker run -it --rm -v $(pwd):/src -w /src node:4 /bin/bash```
4. Inside of container execute ```$ node app.js```

## Go 

1. Create workspace ```$ mkdir ~/go-app && cd ~/go-app```
2. Create file ```app.go``` with content: 

    ```go
    package main
    import "fmt"
    func main() {
        fmt.Println("hello world")
    }
    ```

3. Run node docker interactive environment ```$ docker run -it --rm -w /go/src/go-app -v $(pwd):/go/src/go-app golang:1.4.2```
4. Inside of container execute ```$ go run app.go```

# Sources:

[http://blog.benhall.me.uk/2016/03/docker-as-an-alternative-to-runtime-version-managers/](http://blog.benhall.me.uk/2016/03/docker-as-an-alternative-to-runtime-version-managers/)
