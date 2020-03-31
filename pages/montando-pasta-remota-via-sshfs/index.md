O ssh é um recurso muito conhecido entre os usuários linux, porém há momentos seja por necessidade ou simplesmente para facilitar a vida de outros usuários, precisamos montar uma pasta remota em nosso gerenciador de arquivos, é nesse cenário que surge o sshfs, com ele podemos montar o diretório via comand line e acessar via interface gráfica.


Veja os exemplos:

Primeiro instale o pacote sshfs

**No arch Linux**

```
yaourt  -S  sshfs
```

**No Ubuntu**

```
sudo apt-get install sshfs
```

Após instalado é só executar o comando com a seguinte sintaxe.

```
sshfs usuario@ip-ou-hostname:/diretorio-remoto  /diretorio-local
```

Exemplo :

```
sshfs  administrador@192.168.50.117:/var/www/html/   /home/marcos/webserver/
```

Pronto agora basta abrir o seu gerenciador de arquivos e acessar seu diretório remotamente.