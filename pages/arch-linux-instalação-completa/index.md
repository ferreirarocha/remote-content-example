# 	Parte 1 - Instalando Arch Linux - preparando o ambiente



> Poxa mais um tutorial de arch linux 😩

😃 Sim este é mais um tutorial de instalação do arch linux, sabemos que a sua wiki é muito rica nos detalhes, mas ainda assim, há usuários que encontram dificuldades seguindo o material.
Desta forma este blog assim como tantos outros disponibilizou uma série com três posts sobre o tema , dividimos em três etapas, sendo a primeira onde preparamos o ambiente, a segunda onde instalamos o arch linux e a terceira onde instalamos o ambiente gráfico e aplicações adicionais.
Certamente poderíamos fazê-lo em um único post, mas optamos por essa estratégia afim de tornar a processo de instalação modular , o que facilita a inserção de novos posts sobre provisionamento de outros ambientes gráficos como o Gnome, LXDE, KDE…
Além de não tornar a leitura muito tediosa , efeito comum quando o post é muito extenso.

# Mão na massa

## Configurando teclado

Carregue o mapa do teclado

```
loadkeys br-abnt2
```

Acesse o locale.gen e descomente a linha contendo o `pt_BR.UTF8 UTF-8`

```
nano /etc/locale.gen
pt_BR.UTF8 UTF-8
```

Habilite e exporte a nova configuração de localização.

```
locale-gen && export  LANG=pt_BR.UTF-8
```

Antes de iniciarmos a instalação, teste sua conexão com a internet.

```
ping -c 4 google.com
```

Caso esteja utilizando uma rede sem fio é necessário configurá-la através do `wifi-menu`

```
wifi-menu
```

Os próximos passos são intuitivos onde a aplicação nos pede o nome da rede, senha e a criação de um perfil para registrar a conexão, este perfil pode ser utilizado para estabelecer conexão automática através do netctl.

Continuando…
Vamos fazer o particionamento através do **sfdisk** esse tipo de particionamento é pouco convencional, e o único motivo por tê-lo adotado nesse post, foi devido a sua simplicidade e eficácia na criação de partições, porém você pode usar o seu preferido, seja cfdisk, parted, fdisk…. e tantos outros.

Use o comando `fdisk` para saber quais partições e discos disponíveis para a instalação

```
fdisk -l
```

Nesse momento temos um disco de 20 GB disponível, e faremos o particionamento da seguinte forma;

| Partição  | Tamanho | Tipo         |
| :-------- | :------ | :----------- |
| /dev/sda1 | 1GB     | 83, bootável |
| /dev/sda2 | 8GB     | 83           |
| /dev/sda3 | 11GB    | 83           |

## Criando partições automáticamente

Como dissemos acima utilizaremos o sfdisk simplesmente para acelerar o processo de particionamento, sendo assim nossa instrução ficou da seguinte forma.

```
echo -e 'size=1GB,  type=83, bootable
size=8GB,  type=83 
size=11GB, type=83' | sfdisk /dev/sda
```

