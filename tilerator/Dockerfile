FROM node:8-stretch@sha256:fec2d6c325686988c4e0bf1b17cd32954422333d24ed6a97c28c8570054a6e1d

# ENV NODE_ENV=production
ENV NODE_ENV=development

RUN apt-get update && \
    apt-get install -y \
        git \
        unzip \
        curl \
        libpq-dev \
        libproj-dev \
        liblua5.2-dev \
        libgeos++-dev \
        nmap \
        netcat \
        redis-tools \
    && npm i npm@latest -g

RUN git clone https://github.com/QwantResearch/tilerator.git /opt/tilerator \
    && cd /opt/tilerator \
    && git checkout 64b69c064aae15d474bd61da2c6bc35140f118f8 \
    # && npm install --production
    && npm install

RUN git clone https://github.com/hyperion-hyn/kartotherian_config.git /opt/kartotherian_config \
    && cd /opt/kartotherian_config \
    && git checkout 8fe1dd8c317c90ed8aa4fd509e13a4d01c6643fb

COPY config*.yaml /opt/tilerator/
COPY gen_tiles.sh /gen_tiles.sh

RUN mkdir -p /etc/tilerator
RUN ln -sf /opt/kartotherian_config/tilerator/data_tm2source_base.yml /etc/tilerator

RUN chmod +x /gen_tiles.sh \
    && ln -sf /opt/tilerator/config*.yaml /etc/tilerator \
    && ln -sf /opt/kartotherian_config/tilerator/sources.yaml /etc/tilerator \
    && ln -sf /opt/tilerator/dist/init-scripts/cassandra.wait /usr/local/bin

COPY runserver.sh /runserver.sh
RUN chmod +x /runserver.sh

ENV TILERATOR_OSMDB_HOST=postgres
ENV TILERATOR_OSMDB_USER=gis
ENV TILERATOR_OSMDB_PSWD=
ENV TILERATOR_CASSANDRA_SERVERS=cassandra
ENV TILERATOR_CASSANDRA_USER=gis
ENV TILERATOR_CASSANDRA_PSWD=

CMD ["/runserver.sh"]
