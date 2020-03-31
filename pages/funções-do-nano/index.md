O nano é um editor que por concepção preza pela facildiade e simplicidade de uso, talvez por isso muitas vezes seja subestimado e subutilizado.

Abaixo apresentamos 10 funções que talvez você não conheça, confira.

#### Backup de arquivos editados

*Exemplo*

```
nano -B /etc/fstab
```

Este recurso salva automáticamente um de backup quando alteramos algum arquivo, isso é muito interessante quanto estamos fazendo alguma manutenção nos arquivos de configuração no linux,  os arquivos salvos apresentam o **~** a frente.

#### Abrir o arquivo em uma linha específica

*Exemplo*

```
nano +123  /etc/fstab
```

A primeira vista este comando não parece tão útil mas a coisa muda de figura quando desejamos otimizar ao máximo a documentação e a consolidação de um processo.

#### Multibuffer

Assim como na maioria dos aplicativos com interface gráfica o nano possui o recurso de trabalhar com múltiplos arquivos na memória, algo essencial quando estamos excutando uma tarefa composta de multíplos arquivos de configuração.

*Exemplo*

```
nano -F /etc/fstab
```

#### Abrir o arquivos sem ler o conteúdo

Algumas vezes precisamos abrir o arquivo sem qualquer conteúdo como o arquivo /etc/fstab.
Com a opção -n do nano podemos fazer isso instantaneamente da seguinte forma;

*Exemplo*

```
nano -n /etc/fstab
```

#### Modo leitura

De maneira inversa ao modo -n o nano possibilita o modo somente leitura de um arquivo habilitando a opção -v

*Exemplo*

```
nano -v /etc/fstab
```

#### Mostrar números de linhas na frente do texto

Este é um recurso simples porém faz toda diferença quando estamos consolidando as etapas de um processo.

```
nano -l /etc/fstab
```

#### Ajuste suave de linhas compridas

Se você gosta de utilizar o cursor no modo CLI saiba que isso é possível no editor nano, basta habilitar a opção -g da aplição

```
ctrl + shit + 4
```

#### Comentar e descomentar a linha atual (ou linhas marcadas)

Podemos comentar e descomentar um intervalo de linhas ( marcadas ou não ) para isso basta executarmos o seguinte comando.

Para selecionar as linhas

```
Alt + A 
```

Para comentar e descomentar as linhas

```
Alt + 3
```

#### Ir para o fechamento do parênteses/colchetes/chaves

```
Alt + ]
```

Esse recurso é bem útil quando precisamos saber quando determinda função fechou baseada no fechamento de parentes, cochetes e as chaves.

#### Inserir outro arquivo no atual

```
Ctrl + R
```

Acionando o CRTL + R ou apertando a tecla F5 podemos inserir um arquivo no trabalho atual, algo interessante quando parte do trabalho ou conteúdo já foi elaborado e só nos resta complementar .

### Conclusão

Todas as funções aqui apresentadas podem ser acessadas abrindo o menu de ajuda do próprio editor Nano, que apesar de muitos não o levarem a sério como uma ferramenta de manunteção no linux, prova justamente o contrário. Pois graças a essa facilidade a primeira vista e um conjunto atrativo de funções que o complementa, o nano vai trilhando e deixando sua marca num mundo repleto de opções tão boas ou melhores se equiparadas a ele.


Fico por aqui e até uma próxima oportunidade.