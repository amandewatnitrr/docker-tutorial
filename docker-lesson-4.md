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

- As we can see this sample application is built with a combination of different services, different development tools and multiple different development platforms  such as python, node.js, .NET etc.. 
- This sample application will be used to showcase how easy it is to setup an entire application stack consisting of diverse components in docker.
- Let's for now keep aside `docker swarm` and stacks for a minute and see how can  we put togther this application stack on a single docker engine using first docker run commands and than docker compose.


- Let's assume all images of applications are already built and are available on docker repository.
- We first start with data-layer. First we run the `docker run -d --name=redis redis` command to start an instance of redis in detached mode.
- Next, we deploy the postgresql database by running the command `docker run -d --name=db postgres`. This time as well we add the option to run this container in background and name it db.
- Next, we deploy a voting app for frontend interface by running an instance of voting app image. We run the `docker run -d --name=vote -p 5000:80 voting-app`. Since this is a web-server, it has a web UI Instance running on PORT 80. We will publish that port to 5000 on the host system so that we can access it form the browser.
- Next, we will deploy the results web application that shows the results to the user using `docker run -d --name=result -p 5001:80 result-app`.
- For this, we deploy a container using results-app image and publish port 80 to 5001 on the host. This way we can acces the Web UI of the resulting app on a browser.
- Finally we deploy the worker by running an instance of the worker image using `docker run -d --name=worker worker`.
- After, this we can see that all the instances are running on the host. But there is still one problem that we need to fix up. The Problem is that we have successfully run all the different conatiners, but we actually haven't linked them together. As, in we haven't told the voting web-application to use this particular redis instance. There could be a multiple redis instances running. We haven't told the worker and the resulting app to use this particular PostgreSQL database that we ran.

- So, how do we resolve sucha big issue ?
  - That is exactly where we use the `--link` option. Link is a command line option whcih can be used to link 2 containers together.
  - For example, in our case the voting app web-service is dependant on the redis service, when the web server starts. It looks for redis service running on host redis , but the voting-app container cannot resolve a host by the name redis. To make the voting-app aware of the redis service, we add a link option while running the voting app container to link it to the redis container by adding a `--link` option 

</strong>
</p>