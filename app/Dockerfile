FROM ubuntu:16.04

WORKDIR /root

ENV PATH /root/local/go/bin:/root/go/bin:$PATH
ENV ISUBATA_DB_HOST db
ENV ISUBATA_DB_USER isucon
ENV ISUBATA_DB_PASSWORD isucon

RUN set -e \
	&& apt-get update -y \
	&& apt-get install git -y \
	&& git clone https://github.com/isucon/isucon7-qualify.git isubata \
	&& apt-get install -y git curl libreadline-dev pkg-config autoconf automake build-essential libmysqlclient-dev \
		libssl-dev python3 python3-dev python3-venv openjdk-8-jdk-headless libxml2-dev libcurl4-openssl-dev \
		libxslt1-dev re2c bison libbz2-dev libreadline-dev libssl-dev gettext libgettextpo-dev libicu-dev \
		libmhash-dev libmcrypt-dev libgd-dev libtidy-dev jq \
	&& git clone https://github.com/tagomoris/xbuild.git \
	&& mkdir local \
	&& xbuild/ruby-install   -f 2.4.2   /root/local/ruby \
	&& xbuild/perl-install   -f 5.26.1  /root/local/perl \
	&& xbuild/node-install   -f v6.11.4 /root/local/node \
	&& xbuild/go-install     -f 1.9     /root/local/go \
	&& xbuild/python-install -f 3.6.2   /root/local/python \
	&& xbuild/php-install    -f 7.1.9   /root/local/php -- --disable-phar --with-pcre-regex --with-zlib --enable-fpm --enable-pdo --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd --with-openssl --with-pcre-regex --with-pcre-dir --with-libxml-dir --enable-opcache --enable-bcmath --with-bz2 --enable-calendar --enable-cli --enable-shmop --enable-sysvsem --enable-sysvshm --enable-sysvmsg --enable-mbregex --enable-mbstring --with-mcrypt --enable-pcntl --enable-sockets --with-curl --enable-zip --with-pearAA \
	&& go get github.com/constabulary/gb/... \
	&& cd ~/isubata/bench \
	&& gb vendor restore \
	&& make

