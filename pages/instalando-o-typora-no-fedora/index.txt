O Typora é um editor WYSIWYG Markdown, possui um design minimalista , tendo como foco a não distração durante a criação de textos.

Em outro momento abordaremos sobre seus recursos, hoje trataremos de sua instalação na distribuição Fedora, lembrando ainda que o pacote rpm pode ser usado por outras distribuições como OpenSUSE , CentOS e Mageia.

# Mão na massa

### Dependências

Instale as dependências para a criação do pacote rpm.

```
sudo dnf install git rpmdevtools libXScrnSaver
```

### Clone o repositório

```
git clone https://github.com/RPM-Outpost/typora.git
cd  typora
```

### Crie o pacote

É possível criar o pacote para 32 e 64 bits

```
./create-package.sh x64
```

ou

```
./create-package.sh ia32
```

Ao finalizar a criação do pacote é questionado se você quer remover o não o diretório onde foi realizado o build, isso não afeta o pacote criado, portanto, fique tranquilo se você quiser excluí-lo.

> Remove the directory “/home/marcos/typora/work”? [y/N]:

![1538406807179](https://2.bp.blogspot.com/-dsfqehrpxmc/W7JTBlW-jII/AAAAAAABImY/4IsMbD2BbVY43fUwUSWBm9T2Twq_MN86QCLcBGAs/s1600/1538406807179-min.png)

Em seguida é possível iniciar a instalação do typora, confirme teclando **y**.

![1538406837801](https://3.bp.blogspot.com/-Mu3bqKiIVCM/W7JTRqKWWbI/AAAAAAABImg/Kv5LXozhyxk2pDzpg9FGg1yHviGtL0ewwCLcBGAs/s1600/1538406837801-min.png)

Você também pode salvar o mesmo pacote para instalar em ocasiões futuras ou até mesmo compartilhar com algum amigo, o arquivo fica no seguinte diretório.

*~/typora/RPMs/*

Para instalar manualmente basta fazê-lo via *dnf*

```
dnf install  RPMs/x86_64/typora-0.9.58-0.fc28.x86_64.rpm
```

Por hora é isso, em outro post abordarei o meu uso do typora no blog.

![typora](https://4.bp.blogspot.com/-zzpKW8AUV6k/W7Jd1u11i1I/AAAAAAABIm0/JO4pgKSiYjUkj_PYiUwKKJIHaOrFhAD9wCLcBGAs/s1600/typora.gif)

Fonte

https://github.com/RPM-Outpost/typora

https://typora.io/

https://twitter.com/typora

[hi@typora.io](mailto:hi@typora.io)