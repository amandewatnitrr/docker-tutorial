<p align="justify">
<strong>

# Docker Compose

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/Docker3.png)

- Going forward, we will be working with configurations in YAMl file, so it is important that  we should be comfortable with YAML.
- Initially we learnt to run a docker container using docker run command if we needed to setup a complex application running multiple services, a better way to do it is to use `docker compose`.
- We could create a configurtaion file in YAML format called `docker-compose.yaml` and put together the different services and the options specific to this, to running them in this file.
- Then we could run a `docker compose up` command to bring up the entire application stack.
- This is easier to implement, run and maintain as all changes are always stored in the docker compose configuration file.
- However, this is all only applicable to running containers on a single docker host.

- Let's take a simple general example, to understand what's happening.
- Let's assume a simple yet comprehensive application developed by docker to demonstrate various features available in running and application stack on docker.
- So, let's first get familiarized with the application because we will be working with the same application in different sections through out the course. We assume a sample voting application which provides an interface for user to vote  and another interface to show the results. The application consists of various components such as the voting app, which is web application developed in python to provide the user with an interface 2 choose b/w two options `Option A` and `Option B`.
- When we make a selection the vote is stored in redis, redis here serves as a database in memory.
- This vote is than processed by the worker which is an application written in `.NET`.
- The worker application takes the vote and updates the persistent database which is a PostgreSQL. PostgreSQL simply has table with number of votes for each category ` Option A` and `Option B`. In this case it increments the number of votes depending on which ever option you go for.
- Finally, the result of the vote is displayed in a web-interface , which is another web-application developed in `node.js`. this resulting application reads the count of votes from the PostgreSQL database and displays it to the user.
- So, this is the architecture and data-flow of this sample voting application stack.
<br>
  
> `Voting App` <img  alt="python" src="https://img.shields.io/badge/Python-3776AB?style=plastic&logo=python&logoColor=FFD733" /> -> 
`In-Memory DB` <img  alt="redis" src="https://img.shields.io/badge/Redis-DC382D?style=plastic&logo=redis&logoColor=white" /> ->
`Worker` <img  alt=".NET" src="https://img.shields.io/badge/.NET-512BD4?style=plastic&logo=NET&logoColor=white" /> ->
`Database` <img  alt="PostgreSQL" src="https://img.shields.io/badge/PostgreSQL-4169E1?style=plastic&logo=PostgreSQL&logoColor=white" /> ->
`Result-app` <img alt="Node.js"  src="https://img.shields.io/badge/node.js-339933?style=plastic&logo=node.js&logoColor=white" />


</strong>
</p>