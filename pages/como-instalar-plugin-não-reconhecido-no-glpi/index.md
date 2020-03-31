Plugins são essenciais para pontencializar as funcionalidades do GLPI, instalá-los  normalmente é uma tarefa simples;

Basta  baixarmos o plugin e descompactar no diretório  **/glpi/plugins/**

Porém há casos  e não sei ao certo o motivo, alguns plugins não tem o nome de diretório correspondente ao cadastrado no arquivo **.xml** no seu diretório,  isso pode ser um transtorno se o responsável pela  implantação não souber o seu nome correto ou faltar documentação sobre o assunto.

Para resolvermos isso basta obtermos essa informação no  arquivo .xml do plugin com o seguinte comando

aqui levaremos em conta que nosso GLPI está instalado em  ;

```
grep "key" nome-do-plugin/nome-do-plugin.xml
```



#### Exemplo

Vamos utilizar como exemplo o plugin nebackup

##### Baixanado  o plugin

```
wget https://github.com/jsamaniegog/nebackup/archive/master.zip
```

##### Descompactando

```
unzip master.zip
```

##### Identificando o nome do plugin

```
grep "key" nebackup-master/*.xml
```

saída

```
<key>nebackup</key>
```

o nome do plugin está entre as tags 

##### Enviando o plugin para o glpi

```
mv nebackup-master /var/www/html/glpi/plugins/nebackup
```

##### Habilitando o plugin

Acesse a área de plugin no GLPI em;

**Configurar** > **Plugins**  e habilite o plugin desejado.

![Captura de tela de 2018-01-25 08-29-35](https://2.bp.blogspot.com/-jsRa4Jryhhc/WmvXBr7jeAI/AAAAAAAAB4I/FOob7DS3dQAkOExUWWL-HkhxXGg7JhLYACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-25%2B08-29-35.png)

Quer saber mais sobre o criação de plugins  confira a documentação.

[Documentação Plugins GLPI](http://glpi-plugins.readthedocs.io/en/latest/)



Fico por aqui e até uma próxima oportunidade