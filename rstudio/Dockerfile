FROM ubuntu:20.04

# set versions
ARG RSTUDIO_VERSION
ARG S6_VERSION

# set repository variables
ARG CRAN_REPO
ARG CRAN_KEY

# set environment
ENV DEBIAN_FRONTEND noninteractive
ENV TZ=Europe/Amsterdam
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# install system packages
RUN apt-get -y update && \
    apt-get -y upgrade && \
    # install git
    apt-get -y install git \
    # install tzdata
    tzdata \
    # install system dependencies for R packages
    build-essential \
    default-jdk \
    gfortran \
    gnome-keyring \
    jags \
    jq \
    libatlas3-base \
    libboost-all-dev \
    libcairo2 \
    libcairo2-dev \
    libcurl4-openssl-dev \
    libfftw3-dev \
    libgdal-dev \
    libgraphviz-dev \
    libgsl-dev \
    libmysqlclient-dev \
    libnetcdf-dev \
    libprocps-dev \
    libproj-dev \
    libprotoc-dev \
    libsecret-1-dev \
    libquantlib0-dev \
    libxml2-dev \
    libsasl2-dev \
    libxt-dev \
    libxt6 \
    lsb-release \
    net-tools \
    openssh-client \
    protobuf-compiler \
    python3-pip \
    python3-dev \
    python3-virtualenv \
    tdsodbc \
    texinfo \
    texlive \
    texlive-bibtex-extra \
    texlive-fonts-extra \
    texlive-latex-extra \
    texlive-xetex \
    unixodbc \
    unixodbc-dev \
    # SQL Server ODBC Drivers (Free TDS)
    tdsodbc \
    # PostgreSQL ODBC Drivers
    odbc-postgresql \
    # MySQL ODBC Drivers
    # libmyodbc \ -> No installation candidate in repository!
    # SQLite ODBC Drivers
    libsqliteodbc && \
    # clean up
    apt-get purge && \
    apt-get clean

# add R version 4.x.x repository and install latest r-base
RUN echo ${CRAN_REPO} >> /etc/apt/sources.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ${CRAN_KEY} && \
    apt-get update && \
    apt install -y r-base && \
    # clean up
    apt-get purge && \
    apt-get clean

# install rstudio-server
RUN apt-get install -y wget gdebi-core && \
    wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-${RSTUDIO_VERSION}-amd64.deb && \
    gdebi -n rstudio-server-${RSTUDIO_VERSION}-amd64.deb && \
    # clean up
    apt-get purge && \
    apt-get clean && \
    rm rstudio-server-${RSTUDIO_VERSION}-amd64.deb

# expose standard port for rstudio-server
EXPOSE 8787

# start rstudio-server
CMD ["/usr/lib/rstudio-server/bin/rserver", "--server-daemonize", "0"]
