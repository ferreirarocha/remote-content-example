Hoje demonstraremos como provisionar o GLPI no Docker, utilizaremos também o docker swarm e o traefik para trabalhar como proxy reverso, tanto o docker swarm quanto o traefik são opcionais mas o adotamos para trabalhar como cluster e proxy reverso em nosso ambiente.

#### Iniciando o Swarm

```bash
docker swarm init
```

#### Inicializando o rede

```bash
docker network create -d overlay net
```

#### Criand o serviço Traefik - Proxy Reverso

```bash
docker service create --name traefik \
--constraint 'node.role==manager' \
--publish 80:80 \
--publish 8080:8080 \
--mount type=bind,source=/var/run/docker.sock,target=/var/run/docker.sock \
--network net \
traefik:camembert \
--docker \
--docker.swarmmode \
--docker.domain=alfabe.ch \
--docker.watch \
--logLevel=DEBUG \
--web
```

#### Criando o serviço MariaDB - Banco de Dados

Estamos usando a imagem oficial do MariaDB para esse projeto, mas no mundo docker não há um consenso sobre a segurança do uso de banco de dados executando via container, seu uso é muitas vezes indicado para local onde não há uma carga de trabalho excessiva.

```bash
docker service create \
--name dataserver \
--replicas 1 \
--restart-condition any \
--network net \
--mount type=volume,source=maria-vol,destination=/var/lib/mysql \
--hostname dataserver \
--env MYSQL_ROOT_PASSWORD=12345 \
--env MYSQL_DATABASE=glpi \
--env MYSQL_PASSWORD=12345 \
--env MYSQL_USER=glpi \
-p 3306:3306 \
mariadb
```

#### Criando o serviço GLPI - Helpdesk

Estamos utilizando a imagem ferreirarocha/glpi:9.3, mas recomendamos o uso de suas próprias imagens em ambiente de produção.

```yaml
docker service create \
--name helpdesk \
--replicas 1 \
--restart-condition any \
--network net \
--label 'traefik.port=80' \
--label traefik.frontend.rule="Host:suporte.alfabe.ch;" \
--mount type=volume,src=glpi,dst=/var/www/html/glpi \
--hostname helpdesk \
ferreirarocha/glpi:9.3
```

#### Criando uma imagem

Caso queira buildar a images com suas próprias modificações baixe e altere o dockerfile acima

