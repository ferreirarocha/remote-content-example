Para realizamos os procedimentos é necessário que estejamos logados com o usuário root sendo assim execute o comando abaixo e entre com sua senha.

```
su 
```

Verifique como está oseu usdo de disco e partições utilzadas, em nosso caso queremo alterar a partição do diretório **/home**

```
df -l
```

![Usandoo comando DF ](https://3.bp.blogspot.com/-_FzcQOXAl24/WpvHg1rUzWI/AAAAAAABHmo/T7yDEUwtA7YBpp8TcZFWo92k8hExRWJ0wCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-03-03%2B19-19-09.png)

Verifique a disponibilidade de discos para fazer a sustituição, em nosso caso usaremos o disco /dev/sdb com 25 GB

```
fdisk -l
```

![Listando os Discos](https://2.bp.blogspot.com/-gw3o2Pdv2lI/WpvHkBTviGI/AAAAAAABHms/N9HIp8BWF_Ij_ZXKYX2_3PygsM40K4sMgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-03-03%2B19-19-58.png)

Escolha o disco onde será criado uma nova partição

```
fdisk /dev/sdb2
```

Verifique quantas partições existem nesse disco teclando a letra **P**

```
p
```

![Operando o fdisk](https://2.bp.blogspot.com/-3Okb5W0VHMM/WpvHmuL5vUI/AAAAAAABHmw/UAcHes6BHVwA8rYUW44hAj6BUP0QlgGUQCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-03-03%2B19-22-18.png)

Com a letra **N** você informa que seja criar uma nova partição

```
n
```

Informe o número e tamanhao tamanho da partição em nosso exemplo criamos uma nova partição de numero 2 com **+10GB**

![Criando uma partição](https://2.bp.blogspot.com/-J88vS231yxM/WpvHozDVnxI/AAAAAAABHm0/H0CEXbIQc0oim04taiwOcUw7iTUl_KQOwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-03-03%2B19-23-15.png)

Em seguida salve as alterações com o comando **W**

Caso necessário instale o pacote **parted** para fazer a releitura da tabela de partição sem necessidade de reiniciar a estação/servidor.

```
apt install parted
```

Execute o comando partprobe para fazer a releitura da tabela de partição

```
partprobe
```

Formate a sua partição, escolhemos o EXT4 como sistema de arquivos

```
mkfs.ext4 /dev/sdb2
```

Faça a montagem da nova partição para realizarmos o backup do diretório /home atual.

```
mount /dev/sdb2 /mnt
```

Fazendo o backup do diretório /home

```
rsync  -Pav  /home/*  /mnt
```

Obetendo o UUID através do blkid

O programa blkid é a interface de linha de comando para trabalhar com a biblioteca libblkid. Ela informa campos como LABEL UUID sistema de arquivos e outras opções)

```
blkid
```

![operando o BLKID](https://3.bp.blogspot.com/-hvM0D8f_axI/WpvHrSLHzHI/AAAAAAABHm4/iD6DKNg7jX4vB9r-6EN7J2p2qZZQ-xZtwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-03-03%2B19-31-22.png)

Use o fstab para definir a montagem automática da nova partição para o diretório /home

```
nano /etc/fstab
```

![Operando O FSTAB](https://3.bp.blogspot.com/-k-sg6LJMlq0/WpvHtNJ5CJI/AAAAAAABHm8/Zc__FHK4xRgBPnEFLj6HnrKEyFmWVvrHwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-03-03%2B19-32-48.png)

Salve as alterações teclando **F2**

Desmonte o diretório /mnt ou qualque outro utilizado para fazer o backup do /home,

```
umount /mnt
```

Agora monte todas os dispositivvos configurados no FSTAB com o comando mount -a

```
mount -a
```

Finalmente verifique se já está utilizando a nova partição com o comando df -h, em nosso caso é o disco /dev/sdb2

```
df -h
```

![Operando DF](https://3.bp.blogspot.com/-4Gk9KrP5ang/WpvHunBuaaI/AAAAAAABHnA/E7Dgp0kIg_wddQa0A3Q2-xHqmHvqKWb6ACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-03-03%2B19-34-41.png)

Preparamos um vídeo sobre o asssunto confira.

------



# Conclusão

Alterar a partição de um diretório é algo simples e rápido de ser feito em uma distribuição linux, para isso basta utilziar alguns recursos disponíveis como o fdisk, blkid, fstab, mkfs, parted , mount e rsync, Caso estivermos trabalhando em um ambiente desktop poderíamos usar o Gparted para criamos nossa partição.

Fico por aqui e até o próximo