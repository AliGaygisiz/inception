FROM debian:11

RUN apt-get update -y && apt-get install mariadb-server -y

EXPOSE 3306

COPY ./tools/50-server.cnf /etc/mysql/mariadb.conf.d/

COPY ./tools/db_init.sql /var/www/

RUN sed -i "s/{db_name}/$db_name/g" /var/www/db_init.sql
RUN sed -i "s/{db_user}/$db_user/g" /var/www/db_init.sql
RUN sed -i "s/{db_pwd}/$db_pwd/g" /var/www/db_init.sql

RUN service mariadb start && mysql < /var/www/db_init.sql && rm -f /var/www/db_init.sql;

CMD ["mysqld"]
