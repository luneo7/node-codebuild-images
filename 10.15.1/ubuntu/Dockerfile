FROM ubuntu:14.04.5

# Install git, SSH, and other utilities
RUN set -ex \
    && echo 'Acquire::CompressionTypes::Order:: "gz";' > /etc/apt/apt.conf.d/99use-gzip-compression \
    && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF \
    && apt-get update \
    && apt-get install software-properties-common openssh-client=1:6.6* -y --no-install-recommends \
    && apt install -y apt-transport-https \
    && apt-add-repository ppa:git-core/ppa \
    && apt-get update \
    && mkdir ~/.ssh \
    && touch ~/.ssh/known_hosts \
    && ssh-keyscan -t rsa,dsa -H github.com >> ~/.ssh/known_hosts \
    && ssh-keyscan -t rsa,dsa -H bitbucket.org >> ~/.ssh/known_hosts \
    && chmod 600 ~/.ssh/known_hosts \
    && apt-get install -y --no-install-recommends \
       git=1:2.* wget=1.15-* python3=3.4.* python3.4-dev=3.4.* fakeroot=1.20-* ca-certificates jq \
       tar=1.27.* gzip=1.6-* zip=3.0-* autoconf=2.69-* automake=1:1.14.* \
       bzip2=1.0.* file=1:5.14-* g++=4:4.8.* gcc=4:4.8.* imagemagick=8:6.7.* \
       libbz2-dev=1.0.* libc6-dev=2.19-* libcurl4-openssl-dev=7.35.* libdb-dev=1:5.3.* \
       libevent-dev=2.0.* libffi-dev=3.1~* libgeoip-dev=1.6.* libglib2.0-dev=2.40.* \
       libjpeg-dev=8c-* libkrb5-dev=1.12+* liblzma-dev=5.1.* \
       libmagickcore-dev=8:6.7.* libmagickwand-dev=8:6.7.* libmysqlclient-dev=5.5.* \
       libncurses5-dev=5.9+* libpng12-dev=1.2.* libpq-dev=9.3.* libreadline-dev=6.3-* \
       libsqlite3-dev=3.8.* libssl-dev=1.0.* libtool=2.4.* libwebp-dev=0.4.* \
       libxml2-dev=2.9.* libxslt1-dev=1.1.* libyaml-dev=0.1.* make=3.81-* \
       patch=2.7.* xz-utils=5.1.* zlib1g-dev=1:1.2.* unzip=6.0-* curl=7.35.* \
       e2fsprogs=1.42.* iptables=1.4.* xfsprogs=3.1.*\
       less=458-* groff=1.22.* liberror-perl=0.17-* \
       asciidoc=8.6.* build-essential=11.* bzr=2.6.* cvs=2:1.12.* cvsps=2.1-* docbook-xml=4.5-* docbook-xsl=1.78.* dpkg-dev=1.17.* \
       libdbd-sqlite3-perl=1.40-* libdbi-perl=1.630-* libdpkg-perl=1.17.* libhttp-date-perl=6.02-* \
       libio-pty-perl=1:1.08-* libserf-1-1=1.3.* libsvn-perl=1.8.* libsvn1=1.8.* libtcl8.6=8.6.* libtimedate-perl=2.3000-* \
       libunistring0=0.9.* libxml2-utils=2.9.* libyaml-perl=0.84-* python-bzrlib=2.6.* python-configobj=4.7.* \
       sgml-base=1.26+* sgml-data=2.0.* subversion=1.8.* tcl=8.6.* tcl8.6=8.6.* xml-core=0.13+* xmlto=0.0.* xsltproc=1.1.* python3-pip \
       tk=8.6.* gettext=0.18.* gettext-base=0.18.* libapr1=1.5.* libaprutil1=1.5.* libasprintf0c2=0.18.*  \
    && apt-get install -y libgtk2.0-0 libnotify-dev libgconf-2-4 libnss3 libxss1 libasound2 xvfb \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

ENV NODE_VERSION 10.15.1

RUN ARCH= && dpkgArch="$(dpkg --print-architecture)" \
  && case "${dpkgArch##*-}" in \
    amd64) ARCH='x64';; \
    ppc64el) ARCH='ppc64le';; \
    s390x) ARCH='s390x';; \
    arm64) ARCH='arm64';; \
    armhf) ARCH='armv7l';; \
    i386) ARCH='x86';; \
    *) echo "unsupported architecture"; exit 1 ;; \
  esac \
  && set -ex \
  # gpg keys listed at https://github.com/nodejs/node#release-keys
  && for key in \
    94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
    FD3A5288F042B6850C66B31F09FE44734EB7990E \
    71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
    DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
    C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
    B9AE9905FFD7803F25714661B63B535A4C206CA9 \
    77984A986EBC2AA786BC0F66B01FBB92821C587A \
    8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
    4ED778F539E3634C779C87C6D7062848A1AB005C \
    A48C2BEE680E841632CD4E44F07496B3EB3C1762 \
    B9E2F5981AA6E0CD28160D9FF13993A75599653C \
  ; do \
    gpg --batch --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" || \
    gpg --batch --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys "$key" || \
    gpg --batch --keyserver hkp://pgp.mit.edu:80 --recv-keys "$key" ; \
  done \
  && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-$ARCH.tar.xz" \
  && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-$ARCH.tar.xz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xJf "node-v$NODE_VERSION-linux-$ARCH.tar.xz" -C /usr/local --strip-components=1 --no-same-owner \
  && rm "node-v$NODE_VERSION-linux-$ARCH.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs \
  && rm -rf /var/lib/apt/lists/* \
  && apt-get clean

RUN set -ex \
    && pip3 install awscli boto3

ENV YARN_VERSION 1.13.0

RUN set -ex \
  && for key in \
    6A010C5166006599AA17F08146C2130DFD2497F5 \
  ; do \
    gpg --batch --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" || \
    gpg --batch --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys "$key" || \
    gpg --batch --keyserver hkp://pgp.mit.edu:80 --recv-keys "$key" ; \
  done \
  && curl -fsSLO --compressed "https://yarnpkg.com/downloads/$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz" \
  && curl -fsSLO --compressed "https://yarnpkg.com/downloads/$YARN_VERSION/yarn-v$YARN_VERSION.tar.gz.asc" \
  && gpg --batch --verify yarn-v$YARN_VERSION.tar.gz.asc yarn-v$YARN_VERSION.tar.gz \
  && mkdir -p /opt \
  && tar -xzf yarn-v$YARN_VERSION.tar.gz -C /opt/ \
  && ln -s /opt/yarn-v$YARN_VERSION/bin/yarn /usr/local/bin/yarn \
  && ln -s /opt/yarn-v$YARN_VERSION/bin/yarnpkg /usr/local/bin/yarnpkg \
  && rm yarn-v$YARN_VERSION.tar.gz.asc yarn-v$YARN_VERSION.tar.gz

RUN npm set unsafe-perm true

CMD [ "node" ]