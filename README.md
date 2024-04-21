# DocumentationForDevOps

UUD-Lab1 Commands
| Step | Command                                                           | Description                                     |
|------|-------------------------------------------------------------------|-------------------------------------------------|
| 1    | `sudo yum update -y`                                              | Update the system packages                      |
| 2    | `sudo yum install httpd -y`                                       | Install the Apache HTTP Server (httpd)          |
| 3    | `sudo systemctl start httpd`                                      | Start the httpd service                         |
| 4    | `sudo systemctl enable httpd`                                     | Enable httpd to start at boot                   |
| 5    | `sudo yum install -y yum-utils device-mapper-persistent-data lvm2`| Install the necessary packages                  |
| 6    | `sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo` | Add the Docker repository |
| 7    | `sudo yum install -y docker-ce docker-ce-cli containerd.io`       | Install Docker Engine                           |
| 8    | `sudo systemctl start docker`                                     | Start Docker                                    |
| 9    | `sudo systemctl enable docker`                                    | Set Docker to automatically start on system boot|
| 10   | `mkdir httpd-container && cd httpd-container`                    | Create a directory for the httpd container      | 
| 11   | `echo "FROM httpd:2.4\nEXPOSE 80" > Dockerfile`                   | Create a Dockerfile for the httpd container <b>(ako se kasnije Docker ne zeli startat, koristi razlomljenu komandu u 11 i 12) </b>    |
| 12   | `mkdir -p httpd-container`                                        | Ensure the httpd-container directory is created only if it doesn't already exist |
| 13   | `cd httpd-container`                                              | Navigate into the httpd-container directory     |
| 14   | `echo "FROM httpd:2.4" > Dockerfile`                              | Create and write to Dockerfile                  |
| 15   | `echo "EXPOSE 80" >> Dockerfile`                                  | Append EXPOSE instruction to Dockerfile         |
| 16   | `sudo docker build -t my-httpd .`                                 | Build the Docker image                          |
| 17   | `sudo docker run -d -p 8080:80 --name my-httpd-instance my-httpd` | Run the container                               |

Dodatne komande, mozda zatrebaju: 
1. Stop the Existing Container:
   <b>sudo docker stop my-httpd-instance </b>
2. To Delete a Stopped Container:
<b> sudo docker rm [container_name_or_ID] </b>



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
| 16. Modify container's web page without stopping          | `podman cp index.html web8080:/usr/share/nginx/html/index.html`                              | Change the contents of the default web page of the `web8080` container by copying a new `index.html` file into it without stopping the container. Verify changes with `podman exec web8080 cat /usr/share/nginx/html/index.html`. |

                                 

Dodatne komande:
1. Provjera jel Docker runna: <b> sudo docker run hello-world </b>
2. kak mi se kontejner zove: <b> podman ps </b>
3. To check if the Docker daemon is running on your system: <b> sudo systemctl status docker </b>