[Dockerfile](https://github.com/ferreirarocha/GLPI-Docker/blob/master/Dockerfile)

```bash
docker  build \
--build-arg LAST_RELEASE=9.3.0 \
--build-arg GLPI_VERSION=9.3 \
-t=ferreirarocha/glpi:9.3 .
```

#### Criando o serviço para o Portainer

```bash
docker volume create portainer_data
docker service create \
--name portainer \
--restart-condition any \
--replicas=1 \
--constraint 'node.role == manager' \
--mount type=bind,src=//var/run/docker.sock,dst=/var/run/docker.sock \
--mount type=volume,src=portainer_data,dst=/data \
--label traefik.port=9000 \
--label traefik.frontend.rule="Host:portainer.alfabe.ch;" \
--network net \
portainer/portainer \
-H unix:///var/run/docker.sock
```

#### Configurando o DNS

Nesse momento você deve apontar o domínio do seu serviço para o servidor que está executando o Docker com Traefik, essa tarefa é bem simples, seja em um Windows Server, Bind , Zero-Shell ou qualqer outro servidor de DNS que você possua em sua rede, nesse laboratório faremos a ateração diretamente no arquivo de hosts do sistema

Abra-o e insira o novo dominio

```bash
nano /etc/hosts
```

Ficará algo como na figura abaixo

```bash
127.0.0.1	localhost
192.168.1.111	suporte.alfabe.ch
192.168.1.111	monitor.alfabe.ch
```

A próxima etapa e tão esperada é acessar o serviço via web, e consumir o serviço, nesse caso

[http://suporte.alfabe.ch](http://suporte.alfabe.ch/)

**Conclua o processo de instalação do GLPI**

Na etapa denominada **Criando o serviço MariaDB - Banco de Dados**, criamos o serviço dataserver que executa o mariadb, setamos senhas padrões para o usuário root do banco de dados e também para o usuário GLPI, assim como o hostname do container que é dataserver, user esse dados para configurar a conexão com o GLPI.

![001](https://4.bp.blogspot.com/-9W12sGBJ3QI/W1cTEb-tHqI/AAAAAAABIb8/WFM3mdgoNoYdsRmRcyuq90_Ti2FmyUOZgCLcBGAs/s640/001.png)

![002](https://4.bp.blogspot.com/-kk_lDehG3dE/W1cTEetmWbI/AAAAAAABIcE/JAI4DHs_8X4DEkp7VUS-cvj1P4LGGKbWQCLcBGAs/s640/002.png)

![003](https://2.bp.blogspot.com/-8w09tICOZqA/W1cTEdq00xI/AAAAAAABIcA/KMUkkn4AupY9RFSX7q0MfEQ4y73UHxSwgCLcBGAs/s640/003.png)

![004](https://4.bp.blogspot.com/-Cxzpxai5meI/W1cTE2mKnjI/AAAAAAABIcI/jxlr_kywgrY8AlFF5yqCa_jaVZlDg-BSgCLcBGAs/s640/004.png)

Perceba que nessa etapa inserimos os dados conforme criamos o serviço dataserver;

![dados](https://3.bp.blogspot.com/-TwP4tEiplRU/W1cXv3MZYII/AAAAAAABIdQ/XTBIF0jXFV04aKykoJl5gxG3JsWG-bhdQCLcBGAs/s640/dados.png)

![005](https://4.bp.blogspot.com/-nsQCsTrx7oo/W1cTFN6O1xI/AAAAAAABIcM/TGXwKiQu0UgsP4OiMBPlQpaPGSOvRwQXACLcBGAs/s640/005.png)

![006](https://2.bp.blogspot.com/-iC-ImecePXA/W1cTFcbSfUI/AAAAAAABIcQ/cFSHhs-6SdIyBx6nwHg6SCZlYhnbJGKawCLcBGAs/s640/006.png)

As Demais telas são intuitivas, não necessitando de muita informação, ao final do processo de instalação, execute o comando abaixo para remover o arquivo de instalação.

```bash
docker exec -it $(docker ps -qf name=helpdesk) rm /var/www/html/glpi/install/install.php
```

![tela-de-login](https://2.bp.blogspot.com/-oAisqK0e3OY/W1cUdWxfRFI/AAAAAAABIc8/VLUD8yHAzugQvTnmTIR9nwhSijnfcie4wCLcBGAs/s640/009.png)

#### Portainer

A qualquer momento você poderá acessar os containers GLPI, MariaDB, Volumes persistentes e outros recursos fornecidos pelo docker, vale lembrar que durante o post instalamos o Portainer que faz justamente isso, porém seu gerenciamento é via web, em nosso ambiente podemos acessá-lo da seguinte forma.

http://monitor.alfabe.ch/#/init/admin

Na primeira vez que acessar crie uma senha de 8 dígitos ,e logue novamente.

![Captura de tela_2018-07-24_01-27-45](https://3.bp.blogspot.com/-g3egDpAA2NU/W1cTGCcIuVI/AAAAAAABIcY/XZubg0vx7HA1HvqlT4hzNgtaW_07YJYCgCLcBGAs/s640/Captura%2Bde%2Btela_2018-07-24_01-27-45.png)

Como exemplo acessamos o container GLPI via console do portainer.

![Captura de tela_2018-07-24_08-15-36](https://4.bp.blogspot.com/-9NYdc9ubqQ0/W1cTGADvlmI/AAAAAAABIcg/kfOpbPVopNEeW2COnUC-i1Xt2Z7YFPAkwCLcBGAs/s640/Captura%2Bde%2Btela_2018-07-24_08-15-36.png)

#### Traefik

Caso não tenha percebido Portainer, GLPI poussem portas de acesso diferentes porém o traefik nosso proxy reverso redireciona o tráfego de acordo com a url cadastrada no serviço.

A interface administrativa do traefik é a porta 8080, sendo possível acessá-lo de url host que ele faça a gerência, veja o exemplo abaixo.

```bash
suporte.alfabech:8080
```

![traefik](https://2.bp.blogspot.com/-DfP1XHmt5FY/W1cTGByugNI/AAAAAAABIcw/KLCrSWcJv3MQueaMa8SjQMzS7nQ1P_QpwCPcBGAYYCw/s640/traefik.png)

![traefik monitor](https://4.bp.blogspot.com/-2wpKaK9NZEQ/W1cVdBNnqYI/AAAAAAABIdE/t4MVBR2pBHkW9cRIMWQDMlSRx9jk5ZXzQCLcBGAs/s640/traefik.png.png)

#### Há vantagens ?

Talvez tanta etapa apenas para subir o GLPI não lhe agrade, porém uma vez feito esse processo esse ambiente se comportará com um serviço, ou seja sempre que nosso host docker inicializar os containers do banco de dados, mariadb, traefik e portainers inicialiazão automaticamente.

#### 

- Flexibilidade em testar novas versões, e muitiplas distribuções
- Agilidade no fluxo de ambientes em Teste , Homologação e de Produção.
- Recuperação mais rápida em caso de paradas.
- Mutiplos serviços web disponíveis na mesma porta de acesso, tornando o ambiente transparente para usuário.
- Economia de recursos em um host docker com apenas 2 Gigas de RAM e algum espaço em disco, você pode provisionar vários containers executanto vários serviços com dependencias conflintantes sem a necessidade de criar um isolamento via virtual machine ou máquina física.
- Maior disponibilidade do sistema: O Docker provê um ambiente em cluster o que siginifica que há alguma redundância e em conseguinencia continuidade no serviço em caso de falha isolada.
- Compartilhamento:
- Facilidade de gerenciamento:
- Aplicação como pacote completo:
- Padronização e replicação:
- Acesso à comunidade:

Sugiro a leitura desse post onde são apontadas algumas vantagens

https://www.inventti.com.br/container-docker-e-suas-vantagens/

#### Acabou ?

Para finalizar nesse ambiente, fizemos o provisionameto do GLPI em apenas um host docker no próximo post demonstraremos como utiliza-lo em cluster através do docker swarm.

Fico por aqui e até o próximo 😃

**Fontes**

https://www.mundodocker.com.br/docker-swarm-pratica/

https://docs.docker.com/engine/swarm/

https://traefik.io/

https://mariadb.com/kb/en/library/installing-and-using-mariadb-via-docker/

https://portainer.io/

http://glpi-project.org/