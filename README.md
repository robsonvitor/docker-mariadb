# Container com MariaDB Server

- [ ] Renomeie o arquivo .env-example para .env e edite ele conforme seu ambiente
- [ ] Edite o arquivo my.cnf conforme seu ambiente
- [ ] Execute o projeto ```docker compose up```

##  Gerenciar o serviço via systemd

### Copiar o arquivo de controle do serviço para o diretório do systemd
```cp ./extras/docker-mariadb.service /etc/systemd/system/docker-mariadb.service```

### Daemon reconhecer o novo serviço
```systemctl daemon-reload```

### Habilita o serviço
```systemctl enable docker-mariadb.service```

### Controlar o serviço
```systemctl {start|stop|restart} docker-mariadb.service```