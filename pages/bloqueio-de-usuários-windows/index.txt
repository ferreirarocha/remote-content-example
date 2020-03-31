# Objetivo

Foi nos solicitado o recurso de onde os colaboradores do escritório não pudessem gravar arquivos em seu espaço de usuário, isso inclui o Desktop, Documentos, Vídeos, Pictures, e qualquer área onde não se faça necessário a gravação de arquivos, em nosso ambiente algumas exceções foram aplicadas como salvar arquivos na pasta downloads, e  deixar liberado a pasta  Documents Arquivos do Outlook.



# Problemas e Riscos

Apesar de ser uma tarefa possível alguns cuidados devem ser tomados nesse procedimento, o primeiro é não remover a opção  de gravação na pasta  appdata e  também nos arquivos NETUSER*, caso isso ocorra o usuário não conseguirá acessar novamente a sua sessão, o que  o levará a  entrar no perfil temporário sempre  que efetuar um novo login.


# Desenvolvimento

Apesar de ser uma tarefa possível alguns cuidados devem ser tomados nesse procedimento o primeiro é não remover a opção  de gravação na pasta  appdata e  também nos arquivos NETUSER*, caso isso ocorra o usuário não conseguirá acessar novamente a sua sessão, o que  o levará a  entrar no perfil temporário sempre  que iniciar uma sessão.


## Primeira etapa : Desabilitar Herança de  permissões
A primeira etapa do processo consiste em desabilitar a herança de permissões nos diretórios que não queremos que haja alteração de permissão, é o caso do diretório %appdata%,  Downloads, Arquivos do Outlook e os arquivos NETUSER*.

```javascript
ICACLS %userprofile%\appdata /inheritanceLevel:d
ICACLS %userprofile%\NTUSER* /inheritanceLevel:d
ICACLS "%userprofile%\Downloads" /inheritanceLevel:d
ICACLS "%userprofile%\Documents\Arquivos do Outlook" /inheritanceLevel:d
```

### Sobre o ICACLS

**ICACLS** 

Exibe ou modifica DACLs (listas de controle de acesso discricionário) em arquivos especificados e aplica DACLs armazenadas a arquivos em diretórios especificados.

***
**InheritanceLevel**  
São as permissões que um diretorio ou arquivos herdaram de uma pasta superior, esse comportamento pode ser alterado utilizando as flags d,e,r em nosso projeto utilizaremos a flag **d**  que desabilita a herança e copia as ACEs, como podemos verificar nas demais opcoes abaixo;

```javascript 
/InheritanceLevel: [e|d|r]	Define o nível de herança:
	e -habilita enheritance
 	d -desabilita a herança e copia as ACEs
 	r -remove todas as ACEs herdadas
```

***

### Sobre os arquivos NTUSER
Arquivo criado com o CLFS (Common Log File System) da Microsoft, um componente do Windows usado para criar logs de transação; contém informações de leitura e gravação de transações sobre alterações nos dados de restauração do registro do sistema ou do usuário, que são armazenados nos arquivos NTUSER.DAT ou SCHEMA.DAT; usado com um arquivo .BLF para incluir o log de transações

---

```javascript
NTUSER.DAT
ntuser.dat.LOG1
ntuser.dat.LOG2
NTUSER.DAT{####}.TxR.0.regtrans-ms
NTUSER.DAT{####}.TxR.1.regtrans-ms
NTUSER.DAT{####}.TxR.2.regtrans-ms
NTUSER.DAT{####}.TxR.blf
NTUSER.DAT{####}.TM.blf
NTUSER.DAT{####}.TMContainer00000000000000000001.regtrans-ms
NTUSER.DAT{####}.TMContainer00000000000000000002.regtrans-ms
ntuser.ini
Ntuser.pol
```

***

### Sobre a pasta AppData

A pasta AppData reúne todos os dados de softwares do Windows instalados no seu computador,  cada usuario possui uma pasta apptdata, contendo informacoes sobre configracao de aplicacao, cache de aplicacoes e outros recursos, e extremamente importante que  suas permissões nao sejam alteradas.


## Segunda etapa: Permitir gravação em pastas selecionadas

