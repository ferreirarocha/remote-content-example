Apresentamos uma maneira de realizar um backup  do glpi,  para o  conhecimento  de todos  ainda  há na própria  interface  do glpi  a  ferramenta de backup,  mas é indicada apenas para bases  muito pequenas e quando  dizem pequena , é algo em torno de 10mb,  portanto crie desde  o início  o seu  próprio workflow de backup,  e é por esse motivo que  publicamos esse material envolvendo o **mysqldump** , **cron**, **unix_socket**, e outras práticas.

Basicamente o que deve ser  salvo são os diretórios  files, pics,  plugins e a base de dados,  a partir  disso já temos o mínimo para  realizar  o nosso backup.

Para compactar os diretórios costumo usar o zip ou tar, nesse  tutorial optamos pelo zip. Já o backup do banco de dados utilizo o  mysqldump em faço  a autenticação com usuário específico para essa   função  via **Unix socket**, o que  evita a inserção  de senha no script.

Você pode estar pensando porque criar mais outro usuário, se posso  fazer isso com o root?, pois então uma das melhores  práticas para  gerenciamento de banco de dados é  delegar a cada usuário  uma função,  evite por exemplo fazer com que  o usuário root cuide de tarefas de  backup automáticas, o privilégio ***`LOCK TABLES`***  possibilita a criação  de usuários com acesso  somente leitura, o que  possibilita a realização de backups em apenas um base ou todas as bases  de dados.

# Configurando o usuário de backup

## Criando usuário no mariaDB

Ao longo do processo chamaremos esse usuário de **backupadmin** o Administrador de Backup, vale lembrar que  faremos sua autenticação  via unix_socket, isso evita a  publicação de seu password via script ou  qualquer outro meio,  em nosso blog temos um post tratando  especificamente desse recurso.

## Criando o usuário no sgdb

```
mysql -u root -p
GRANT LOCK TABLES, SELECT ON *.* TO 'backupadmin'@'%' IDENTIFIED VIA unix_socket;
```

## Criando  usuário para tarefa de backup

O unix-socket necessita que haja um mesmo  login de usuário em nosso  shell, por isso o criaremos   também no  linux, e como tarefa  complementar  o adicionaremos ao grupo *www-data* e  *backup* para ter acesso ao diretório  /var/www/ e /var/backups.

```
useradd  -m backupadmin -G www-data,backup  -s /bin/bash
passwd backupadmin
```

A partir de agora acesse o shell com o usuário backupadmin.

```
su - backupadmin
```

# Testando backup do  banco

Antes de iniciarmos a automação de backup , certifique-se de que o  usuário realmente consiga fazer um dump na base de dados, a título de  teste  faremos o cópia da base glpi salvando-a em backup.sql

```sh
mysqldump -u backupadmin glpi > backup.sql
```

# Criando Tarefa de Backup

## Instalando dependências

O zip e o multitail são dependências usadas para compactar o arquivo e ler os logs, caso queira utilizar o tar para  compactação e   outro  leitor de logs, bastar ajusar o  script abaixo.

```
apt install zip multitail
```

## Criando Script-Backup

Este é o script responsável por automatizar  nossos backups, salve-o dentro do diretório /opt como no exemplo abaixo.

```javascript
nano /opt/backup-glpi.sh
```



```
#!/bin/bash
# Nome : glpibackup.sh
# Versão: 1.0
# Autor : Marcos Ferreira da Rocha
# Email : marcos.fr.rocha@gmail.com
# telegram: @ferreirarocha
# Site : alfabe.ch
# Descrição :  O script glpibackup faz  o bacup do banco de dados e diretório do glpi, possui as opções  full (backup completo) e lite (backup dos dados e  pics, files e plugins do glpi), o processo de autenticação do banco de dados usa o unix_socket evitando a exposição da senha, também é  gerado um arquivo de logs quando o configuramos no cron.


#VARIAVEIS
data=$(date +"%Y-%m-%d_%H-%M")
dirglpi=/var/www/html/glpi
bkpdir=/var/backups/glpi
logdir=/var/log/backupglpi

# CRIANDO DIRETÓRIO DE BACKUP
if [ ! -d $bkpdir/]; then
	mkdir  -m 770 -p $bkpdir/
	chown .backup $bkpdir -Rf
fi

# CRIANDO DIRETÓRIO DE LOGS
if [ ! -d  $logdir ]; then
	mkdir $logdir
	chmod  775 $logdir
	chown  .backup $logdir -Rf
fi

cd $dirglpi

case $1 in

 lite)

	## Backup arquivos glpi
	
	zip -r $bkpdir/$data-bkpglpi-lite.zip files/ plugins/ pics/  -x files/_sessions/*

	## Backup banco glpi
	
	mysqldump -u backupadmin  glpi >  files/_dumps/$data.sql
	zip -ur $bkpdir/$data-bkpglpi-lite.zip files/_dumps/$data.sql
	rm files/_dumps/$data.sql

	;;
	
 full)

	## Backup arquivos glpi

	zip -r $bkpdir/$data-bkpglpi-full.zip ./  -x files/_sessions/*

	## Backup banco 
	
	mysqldump -u backupadmin  glpi >  files/_dumps/$data.sql
	zip -ur $bkpdir/$data-bkpglpi-full.zip files/_dumps/$data.sql
	rm files/_dumps/$data.sql

	;;

 *)

	echo "Função não  implementada"
	;;
esac

```

