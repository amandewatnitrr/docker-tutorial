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
- We can create our own internal network using the command `docker network create \ --driver driver_name\ --subnet subnet_IP custom-isolated-network-name`.
- Run the `docker network ls` command to list all networks.

## Inspect Network

- So, hwo do we see the Network settings and IP Address assigned to an exsisting container. Use the `docker inspect <container-name_or_container_ID>` command to find the network settings.

## Embedded DNS

- Container can reach each other using there names.
- For example, let;s assume a case where we have Web Server and MySQL Database container running on the same note.
- How can I get my web server to access the database on the database container.

  - One thing we can do is use the Internal IP assigned to mysql container whcih in this case assume to be 172.70.0.3 because that is not very ideal and not guaranted that the container will get the same IP when the system reboots right away.

  - The right way to do it is to use the container name, All containers in a docker host can resolve each other with the name of the docker container.

  - Docker has a built-in DNS Server, that helps the container to resolve ech other using the container name.

  - Note that the built-in DNS Server always runs at address `127.0.0.11`.

- Docker uses network namespaces that creates a seprate namespace for each container. It then uses virtual ethernet pairs to connect containers together.


</strong>
</p>