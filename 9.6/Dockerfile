FROM postgres:9.6.6

LABEL Author="Chia-liang Kao <clkao@clkao.org>" 




ENV PLV8_VERSION=v1.5.3 \
    PLV8_SHASUM="fac8052c926c9ece74f655500caeca50552c0c4b4c7081c0c7946e06ed114d1c  v1.5.3.tar.gz"


RUN buildDependencies="build-essential \
    ca-certificates \
    curl \
    linux-libc-dev \
    libpci3 \
    git-core \
    postgresql-server-dev-$PG_MAJOR \
    zip \
    xcompmgr \
    x11-utils \
    subversion \
    ruby \
    python-yaml \
    python-psutil \
    python-openssl \
    python-opencv \
    python-numpy \
    python-crypto \
    python-cherrypy3 \
    php5-cgi \
    p7zip \
    openbox \
    libxtst-dev \
    libxtst6 \
    libxt-dev \
    libxss-dev \
    libxslt1-dev \
    libxrender1 \
    libxrandr2 \
    libxkbcommon-dev \
    libxinerama1 \
    libxi6 \
    libxfixes3 \
    libxext6 \
    libxdmcp6 \
    libxdamage1 \
    libxcursor1 \
    libxcomposite1 \
    libxcb1 \
    libxau6 \
    libx11-xcb1 \
    libx11-6 \
    libwww-perl \
    libudev-dev \
    libssl-dev \
    libsqlite3-dev \
    libspeechd-dev \
    libspeechd2 \
    libsctp-dev \
    libpulse-dev \
    libpulse0 \
    libpixman-1-0 \
    libpci-dev \
    libpango1.0-0 \
    libnss3-dev \
    libnss3 \
    libnspr4-dev \
    libnspr4  \
    libkrb5-dev \
    libjpeg-dev \
    libgtk-3-dev \
    libgtk-3-0 \
    libgtk2.0-dev \
    libgtk2.0-0 \
    libgnome-keyring-dev \
    libgnome-keyring0 \
    libglu1-mesa-dev \
    libglib2.0-dev \
    libglib2.0-0 \
    libgbm-dev \
    libfreetype6 \
    libfontconfig1 \
    libffi-dev \
    libelf-dev \
    libdrm-dev \
    libcurl4-gnutls-dev \
    libcups2-dev \
    libcups2 \
    libcap-dev \
    libcairo2-dev \
    libcairo2 \
    libc6-i386 \
    libbz2-dev \
    libbrlapi-dev \
    libbrlapi0.6 \
    libbluetooth-dev \
    libav-tools \
    libatk1.0-0 \
    libasound2-dev \
    libasound2 \
    libappindicator-dev \
    libappindicator3-dev \
    libappindicator3-1 \
    libappindicator1 \
    lib32stdc++6 \
    lib32gcc1 \
    gperf \
    fonts-ipafont \
    elfutils \
    dbus-x11 \
    cdbs \
    binutils-mipsel-linux-gnu \
    binutils-mips64el-linux-gnuabi64 \
    binutils-arm-linux-gnueabihf \
    binutils-aarch64-linux-gnu \
    apache2.2-bin" \
  && apt-get update \
  && apt-get install -y --no-install-recommends ${buildDependencies} \
  && mkdir -p /tmp/build \
  && cd /tmp/build \
  && git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git \
  && export PATH=$PATH:/tmp/build/depot_tools \
  && fetch v8 \
  && cd v8 \
  && gclient sync \
  && ./build/install-build-deps.sh --quick-check --no-prompt --no-syms  --no-nacl --no-arm --no-chromeos-fonts\
  && tools/dev/v8gen.py x64.release \
  && ninja -C out.gn/x64.release \
  && cd .. \
  && curl -o /tmp/build/${PLV8_VERSION}.tar.gz -SL "https://github.com/plv8/plv8/archive/$PLV8_VERSION.tar.gz" \
  && echo ${PLV8_SHASUM} | sha256sum -c \
  && tar -xzf /tmp/build/${PLV8_VERSION}.tar.gz -C /tmp/build/ \
  && cd /tmp/build/plv8-${PLV8_VERSION#?} \
  && make install \
  && strip /usr/lib/postgresql/${PG_MAJOR}/lib/plv8.so \
  && cd / \
  && apt-get clean \
  && apt-get remove -y  ${buildDependencies} \
  && apt-get autoremove -y \
  && rm -rf /tmp/build /var/lib/apt/lists/*