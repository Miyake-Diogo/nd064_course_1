# Go Hello World

This is a simple Go web application that prints a "Hello World" message.

## Run the application

This application listens on port `6111`

To run the application, use the following command:
```
go run main.go 
```

Access the application on: http://127.0.0.1:6111/


# To use Dockerfile

You can pull my image from dockerhub
`docker pull miyakediogo/go-helloworld:v1.0.0`

To mount this image see Dockerfile in repo. 
I use this commands: 
```bash
# build the image
docker build -t go-helloworld .

# tag the image
docker tag go-helloworld miyakediogo/go-helloworld:v1.0.0

# push the image
docker push miyakediogo/go-helloworld:v1.0.0
```

To run I use:
```bash
docker run -p 6111:6111 miyakediogo/go-helloworld:v1.0.0  
```

After this go to browser and write `localhost:6111` to see helloworld webpage.