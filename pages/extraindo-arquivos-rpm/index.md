São vários motivos que justificam alguém a descompactar um arquivo **rpm**, seja para estudos, teste de aplicação , entre outras opções, o fato é que podemos fazer isso usando o [**rpmextract**](https://github.com/dowjones/rpm-extract) ou o [**file-roller**](http://fileroller.sourceforge.net/features.html) que é um gerenciador de archive para o ambiente de área de trabalho GNOME.

## FILE-ROLLER

### Instalando

```
yay -S file-roller
```

### Baixando um pacote rpm

```
wget https://www.rpmfind.net/linux/fedora/linux/development/rawhide/Everything/x86_64/os/Packages/n/ncdu-1.13-4.fc29.x86_64.rpm
```

```
mkdir ncdu
```

#### Descompactando

O file-roller nos permite inclusive informar onde extrair os arquivos, nesse exemplo informamos o diretório **ncdu**

```
sudo file-roller --extract-to=ncdu ncdu-1.13-4.fc29.x86_64.rpm
```

Agora já temos o pacote extraído, podemos executá-lo diretamente caso não haja dependências, no caso do ncdu basta navegados no diretório **ncdu/usr/bin** e dentro dele executar o **ncdu**

```
cd  ncdu/usr/bin
```

```
./ncdu
```

## RMPEXTRACT

### Instalando

```
yay -Ss  rpmextract
```

#### Baixando um pacote rpm

```
mkdir  vlc ; cd vlc
```

```
wget https://www.rpmfind.net/linux/rpmfusion/free/fedora/releases/29/Everything/x86_64/os/Packages/v/vlc-3.0.5-4.fc29.i686.rpm
```

#### Descompactando

```
 rpmextract.sh vlc-3.0.5-4.fc29.i686.rpm
```

Agora já temos o pacote extraído, podemos inclusive executá-lo diretamente caso não haja dependências, no caso do vlc basta navegados no diretório usr/bin/ e dentro dele executar o **qvlc**

```
cd usr/bin/
```
```
./qvlc
```