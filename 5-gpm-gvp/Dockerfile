FROM golang:1.4

RUN apt-get update
RUN apt-get install git build-essential -y --force-yes

RUN git clone https://github.com/pote/gvp.git /tmp/gvp
WORKDIR /tmp/gvp
RUN ./configure && make install

RUN git clone https://github.com/pote/gpm.git /tmp/gpm
WORKDIR /tmp/gpm
RUN ./configure && make install
