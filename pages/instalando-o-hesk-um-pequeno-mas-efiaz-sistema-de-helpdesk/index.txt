O hesk é software de helpdesk simples, tendo como proposta a facilidade, possui uma pequena mas eficiente base conhecimento integrada, com tantos softwares disponíveis por aí como o **GLPI**, **OTRS**, **SyAid** … e tantos outros, o hesk visa atender justamente quem deseja inserir um software de helpdesk em seu ambiente porém sem outras camadas de processos que ainda não o utilizarão.

# Instalando dependências

Até mesmo as dependências do hesk são poucas bastando apenas o php, apache e a extensão php-mysql.

```
apt install php  apache2 unzip php-mysql
```

# Baixando o hesk

Acesse o link e baixe a última versão.

https://www.hesk.com/download.php

![hesk-001-download](https://4.bp.blogspot.com/-wZuLKvbCWoE/W6ajUyW3C7I/AAAAAAAACdE/TX_VR5iDYFwkUI7j_WxfegWuGBfKLLY9ACLcBGAs/s1600/hesk-001-min.png)

O hesk possui um pequeno captcha para realizar o download, sendo assim não foi possível disponibilizar o link direto nesse post.

Então o melhor nesse caso é baixar o arquivo via browser convecional e enviá-lo via **ssh** ao servidor.

Exemplo

```
scp ~/Downloads/hesk282.zip 192.168.1.96:/tmp
```

# Instalando Hesk

```
unzip /tmp/hesk*.zip -d /var/www/html/
```

```
chmod  755 -Rf /var/www/html/ 
```

```
chown  www-data. -Rf /var/www/html/
```



# Instalando o pacote de linguagem

Baixe também o pacote de tradução do hesk , confira o link.

https://www.hesk.com/language/

![hesk-002-pacote-de-tradução](https://4.bp.blogspot.com/-I1eYg__UWeE/W6ajTbsQ-zI/AAAAAAAACcs/hZKAYgXjlks7TnOAlUHODO3XQoIr-xRDgCLcBGAs/s1600/1537644284135-min.png)

```
scp ~/Downloads/pt-BR.zip 192.168.1.96:/tmp
```

```
unzip /tmp/pt-BR.zip  -d /var/www/html/language/
```



```
systemctl  restart  apache2
```



# Configurando o banco de dados

**Criando o banco**

```
CREATE DATABASE hesk ;
```

**Criando o usuário**

```
CREATE USER 'hesk'@'localhost' IDENTIFIED by "senha" ;
```

**Concedendo permissão ao banco de dados**

```
GRANT ALL PRIVILEGES ON hesk.* TO 'hesk'@'localhost';
FLUSH PRIVILEGES;
```

# Configurando via wizard

Agora conclua a instalaçaõ via interface web

```
http://192.168.1.96/install/install.php
```

![hesk–003-instalação-1](https://4.bp.blogspot.com/-9n62c27vL7I/W6ajTa5ZGmI/AAAAAAAACc0/xmTJYmvJWbszKH1vOqboHtggWvsITzc9wCLcBGAs/s1600/1537645731995-min.png)

![hesk–003-instalação-2-configurando o banco](https://1.bp.blogspot.com/-4RbSPy5R8nI/W6ajTQVem1I/AAAAAAAACcw/IDktKnYg39cZWwOTx6Ikw2MeMtaJ58XNgCLcBGAs/s1600/1537645800212-min.png)

![hesk-003-Finalizando](https://3.bp.blogspot.com/-n0wBDHVX4wo/W6ajTwzCu0I/AAAAAAAACc4/jlkMDFeGS7khvymviid1r2Swe1HE1O8TACLcBGAs/s1600/1537645953441-min.png)


Remova o diretório install, após concluir a instalação do hesk.

```
sudo rm -rf /var/www/html/install/
```

# Alterando o idioma

Navegue em

**Settings** e altere o default language para Português Brasileiro

![hesk-004-alterando o idioma](https://1.bp.blogspot.com/-8pwTKi01CQU/W6ajUOVT4II/AAAAAAAACc8/jGyBLePJoWkYCXamiXnJi-Dv4spFAfKDwCLcBGAs/s1600/1537646188087-min.png)

# Interface do sistema![hesk-005-finalizado](https://1.bp.blogspot.com/-UQpxJvoIveY/W6ajUeXSYoI/AAAAAAAACdA/O-J1DLdNM3wrugPDKj-xb3PWmtngDTaTACLcBGAs/s1600/1537646236340-min.png)



# Finalizando

Chegamos ao fim do processo de instalação do hesk, uma tarefa bem simples, nos resta agora, configurá-lo para que fique de acordo a necessidade.

O hesk claramente não é a opção mais robusta, porém cumpre o que propõe, ele possui uma boa base de conhecimento, e um workflow de chamados bem simples, ideal para quem deseja organizar seu ambiente de suporte. Atualmente o hesk fez parceria com a SysAid uma solução de software ITSM, Service Desk e Help Desk que integra todas as ferramentas essenciais de TI em um único produto.

![hesk-005-finalizado](https://1.bp.blogspot.com/-UQpxJvoIveY/W6ajUeXSYoI/AAAAAAAACdA/O-J1DLdNM3wrugPDKj-xb3PWmtngDTaTACLcBGAs/s1600/1537646236340-min.png)

