# Getting started with Docker

<p align="justify">
<strong>

- Let's first understand the need for Docker. Let's say you are working on a project where we have different set of tech stack we are working on, let's suppose them as follows:
  - `Web Server` : `node.js` & `express.js`
  - `Database` : `MongoDB`
  - `Messanging` : `Redis`
  - `Orchestration` : `Ansible`
- It's difficult to build this application with all these differnet componetns.
- Mainly, there compatibility with the underlying OS is a big issue. We have to ensure that all these services are com[paitble with the OS version we are working on or planning to use.
- Than we have to check the compatibility of the versions of libraries an dependencies on the OS. Then we have to check compatibility pf services with the library. There may be cases when one service requires a specific version of dependent library, whereas another service requires another version of the same library.
- The architecture of the application would need to be changed over time. As they upgrade to the newer versions of these components or change the database etc.. and every time something changes the developer needs to go throught the process of compatibility check b/w these various components and the underlying infrastructure. 
- This compatibility metrics issue is usually reffered to as `The Matrix from Hell!!`.
- Every time a new developer is on-boarded to the team to work on project, it's difficult to setup a new environment. As they have to follow a large set of instructions and hundreds of commands to finally setup there environemnt.
- In Docker, we are able to run each component in seprate containers with it's own dependecies and libraries all on the same VM and the OS but within seprate environments or containers we jsut had to build the docker configuration once and all our developers could now get started with a simple docker run command, irrespective of what underlying operating system they run.

## What are Containers ?

- Containers are completely isolated environements.
- As in they can have there own processes or services their own network interfaces their own mounts just like VMs, except the fact that they share same OS Kernel.
- The Concept of Docker is not very new and has exsisted for a decade now(from 2022) and some of the tthe different type of containers are `LXC`, `LXD` and `LXCFS` etc.
- Docker utilizes LXC containers setting up these container environements. Setting up these container env is hard as they are very low level and that is where docker offers high level tool with several powerful functionalities making it really easy for end users like us to understand how docker works.
- If we look at operating systems like Ubuntu, Fedora, Suse or Centos - they all consist of 2 things - an OS Kernel and a set of software.
- The OS Kernel is responsible for interacting with the underlying hardware while the OS Kernel remains the smae which is Linux in this case. It's the software above it that makes the Operating System different. This software may consist of different user interface drivers, compilers, file managers, dveloper tools etc.
- So, we have a common kernel shared across all the OS and some custom software that differentiate OS from each other.
- Sharing the Kenel:
  - Let's say we have a system with Ubuntu OS with Docker installed on it. Docker can any OS on top of it as long as they are all based on the same kernel, in this case linux. If the underlying OS is Ubuntu Docker can run a container based on another distribution like Debian, fedora, suse or centos. Each docker container has this one specific diffrentiated software that makjes these operatring system different and docker utilizes the underlying kernel of the docker host which work with all the OSes above.
- So, what is an OS that donot share the smae kernel as these `Windows` !!
  - And so we won't be able to run a Windows based container on a docker host with linux  on it for that, you will require a docker on a windows server. 
  - So, when we install docker on windows and run a Linux based Container on windows we aren't really running a linux container on windows. Windows runs a linux container on windows Virtual Machine under the hood, so it's really Linux Container on Linux Viirtual Machine on windows.
- Unlike Hypervisor, Docker is not meant to virtualize and run different OS and kernels on same hardware.
- The main purpose of docker is to package and containerized applications and ship them and run them anywhere any times as many times as we want to.

## Containers vs Virtual Machines

![](https://github.com/amandewatnitrr/docker-tutorial/blob/master/imgs/containers-vs-virtual-machines.jpg)
- 

</strong>
</p>
