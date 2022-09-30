<p align="justify">
<strong>

# Basic Docker Commands

<img src="https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/docker-volume-mapping-header.gif" align="center">

## `docker run <image-name>` - Run a docker container

- The Docker run command is used to run a container from an  image.
- Running the `docker run <image-name>` command will run an instance of that image application on the docker host, that is it will create a container if it already exsists.
- If the image is not present on the host, it will go out to docker hub and pull the image down. But this only happens first time, for the subsequent executions, the same image will be reused.

## `docker ps` - List docker containers

- The `docker ps` command lists all the running containers and some basic info about them such as `CONTAINER ID`, `IMAGE`(image name) we used to run the containers, the current status and name of the container.
- Each container automatically gets a random ID and name created for it by docker.
- To see all the containers running or not use `-a` option as `docker ps -a`. This outputs all running as well as previously stopped or exited containers.

## `docker stop <container-id or container-name>` - Stop a Container

- For the `docker stop <container-id or container-name>` command, we must either provide the container id or the container name which we want to stop.
- This takes the container to an exited state.
- Note: This command doesn't actually completely removes it, it just stops it.

## `docker rm <container-id or container-name>` - Remove a Container

- If we want to get rid of the container, use the `docker rm` command to remove a stopped or exited container permanently.
- If it prints the container-id or the name back, you can clearly see using `docker ps -a` that thte container has now been completely deleted and doen't exsist or occupy space on the host anymore.

## `docker images` - Lists Images on Host

- `docker images` lists all available images with their respective sizes that are present on the host.

## `docker rmi <image-name>` - Removes Images

- To remove an image that you no longer plan to use, run the `docker rm <image-id or image-name>` command.
- Note: Remember that we must ensure that the image we are trying to remove no container are running off of that image. We must stop and delete all dependent containers to be able to delete the image.

## `docker pull <image-name>` - Download an image

- Use the `docker pull <image-name>` command to only pull that image and not run the container.
- `docker pull <image-name>` and `docker run <image-name>` don't work the same way as when we use `docker run <image-name>` with an image that is not present locally, `docker run` calls `docker pull`.
- So, the `docker run <image-name>` pulls the specific image and starts it on our host.
- Let's take an example.
  - Say we are to run a docker container from an image when we run `docker run <image-name>` command, it runs an instance of the specified image and exits immediately. If we were to list the running containers, we wouldn't see the container running.
  - Now, when we run `docker ps -a` and list all the containers including the exited or stopped ones, we will see the new container we ran is in exited stage.
- Unlike VMs, Containers are not meant to be hosted on an Operating System. Containers are meant to run a specific task or processes such as to host an instance of a web-server or application-server or database or simply to carry some kind of computation or analysis task. Once the task is complete the container exits.
- A Container only lives as long as the process inside it is alive.
- If the web service inside the container is stopped or crashes then container exits.This is why when you run a container from an image it stops immediately because it's just an image that is used as base image for other applications.There is no process or application running in it by default.

## `Append a Command`

- If the image isn't running any service as in the case with a certain image you could instruct docker to run a process with the docker run command.
- For example a `sleep` command with duration of 5 seconds like `docker run ubuntu sleep 5`.
- When the container starts it runs sleep command and goes into sleep for 5 seconds, close to it the sleep command exits and the container stops what we just saw was executing a command when we run the container.

## `Exec` - Execute a Command

- What if we would like to execute a command on a running container.
- For example when I run the `docker ps` command, we will see a container which uses the image and sleeps for specified time.
- Let's say I want to view the contents of the file inside this particular container, we can use the docker exec command to execute a command on my docker container in this case to print the content of a certain file location.
  - example:
    `docker exec container-name cat /dir1/dir2/dir3`

## `Run` - attach and detach

- When we run a docker rum command let's say `docker run <container-name>`, it runs in the foreground or in an attached mode meaning you will be attached to the console or the standard out of the docker container and you will see the output of the web service on your screen.
- We won't be able to do anything else on this console other than view the output untill this docker container stops it won't reposnd to your inputs.
- Press `CTRL + C` combination to stop the container and the application hosted on the container exits and you get back to your prompt.
- Another option is to run the container in detached mode by providing `-d` option as follows:
  - `docker run -d <image-name>`
- This will run the docker container in background mode and we will be back to our prompt immediately. The container will continue to run in the backend. Run the `docker ps` command to view the running docker container.
- Now if you want to attach back to the running container later, run the docker attach command and specify the name or id of the docker container as follows:
  - `docker attach <container-id or container-name>`
