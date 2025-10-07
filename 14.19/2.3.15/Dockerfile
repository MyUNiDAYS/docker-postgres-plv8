FROM postgres:14.19-bookworm

LABEL authors="Platform (platform.team@myunidays.com)"

ENV PLV8_VERSION=2.3.15 \
    PLV8_SHA256SUM="8a05f9d609bb79e47b91ebc03ea63b3f7826fa421a0ee8221ee21581d68cb5ba"

RUN buildDependencies="build-essential \
    ca-certificates \
    curl \
    git-core \
    python3 \
    gpp \
    cpp \
    pkg-config \
    apt-transport-https \
    cmake \
    libc++-dev \
    libc++abi-dev \
    libssl-dev \
    zlib1g-dev \
    libffi-dev \
    linux-libc-dev \
    clang-15 \
    llvm-15 \
    llvm-16 \
    make" \
  && postSetupDeps="postgresql-server-dev-$PG_MAJOR" \
  && runtimeDependencies="libc++1 \
    libc++abi1" \
  && apt-get update \
  && apt-get install -y --no-install-recommends ${buildDependencies}  \
  && cd /tmp \
  && curl -O https://www.python.org/ftp/python/2.7.18/Python-2.7.18.tgz \
  && tar -xzf Python-2.7.18.tgz \
  && cd Python-2.7.18 \
  && ./configure --enable-optimizations \
  && make -j$(nproc) \
  && make altinstall \
  && ln -s /usr/local/bin/python2.7 /usr/local/bin/python \
  && cd /tmp \
  && rm -rf Python-2.7.18* \
  && git config --global user.email "some.user@test.com" \
  && git config --global user.name "someuser" \
  && apt-get install -y --no-install-recommends ${postSetupDeps} \
  && mkdir -p /tmp/build \
  && curl -o /tmp/build/v$PLV8_VERSION.tar.gz -SL "https://github.com/plv8/plv8/archive/v${PLV8_VERSION}.tar.gz" \
  && cd /tmp/build \
  && echo $PLV8_SHA256SUM v$PLV8_VERSION.tar.gz | sha256sum -c \
  && tar -xzf /tmp/build/v$PLV8_VERSION.tar.gz -C /tmp/build/ \
  && cd /tmp/build/plv8-$PLV8_VERSION \
  && make static \
  && make install \
  && strip /usr/lib/postgresql/${PG_MAJOR}/lib/plv8-${PLV8_VERSION}.so \
  && rm -rf /root/.vpython_cipd_cache /root/.vpython-root \
  && apt-get clean \
  && apt-get remove -y ${buildDependencies} \
  && apt-get autoremove -y \
  && rm -rf /tmp/build /var/lib/apt/lists/*