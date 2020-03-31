Quando instalamos algumas distribuições como Lubuntu, Xubuntu ou outra em modo minimalista, é possível que o gerenciador de arquivos não instalado não possua suporte às operações como abrir arquivo em rede,gerenciar discos e etc, como é o caso do gerencadiro [PCManFM](https://wiki.lxde.org/en/PCManFM).



O erro comum é a senguite mensagem

`Trash: Operation not supported`

[![img](https://3.bp.blogspot.com/-wwNFmVWKDok/WNXIq1gWIMI/AAAAAAAATZM/lmI1C2lZo08WySF4gUKEl0ZjmuafCUJ-QCLcB/s400/Instalando%2Bsuporte%2BGVFS%2Bno%2BGerenciador%2Bde%2BArquivos.png)](https://3.bp.blogspot.com/-wwNFmVWKDok/WNXIq1gWIMI/AAAAAAAATZM/lmI1C2lZo08WySF4gUKEl0ZjmuafCUJ-QCLcB/s1600/Instalando%2Bsuporte%2BGVFS%2Bno%2BGerenciador%2Bde%2BArquivos.png)

**Sobre**

O gvfs é um sistema de arquivos virtual em espaço de usuário no qual o comando mount é executado como um processo separado e com o qual você se comunica via D-Bus. Ele também contém um módulo gio que, de forma transparente, adiciona suporte a gvfs a todas as aplicações que usam a API gio. Ele também fornece suporte para expor montagens gvfs a aplicações não-gio usando fuse. [Debian](https://packages.debian.org/pt-br/sid/gvfs")
Este pacote contém os mecanismos afc, afp, archive, cdda, dav, dnssd, ftp, gphoto2, http, mtp, network, sftp, smb e smb-browse. Sua instalação é bem simples em distribuições Debian Like



**Ubuntu**


```
 sudo apt-get install gvfs-backends gvfs-fuse  

```

Após a instalação, recomendanda-se reinciar o sistema para que o gerenciador de arquivos utilize o recurso recém instalado.

```
sudo reboot
```