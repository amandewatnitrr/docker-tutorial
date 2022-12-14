# Docker Registry

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/Docker1.png)

<p align="justify">
<strong>

- If the containers were the rain, than they will rain from docker registry which are the clouds. That's where the docker images are stored.
- It's a central repository of all docker images.
- Let's consider a simple example where we have a simple nginx container. We run the `docker run nginx` command, to run an instance of the nginx imgae. Let's took a closer look at this image name. The image name is nginx `image: nginx`.
- But what is this image and where is the image pulled form. This name follows Docker Image naming convention. "nginx" here is the image or the repository name. When we say `nginx`, it's actually `nginx/nginx`.
- The first part stands for the user or accounting. So, if we don't provide an account or repository name it assumes that it is same as the given name which in this case is nginx.
- The user name is usually, your Docker Hub account name or if it is an organization, then it's the name of the organization.
- If you were to create your own account and your own account and create your own repositories or images under it then you would use a similar pattern.
- Now where are these images stored and pulled from since we have not specified the location where these images are to be pulled from. It is assumed to be on docker default registry Docker Hub. The dns name for which is `docker.io`.
- The Registry is where all the images are stored. Whenever you create a new image or update an exsisting image we push it to the registry and everytime anyone deploys this application, it is pulled form that registry.
- There are many other popular registries as well. For example, Google's registry is at `gcr.io` where a lot of kubernetes  related images are stored. like the ones used for performing end to end tests on the cluster.
- These are all publically accessible images that anyone can download and access.
- When we have applications built in-house, that shouldn't be made available to the public, hosting an internal private registry maybe a good solution.
- Many cloud service providers such as AWS, Azure or GCP provide a private regitry by default, when you open a account with them.
- On any of these solutions be a Docker Hub or Google Registry or Internal Private Registry, you may choose to make a repository private, so that it can be accessed only using a set of credentials.
- From docker's perspective to run a container using an image from a private registry.You first login to your private registry using the `docker login private_registry_name.io` command and put your credentials.
- Once you successfully login, run the application using private registry as part of the image name as follows: `docker run private_registry_name.io/apps/internal-app`.
- Now if you did not login to the private registry, it will come back with an error saying that the image cannot be found. So, remember to always login before pulling or pushing to a private registry.
- We said that cloud providers like AWS or GCP provide a private registry when you create an account with them. But what if you are running your application on premise and don't have a private registry. How do we deploy our own private registry within our organization. The docker registry is intself another application and of course it's available as a docker image. The name of the image is registry and it exposes the API on port 5000.

    ```bash
        docker run -d -p 5000:5000 --name registry registry:2
    ```

- Now that we have our custom registry running at port 5000 on this docker host. How do we push our own image to it.
- Use the `docker image tag my-image-name localhost:5000/my-ima` command, to tag the image with a private registry URL in it. In this case, since it is running on the same docker host, I can use `localhost:5000` followed by the image name. I can than push my image to my local private registry using the `docker push localhost:5000/my-image-name` with the docker registry information in it, from there on I can pulll my image from anywhere within the network using either local hosts, if we are on the same host or the IP or Domain Name of my docker host, if I am accessing from another host in my environment.

# Docker Engine

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/Docker4.png)

- Docker engine as we have learned before is simply refered to a host with docker installed on it.
- When we install docker on a linux host, we're actually installing three different competence. The <img src="https://img.shields.io/badge/Docker_Daemon-2496ED?style=plastic&logo=Docker&logoColor=white">, <img src="https://img.shields.io/badge/REST_API_SERVER-FF6C37?style=plastic&logo=POSTMAN&logoColor=white"> and <img src="https://img.shields.io/badge/Docker_CLI-2496ED?style=plastic&logo=Docker&logoColor=white">.
- The Diocker Daemon is a backgorund process that manages docker objects such as the images, containers, volumes and networks. The Docker REST API SERVER is the API Interface that programs can use to talk to the Daemon and provide instructions.
- You could create your own tools using this REST API, and the Docker CLI is nothing but the Command line interface that we have been using untill now to perform actions such as running containers, stopping them, deleting them, deleting images etc.
- It uses the REST API to interact with the Docker Daemon. Something to note here is that, the Docker CLI need not nessecarily be on the same host. It could be on another system like a laptop and can still work with a remote docker engine.
- Simply use `-H` option on the docker command and specify the remote Docker engine Address and the port as follows:
  
  ```bash
    docker -H=remote-docker-engine:port_number
  ```

- For example, to run a container based on NGINX on a remote docker host run the command: `docker -H=10.123.2.1:2375 run -d --name=nginx nginx`.

