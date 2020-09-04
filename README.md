# DEVOPS - GOOGLE CLOUD

## **SUMÁRIO**

- [ARQUITETURA](#arquitetura)
- [CONFIGURAÇÕES GOOGLE CLOUD](#config)
- [INSTALAÇÃO DO DOCKER](#docker)

<a id="arquitetura"></a>
## ARQUITETURA
![ARQUITETURA](/imagens/arquitetura.png)

<a id="config"></a>
## CONFIGURAÇÕES GOOGLE CLOUD

### Configurações das instâncias

```
Criar usando o Ubuntu e com no mínimo 30GB de armazenamento.
```

![Configurações das instâncias]
(/imagens/instancias-google-cloud.png)

### Configurações do DNS

![Configurações do DNS](/imagens/dns-google-cloud.png)

<a id="docker"></a>
## INSTALAÇÃO DO DOCKER

```
É preciso entrar em todas as máquinas e instalar o Docker.
```

```
$ sudo su
$ curl https://releases.rancher.com/install-docker/18.09.sh | sh
$ usermod -aG docker ubuntu
```
### Instalação docker-compose

```
$ sudo su
$ apt-get install git -y
$ curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
$ chmod +x /usr/local/bin/docker-compose
$ ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```