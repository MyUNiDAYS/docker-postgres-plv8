FROM postgres:14.10

LABEL Maintainer="Robert Davies <robert.davies@myunidays.com>" 

ENV PLV8_VERSION=v3.0.0 \
    PLV8_SHASUM="b78b4e0e442c8a7d0c34c85b1b8c1c5e5c5f5c5f5c5f5c5f5c5f5c5f5c5f5c5f  v3.0.0.tar.gz"


RUN buildDependencies="build-essential \
    ca-certificates \
    curl \
    git-core \
	python3 \
    postgresql-server-dev-$PG_MAJOR" \
  && apt-get update \
  && apt-get install -y --no-install-recommends ${buildDependencies} \
  && mkdir -p /tmp/build \
  && curl -o /tmp/build/${PLV8_VERSION}.tar.gz -SL "https://github.com/plv8/plv8/archive/$PLV8_VERSION.tar.gz" \
  && cd /tmp/build \
  && echo ${PLV8_SHASUM} | sha256sum -c \
  && tar -xzf /tmp/build/${PLV8_VERSION}.tar.gz -C /tmp/build/ \
  && cd /tmp/build/plv8-${PLV8_VERSION#?} \
  && make static \
  && make install \
  && strip /usr/lib/postgresql/${PG_MAJOR}/lib/plv8.so \
  && cd / \
  && apt-get clean \
  && apt-get remove -y  ${buildDependencies} \
  && apt-get autoremove -y \
  && rm -rf /tmp/build /var/lib/apt/lists/*


    