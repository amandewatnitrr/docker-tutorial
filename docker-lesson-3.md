<p align="justify">
<strong>

# Docker Images

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/docker-in.png)

## `Why Containerizing ?` - Why there's a need to create a docker image ?

- It could either be because we cannot find a component or a service that we want to use as a part of our application on Docker Hub already, or you or your team decided that the application you are developing will be dockerised for ease of shipping and deployment.

## How to create my own image ?

- Let's suppose a case where we are going to containerise an application, a simple web application that we have builtusing Python Flask Framework. For this, we need to first understand what we are containerising or what application we are creating the image for and how the application is built.
- So, we start by thinking about what we might do if we want to deploy the application manually.
- Write down the steps required in the right order and creating an image for simple web application.
  - If we were to set it up manually, we would start with an OS let's assume it to be Ubuntu now, then update the source repositories using apt command, then install dpendencies using apt, then install python dependencies using pip command, then copy over the source of the app to a location like OPT and then finally run the web server using flask command.
  
  - Summary of the steps:
    <ol>

    <li>OS - Ubuntu</li>
    <li>Update apt repo</li>
    <li>Install Dependencies using apt</li>
    <li>Install python dependencies using pip</li>
    <li>Copy source code to /opt folder</li>
    <li>Run the web server using "flask" command</li>

    </ol>

- Here's a quick overview of process of creating your own image.
  - Create a dockerfile named `Dockerfile` and write down the instructions for setting up your application in it, such as installing dependencies where to copy the source code from and to, what the entry point of application is etc...

    - ```Dockerfile
        FROM Ubuntu
        RUN apt-get update && apt-get -y install python
        RUN pip install flask flask-mysql
        COPY . /opt/sour_code
        ENTRYPOINT FLASK_APP=/opt/source_code/app.py flask run
      ```

    - Let's have a look on the above docker file.
    - Dockerfile is a text file written in a specific format that docker can understand.
    - It's in an instruction and argument format.
    - In the above example, in this docker file everything on the leftmost in caps is an instruction in this case, `FROM`, `RUN`, `COPY`, `ENTRYPOINT` are all instructions.
    - Each of these instruct docker to perform a specific action while creating the image.
    - Everything on the right to those instructions is an argument.
    - The first line from Ubuntu defines what the base OS should be for this contaioner.
    - Every docker image must be based off of another image, either an OS or another image that was created before based on an OS.
    - It's important to note that all docker files must start with a `FROM` instruction.
    - The `RUN` instruction instructs docker to run a particular command on those base images.
    - So, at this point docker runs the apt-get update command to fetch the updated packages and installs required dependencies on the image. 
    - Then `COPY` instruction copies file from the local system onto the docker image.
    - In this case, the source code of our  application is in the current folder and we will be coying it over to the location /opt.source_code inside the docker image.
    - `ENTRYPOINT` allows us to specify a command that will be run when the image is run as a container.

  - Once done, build your image using the `docker build` command and specify the docker file as input as well as a tag name for the image. This will create an image locally on your system.
    - `docker  build . -f Dockerfile -t username/app_name`
  - To make it available on the `Public Docker Hub Registry`, run the docker push command and specify the name of the image you just created.
    - `docker push username/app_name`

</strong>
</p>
