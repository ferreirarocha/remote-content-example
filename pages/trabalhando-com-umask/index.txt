
**Umask**, abreviação para máscara de usuário (**u**ser **mask**),  é um recurso  em ambientes posix que definem as permissões padrões para novos arquivos e diretórios criados.

Há 2 métodos para aplicarmos o umask podendo ser  **simbólico**  ou  **octal**;

#### Umask Octal

O método octal é bem fácil pois o que faremos é um simples cálculo subtraindo a permissão full(**777**) do diretórios  por permissão desejada.

O resultado é  evidentimente o **umask**

Veja neesse exemplo necessitamos que os diretórios tenham permissão 775, então faremos a seguinte subtração.

![subtração](https://3.bp.blogspot.com/-Zhg7R9CR0cM/WmwMQ95ygxI/AAAAAAAAB6Q/TwXn4ZW9gUY4cgsnr3SwNbY6aFDMDwsogCPcBGAYYCw/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-27%2B03-20-52.png)

O umask  será o **002**  para o definirmos imediatamente basta excutarmos o comando;

```
umask 002
```

Para  definirmos como padrão do usuário é necessário inserí-lo no  **.bashrc** da seguinte forma;

```
echo "umask 0002" >> ~/.bashrc
```

ou

```
echo "umask 0022" >> /home/SEU-USUARIO/.bashrc
```

Agora  crie um diretório qualquer e veja as  permissões  concedidas;

Exemplo

```
mkdir alfabech
```

Listando permissões

```
stat -c "%a %n %U %G" alfabech/
Saída

775 alfabech/ marcos marcos
```

Como podemos ver o diretório rebeceu a  permissão 775 , sem qualquer  intervenção adicional do usuário

#### Umask Simbólico

A segunda forma   é o umask simbólico que também possui uma simples  operação, basta  definirmos o nível de permissão simbólica diretamente  no umask;

Novamente desejamos  definir a permissão 775 em nossos diretório, então faremos da seguinte forma;

#### Nossa tabela

| Permissão | Proprietário |
| --------- | ------------ |
| rwx = 7   | para dono    |
| rwx = 7   | para  grupo  |
| r-x  = 5  | para outros  |

Baseado nas informações acima podemos denir o seguinte umask

```
umask u=rwx g=rwx, o=r-x
```

Novamente  caso queira torná-lo com padrão insira no arquivo  .bashrc do usuário.

```
echo umask u=rwx g=rwx, o=r-x >> /home/SEU-USUARIO/.bashrc
```

### Conclusão

A primeira vista   alterar o umask não parece muito útil, mas se  analisado de perto  é possível ver  ganhos reais nessa estratégia como  baixa intervenção do administrador do sistema   quanto a alteração  permissão de arquivos e diretórios, padronização de processos, esses  benefícios tornan-se mais visíveis quanto introduzimos outras práticas  como preservar o umask ao descompactar um arquivo, assunto que  trataremos no próximo post.

Abaixo deixo fontes para complementar o assunto 😃

Fontes

https://www.vivaolinux.com.br/artigo/Umask-para-leigos

http://www.blogdodanilo.com.br/2013/04/configurando-umask-linux.html

https://pt.wikibooks.org/wiki/Guia_do_Linux/Iniciante%2BIntermediário/Permissões_de_acesso_a_arquivos_e_diretórios/umask

http://www.uniriotec.br/~morganna/guia/umask.html

https://www.infowester.com/linuxpermissoes2.php

http://www.unixmantra.com/2013/04/unix-linux-file-permissions.html