Caso queira saber mais sobre o sfdisk , acesse o manual do programa em [sfdisk](https://www.systutorials.com/docs/linux/man/8-sfdisk/).

## Formatando partições

Optamos por utilizar o **EXT4** em todas as três partições do disco **/dev/sda**, criamos um label **-L** para facilitar a identificação numa futura manutenção.

```
mkfs.ext4 /dev/sda1 -L boot
mkfs.ext4 /dev/sda2 -L sistema
mkfs.ext4 /dev/sda3 -L usuario
```

## Montando as partições

```
mount /dev/sda2 /mnt
mkdir /mnt/home /mnt/boot
mount /dev/sda1 /mnt/boot
mount /dev/sda3 /mnt/home
```

## Assim ficou nossa tabela de particionamento

| Partição  | Uso                      | montagem |
| :-------- | :----------------------- | :------- |
| /dev/sda1 | inicialização do sistema | /boot    |
| /dev/sda2 | arquivos do sistema      | /        |
| /dev/sda3 | arquivos do usuário      | /home    |

É isso aí ! 😃 neste momento definimos em quais partições o nosso arch linux será instalado, assim como a partição /home e /boot, ambas opcionais. No próximo post veremos como instala-lo e também fazer a pós instalação.





# Parte 2 - Instalando o Arch linux - instalação



No [post anterior](https://www.alfabe.ch/2018/09/parte-1-instalando-arch-linux.html), demonstramos como preparar o ambiente para a instalação do arch, formatando e montando os discos para serem utilizados pelo pacstrap e arch-root, esse método instala os pacotes no diretório raiz especificado neste caso em `/mnt` , e não havendo pacotes especificados, o pacstrap usará o grupo `base`, mas também adicionamos o grupo `base-devel`, `wget` e também o `openssh`.

# Mão na massa

## Executando o pacstrap

```
pacstrap /mnt base base-devel openssh wget
```

## Gerando fstab através do genfstab

```
genfstab -U /mnt >> /mnt/etc/fstab
```

## Entrando no arch-root

O arch-root , assim como o chroot é uma operação que muda o diretório root do processo corrente e de seus processos filhos. Um programa que é executado em chroot em um outro diretório não pode acessar arquivos fora daquele diretório, e o diretório é chamado de “prisão chroot” .

```
arch-chroot /mnt bash
```

## Definindo a senha do root

```
passwd root
```

## Definindo a zona horária

Por comodidade remova o arquivo localtime, não se preocupe, vamos criá-lo logo em seguida com a localização específica.

```
rm /etc/localtime
ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime
hwclock --systohc --utc
```

## Alterando o arquivo de idioma padrão

O sed abaixo descomenta o idioma pt_BR.UTF-8.
Caso queira alterar o /etc/locale.gen por editores como o nano, vi , etc. fique a vontande.

```
sed -i s/\#pt_BR.UTF-8/pt_BR.UTF-8/g /etc/locale.gen
```

## Gerando o arquivo de linguagem

```
locale-gen
echo LANG=pt_BR.UTF-8 > /etc/locale.conf
export LANG=pt_BR.UTF-8
```

## Alterando o hostname

```
echo alfabech > /etc/hostname
```

## Instalando o GRUB

Esta etapa já deixa o nosso arch linux tecnicamente instalado, ainda que não possua interface gráfica e as demais aplicações.
A instalação do os-prober é necessária caso queira utilizar este sistema em dualboot.

```
pacman -S grub os-prober 
grub-install /dev/sda
grub-mkconfig -o /boot/grub/grub.cfg
```

## Configurando a rede e ssh

```
systemctl enable dhcpcd
systemctl enable sshd
```

## Configurando conta de usuário

```
sudo useradd -m -G sys,lp,network,video,optical,storage,scanner,power,wheel marcos
passwd	usuario
```

Nesse momento criamos o usuário marcos, com permissão para alguns grupos como vídeos, eles são necessários para que este usuário possa utilizar a interface gráfica sem maiores problemas.

## Configurando permissão administrativa

```
sed  -i s/\# %wheel/%wheel/g /etc/sudoers
```

Da mesma forma vc também pode alterar a configurações do sudoers com o visudo, ou editor nano exemplo:

```
nano /etc/sudoers
```

descomente a linha contendo

```
# %wheel ALL=(ALL) ALL
```

ficando assim

```
%wheel ALL=(ALL) ALL
```

## Finalizando

Pronto instalação concluída já temos o grub , pacotes básicos e rede, o que nos resta agora é instalar a interface gráfica, aplicações do cotidiano , ativar o dhcp, ssh, caso queira, e outros pequenos ajustes que variam de acordo com o usuário.





# Parte 3 - Instalando Arch Linux - pós instalação XFCE


No posts anteriores demonstramos como preparar o ambiente e a instalar o arch linux, nesse momento nós vamos incluir o XFCE e algumas aplicações complementares como , descompactadores, navegadores, players e yay, um helper para gerenciar a instalação de pacotes do repositório AUR.

# Mão na massa

Caso o teclado ainda não esteja configurado com o layout br-abnt2 faça o seguinte passo

```
loadkeys br-abnt2
```

## Configurando mirrorlist

O Reflector é um script que recupera a última lista de espelhos da página mirrorstatus, filtra os espelhos mais atualizados, classificá-os por velocidade e sobrescreve o arquivo /etc/pacman.d/mirrorlist.

```
pacman -S reflector
reflector --verbose --latest 5 --sort rate --save /etc/pacman.d/mirrorlist
```

## Instalando o XORG

```
pacman -Syu	xorg xorg-server xorg-xinit
```

## Instalando o XFCE4

```
pacman -S xfce4 xfce4-goodies \
	lxdm xdg-user-dirs ttf-dejavu ttf-droid 	
```

Adicionando os subdiretórios de usuários:

```
xdg-user-dirs-update
```

## Instalando o drive de vídeo

Aqui abordaremos a instalação dos drivers open-source disponíveis no próprio repositório da distribuição.

### Drivers padrão.

```
pacman -S xf86-video-vesa
```

### Drivers Radeon

Instale o driver Radeon de código aberto.

```
pacman -S xf86-video-ati
```

### Drivers da Nvidia

```
pacman -S xf86-video-nouveau
```

### Drivers da Intel

```
pacman -S xf86-video-intel
```

## Instalando navegador web

```
pacman -S firefox 
```

## Configurando o gerenciador de login

Acesse

```
nano /etc/lxdm/lxdm.conf
```

Altere

```
#session=/usr/bin/startlxde 
```

para

```
session=/usr/bin/startxfce4
```

## Ativando o gerenciador de login lxdm

```
systemctl enable lxdm
```

## Alterando o idioma da interface

```
localectl set-locale LANG=pt_BR.UTF-8
```

## Reiniciando o sistema

Nesse momento basicamente já temos o sistema instalado, portanto podemos sair do arch-root e iniciar o sistema.

```
exit
umount /mnt
umount /mnt/home
umount /mnt/boot
reboot
```

Se tudo ocorreu como esperado a tela de login do lxdm já está disponível, acesse com o usuário que você tenha criado.

![lxdm-login](https://2.bp.blogspot.com/-ebfrQSz8ic0/W40tqg5CBfI/AAAAAAAACRg/Clo8TRJhXhQ-KF8rbNWDq_9nJolqFjpngCPcBGAYYCw/s1600/xfce-login-lxde.png)

## Alterando teclado na interface XFCE

Abra a central de configuração do XFCE

```
xfce4-settings-manager
```

Navege em

**Teclado** > **Disposição** > **Adicionar** > **Português Brasil**

Caso queira apenas o idioma português, remova o Inglês (EUA)

![Screenshot_Arch-Linux_2018-09-13_07:48:17](https://1.bp.blogspot.com/-1GvaKNlDzEk/W5pFzIb3glI/AAAAAAAACTE/FUZdCAaG0KQlkqeKX64Adm9032kuV_t9ACLcBGAs/s1600/Screenshot_Arch-Linux_2018-09-13_07%253A48%253A17.png)

## Configurando o repositório multilib

```
sudo nano /etc/pacman.conf
```

Descomente as linhas a seguir e salve o arquivo.

```
#[multilib]
#Incluide = /etc/pacman.d/mirrorlist
```

Para

```
[multilib]
Incluide = /etc/pacman.d/mirrorlist
```

Atualize a lista de pacotes.

```
sudo pacman -Syu
```

## Instalando pulse áudio

```
pacman -Syu alsa-{utils,plugins,firmware} \
	pulseaudio pulseaudio-{equalizer,alsa}
```

## Instalando complementos

Instalando demais aplicações como vlc, compactadores, git dependência para instalar o yay .

```
pacman -Syu \
	exfat-utils \
	vlc \
	tar \
	unzip \
	p7zip \
	unrar \
	rsync \
	file-roller \
	go \
	git \ 
	screenfetch \
	archlinux-keyring
```

## Instalando codecs

Para mais codecs, visite o wiki.[1](https://www.sinergicait.com.br/2018/09/parte-3-instalando-arch-linux-pos.html#fn1)

```
pacman -Syu \
	a52dec \
	faac \
	faad2 \
	flac \
	jasper \
	lame \
	libdca \
	libdv \
	libmad \
	libmpeg2 \
	libtheora \
	libvorbis \
	libxv \
	wavpack \
	x264 \
	xvidcore
```

## Instalando o Yay

Yet another Yogurt - An AUR Helper written in Go.[2](https://www.sinergicait.com.br/2018/09/parte-3-instalando-arch-linux-pos.html#fn2)

Com o Yay podemos instalar pacotes disponíveis no repositório **AUR**.

Execute esta instalação como usuário comum e que tenha permissões administrativas

```
git clone https://aur.archlinux.org/yay.git
cd yay/
makepkg -si
```

### Usando o yay

```
yay -Syu
```

## Utilizando o Pacman

[Manual pacman](https://wiki.archlinux.org/index.php/Pacman_(Português))

Essa página contém uma introdução para utilizar o pacman.

# Finalizando

Esta terceira etapa conclui a série de instalação do arch linux, no decorrer desses três posts você deve ter percebido que o maior tempo gasto foi durante a pós instalação, onde a interface e pacotes adicionais interferem no tempo total da tarefa. Um ponto importante a ser considerado, é que o uso de mirror distantes são bem mais lentos como é de se esperar , isso também interfere de forma considerável em todo o procedimento, portanto caso não queira passar por esse transtorno, avalie quais são os seu melhores espelhos através de aplicações como o `reflector` e similares. Outra maneira seria utilizar os espelhos nacionais , a taxa de transferência também aumenta consideravelmente, esta lista pode ser econtrada em [Mirror Generator](https://www.archlinux.org/mirrorlist/) .

E caso de dúvidas, ou interesse em obter mais conhecimento sobre a distribuição, recorra a sua [Wiki](https://wiki.archlinux.org/) , reconhecidamente rica em documentação. Dito isso, resta a você leitor usufruir desta distribuição vista atualmente como uma das mais “divertidas” de utilizar.



# Parte 4 - Instalando Arch Linux - Personalizando XFCE



![tela](https://4.bp.blogspot.com/-HiYKKtpLmQM/W6NejS9eKRI/AAAAAAAACbQ/uOY5VPXK0TYc0j4dbFM_35VsUY0O3z4WwCLcBGAs/s1600/tela-xfce-min.png)

Nesta quarta etapa, demonstraremos como personalizar a interface xfce, alterando aplicativos padrões , painéis, gerenciador de arquivos padrão, etc, o objetivo é tão somente tornar o ambiente produtivo agradável sob o ponto de vista do usuário, dessa forma deixando de lado algumas opções padrões da interface, vou me abster sobre opiniões quanto a beleza no resultado final, pois obviamente isso é algo subjetivo e que assim como produtividade, não tem como ser obtida através de um valor mensurável.

# XFCE-LOOK e openDesktop

O XFCE-Look faz parte da rede de sites como [KDE-Look.org](http://kde-look.org/), [GNOME-Look.org](http://gnome-look.org/), [MATE-Look.org](http://mate-look.org/) entre outros, esta rede compõe o projeto openDesktop iniciado em 2001, tendo como objetivo o compartilhamento de aplicativos, temas e outros conteúdos entre artistas , desenvolvedores e usuários, formando assim uma extensa comunidade formada por mais de 100.000 usuários registrados e 2,6 milhões de visitantes únicos por mês.[1](https://www.sinergicait.com.br/2018/09/parte-4-instalando-arch-linux_14.html#fn1)

## Links de temas e ícones

O tema utilizado nesse post é o **[Matcha Gtk](https://www.xfce-look.org/p/1187179/)** , disponível no link abaixo.

```
https://www.xfce-look.org/p/1187179/
```

![tema-escolhido](https://4.bp.blogspot.com/-UmuaJiZzJYI/W5vjSx155DI/AAAAAAAACTk/UmP6mP4OyCANEJ4RqPcqrusADxE4WY67ACLcBGAs/s1600/2726958fde42a661fd7c0ffd581c3020a5d1.jpg)

#### Baixando e instalando

```
wget https://dl.opendesktop.org/api/files/download/id/1536917968/s/df8db84e774e7b06d82ab9ff3bb4a694/t/1536942460/u//Matcha-sea.tar.xz
tar   -Jxf Matcha-sea.tar.xz -C ~/.themes/
```

Após descompactar o tema em ~/.temes , defina-o em

**xfce4-settings-manager** >> **Aparência** >> **Estilo**

![tema-escolhido-ap](https://4.bp.blogspot.com/-fsq7gBwFr1Y/W5vjfal1zhI/AAAAAAAACTo/Dovc3eGi3qUqICFsyuc6jeOEbYTou7KKwCLcBGAs/s400/tema-escolhido-ap.png)

## Gerenciador de Janelas

Utilize a aba **Estilo** do gerenciador de janelas para, alterar os botões, maximizar, minimizar e fechar

**xfce4-settings-manager** >> **Gerenciador de Janelas** >> **Estilo**

![botoes](https://4.bp.blogspot.com/-bNL1zXOIJnc/W5vjqNePxlI/AAAAAAAACTw/dwu0_7dzSygmX5LUeWSyFx5v1p7OO2WDwCLcBGAs/s1600/botoes.png)

## Ícones

O pacote de ícones escolhido foi o [**Papirus**](https://www.xfce-look.org/p/1166289/)

```
https://www.xfce-look.org/p/1166289/
```

![56595a54f9c931a8cac235caf97bccef5ab4](https://3.bp.blogspot.com/-gAyNyJPccQc/W5vjygavRrI/AAAAAAAACT0/P0qzv6A4oX00ZfEI0G6CYq30i3GFrpO-QCLcBGAs/s1600/56595a54f9c931a8cac235caf97bccef5ab4.png)

#### Baixando e instalando

```
wget https://dl.opendesktop.org/api/files/download/id/1534394282/s/ef8571ecc843e9df594b1efb8a481dcd/t/1536942326/u//papirus-icon-theme-20180816.tar.gz
tar xfz papirus-icon-theme-20180816.tar.gz -C ~/.icons/
```

**xfce4-settings-manager** >> **Aparência** >> **Ícones**

![icones-escolhidos](https://3.bp.blogspot.com/-CscpcA6eXbw/W5vj7tmY4FI/AAAAAAAACT4/fYuymIlc0vA7n_ZyIjR2WrIpG7K1cJAhgCLcBGAs/s400/icones-escolhidos.png)

O alerta em amarelo indica que não há um cache para o pacote de ícones e que pode ser gerado com o ***gtk-update-icon-cache***

```
 gtk-update-icon-cache ~/.icons/Papirus/
 gtk-update-icon-cache ~/.icons/Papirus-Adapta
 gtk-update-icon-cache ~/.icons/Papirus-Adapta-Nokto/
 gtk-update-icon-cache ~/.icons/Papirus-Light/
 gtk-update-icon-cache ~/.icons/Papirus-Dark/
```

## Backgrounds

Imagens legais para compor nosso desktop tem aos montes pela internet, mas custou nada informar mais um deles, divirta-se a quantidade e qualidade de imagens são excelentes.

```
https://unsplash.com/search/photos/desktop-background
```

![Captura de tela_2018-09-14_09-46-07](https://4.bp.blogspot.com/-7j7gYvf4P_A/W5vkv5CXjnI/AAAAAAAACUI/ZIkEAsN1n6sEzNhvWfIyuVKEqXfVT9EJwCLcBGAs/s1600/backgrouns.png)

## Configurando o painel

Não sei se isso ocorre com vocês, mas no meu uso diário apenas um painel já é o bastante em qualquer desktop, seja Gnome, KDE, XFCE, LXDE ou até no Windows. sendo assim um dos primeiros itens que já removo no XFCE é o painel secundário. as funções que são incorporadas a ele, eu transfiro para o pluguin, **dockbarx**.

## Adicionando o dockbarx plugin

O xfce4-dockbarx-plugin é um par de programas - um soquete gtk no Vala e um plug-in gtk no Python - que funcionam juntos para incorporar o DockbarX no xfce4-panel, [link do projeto](https://github.com/TiZ-EX1/xfce4-dockbarx-plugin).

```
yay -S xfce4-dockbarx-plugin
```

Para adicionar o dockbarx ao painel , clique com o botão direito no painel, clique em adicionar itens e escolha o plugin DockbarX.

![docbarx](https://4.bp.blogspot.com/-7Q6rFotYy4w/W5vk-nHXgZI/AAAAAAAACUM/p_uF1ROMq0cGJmqWZ1eEW8rnOdfxEv9ywCLcBGAs/s1600/docbarx.png)

![panel-config](https://4.bp.blogspot.com/-9B5SwUvQ5UM/W5vlIEcg9aI/AAAAAAAACUU/UcwtpX59TnAZ9ItRvOvJQGOfxADdkhHogCLcBGAs/s1600/panel-config.png)



Por hora são apenas esses itens que abordarei, em outro momento publicarei alguns métodos que utilizo para exportar e importar minhas configurações do XFCE, pois convenhamos realizar todas essas tarefas numa pós instalação chega a ser tediosa, incluindo o esquecimento de alguns itens importantes.

![tela-2](https://2.bp.blogspot.com/-TpUg5uJiizo/W5vlUMTGJgI/AAAAAAAACUc/YyCbX0Mj5yAkJMH62z-Geed5aiH9IrbmgCLcBGAs/s1600/tela-2.png)

