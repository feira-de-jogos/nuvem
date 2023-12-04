# Serviços em nuvem

Ver https://boidacarapreta.gitbook.io/feira-de-jogos/integracao-entre-jogos-e-banco.

## Arquivos

root@TELE-Ederson-feira-de-jogos:/opt/github/feira-de-jogos/nuvem/api# cat .env 
PGDATABASE="feira"
PGPASSWORD="feira"
PGHOST="localhost"
PGUSER="feira"
PGPORT="5432"
PORT="3000"
GOOGLE_CLIENT_ID="***" # ID de credencial criado no GCP
COOKIE_SECRET="***" # Hash para gerar cookie

root@TELE-Ederson-feira-de-jogos:/opt/github/feira-de-jogos/nuvem/mqtt# cat .env 
MQTT_URI="mqtt://feira:feira@feira-de-jogos.sj.ifsc.edu.br"
PGDATABASE="feira"
PGPASSWORD="feira"
PGHOST="localhost"
PGUSER="feira"
PGPORT="5432"

# cat /etc/nginx/sites-enabled/default 
# site default

server {
  listen 80;
  listen [::]:80;
  server_name feira-de-jogos.sj.ifsc.edu.br;

  if ($host = feira-de-jogos.sj.ifsc.edu.br) {
    return 301 https://$host$request_uri;
  } # managed by Certbot

  return 404; # managed by Certbot
}

server {
  listen [::]:443 http2 ssl ipv6only=on; # managed by Certbot
  listen 443 http2 ssl; # managed by Certbot
  server_name feira-de-jogos.sj.ifsc.edu.br; # managed by Certbot
  
  ssl_certificate /etc/letsencrypt/live/feira-de-jogos.sj.ifsc.edu.br/fullchain.pem; # managed by Certbot
  ssl_certificate_key /etc/letsencrypt/live/feira-de-jogos.sj.ifsc.edu.br/privkey.pem; # managed by Certbot
  include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
  ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

  location /api/ {
    proxy_pass http://127.0.0.1:3000/api/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header Host $host;
  }

  location /mqtt/ {
    proxy_pass http://ip6-localhost:8080/;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "Upgrade";
    proxy_set_header Host $host;
  }

  root /opt/github/feira-de-jogos/nuvem/frontend;
  index index.html index.htm index.nginx-debian.html;
  location / {
    try_files $uri $uri/ =404;
  }
}

- `/etc/turnserver.conf`:

```
cli-password=ifsc

# Rede
listening-ip=0.0.0.0
listening-ip=::

# Transporte
listening-port=3478
min-port=49152
max-port=65535

# Logging
#syslog
#verbose

# Identificação do servidor
server-name=feira-de-jogos.sj.ifsc.edu.br
realm=feira-de-jogos.sj.ifsc.edu.br

# TLS
fingerprint
cert=/etc/letsencrypt/live/feira-de-jogos.sj.ifsc.edu.br/fullchain.pem
pkey=/etc/letsencrypt/live/feira-de-jogos.sj.ifsc.edu.br/privkey.pem
no-tlsv1
no-tlsv1_1

# Autenticação
lt-cred-mech
user=adcipt:adcipt20232
```

# cat /etc/systemd/system/api.service 
[Unit]
Description=Feira de jogos
Documentation=https://github.com/feira-de-jogos/nuvem
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/node servidor.js
WorkingDirectory=/opt/github/feira-de-jogos/nuvem/api
Restart=on-failure

[Install]
WantedBy=multi-user.target

# cat /etc/systemd/system/mqtt.service 
[Unit]
Description=Feira de jogos
Documentation=https://github.com/feira-de-jogos/nuvem
After=network.target

[Service]
Type=simple
ExecStart=/usr/bin/node assinante.js
WorkingDirectory=/opt/github/feira-de-jogos/nuvem/mqtt
Restart=on-failure

[Install]
WantedBy=multi-user.target