```
chmod +x /opt/backup-glpi.sh
```



# Agendando  a tarefa de backup no cron

```
crontab -u backupadmin -e
```



```

# Backup lite
30 07-18 * * * bash /opt/backup-glpi.sh lite  2>&1 | tee  /var/log/backupglpi/`date +\%Y-\%m-\%d_\%H-\%M`-lite.log

# Backup completo
00 13,23 * * * bash /opt/backup-glpi.sh full  2>&1 | tee  /var/log/backupglpi/`date +\%Y-\%m-\%d_\%H-\%M`-full.log
```

## Backup lite

O backup lite foi elaborado para criar cópias dos  diretórios  files, plugins e pics presentes no glpi, e de sua base de dados, o ideal é que esse script cubra todo o período de produção do seu ambiente de  trabalho, normalmente nós atendemos à clientes com horários  convencionais de 07:00 às 18:00 portanto para cobrir  essa janela ,   definimos a executação do script com a opção lite a cada hora por  exemplo 07:30, 08:30 até as 18:30.

Lembrando que essa cobertura deve ser avaliada  observando  a  quantidade de dados gerados pelo GLPI, Capacidade de armazenamento do  backup, Ciclo de retenção, Impacto causado na operação, aceitação do  risco de  janela sem backup…, entre outras variáveis igualmente  importantes.

Nesse exemplo criamos a seguinte tafefa no cron para  execução do  **backup lite**.

Onde entre 7 e 18 horas são gerados backups  e tambem um  log da   tarefa em /var/log/backupglpi.

```
30 07-18 * * * bash /opt/backup-glpi.sh lite 2>&1 | tee /var/log/backupglpi/date +\%Y-\%m-\%d_\%H-\%M-lite.log
```

Exemplo do arquivo de log gerado.

```
/var/log/backupglpi/lite-05-10-2018_10-30.log
```

## Backup completo

Também configuramos o script com a opção  de  realizar o  backup  completo de todo diretório glpi 2 vezes ao dia uma as 00:00 e outra às  13:00, como esta é uma tarefa que consome mais espaço e também  processamos o fizemos em horário de menor movimento, nesse caso o  horário do almoço 13:00 e as 00:00.

Nesse exemplo criamos a seguinte tafefa no cron para  execução do  **backup full**.

Não gerados backups completos incluido arquivos de configuração às  23:00 e 13:00 e assim como o backup lite, são gerados logs da   tarefa  em /var/log/backupglpi.

```
*00 13,23 * * * bash /opt/backup-glpi.sh full 2>&1 | tee /var/log/backupglpi/date +\%Y-\%m-\%d_\%H-\%M-full.log*
```

Exemplo do arquivo de log gerado.

```
/var/log/backupglpi/full-05-10-2018_13-00.log
```

A tabela  abaixo  e para deixar documentado de forma clara , seu  ciclo de backup, um  ponto de partida caso queira  aumentar ou dimunir o interva-lo entre as  tarefas.

| Horário   | Tipo de backup  |
| --------- | --------------- |
| 07:30     | Backup lite     |
| 08:30     | Backup lite     |
| 09:30     | Backup lite     |
| 10:30     | Backup lite     |
| 11:30     | Backup lite     |
| 12:30     | Backup lite     |
| **13:00** | **Backup Full** |
| 13:30     | Backup lite     |
| 14:30     | Backup lite     |
| 15:30     | Backup lite     |
| 16:30     | Backup lite     |
| 17:30     | Backup lite     |
| 18:30     | Backup lite     |
| **00:00** | **Backup Full** |

***Tabela:*** *Tabela de horários dos backups*

## Conferindo logs  de backup