## Containerization

- Docker uses namespaces to isolate workspace, process ids, network, inter-rocess communication mounts and UNIX time sharing systems are created in their own namespace thereby providing isolation b/w conatiners.<br>
  
- Let's take a look at one of the isolation technique "Prcoess ID Namespaces".<br>
  
- Whenever a Linux system boots up it starts with just one process with a Process ID of '1'. This is the root process and kicks off all the other processes in the system. By the time computer boots up completely, we have a handful of processes running. This can be seen by running the `ps` command to list all the running processes.<br>
  
- The Process IDs are unique and 2 processes cannot have same process ID. Now if we were to create a container which is basically like a child system within the current system. The child system needs to think that it is an independent system on it's own and it has it's own set of processes originating from a root process with a process ID of "1", but we know that there is no hard isolation b/w the containers and the underlying host. So, the process running inside the container are infact processes running on the underlying host.And so, 2 processes cannot have the same process ID of "1".<br>
  
- This is where namespaces come into play with process ID namespaces. Each process can have multiple processes associated with it. For example, when the processor start in the container it's actually just another set of processes on the base linux system and it gets the next available process ID, let'say 5 and 6. However, they also get another process ID starting with PID 1 in the container namespace which is only visible inside the container. So the container thinks that it has it's own root process tree and so it is an independent system. But how does that relate to an actual systm.<br>
  
- Let's say I want to run an NGINX server as a container. We know that the nginx and it's container runs an nginx service. If we were to list all the services inside the docker container we see that the nginx service is running with a PID of 1, this is the PID of the service inside of the container namespace. If we list the services on the docker host, we will see the same service but with a different PID. That indicates that all the processes are infact running on the same host but seprated into their own containers using namespaces. So, we learn that the underlying docker host as well as the containers share the same systme resources such as CPU and memory.<br>
  
- How much of the resources are dedicated to the host and the containers and how does docker manage and share the resources b/w the containers. By default there is no restriction as to how much of a resource a container can use and hence a container may end up utilizing all of the resources on the underlying host. Bu there is a way to restrict the amount of CPU or memory a container can use.<br>
  
- Docker uses 3 groups or control groups to restrict the amount of hardware resources allocated to each container. This can be done by providing the `--CPU` option to the docker run command. For example `docker run --cpu=0.5 ubuntu` command will ensure that the ubuntu container does not take up more than 50% OF THE host CPU at any given time. <br>
  
- The same goes with memory. For example running `docker run --memory=100m ubuntu` command. Here setting `--memory` option limits the amount of memory the container can use to 100MB only.

# Docker Storage

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/0_GdJIbmPH1JLSLSj1.gif)

- When we install docker on a system, it creates this folder structure at `var/lib/docker`. We have multiple folder under it called aufs, containers, image, volumes etc. This is where docker stores all it's data by default. When we say data we mean files related to images and containers running on the docker host.
- For example, all files related to containers are stored under the containers folder and the files related to images are stored under the image folder. Any volumes created by the docker container are created under the volumes folder.
- So, hwo exactly does docker stored the files of an image and a container. We need to understand Docker's Layered Architecture. When docker builds these images, it builds them in a layered architecture.
- Each line of instruction in the `dockerfile` creates a new layer in the docker image with the changes from the previous layer.
- Let's consider an example. Let's say we have 2 applications aand docker files for them as follows:
  
  ```Dockerfile
  
    FROM Ubuntu

    RUN apt-get update && apt-get -y install pyhton

    RUN pip install flask flask-mysql

    COPY . /opt/source-code

    ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run

  ```

  ```Dockerfile
  
    FROM Ubuntu

    RUN apt-get update && apt-get -y install pyhton

    RUN pip install flask flask-mysql

    COPY  app2.py /opt/source-code

    ENTRYPOINT FLASK_APP=/opt/source-code/app2.py flask run

  ```

  In the first application, the first layer is a base ubuntu operating system followed by the second instruction that creates a second layer which installs all the apt packages, and than the 3rd instruction creates a third layer with the python packages followed by the fourth layer that copies the source code over and then finally the fifth layer  that updates the entry point of the image since each layer only stores. Since each layer only stores the changes from the previous layer. It is reflected in the size as well.
  
  To consider the advantage of the layered architecture, let's suppose that we have an another application which has a different docker file represented by the 2nd one above, but is similar to our first application. As it uses the same base image as Ubuntu, uses the same python and flask dependencies but uses a different source code to create a different application. And so a different ENTRYPOINT as well.

  When we run the `docker build` command to build a new image for the 2nd application. Since, the first 3 layers of both the application are the same, docker is not going to build the first three layers. Instead it uses the same three layers, it built for the first application from the cache and only creates the last 2 layers with the new sources and the new ENTRYPOINT. This way docker builds images faster and efficiently saves disk space.

  This is also applicable if you were to update your application code. Whenever you update your application code such as the app.py, in this case docker simply reuses all the previous layers from the cache and quickly rebuilds the application image by updating the latest source code thus saving us a lot of time during rebuilds and updates.

  Once the build is complete you cannot the modify the contents of these layers and so they are read only and we can only modify them by initiating a new build. When we run a container based off of this image, using the docker urn command. Docker creates a container based off of these layers and creates a new writeable layer on top of the image layer. The writable layer is used to store data crated by container such as log files by the applications, any temporary files generated by the container or just any file modified by the user on that container. The life of this layer is only as long as the container is alive. When the container is destroyed this layer and all of the changes in it are also destroyed. Remember the same image layer is shared by all containers created using this image. If we were to login to the newly created container and say create a new file called temp.txt, it would create that file in the container layer which is read and write. We just said that files in image layer are read only, meaning we cannot edit anything in those layers. No, we can still modify this file, but before we save the modified file docker automatically creates a copy of the file in the read write layer and we will than be modifying a different version of the file in the read write layer. All future modifications will be done on this copy of the file in the read write layer. This is called `COPY-ON-WRITE` mechanism. The image layer being read only just means that the files in these layers will not be modified in the image itself. So the image will remain the same all the time untill we rebuild the image using the `docker build` command.

