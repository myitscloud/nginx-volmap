NGINX WEB SERVER - Docker Container

Overview:
This project is a learning and training project.
At completion of these steps you will have created a Nginx container, on a custom network, that includes local host to container volume mapping for web content to be servered from the Nginx containe with a simple customized Index.html file.

Prerequisites not included in this project:
Docker must be installed and the installation steps for Docker is not included in this project.
There are many resources such as YouTube videos, Udemy classes, AI Chat that you can get all the required information to perform the setup of Docker.

Some options for Docker installation are:
Windows direct Docker Desktop installation
Linux direct Docker Installation
MacOS direct Docker Installation
Virtual Machines: VMware,VirtualBox, Hyper-V, Orbstack
Virtual Private Server (VPS)
Cloud Providers

For this project "My Technician PC" is: Apple Macbook with M4, but I have used Windows 10/11 with Docker Desktop and Hyper-V Ubuntu images and they work very well also for this project.


!Important Commands!
List Docker running containers:
    docker ps
List stopped containers:
    docker ps -a
List all images:
    docker images
Stop running docker container:
docker stop <container name or id>
Delete a container:
    docker rm <container name or id>
Delete an image:
    docker rmi <image name or id>

Project Directory Structure

Project Top Level Root Folder
nginx
    dockerfile (File)
    docker-compose.yml (File blank at this time)
    init.sh (File blank at this time)
    html (Folder)
        index.html (File)

Note: For now, only be concerned of having: dockerfile, html folder and index.html
We will add the other files as we progress through next steps.
The index.html file is included in the project file for this project.
View dockerfile on how we are copying the file into the docker nginx container, image and port exposure.

LET's Begin:
Docker Hub Nginx Image Pull
Note: It is best practice to do a docker pull command with a specific tag version. "Latest" version is what docker pull nginx command does, but "Latest" is a transitory term and the "Letest" tag becomes unfriendly when it is used in docker pull command 6 months or 2 years later, etc... It makes it unfriendly to know the version what was used whereas a tag of the exact image is known just by reading the name.
A better command would be: docker pull nginx:alpine3.22
Command:
docker pull nginx
or better
docker pull nginx:alpine3.22

Docker Network Creation to run Nginx web server on isolated network
Note: (Skip this option if you like, but you will need to adjust some of the commands used and/or edit some of the files if you do not want to include this option)
command:
docker network create nginx-webserver

Test Checkpoint
Note: A checkpoint is a quick stopping point to run a test on what has already been performed.
Verify Network Creation. 
# After running the below command you should see your new network in list.
Command:
docker network ls
# After running the below command you should see the nginx image you did a docker pull command on
command:
docker images

Go to project root folder
Assuming:
nginx
Go to html folder and verify index.html file is in the folder

Create your dockerfile
dockerfile contents is what is in-betwwen the top and bottom dash marks, nothing else.
-----
FROM nginx:alpine3.22

RUN apk update && \
    apk add --no-cache \
        curl \ 
        nano \
        wget

COPY ./html/index.html /tmp/html/
RUN cp -r /tmp/html/* /usr/share/nginx/html/ 


EXPOSE 80
-----
save the file name as dockerfile in the top root project directory

In the terminal test running the image with this command:

docker run -d --name nginx-websrv01 --network nginx-webserver -p 8080:80 -v ./html:/usr/share/nginx/html nginx:alpine3.22

docker run = docker execute the following for me
-d = detatch the process from my terminal and run it in separate process
--name nginx-websrv01 = name the image specificallyl for me and do not generate a random name for image
--network nginx-webserver = use this network to run the nginx-websrv01 image
-p 8080:80 = on the host machine use port 8080, on the nginx-websrv01 container use port 80
-v ./html:/usr/share/nginx/html  = map the folder html on the host machine to folder location /usr/share/nginx/html on the container
nginx:alpine3.22 = use this specific image to run the container

Open a web browser: Safari, Chrome, Edge, Firefox, etc...
Go to: http://localhost:8080
Do you see the Nginx web page?

Test Checkpoint
Note: Did you make it to this point and the image runs and you can view index.html on: http://localhost:8080
We need to get to this point.
If you are not able to see index.html page I will suggest you carefully go back through the steps, instructions, and also carefully review the file structure and check files for mistakes.

Continued Progression on project
If you have run the full docker command:
docker run -d --name nginx-websrv01 --network nginx-webserver -p 8080:80 -v ./html:/usr/share/nginx/html nginx:alpine3.22

and the command runs without erros and you can view the index.html web page at: http://localhost:8080
then we are now ready to build and save our first nginx custom image.

Creating a new Docker Image

Next, we will now "Automate the process more fully, build an image, save image, create docker-compose.yml file and add a few more customizations.

Creating the docker-compose.yml
From Windows, MAC, or Linux create a file called: docker-compose.yml
