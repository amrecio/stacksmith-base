FROM bitnami/minideb@sha256:baadc2ac789b69452d9f19c3d641908b42373a0db014901c16f4b1631e5d05eb
MAINTAINER Bitnami <containers@bitnami.com>

RUN apt-get update && \
  DEBIAN_FRONTEND=noninteractive apt-get install --no-install-recommends -y curl ca-certificates cron sudo locales procps libaio1 libssl1.0.0 netcat-traditional && \
  useradd -ms /bin/bash bitnami && \
  mkdir -p /opt/bitnami && chown bitnami:bitnami /opt/bitnami && \
  sed -i -e 's/\s*Defaults\s*secure_path\s*=/# Defaults secure_path=/' /etc/sudoers && \
  echo "bitnami ALL=NOPASSWD: ALL" >> /etc/sudoers && \
  update-locale LANG=C.UTF-8 LC_MESSAGES=POSIX && \
  locale-gen en_US.UTF-8 && \
  DEBIAN_FRONTEND=noninteractive dpkg-reconfigure locales && \
  apt-get clean && \
  rm -rf /var/lib/apt /var/cache/apt/archives/* /tmp/*

ENV NAMI_VERSION 0.0.5-0

RUN cd /tmp && \
  curl -sSLO https://nami-prod.s3.amazonaws.com/tools/nami/releases/nami-$NAMI_VERSION-linux-x64.tar.gz && \
  echo "efc372046ce476984e7074ec0d91f1d57d050ea08ac5a524e8b46217b754963c  nami-$NAMI_VERSION-linux-x64.tar.gz" | sha256sum -c - && \
  mkdir -p /opt/bitnami/nami && \
  tar xzf nami-$NAMI_VERSION-linux-x64.tar.gz --strip 1 -C /opt/bitnami/nami && \
  rm nami-$NAMI_VERSION-linux-x64.tar.gz

ENV TINI_VERSION v0.9.0

RUN cd /tmp && \
  curl -sSLO https://github.com/krallin/tini/releases/download/$TINI_VERSION/tini && \
  curl -sSLO https://github.com/krallin/tini/releases/download/$TINI_VERSION/tini.asc && \
  gpg --keyserver ha.pool.sks-keyservers.net --recv-keys 595E85A6B1B4779EA4DAAEC70B588DFF0527A9B7 && \
  gpg --verify tini.asc && \
  chmod +x tini && \
  mv tini /usr/local/bin/ && \
  rm tini.asc

ENV PATH=/opt/bitnami/nami/bin:$PATH

COPY rootfs /
ENTRYPOINT ["/entrypoint.sh"]
