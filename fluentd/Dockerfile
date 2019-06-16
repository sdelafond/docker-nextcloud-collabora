FROM fluent/fluentd:v1.5.0-debian-1.0
ARG GEOLITE_FLAVOR=GeoLite2-City
ARG GEOLITE_VERSION=20190611

# Use root account to use apk
USER root

RUN apt-get update -q
RUN apt-get install -y --no-install-recommends --no-install-suggests build-essential
RUN apt-get install -y --no-install-recommends --no-install-suggests ruby-dev
RUN apt-get install -y --no-install-recommends --no-install-suggests libgeoip-dev
RUN apt-get install -y --no-install-recommends --no-install-suggests autoconf
RUN apt-get install -y --no-install-recommends --no-install-suggests automake
RUN apt-get install -y --no-install-recommends --no-install-suggests libtool
RUN apt-get install -y --no-install-recommends --no-install-suggests wget
RUN apt-get clean

RUN gem install geoip2_c
RUN gem install fluent-plugin-geoip
RUN gem install fluent-plugin-elasticsearch
RUN gem install fluent-plugin-rewrite
RUN gem install fluent-plugin-rewrite-tag-filter
RUN gem sources --clear-all

RUN wget https://geolite.maxmind.com/download/geoip/database/${GEOLITE_FLAVOR}.tar.gz
RUN tar xavf ${GEOLITE_FLAVOR}.tar.gz ${GEOLITE_FLAVOR}_${GEOLITE_VERSION}/${GEOLITE_FLAVOR}.mmdb
RUN mv ${GEOLITE_FLAVOR}_${GEOLITE_VERSION}/${GEOLITE_FLAVOR}.mmdb /
RUN rm -fr ${GEOLITE_FLAVOR}.tar.gz ${GEOLITE_FLAVOR}_${GEOLITE_VERSION}

COPY conf/fluent.conf /fluentd/etc/

USER fluent
