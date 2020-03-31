RancherOS é uma distribuição Linux simplificada construída a partir de contêineres para contêineres. Tudo no RancherOS é um contêiner gerenciado pelo Docker. Inclui apenas a quantidade mínima de software necessária para executar o Docker. Tudo o resto pode ser executado dinamicamente através do Docker.

# Gerando o par de chaves pública e privada

Antes de iniciarmos a instalação do ranher, crie o par de chaves na sua máquinha cliente de onde você fará acesso ao rancherOS.

```bash
ssh-keygen -f $HOME/.ssh/key-rancheros
```

# Instalando

Inicie o rancherOS , confira o endereço IP que foi recebido , usaremos para definir o arquivo cloud-config.yml, que vamos gerar logo abaixo.

```bash
ip a s eth0 
```

![Captura de tela de 2018-04-02 08-01-43](https://2.bp.blogspot.com/-iARHtfR2pgE/W4w0LN7xupI/AAAAAAAACQc/J4TruGS1UqQZQKrCtq516o3tcF8jVeqIwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-04-02%2B08-01-43.png)

Perceba que o endereço ip desse sistema é ***192.168.50.144***, podemos então inserí-lo no arquivo cloud-config.yml

# Criando o arquivo cloud-config-yml

```bash
cat <<CONFIG> $HOME/cloud-config.yml
#cloud-config
ssh_authorized_keys:
 - $(cat $HOME/.ssh/key-rancheros.pub )

# rancherOS hostname
hostname: rancheros

# rancher-network settings
rancher:
  network:
    interfaces:
      eth0:
        dhcp: false
        address: 192.168.50.144/24
        gateway: 192.168.50.1
        mtu: 1500    
CONFIG
```

Abra um terminal na sua máquina cliente, copie e cole esse conjunto de comandos ele vai gerar nosso arquivo cloud-config.yml na home do seu usuário, esse comando insere.

* Chave pública do cliente ssh.

* Hostname do RancherOS.

*  Endereço ip do RancherOS.

*  Cria um usuário para acesso remoto e gerenciamento do rancher.



# Instalando o rancher server no HD

Agora acesse o RancherOS e transfira o arquivo cloud-config.yml que está na máquina cliente para nosso servidor.

Minha máquina cliente possui o endereço ip 192.168.50.123.

```bash
scp marcos@192.168.50.123:cloud-config.yml .
```

Nesse momento já podemos instalar o sistema no hd executando o seguinte comando;

```bash
sudo ros install -c  cloud-config.yml  -d /dev/sda
```

# Acessando o servidor após a instalação

Após a instalação remova a iso do drive de dvd físico, ou virtual no caso do virtuabox ou qualquer outro hipervisor.

```bash
ssh -i $HOME/.ssh/key-rancheros rancher@192.168.50.144
```

# Criando novo usuários

Caso queira utiilizar outro usuário após a instalação do rancher basta criá-lo como em qualquer distribuição linux.

```
sudo adduser marcos
```

```
sudo addgroup marcos sudo
```

```
sudo addgroup marcos docker
```

Como este usuário podemos acessar o rancher tanto via `ssh` quanto via `console`

# Executado o Rancher

```
sudo docker run -d --restart=unless-stopped -p 8080:8080 rancher/server:stable
```

```
sudo docker run -d --restart=always -p 8080:8080 rancher/server
```

# Adicionando um Host ao Rancher

**Infraestructure** > **Hosts** > **Add Host**

![Captura de tela de 2018-04-02 12-49-14](https://4.bp.blogspot.com/-KXWnzrnMDSw/W4w0PmDQUBI/AAAAAAAACQg/4DQxf2fUjLE75rr_zmqxP-QCpG5LIfjvwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-04-02%2B12-49-14.png)

------
