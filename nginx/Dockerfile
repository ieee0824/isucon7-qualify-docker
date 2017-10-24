FROM ubuntu:16.04 AS nginx

WORKDIR /root

RUN set -e \
	&& apt-get update -y \
	&& apt-get install -y nginx git \
	&& git clone https://github.com/isucon/isucon7-qualify.git isubata

FROM nginx:1.10.3

RUN set -e \
	&& rm -rf /etc/nginx

COPY --from=nginx /root/isubata/files/app/nginx.conf /etc/nginx/sites-available/nginx.conf
COPY --from=nginx /root/isubata/files/app/nginx.php.conf /etc/nginx/sites-available/nginx.php.conf
COPY --from=nginx /root/isubata/webapp/public /var/www/public

COPY --from=nginx /etc/nginx /etc/nginx

RUN set -e \
	&& chown -R www-data /var/www \
	&& cd /etc/nginx/sites-enabled \
	&& unlink default \
	&& ln -s ../sites-available/nginx.conf \
	&& sed -i -e 's/127\.0\.0\.1/app/g' /etc/nginx/sites-available/nginx.conf \
	&& sed -i -e 's/\/home\/isucon\/isubata\/webapp\/public/\/var\/www\/public/g' /etc/nginx/sites-available/nginx.conf
