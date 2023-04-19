FROM rocker/r-ver:3.4
MAINTAINER Peter Kharchenko "peter_kharchenko@hms.harvard.edu"

RUN apt update && apt install software-properties-common --yes

RUN apt-get update --yes && apt-get install --no-install-recommends --yes \
  build-essential \
  cmake \
  git \
  libbamtools-dev \
  libboost-dev \
  libboost-iostreams-dev \
  libboost-log-dev \
  libboost-system-dev \
  libboost-test-dev \
  libssl-dev \
  libcurl4-openssl-dev \
  libxml2-dev \
  libz-dev \
  curl \
  libhdf5-cpp-100 \ 
  libarmadillo7 \
  libarmadillo-dev \
  libfontconfig1-dev \
  libharfbuzz-dev \
  libfribidi-dev \
  libfreetype6-dev \
  libpng-dev \
  libtiff5-dev \
  libjpeg-dev \
  librsvg2-dev \
  libwebp-dev \
  libpoppler-cpp-dev \
  libmagick++-dev \
  libgdal-dev \
  libtesseract-dev \
  libleptonica-dev \
  tesseract-ocr-eng \
  libglu1-mesa-dev \
  libssh2-1-dev \
  wget

RUN \
  R -e 'install.packages("h5", dependencies = TRUE)'

RUN \
  R -e 'install.packages("Rtsne", dependencies = TRUE)'

RUN \
  R -e 'install.packages("cluster", dependencies = TRUE)'

RUN \
  R -e 'install.packages("Rcpp", dependencies = TRUE)'

RUN \
  R -e 'install.packages("RcppArmadillo", dependencies = TRUE)'

RUN \
  R -e 'install.packages("Matrix", dependencies = TRUE)'

RUN \
  R -e 'install.packages("mgcv", dependencies = TRUE)'

RUN \
  R -e 'install.packages("abind", dependencies = TRUE)'

RUN \
  R -e 'chooseCRANmirror(ind=1); install.packages("BiocVersion", dependencies = TRUE)'

RUN \
  R -e 'chooseCRANmirror(ind=1); install.packages("BiocManager", dependencies = TRUE)'

RUN \
  R -e 'source("https://bioconductor.org/biocLite.R"); biocLite(c("graph"),suppressAutoUpdate=TRUE,ask=FALSE,suppressUpdates=TRUE)'

RUN \
  R -e 'install.packages("magick", dependencies = TRUE)'

RUN \
  R -e 'install.packages("rgl", dependencies = TRUE)'

RUN \
  R -e 'install.packages("igraph", dependencies = TRUE)'

RUN \
  R -e 'source("https://bioconductor.org/biocLite.R"); biocLite(c("GenomicRanges"),suppressAutoUpdate=TRUE,ask=FALSE,suppressUpdates=TRUE)'

RUN \
  R -e 'install.packages("data.table", dependencies = TRUE)'

RUN \
  R -e 'install.packages("devtools", dependencies = TRUE)'

RUN \
  R -e 'install.packages("locfit", dependencies = TRUE)'

RUN \
  R -e 'source("https://bioconductor.org/biocLite.R"); biocLite(c("pcaMethods","edgeR","Rsamtools","GenomicAlignments","GenomeInfoDb","Biostrings"),suppressAutoUpdate=TRUE,ask=FALSE,suppressUpdates=TRUE)'


RUN wget https://www.python.org/ftp/python/3.9.5/Python-3.9.5.tgz 
RUN tar xzf Python-3.9.5.tgz 
WORKDIR "/Python-3.9.5"
RUN ./configure --enable-optimizations
RUN make altinstall

RUN python3.9 -m pip install --upgrade pip setuptools wheel

RUN useradd -m user
USER user
ENTRYPOINT ["/bin/bash"]
RUN mkdir "/home/user/data"
WORKDIR "/home/user"

COPY --chown=user:user requirements.txt .
RUN pip3.9 install --user numpy
RUN pip3.9 install --user Cython
RUN pip3.9 install --user velocyto

RUN \
  git clone https://github.com/velocyto-team/velocyto.R && \
  mkdir -p ~/R/x86_64-redhat-linux-gnu-library/3.4

RUN \
  echo '.libPaths(c("~/R/x86_64-redhat-linux-gnu-library/3.4", .libPaths()))' > .Rprofile && \
  R -e 'library(devtools)' && \
  R -e 'devtools::install_local("~/velocyto.R/",dep=T,upgrade_dependencies=F)'

ENV  LD_LIBRARY_PATH=/usr/local/lib/R/lib/:$LD_LIBRARY_PATH \
  R_PROFILE=~/.Rprofile
  
ENV PATH="/home/user/.local/bin:${PATH}"