
Below is an example of one of the most common developer workflows which involves: containerizing an app > Pushing image to registry > Deploying the app > Running the app.

## Step1: Create a docker file

Let us create a docker file (basically a plain text file). It has instructions to build an image. 

```
FROM openjdk:11-jre-slim
WORKDIR /app
COPY target/java-hello-world-0.0.1.jar java-hello-world.jar
ENTRYPOINT ["java", "-jar", "/app/java-hello-world.jar"]
EXPOSE 8080

```

## Step 2: Create an Image


Run the below command in the terminal to create an image. The context directory contains the Dockerfile and related files that supposted to be included in the image. 

```
docker build -t java-hello-world:1.0.0 .
```

Run `docker images` on the terminal to see the list of images in your local Docker Engine environemnt.


## Step 3: Run app in container

```
docker run -d -p 8080:8080 java-hello-world:1.0.0
```

To check if the app is up and running run `curl localhost:8080` in the terminal and check if the app responds with `Hello World!`.

## Step 4: Publish image to a registry

```
docker login --username=<username> --password=<password>


docker tag java-hello-world:1.0.0 <username>/java-hello-world:1.0.0

docker push <username>/java-hello-world:1.0.0

```