```
multitail  -n 300 /var/log/backupglpi/2018-10-05_23-00-full.log
```

Nesse exemplo  o log nos informa que  o banco de dados glpi não foi  encontrado ao iniciar o backup, sendo assim mesmo   o aquivos.sql  sendo gerado  ele  é inútil durante a restauração, certamente isso foi  proposital para demonstrar os inconvenientes que podem  ocorrer ao  tentar restaurar  um backup e não soubermos o que ocorreu nos logs.

![1538617770444](https://2.bp.blogspot.com/-G8IvSUdxY8k/W7iH7IDTRdI/AAAAAAABIpk/PUN_oWVEfowsKIbowjXUb-LX0k9Q-spTgCLcBGAs/s1600/1-lendo-logs-com-multitail.gif)

***Image:*** *multitail*

# Restaurando  o backup

Tão  importante quanto o backup  é o sua restauração, como  demonstrado acima    manter o log dos eventos  também  é primordial,  abaixo apresentamos  um  modo  manual para  restaurar o backup feito por nosso script.

Navegue até o diretório de backups.

```
cd /var/backups/glpi/
```

Liste os backups disponíveis, aqui  fizemos uso do **stat** mas pode  utilizar o já conhecido **ls**.

```
stat -c %n *
```



```

2018-10-05_17-30-bkpglpi-lite.zip
2018-10-05_18-30-bkpglpi-lite.zip
2018-10-05_23-00-bkpglpi-full.zip
2018-10-06_07-30-bkpglpi-lite.zip
2018-10-06_08-30-bkpglpi-lite.zip
```

## Lendo os  arquivos zipados

Algo interessante que podemos fazer num arquivo zip, é a leitura prévia antes de  o descompactarmos.

*Exemplo*

```
unzip -l  2018-10-06_07-30-bkpglpi-lite.zip | grep .sql
```

Nesse exemplo demonstramos o arquivo .sql presente no backup, o mesmo que será utilzado na restauraçã do banco de dados.

```
...   files/_dumps/2018-10-06_07-30.sql
```

Você pode usar essa saída como parâmetro de extração no  zip, como demostraremos abaixo.

## Descompactando apenas o backup do banco de dados

o unzip fornece um meio de  descompactar apenas um arquivo , sua sintaxe é

*unzip file.zip  arquivo-solicitado*

No caso de um arquivo de nosso backup ficaria algo parecido com este comando.

```
cd /var/backups/glpi
```

Nesse comando extraímos somente o arquivo 2018-10-06_07-30.sql para o diretório /var/www/html/glpi/.

```
 unzip 2018-10-05_23-00-bkpglpi-full.zip  files/_dumps/2018-10-05_23-00.sql -d  /var/www/html/glpi/
```

Uma vez extraído o aquivos podemos restaurar a base usando a seguinte sintaxe.

*mysql -u root  -p base < base.sql*

Em nosso cenário a tarefa ficaria da seguinte forma.

```bash
mysql  -u root  -p glpi < /var/www/html/glpi/files/_dumps/2018-10-06_07-30.sql
```

## Restauração completa do diretório

Esse é um exemplo de  restauração completa do diretório e banco de dados, caso queira utilizar o backup  da tarefa *lite* isso é perfeitamente possível já que ambas  utilizam o mesmo processo.

Descompactar o backup full diretamente no diretório glpi

```
 unzip  /var/backups/glpi/2018-10-05_23-00-bkpglpi-full.zip -d /var/www/html/glpi/ 
```

Definir o grupo e usuário do diretório glpi

```
 chown  www-data.www-data -Rf /var/www/html/glpi/
```

restaurar o base de dados, caso  o banco não denha sido criado ainda, é necessário entrar no banco de  recriá-lo.

```
mysql -u root -e "create database glpi"; 
mysql  -u root  -p glpi < /var/www/html/glpi/files/_dumps/2018-10-05_23-00.sql
```

# Conclusão

No decorrer desse post  pudemos ver  algumas maneiras de  criar um  usuário específico para backup, criar rotina de backup, conferir os  eventos através dos logs, e é claro restaurar os backups. ainda podemos  abordar também outro aspécto importante desse ciclo que é enviar o  backup para   um  local fora  do próprio servidor, e também a trabalhar  com backup incremental, tal extratégia  se torna  interessante quando o  volume de dados  é elevado, e a  janela de backup  um fator decisivo .  No próximo post abordaremos esse cenário….

E o que você acha o que podemos melhorar nesse cenário ?, deixe o seu comentário logo abaixo.