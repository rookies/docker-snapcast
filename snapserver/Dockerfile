ARG DISTRIBUTION
FROM rust:slim-${DISTRIBUTION} AS librespotbuilder

ARG LIBRESPOT_VERSION

RUN apt-get update && \
  apt-get install -y libssl-dev pkg-config git && \
  mkdir /src && \
  cd /src && \
  git init && \
  git fetch --depth=1 "https://github.com/librespot-org/librespot.git" "${LIBRESPOT_VERSION}" && \
  git -c advice.detachedHead=false checkout FETCH_HEAD && \
  cargo install --no-default-features --features "native-tls" --path .


ARG DISTRIBUTION
FROM debian:${DISTRIBUTION}-slim

ARG DISTRIBUTION
ARG SNAPCAST_VERSION
ARG SNAPWEB_VERSION
ARG TARGETPLATFORM

COPY --from=librespotbuilder /usr/local/cargo/bin/librespot /usr/bin/librespot
RUN apt-get update && \
  apt-get install -y curl && \
  curl -fsSLo /tmp/snapserver.deb \
    "https://github.com/snapcast/snapcast/releases/download/v${SNAPCAST_VERSION}/snapserver_${SNAPCAST_VERSION}-1_${TARGETPLATFORM#linux/}_${DISTRIBUTION}.deb" && \
  apt-get install -y /tmp/snapserver.deb && \
  curl -fsSLo /tmp/snapweb.deb \
    "https://github.com/snapcast/snapweb/releases/download/v${SNAPWEB_VERSION}/snapweb_${SNAPWEB_VERSION}-1_all.deb" && \
  apt-get install -y /tmp/snapweb.deb && \
  rm /tmp/snapserver.deb /tmp/snapweb.deb && \
  rm -rf /var/cache/apt/archives /var/lib/apt/lists/*

USER snapserver
ENTRYPOINT ["snapserver"]
