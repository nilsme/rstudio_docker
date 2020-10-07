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

By using `docker-compose` all images will be loaded or build on the first run.
Go to the main directory with the `docker-compose.yml` and run
`docker-compose up -d` to build and start all containers in detached mode.
After a successful run RStudio can be accessed on
`https://<ip-of-your-machine>:10443`.

## Troubleshooting

If RStudio Server does not install correctly during the build, it may got stuck
on number of open files. You can build it manually with the following command
and use `docker-compose` just as a start-up an not build setup.

```shell
sudo docker build ./rstudio \
  -t rstudio_docker_app \
  --build-arg RSTUDIO_VERSION=1.2.5042 \
  --build-arg S6_VERSION=2.0.0.1 \
  --ulimit nofile=65535:65535
```
