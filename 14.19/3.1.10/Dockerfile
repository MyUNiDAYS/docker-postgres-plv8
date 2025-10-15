FROM postgres:14.19-bookworm

LABEL authors="Platform Team (platform.team@myunidays.com)"

ENV PLV8_VERSION=3.1.10 \
    PLV8_SHA256SUM="7096c3290928561f0d4901b7a52794295dc47f6303102fae3f8e42dd575ad97d"

ENV GYP_CHROMIUM_NO_ACTION=0

RUN buildDependencies="build-essential \
    ninja-build \
    ca-certificates \
    curl \
    git-core \
    python3 \
    python-is-python3 \
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
    clang \
    llvm-15 \
    llvm-16 \
    make" \
  && postSetupDeps="postgresql-server-dev-$PG_MAJOR" \
  && runtimeDependencies="libc++1 \
    libtinfo5 \
    libc++abi1" \
  && apt-get update \
  && apt-get install -y --no-install-recommends ${buildDependencies} ${runtimeDependencies} \
  && git config --global user.email "some.user@test.com" \
  && git config --global user.name "someuser" \
  && apt-get install -y --no-install-recommends ${postSetupDeps} \
  && mkdir -p /tmp/build \
  && curl -o /tmp/build/v$PLV8_VERSION.tar.gz -SL "https://github.com/plv8/plv8/archive/v${PLV8_VERSION}.tar.gz" \
  && cd /tmp/build \
  && echo $PLV8_SHA256SUM v$PLV8_VERSION.tar.gz | sha256sum -c \
  && tar -xzf /tmp/build/v$PLV8_VERSION.tar.gz -C /tmp/build/ \
  && cd /tmp/build/plv8-$PLV8_VERSION \
  && make static DOCKER=1  \
  && make install DOCKER=1 \
  && strip /usr/lib/postgresql/${PG_MAJOR}/lib/plv8-${PLV8_VERSION}.so \
  && rm -rf /root/.vpython_cipd_cache /root/.vpython-root \
  && apt-get clean \
  && apt-get remove -y ${buildDependencies} \
  && apt-get autoremove -y \
  && rm -rf /tmp/build /var/lib/apt/lists/*