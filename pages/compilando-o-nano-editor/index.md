O editor nano é conhecido por sua simplicidade entre as opções oferecidas no ambiente linux, apesar de oferecer algumas funções que muitos usuário desconhecem, mantê-lo sempre atualizado garante ao utilizador experimentar as últimas funções implementadas, porém muitas distros o oferece em versões bem antigas se comparado à disponível no site.

A boa notícia é que sua compilação em simples e rápida como a filofia de uso do próprio programa
abaixo segue as etapas de compilação e instalação

Site Oficial do Editor
https://www.nano-editor.org/

Página de Download
https://www.nano-editor.org/download.php



### 1 ) Entre como usuário root

```
su
```



#### Criar um diretório temporário para a instalação 

```
mkdir /tmp/arquivos
```

```
cd /tmp/arquivos
```


Antes de compilar o Nano, precisaremos instalar a biblioteca NCurses que provê uma API para o desenvolvimento de interfaces em modo texto. Garantindo também uma otimização quanto às mudanças de telas, reduzindo a latência quando se utiliza acesso remoto via shells, é desenvolvida pelo [Projeto GNU](https://pt.wikipedia.org/wiki/Projeto_GNU).



### 2 ) Instalando o NCurses



#### Baixando

```
wget http://ftp.gnu.org/pub/gnu/ncurses/ncurses-5.6.tar.gz
```



#### Descompactando

```
tar zxvf ncurses-5.6.tar.gz
```
 ```
cd ncurses-5.6
 ```

####  

#### Compilando

```
./configure
```
 ```
make
 ```

 ```
make install
 ```

### 3) Instalando o Editor Nano;

#### Baixando

```
wget https://www.nano-editor.org/dist/v2.8/nano-2.8.7.tar.gz
```
 ```
tar -zxvf nano-2.8.7.tar.gz
 ```
 ```
cd nano-2.8.7
 ```



#### Compilando

```
./configure
```

```
make
```
```
make install
```
Agora execute o nano, se tudo ocorreu conforme o esperado a seu editor está operando na versão 2.8.7 a última disponível enquando publicávamos este post.



```
nano --version
```



##### Fonte

http://mybookworld.wikidot.com/compile-nano-from-source

