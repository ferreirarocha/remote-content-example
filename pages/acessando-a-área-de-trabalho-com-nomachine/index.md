Então você necessita de ter acesso remoto a um computador que está na sua própria rede local ou privada?, prestar suporte, fornecer algum recurso, e muito mais ? , dependendo do caso o ssh, teamviewer, vnc e muitos outros podem ser a solução, porém hoje apresentamos a [NoMachine](https://www.nomachine.com/pt-pt) que fornece isso e muitos outros recursos para o seu ambiente.
O processo de instalação é bem rápido, cobrindo uma gama enorme de plataformas entre eles Windows, Linux, MAC, Android, IOS, dispositovos ARM e Raspberry. O nomachine também oferece serviços e recursos para empresas que desejam implantar ou manter uma nuvem privada através de produtos como NoMachine Terminal Server e NoMachine Virtualization Server , [Saiba Mais](https://www.nomachine.com/pt-pt/enterprise)
**https://www.nomachine.com/pt-pt/enterprise**
Para baixar a aplicação acesse o link de download abaixo
**https://www.nomachine.com/download**

### Instalando em Arch Linux e família

Se estiver utilizando uma distribuição que tenha como base o Arch Linux, instale-o pelo yaourt

```
yaourt -S nomachine
```

### Instalação via pacote RPM

#### *Homologados para;*

Red Hat Enterprise 4/5/6/7, SLED 10.x/11.x/12.x, SLES 10/11/12, Open SUSE 10.x/11.x/12.x/13.x, Mandriva 2009/2010/2011, Fedora 10/11/12/13/14/15/16/17/18/19/20/21/22/23/24/25

```
wget http://download.nomachine.com/download/6.0/Linux/nomachine_6.0.66_2_x86_64.rpm
sudo rpm -ivh nomachine_6.0.66_2_x86_64.rpm
```

#### Para desinstalar

```
sudo rpm -e nomachine
```

### Instalação via pacote DEB

#### *Homologados para;*

Debian GNU Linux 4.0 Etch/5.0 Lenny/6.0 Squeeze/7.0 Wheezy/8.0 Jessie, Ubuntu 8.04 Hardy Heron/8.10 Intrepid Ibex/9.04 Jaunty Jackalope/9.10 Karmic Koala/10.4 Lucid Lynx/10.10 Maverick Meerkat/11.04 Natty Narwhal/11.10 Oneiric Ocelot/12.04 Precise Pangolin/12.10 Quantal Quetzal/13.04 Raring Ringtail/13.10 Saucy Salamander/14.04 Trusty Tahr/14.10 Utopic Unicorn/15.04 Vivid Vervet/15.10 Wily Werewolf/16.04 Xenial Xerus/16.10 Yakkety Yak/17.04 ZestyZapus

```
wget http://download.nomachine.com/download/6.0/Linux/nomachine_6.0.66_2_amd64.deb
```

#### Para instalar

```
sudo dpkg -i nomachine_6.0.66_2_amd64.deb
```

#### Para desinstalar

```
sudo dpkg -r nomachine
```

### Instalação via TAR.GZ

O Nomachine ainda fornece o pacote tar.gz para sistemas que não utilizam gerenciadores de pacotes como DEB e RPM

```
cd /usr
sudo tar xvzf pkgName_pkgVersion_arch.tar.gz
sudo /usr/NX/nxserver --install
```

#### Exemplo:

```
cd /usr
wget http://download.nomachine.com/download/6.0/Linux/nomachine_6.0.66_2_x86_64.tar.gz
sudo tar xvzf nomachine_4.0.369_1_x86_64.tar.gz
sudo /usr/NX/nxserver --install
```

#### Para desinstalar

```
sudo /usr/NX/scripts/setup/nxserver --uninstall
```




Preparamos um pequeno vídeo demonstrando a sua utilização

<iframe src='https://www.youtube.com/embed/snxCuT16ISc' frameborder='0' allowfullscreen></iframe>

#####  Saiba mais

https://www.nomachine.com/pt-pt/introdução-ao-nomachine

https://www.nomachine.com/pt-pt