FROM --platform=linux/arm64 docker.io/postgis/postgis:16-3.4

COPY ./compose/production/postgres/maintenance /usr/local/bin/maintenance
RUN chmod +x /usr/local/bin/maintenance/*
RUN mv /usr/local/bin/maintenance/* /usr/local/bin \
    && rmdir /usr/local/bin/maintenance

RUN echo "listen_addresses='*'" >> /etc/postgresql/postgresql.conf
RUN echo "host all all 0.0.0.0/0 md5" >> /etc/postgresql/pg_hba.conf
