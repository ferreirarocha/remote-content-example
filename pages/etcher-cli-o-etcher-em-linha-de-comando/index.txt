O Etcher CLI, é uma ferramenta de linha de comando que visa fornecer todos os benefícios do aplicativo de desktop Etcher, de uma maneira que pode ser executada a partir de um terminal ou até mesmo usada em um script. Lembrando que o Etcher CLI é atualmente experimental, sendo assim devemos proceder com cautela e relatando problemas.

### Instalando

```
wget  https://github.com/balena-io/etcher/releases/download/v1.4.6/etcher-cli-1.4.6-linux-x64.tar.gz
```

```
sudo tar xfz etcher-cli-1.4.6-linux-x64.tar.gz  -C /opt
```

```
sudo mv /opt/etcher-cli-1.4.6-linux-x64-dist/* /opt
```

```
etcher -v
```

### Utilizando

O dispositivo que desejo usar como mídia bootável é o **sdd**

```
etcher -d /dev/sdd  slitaz-rolling.iso
```

![etcher-cli](https://j.gifs.com/D9XywY.gif)

------

### Outros exemplos

```
etcher.js raspberry-pi.img
```

```
etcher.js -d /dev/disk2 -y rpi.img
```

```
etcher.js -d /dev/disk2 ubuntu.iso
```

```
etcher.js --no-check raspberry-pi.img
```

### Ajuda

–help, -h show help

#####  Fontes
https://www.balena.io/etcher/cli/