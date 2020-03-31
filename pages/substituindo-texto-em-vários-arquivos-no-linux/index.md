Existem mil manerias de substituir um texto em vários arquivos no linux, dentre eles o comando sed e find, porém hoje vou-lhes apresentar o comando rpl, ele não é novo porém é pouco usado pelos novos adeptos do Linux.


Pois bem, de maneira rápida faremos a substituição da palavra Vita por Vida presente nos arquivos , Doc1.txt , Doc2 e Doc3.txt no diretório **/home/marcos/tmp.**

Dentro deles há um pequeno texto onde a palavra Vida foi erroneamente escrita em latim Vita, agora queremos alterá-la para o Português do Brasil.



> A Vita é um conceito muito amplo e admite diversas definições. Pode-se referir ao processo em curso do qual os seres vivos são uma parte, um espaço de tempo entre a concepção e a morte de um organismo. A Vita é condição de uma entidade que nasceu e ainda não morreu, é aquilo que faz com que um ser esteja vivo. E metafisicamente falando, a Vita é um processo contínuo de relacionamentos"


Para isso basta utilizarmos o comando rpl.

**Instalando o RPL**

Arch Linux e Derivados

Ubuntu e Derivados


**Utilizando o RPL**

Acesse o local onde estão os arquivos, ou execute o comando diretamente conforme o exemplo abaixo;



```
 rpl -R "Vita" "Vida" /home/marcos/tmp/*  
```


A alteração é bem rápida, e quando finalizada o camando nos informa quantas substituições foram realizadas, em nosso caso foram 9 veja a saída abaixo;



```
 Replacing "Vita" with "Vida" (case sensitive) (partial words matched)  
 ........  
 A Total of 9 matches replaced in 6 files searched.  
```


**Utilizando o comando cat para visualizar um arquivo alterado;**

```
 cat Arquivo1.txt  
```

> "A Vida é um conceito muito amplo e admite diversas definições. Pode-se referir ao processo em curso do qual os seres vivos são uma parte, um espaço de tempo entre a concepção e a morte de um organismo. A Vida é condição de uma entidade que nasceu e ainda não morreu, é aquilo que faz com que um ser esteja vivo. E metafisicamente falando, a Vida é um processo contínuo de relacionamentos".



O rpl aceita várias opções e filtros veja outro exemplo :



```
 rpl -Rwd -x'.txt' 'Vita' 'Vida' /home/marcos/tmp/*  
```

Aqui alteramos a palavra apenas em arquivos com o sufixo .txt através da flag -x.

Outras Opções

Para mais opções acesse a ajuda e o manual do programa.



```
 rpl help  
```

e

```
 man rpl  
```

##### Fonte: 

Comando RPL