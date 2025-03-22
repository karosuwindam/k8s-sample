FROM ubuntu:24.04

ENV FILEBEAT_VERSION 8.15.5
ENV FILEBEAT_BASE_URL https://artifacts.elastic.co/downloads/beats/filebeat/
# ENV ARCH x86_64
ENV ARCH arm64

RUN \
  apt-get update && apt-get upgrade -y && \
  # apt-get -y install systemd && \
  apt-get -y install wget && \
  apt-get -y install bash && \
  wget -q ${FILEBEAT_BASE_URL}filebeat-${FILEBEAT_VERSION}-linux-${ARCH}.tar.gz && \
  wget -q ${FILEBEAT_BASE_URL}filebeat-${FILEBEAT_VERSION}-linux-${ARCH}.tar.gz.sha512 && \
  sha512sum -c filebeat-${FILEBEAT_VERSION}-linux-${ARCH}.tar.gz.sha512 && \
  tar xzf filebeat-${FILEBEAT_VERSION}-linux-${ARCH}.tar.gz && \
  mv filebeat-${FILEBEAT_VERSION}-linux-${ARCH}/filebeat /usr/local/bin && \
  mkdir -p /etc/filebeat/conf.d && \
  mkdir -p /etc/filebeat/modules.d && \
  mkdir -p /etc/filebeat/log && \
  rm -rf /filebeat* && \
  rm -rf /var/lib/apt/lists/*

COPY filebeat.yml /etc/filebeat/filebeat.yml
RUN chmod go-w /etc/filebeat/filebeat.yml
COPY docker_app.yml /etc/filebeat/conf.d/docker_app.yml

# CMD ["/usr/local/bin/filebeat", "-e", "-c", "/etc/filebeat/filebeat.yml"]
CMD ["/usr/local/bin/filebeat","-e","-strict.perms=false","-c","/etc/filebeat/filebeat.yml"]