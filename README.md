[Intro_to_DevOps_-_LAB4.pdf](https://github.com/KrzakLara/DocumentationForDevOps/files/15067548/Intro_to_DevOps_-_LAB4.pdf)
[Intro_to_DevOps_-_LAB3.pdf](https://github.com/KrzakLara/DocumentationForDevOps/files/15067547/Intro_to_DevOps_-_LAB3.pdf)
[Intro_to_DevOps_-_LAB2.pdf](https://github.com/KrzakLara/DocumentationForDevOps/files/15067546/Intro_to_DevOps_-_LAB2.pdf)
[Intro_to_DevOps_-_LAB1.pdf](https://github.com/KrzakLara/DocumentationForDevOps/files/15067545/Intro_to_DevOps_-_LAB1.pdf)
# DocumentationForDevOps


UUD-Lab1 Commands
| Step | Command | Description |
|------|---------|-------------|
| 1    | `sudo yum update -y` | Update the system packages. |
| 2    | `sudo yum install httpd -y` | Install the Apache HTTP Server (httpd). |
| 3    | `sudo systemctl start httpd` | Start the httpd service. |
| 4    | `sudo systemctl enable httpd` | Enable httpd to start at boot. |
| 5    | `sudo yum install -y yum-utils device-mapper-persistent-data lvm2` | Install the necessary packages for managing Docker. |
| 6    | `sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo` | Add the Docker repository to the system's repository list. |
| 7    | `sudo yum install -y docker-ce docker-ce-cli containerd.io` | Install Docker Engine, CLI, and containerd. |
| 8    | `sudo systemctl start docker` | Start Docker. |
| 9    | `sudo systemctl enable docker` | Set Docker to automatically start on system boot. |
| 10   | `mkdir httpd-container && cd httpd-container` | Create a directory for the httpd container and navigate into it. |
| 11   | `echo "FROM httpd:2.4\nEXPOSE 80" > Dockerfile` | Create a Dockerfile for the httpd container specifying Apache version 2.4 and expose port 80. |
| 12   | `mkdir -p httpd-container` | Ensure the httpd-container directory is created only if it doesn't already exist. |
| 13   | `cd httpd-container` | Navigate into the httpd-container directory. |
| 14   | `echo "FROM httpd:2.4" > Dockerfile` | Create and write the base image to the Dockerfile. |
| 15   | `echo "EXPOSE 80" >> Dockerfile` | Append the EXPOSE instruction to the Dockerfile to make port 80 available outside the container. |
| 16   | `sudo docker build -t my-httpd .` | Build the Docker image named my-httpd from the Dockerfile in the current directory. |
| 17   | `sudo docker run -d -p 8080:80 --name my-httpd-instance my-httpd` | Run the container from the my-httpd image in detached mode, mapping port 8080 on the host to port 80 in the container, and name the container instance my-httpd-instance. |


Dodatne komande, mozda zatrebaju: 
1. Stop the Existing Container:
   <b>sudo docker stop my-httpd-instance </b>
2. To Delete a Stopped Container:
<b> sudo docker rm [container_name_or_ID] </b>
3. To start a stopwatch:  <b> { time sh -c 'command1; command2; command3' ; } 2> time-output.txt</b>
4. Stop the Stopwatch: <b>cat time-output.txt</b>



UUD-Lab2 Commands
| Step                                                      | Command                                                                                     | Description                                                                                        |
|-----------------------------------------------------------|---------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| 2. Pull latest Nginx image                                | `sudo docker pull nginx`                                                                    | Pulled the latest Nginx container image from Docker Hub                                            |
| 3. Find latest Nginx image with Podman                    | `podman search nginx --limit 1`                                                             | Find the latest Nginx container image from Docker Hub using Podman                                 |
| 4. List running containers with Podman                    | `podman ps`                                                                                 | List all currently running containers on the system using Podman                                   |
| 5. Pull the latest image found with Podman search         | `podman pull [last_image_found_in_search_command]`                                          | Pull the latest Nginx container image found in the previous Podman search command                  |
| 6. Inspect the image found with Podman search             | `podman image inspect [last_image_found_in_search_command]`                                  | Find more information about the Nginx container image found with the Podman search                 |
| 7. Create a container instance and map to host port 80    | `podman run -d -p 80:80 --name my-nginx nginx:latest`                                       | If an error occurs due to port 80 being in use, resolve by selecting a different port or ensuring no other services are using port 80. Use `sudo ss -tuln` to find a free port, stop and remove the existing container with `podman stop my-nginx` and `podman rm my-nginx`, then run the command again with the new port. |
| 8. Create an instance and map to host port 8080           | `podman run -d -p 8080:80 --name web8080 nginx:latest`                                      | If an error occurs (e.g., name already in use), choose a different name or remove the existing container. |
| 9. List running containers                                | `podman ps`                                                                                 | List all currently running containers on the system using Podman                                   |
| 10. Stop the first container                              | `podman stop my-nginx`                                                                      | Stop the first container you created by using `podman stop`.                                       |
| 11. Attempt to create container named web8080             | `podman run -d -p 8081:80 --name web8080 nginx:latest`                                      | Attempt to create another container instance named `web8080` will result in an error due to the name being already in use. To resolve the issue, choose a different name with `podman run -d -p 8082:80 --name web8080-2 nginx:latest` or remove the existing container with `podman stop web8080` followed by `podman rm web8080`, and then rerun the command with `podman run -d -p 8081:80 --name web8080 nginx:latest`. |
| 12. Inspect the details of web8080                        | `podman inspect web8080`                                                                    | Inspect the details of the `web8080` container.                                                    |
| 13. List all downloaded images                            | `podman images`                                                                             | List all the images you have downloaded using the `podman images` command.                         |
| 14. Export the nginx image                                | `podman image save -o nginx.tar nginx:latest`                                                | Export the nginx image to a tar archive with `podman image save`.                                  |
| 14a. Export the nginx container                           | `podman export -o my-nginx.tar my-nginx`                                                     | Export the container's filesystem with `podman export`.                                            |
| 14b. Differences between save and export                  | N/A                                                                                         | `podman image save` saves the image with all its layers and metadata, suitable for image distribution or backup. `podman export` exports the current filesystem of a container.               |
| 15. Save changes to a new image                           | `podman commit modify-nginx custom-nginx-image`                                              | Save changes made to a container to a new image with `podman commit`.                              |
| 16. Modify container's web page without stopping          | `docker cp index.html web8080:/usr/share/nginx/html/`                              | Change the contents of the default web page of the `web8080` container by copying a new `index.html` file into it without stopping the container. Verify changes with `podman exec web8080 cat /usr/share/nginx/html/index.html`. |
                                

Dodatne komande:
1. Provjera jel Docker runna: <b> sudo docker run hello-world </b>
2. kak mi se kontejner zove: <b> podman ps </b>
3. To check if the Docker daemon is running on your system: <b> sudo systemctl status docker </b>






UUD-Lab3 Commands
| Step | Command | Description |
|------|---------|-------------|
| 1. Containerize the Application Using Podman | 1.`cd ..`<br> 2.`git clone https://github.com/docker/getting-started-app.git`<br> 3.`cd getting-started-app`<br> 4.`echo "" > Dockerfile`<br> 5. Open Dockerfile in file explorer and paste:<br> 6. `# syntax=docker/dockerfile:1`<br> 7.`FROM node:18-alpine`<br> 8.`WORKDIR /app`<br> 9.`COPY . .`<br>`RUN yarn install --production`<br>`CMD ["node", "src/index.js"]`<br>`EXPOSE 3000`<br>`podman build -t my-nodejs-app .`<br>`podman run -dp 127.0.0.1:3000:3000 my-nodejs-app`<br>10. Open browser to `localhost:3000` | Start by positioning yourself in the initial directory using the command line. Clone the repository, navigate to the cloned directory, create and edit the Dockerfile with the necessary Node.js setup. Build the container image with Podman and run the container, mapping port 3000 to the host. Access the application by opening a web browser and navigating to `localhost:3000`. |
| 2. Rebuild and Tag Image with 0.0.1 | `podman build -t getting-started-app:0.0.1 .` | Build and tag the image with version 0.0.1. |
| 3. Inspect the Image | `podman image inspect getting-started-app:0.0.1` | Inspect the detailed information of the image tagged as 0.0.1. |
| 5. Modify Application and Rebuild | Modify the application code.<br>`podman build -t getting-started-app:0.0.2 .` | Change the message displayed when there are no to-do items.| 
| 6. Rebuild the image with the new modifications and tag it as 0.0.2: |  `podman build -t getting-started-app:0.0.2 .`| Build this image again, but this time tag it with 0.0.2. |
| 7. Compare Image Versions | `podman diff getting-started-app:0.0.1 getting-started-app:0.0.2` | Show the differences between the two image versions, 0.0.1 and 0.0.2. |
| 8. Push Images to Docker Hub | 1.`podman login --username yourusername docker.io`<br> 2.`podman tag getting-started-app:0.0.1 docker.io/yourusername/getting-started-app:0.0.1`<br> 3.`podman push docker.io/yourusername/getting-started-app:0.0.1` <br> 4.`podman push getting-started-app:0.0.2 docker.io/yourusername/getting-started-app:0.0.2` <br> 5.`podman push docker.io/yourusername/getting-started-app:0.0.2`| Log in to Docker Hub and push the images tagged 0.0.1 and 0.0.2 to Docker Hub. |
| 9. Create a UBI8 Based Containerfile and Build | 1. `cd /path/to/existing-directory` (Navigate to the Desired Directory)<br>2. `touch Containerfile` (Create the Containerfile)<br>3. `nano Containerfile` (Open the Containerfile for Editing, save and exit with ctrl+X)<br>4. Insert into Containerfile:<br>```<br># Use UBI8 as a base image<br>FROM registry.access.redhat.com/ubi8/ubi:latest<br><br># Update installed packages<br>RUN yum update -y && yum clean all<br><br># Install httpd (Apache web server)<br>RUN yum install httpd -y && yum clean all<br><br># Copy a simple webpage to the default directory of Apache<br>COPY index.html /var/www/html/<br><br># Configure httpd to start when the container launches<br>CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"]<br><br># Expose port 80 to access the web server<br>EXPOSE 80<br>``` (Write these commands into the Containerfile)<br>5. `podman build -t myweb:0.0.1 .` (Build the Container Image)<br>6. `podman login --username yourusername docker.io` (Log In to the Public Repository)<br>7. `podman tag myweb:0.0.1 docker.io/yourusername/myweb:0.0.1` (Tag the Image)<br>8. `podman push docker.io/yourusername/myweb:0.0.1` (Push the Image) | Define a new Containerfile using UBI8 as the base image, update packages, install and configure httpd, copy a webpage, expose port 80, build the image, tag it, and push it to Docker Hub. |
| 10. Configure Quadlet for System Integration | 1. `mkdir -p $HOME/.config/containers/systemd/` (Create the Necessary Directories)<br>2. `cd $HOME/.config/containers/systemd/` (Navigate to the Directory)<br>3. `touch web.container` (Create the Quadlet unit file)<br>4. `nano web.container` (Edit the file)<br>5. Add the following content to `web.container`:<br>```ini<br>[Unit]<br>Description=Web container<br>After=network.target<br><br>[Container]<br>Image=myweb:0.0.1<br>ExecStartPre=-/usr/bin/podman rm -f web<br>Exec=/usr/bin/podman run --name web --replace --rm myweb:0.0.1<br><br>[Install]<br>WantedBy=multi-user.target<br>```<br>6. Reload systemd to Recognize the New Unit File:<br>`systemctl --user daemon-reload` or `systemctl --user restart web.service` (Double-Check Command Syntax)<br>7. Enable and Start the Service:<br>`systemctl --user enable web.service`<br>`systemctl --user start web.service`<br>8. Check the Status:<br>`systemctl --user status web.service` (Verify the service is running correctly) | Configure the system so that the container automatically starts with the system using a quadlet configuration file. Each step ensures that the containerized application is properly integrated with systemd, making it start automatically at boot and manageable via standard systemd commands. |
| 11. Clone and Build Go Application Using Multi-Stage Builds | `git clone https://github.com/jstanesic/example-go-app`<br>`cd example-go-app`<br>`podman build -f Dockerfile1 -t example:Dockerfile1 .`<br>`podman build -f Dockerfile2 -t example:Dockerfile2 .`<br> `podman image ls`  | Clone the Go application repository and navigate to the project directory. Build the image using Dockerfile1 and Dockerfile2 to see the benefits of multi-stage builds. |


dodatne komande(vjezbe 3): 
1.`podman ps -a ` (list all containers) i `podman images `



UUD-Lab4 Commands
| Step | Command | Description |
|------|---------|-------------|
| 2. List All Podman Networks | `podman network ls` | List all Podman networks; should only show the default network called "podman" with the bridge driver. |
| 3. Inspect the Default Network | `podman network inspect podman` | Inspect the network configuration of the default Podman network. |
| 4. Create Custom Network with DNS Enabled | `podman network create --subnet 10.88.0.0/16 --gateway 10.88.0.1 --opt "com.docker.network.bridge.name=podman1" --dns-enabled=true labnet` | Create a custom network named "labnet" with DNS enabled. |
| 5. Create MySQL Container | `podman run -d --name mysql --network labnet -e MYSQL_ROOT_PASSWORD=$(openssl rand -base64 32) -e MYSQL_DATABASE=wordpress -e MYSQL_USER=student -e MYSQL_PASSWORD=DB15secure! mysql:latest` | Create a MySQL database container using the official MySQL image. Configure it to randomize the root password, create a "wordpress" database, and a user "student" with a specified password. |
| 6. Create WordPress Container | `podman run -d --name wordpress --network labnet -p 8080:80 -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_USER=student -e WORDPRESS_DB_PASSWORD=DB15secure! -e WORDPRESS_DB_NAME=wordpress wordpress:latest` | Create a WordPress container using the official WordPress image. It should be started in the background, connected to the "labnet" network, and publish port 80 from the container to port 8080 on the host. |
| 8. Create Default Web Page in WordPress | `Step 1: Access Your WordPress Site: http://<VM's-IP-address>:8080`<br>`Step 2: Complete WordPress Installation`<br>`Step 3: Access the WordPress Admin Dashboard, Navigate to Pages, Add content using the WordPress editor, Preview your page, Publish your page.`<br>`Step 5: Go to Settings → Reading, select "A static page", choose the page you created, and click "Save Changes".` | Create a default web page in the WordPress application by accessing the admin dashboard and configuring the homepage. |
| 9. Manage MySQL Container with Bind Mount | `podman stop mysql`<br>`podman rm mysql`<br>`podman run -d --name mysql --network labnet -v /home/student/mysql:/var/lib/mysql:Z -e MYSQL_ROOT_PASSWORD=$(openssl rand -base64 32) -e MYSQL_DATABASE=wordpress -e MYSQL_USER=student -e MYSQL_PASSWORD=DB15secure! mysql:latest` | Delete the old MySQL container, and create a new one which will bind mount the directory `/var/lib/mysql` to `/home/student/mysql`. |
| 10. Recreate MySQL Container with Volume | `podman stop mysql`<br>`podman rm mysql`<br>`podman volume create mysql_data`<br>`podman run -d --name mysql --network labnet -v mysql_data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=$(openssl rand -base64 32) -e MYSQL_DATABASE=wordpress -e MYSQL_USER=student -e MYSQL_PASSWORD=DB15secure! mysql:latest` | Recreate the MySQL container using a volume instead of a bind mount to ensure data persistence. |
| 11. Install Podman Compose | `sudo dnf install -y podman-compose` | Install the podman-compose package to manage multi-container applications with a single command. |
| 13. Create Docker Compose File | `Create a file named docker-compose.yml` | Create a compose file to deploy WordPress and MySQL using podman-compose. |
| 14. Deploy Using Compose File | `podman-compose up -d` | Deploy WordPress and MySQL by using the compose file you created. |

------------------------------------------------------------------------------
Dodatne komande:
1. task 6: contents of a docker file which you have to manually create:
(ljestve) Use the official Ubuntu base image (komentar)
FROM ubuntu:latest
# Install Apache2 (komentar)
RUN apt-get update && apt-get install -y apache2
# Set environment variable STUDENT (komentar)
ENV STUDENT "YourUsername YourSurname"
# Expose port 80 (komentar)
EXPOSE 80
# Set the default command to start Apache2 (komentar)
CMD ["apache2ctl", "-D", "FOREGROUND"]


2. To deploy WordPress and MySQL containers using Podman and ensure that the WordPress container successfully connects to the MySQL database container, you can follow these steps:
   1. Pull the required Docker images:
    <b> podman pull mysql:5.7
   podman pull wordpress:php8.2  </b>
     2. Create a custom network with DNS enabled (if not already created):
    <b> podman network create --dns 8.8.8.8 labnet </b>
   3. Start the MySQL container:
   <b> podman run -d --name mysql --network labnet \
   -e MYSQL_ROOT_PASSWORD=<your_root_password> \
   -e MYSQL_DATABASE=wordpress \
   -e MYSQL_USER=student \
   -e MYSQL_PASSWORD=DB15secure! \
   mysql:5.7
   Replace <your_root_password> with a randomized root password for MySQL.  </b>
   4. Start the WordPress container:
     <b> podman run -d --name wordpress --network labnet \
      -e WORDPRESS_DB_HOST=mysql:3306 \
      -e WORDPRESS_DB_NAME=wordpress \
      -e WORDPRESS_DB_USER=student \
      -e WORDPRESS_DB_PASSWORD=DB15secure! \
      -e WORDPRESS_DB_CHARSET=utf8 \
      -e WORDPRESS_DB_COLLATE=utf8_general_ci \
      -p 8080:80 \
      wordpress:php8.2 </b>
   5. Access WordPress in your browser by navigating to <b> http://localhost:8080 or <your_server_ip>:8080 </b>. Follow the setup       instructions to complete the WordPress installation.
   Ensure that you replace <your_root_password> with a secure password for the MySQL root user.

This setup will create two containers: mysql and wordpress, connected to the labnet network, allowing the WordPress container to communicate with the MySQL container.  
