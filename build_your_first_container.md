# Build Your First Docker Container

This guide walks through building a simple Docker image that runs an
Apache web server and serves an `index.html` page.

## 1. Prerequisites

Make sure Docker is installed and running on your Linux machine. In this
example, the machine is named `ansible-server` and the shell user is
`docker`.

Check Docker:

``` bash
docker --version
sudo systemctl status docker
```

If your user is not allowed to run Docker commands without `sudo`,
either use `sudo` before Docker commands or add the user to the Docker
group (then log out and back in):

``` bash
sudo usermod -aG docker "$USER"
```

## 2. Create a project directory

Create a directory for the website and Dockerfile, then move into it:

``` bash
mkdir first-container
cd first-container
```

Keep the files used by the build in this directory. Docker uses the
current directory (`.`) as the build context.

## 3. Create the website page

Create an HTML file:

``` bash
vim index.html
```

Add your website HTML, for example:

``` html
<!DOCTYPE html>
<html>
<head>
    <title>My First Docker Container</title>
</head>
<body>
    <h1>Hello from my first Docker container!</h1>
    <p>Apache is running inside Docker.</p>
</body>
</html>
```

Save and exit. You can use your own `index.html` content instead.

## 4. Create the Dockerfile

Create a file named `Dockerfile` (the conventional capitalization is
recommended):

``` bash
vim Dockerfile
```

Add the following instructions:

``` dockerfile
FROM ubuntu:16.04

RUN apt-get update -y
RUN apt-get install -y apache2
RUN chown -R www-data:www-data /var/www/

ENV APACHE_RUN_USER=www-data
ENV APACHE_RUN_GROUP=www-data
ENV APACHE_LOG_DIR=/var/log/apache2
ENV APACHE_LOCK_DIR=/var/lock/apache2
ENV APACHE_PID_FILE=/var/run/apache2.pid

COPY index.html /var/www/html/

EXPOSE 80

ENTRYPOINT ["/usr/sbin/apache2ctl"]
CMD ["-D", "FOREGROUND"]
```

### What each Dockerfile instruction does

  --------------------------------------------------------------------------------
  Instruction                                  Purpose
  -------------------------------------------- -----------------------------------
  `FROM ubuntu:16.04`                          Uses the Ubuntu 16.04 image as the
                                               base image.

  `RUN apt-get update -y`                      Updates the package index while
                                               building the image.

  `RUN apt-get install -y apache2`             Installs the Apache web server.

  `RUN chown -R www-data:www-data /var/www/`   Changes ownership of the web
                                               directory to Apache's `www-data`
                                               user and group.

  `ENV`                                        Sets environment variables used by
                                               Apache.

  `COPY index.html /var/www/html/`             Copies the website page into
                                               Apache's document root.

  `EXPOSE 80`                                  Documents that the container's web
                                               server listens on port 80. It does
                                               not publish the port on its own.

  `ENTRYPOINT`                                 Sets the main executable that runs
                                               when the container starts.

  `CMD`                                        Supplies the default arguments to
                                               Apache, keeping it in the
                                               foreground.
  --------------------------------------------------------------------------------

> **Note:** Ubuntu 16.04 is an old, end-of-life release. It is retained
> here to match the original lab exercise. For a new or production
> project, use a currently supported base image and review its
> compatibility.

## 5. Check your project files

Make sure both files are present:

``` bash
ls -l
```

Expected files:

``` text
Dockerfile
index.html
```

The Dockerfile and `index.html` must be inside the build context
directory.

## 6. Build the Docker image

Run this command from the project directory:

``` bash
docker build -t suman:latest .
```

Explanation:

-   `docker build` builds an image from a Dockerfile.
-   `-t` assigns a name and tag to the image.
-   `suman` is the image name.
-   `latest` is the tag.
-   `.` means the current directory is the build context.

During the build, Docker reads the Dockerfile, downloads the base image
if needed, installs Apache, copies the HTML file, and creates the image.

If the build succeeds, the output includes a line similar to:

``` text
naming to docker.io/library/suman:latest
```

### A common error from the lab

The first attempt used:

``` dockerfile
FROM ubuntu:16:04
```

This is invalid because the tag separator is a dot in this Ubuntu
version, not another colon. Use:

