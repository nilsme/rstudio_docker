# README

This is a self-build container setup to run RStudio Server behind a reverse
https proxy. It was mostly done to learn more about container orchestration and
how to use it in a private network. As no detailed configuration was done, it
is not advised to use this setup in any productive environment.

The project uses traefik as a reverse proxy that also manages ssl connections
to all services. Thus, a daemon with active socket communication is required.
Docker provides this by default.

> For podman sockets can be used as well. E.g. for RHEL/ Fedora install the
> following packages and enable the service.

```Shell script
sudo yum install -y podman podman-docker docker-compose
sudo systemctl enable --now podman.socket

# Allow rootless container
systemctl --user enable --now podman.socket
echo -e "export DOCKER_HOST=unix:///run/user/$UID/podman/podman.sock" >> ~/.profile
```

## Usage

Go to the main directory and run `docker-compose up -d` to build and start all
containers in detached mode.

### Traefik

The dashboard is available on the configured `HOST` or as default `localhost`.

> `https://localhost`

### RStudio

RStudio is available on the subdomain `rstudio`.

> `https://rstudio.localhost`

The container for RStudio will use the `/home` directory of the host as its
`/home` directory and also use the `/etc/passwd`, `/etc/group`, and
`/etc/shadow` from the host machine. If you don't want this, you have to
change the `docker-compose.yml` and also the dockerfile for rstudio according
to your needs.

> If you mount the suggested volumes from the host, it does need the user
> `rstudio-server` on the host system, otherwise the service will fail to start.
> You can add such a user on your host with `sudo adduser rstudio-server`.
