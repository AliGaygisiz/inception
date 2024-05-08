FROM debian:11


RUN apt update -y 

RUN apt install -y php7.4 php-mysqli php-fpm wget


COPY ./conf/www.conf /etc/php/7.4/fpm/pool.d

COPY ./tools /var/www/

RUN chmod +x /var/www/wp_script.sh

ENTRYPOINT [ "/var/www/wp_script.sh" ]

CMD ["php-fpm7.4", "--nodaemonize"]