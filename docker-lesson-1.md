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
<br>

- In case of Docker we have the Underlying hardware infrastructure and than OS and than Docker installed on the OS. Docker than manages the container with dependecies and libraries along. In case of Virtual Machines, we have the hypervisor on the Hardware and then VMs on them where each VM has it's own OS Application, Libraries and Dependencies. In short docker has common OS/Kernel for all the containers while VMs have different OS for each VM. 
- The Overhead causes higher utilization of Underlying resources as there are multiple OS and Kernel running. 
- The VMs also consume higher disk space as each VM is heavy and usually in GBs in size whereas Docker containers are light weight and are usually in MBs. This allows docker containers to boot up faster than VMs as they need to boot-up the whole OS. 
- It's also important to note that docker has less isolation as more resources are shared b/w the contaienrs like the kernel, whereas VMs have complete isolation from others as they don't rely on underlying OS or Kernel. We can run differnt type of OSes such as Windows or Linux. 

## Containers and Virtual Machines

- Now when we have large environment with 1000s of applications , container running thousands of docker hosts, we will often see containers provisoned on virtual machine docker hosts. That way we can utilize the advantages of both technologies we can use the benefits of virtualization to easily provison or decommision docker hosts as required. 
- At the same time making use of the benefits of docker to easily provison applications and quickly scale them as required. But remember in this case we will not be provisioning that many VMs as we used to before because earlier we provisioned a VM for each application 
  
## How is it done??

- There are lots of containerizedd versions of applications readily available, as of today. So, most organiztions have there product containerized and avaialable in a public docker repository called `docker hub` or `docker store`. For example you can find images of most commom OS, databases and other services and tools. 

## Containers vs Images

- An Image is a package or a templatejust like a VM Template that you might have worked with in the virtualization world. It is used to create one or more containers. While containers are running isntances of images that are isolated and have there own environments and set of processes. 

<hr>

- Traditionally developers developed applications. Than they hand it ovet to OPs team to deploy and mange it in prodcution environments . They do that by providing a set of instructions such as information about how the host must be setup, what prereuisites are to be installed on the host and how the dependencies are configured etc..
- Since, the OPs team didnot really develop the application on their own they struggle  with setting it up when they hit an isssue they work with the developers to resolve it.
- With Docker, the developers and operations team work hand-in-hand to transform the guide into a docker file with both of thei requirements. This docker file is then used to create an image of their applications. This image can now run on any host with docker installed on it and is guaranted to run the smae way everywhere.
- So, the OPs team can now simply use the image to deploy the application since the image was already working when the developer built it and operations have not modified it. It continues to work the same way when deployed in production.

</strong>
</p>
