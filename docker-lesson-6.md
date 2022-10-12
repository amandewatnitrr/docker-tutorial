# Docker Networking

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/Docker5.png)

<p align="justify">
<strong>

- When we install docker, it creates three networks automatically: `Bridge`, `None` and `Host`
- Bridge is the default network a container gets attached to.
- If we would like to associate the network with any other network, we specify the network information using `--network` option.

## Bridge Network

- The Bridge Netowrk is a private internal network created by Docker on the host. 
- All containers are attached to this network by default and they get an internal IP address usually in the range 172.17 series. 
- The containers can access each other using Internal IP if required.
- To access any of these containers from the outside world, map the port of these containers to ports on the docker host as we have seen before.

## Host Network

- Another way to access the containers externally is to associate the container to the host network.
- This takes out any network isolation b/w the docker host and the docker container. Meaning if we were to run a web server on port 5000 in a web container it is automatically as accessible on the same port externally without requiring any port mapping as the web container uses the host network.
- This would also mean that unlike before, we will now not be able to run multiple web containers on the same host on the same port as the ports are now common to all containers in the host network.

## None Network

- With the None Networks, the containers are not a test to any network and doesn't have any access to external network or other containers. They run in an isolated network

## User-defined networks

- So, we just saw that default bridge network with the network ID `172.17.0.1`. So, all containers asssociated to this default network will be able to communicate with each other. But what if we wish to isolate the containers within the docker host.
- For example, the first 2 web containers on interal network 172 and second container on a different internal network let's assume to be 182.
- By default docker only creates one internalbridge network.
- We can create our own internal network using the command `docker network create --driver driver_name --subnet subnet_IP --gateway gateway_IP custom-isolated-network-name`.
- Run the `docker network ls` command to list all networks.

## Inspect Network

- So, hwo do we see the Network settings and IP Address assigned to an exsisting container. Use the `docker inspect <container-name_or_container_ID>` command to find the network settings.

## Embedded DNS

- Container can reach each other using there names.
- For example, lets assume a case where we have Web Server and MySQL Database container running on the same note.
- How can I get my web server to access the database on the database container.

  - One thing we can do is use the Internal IP assigned to mysql container whcih in this case assume to be 172.70.0.3 because that is not very ideal and not guaranted that the container will get the same IP when the system reboots right away.

  - The right way to do it is to use the container name, All containers in a docker host can resolve each other with the name of the docker container.

  - Docker has a built-in DNS Server, that helps the container to resolve ech other using the container name.

  - Note that the built-in DNS Server always runs at address `127.0.0.11`.

- Docker uses network namespaces that creates a seprate namespace for each container. It then uses virtual ethernet pairs to connect containers together.

# Container Orchestration

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/Docker6.png)

- So, far we have lernt how we can run single instance of an appication with a simple `docker run` command. In this case to run a node.js based appliction, we run `docker run nodejs` command.

- But that's just one instance of our application on one docker host. What happens when the number of users increase and that instance is no longer able to handle the load. We deploy additional instance of our application by running the `docker run` command multiple times. That's something we need to do manually.

- We have to keep a close watch on the load and performance of our application and deploy additional instances by our own. And not just that we have to keep a close watch on the help of these applications. And if a container was to fail, we should be able to detect that and run `docker run` command again to deploy another instance of that application.

- What about the health of Docker Host itself. What if the host crashes and is inaccessible. The Containers hosted on that host become inaccessible too. So, what to do to resolve such issues. We will need a dedicated engineer who can sit and monitor the  state, performance and health of containers and take nessecary actions to remidate the situation.

- But when we have large applications deployed with tens and thousands of containers that's not a practical approach. So, you can build your own scripts and that will help you tackle these issues to some extent.

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/Docker8.png)

- `Container Orchestration` is just the solution for this problem. It is a solution that consists of set of tools and scripts that can help host containers in a  production environment.Typically a Container Orchestration soution consists of multiple dokcer hosts that can host containers.

- That way even if one fails, the application is still accessible through the others. A Container Orchestration solution easily allows you to deploy hundreds or thousands of instances of your application with single command. This is the command used for Docker Swarm. Some of these orchestration solution can help you automatically scale up the number of instances when users increase and scale down the number of instances when the demand decreases.

- Some solutions can even help you in automatically adding additional hosts to support the user load and not just clustering and scaling.
- The Container Orchestration solutions also provide support for advanced networking b/w containers across different hosts as well as Load Balancing user requests across different host.
- They also provide support for sharing storage b/w host as well as support for configuration management and security within the cluster.
- There are multiple container orchestration solutions available today. <img src="https://img.shields.io/badge/Docker-2496ED?style=plastic&logo=Docker&logoColor=white"> has <img src="https://img.shields.io/badge/Docker_Swarm-2496ED?style=plastic&logo=Docker&logoColor=white">, <img src="https://img.shields.io/badge/Kubernetes-326CE5?style=plastic&logo=Kubernetes&logoColor=white"> from <img src="https://img.shields.io/badge/Google-4285F4?style=plastic&logo=Google&logoColor=white">, <img src="https://img.shields.io/badge/MESOS-7A1FA2?style=plastic&logo=Gmail&logoColor=white"> from <img src="https://img.shields.io/badge/Apcahe-D22128?style=plastic&logo=Apache&logoColor=white">.
- While Docker Swarm is really easy to setup and get started. It lacks some of the advanced auto-scaling features required for complex production great applications. MESOS on the other hand is quite difficult to setup and get started but supports many advanced features. Kubernetes arguably the most popular of it is a bit difficult to setup and get started but provides a lot of ooptions, to customize deployments and has support for many different vendors, credit that it is now supported on all public cloud service providers like GCP, Azure and AWS.
- The Kubernetes project is one of the top most ranked projects on the github.

# Docker Swarm

</strong>
</p>