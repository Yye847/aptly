FROM debian:trixie-slim

RUN apt-get update -y && apt-get install -y --no-install-recommends curl gnupg bzip2 xz-utils ca-certificates vim procps \
        golang golang-go golang-doc golang-src \
        make git python3 python3-requests-unixsocket python3-termcolor python3-swiftclient python3-boto3 python3-azure-storage \
        g++ python3-etcd3 python3-plyvel graphviz devscripts sudo dh-golang binutils-i686-linux-gnu binutils-aarch64-linux-gnu \
        binutils-arm-linux-gnueabihf bash-completion zip ruby-dev lintian npm \
        libc6-dev-i386-cross libc6-dev-armhf-cross libc6-dev-arm64-cross \
        gcc-i686-linux-gnu gcc-arm-linux-gnueabihf gcc-aarch64-linux-gnu \
        faketime && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

RUN useradd -m --shell /bin/bash --home-dir /var/lib/aptly aptly
RUN sed -i 's/#force_color_prompt=yes/force_color_prompt=yes/' /var/lib/aptly/.bashrc

RUN mkdir /work
WORKDIR /work/src
RUN chown aptly /work

# install test dependencies
RUN cd /var/lib/aptly; git clone https://github.com/aptly-dev/aptly-fixture-db.git
RUN cd /var/lib/aptly; git clone https://github.com/aptly-dev/aptly-fixture-pool.git
RUN cd /var/lib/aptly; curl -O http://repo.aptly.info/system-tests/etcd.db.xz && xz -d etcd.db.xz
RUN echo "aptly ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/aptly
ADD system/t13_etcd/install-etcd.sh /src/
RUN /src/install-etcd.sh
RUN rm -rf /src
RUN npm install -g azurite
