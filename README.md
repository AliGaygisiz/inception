# Inception

A WordPress site split across three Debian 11 containers, orchestrated with Docker Compose. Nginx handles HTTPS on 443 and passes PHP requests to WordPress running under PHP-FPM; WordPress reads and writes a MariaDB database. The three services share an internal bridge network, and only nginx is reachable from outside it.

I built this for 42's Inception project, but I set it up the way I'd set up a small real deployment. Every image is built from a local Dockerfile, so there's no prebuilt image hiding the interesting parts. Settings and credentials come from an untracked `srcs/.env`. Nginx generates its own self-signed certificate and RSA key on first boot. WordPress files and the database live on named volumes bound to `~/data`, so `make down` doesn't erase them, and all three containers come back automatically after a restart.

## Getting it running

Create `srcs/.env` with these keys:

```
db_name=
db_user=
db_pwd=
db_root_pwd=
DOMAIN_NAME=
WP_TITLE=
WP_ADMIN_USR=
WP_ADMIN_PWD=
WP_ADMIN_EMAIL=
WP_USR=
WP_EMAIL=
WP_PWD=
```

Set `DOMAIN_NAME` to the domain you're using. The `server_name` in the nginx config and the certificate CN are both set to `agaygisi.42.fr`, so change those too if this is going anywhere other than 42.

Then:

```
make           # create the host data directories and start the stack
make status    # see what's running
make stop      # stop containers without removing them
make start     # bring stopped containers back
make down      # remove containers; volumes stay on disk
```

`make` creates `~/data/wordpress` and `~/data/mariadb` before starting anything. You need Docker and Compose v1 (`docker-compose`) installed. Once it's up, the site is at `https://<DOMAIN_NAME>/`.

## Where things are

`srcs/docker-compose.yml` defines the services, network, and volumes. Under `srcs/requirements/`, each service has its own Dockerfile and startup files: nginx has the site config and a script that creates the certificate, wordpress has the PHP-FPM pool config and a WP-CLI bootstrap that installs core and creates the users, and mariadb has a server config plus an init SQL file.

I kept this to a single host with no CI or health checks. The exercise was about containers, networking, and persistence, so that's where the detail is.
