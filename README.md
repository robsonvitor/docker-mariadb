## Container com MariaDB Server

- [ x ] Renomeie o arquivo .env-example para .env e edite ele conforme seu ambiente
- [ x ] Edite o arquivo my.cnf conforme seu ambiente
- [ x ] Execute o projeto ```docker compose up```


## Configurações extras

###  Gerenciar o serviço via systemd

#### Copiar o arquivo de controle do serviço para o diretório do systemd
```bash 
cp ./extras/docker-mariadb.service /etc/systemd/system/docker-mariadb.service
```

#### Daemon reconhecer o novo serviço
```bash 
systemctl daemon-reload
```

#### Habilita o serviço
```bash 
systemctl enable docker-mariadb.service
```

#### Controlar o serviço
```bash 
systemctl {start|stop|restart} docker-mariadb.service
```

### Limitação de recursos para o container
```YAML
services:
  mariadb:
    image: mariadb:11.3
    container_name: mariadb
    mem_limit: 4g
    cpus: 2.0
...

```

### Otimização do Sistema Operacional Host

Algumas otimizações importantes para o Sistema Operacional em que os containers estão sendo executados

#### Quantidade de arquivos que podem ser abertos

```bash 
# /etc/security/limits.conf

* soft nofile 65535
* hard nofile 65535
```

```bash 
# /etc/systemd/system.conf e /etc/systemd/user.conf

DefaultLimitNOFILE=65535
```

#### Reduzir o uso de swap 

```bash 
echo vm.swappiness=10 | sudo tee -a /etc/sysctl.conf
```