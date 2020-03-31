O **PAC** (Perl Auto Connector) **Manager**, fornece uma GUI para configurar conexões: usuários, senhas, expressões regulares, macros, etc, ele suporta vários tipos de conexões entre elas ssh, vnc, telnet, rpd(rdesktop e xfreerdp),ftp, sftp, mosh, serial, webDAV, IBM 3270/5250 e Comandos Genéricos. Vale lembrar que seu último commit foi em 17/01/2017, o que demonstra uma certa estagnação do software, ainda que ele esteja totalmente funcional. Você pode conferir mais sobre essa aplicação acessando o [Github](https://github.com/perseo22/pacmanager) ,[Site](https://sites.google.com/site/davidtv/) e [Sourceforge](https://sourceforge.net/projects/pacmanager/).

### Recursos

Abaixo segue alguns recursos interessantes dessa ferramenta.

- Macros remotas e locais.
- Enviar remotamente comandos com regexp EXPECT.
- Conexões de cluster !! Conexões no mesmo cluster compartilham as teclas digitadas
- Suporte de script! (vía código Perl).
- Conexão serial/tty.
- Pre / post conexões execuções locais.
- Suporte Proxy.
- .Integração KeePass
- Recursos do Wake On LAN.
- Possibilidade de dividir terminais no mesmo TAB
- Acesso rápido a conexões configuradas através do ícone do menu da bandeja.

## Instalando

O Pac Manager é exclusivo para linux, está disponível em .deb, rpm e TGZ, confira nesse [link ](https://sourceforge.net/projects/pacmanager/files/pac-4.0/), ele também pode ser instalando em distribuições Arch Like, utilizando o repositório AUR.

### Arch , Manjaro, Antergos

```
yay -S aur/pacmanager-bin
```

### Linux Mint e Ubuntu

```
wget https://ufpr.dl.sourceforge.net/project/pacmanager/pac-4.0/pac-4.5.5.7-all.deb
 sudo dpkg -i pac-4.5.5.7-all.deb
sudo apt-get install -f
```

**Demo**

Abaixo é demonstrado uma configuração de conexão ssh.

![capa pacmanager](https://j.gifs.com/1r08Jo.gif)