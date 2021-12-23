FROM fedora:34

# set versions
ARG R_VERSION
ARG RSTUDIO_VERSION
ARG MINICONDA_VERSION

# set environment
ENV TZ=Europe/Amsterdam
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PATH=/opt/python/conda/bin:$PATH

# install system packages
RUN yum -y update && \
    yum -y install \
    # install git
    git \
    # install ldap client packages
    openldap-clients \
    # install compilers
    gcc \
    gcc-c++ \
    gcc-gfortran \
    java-latest-openjdk \
    # install system packages for compilers and R \
    readline-devel \
    libX11-devel \
    libICE-devel \
    libXt-devel \
    zlib-devel \
    cairo \
    cairo-devel \
    libcurl-devel \
    libsodium-devel \
    gsl-devel \
    jq \
    tesseract \
    libpng-devel \
    libtiff-devel \
    unixODBC-devel \
    sqlite-devel \
    postgresql-odbc \
    openssl-devel \
    # install latex
    texlive \
    # install auxiliary system packages
    wget \
    zip \
    unzip \
    vim

# download R and compile R from source
RUN curl https://cran.rstudio.com/src/base/R-${R_VERSION:0:1}/R-${R_VERSION}.tar.gz \
    -o /tmp/R-${R_VERSION}.tar.gz && \
    tar -xzvf /tmp/R-${R_VERSION}.tar.gz -C /tmp && \
    cd /tmp/R-${R_VERSION} && \
    ./configure \
    --prefix=/opt/R/${R_VERSION} \
    --enable-memory-profiling \
    --enable-R-shlib \
    --with-blas \
    --with-lapack && \
    make && \
    make install && \
    rm /tmp/R-${R_VERSION}.tar.gz

# link R
RUN ln -s /opt/R/${R_VERSION}/bin/R /usr/local/bin/R && \
    ln -s /opt/R/${R_VERSION}/bin/Rscript /usr/local/bin/Rscript

# download and install rstudio server
RUN curl https://download2.rstudio.org/server/centos8/x86_64/rstudio-server-rhel-${RSTUDIO_VERSION}-x86_64.rpm \
    -o /tmp/rstudio-server.rpm && \
    yum install -y /tmp/rstudio-server.rpm && \
    rm /tmp/rstudio-server.rpm

# install miniconda3 for Python support
RUN wget https://repo.continuum.io/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh \
    -O /tmp/miniconda.sh  && \
    bash /tmp/miniconda.sh -f -b -p /opt/python/conda && \
    /opt/python/conda/bin/conda install --yes -c conda-forge \
      python \
      pip \
      && \
    /opt/python/conda/bin/pip install --upgrade pip && \
    rm /tmp/miniconda.sh

# make Python visible to RStudio
RUN echo 'options(rstudio.python.installationPath = "/opt/python/conda/bin")' \
    >> /etc/Rprofile.site

# expose standard port for rstudio-server
EXPOSE 8787

# start rstudio-server
CMD ["/usr/lib/rstudio-server/bin/rserver", "--server-daemonize", "0"]
