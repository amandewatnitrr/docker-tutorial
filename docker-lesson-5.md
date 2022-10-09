<p align="justify">
<strong>

# Docker Registry

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/Docker1.png)

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

- Docker uses namespaces to isolate workspace, process ids, network, inter-rocess communication mounts and UNIX time sharing systems are created in their own namespace thereby providing isolation b/w conatiners.
- Let's take a look at one of the isolation technique "Prcoess ID Namespaces".
- Whenever a Linux system boots up it starts with just one process with a Process ID of '1'. This is the root process and kicks off all the other processes in the system. By the time computer boots up completely, we have a handful of processes running. This can be seen by running the `ps` command to list all the running processes.
- The Process IDs are unique and 2 processes cannot have same process ID. Now if we were to create a container which is basically like a child system within the current system. The child system needs to think that it is an independent system on it's own and it has it's own set of processes originating from a root process with a process ID of "1", but we know that there is no hard isolation b/w the containers and the underlying host. So, the process running inside the container are infact processes running on the underlying host.And so, 2 processes cannot have the same process ID of "1".
- This is where namespaces come into play with process ID namespaces. Each process can have multiple processes associated with it. For example, when the processor start in the container it's actually just another set of processes on the base linux system and it gets the next available process ID, let'say 5 and 6. However, they also get another process ID starting with PID 1 in the container namespace which is only visible inside the container. So the container thinks that it has it's own root process tree and so it is an independent system. But how does that relate to an actual systm.
- 


</strong>
</p>
