FROM ubuntu:latest
ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update -y \
    && apt-get upgrade -y

RUN apt install -y python3-pip
RUN pip3 install ninja meson

RUN apt-get install -y \
    build-essential \
    libmicrohttpd-dev \
    libjansson-dev \
    libssl-dev \
    libini-config-dev \
    libconfig-dev \
    libsofia-sip-ua-dev \
    libglib2.0-dev \
    libopus-dev \
    libcurl4-openssl-dev \
    liblua5.3-dev \
    libogg-dev \
    libini-config-dev \
    libcollection-dev \
    pkg-config \
    gengetopt \
    libtool \
    autotools-dev \
    automake

RUN apt-get install -y \
    sudo \
    make \
    git \
    doxygen \
    graphviz \
    cmake

RUN cd ~ \
    && git clone https://github.com/cisco/libsrtp.git \
    && cd libsrtp \
    && git checkout v2.1.0 \
    && ./configure --prefix=/usr --enable-openssl \
    && make shared_library \
    && sudo make install

RUN cd ~ \
    && git clone https://github.com/sctplab/usrsctp \
    && cd usrsctp \
    && ./bootstrap \
    && ./configure --prefix=/usr \
    && make \
    && sudo make install

RUN cd ~ \
    && git clone https://gitlab.freedesktop.org/libnice/libnice \
    && cd libnice \
    && meson --prefix=/usr build && ninja -C build && sudo ninja -C build install

RUN cd ~ \
    && git clone https://github.com/warmcat/libwebsockets.git \
    && cd libwebsockets \
    && git checkout v2.1.0 \
    && mkdir build \
    && cd build \
    && cmake -DCMAKE_INSTALL_PREFIX:PATH=/usr .. \
    && make \
    && sudo make install

RUN cd ~ \
    && git clone https://github.com/meetecho/janus-gateway.git \
    && cd janus-gateway \
    && sh autogen.sh \
    && ./configure --prefix=/opt/janus --disable-rabbitmq --disable-mqtt --enable-docs \
    && make CFLAGS='-std=c99' \
    && make install \
    && make configs

#RUN cp -rp ~/janus-gateway/certs /opt/janus/share/janus

COPY conf/* /opt/janus/etc/janus/

#RUN apt-get install nginx -y
#COPY nginx/nginx.conf /etc/nginx/nginx.conf

EXPOSE 80 7088 8088 8188 8089 9595 9696
EXPOSE 10000-10200/udp

CMD /opt/janus/bin/janus
#service nginx restart && /opt/janus/bin/janus --nat-1-1=${DOCKER_IP}
