FROM ubuntu:18.04

# install system packages
ENV DEBIAN_FRONTEND noninteractive
ENV TZ=Europe/Amsterdam
RUN apt-get -y update && \
    apt-get -y upgrade && \
    # install git
    apt-get -y install git && \
    # install tzdata
    apt-get -y install tzdata && \
    # install system dependencies for R packages
    apt-get -y install build-essential && \
    apt-get -y install default-jdk && \
    apt-get -y install gdebi-core && \
    apt-get -y install gfortran && \
    apt-get -y install gnome-keyring && \
    apt-get -y install jags && \
    apt-get -y install jq && \
    apt-get -y install libatlas3-base && \
    apt-get -y install libboost-all-dev && \
    apt-get -y install libcairo2 && \
    apt-get -y install libcairo2-dev && \
    apt-get -y install libcurl4-openssl-dev && \
    apt-get -y install libfftw3-dev && \
    apt-get -y install libgdal-dev && \
    apt-get -y install libgraphviz-dev && \
    apt-get -y install libgsl-dev && \
    apt-get -y install libmysqlclient-dev && \
    apt-get -y install libnetcdf-dev && \
    apt-get -y install libprocps-dev && \
    apt-get -y install libproj-dev && \
    apt-get -y install libprotoc-dev && \
    apt-get -y install libsecret-1-dev && \
    apt-get -y install libquantlib0-dev && \
    apt-get -y install libxml2-dev && \
    apt-get -y install libsasl2-dev && \
    apt-get -y install libxt-dev && \
    apt-get -y install libxt6 && \
    apt-get -y install lsb-release && \
    apt-get -y install net-tools && \
    apt-get -y install odbc-postgresql && \
    apt-get -y install openssh-client && \
    apt-get -y install protobuf-compiler && \
    apt-get -y install python-pip && \
    apt-get -y install python-dev && \
    apt-get -y install python-virtualenv && \
    apt-get -y install tdsodbc && \
    apt-get -y install texinfo && \
    apt-get -y install texlive && \
    apt-get -y install texlive-bibtex-extra && \
    apt-get -y install texlive-fonts-extra && \
    apt-get -y install texlive-latex-extra && \
    apt-get -y install texlive-xetex && \
    apt-get -y install unixodbc && \
    apt-get -y install unixodbc-dev && \
    apt-get -y install wget && \
    # install r-base 
    apt-get -y install r-base && \
    # clean up
    apt-get purge && \
    apt-get clean
    
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# install rstudio-server
RUN wget https://download2.rstudio.org/server/bionic/amd64/rstudio-server-1.2.5019-amd64.deb && \
    gdebi -n rstudio-server-1.2.5019-amd64.deb && \
    rm rstudio-server-1.2.5019-amd64.deb
    
# add directory for R related files
RUN mkdir -p /etc/R 

# expose standard port for rstudio-server
EXPOSE 8787

ENTRYPOINT service rstudio-server start && /bin/bash
