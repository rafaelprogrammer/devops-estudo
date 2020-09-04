# DEVOPS
<table border=0>
    <td>
    <a href="https://console.cloud.google.com/">
    <img src="imagens/Google-Cloud-Logo.png" width="300">
    </a>
    </td>
    <td>
    <a href="https://kubernetes.io/">
    <img src="imagens/logo-kubernetes.png" width="300">
    </a>
    </td>
    <td>
    <a href="https://www.docker.com/">
    <img src="imagens/logo-docker.png" width="200">
    </a>
    </td>
    <td>
    <a href="https://rancher.com/">
    <img src="imagens/logo-rancher.png" width="300">
    </td>
    </a>
    <td>
    <a href="https://longhorn.io/">
    <img src="imagens/logo-longhorn.png" width="300">
    </td>
    </a>
    <td>
    <a href="https://docs.traefik.io/">
    <img src="imagens/logo-traefik.png" width="300">
    </a>
    </td>
    <td>
    <a href="https://istio.io/">
    <img src="imagens/logo-istio.jpg" width="300">
    </a>
    </td>
    <td>
    <a href="https://www.graylog.org/">
    <img src="imagens/logo-graylog.png" width="300">
    </a>
    </td>
    <td>
    <a href="https://grafana.com/">
    <img src="imagens/logo-grafana.png" width="300">
    </a>
    </td>
    <td>
    <a href="https://prometheus.io/">
    <img src="imagens/logo-prometheus.png" width="300">
    </a>
    </td>
    <td>
    <a href="https://helm.sh/">
    <img src="imagens/logo-helm.jpg" width="300">
    </a>
    </td>
    
<table>

## **SUMÁRIO**

- [ARQUITETURA](#arquitetura)
- [DOMÍNIO](#dominio)
- [CONFIGURAÇÕES GOOGLE CLOUD](#config)
- [INSTALAÇÃO DO DOCKER](#docker)

<a id="arquitetura"></a>
## ARQUITETURA
![ARQUITETURA](/imagens/arquitetura.png)

<a id="dominio"></a>
## DOMÍNIO

```
Criar domínio: https://registro.br/
```
### Configurar servidores de DNS do google cloud no registro.br

![DOMINIO](/imagens/dominio.png)

<a id="config"></a>
## CONFIGURAÇÕES GOOGLE CLOUD

### Criação das instâncias

```
Criar usando o Ubuntu e com no mínimo 30GB de armazenamento.
```

![Configurações das instâncias](/imagens/instancias-google-cloud.png)

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