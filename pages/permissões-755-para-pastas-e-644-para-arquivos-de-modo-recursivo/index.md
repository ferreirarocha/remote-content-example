Quando criamos alguma aplicação seja ela um script , programa ou  site, é comum iniciarmos com permissão total 777 em ambiente de  desenvolvimento e teste iniciais, porém se a aplicação for hospedada  online é provável que o provedor do serviço não aceite este mesmo nível  de acesso, é o caso do hostagtor e provavelmente muitos outros  provedores, alguns oferecem a opção de alteração recursiva, já outros  não. É nesse momento que podemos utilizar uma o shell  no linux.

Em nosso caso utilizaremos o comando `find` aliado a opção `-exec`, assim evitamos a triste tarefa de alteração manual de centenas de diretórios ( pastas ).

Pois bem, para ajustar a permissão de arquivos de acordo com as normas de nosso servidor/provedor faremos da seguinte forma.

```
find diretorio-da-aplicação -type f -exec chmod 644 {} \;
```

E para ajustar o nivel de permissão dos diretórios executamos o seguinte comando;

```
find diretorio-da-aplicação -type d -exec chmod 755 {} \;
```

Cada objeto encontrado pelo comando `find` dentro do *diretorio-da-aplicação* terá sua permissão alterada de acordo a flag`-type f`para arquivos *files* e `-type d` para diretórios.

Os comandos `chmod 644` ou `chmod 755`serão executados sempre que necessário através da opção `-exec`.

Agora podemos compactar e enviá-lo ao nosso servidor.

Caso tenha interesse pela compactação via terminal é simples, veja o exemplo abaixo;

```
cd pasta-da-minha-aplicacao/  ;  zip -r minha-aplicacao.zip  
```

O que fizemos aqui foi entrar no diretório *pasta-da-minha-aplicacao* e com o `;` executamos o comando `zip -r minha-aplicacao.zip` logo em seguida.

Ao realizar a compactação dentro do diretório, copiamos somente os  subdiretórios que nos interessam ,algo simples porém deixa o arquivo  zipado mais organizado.

Até a próxima.