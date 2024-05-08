FROM debian:11

RUN apt -y update && apt install -y nginx && apt install openssl -y

COPY ./conf/default /etc/nginx/sites-available/

COPY ./tools/nginx_script.sh /var/www

RUN chmod +x /var/www/nginx_script.sh

ENTRYPOINT ["var/www/nginx_script.sh"]

CMD ["nginx", "-g", "daemon off;"]
