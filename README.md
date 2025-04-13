## Container com MariaDB Server

#### Entre no diretório onde será armazenado o código e dados do container, exemplo /opt/docker
```bash 
cd /opt/docker
```

#### Clonar repositório
```bash 
git clone git@github.com:robsonvitor/docker-mariadb.git
```

#### Entre no diretório onde foi realizado o clone
```bash 
cd /opt/docker/docker-mariadb
```


#### Copie o arquivo env-example para .env
```bash 
cp env-example .env
```
> [!IMPORTANT]  

> Ajuste as variáveis no arquivo .env conforme seu ambiente

> Ajuste a configuração do arquivo conf/my.cnf para seu ambiente

## Configurações extras

###  Gerenciar o serviço via systemd

#### Copiar o arquivo de controle do serviço para o diretório do systemd
```bash 
sudo cp ./extras/docker-mariadb.service /etc/systemd/system/docker-mariadb.service
```
> Ajuste o caminho do docker-compose.yaml no arquivo docker-mariadb.service

#### Daemon reconhecer o novo serviço
```bash 
sudo systemctl daemon-reload
```

#### Habilita o serviço
```bash 
sudo systemctl enable docker-mariadb.service
```

#### Controlar o serviço
```bash 
sudo systemctl {start|stop|restart} docker-mariadb.service
```

#### Antes de iniciar o serviço com o ```systemctl start docker-mariadb.service```, execute o ambiente manualmente para verificar se não há erros
```bash 
docker compose up
```

#### Verifique se o serviço está em execução
```bash 
docker ps -a
```

#### Resultado esperado
```bash 
CONTAINER ID   IMAGE                             COMMAND                  CREATED       STATUS                             PORTS                                         NAMES
cdd6a8057a6a   mariadb:11.3                      "docker-entrypoint.s…"   2 days ago    Up 10 seconds (health: starting)   0.0.0.0:3306->3306/tcp, [::]:3306->3306/tcp   cont-mariadb
```


> [!WARNING]  
> CUIDADO: Todos os dados do MariaDB serão gravados no diretório ```dados```

***
> [!NOTE]  
> Caso julgue necessário limitar recursos do container, ajuste conforme necessidade.

## Limitação de recursos para o container
```YAML
services:
  mariadb:
    image: mariadb:11.3
    container_name: mariadb
    mem_limit: 4g
    cpus: 2.0
...

```

***

## Otimização do Sistema Operacional Host

Algumas otimizações importantes para o Sistema Operacional em que os containers estão sendo executados

#### Quantidade de arquivos que podem ser abertos

```bash 
# /etc/security/limits.conf

sudo echo '* soft nofile 65535' >> /etc/security/limits.conf
sudo echo '* hard nofile 65535' >> /etc/security/limits.conf
```

```bash 
# /etc/systemd/system.conf e /etc/systemd/user.conf

sudo echo 'DefaultLimitNOFILE=65535' >> /etc/systemd/system.conf
sudo echo 'DefaultLimitNOFILE=65535' >> /etc/systemd/user.conf
```

#### Reduzir o uso de swap 

```bash 
echo vm.swappiness=10 | sudo tee -a /etc/sysctl.conf
```