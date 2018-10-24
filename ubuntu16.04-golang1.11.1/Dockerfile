FROM peernine/ubuntu:16.04
MAINTAINER Alex Lee <alex.lee@peernine.co.kr>

ENV GOLANG_VERSION 1.11.1

# -----------------------------------------------------------------------------
# golang1.11.1 다운로드
RUN set -eux \
	&& mkdir -p /home/peernine/_src  /home/peernine/_project \
	&& axel -n 10 -o /home/work/_src/go.tgz https://storage.googleapis.com/golang/go$GOLANG_VERSION.src.tar.gz \
	&& echo '558f8c169ae215e25b81421596e8de7572bd3ba824b79add22fba6e284db1117 /home/peernine/_src/go.tgz' | sha256sum -c - \
	&& tar -C /home/peernine/_app -xzf /home/peernine/_src/go.tgz \
	&& cd /home/peernine/_app/go/src \
	&& ./make.bash \
	&& mv /home/peernine/_app/go /home/peernine/_app/go1.11.1 \
	&& mkdir -p /home/peernine/_project/golang/src /home/peernine/_project/golang/pkg /home/peernine/_project/golang/bin


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