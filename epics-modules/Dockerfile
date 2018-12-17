ARG epics_version
FROM epics-base:$epics_version

RUN apk --no-cache add \
    readline \
    libtirpc

RUN apk --no-cache add --virtual .temp-deps openssl git \
    && mkdir -p /EPICS/support && cd /EPICS/support \
    && git config --global advice.detachedHead false \
    && git clone https://github.com/epics-modules/alive.git \ 
    && git clone --depth 1 --branch R4-30 https://github.com/epics-modules/asyn.git \
    && git clone --depth 1 --branch R5-7-1 https://github.com/epics-modules/autosave.git \
    && git clone --depth 1 --branch R1-6-1 https://github.com/epics-modules/busy.git \
    && git clone --depth 1 --branch R3-6-1 https://github.com/epics-modules/calc.git \
    && git clone --depth 1 --branch 2.14 https://github.com/epics-modules/ipac.git \
    && git clone --depth 1 --branch R2-10-2 https://github.com/epics-modules/sscan.git \
    && git clone --depth 1 --branch R3-4-1 https://github.com/epics-modules/std.git \
    && git clone --depth 1 --branch R2-6c https://github.com/epics-modules/stream.git \
#    && cd stream/StreamDevice \  # These lines will be need if grabing a newer release. Check repository for details
#    && git submodule init \
#    && git submodule update \
#    && cd /EPICS/support \
    && wget http://www-csr.bessy.de/control/SoftDist/sequencer/releases/seq-2.2.3.tar.gz \
    && tar xf seq-2.2.3.tar.gz \
    && rm seq-2.2.3.tar.gz \
    && mv seq-2.2.3/ seq \
    && find . -name '.git'  -print0 | xargs -0 rm -r \
    && find . -name 'RELEASE' -exec sh -c 'sed -i "s|SUPPORT=.*|SUPPORT=/EPICS/support|" {} && sed -i "s|EPICS_BASE=.*|EPICS_BASE=/EPICS/base|" {}' \; \
    && find . -name 'RELEASE' -exec sh -c 'sed -i "s/ipac.*/ipac/" {} \
                                            && sed -i "s/alive.*/alive/" {} \
                                            && sed -i "s/seq.*/seq/" {} \
                                            && sed -i "s/sscan.*/sscan/" {} \
                                            && sed -i "s/std.*/std/" {} \
                                            && sed -i "s/busy.*/busy/" {} \
                                            && sed -i "s/calc.*/calc/" {} \
                                            && sed -i "s/autosave.*/autosave/" {} \
                                            && sed -i "s/asyn.*/asyn/" {} \
                                            && sed -i "s/stream.*/stream/" {}' \; \
    && apk del --purge .temp-deps

RUN apk --no-cache add --virtual .build-deps g++ make perl re2c rpcgen readline-dev libtirpc-dev linux-headers \
    && cd /EPICS/support \

    # fix rpc -> tirpc for async and sscan
    && sed '/USR_INCLUDES_cygwin32/a USR_INCLUDES_Linux += -I/usr/include/tirpc' -i asyn/asyn/Makefile \
    && sed '/asyn_SYS_LIBS_cygwin32/a asyn_SYS_LIBS_Linux += tirpc' -i asyn/asyn/Makefile \
    && sed '/USR_INCLUDES_cygwin32/a USR_INCLUDES_Linux += -I/usr/include/tirpc' -i sscan/sscanApp/src/Makefile \
    && sed '/sscan_SYS_LIBS_cygwin32/a asyn_SYS_LIBS_Linux += tirpc' -i sscan/sscanApp/src/Makefile \

    # build
    && make -C alive install \
    && make -C seq install \
    && make -C ipac install \
    && make -C asyn install \
    && make -C busy install \
    && make -C autosave install \
    && make -C sscan install \
    && make -C std install \
    && make -C calc install \
    && make -C stream install \

    # clean up
    && cd /EPICS/support \
    && make -C alive clean \
    && make -C asyn clean \
    && make -C autosave clean \
    && make -C busy clean \
    && make -C calc clean \
    && make -C ipac clean \
    && make -C seq clean \
    && make -C sscan clean \
    && make -C std clean \
    && make -C stream clean \
    && apk del --purge .build-deps
