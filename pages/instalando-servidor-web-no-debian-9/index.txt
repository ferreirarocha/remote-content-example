O provisionamento de um webserver no Linux não é bicho de 7 cabeças, mas exige um pouco de dedicação e atenção aos detalhes, garantir um webserver no mínimo funcional envolve disponibilizar o Apache, MariaDB e o PHP, há muitas soluções para entregar esse ambiente no Linux dentre eles o **LAMP** e o **XAMMP**, porém nesse tutorial faremos o procedimento manual visando demonstrar a utilidade de cada componente.

### Atualizando o source.list do do sistema

Antes de começarmos a instalar programas devemos garantir que a [**lista de repositórios**](https://wiki.debian.org/SourcesList) esteja atualizada.

O comando abaixo insere lists de repositório main.

```
su -
```



```
echo -e "deb  http://deb.debian.org/debian stretch main\ndeb-src http://deb.debian.org/debian stretch main\n\ndeb http://deb.debian.org/debian stretch-updates main\ndeb-src  http://deb.debian.org/debian stretch-updates main\n\ndeb http://security.debian.org/ stretch/updates main\ndeb-src http://security.debian.org/ stretch/updates main"  >  /etc/apt/sources.list
apt update
```

## Upgrade

Confira se há algum pacote necessitando de upgrade, isso é importante para manter sua distribuição sempre atualizada.

```
apt list --upgradable
```

```
apt upgrade
```



## Instalando o Apache, PHP e MariaDB

A etapa seguinte instalaremos os pacotes apache, mariadb, php e mais alguns conjuntos de extensões e e programas úteis durante a gestão do seu webserver, como o caso do curl, wget e unzip.

```
apt-get install wget \
curl \
unzip \
snmp \
apache2 \
php \
libapache2-mod-php \
php-gd \
php-ldap \
php-curl \
php-mbstring \
sudo \
php-bcmath \
php-imap \
php-soap \
php-cli \
php-common \
php-snmp \
php-xmlrpc \
libmariadbd18 \
libmariadbd-dev \
mariadb-server \
php-dev \
php-pear \
php-mysql \
php-xml \
php-apcu-bc -y
```

### Criando usuário no banco de Dados MariaDB

Como você deve ter notado estamos utilizando o MariaDB como SGDB, ele é um fork do MySQL possuindo todos os recursos de seu predecessor, a diferença é que ele não sofre restrição para uso comercial, para sabser mais sobre ele acess o [**site oficial**](https://mariadb.org/).

#### Definindo senha para o usuário root

Aqui nós definimos uma senha e privilégios ao usuário root.

```
mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY 'SenhaROOT';
FLUSH PRIVILEGES;"
```

#### Criando uma base de dados no MariaDB

Faça um teste criando um banco de dados a fim de testá-lo.

```
mysql -u root  -p -e "create database MEUBANCODEDADOS character set utf8";
```

#### Criando Usuário para gerenciar uma base MariaDB

#### Criando o usuário

Crie um usuário extra para gerenciar a sua base de dados.

```
mysql -u root -p -e "create user 'USUARIO'@'localhost' identified by '12345'";
```

#### Concedendo permissões de administrador ao USUARIO à base MEUBANCODEDADOS

```
mysql -u root -p -e "grant all on MEUBANCODEDADOS.* to 'USUARIO'@'localhost'  with grant option;
```

```
FLUSH PRIVILEGES;"
```



#### Listando usuários criados

Entre com o usuário root

```
su -
```

Execute o comando abaixo para lista os usuário administradores do banco de dados.

```
mysql -u root  -p -e "select user,host from mysql.user";
```

## Gerenciando o servidor via interface web

As aplicações, webmin e phpmyadmin, são opcionais, servindo apenas como auxílio para quem ainda não domina o gerenciamento de um servidor linux via CLI, sendo totalmente dispensável a sua instalação.

### PhpMyadmin

Você pode gerenciar tranquilamente o webserver via CLI ( Comand Line Interface), no entanto há casos onde o admin tem preferência por utilizar um ambiente gráfico e no caso do linux temos as opções como o PHPMyAdmin

```
apt-get install phpmyadmin
```

Escolha o apache2

![Captura de tela de 2018-02-01 12-43-07](https://3.bp.blogspot.com/-6CC63ToXpIA/WnM-XqodQ3I/AAAAAAAACDE/AZ0vxkSiINEGtjz2jcKwKrQY5o0HPXi1gCKgBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-02-01%2B12-43-07.png)

Como definimos uma senha padrão para o user root do mysql, o **dbconfig-common** não será executado corretamente então marque a opção **Não** logo quando surgir esta tela.

![Captura de tela de 2018-02-01 12-43-35](https://1.bp.blogspot.com/-2TAcT7WAvPM/WnM-Xq6LZNI/AAAAAAAACDE/SHR92_YJpXI_jCqr5EPThprC3yVpqwSOwCKgBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-02-01%2B12-43-35.png)

Execute o seguinte comando para criar o banco pertecente ao phpMyAdmin.

```
mysql -u root -p < /usr/share/doc/phpmyadmin/examples/create_tables.sql
```

#### Etapa Opcional

Este comando cria um usuário chamado phpmyadmin capaz de gerenciar todas as bases de dados, nesse caso altere a senha o user de acordo com o seu ambiente.

```
mysql -u root  -p -e "CREATE USER 'phpmyadmin'@'localhost' IDENTIFIED BY 'SenhaMyAdmin';
GRANT ALL PRIVILEGES ON *.* TO 'phpmyadmin'@'localhost' WITH GRANT OPTION ;
FLUSH PRIVILEGES; "
```

### Como acessar o phpMyAdmin ?

Muito simples insira o endereço IP do servidor web, seguido de phpmyadmin.
Exemplo
[ip-do-servidor/phpmyadmin](https://www.sinergicait.com.br/2018/02/ip-do-servidor/phpmyadmin)

![Captura de tela de 2018-02-01 13-56-59](https://3.bp.blogspot.com/-yxbULBnj26U/WnM-XvtPr8I/AAAAAAAACDE/DPiC7YakVtIL8-b1kui1MUgiLIe_LPQywCKgBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-02-01%2B13-56-59.png)

### Instalando o Webmin

Webmin é uma interface baseada na web para administração de sistema para Unix. Usando qualquer navegador web moderno, você pode configurar contas de usuários, Apache, DNS, compartilhamento de arquivos e muito mais. O Webmin remove a necessidade de editar manualmente os arquivos de configuração do Unix como / etc / passwd e permite gerenciar um sistema a partir do console ou remotamente. Consulte a página de módulos padrão para obter uma lista de todas as funções incorporadas no Webmin.

#### Inserindo o repositório do webmin no source list

```
echo -e "deb https://download.webmin.com/download/repository sarge contrib" >>  /etc/apt/sources.list
```

#### Baixando a chave

```
wget http://www.webmin.com/jcameron-key.asc
```

#### Adicionando o chave

```
apt-key add jcameron-key.asc
```

#### Instalando o https download transport for APT

Este pacote permite o uso de linhas ‘deb https: // foo distro main’ na /etc/apt/sources.list para que todos os gerenciadores de pacotes que usam a biblioteca **libapt-pkg** possam acessar metadados e pacotes disponíveis em fontes acessíveis em https ( Protocolo de transferência de hipertexto seguro).

```
apt  install apt-transport-https
```

#### Atualizando

```
apt update
```

#### Instalando o Webmin

```
apt install webmin
```

### Como acessar o Webmin ?

[https://ip-do-webserver:10000](https://ip-do-webserver:10000/)

![Captura de tela de 2018-02-01 13-58-47](https://4.bp.blogspot.com/-EUB5YNhKRgg/WnM-Xpi9acI/AAAAAAAACDE/GoAvpDmLsXUQyxaffrnfBBUI7vtp2iEVACKgBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-02-01%2B13-58-47.png)

### Instalando FirewallD

O Firewalld fornece um firewall gerido dinamicamente com suporte para zonas de rede / firewall que definem o nível de confiança de conexões de rede ou interfaces. Possui suporte para IPv4, configurações de firewall IPv6, pontes de ethernet e conjuntos de IP. Existe uma separação de tempo de execução e opções de configuração permanente. Ele também fornece uma interface para serviços ou aplicativos para adicionar regras de firewall diretamente.

Instalando

```
apt install firewalld
```

Inciando

```
systemctl start firewalld 
```

Colocando o FirewallD no boot do sistema

```
systemctl enable firewalld 
```

Conferido o estado do FirewallD

```
systemctl status firewalld 
```

Verificando a atual zona de operação

```
firewall-cmd --get-default-zone
```

### Liberando portas no firewall

**80 e 443** para o servidor web
**10000** para o webmin

```
firewall-cmd --permanent --zone=public --add-port=80/tcp
firewall-cmd --permanent --zone=public --add-port=443/tcp
firewall-cmd --permanent --zone=public --add-port=10000/tcp
```

Recarregue o firewallD com o seguinte comando

```
firewall-cmd --reload
```

Para listar as portas liberadas no FirewallD execute o comando;

```
firewall-cmd --zone=public --list-all
public
  target: default
  icmp-block-inversion: no
  interfaces: 
  sources: 
  services: ssh dhcpv6-client
  ports: 80/tcp 443/tcp 10000/tcp
  protocols: 
  masquerade: no
  forward-ports: 
  source-ports: 
  icmp-blocks: 
  rich rules: 
```

## O que instalamos ?

Nosso webserver está instalado e pronto para uso contendo os seguintes recursos

**PHP 7** - Para executar aplicações em PHP

**MariaDB** - Para Banco de Dados

**Apache 2** - Apache para Servidor Web

**Opache** - Para Otimizar o carregamento de aplicações

**Webmin** - Para Gerenciar o servidor via web

**PHP MyAdmin** - Para gerenciar o SGDB

**FirewallD** - Para prover uma camada de segurança principalmente em VPS

## Conclusão

Ainda que essa provisão de web server tenha propósito geral, é bem provável que ela não sirva de imediato a todos os casos, pois não instalamos outros SGDBs como PostgreSQL, Firebird, SQLite3, entre tantas outras opções, dessa forma também não habilitamos as respectivas extensões no apache, já que seria algo desnecessário e consumiríamos recursos importantes como memória e cpu em nosso servidor, e em muitos casos incompatibilidade entre as aplicações.

Levando tudo isso em conta percebemos que provisionar o webserver no Debian pode não ser uma tarefa extremamene complexa, mas exige atenção e um pouco de cuidado para que o ambiente seja produtivo, seguro e que tenha uma boa documentação do processo, tornando-o assim uma solução viável e também escalável para a seu projeto.

Fico por aqui e até uma próxima oportunidade 😃

Fontes

[PHP](https://secure.php.net/)

[Webmin](http://www.webmin.com/deb.html)

[phpMyAdmin](https://www.phpmyadmin.net/)

[MariaDB](https://mariadb.org/)

[Apache](https://www.apache.org/)

[FirewallD](http://www.firewalld.org/)