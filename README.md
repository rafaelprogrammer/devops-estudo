# DEVOPS
<table border="0">
    <tr>
    <td><a href="https://console.cloud.google.com/">
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
    </tr>
<table>
<table border="0">
<tr>
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
    </tr>
</table>

## **SUMÁRIO**

- [ARQUITETURA](#arquitetura)
- [DOMÍNIO](#dominio)
- [CONFIGURAÇÕES GOOGLE CLOUD](#config)
- [INSTALAÇÃO DO RANCHER - SINGLE NODE](#rancher)
- [INSTALAÇÃO DO KUBERNETES](#kubernetes)
- [INSTALAÇÃO DO TRAEFIK - DNS](#traefik)

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

<a id="rancher"></a>
## INSTALAÇÃO DO RANCHER - SINGLE NODE

Entrar no host, que será usado para hospedar o Rancher Server.

```
$ docker ps -a
$ docker run -d --name rancher --restart=unless-stopped -v /opt/rancher:/var/lib/rancher  -p 80:80 -p 443:443 rancher/rancher:latest
```
Acessar e configurar:

```
https://rancher.<dominio>/
```
<a id="kubernetes"></a>
## INSTALAÇÃO DO KUBERNETES

No rancher, adicionar o cluster e depois o rancher irá exibir um comando de docker run, para adicionar os host's (k8s-1 e k8s-2). Obs: deixar o Nginx Ingress (Aba - Advanced Options) desabilitado, pois será criado um.

Executar o comando que vai ser gerado pelo rancher durante a criação do cluster, para cada host gerenciado (k8s-1 e k8s-2):

Obs: adicionar sempre: etcd, controlplane, worker

```
k8s-1: 
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.4.7 --server https://rancher.<dominio> --token rvdgt4882qg84w244jshfm2tc7g5phjkrhnsrhfh8l69bwtxdghvqp --ca-checksum 2ee13e4c57c9f9ca6eda2c5f6707bb54148e63c026fd17cefbacb5dd501f6d23 --node-name k8s-1 --etcd --controlplane --worker

k8s-2: 
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run rancher/rancher-agent:v2.4.7 --server https://rancher.<dominio> --token rvdgt4882qg84w244jshfm2tc7g5phjkrhnsrhfh8l69bwtxdghvqp --ca-checksum 2ee13e4c57c9f9ca6eda2c5f6707bb54148e63c026fd17cefbacb5dd501f6d23 --node-name k8s-2 --etcd --controlplane --worker
```

Será um cluster com 2 nós. Navegar pelo Rancher e ver os painéis e funcionalidades.

### Instalar kubectl no host rancher-server

```
$ sudo apt-get update && sudo apt-get install -y apt-transport-https gnupg2
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
$ echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee -a /etc/apt/sources.list.d/kubernetes.list
$ sudo apt-get update
$ sudo apt-get install -y kubectl
```

Com o kubectl instalado, pegar as credenciais de acesso no Rancher e configurar o kubectl (pegar o dados do arquivo - Kubeconfig File no cluster criado).

```
$ cd /home/ubuntu
$ mkdir -p ~/.kube
$ vi ~/.kube/config
$ kubectl get nodes
```
<a id="traefik"></a>
## INSTALAÇÃO DO TRAEFIK - DNS

```
*.rancher.<dominio>
```

O Traefik é a aplicação que será usada como ingress. Ele irá ficar escutando pelas entradas de DNS que o cluster deve responder. Ele possui um dashboard de monitoramento e com um resumo de todas as entradas que estão no cluster.

Executar os comandos no host (rancher-server):

```
$ kubectl apply -f https://raw.githubusercontent.com/containous/traefik/v1.7/examples/k8s/traefik-rbac.yaml
$ kubectl apply -f https://raw.githubusercontent.com/containous/traefik/v1.7/examples/k8s/traefik-ds.yaml
$ kubectl --namespace=kube-system get pods
```


Configurar o DNS pelo qual o Traefik irá responder. No arquivo traefik-web-ui.yml, localizar a url, e fazer a alteração (onde possui 'dominio' alterar para o desejado). Após a alteração feita, rodar o comando abaixo para aplicar o deployment no cluster.

```
$ cd /home/ubuntu
$ git clone https://github.com/rafaelprogrammer/devops-estudo.git
$ cd devops-estudo/traefik
$ vi traefik-web-ui.yml
$ kubectl apply -f traefik-web-ui.yml
```

Acessar:

```
http://traefik.rancher.<dominio>/
```