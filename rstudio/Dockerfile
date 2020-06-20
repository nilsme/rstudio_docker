FROM ubuntu:18.04

# install system packages
ENV DEBIAN_FRONTEND noninteractive
ENV TZ=Europe/Amsterdam
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
    python-pip \
    python-dev \
    python-virtualenv \
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
    # PostgreSQL ODBC ODBC Drivers
    odbc-postgresql \
    # MySQL ODBC Drivers
    # libmyodbc \
    # SQLite ODBC Drivers
    libsqliteodbc
    
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# add R version 4.x repository
RUN echo 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran40/' >> /etc/apt/sources.list && \
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 && \
    apt-get update

# install r-base
RUN apt-get install -y r-base

# install rstudio-server
RUN apt-get install -y wget gdebi-core && \
    wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-1.2.5019-amd64.deb && \
    gdebi -n rstudio-server-1.2.5019-amd64.deb

# clean up
RUN apt-get purge && \
    apt-get clean && \
    rm rstudio-server-1.2.5019-amd64.deb

# add directory for R related files
RUN mkdir -p /etc/R 

# expose standard port for rstudio-server
EXPOSE 8787

