Algumas vezes precisamos verificar a lista de dependência de alguma aplicação  em nosso sistema, seja para instalar um novo recurso, atualizar,  corrigir algum problema... Independente do motivo, uma vez ou outra  precisaremos saber como extrair esta informação em nosso sistema  operacional.

Quando o assunto é o GLPI ( Sistema para  Gerenciamento Livre de Parques de Informática) a situação não é muito  diferente, devido à uma considerável lista de dependências,  principalmente as extensões php, não é incomum ocorrerem instalações de  pacotes conflitantes ou que não correspondam com a base de dependência  necessária para o correto funcionamento da aplicação.

Tendo isto  em mente, apresentamos um modo simples de verificar e monitorar os  recursos básicos para o correto funciomento do GLPI.


**GLPI em Debian ou Ubuntu**



```
dpkg -l  | grep -E 'php|httpd|mariadb|mysql|apache|curl|unzip|snmp'
```



[![img](https://1.bp.blogspot.com/-pJdSN4Gfalk/WcfzZjizdeI/AAAAAAAAAqw/bniPtYmuYus-LOn-Ye7_3Z5bSuWAQ7diQCPcBGAYYCw/s1600/Sele%25C3%25A7%25C3%25A3o_010.png)](https://1.bp.blogspot.com/-pJdSN4Gfalk/WcfzZjizdeI/AAAAAAAAAqw/bniPtYmuYus-LOn-Ye7_3Z5bSuWAQ7diQCPcBGAYYCw/s1600/Sele%C3%A7%C3%A3o_010.png)


 OBS: É possível usar o apt list porém sua saída é muito poluída em comparação ao **dpkg -l**



```
apt list --installed  | grep -E 'php|httpd|mariadb|mysql|apache|curl|unzip|snmp'
```


Se desejar obter informação sobre um determado pacote aproveite a lista acima e realize um novo fitro

Exemplo:  preciso obter as informações detalhadas do pacote php7.0



```
dpkg-query -s php7.0
```


**Saída** 



[![img](https://1.bp.blogspot.com/-eHNr3Sik2CM/Wcf0vO4DBNI/AAAAAAAAAq4/J9xVJQ2j6gIh_V2u8_wKXhAeGJcBz5zRQCLcBGAs/s1600/Sele%25C3%25A7%25C3%25A3o_011.png)](https://1.bp.blogspot.com/-eHNr3Sik2CM/Wcf0vO4DBNI/AAAAAAAAAq4/J9xVJQ2j6gIh_V2u8_wKXhAeGJcBz5zRQCLcBGAs/s1600/Sele%C3%A7%C3%A3o_011.png)


A Saída informa as dependências necessárias para a execução do php7.0
 \--
**GLPI em CentOS**



```
yum list installed | grep -E 'php|httpd|mariadb|mysql|apache|curl|unzip|snmp'
```



[![img](https://4.bp.blogspot.com/-x3wbd1U6J-o/Wcf_wPjfvMI/AAAAAAAAArg/2viaREYlvCImhm9cao8JlRj24zWqcsdAwCLcBGAs/s1600/Sele%25C3%25A7%25C3%25A3o_014.png)](https://4.bp.blogspot.com/-x3wbd1U6J-o/Wcf_wPjfvMI/AAAAAAAAArg/2viaREYlvCImhm9cao8JlRj24zWqcsdAwCLcBGAs/s1600/Sele%C3%A7%C3%A3o_014.png)



Se desejar obter  as informações detalhadas do pacote  php71u-cli


Execute o comando 



```
yum info php71u-cli
```


**Saída** 



[![img](https://3.bp.blogspot.com/-Yv1JDkyLrJ0/Wcf1enEGT0I/AAAAAAAAArA/vRc_LtU2Ul0QdfLb5Xh-M9jyF9mDHXFbgCLcBGAs/s1600/Sele%25C3%25A7%25C3%25A3o_013.png)](https://3.bp.blogspot.com/-Yv1JDkyLrJ0/Wcf1enEGT0I/AAAAAAAAArA/vRc_LtU2Ul0QdfLb5Xh-M9jyF9mDHXFbgCLcBGAs/s1600/Sele%C3%A7%C3%A3o_013.png)


A Saída informa as dependências necessárias para a execução do php71u-cli

Tendo estas informações sob controle, a chance de gerar algum conflito no  sistema operacional e na aplicação diminui considerávelmente.

Dê uma atenção especial ao PHP, procure manter todas as extensões compatíves com pacote base.

**Fonts**
https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Displaying_Package_Information.html

https://linuxprograms.wordpress.com/tag/dpkg-pi/ 

https://stackoverflow.com/questions/16157256/regex-search-text1-or-text2 