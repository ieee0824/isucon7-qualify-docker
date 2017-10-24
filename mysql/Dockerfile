FROM mysql:5.7

WORKDIR /root

ENV PATH /tmp/local/go/bin:/root/go/bin:$PATH

RUN set -e \
	&& apt-get update -y \
	&& apt-get install git -y \
	&& apt-get install -y git curl libreadline-dev pkg-config autoconf automake build-essential libmysqlclient-dev \
		libssl-dev python3 python3-dev python3-venv libxml2-dev libcurl4-openssl-dev \
		libxslt1-dev re2c bison libbz2-dev libreadline-dev libssl-dev gettext libgettextpo-dev libicu-dev \
		libmhash-dev libmcrypt-dev libgd-dev libtidy-dev jq make \
	&& cd /tmp \
	&& git clone https://github.com/isucon/isucon7-qualify.git isubata\
	&& git clone https://github.com/tagomoris/xbuild.git \
	&& mkdir local \
	&& xbuild/go-install     -f 1.9     /tmp/local/go \
	&& go get github.com/constabulary/gb/... \
	&& cd /tmp/isubata/bench \
	&& gb vendor restore \
	&& make


RUN set -e \
	&& chmod +x /tmp/isubata/bench/bin/gen-initial-dataset \
	&& echo "cd /tmp/isubata/bench" >> /docker-entrypoint-initdb.d/init.sh \
	&& echo "/tmp/isubata/bench/bin/gen-initial-dataset" >> /docker-entrypoint-initdb.d/init.sh \
	&& echo "cd /tmp/isubata" >> /docker-entrypoint-initdb.d/init.sh \
	&& echo "bash /tmp/isubata/db/init.sh" >> /docker-entrypoint-initdb.d/init.sh \
	&& echo "mysql -uroot -e \"CREATE USER isucon@'%' IDENTIFIED BY 'isucon'; GRANT ALL on *.* TO isucon@'%'; CREATE USER isucon@'localhost' IDENTIFIED BY 'isucon'; GRANT ALL on *.* TO isucon@'localhost';\"" >> /docker-entrypoint-initdb.d/init.sh \
	&& echo "zcat /tmp/isubata/bench/isucon7q-initial-dataset.sql.gz | mysql -uroot --default-character-set=utf8 isubata" >> /docker-entrypoint-initdb.d/init.sh \
	&& chmod -R 777 /tmp/isubata/bench


WORKDIR /
