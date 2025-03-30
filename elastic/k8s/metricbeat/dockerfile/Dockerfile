FROM ubuntu:24.04

ENV METRICBEAT_VERSION 8.15.5
ENV METRICBEAT_BASE_URL https://artifacts.elastic.co/downloads/beats/metricbeat/
# ENV ARCH x86_64
ENV ARCH arm64

RUN \
  apt-get update && apt-get upgrade -y && \
  apt-get -y install wget && \
  apt-get -y install bash && \
  apt-get -y install curl
RUN  \
  mkdir -p //usr/share/metricbeat/ && \
  wget -q ${METRICBEAT_BASE_URL}metricbeat-${METRICBEAT_VERSION}-linux-${ARCH}.tar.gz && \
  wget -q ${METRICBEAT_BASE_URL}metricbeat-${METRICBEAT_VERSION}-linux-${ARCH}.tar.gz.sha512 && \
  sha512sum -c metricbeat-${METRICBEAT_VERSION}-linux-${ARCH}.tar.gz.sha512 && \
  tar xzf metricbeat-${METRICBEAT_VERSION}-linux-${ARCH}.tar.gz && \
  mv metricbeat-${METRICBEAT_VERSION}-linux-${ARCH}/* //usr/share/metricbeat && \
  mkdir -p //usr/share/metricbeat/log && \
  mkdir -p //usr/share/metricbeat/data && \
  chmod 777 //usr/share/metricbeat/data && \
  chmod 777 //usr/share/metricbeat/log && \
  # mkdir -p /etc/metricbeat/conf.d && \
  rm -rf /metricbeat* && \
  rm -rf /var/lib/apt/lists/*

WORKDIR /usr/share/metricbeat
# COPY docker_app.yml /etc/metricbeat/conf.d/docker_app.yml
RUN chmod 644 /usr/share/metricbeat/metricbeat.yml && \
    chmod go-w /usr/share/metricbeat/metricbeat.yml
RUN usermod -l metricbeat ubuntu && \
    usermod -d /home/metricbeat -m metricbeat && \
    groupmod -n metricbeat ubuntu

USER 1000
CMD ["./metricbeat","-e","-strict.perms=false" ]