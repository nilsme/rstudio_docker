# README

This is a self-build container setup to run RStudio Server behind a reverse
https proxy. It was mostly done to learn more about container orchestration and
how to use it in a private network. As no detailed configuration was done, it
is not advised to use this setup in any productive environment.

Nginx is used as the reverse proxy for https support and its config can
be accessed and changed in `proxy/nginx.conf`. The ssl certificate is provided
with the docker container `paulczar/omgwtfssl`. The environment variables for
the certificate can be changed in `docker-compose.yml`.

> The container for RStudio will use the `/home` directory of the host as its
> `/home` directory and also use the `/etc/passwd`, `/etc/group`, and
> `/etc/shadow` from the host machine. If you don't want this, you have to
> change the `docker-compose.yml` and also the dockerfile for rstudio according
> to your needs.

## Usage

Go to the main directory with the `docker-compose.yml` and run
`docker-compose up -d` to build and start all containers in detached mode.
After a successful run RStudio can be accessed on
`https://<ip-of-your-machine>:10443`.

> If you mount the suggested volumes from the host, it does need the user
> `rstudio-server` on the host system, otherwise the service will fail to start.
> You can add such a user on your host with `sudo adduser rstudio-server`.
