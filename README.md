# Introduction
Setting up parse-server and the dashboard which requires a valid cert, which requires letsencrypt and a reverse proxy like `nginx` or `apache` takes too much time. Using `docker` and `traefik` is so much easier.

`Traefik` will watch any docker container using the `labels` set in the configuration file.
If a new container is found, it will grab the cert automatically. 

# Instruction
Clone the repo
```
git clone https://github.com/seeya/traefik-parseserver.git
cd traefik-parseserver
```

Configure the following files to suit your needs
### Traefik/traefik.toml
The `domain` in `Trafik/traefik.toml` should be the same as its other `subdomain`.

### Parse/docker-compose.yml

```
13. PARSE_SERVER_DATABASE_URI=mongodb://mongo:27017/yourdatabase

25. - "traefik.basic.frontend.rule=Host:subdomain.example.com"

36. - PARSE_DASHBOARD_SERVER_URL=https://subdomain.example/parse
37. - PARSE_DASHBOARD_MASTER_KEY=MASTER_KEY
38. - PARSE_DASHBOARD_APP_ID=APP_ID
39. - PARSE_DASHBOARD_APP_NAME=APPNAME
40. - PARSE_DASHBOARD_USER_ID=CHANGE_THIS
41. - PARSE_DASHBOARD_USER_PASSWORD=CHANGE_THIS

51. - "traefik.basic.frontend.rule=Host:dashboard.example.com" 
```

Once configured, start up `traefik` first and then the `parse-server`.

```
cd Traefik
docker-compose up -d
```

```
cd Parse
docker-compose up -d
```

Make sure to allow incoming request on both port `80` and `443`
```
ufw allow http
ufw allow https
```

# References
Look for the environment variables here:
```
https://hub.docker.com/r/parseplatform/parse-server/
https://hub.docker.com/r/parseplatform/parse-dashboard/
```
