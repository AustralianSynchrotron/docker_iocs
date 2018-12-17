# Based on https://github.com/DMSC-Instrument-Data/lewis-misc/blob/master/docker/epics-base/Dockerfile
# Using alpine 3.5 instead of 3.6 as galil driver wasn't happy with 3.6. Possibly GCC?
FROM alpine:3.5

# Runtime dependencies
RUN apk --no-cache add \
    libstdc++ \
    tini \
    readline \
    libexecinfo

# Build and install procServ
RUN apk --no-cache add --virtual .build-deps g++ make openssl wget ca-certificates \
    && cd /tmp \
    && wget http://downloads.sourceforge.net/project/procserv/2.7.0/procServ-2.7.0.tar.gz \
    && wget https://launchpad.net/ubuntu/+archive/primary/+files/libtelnet_0.21.orig.tar.gz \
    && tar xf procServ*.tar.gz && rm procServ*.tar.gz \
    && tar xf libtelnet*.tar.gz && rm libtelnet*.tar.gz \
    && cd libtelnet* \
    && ./configure \
    && make install \
    && cd .. && rm -rf libtelnet* \
    && cd procServ* \
    && ./configure --enable-access-from-anywhere \
    && make install \
    && cd .. && rm -rf procServ*

EXPOSE 5001

ARG epics_version

# Get EPICS source -> Build -> Cleanup - commented commands are for epics 3.14.12.5
RUN apk --no-cache add --virtual .build-deps cmake git perl-dev readline-dev libexecinfo-dev \
\
    && mkdir /EPICS \
    && cd /EPICS \
    && wget http://www.aps.anl.gov/epics/download/base/base$epics_version.tar.gz -O EPICS.tar.gz \
    && tar xzf EPICS.tar.gz \
    && mv base-*/ base/ \
#    && sed -i '/\$/d' /EPICS/base/configure/os/CONFIG_SITE.linux-x86_64.UnixCommon \
    && rm EPICS.tar.gz \
\
    && cd /EPICS/base \
 #   && make -j4 CFLAGS="-DIPPORT_USERRESERVED=5000 -fPIC" CXXFLAGS="-DIPPORT_USERRESERVED=5000 -fPIC -std=gnu++98" \
    && make -j4 CFLAGS="-DIPPORT_USERRESERVED=5000 -fPIC" CXXFLAGS="-DIPPORT_USERRESERVED=5000 -fPIC" \
    && make clean \
\
    && apk del --purge .build-deps

# Copy additional files to image filesystem
COPY copyroot/ /

# Entering via init.sh ensures environment is sourced and passed in command executed via /tini
ENTRYPOINT ["/init.sh"]
