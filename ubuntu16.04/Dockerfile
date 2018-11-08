#ubuntu16.04-peernine
FROM ubuntu:16.04
MAINTAINER Alex Lee <alex.lee@peernine.co.kr>

ARG DEBIAN_FRONTEND=noninteractive
COPY files/ /

# -----------------------------------------------------------------------------
# Run Upgrade
RUN apt-get update -y \
	&& apt-get -y upgrade \
	&& apt-get -o Dpkg::Options::=--force-confdef -y install curl netcat wget telnet vim bzip2 ssmtp locales python-pip software-properties-common python-software-properties bash-completion net-tools iputils-ping language-pack-ko \
	&& locale-gen ko_KR.utf8 \
	&& localedef -i ko_KR -c -f UTF-8 -A /usr/share/locale/locale.alias ko_KR.UTF-8 \
	&& chmod -R 777 /var/run /var/log /etc/passwd /etc/group \
	&& pip --no-cache install --upgrade pip

# -----------------------------------------------------------------------------
# apt-fast && axel
RUN add-apt-repository ppa:saiarcot895/myppa \
	&& apt-get update -y \
	&& apt-get -y install apt-fast axel

# -----------------------------------------------------------------------------
# timezone Asia/Seoul
RUN apt-fast install -y tzdata \
	&& ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime \
	&& echo "Asia/Seoul" > /etc/timezone \
	&& dpkg-reconfigure -f noninteractive tzdata

# -----------------------------------------------------------------------------
# add user peernine
RUN useradd -r -m peernine \
    && echo "peernine:123455" | chpasswd \
    && echo "root:123455" | chpasswd

# -----------------------------------------------------------------------------
# supervisor
RUN apt-fast install -y supervisor \
	&& mkdir -p /home/peernine/_app/supervisord/conf.d/ /home/peernine/_logs/supervisord/ \
	&& echo_supervisord_conf > /home/peernine/_app/supervisord/supervisord.ini \
	&& echo "[include]" >> /home/peernine/_app/supervisord/supervisord.ini \
    && echo "files = /home/peernine/_app/supervisord/conf.d/*.ini" >> /home/peernine/_app/supervisord/supervisord.ini \
    && sed -i "s/\/tmp\/supervisord.pid/\/var\/run\/supervisord.pid/" /home/peernine/_app/supervisord/supervisord.ini \
    && sed -i "s/logfile=\/tmp\/supervisord.log/logfile=\/home\/peernine\/_logs\/supervisord\/supervisord.log/" /home/peernine/_app/supervisord/supervisord.ini \
    && sed -i "s/\/etc\/supervisor\/supervisord.conf/\/home\/peernine\/_app\/supervisord\/supervisord.ini/" /etc/init.d/supervisor


# -----------------------------------------------------------------------------
# remove
RUN chmod 755 /hooks \
	&& chown -R peernine:peernine /home/peernine/* \
	&& apt-get -y clean \
  	&& rm -rf /var/lib/apt/lists/* \
  	&& rm -rf /var/cache/apt/archives/apt-fast/* \
  	&& rm -rf /home/peernine/_src/*


# -----------------------------------------------------------------------------
# Define working directory.
WORKDIR /home/peernine


# -----------------------------------------------------------------------------
# Ensure UTF-8
ENV \
	LC_ALL=ko_KR.UTF-8 \
	LANG=ko_KR.UTF-8 \
	LANGUAGE=ko_KR.UTF-8

ENTRYPOINT ["/bin/bash", "-e", "/init/entrypoint"]
CMD ["run"]
