ARG DISTRIBUTION
FROM debian:${DISTRIBUTION}-slim

ARG DISTRIBUTION
ARG SNAPCAST_VERSION
ARG TARGETPLATFORM

RUN apt-get update && \
  apt-get install -y curl && \
  curl -fsSLo /tmp/snapclient.deb \
    "https://github.com/badaix/snapcast/releases/download/v${SNAPCAST_VERSION}/snapclient_${SNAPCAST_VERSION}-1_${TARGETPLATFORM#linux/}_${DISTRIBUTION}.deb" && \
  apt-get install -y /tmp/snapclient.deb && \
  rm /tmp/snapclient.deb && \
  rm -rf /var/cache/apt/archives /var/lib/apt/lists/*

USER snapclient
ENTRYPOINT ["snapclient"]