- Remember, if you are providing the ID of the container in any docker command  you can simply provide the first few charaters alone just so it is different from the other container ID on the host.
- We can also run and name a container using the command `docker run --name <container-name> <image-name>`.

### `run -tag` - What if we want to run a spcific version of a image??

- In that case we use the concept of tag.
- Let's suppose we want to use version 4.0 of an image, we do it using the command:
  - `docker run <image-name>:version`
- When the version is specified, docker instead of pulling the latest version available of the image, it pulls the specified version of the image and runs that.
- Note: If we don't specify any tag, docker will consider the default tag to be latest.
- Latest is the tag associated to the latest version of that imgae/software which is governed by the authours of that image or software.
- But, what if we want to have info about the versions of the image available.
  - At hub.docker.com lookup an image and you will find all the supported tags in it's despcription.
  - Each version of the software can have multiple short and long tags associated with it.

### `run -it` - User Command or Input during run

- By default the docker container doesnot listen to any standard input, even though you are attached to it's console, it doesnot read any input from the user. It doesn't have any terminal to read inputs from.
- It runs in a non-interactive mode.
- If you would like to provide your input, you must map the standard input of your host to the docker container using `-i` parameter.
- The `-i` parameter is for interactive mode and when i input my name, it prints the expected output.
- But there's still something missing the prompt.
- When you run a app, it wpould ask you for the input, but when dockerised the prompt is missing, even though it seems to have accepted my input. That is because the application prompt on the terminal and we have not attached to the containers terminal.
- For this we use `-t` option as well, together use it as `-it`. `t` stands for pseudo terminal.

### `docker run -p local_port:container_port --name container name <image-name>` - Port Publishing on Containers

- The Underlying host where Docker is installed is called Docker Host or Docker Engine.
- When we run a containerized web application, it runs and we are able to see that the server is running.

- But than how does a user access my application??

  - As we can see, my application is listening on port `x.y.z.k:port_number`, so we can access this application by using this mentioned port number.
- But what IP do I use it to access it form a Web Browser??
  
  - For this there are 2 options available:
    - Using the IP of the Docker Container. Every Docker container gets an IP Assigned by default. But remember this is an internal IP and is only accessible within the docker host. So, when we open the application on browser from within the dcoker host, you can go to `http://w.x.y.z:port_number` to access the IP Address.
    - But since, the IP above we talked about is an internal IP, users outside the docker host cannot access it using this IP. For this we can use the IP of the docker host whcih is `k.l.m.n` but for that to work we need to map the port inside the docker container to a free port on the docker host.
      - For example, If I want my users to access the application through port 80 on my docker host, we can map port 80 of local host to port 5000 on the docker container using `-p` parameter in my run command like `docker run -p local_port:container_port --name container name <image-name>`.
      - And so the user can access the application by going to the URl obtainer from the docker.
      - And all trafic on port 80 on my docker host will get routed to port 5000 inside the docker container.

### Volume Mapping

- Let's now have a look at how data is persisted in a Docker Container.
- For example let's say you were to run a MySQL Container. When Databases and tables are created. The datafiles are located at some location inside the docker container. Remember the docker container has it's own isolated fiel system and any changes to any files happen within the container. let's assume we dump a lot of data in our database, what if we delete or remove the MySQL Container. As soon as we do that the container along with all the data inside it gets blown away, meaning all your data is gone.
- If we would like to persist data, we want to map a directory outside the container on the docker host to a directory inside the container.
- In this case I create a directory and map that to MySQL inside the docker container using `-v` option and specifying the directory on the docker host followed by a colon and the directory inside the docker container.
- This way when the docker container runs , it will implicity mount the external directory to the folder inside the docker container.
- This way all the data will be stored in external volume at the specified location data directory and thus the data persists even if we delete the docker container.

## `docker inspect <container_name or container_ID>` - Inspect the container

- The `docker ps` command is good enough to get basic details about containers like their names and IDs.
- But if we want to see additional details about container use the `docker inspect <container_name or container_ID>` command.
- It returns all details of a container in a JSON Format such as the state, mounts, configuration data, network settings etc.
- Remeber to use it when you are required to find details on a container.

## `docker logs <container_name or container_ID>` - Container Logs

- Used to view the logs which happens to be the content written to standard out of the container.

# Demo Examples

- `docker run -it centos bash`
  - The first thing you see is the prompt has changed.
  - So, what happens when we run this command is docker ran the centos container which is the centos operating system and than run the bash command and it logged me in directly to the container since we specified the `-it` option.
  - So, you can check if you are really yhere by running the command

  ```bash
    cat /etc/*release*    
  ```

  - We can exit it by using the `exit` command.

</strong>
</p>
