FROM peernine/ubuntu:16.04
MAINTAINER Alex Lee <alex.lee@peernine.co.kr>

ENV GOLANG_VERSION 1.11.1

# -----------------------------------------------------------------------------
# golang1.11.1 다운로드
RUN set -eux \
	&& mkdir -p /home/peernine/_src  /home/peernine/_project \
	&& axel -n 10 -o /home/peernine/_src/go.tgz https://storage.googleapis.com/golang/go$GOLANG_VERSION.linux-amd64.tar.gz \
	&& echo '2871270d8ff0c8c69f161aaae42f9f28739855ff5c5204752a8d92a1c9f63993 /home/peernine/_src/go.tgz' | sha256sum -c - \
	#go를 _app에 풀어놓음.
	&& tar -C /home/peernine/_app -xzf /home/peernine/_src/go.tgz \
	&& rm /home/peernine/_src/go.tgz \
	&& mv /home/peernine/_app/go /home/peernine/_app/go1.11.1 \
	&& mkdir -p /home/peernine/_project/golang/src /home/peernine/_project/golang/pkg /home/peernine/_project/golang/bin

# -----------------------------------------------------------------------------
# geth 다운로드
RUN add-apt-repository -y ppa:ethereum/ethereum

ARG binary
RUN apt-get update \
     && apt-get install -y ethereum=$binary

# -----------------------------------------------------------------------------
# git
RUN apt-fast update -y \
	&& apt-fast install -y git

# -----------------------------------------------------------------------------
# 정리
RUN chmod 755 /hooks \
	&& chown -R peernine:peernine /home/peernine/* \
	&& apt-get -y clean \
  	&& rm -rf /var/lib/apt/lists/* \
  	&& rm -rf /var/cache/apt/archives/apt-fast/* \
  	&& rm -rf /home/peernine/_src/*

ENV GOPATH /home/peernine/_project/golang
ENV GOROOT /home/peernine/_app/go1.11.1
ENV GOROOT_BOOTSTRAP $GOROOT
ENV GOBIN $GOROOT/bin
ENV PATH $GOBIN:$PATH