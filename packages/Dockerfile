FROM	debian:testing-slim

ENV     DEBIAN_FRONTEND noninteractive

RUN	mkdir /etc/sudoers.d \
     &&	echo "%wheel ALL=(ALL:ALL) NOPASSWD: ALL" > /etc/sudoers.d/wheel \
     && echo "deb-src http://deb.debian.org/debian testing main" >> /etc/apt/sources.list \
     &&	echo "#deb http://deb.debian.org/debian unstable main" >> /etc/apt/sources.list \
     && echo "#deb-src http://deb.debian.org/debian unstable main" >> /etc/apt/sources.list \
     &&	echo "debconf debconf/frontend select Noninteractive" | debconf-set-selections \
     && echo "APT::Install-Recommends false;\nAPT::Install-Suggests false;\nApt::AutoRemove::SuggestsImportant false;\n" > /etc/apt/apt.conf.d/no-recommends \
     &&	echo "progress=bar:force:noscroll" >> /etc/wgetrc \
     &&	echo "force-confold\nforce-confdef" > /etc/dpkg/dpkg.cfg.d/forceold 

ADD	http://deb.debian.org/debian/pool/main/g/gcc-defaults/gcc_10.2.0-1_amd64.deb /
ADD	http://deb.debian.org/debian/pool/main/g/gcc-defaults/g++_10.2.0-1_amd64.deb /
ADD	http://deb.debian.org/debian/pool/main/g/gcc-defaults/cpp_10.2.0-1_amd64.deb /
ADD	http://deb.debian.org/debian/pool/main/g/gcc-defaults/gcc-multilib_10.2.0-1_amd64.deb /

RUN	apt-get update \
     &&	apt-get install -y --no-install-recommends -f /*.deb \
     &&	apt-mark auto gcc g++ cpp gcc-multilib \
     &&	rm -f /*.deb \
     &&	dpkg -P gcc-9-base \
     &&	apt-get install -y --no-install-recommends build-essential fakeroot sudo devscripts \
     &&	addgroup --system wheel \
     &&	adduser dev --disabled-password --gecos dev \
     &&	adduser dev wheel

USER	dev
WORKDIR	/home/dev
