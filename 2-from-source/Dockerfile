FROM debian:jessie

RUN apt-get update
RUN apt-get install git build-essential -y --force-yes

RUN git clone https://github.com/golang/go.git -b release-branch.go1.4 /tmp/go
WORKDIR /tmp/go/src
RUN ./all.bash

ENV PATH /tmp/go/bin:$PATH