UUD-Lab3 Commands
| Step | Command | Description |
|------|---------|-------------|
| 1. Containerize the Application Using Podman | `git clone https://github.com/docker/getting-started-app`<br>`cd getting-started-app`<br>`podman build -t my-nodejs-app .`<br>`podman run -d --name my-running-app -p 3000:3000 my-nodejs-app` | Clone the repository and navigate to the application directory. Build the container image and run the container, mapping port 3000 to the host. |
| 2. Build Image with Initial Tag | `podman build -t getting-started-app .` | Build the initial Docker image from the Containerfile. |
| 3. Rebuild and Tag Image with 0.0.1 | `podman build -t getting-started-app:0.0.1 .` | Build and tag the image with version 0.0.1. |
| 4. Inspect the Image | `podman image inspect getting-started-app:0.0.1` | Inspect the detailed information of the image tagged as 0.0.1. |
| 5. Modify Application and Rebuild | Modify the application code.<br>`podman build -t getting-started-app:0.0.2 .` | Change the message displayed when there are no to-do items. Rebuild the image with the new modifications and tag it as 0.0.2. |
| 6. Compare Image Versions | `podman diff getting-started-app:0.0.1 getting-started-app:0.0.2` | Show the differences between the two image versions, 0.0.1 and 0.0.2. |
| 7. Push Images to Docker Hub | `podman login --username yourusername docker.io`<br>`podman push getting-started-app:0.0.1 docker.io/yourusername/getting-started-app:0.0.1`<br>`podman push getting-started-app:0.0.2 docker.io/yourusername/getting-started-app:0.0.2` | Log in to Docker Hub and push the images tagged 0.0.1 and 0.0.2 to Docker Hub. |
| 8. Create a UBI8 Based Containerfile and Build | Create the Containerfile.<br>`podman build -t myweb:0.0.1 .` | Define a new Containerfile using UBI8 as the base image, including httpd setup. Build the image and tag it as 0.0.1. |
| 9. Configure Quadlet for System Integration | Follow the quadlet guide to create configuration file. | Configure the system so that the container automatically starts with the system using a quadlet configuration file. |
| 10. Clone and Build Go Application Using Multi-Stage Builds | `git clone https://github.com/jstanesic/example-go-app`<br>`cd example-go-app`<br>`podman build -f Dockerfile1 -t example:Dockerfile1 .`<br>`podman build -f Dockerfile2 -t example:Dockerfile2 .` | Clone the Go application repository and navigate to the project directory. Build the image using Dockerfile1 and Dockerfile2 to see the benefits of multi-stage builds. |
| 11. Compare Built Images and Check Benefits of Multi-Stage | `podman image ls` | List and compare the sizes of the built images to discover the benefits of multi-stage builds, especially in reducing the final image size and isolating build dependencies. |


UUD-Lab4 Commands
| Step | Command/Instruction                                                                                 | Description                                                                                                                |
|------|-----------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------|
| 1    | (log in) na radnu stanicu VM u RH (Red Hat) akademiji                                               | Logging into the VM workstation at the Red Hat academy                                                                     |
| 2    | `podman network ls`                                                                                 | Listing all networks managed by Podman                                                                                     |
| 3    | `podman network inspect podman`                                                                     | Inspecting the default network settings for Podman                                                                         |
| 4    | `podman network create --subnet 10.88.0.0/16 --gateway 10.88.0.1 --opt "dns_enabled=true" labnet`   | Creating a custom network with DNS enabled                                                                                 |
| 5    | `podman run -d --name mysql --network labnet -e MYSQL_RANDOM_ROOT_PASSWORD=yes -e MYSQL_DATABASE=wordpress -e MYSQL_USER=student -e MYSQL_PASSWORD=DB15secure! mysql` | Setting up a MySQL container with specific environment variables                                                           |
| 6    | `podman run -d --name wordpress --network labnet -p 8080:80 -e WORDPRESS_DB_HOST=mysql -e WORDPRESS_DB_USER=student -e WORDPRESS_DB_PASSWORD=DB15secure! -e WORDPRESS_DB_NAME=wordpress wordpress` | Creating a WordPress container linked to the MySQL container                                                               |
| 7    | -                                                                                                   | Explanation that using the default network could work, but a custom network allows for finer network settings adjustments including DNS |
| 8    | Access WordPress through a browser at http://localhost:8080 and follow the setup instructions       | Setting up a WordPress site by accessing the local server                                                                   |
| 9    | First, remove the old container with `podman rm -f mysql`, then create a new one with bind mounts   | Instructions to set up a MySQL container using bind mounts for persistent storage                                          |
| 10   | `podman volume create mysql_volume`<br>`podman run -d --name mysql --network labnet -v mysql_volume:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=mysecret -e MYSQL_DATABASE=wordpress -e MYSQL_USER=student -e MYSQL_PASSWORD=DB15secure! mysql` | Creating a MySQL container with a volume for data persistence                                                              |
| 11   | `sudo dnf install podman-compose` or `sudo apt install podman-compose`                               | Installing podman-compose using the package manager                                                                        |
| 12   | Visit GitHub pages for examples                                                                     | Investigating examples of compose files on GitHub                                                                          |
| 13   | Create a new file named docker-compose.yml and define services for MySQL and WordPress using compose file syntax | Instructions for creating a docker-compose.yml with the necessary service definitions                                      |
| 14   | In the terminal, in the directory where your docker-compose.yml file is                                     In the terminal, in the directory where your                                                                                                                   docker-compose.yml file is located, run podman-compose up -d
