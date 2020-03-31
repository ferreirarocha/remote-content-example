Sem sombra de dúvidas  o [cron](https://wiki.gentoo.org/wiki/Cron) é o agendador de tarefas mais utilizado em ambientes linux,  porém ele não é o único,  [Anacron](https://debian-handbook.info/browse/pt-BR/stable/sect.asynchronous-task-scheduling-anacron.html) , [fcron](https://www.ibm.com/developerworks/community/blogs/752a690f-8e93-4948-b7a3-c060117e8665/entry/fcron_melhor_que_cron_e_anacron_juntos41?lang=en),  [Hcron](https://expl.info/display/HCRON/Home), Jobber, [Hundeck](http://rundeck.org/downloads.html?__hssc=206669708.1.1516749213265&__hstc=206669708.b82443119e17ac12a12c95b41df6b579.1516749213265.1516749213265.1516749213265.1&hsCtaTracking=6c34b5b9-9523-4ad1-814f-6) e o  [systemd.timer](https://www.freedesktop.org/software/systemd/man/systemd.timer.html)

Os temporizadores são arquivos da unidade systemd cujo nome termina em  .timer que controla arquivos ou eventos de serviços. Possuem suporte  integrado para eventos de horário de calendário, eventos de tempo [monotônicos](https://pt.wikipedia.org/wiki/Sistema_operacional_de_tempo-real) e podem ser executados de forma assíncrona.

SystemD  Timer é  relativamente fácil de configurar e com apenas um comando  podemos ver o status tudo em de ums mais serviços, todos os logs no  diário, próxima execução, e aliada ao .service podemos ver de forma  clara a causa da falha, o SistemD Timer é utilizado como o  agendador de tarefas do [projeto cockipt](http://cockpit-project.org/running.html)

Hoje demonstraremos como substituir o cron pelo SystemD Timer na execução de  taregas automáticas do GLPI.

### CentOS

No CentOS  o local onde estão armazenados os arquivos de serviço e temporizadores é ;

```
/usr/lib/systemd/system/
```

### Debian

Já no Debian o local padrão é no

```
/lib/systemd/system
```

O arquivo responsável pela automação de tarefas no GLPI é o cron.php   e a sintaxe para a inserção no agendador de tarefas  pode ser uma das duas  opções abaixo

**1ª** Opção

```
/usr/bin/php /var/www/html/glpi/front/cron.php  &>/dev/null
```

**2ª** Opção - Caso a execução via CLI não consiga capturar os emails force processo com a flag --force mailgate

```
/usr/bin/php /var/www/html/glpi/front/cron.php --force mailgate &>/dev/null
```

Em nosso cenário  a  execução se dará da seguinte forma

glpi.timer > glpi.service > /var/www/html/glpi/front/cron.php

### Criando os serviços

#### glpi.timer

O glpi.timer será responsável por executar de temos em tempos  os serviço glpi.service

o system timer provê  maior controle e gestão sobre os serviços  habilitados, além de  demonstrar estatíticas sobre a execução dos  serviços.

**Centos**

```
nano /usr/lib/systemd/system/glpi.timer
```

**Debian**

```
nano /lib/systemd/system/glpi.timer
```

Insira esse conteúdo no arquivo *glpi.timer*

```
[Unit]
Description=Atualizador GLPI

[Timer]
OnBootSec=5min
OnCalendar=*:0/2
Unit=glpi.service

[Install]
WantedBy=multi-user.target
```

#### glpi.service

O glpi.service executará o arquivo cron.php responsavel por todas as tarefas em CLI ( Comand Line Interface)

**Centos**

```
nano /usr/lib/systemd/system/glpi.service
```

**Debian**

```
nano /lib/systemd/system/glpi.service
```

Insira esse conteúdo no arquivo *glpi.service*

```
[Unit]
Description=Atualiador GLPI

[Service]
Type=simple
ExecStart=/usr/bin/php /var/www/html/glpi/front/cron.php --force mailgate &>/dev/null

[Install]
WantedBy=multi-user.target
```

### Habilitando o serviço

Com o serviço criado no diretório do systemd, basta apenas habilitá-los para que inicializem automaticamente no próximo boot.

Habilitando o glpi.timer

```
systemctl  enable  glpi.timer 
```

Habilitando o glpi.service

```
systemctl  enable  glpi.service
```

### Ativando o serviço

Caso queira testar  imediatamente os novos serviços, basta executá-los  com o comand start

Iniciando o glpi.service

```
systemctl start  glpi.service
```

Iniciando o glpi.timer

```
systemctl start glpi.timer
```

Como um genuino  serviço do sytemd todas as opções desse gerenciador estão  disponíveis para nosso serviço, ssytemctl show, systemctl status,  journalctl -u glpi.service e  várias outras  opções que lhe ajudarão  na gestão do helpdesk.

Podemos ainda utilziar o  **systemctl list-timers** para obter as métricas de execução dos serviços

```
systemctl list-timers
SAIDA

NEXT                         LEFT       LAST                         PASSED       UNIT                         ACTIVATES
Tue 2018-01-23 18:54:00 -02  23s left   Tue 2018-01-23 18:52:04 -02  1min 31s ago glpi.timer                   glpi.service
Tue 2018-01-23 19:04:09 -02  10min left Tue 2018-01-23 18:02:54 -02  50min ago    anacron.timer                anacron.service
Tue 2018-01-23 19:09:00 -02  15min left Tue 2018-01-23 18:39:01 -02  14min ago    phpsessionclean.timer        phpsessionclean.service
Wed 2018-01-24 06:36:06 -02  11h left   Tue 2018-01-23 06:48:09 -02  12h ago      apt-daily-upgrade.timer      apt-daily-upgrade.service
Wed 2018-01-24 10:34:44 -02  15h left   Tue 2018-01-23 18:09:54 -02  43min ago    apt-daily.timer              apt-daily.service
Wed 2018-01-24 14:04:28 -02  19h left   Tue 2018-01-23 14:04:28 -02  4h 49min ago systemd-tmpfiles-clean.timer systemd-tmpfiles-clean.service

6 timers listed.
Pass --all to see loaded but inactive timers, too.
```

![Captura de tela de 2018-01-23 18-52-57](https://2.bp.blogspot.com/-0ISCPe2zR0s/WmvZE910OhI/AAAAAAAAB5A/tEvhgQ2HCHw3X7F5xq7ek6OK1lSFM4njACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-23%2B18-52-57.png)

Acima o foi nos informado  a quando tempo o serviço foi executado **PAST** , quanto tempo falta para sua execução **LEFT**,  o temporizador  **INIT** o e serviço que de fato executa uma tarefa em next podemo ver  a sua data de execução.

Se executarmos o  comando

```
systemctl  status glpi.service
```

Terermos mais informações sobre o  serviço como;

O status enable ou disable,  em **Loaded**

O seu tempo de execução, em **Active**

Local  de carregamento em execução do processo em **Process**

O pid  informando falha ou sucesso na operação.

![Captura de tela de 2018-01-23 22-13-03](https://4.bp.blogspot.com/-i7tsqs3km5Y/WmvZFG1dTbI/AAAAAAAAB5E/q0Gs3MKoKnk21RM97W1F0sDMnlIkwysUACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-23%2B22-13-03.png)

### Configurando o mailgate

Configurar > Ações Automáticas >  mailgate

Configure o modo de execução para **CLI**  ( Comand Line Interface)

Altere a Frequência de execução para  5 minutos  e Salve

![Captura de tela de 2018-01-23 22-21-36](https://4.bp.blogspot.com/-RHSxT0Le8HE/WmvZFPwl35I/AAAAAAAAB5I/BPRBu-_vOmgXgrBnE7Q_W4Bq6RPsTIpIQCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-23%2B22-21-36.png)

Agora podemos [configurar o email de destinatário para  podermos  receber  chamados via email, confira esse link demosntrando o passo a passo.](http://www.alfabech.com/2018/01/configurando-email-de-destinatario-no.html)

### Conclusão

Estamos muito bem servidos com o **CRON** , contudo, eu fico muito feliz em sabre que temos  substitutos a sua  altura e dependendo do contexto possuindo até mais  recursos de gestão  e controle que não  estamos acostumos nesse tipo de categoria de  ferramenta.

Fico por aqui e até  uma próxima oportunidade

##### Links complementares

https://niels.kobschaetzki.net/post/2015-11-11-creating-systemd-timers-instead-of-a-personal-crontab/

https://medium.com/@harishanand95/gsoc-creating-timers-in-cockpit-f4034c79df51

https://www.redpill-linpro.com/sysadvent/2016/12/07/systemd-timers.html