A Segunda etapa  concedede permissão de modificação nas pastas  Downloads, Arquivos do Outlook.
```javascript
ICACLS "%userprofile%\Downloads" /Grant %username%:M
ICACLS "%userprofile%\Documents\Arquivos do Outlook" /Grant %username%:M

```

## Terceira etapa: Desabilitar a gravação

A Terceira etapa concede apenas permissões de leitura e execução nas demais pastas e arquivos do espaço do usuário,  portando diretórios como Desktop, Documentos, Vídeos, Imagens e etc, não poderão receber gravação por parte do usuário corrente, exceto do administrador e a quem a política não for aplicada.

Conceder acesso somente leitura e execução na pasta home dom usuário
```javascript
REM 3ª ETAPA: 
ICACLS %USERPROFILE% /GRANT:R %USERNAME%:(OI)(CI)RX
```



## Quarta etapa: Criando o script

Acima definimos o que necessitamos bloquear e liberar  na home dos nosso usuários, agora é chegada a hora de criar o script e salvá-lo em um local acessível na rede, como boa prática definimos o netlogon

```javascript
ICACLS %userprofile%\appdata /inheritanceLevel:d
ICACLS %userprofile%\NTUSER* /inheritanceLevel:d
ICACLS "%userprofile%\Downloads" /inheritanceLevel:d
ICACLS "%userprofile%\Documents\Arquivos do Outlook" /inheritanceLevel:d

ICACLS "%userprofile%\Downloads" /Grant %username%:M
ICACLS "%userprofile%\Documents\Arquivos do Outlook" /Grant %username%:M

ICACLS %USERPROFILE% /GRANT:R %USERNAME%:(OI)(CI)RX
```

***
### Savando o Script no netlogon

```javascript
\\sit-srv001\netlogon\bloqueio_de_espaco_do_usuario.bat
```


## Quinta etapa: Criando GPO

Apos a criação do script de controle de permissões, é chegada a hora de criarmos a GPO, em nosso ambiente faremos algumas exceções entre elas, a equipe de TI nao sera afetada e a diretoria, não receberão a aplicação da GPO, faremos a GPO na raiz do dominio mas isso fica a criterio da particularidade de cada  ambiente.
abra o GPO.MSC navegue ate os domnio crie uma gpo de nome sugestivo como *Bloqueio de Espaco do Usuario*

Na aba delegacao insira os grupos aos quais vc nao queira que esta gpo seja aplicada.


![](Captura%20de%20tela%20de%202020-02-23%2017-03-02.png)



### GPO - Bloqueio de espaço de usuário

A GPO sera aplicada nos dois escopos  usuario e computador, no scopo do usuario aplicaremos o script de logon anteriormente criado   como segue na imagem abaixo;



#### Escopo do usuario
User Configuration > Policies > 	Windows Settings  > Scripts > Logon 

```javascript 
\\sit-srv001\NETLOGON\bloqueio_de_espaco_do_usuario.bat
```
![](Captura%20de%20tela%20de%202020-02-23%2019-12-03.png)

Ainda no escopo do usuario aplicaremos tambem a politica de restricao a aba seguranca nas propriedades de arquivos e pastas, assim dificultamos a obtencao do acesso por parte do usuario em quem tem acesso as pastas na rede e no computador.



User Configuration > Preferences > Windows Settings > Components Windows > File Exporer > Remove Tab Security


#### Escopo Computador
No escopo do computador sera aplicado a politica de restricao de acesso o filesystem, onde impediremos que o usuario consiga gravar tambem no %systemdrive% e na pasta do usuario publico.




## APRESENTAÇÃO NTFS

<style>.embed-container { position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; } .embed-container iframe, .embed-container object, .embed-container embed { position: absolute; top: 0; left: 0; width: 100%; height: 100%; }</style><div class='embed-container'><iframe src='https://www.youtube.com/embed/4ZoVVyrjn4Y' frameborder='0' allowfullscreen></iframe></div>

# Fontes
ICALS
https://docs.microsoft.com/pt-br/windows-server/administration/windows-commands/icacls

NTUSER
https://filememo.info/extension/regtrans-ms



# Conclusão

