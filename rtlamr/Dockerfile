ARG BUILD_FROM
FROM $BUILD_FROM AS builder

ENV LANG C.UTF-8

#
# First install software packages needed to compile rtl_433 and to publish MQTT events
#
RUN apk add --no-cache --virtual build-deps alpine-sdk cmake git libusb-dev && \
    mkdir /tmp/src && \
    cd /tmp/src && \
    git clone git://git.osmocom.org/rtl-sdr.git && \
    mkdir /tmp/src/rtl-sdr/build && \
    cd /tmp/src/rtl-sdr/build && \
    cmake ../ -DINSTALL_UDEV_RULES=ON -DDETACH_KERNEL_DRIVER=ON -DCMAKE_INSTALL_PREFIX:PATH=/usr/local && \
    make && \
    make install && \
    chmod +s /usr/local/bin/rtl_* && \
    apk del build-deps && \
    rm -r /tmp/src && \
    apk add --no-cache libusb mosquitto-clients jq coreutils wget make musl-dev go git gcc

# Configure Go
ENV GOROOT /usr/lib/go
ENV GOPATH /go
ENV PATH /go/bin:$PATH
RUN mkdir -p ${GOPATH}/src ${GOPATH}/bin

RUN go get github.com/bemasher/rtlamr

FROM $BUILD_FROM

ENV LANG C.UTF-8

MAINTAINER y.aydogan@gmail.com
LABEL Description="Receive AMR readings from utility meters using RTL2832U based DVB-T USB tuners"

COPY --from=builder  /usr/local/bin/rtl_* / /usr/local/bin/
COPY --from=builder  /go/bin/* /usr/local/bin/
COPY run.sh /
RUN chmod a+x /run.sh

ENTRYPOINT ["sh","/run.sh"]
