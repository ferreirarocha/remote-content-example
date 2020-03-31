Instalar o docker é uma tarefa relativamente fácil, porém algumas vezes precisamos informar o passo a passo a algum interessado, sendo assim resolvi fazer essa publicação demonstrando o procedimento no Debian 9

#### Atualizando e instalando dependencias do sistema

```bash
apt-get update -y
apt-get install -y apt-transport-https ca-certificates wget software-properties-common
```

#### Instalando o docker

```bash
wget https://download.docker.com/linux/debian/gpg
```

```
apt-key add gpg
```

```
echo "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | tee -a /etc/apt/sources.list.d/docker.list
```

```
apt-get update
```

```
apt-get -y install docker-ce
```

```
systemctl start docker
```

```
systemctl status docker
```

```
systemctl enable docker
```

#### Instalando o Docker Compose

```
curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
```
```
chmod +x /usr/local/bin/docker-compose 
```

#### Adicionando seu usuário ao grupo Docker

```bash
usermod -aG docker marcos
```

Divirta-se na criação de containers em seu ambiente. 😄
#####Fontes: 
https://docs.docker.com/compose/install/#install-compose