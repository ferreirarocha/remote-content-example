# Contextualizando

Este plugin permite que o usuário use as credenciais do sistema  operacional para conectar-se ao MariaDB via socket Unix. Ele funciona  recuperando o uid do processo que se conectou ao socket, permitindo ao  usuário conectar-se com uma conta correspondente no MariaDB.



Esta técnica evita que outro usuário se conecte ao banco com outro  login baseadas no unix_socket, outro ponto importante também é que esta  abordagem não permite que esse usuário se conecte por outros meios além  do seu shell corrente.

Portando ao tentar autenticar sua aplicação na base de dados por  exemplo ,GLPI, hesk ou qualquer outra aplicação , um erro de conexão  será informado.

Vale lembrar que este  plugin é instalado por padrão a partir do  Ubuntu 15.10 e no Debian, já no CentOS ele não vem instalado mas esta é  uma tarefa extremamente simples.

# Mão na massa

Antes de fazermos qualquer modificação em nossa tabela de usuários,  vamos analisar como estão o modo de autenticação e privilégios de  usuários e se o plugin está ativo.

```
mysql -u root 
```

### Conferindo se o plugin está instalado e ativo

```
SELECT plugin_name,plugin_status FROM information_schema.plugins WHERE plugin_name='unix_socket';
 +-------------+---------------+  
  | plugin_name | plugin_status |  
  +-------------+---------------+  
  | unix_socket | ACTIVE        |  
  +-------------+---------------+  
  1 row in set (0.01 sec)
```

Se você receber a seguinte informação, significa que o seu plugin  está ativo, e caso não esteja instalado, instale-o com o procedimento  abaixo.

```
INSTALL PLUGIN unix_socket SONAME 'auth_socket';
```

### Listando o modo de autenticação

```
select  user,host,plugin,password from mysql.user;
 +------+-----------+-------------+----------+  
  | user | host      | plugin      | password |  
  +------+-----------+-------------+----------+  
  | root | localhost | unix_socket |          |  
  +------+-----------+-------------+----------+  
  1 row in set (0.00 sec)
```

Aqui podemos perceber que o usuário root,está realizando a autenticação via **plugin unix_socket**, e dessa forma não há necessidade de um password definido.

### Listando os privilégios dos usuários

```
show grants for 'root'@'localhost';
```

Com o comando `show grants` podemos exibir os privilégios concedidos a um determinado usuário.

![img](https://4.bp.blogspot.com/-Do_yYY0f72U/W6J1jVfwlPI/AAAAAAAACaY/wK-3LK9RkO8C9kG0_aFXVwCKQa-kdkMLQCLcBGAs/s1600/gif%2B%25283%2529.gif)

A Saída nos informa que o usuário **root@localhost**  possui permissão total a todos os bancos de dados com autenticação via  unix_socket, esse usuário ainda possui opção de autenticação via **proxy**, um tema interessante mas não o abordaremos nesse momento.

Portanto caso queira conceder o mesmo nível de permissão confira os seus privilégios.

# Criando novos usuários

Para demonstrar o funcionamento desse plugin, criaremos 2 usuários  com autenciação via unix_socket e 1 com autenticação por senha.  Criaremos esse usuários tanto no mariaDB quanto no sistema operacional.

```
mysql -u root
```

**Usuários com autenciação via unix_socket**

```
CREATE USER marcos IDENTIFIED VIA unix_socket;
CREATE USER reginaldo IDENTIFIED VIA unix_socket;
```

**Usuário com autenticação por senha**

```
CREATE USER 'marcelo'@'localhost' IDENTIFIED by "senha" ;
```

# Conferindo os usuários

Verifique como ficaram configurados o modo de autenticação dos novos usuários.

```
select  user,host,plugin,password from mysql.user; \q
```

![img](https://2.bp.blogspot.com/-nCf6ZLTvPTU/W6Jf2fUasgI/AAAAAAAACaM/_jBDGVbSQqQdPczNmE2crXgNjk12Ce95wCLcBGAs/s1600/gif%2B%25282%2529.gif)

# Criando usuario no sistema

Agora precisamos criar os usuários como o mesmo login que definimos no mariaDB.

```
useradd -m -s /bin/bash marcos   ; passwd marcos
useradd -m -s /bin/bash marcelo   ; passwd marcelo
useradd -m -s /bin/bash reginaldo ; passwd reginaldo
```

# Acessando o MariaDB via unix socket

Altere para o usuário reginaldo e acesse o mariadb.

```
su reginaldo
mysql --user=reginaldo
Welcome to the MariaDB monitor.  Commands end with ; or \g.  
  Your MariaDB connection id is 2  
  Server version: 5.2.0-MariaDB-alpha-debug Source distribution
```

Perceba que sua conexão foi bem sucedida.

```
quit
```

Ainda como usuário **reginaldo** tente logar com o usuário marcos

```
mysql --user=marcos
ERROR 1045 (28000): Access denied for user 'reginaldo'@'localhost' (using password: NO)
```

Se tudo estiver de acordo não será possível logar com o usuário  marcos através do usuário reginaldo, e m contra partida podemos acessar o sistema com o login do usuário marcelo, já que ele possui autenticação  via password, que é independente do usuário do shell corrente.

```
mysql --user=marcelo -p
Welcome to the MariaDB monitor.  Commands end with ; or \g.  
Your MariaDB connection id is 92  
Server version: 10.1.34-MariaDB-0ubuntu0.18.04.1 Ubuntu 18.04
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.    
Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.
```

# Conexões via a aplicações web

Como informando no início desse post, sua aplicação web não  conseguirá conectar-se ao banco de dados utilizando usuários com  autenticação via unix_socket, e nesse ponto fica evidente a necessidade  de criar um usuário com credenciais via password.

# Ideias

Com a informações acima podemos inferir que um das utilidades  notáveis desse modo de autenticação seria a tarefa de backup do seu  banco onde por exemplo, pois não haveria a necessidade de informar ou  armazenar a senha no momento de execução, já que o processo será  realizado via plugin unix_socket, tal processo poderia ficar a cargo de  apenas um usuário backup responsável por toda essa rotina em seu  MariaDB.

Vale lembrar também que não há necessidade de remover o usuário root  com autenticação unix_socket, você pode ter ambos desde que possuam  hostnames distintos.

Outro ponto levantado é que esse método traria uma insegurança ao  SGDB, mas de acordo com o equipe do projeto, isso não seria um problema, pois caso algum usuário ganhe acesso administrativo em seu sistema  operacional ele poderia simplesmente colocar o seu banco em modo safe e  realizar todos os procedimentos ilícitos que desejasse. Portanto usar ou não usar esse recurso de autenticação passaria por por outros crivos  como a automação de tarefas.

Fonte:

https://mariadb.com/kb/en/library/authentication-plugin-unix-socket/
