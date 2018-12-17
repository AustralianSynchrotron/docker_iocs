ARG epics_version
FROM epics-modules:$epics_version
RUN apk --no-cache add --virtual .build-deps perl git g++ make linux-headers
COPY generic_ioc \IOC
RUN cd /IOC && make
RUN apk del --purge .build-deps
WORKDIR /IOC/iocBoot/generic_ioc/
ENTRYPOINT ["/init.sh", "/IOC/bin/linux-x86_64/generic_ioc"]
CMD ["/IOC/iocBoot/generic_ioc/st.cmd"]