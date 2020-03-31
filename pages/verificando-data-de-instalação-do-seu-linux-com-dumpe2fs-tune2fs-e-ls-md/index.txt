Existem algumas maneiras de obtermos a data de instalação de um SO Linux ,entre elas o dumpe2fs, tune2fs, o ls e o last + wtmp, algumas mais precisas outras menos.
Vamo começar pelo last + wtmp, a data exibida vai depender muito de como sua distribuição gerencia os logs do sistema.

**Last + wtmp**

```
last -f /var/log/wtmp | grep "wtmp"
```

Caso sua distribuição faça o logrotate nesse arquivo, ele torna-se praticamente inútil para esse fim, pois na verdade ele é utilizado para checar os usuário e períodos de acesso no sistema.

Mais sobre o WTMP

**LS**

O Comando ls juntamente com alguns parâmetros consegue retornar um data bem próxima do momento da instalação, veja alguns exemplos;

O 1º Comando sem a flag --full-time, nos fornece um valor sem o ano correposdente à instalação, já o 2º Comando com a flag --full-time ativa, nos retorna o ano e o horário bem mais precisos.



- **1º Comando**

```
ls -alct /|tail -1|awk '{print $6, $7, $8}'
```

**Saída**

*Nov 17 21:21*



**2º Comando**

```
ls -lact --full-time /etc |awk 'END {print $6,$7,$8}'
```

**Saída**

*2016-11-17 21:21:56.384944000 -0200*

**Tune2FS e Dumpe2FS**

O Tune2FS assim como o Dumpe2FS são muito mais que comandos para retornar data de instalação, porém ele possui informações que satisfazem perfeitamente essa necessidade.

Considerando que o seu sistema foi instalado em um disco na partição /dev/sda1, execute o comando;

```
sudo dumpe2fs  -h  /dev/sda1 | nl
```

**Saída**

```
 dumpe2fs 1.42.13 (17-May-2015)  
  1  Filesystem volume name:  <none>  
  2  Last mounted on:     /boot  
  3  Filesystem UUID:     485c6370-6170-4a4a-abbc-f78b3ee5bf24  
  4  Filesystem magic number: 0xEF53  
  5  Filesystem revision #:  1 (dynamic)  
  6  Filesystem features:   ext_attr resize_inode dir_index filetype sparse_super large_file  
  7  Filesystem flags:     signed_directory_hash   
  8  Default mount options:  user_xattr acl  
  9  Filesystem state:     not clean  
 10  Errors behavior:     Continue  
 11  Filesystem OS type:    Linux  
 12  Inode count:       124928  
 13  Block count:       498688  
 14  Reserved block count:   24934  
 15  Free blocks:       378209  
 16  Free inodes:       124619  
 17  First block:       1  
 18  Block size:        1024  
 19  Fragment size:      1024  
 20  Reserved GDT blocks:   256  
 21  Blocks per group:     8192  
 22  Fragments per group:   8192  
 23  Inodes per group:     2048  
 24  Inode blocks per group:  256  
 25  Filesystem created:    Thu Nov 17 21:21:52 2016  
 26  Last mount time:     Sun Feb 5 07:57:22 2017  
 27  Last write time:     Sun Feb 5 07:57:22 2017  
 28  Mount count:       1  
 29  Maximum mount count:   -1  
 30  Last checked:       Sun Feb 5 07:57:12 2017  
 31  Check interval:      0 (<none>)  
 32  Lifetime writes:     295 MB  
 33  Reserved blocks uid:   0 (user root)  
 34  Reserved blocks gid:   0 (group root)  
 35  First inode:       11  
 36  Inode size:      128  
 37  Default directory hash:  half_md4  
 38  Directory Hash Seed:   23424559-70dd-4e25-adb9-bf2a1b808361  
```



Veja que ele retorna o estado de saúde do seu disco, e se vasculhar um pouco mais verá que na linha 25, há o retorno da data de criação do sistema de arquivos analisado.

*Thu Nov 17 21:21:52 2016*

Nesse caso Quinta Feira 17 de Novembro de 2016 as 21:21:52 Segundos
Porém você pode ter o seu sistema de arquivos raiz instalado em outro partição que não seja o /dev/sda1, então podemos utilizar o comando com o seguintes parâmetros.

**Usando o Tune2FS**

```
sudo tune2fs -l $(mount | grep 'on \/ ' | awk '{print $1}') | grep 'Filesystem created:'
```

**Saída**

*Filesystem created:    Thu Nov 17 21:21:53 2016*

**Usando o Dumpe2FS**

O mesmo comando podemos utilizar com o utilitário Dumpe2fs veja abaixo como ficaria.

```
dumpe2fs $(mount | grep 'on \/ ' | awk '{print $1}') | grep 'Filesystem created:'
```

*Filesystem created:    Thu Nov 17 21:21:53 2016*

**Fontes:**

http://unix.stackexchange.com/questions/9971/how-do-i-find-how-long-ago-a-linux-system-was-installed

https://linux.die.net/man/8/dumpe2fs

https://linux.die.net/man/8/tune2fs

http://man7.org/linux/man-pages/man5/utmp.5.html