FROM debian:jessie

RUN apt-get update
RUN apt-get install -y --force-yes curl git mercurial make binutils bison gcc build-essential

RUN curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer > /tmp/gvm_install
RUN chmod +x /tmp/gvm_install
RUN /tmp/gvm_install