O glpi possui um wizard de instalação super simples e eficaz, contudo o provisionamento automático de uma instância do GLPI pode ser ainda mais simples através da cliinstall.php e cliupdate.php ambos ferramentas nativas oferecidas pelo projeto no diretório **scripts**, é interessante informar que essas ferramentas foram disponibilizaas oficialmente a partir do GLPI 9.2.2, e que anteriormente eram disponbilizados de forma não oficial no diretório tools[1](https://www.sinergicait.com.br/2018/08/instalando-o-glpi-via-command-line-tool_14.html#fn1)

O processo de instalação é super simples sua sintaxe é a seguinte;

acesse o diretório glpi

```
cd /var/www/html/glpi
```

Lista completa de opções acionando o *–help*

```
php scripts/cliinstall.php --help
```



```
Usage: scripts/cliinstall.php [ options ]
--help                 Display usage
--host|-h <string>     Mach–-ine hosting the database
--db|-d <string>       Database name (required)
--user|-u <string>     Database user (required)
--pass|-p [ <string> ] Database password (default: no passwo
--lang|-l <string>     Locale (default: en_GB)
--tests                Test configuration
--force|-f             Override existing configuration
```

## Exemplo

```
php scripts/cliinstall.php \
--db=glpi \
--lang=pt_BR \
--user=glpi \
--pass=123456 
```

O Command line tools não soluciona as dependências do GLPi, ele apenas complementa a instalação que feita via interface web, processo bem conhecido pelos usuários *GLPI*

Outra curiosidade é que a execução do script não é tão rápida quanto gostaríamos, basicamente ela dura o mesmo tempo se não mais um pouquinho para carregar o schema padrão na base de dados, o schema está presente em [glpi](https://github.com/glpi-project/glpi)/[install](https://github.com/glpi-project/glpi/tree/9.3/bugfixes/install)/[mysql](https://github.com/glpi-project/glpi/tree/9.3/bugfixes/install/mysql)/**glpi-empty.sql** , o processo dura em torno de 50 segundos, então não se assuste se parecer que o seu script travou, tudo correndo como previsto a mensagem **Done** será exebida ao final.

```
Connect to the DB...
Database version seems correct (10.1.26) - Perfect!
Create the DB...
Save configuration file...
Load default schema...
Done
```

Ao final da instalação você poderá logar no GLPI. com as credenciais padrão.

*glpi glpi* e as demais



## Atenção

É importante informar que não obtive sucesso ao utilizar a flag `--test` junto ao comando, sempre que o habilitava o arquivo **config_db.php** não era criado no diretório config, ainda não consegui confirmar se este é um comportamento padrão da flag ou algum erro no código.

# Executando meu Script junto com a cli de instalação

Já que o Command line tools não soluciona as dependências do GLPI, resolvemos agregá-lo ao nosso script, possuímos opções para algumas distribuições como CentOS, Ubuntu e Debian, nesse exemplo abaixo é para o debian 9, o que fazemos abaixo é baixar o script que está no github com link encurtado no [bit.ly](http://bit.ly/)

```
wget http://bit.ly/debian9glpi
```

Abaixo é possível executar o script informando os parâmetros.

```
bash debian9glpi \
-l https://github.com/glpi-project/glpi/releases/download/9.3.0/glpi-9.3.tgz \
-p senha \
-u usuario \
-b banco \
-d diretorio \
-a y
```

## Descrição das opções;

***-l*** link da versão do GLPI

***-u*** usuário

***-p*** senha

***-b*** banco

***-d*** diretório

***-a*** y (Configuração automática)

A contrabarra `\` foi apenas para deixar o comando mais legível poŕem ele pode ser inserido em uma única senteça.

# Conclusão

Exceto pelo incoveniente da flag`--test` e o tempo elevado em popular o banco de dados pela shcema, esse incoveniente o script funciona como o esperado e pode ser um aliado se você deseja automatizar ainda mais o processo de provisionamento do GLPI.

Fonte:

------

1. https://glpi-install.readthedocs.io/en/latest/command-line.html#command-line-tools [↩︎](https://www.sinergicait.com.br/2018/08/instalando-o-glpi-via-command-line-tool_14.html#fnref1)