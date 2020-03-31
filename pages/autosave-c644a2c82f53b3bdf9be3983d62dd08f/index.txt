O protocolo Remote Desktop Protocol (RDP) permite aos usuarios se conectarem a outras máquinas. A utilização do protocolo RDP é muito frequente entre máquinas Windows, no entanto hoje vamos ensinar como usar o mesmo protocolo para aceder remotamente ao Ubuntu. Para isso vamos recorrer ao serviço xrdp via script do renbuar.

### Baixar o Script de instalação do XRDP e Dependências para o Ubuntu 16.04.3

```
wget https://raw.githubusercontent.com/renbuar/scripts/master/ubuntu16.04.3/install-xrdp-1.9.1.sh
```

### Instalando o XRDP

```
chmod +x install-xrdp-1.9.1.sh 
./install-xrdp-1.9.1.sh 
```

### Reinstalando o Ubuntu Desktop

No meu sistema tive que reinstalar o ubuntu desktop.

sudo apt install ubuntu-desktop

### Conferindo se o serviço á está ativo

sudo systemctl status xrdp.service

### Criando/Copiando o arquivo de sessão para o XRDP em outros usuários do sistema

```
sudo cp .xsession /home/OUTRO-USUARIO
```

Permitir ao usuário corrente alterar o arquivo .xsession de sua /home

```
sudo chown usuario /home/usuario/.xsession
```

### Reiniciar o sistema

```
sudo reboot
```

Ao final já será possível logar remotamente e localmente simultâneamente

Pode logar com a própria aplicação de acesso remoto do rindows o **MSTSC**, ou outros como o **Remmina**, **MobaXTerm** entre outros.

Mais informações

Site Oficial

http://www.xrdp.org/

Fonte do script

http://c-nergy.be/blog/?p=10993