FROM python:3-alpine3.6

ENV PGADMIN4_VERSION 2.0

# Metadata
LABEL org.label-schema.name="pgAdmin4" \
      org.label-schema.version="$PGADMIN4_VERSION" \
      org.label-schema.license="PostgreSQL" \
      org.label-schema.url="https://www.pgadmin.org" \
      org.label-schema.vcs-url="https://github.com/fenglc/dockercloud-pgAdmin4"

RUN set -ex \
	&& apk add --no-cache --virtual .run-deps \
		bash \
		postgresql \
		postgresql-libs \
	&& apk add --no-cache --virtual .build-deps \
		gcc \
		musl-dev \
		openssl \
		postgresql-dev \
	&& pip --no-cache-dir install \
		https://ftp.postgresql.org/pub/pgadmin/pgadmin4/v2.0/pip/pgadmin4-2.0-py2.py3-none-any.whl \
	&& apk del .build-deps

#USER pgadmin4:pgadmin4
RUN mkdir -p /var/lib/pgadmin4 && chown -R 998:998 /var/lib/pgadmin4 \
    && mkdir -p /var/log/pgadmin && chown -R 998:998 /var/log/pgadmin \
    && chown -R 998:998 "$(python -c 'import os; print(os.path.dirname(os.__file__))')/site-packages/pgadmin4"
VOLUME /var/lib/pgadmin4

COPY docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

USER 998:998

EXPOSE 5050
CMD ["pgadmin4"]
