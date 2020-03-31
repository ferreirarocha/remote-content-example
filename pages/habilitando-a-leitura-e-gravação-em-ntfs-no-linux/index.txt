O NTFS não é um sistema de arquivos nativo para linux, e muito tempo atrás isso causava um problema de gestão do disco, sendo assim foi lançado o NTFS-3G.

O ntfs-3g é um driver de leitura de gravação para sistem de arquivos NTFS, é cross plataform, e ainda é licenciado sob a GPL, seu objetivo é fornecer um gerenciamento seguro de sistemas de arquivos NTFS, como criar, remover, renomear, mover arquivos, diretórios, links rígidos, etc.

## Instalando

### Fedora

Uma vez que o EPEL está instalado e habilitado, vamos instalar o pacote ntfs-3g usando o comando abaixo com o usuário root.

```
yum -y instale ntfs-3g
```

### Ubuntu

```
sudo apt-get install ntfs-3g
```

### Arch Linux

```
yaourt -S ntfs-3g
```

## Conclusão

O NTFS-3G é um driver NTFS estável, completo, de leitura e gravação para Linux, Android, Mac OS X, FreeBSD, NetBSD, OpenSolaris, QNX, Haiku e outros sistemas operacionais. Ele fornece o gerenciamento seguro dos sistemas de arquivos Windows XP, Windows Server 2003, Windows 2000, Windows Vista, Windows Server 2008, Windows 7, Windows 8 e Windows 10 NTFS. Uma alternativa de alto desempenho, chamada Tuxera NTFS, está disponível para dispositivos embutidos e Mac OS X.

Se vocề deseja manter um sistema de arquivos NTFS operando em multiplataforma windows , Linux, Mac OS…, é imprescindível provisionar o ntfs-3g para a sua gestão.

-----
##### Fontes: 


https://www.tuxera.com/community/open-source-ntfs-3g/
https://en.wikipedia.org/wiki/NTFS-3G