``` dockerfile
FROM ubuntu:16.04
```

The original file also had `Run` instead of `RUN`, and `chmod` where
ownership was intended. Docker instruction names are conventionally
uppercase, and the ownership command should be:

``` dockerfile
RUN chown -R www-data:www-data /var/www/
```

## 7. List the available images

Check that the image was created:

``` bash
docker images
```

You should see an entry named `suman` with the tag `latest`.

## 8. (Optional) Remove an incorrectly tagged image

In the lab, the image was initially built with the misspelled tag
`letest`:

``` bash
docker build -t suman:letest .
```

It was removed with:

``` bash
docker rmi suman:letest
```

Then it was rebuilt with the correct tag:

``` bash
docker build -t suman:latest .
```

Use the exact image name and tag shown by `docker images`. If an image
is being used by a container, stop and remove that container before
removing the image.

## 9. Pull an Alpine image (optional practice)

The lab also demonstrates downloading a separate image:

``` bash
docker pull alpine
```

Verify the downloaded image:

``` bash
docker images
```

This Alpine image is separate from the Apache image built in this
exercise; it is not required to run the website container.

## 10. Run the container

Start a container in detached mode and publish the web server port:

``` bash
docker run -itd --name my-web-container -p 8080:80 suman:latest
```

Explanation:

-   `docker run` creates and starts a container.
-   `-i` keeps standard input open.
-   `-t` allocates a pseudo-terminal.
-   `-d` runs the container in the background.
-   `--name my-web-container` gives the container a readable name.
-   `-p 8080:80` maps port `8080` on the Docker host to port `80` inside
    the container.
-   `suman:latest` is the image used to create the container.

You can also use the shorter command from the lab:

``` bash
docker run -itd -p 8080:80 suman:latest
```

Docker will print the new container ID.

## 11. Verify that the container is running

Run:

``` bash
docker ps
```

Check that the container status is `Up` and the port mapping includes:

``` text
0.0.0.0:8080->80/tcp
```

To see all containers, including stopped ones:

``` bash
docker ps -a
```

## 12. Open the website

On the Docker host, open:

``` text
http://localhost:8080
```

From another machine, use the Docker host's IP address:

``` text
http://<DOCKER-HOST-IP>:8080
```

For example, the lab host showed the address `192.168.97.3`, so the URL
on that network would be:

``` text
http://192.168.97.3:8080
```

The example IP is specific to the lab output; your host may have a
different address. Find it with:

``` bash
ip a
```

If the page does not load, check that the container is running, that the
port mapping is correct, and that the host firewall or network allows
TCP port `8080`.

## 13. Stop the container

Stop the running container using its name:

``` bash
docker stop my-web-container
```

Or use its container ID:

``` bash
docker stop <CONTAINER_ID>
```

Verify that it has stopped:

``` bash
docker ps
```

A stopped container will not appear in the default `docker ps` output.
Use `docker ps -a` to see it.

## 14. Useful Docker commands recap

  -------------------------------------------------------------------------------
  Command                                     What it does
  ------------------------------------------- -----------------------------------
  `docker --version`                          Displays the Docker version.

  `docker build -t suman:latest .`            Builds the image with the
                                              `suman:latest` tag.

  `docker images`                             Lists local images.

  `docker pull alpine`                        Downloads the Alpine image.

  `docker run -itd -p 8080:80 suman:latest`   Creates and starts a detached
                                              container with port mapping.

  `docker ps`                                 Lists running containers.

  `docker ps -a`                              Lists all containers, including
                                              stopped ones.

  `docker stop my-web-container`              Stops the named container.

  `docker rmi suman:letest`                   Removes the incorrectly tagged
                                              image, if it is not in use.
  -------------------------------------------------------------------------------

## 15. Understand the workflow

The overall process is:

1.  Create `index.html`.
2.  Write the `Dockerfile`.
3.  Build an image from the Dockerfile.
4.  Check the image with `docker images`.
5.  Create and start a container from the image.
6.  Publish host port `8080` to container port `80`.
7.  Access the website in a browser.
8.  Stop the container when finished.

**Remember:** An image is the packaged template. A container is a
running (or stopped) instance created from an image.
