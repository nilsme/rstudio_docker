FROM fedora:35

# Set versions
ARG R_VERSION
ARG RSTUDIO_VERSION
ARG MINICONDA_VERSION

# Set environment
ENV TZ=Europe/Amsterdam
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PATH=/opt/python/conda/bin:$PATH

# Install system packages
RUN yum -y update && \
    yum -y install \
    # Install git
    git \
    # Install ldap client packages
    openldap-clients \
    # Install compilers
    gcc \
    gcc-c++ \
    gcc-gfortran \
    java-latest-openjdk \
    # Install system packages for compilers and R \
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
    # Install latex
    texlive \
    # Install auxiliary system packages
    wget \
    zip \
    unzip \
    vim

# Download R and compile R from source
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

# Link R
RUN ln -s /opt/R/${R_VERSION}/bin/R /usr/local/bin/R && \
    ln -s /opt/R/${R_VERSION}/bin/Rscript /usr/local/bin/Rscript

# Download and install rstudio server
RUN curl https://download2.rstudio.org/server/rhel8/x86_64/rstudio-server-rhel-${RSTUDIO_VERSION}-x86_64.rpm \
    -o /tmp/rstudio-server.rpm && \
    yum install -y /tmp/rstudio-server.rpm && \
    rm /tmp/rstudio-server.rpm

# Install miniconda3 for Python support
RUN wget https://repo.continuum.io/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh \
    -O /tmp/miniconda.sh  && \
    bash /tmp/miniconda.sh -f -b -p /opt/python/conda && \
    /opt/python/conda/bin/conda install --yes -c conda-forge \
      python \
      pip \
      && \
    /opt/python/conda/bin/pip install --upgrade pip && \
    rm /tmp/miniconda.sh

# Make Python visible to RStudio
RUN echo 'options(rstudio.python.installationPath = "/opt/python/conda/bin")' \
    >> /etc/rstudio/Rprofile.site

# Expose standard port for rstudio-server
EXPOSE 8787

# Start rstudio-server
CMD ["/usr/lib/rstudio-server/bin/rserver", "--server-daemonize", "0"]
