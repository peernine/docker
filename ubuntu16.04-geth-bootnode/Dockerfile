FROM peernine/ubuntu16.04-golang1.11.1
MAINTAINER Alex Lee <alex.lee@peernine.co.kr>

RUN apt-get update \
     && apt-get install -y wget software-properties-common \
     && rm -rf /var/lib/apt/lists/*

WORKDIR "/home/peernine"

ARG BINARY="geth-alltools-linux-amd64-1.8.17-8bbe7207.tar.gz"
RUN axel -n 10 -o $BINARY "https://gethstore.blob.core.windows.net/builds/$BINARY"
RUN tar -xzvf $BINARY --strip 1
RUN rm $BINARY
ENV nodekeyhex=""
CMD exec ./bootnode -nodekeyhex $nodekeyhex

EXPOSE 30301/udp
EXPOSE 30303/udp
