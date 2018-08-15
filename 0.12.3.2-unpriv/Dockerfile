FROM debian:stable-slim

LABEL maintainer="Maksym Pugach <pugach.m@gmail.com>"

RUN mkdir -p /dash

WORKDIR /dash

ENV GOSU_VERSION=1.10 \
    DASH_VERSION=0.12.3.2

RUN useradd -r dashcoin \
 && apt-get update -y \
 && apt-get install -y curl gnupg \
 && gpg --keyserver pgp.mit.edu --recv-keys "B42F6819007F00F88E364FD4036A9C25BF357DD4" \
 || gpg --keyserver keyserver.pgp.com --recv-keys "B42F6819007F00F88E364FD4036A9C25BF357DD4" \
 || gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "B42F6819007F00F88E364FD4036A9C25BF357DD4" \
 || gpg --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "B42F6819007F00F88E364FD4036A9C25BF357DD4" \
 && curl https://keybase.io/udjinm6/pgp_keys.asc | gpg --import \
 && curl -OfSL https://github.com/dashpay/dash/releases/download/v${DASH_VERSION}/dashcore-${DASH_VERSION}-x86_64-linux-gnu.tar.gz \
 && curl -OfSL https://github.com/dashpay/dash/releases/download/v${DASH_VERSION}/dashcore-${DASH_VERSION}-x86_64-linux-gnu.tar.gz.asc \
 && gpg --verify dashcore-${DASH_VERSION}-x86_64-linux-gnu.tar.gz.asc \
 && tar -xzvf dashcore-${DASH_VERSION}-x86_64-linux-gnu.tar.gz -C . --strip-components=1 \
 && rm -rf share/ dashcore-${DASH_VERSION}-x86_64-linux-gnu.tar.gz* \
 && curl -o /usr/local/bin/gosu -fSL https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-$(dpkg --print-architecture) \
 && curl -o /usr/local/bin/gosu.asc -fSL https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-$(dpkg --print-architecture).asc \
 && gpg --verify /usr/local/bin/gosu.asc \
 && rm /usr/local/bin/gosu.asc \
 && apt-get purge gnupg -y \
 && apt-get autoremove -y \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
 && chmod +x /usr/local/bin/gosu

ENV DASH_DATA="/home/dash/.dash" PATH="/dash/bin:$PATH"

COPY docker-entrypoint.sh /entrypoint.sh

RUN chmod u+x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]

CMD ["dashd"]
