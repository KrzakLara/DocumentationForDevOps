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
| Step                                                       | Command                                                                   | Description                                                                           |
|------------------------------------------------------------|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------|
| 2. Pull latest Nginx image                                 | `sudo docker pull nginx`                                                  | Pulled the latest Nginx container image from Docker Hub                               |
| 3. Find latest Nginx image with Podman                     | `podman search nginx --limit 1`                                          | Find the latest Nginx container image from Docker Hub using Podman                    |
| 4. List running containers with Podman                     | `podman ps`                                                               | List all currently running containers on the system using Podman                       |
| 5. Pull the latest image found with Podman search          | `podman pull [last_image_found_in_search_command]`                        | Pull the latest Nginx container image found in the previous Podman search command     |
| 6. Inspect the image found with Podman search              | `podman image inspect [last_image_found_in_search_command]`                | Find more information about the Nginx container image found with the Podman search    |

          
                                       |

Dodatne komande:
1. Provjera jel Docker runna: <b> sudo docker run hello-world </b>
2. 




UUD-Lab3 Commands
| Concept                   | Description                                                                | Commands and Steps                                                                                                           |
|---------------------------|----------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
| PowerShell                | Program used by Microsoft for Windows task automation                      | -                                                                                                                           |
| Podman                    | Open-source container management tool, alternative to Docker               | -                                                                                                                           |
| Podman Setup              | Instructions for installing and checking Podman                            | `1. sudo apt update && sudo apt upgrade -y`<br>`2. sudo apt-get install -y podman`<br>`3. podman --version`<br>`4. podman ps` |
| Nginx                     | A popular web server for hosting web pages                                 | -                                                                                                                           |
| Docker Hub                | An online library for storing and sharing containers                       | -                                                                                                                           |
| Searching for Nginx       | Using Podman to search for the latest Nginx container image                | `podman search nginx`                                                                                                       |
| Running Nginx Instance    | Running a new Nginx instance, mapping port 80                              | `podman run --name my-nginx -p 80:80 -d nginx`                                                                              |
| Container Management      | Listing all currently running containers. If stopped containers are to be seen, the -a option can be added | `podman ps -a`                                                      |
| Pulling Nginx Image       | Pulling the latest version of the Nginx image from the default registry    | `podman pull nginx`                                                                                                         |
| Running Nginx Container   | Starting a new container named my-nginx-container, mapping port 8080 on the host to port 80 in the container | `podman run -d --name my-nginx-container -p 8080:80 nginx`           |
| Inspecting Nginx Image    | Getting detailed information about a specific container image locally or in the registry | `podman image inspect nginx`<br>`podman image inspect nginx:latest` |
| Container Creation        | Creating a container instance from an image in the background and mapping it to the host's port 80 | `podman run -d --name web8080 -p 8080:80 nginx`                     |
| Stopping Containers       | Command to stop a container by its ID or name                              | `podman stop my-web-container`<br>`podman stop <container_id>`      |
| Removing Containers       | Removing a container if creating another with the same name               | `podman stop web8080`<br>`podman rm web8080`                                                                                |
| Exporting Container Image | Exporting the Nginx container image using Podman                           | `podman image save nginx > nginx.tar`<br>`podman export my-nginx-container > my-nginx-container.tar`                        |
| Modifying Containers      | Commands to modify a container and save the changes as a new image        | `1. podman run --name my-nginx-container -d nginx`<br>`2. podman exec -it my-nginx-container /bin/bash`<br>`3. podman commit my-nginx-container my-custom-nginx` |
| Changing Default Web Page | Changing the content of the default web page within a container without stopping its execution | `podman cp index.html web8080:/usr/share/nginx/html/index.html`     |


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
