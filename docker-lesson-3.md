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

### Layered Architecture

- When docker builds the images, it builds these in alayered architecture.
- Each line of instruction creates a new layer in the Dcoker Image with just the changes from previous layer.
- For example, in the case discussed previously:
  - Layer 1. Base Ubuntu Layer
  - Layer 2. Changes in apt packages
  - Layer 3. Changes in pip packages
  - Layer 4. Source Code
  - Layer 5. Update Entrypoint with "flask" command
- Since each layer only stores the changes from the previous layer, it is reflected in the size as well.
  - We can view this by using `docker history usernmae/app_name` command.
- When we run the `docker build` command, we can see the various steps involved and the result of each task.
- All the layers built are cast, so the layer architecture helps you restart docker build from that partiucular step in case it fails.
- Or if we were to add new steps in build process, you wouldn't have to stgart all over again.
- All the layers built are cached by docker.
- So, in case a particular step was to fail and we were to fix the issue and rerun `docker build` command, it will reuse the previous layers from cache and continue to build the remaining layers.
- The same is true if we were to add additional steps in docker file.
- This way rebuilding the image is faster and you don't have to wait for docker to rebuild the entire image each time.
- This is helpful specially when you update source code of your application as it may change more frequently.
- Only the layers above the updated layers need to be rebuilt.

## Environment Variables

- Let's have a look at this example:

  ```python
  
  import OS
  flask import Flask

  app = Flask(_ name_)

  ..
  ..

  color = "red"
  @app. route ("/")
  def main():
    print (color)
    return render_template("hello.html', color=color)
  
  if __name__ == "__main__":
    app.run(host="0.0.0.0", port="8080")

  ```

- If you look closely at this application, we will see a line that sets background color to red.
- But however, if we decide to change the application color in future, we will have to change the application code.
- It is the best practice to move such information out of the application code and into an environment variable called `APP_COLOR` as follows:

  ```python
  
  import OS
  flask import Flask

  app = Flask(_ name_)

  ..
  ..

  color = os.environ.get('APP_COLOR')

  @app. route ("/")
  def main():
    print (color)
    return render_template("hello.html', color=color)
  
  if __name__ == "__main__":
    app.run(host="0.0.0.0", port="8080")

  ```

- The next time we run the application set an environment variable called `APP_COLOR` to a desired value and the application will now have the new colour specified. This can be done as follows:
  
  ```bash
  export APP_COLOR=blue; python app.py
  ```
  
- Once the application gets packaged into a docker image, we can set the color using `-e` option of docker run command that enables us to set the environment variable within the container to deploy multiple containers with different environment variable inputs. For our given example this would be:

  ```docker
    docker run -e APP_COLOR=blue app_image_name
  ```

- We can use docker run command multiple times and set a different value for environement variable each time.

### `docker inspect <container_name or container_ID>` - Inspect Environment Variable

- So, how do we find the environment variable set on a container that's already running.
- Use the `docker inspect` command to inspect the properties of the running container under the config section we wil find the list of environment variables set on the container.

## CMD vs ENTRYPOINT

- Let's assume a simple scenario: Say, we were to run a docker container from an Ubuntu Image.
- When we run the `docker run ubuntu` command, it runs an instance of ubuntu image and exits immediately.
- If we were to list the running conytainers using `docker ps` command, we would see nothing.
- If we see all the list of containers including the stopped ones, we will find the new container in an exited state.
- Unlike VMs, Containers are not meant to host an OS. Containers are meant to run a specific task or process such as to host an instance of web server or application server or a database or simply to carry out some kind of computation or analysis.
- Once the task is complete, the container exits.
- A Container only lives as long as the process inside it is alive.
- If the web-service inside the container is stopped or crashes the container exits.
- So, who defines what process is run within the container.
- So, if we see the docker file for popular docker images like NGINX we will see an instruction called `CMD` which stands for `COMMAND` that defines the program that will be run within the container when it starts. For the `NGINX`, it is the `NGINX` command. For mysql, it is the `mysqld` command.
- What we tried to do previously is to run a conmtainer with plain ubuntu operating system.
- So, if you look at the docker file for the ubuntu, we will see it uses `bash` as the default command.
- Now bash is not really a process like a web-server  or database server. It is a shell that listens for inputs from a terminal, if it cannot find a terminal it exits.
- When we ran the ubuntu container earlier, docker created a container from the Ubuntu Image and launched the bash program. By default, docker does not attach a terminal to a container when it is run. And so the bash program doesnot find the terminal and so it exits. Since, the process that was started when the container was created finished the container exits as well.

- So, how do we specify a diffferent command when we start a container.

  - One option is to append a command to the docker run command and that way it overrides the default command specified within the image.
    - In case, we exectue `docker run ubuntu` command with the `sleep 5` command added as option as follows:

      ```bash
        docker run ubuntu sleep 5
      ```

    - This way when the container starts it runs the sleep program waits for 5 seconds for execution to complete and than exits. But how do we make that change permanent. Say if we want to run the sleep command when it starts.
    - In that case we will create a our own Image from the base Ubuntu Image and specify in new command. For our example, it will appear something like:

      ```dockerfile
        FROM Ubuntu

        CMD sleep 5
      ```

    - There are different ways of specifying the command either the command simply is in a shell form or in a JSON array format as shown below.
      - CMD command param1
      - CMD ["command","param1"]
    - But remember when we specify in JSON array format. The first element in the array should be the executable. In our case the `sleep` program.
    - Donot specify the command and parameters together when using JSON format. The command and it's parameters should be separate elements in the list.
    - So, now we can build our image using the `dodcker build` command and name it. For our example, it looks something like:

      ```bash
        docker build -t image_name .
      ```

    - But what if we want use to decide the number of seconds it sleeps everytime we exxecute `docker run` command.
    - One option is to run `docker run` command with new command appended to it.
    - The other way is using `ENTRYPOINT` command, using this  we only will pass the number of seconds the container needs to sleep  and it will be invoked automatically.

      ```dockerfile
        FROM Ubuntu

        ENTRYPOINT ["sleep"]
      ```

    - The `ENTRYPOINT` instruction is like the command instruction as in we can specify the program that will be run when the container starts and whatever we specify on the command line  as input will get appended to the entrypoint.
    - In case of the CMD instruction, the command line parameters passed will get replaced entirely whereas in case of ENTRYPOINT the command line parameters will get appended.
    - But always remember that if we don't provide an input, it gives an error of missing operand.
    - So, we can use CMD instruction back again to set a default value for the missing opernad in case not provided by the user, but should be strictly in JSON format. For our example, it appears something like:

      ```dockerfile
        FROM Ubuntu

        ENTRYPOINT ["sleep"]
        CMD ["5"]
      ```

    - So,  in the above example if we don't provide any input the container will sleep for 5 seconds automatically by it's own.
    - And if we specify any input, it will override the CMD instruction.
    - But what if we want to complete override our `ENTRYPOINT` instruction that we specifed before. We can do it using `--entrypoint` option in `docker run` command.

</strong>
</p>
