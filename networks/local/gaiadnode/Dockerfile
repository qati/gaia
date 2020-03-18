#syntax = docker/dockerfile:experimental
#export DOCKER_BUILDKIT=1

FROM golang:buster

RUN apt update -y && \
    apt upgrade  -y && \
    apt install -y curl jq file make git libgmp-dev wget gcc g++ swig && \
    git clone https://github.com/herumi/mcl && cd mcl && make install && ldconfig &&\
    mkdir -m 700 /root/.ssh && \
    touch -m 600 /root/.ssh/known_hosts && \
    git config --global url."git@github.com:".insteadOf https://github.com/ && \
    ssh-keyscan github.com > /root/.ssh/known_hosts && \
    echo "GOPRIVATE=github.com/fetchai/*,github.com/jinmannwong/*" >> $HOME/.bashrc 

WORKDIR /source
RUN --mount=type=ssh \
    git clone https://github.com/qati/gaia && cd gaia && git checkout drb && \
    make build && \
    mkdir /gaiad && \
    mv build/gaiad /gaiad/

VOLUME [ /gaiad ]
WORKDIR /gaiad
EXPOSE 26656 26657
ENTRYPOINT ["/usr/bin/wrapper.sh"]
CMD ["start"]
STOPSIGNAL SIGTERM

COPY wrapper.sh /usr/bin/wrapper.sh