## Persisting Data on Docker

- What happens when we get rid of the container. All of the data that was stored in the container layer also gets deleted.The change we made to the app.py and the new temp file we created also get removed. So what if we wish to persist data. For example if we were working with a database and we would like to preserve data created by the container, we can add a persistent volume to the container. To do this first create a volume using the `docker volume create data_volume_name` command. When we run this command, it creates a folder called `data_volume_name` within the volumes folder inside the `/var/lib/docker/volumes` directory. Then when we run the `docker run -d -v data_volume_name:/var/lib/mysql mysql` command. We can mount this volume inside the docker containers read write layer using `-v` option like this. So, when we run the mentioned docker run command the data will be stored inside mysql folder which is the default location where mysql stores data and that is `var/lib/mysql`. This will create a new container and mount the data volume we created into var/lib/mysql folder inside the container. So all data written by database is infact stored on the volume created on the docker host. Even if the container is destroyed the data still remains.

- Now what if we don't run the `docker volume create data_volume_name` command to create the volume before the docker run command. For example if I run the `docker run` command to cretae a new instance of mysql container with the a non-exsisting volume, docker will automatically create a volume, name it and mount it to the container. We should be able to see all these volumes if we list the contents of var/lib/docker/volumes folder. This us called `Volume Mounting` as we are mounting in volume created by docker under the /var/lib/docker/volumes folder.

- But what if we already have our data at another location. For example, let's say we have some external data storage on the docker host at `/data` and we would like to store database data on that volume and not in the default var/lib/docker/volumes folder. In that case we would run a container using the `docker run -v /folder_path:/var/lib/mysql mysql` command. Here we will specify the complete path to the folder where we want data to be stored and so it will create a container and mount the folder to the container. This is called `Bind Mounting`.

- So, there are 2 types of mounts `Volume mounting` and `Bind Mounting`.
- Volume mount mounts a volume from the volumes directory and Bind Mount mounts a directory from another location on the docker host.
- Note: Using `-v` is an old option, `-mount` is more prefered way as it is more verbose. So, we have to specify each parameter in key equals value format. So the previous command can be written as `docker run \ --mount type=bind,source=/data/mysql, target=/var/lib/mysql mysql` using the type, source and target options respectively.
- So, who is responsible for doing all these operations, maintaining the layered architecture, creating a writeable layer, moving files across layers to enable copy and write etc. It's the storage drivers.
- So, docker uses `Storage Drivers` to enable layered architecture. Some of the common storage drivers are:
  - AUFS
  - ZFS
  - BTRFS
  - Device Mapper
  - Overlay
  - Overlay2
- The selecion of the storage driver depend on the underlying OS being used. For example with Ubuntu, the default storage driver is AUFS, while this driver is not available on other OS like Fedora or Cent OS. In that case device mapper may be a good option. Docker will choose the best storage driver avaialable automatically based on the OS. The different storage drives also have different performance and stability charateristics, so we may want to choose one that fits the need of our application and our organization.

</strong>
</p>
