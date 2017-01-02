FROM hepdocker/analysis-base:latest

MAINTAINER Luke Kreczko <kreczko@cern.ch>

ARG ROOT_VERSION=v6-08-02
ENV ROOTSYS /usr/local

# get ROOT source code
RUN git clone --quiet http://root.cern.ch/git/root.git /code/root-${ROOT_VERSION} &&\
    cd  /code/root-${ROOT_VERSION} &&\
    git checkout ${ROOT_VERSION}

# install dependencies
# from https://root.cern.ch/build-prerequisites
RUN apt-get install -y dpkg-dev libx11-dev libxpm-dev libafterimage-dev \
                       libxft-dev libxext-dev gfortran libssl-dev libpcre3-dev \
                       xlibmesa-glu-dev libglew1.5-dev libftgl-dev \
                       libmysqlclient-dev libfftw3-dev libcfitsio-dev \
                       graphviz-dev libavahi-compat-libdnssd-dev \
                       libldap2-dev python-dev libxml2-dev libkrb5-dev \
                       libgsl-dev libqt4-dev &&\
    apt-get clean -y

# compile & install ROOT
ENV LIBPNG /usr/lib/x86_64-linux-gnu
RUN cd /code/root-${ROOT_VERSION} &&\
    LIBPNG=/usr/lib/x86_64-linux-gnu \
    ./configure \
    --enable-roofit \
    --enable-minuit2 \
    --with-gsl-libdir=/usr/lib/x86_64-linux-gnu \
    --with-x11-libdir=/usr/lib/x86_64-linux-gnu \
    --with-xpm-libdir=/usr/lib/x86_64-linux-gnu \
    --with-xft-libdir=/usr/lib/x86_64-linux-gnu \
    --with-xext-libdir=/usr/lib/x86_64-linux-gnu &&\
    make -j2 &&\
    make -j2 install &&\
    cd / &&\
    rm -rf /code
