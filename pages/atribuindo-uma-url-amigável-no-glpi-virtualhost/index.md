

Atribuir uma URL amigável no GLPI é tarefa fácil, mas que traz um  grande retorno na qualidade de entrega de serviços pelo Departamento de  TI, pois o usuário não será obrigado a gravar um endereço ip de , e que  no melhor dos casos é para acessar com baixa frequência, por esse e  outros motivos nomes como [suporte.empresa.com](http://suporte.empresa.com), ou [ti.empresa.com](http://ti.empresa.com), são fáceis de gravar e consolida o serviços na instituição.

Abaixo apresento em 3 etapas o método para habilitar a URL amigável no GLPI.

**OBS** Este  post é baseado nas distribuições Debian e debian like.

### Etapas

1. Ativar URL amigável na interface do GLPI
2. Criar  um Virtualhost no  servidor GLPI
3. Apontar o  FQDN ( URL amigável ) para  servidor GLPI  no servidor DNS

# 1 Ativar URL  da Aplicação na interface do GLPI

Acesse  a  URL da aplicação em

**Configuração** > **Configuração Geral** > **URL da aplicação**

Crie um url fácil para  seus usuários, evite colocar apenas o  nome  glpi, ou suporte , pois na maioria se não todas as vezes o navegador  entederá isso como uma busca  e o que veio para facilitar a vida do  cliente  tournou-se um problema adicional.

Então tenha parcimônia em crie algo como [ajuda.empresa.com](http://ajuda.empresa.com), [ti.empresa.com](http://ti.empresa.com), ti.empresa …

Optamos por [suporte.alfabech.com](http://suporte.alfabech.com)

![Captura de tela de 2018-01-27 12-27-08](https://4.bp.blogspot.com/-PmVdsIFZF_o/Wm2vvRDJsaI/AAAAAAAAB88/0nq6ycZaN_8RW-f9SXflWqB35THFVaGmACKgBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-27%2B12-27-08.png)

Salve as alterações.

# 2 Criar  um Virtualhost no  servidor GLPI

Este comando cria o arquivo de configuração onde  é  habilitado o override.

Leve em consideração o local onde o seu GLPI está instalado, em nosso servidor ambiente a localização é

*/var/www/html/glpi.*

```
cat <<CONFIG-SITE >>  /etc/apache2/conf-available/suportealfabech.conf

<Directory "/var/www/html/glpi">
 AllowOverride All
</Directory>

CONFIG-SITE
```

O objetivo do AllowOverride é o gerenciar os arquivos de configuração principais do apache, que decide qual parte da configuração pode ser  alterada dinamicamente  através aplicações como o instalador do GLPI.

## VHOSTS  suporte.alfabech.com.conf

```
cat <<CONFIG-VIRTUALHOST >>  /etc/apache2/sites-available/suportealfabech.conf
<VirtualHost *:80> 
ServerAdmin admin@suporte.alfabech.com
 ServerName suporte.alfabe.ch
 ServerAlias www.suporte.alfabe.ch
 DocumentRoot /var/www/html/glpi
 ErrorLog /error.log
 CustomLog /access.log combined

</VirtualHost>
CONFIG-VIRTUALHOST
```

Usando o comando `cat << >>` facilitamos a inserção e manutenção de arquivos, mas se quiser fazê-lo manualmente segue o arquivo modelo.

```
nano /etc/apache2/sites-available/suportealfabech.conf
```

Conteúdo

```
<VirtualHost *:80>
ServerAdmin admin@suporte.alfabe.ch
ServerName suporte.alfabe.ch
ServerAlias www. suporte.alfabe.ch
DocumentRoot /var/www/html/glpi
ErrorLog /error.log
CustomLog /access.log combined
</VirtualHost>
```

#### Ativando o virtualhost

Com os arquivos de configuração e do virtualhost criados, basta o artivarmos com os seguintes comandos

```
a2enconf suportealfabech.conf
a2ensite suportealfabech.conf
```

Não se esqueça de restartar o apache

```
systemctl  restart apache2
```

# 3 Criar o apontamento  FQDN ( URL amigável ) no servidor DNS

A última etapa e não menos importante é  cadastar  o fqdn (Fully  Qualified Domain Name) no seu servidor de DNS, vamos demonstrar  essa  etapa no no windows server, porém  a mesma tática poderá ser aplicada  nos  demais servidores de DNS.

Abra o  **DNS Manager** na zona direta (Forward Lookup Zones)  insira  um tipo de host **A** ou **AAAA**

![Captura de tela de 2018-01-28 08-35-43](https://1.bp.blogspot.com/-B3FNoQ7rGGQ/Wm2vvVoGniI/AAAAAAAAB88/P4YznvjwVwgC6UBudeqxrjeTYeYNxGXlgCKgBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-28%2B08-35-43.png)

Na área de cadastro insira os dados o host glpi, aqui em nosso ambiente chamado de  **suporte** , devido ao dominio **[suporte.alfabech.com](http://suporte.alfabech.com)** cadastrado no virtualhost, lembre-se que esse campo pode ser **coincidentemente** o mesmo do **hostname**, porém nem sempre será será uma verdade.

![Captura de tela de 2018-01-28 08-36-21](https://1.bp.blogspot.com/-T6EF19K3FWE/Wm2vvevjGHI/AAAAAAAAB88/HgjjeZx51WEHqGL4zLSBgyGtdM8bgKO2ACKgBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-28%2B08-36-21.png)

Após adicionar o host faça um teste  com nlslookup para ver se  o dominio já responde pelo ip cadastrado no próprio windows.

```
nslookup suporte.alfabech.com
```

saída

```
Server:  UnKnown
Address:  ::1
Name:    suporte.alfabech.com
Address:  172.16.90.38
```

Pesquisa  reversa

```
nslookup 172.16.90.38
```

saída

```
Server:  UnKnown
Address:  ::1
Name:    suporte.alfabech.com
Address:  172.16.90.38
```

#### Acessando

Se tudo acima ocorreu como esperado, já é possível acessar o glpi por uma URL amigável

![Captura de tela de 2018-01-28 08-37-41](https://2.bp.blogspot.com/-4jy0w1rcwYc/Wm2vvZ9ZfRI/AAAAAAAAB88/9n1nqo5oaz4nVe0Sw9RNql9I6blISkVJACKgBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-28%2B08-37-41.png)

# Conclusão

A adoção de url amigável traz vários benefícios entre eles consolidar um único meio de acesso ao  serviço de suporte, facilitar esse acesso,  possibilita de maneira simples o trabalho com envio e recebimento de  notificações por e-mail, fortalece o consumo de outros serviços locais  por fqdn, como intranet, chat…  e não apenas o GLPI.

Fico por aqui e até a próxima 😄