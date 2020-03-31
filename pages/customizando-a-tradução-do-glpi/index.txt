Por  diversos motivos, algumas vezes precisamos  customizar a  tradução do GLPI, para realizar esse procedimento, devemos  editar o  arquivo

```
/glpi/locales/pt_BR.po
```

Podemos fazer a alteração com qualquer  editor de texto puro por  exemplo:  nano , vim , emacs  e  também  programas específicos como o   poedit . Porém o que devemos ter em mente é que após a edição é  necessário converter o  arquivo **.po** para  **.mo**  que é o arquivo compilado. Podemos fazer esta conversão utilizando sites como o  https://po2mo.net/ contudo existe o utilitário  **msgfmt** , que faz exatamente isso de forma bem rápida e ainda com a vantagem de realizar o procedimento sem sair do  console.

Sua sintaxe é muito simples:

```
msgfmt arquivo.po -o  arquivo.mo
```

Após a conversão do arquivo  precisamos  reiniciar o servidor web,  apache ou ngix. Veja o exemplo prático  onde utilizaremos o  Servidor  web apache , faremos a edição com o nano e o diretório onde está  hospedado o glpi chama-se  glpi.

## Mão na massa

Editando o arquivo pt_BR.po

```
nano  /var/www/html/glpi/locales/pt_BR.po 
```

Queremos alterar a tradução

**Processando (atribuído)**   para  **Em atendimento**

Podemos também utilizar as teclas  **CTRL + W**  para buscar o texto desejado

[![img](https://3.bp.blogspot.com/-wg74QLoMnLI/Wcpcohr_EcI/AAAAAAAAAuA/OKBo1NEiGWAIjSHly3UfeWjv72Dkc_oUwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-09-26%2B10-23-53.png)](https://3.bp.blogspot.com/-wg74QLoMnLI/Wcpcohr_EcI/AAAAAAAAAuA/OKBo1NEiGWAIjSHly3UfeWjv72Dkc_oUwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-09-26%2B10-23-53.png)

```
#: inc/problem.class.php:520 inc/ticket.class.php:2876
msgctxt "status"
msgid "Processing (assigned)"
msgstr "Processando (atribuído)"
#: inc/problem.class.php:521 inc/ticket.class.php:2877
msgctxt "status"
msgid "Processing (planned)"
msgstr "Processando (planejado)";
```

Concluída a alteração do arquivo,  aperte  **F2**  para sair e salvar.

Utilizando o msgfmt para converter o arquivo pt_BR.po para pt_BR.mo,  feita a alteração do arquivo de tradução, utilize o  **msgfmt**  para compilar o arquivo pt_BR.po para pt_BR.mo

Em nosso cenário faremos assim;

```
cd /var/www/html/
msgfmt glpi/locales/pt_BR.po -o glpi/locales/pt_BR.mo
```

## Reinicie o servidor web Apache

Após compilar o arquivo reinicie o servidor web

```
/etc/init.d/apache2 restart
```

Ou

```
systemctl restart apache2
```

Ao final dessa etapa o glpi já apresentará a nova tradução.

[![img](https://4.bp.blogspot.com/-J62yVVcGBn4/WcpcdR9kWDI/AAAAAAAAAt8/v3tBx7VhgEM7DcnfKOSNsUzfadbgMqcnwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-09-26%2B10-33-13.png)](https://4.bp.blogspot.com/-J62yVVcGBn4/WcpcdR9kWDI/AAAAAAAAAt8/v3tBx7VhgEM7DcnfKOSNsUzfadbgMqcnwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-09-26%2B10-33-13.png)

Esse  post teve a  colaboração do Renato Feijão via grupo do GLPI no Telegram

http://telegram.me/RenatoFeijao

https://t.me/glpibr

Se quiser contribuir oficialmente com o time  de tradução do GLPI, acesse o  link abaixo para mais detalhes ;

**Language-Team: Portuguese (Brazil)**

http://www.transifex.com/glpi/GLPI/language/pt_BR

Lista do time de tradução PT-BR

```
# Translators:
# Jose Augusto Ferronato , 2013
# Daniel Berton , 2016
# Danilo da Cruz dos Santos , 2016
# DOMBRE Julien , 2014
# Eduardo Blumer, 2013
# Frederico Freire Boaventura , 2013
# Juranir Santos , 2014
# Rafael Fontenelle , 2016-2017
# Rodrigo de Souza , 2014
# Thiago Passamani , 2016
# Thiago Passamani , 2016
# Vilgidasio Junior , 2016
# Wanderlei Hüttel , 2016-2017
# wcreis , 2016
# wcreis , 2016
```

**Fontes;**

https://www.gnu.org/software/gettext/manual/html_node/msgfmt-Invocation.html

https://www.nano-editor.org/

http://glpi-project.org/

Fico por aqui e  até breve !