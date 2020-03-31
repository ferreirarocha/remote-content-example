O roteador **TD5136v2** fornecido pela operadora OI vem com alguns recursos de firewall bloqueados no interface gráfica, este bloqueio vem em apenas algumas versões , sobre o nat isso dificulta o seu uso quando você quer disponibilizar algum serviço em sua rede.

Abaixo apresentamos como contornar esse problema.

# Mão na massa

Faça backup de sua configurações antes de começar esse tutorial, acesse o seu roteador via web e navege até

**MAINTENANCE** >> **Backup** e **Restore**

Clique em **backup** será baixado o arquivo config.xml contendo todas as suas configurações atuais, guarde-o para necessidades futuras.

**Informações do equipamento**

![img](https://4.bp.blogspot.com/-BFG8RdRse9Q/WNZvhBFCJPI/AAAAAAAATjY/dQwwkaRbS14P9thdrZo5kgNc-eqE_jzxgCLcB/s1600/20161230153900-info-roteador.PNG)

Se estiver em um sistema operacional windows, instale o programa [**putty**](https://the.earth.li/~sgtatham/putty/latest/x86/putty.exe), para realizar o acesso via console ao roteador, se etiver em qualquer Unix, basta abrir o seu terminal favorito e acessar o equipamento, Caso não consiga acessar o roteador via ssh, entre com a seguinte sintaxe para lograr êxito:

```
ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 10.10.50.1  -l admin
```

## Acessando o roteador via ssh

Em meu cenário o roteador está com o ip 10.10.50.1 e o usuário admin habilitado. Se não tiver um terminal dissponível, baixe o [Putty aqui](https://the.earth.li/~sgtatham/putty/latest/x86/putty.exe) Agora como na figura insira o usuário admin seguido de @ e ip do roteador.

![img](https://2.bp.blogspot.com/-L8OkR0Yh6-o/WNZvui2Sr4I/AAAAAAAATjc/gx6l0PtAqBAaBRjdHCpjYGg6WkaKMV0mwCLcB/s1600/20161230174448-telea-de-login-putty.png)

Entre com a **senha** do admin, o padrão de fábrica é password, na dúvida confira na traseira do equipamento. Após acessar o roteador digite:

```
help
```

para obt er as opções de comandos e se familiarizar com o ambiente, nesse tutorial trabalharemos com o comando **config** seguido do função **apps** que é a interace cli para gerenciar o NAT desse roteador.Basicamente trabalharemos com a sintaxe*config apps* e suas variantes listadas abaixo: Logo abaixo estão as funções disponíveis para o comando :

```
config apps
```

**enable** Ativa o servidor virtual responsável pelo *masquerading* (mascaramento) dos ips em sua rede interna.

**disable** Desativa o servidor virtual responsável pelo *masquerading* (mascaramento) dos ips em sua rede interna, isso é extremamente necessário quando você não estiver publicando serviços na internet, evitando assim brechas de segurança em seu roteador.

**add** Adiciona una nova regra NAT ao seu roteador.

**del** Deleta uma regra nat em seu roteador.

**show** Exibe todas as regras NAT aplicadas em seu roteador.

**help** Exibe a lista de funções presente em config apps.

```
enable  enable
            enable Virtual-Server
disable disable
        disable Virtual-Server
add     add [Name] [Status] [Private IP] [Public Port] [Private Port] [Protocol],
        ex. add Test01 enable 192.168.1.120 88-99 66-77 both
        ex. add Test02 disable 192.168.1.123 84-95 67-78 tcp
        add a rule
del     del [Index],
        ex. del 5
        del a rule
show    show
        list all application-rules
help    help
        apps help
```

Antes de trabalhar com o redirecionamento de portas, primeiro devemos conferir se o firewall possui alguma regra que libere o tráfego de entrada em nossa rede. Execute o seguinte comando para conferir esse detalhe importante;

```
config firewall default show
```

![imagem](https://2.bp.blogspot.com/-ewoD0IqUOfI/WNZv5nIjcEI/AAAAAAAATjg/Yz1roBdcYP8kaxH7Py1aQsQg2xdE25mBQCLcB/s1600/config-firewall-default-show-sem-regra-in.PNG)

Para permitir acesso externo deverá existir uma regra com **interface** *WAN* ou *ALL* e **direction** *Incoming* coma ação **Permit**, e como podemos observar não há uma regra com partâmentos que permitam a conexão externa ao nosso roteador e a nossa rede interna, sendo assim devemos adicioná-la pois de nada adiantaria criarmos as regras de redirecionamento mais adiante.

# Criando regra de entrada no Firewall

```
config firewall default add Enable WAN-LIVRE WAN In Permit
```

![imagem2](https://2.bp.blogspot.com/-34QAK4Z47A4/WNZwCj3lrUI/AAAAAAAATjk/YXCMeZ8Ed6Mush8xkasnTnKt4CYcI1lkACLcB/s1600/config-firewall-default-show.PNG)

Agora a regra de número 3 nos possibilita o acesso aos serviços internos de nossa rede. **Deletando regra de firewall** Caso queira deletar alguma regra, a sintaxe é simples, veja:

```
config firewall default del numero-da-regra
```

Por exemplo, gostaria de deletar a regra de número três de nome WAN-Livre, o comando seria o seguinte:

```
 config firewall default del 3
```

# NAT

## Desativando o servidor virtual (NAT)

```
config apps disable
```

![imagem3](https://2.bp.blogspot.com/-qR3N_kVZsQI/WNZwISWvt8I/AAAAAAAATjo/h6gKNWmjG8QKxyF9EPzFTQWiSSfrLQV4QCLcB/s1600/config-apps-disable.PNG)

A Saída *virtual server current settings* **Disabled** indica que agora o servidor virtual está desativado. Executando novamente o comando:

```
config apps show
```

Veremos que o servidor virtual encontra-se desativado

![img](https://3.bp.blogspot.com/-vvMDei4_jwk/WNZwTFBZaRI/AAAAAAAATjs/1cx7kjgpx3UL18K2wm4CKxhE0es3DQ9ygCLcB/s1600/config-apps-show-disable.PNG)

## Ativando o servidor virtual (**NAT**)

```
config apps enable
```

![img](https://1.bp.blogspot.com/-J09sbUWpzzs/WNZwZ3r06KI/AAAAAAAATjw/d-WWKXmZ08o8M7sSwe6985mZGbh0upoEQCLcB/s1600/config-apps-enable.PNG)

Execute o comando e verá logo abaixo se houver é claro, as suas regras **NAT**, e o status Ativado do servidor.

```
config apps show
```

![img](https://4.bp.blogspot.com/-79AFz_gOnzI/W6YhLGFv44I/AAAAAAAACb0/a5LI15EOZUkDtJy_jYY46aL5ywd1fIFEgCLcBGAs/s1600/config-apps-show-enable.PNG)

## Criando regras NAT

Para criar uma regra é bem simples basta seguir a seguinte sintaxe de inserção

```
add     add [Name] [Status] [Private IP] [Public Port] [Private Port] [Protocol]
```

Como exemplo vamos inseir uma regra para o Moodle em execução no servidor de ip **10.10.50.10** e disponível na porta **80** , criaremos uma segunda regra para um sistema de **helpdesk** em execução no servidor **10.10.50.8** também disponível na porta **80**.

*Veja a tabela:*

| Serviço  | Configs  |             |               | -------- | -----------   | ------------- |
| :------- | :------- | :---------- | :------------ | :------- | :------------ | :------------ |
| Moodle   | Servidor | 10.10.50.10 | Porta Interna | 80       | Porta Externa | 8181          |
| Helpdesk | Servidor | 10.10.50.8  | Porta Interna | 80       | Porta Externa | 80            |

## NAT Para o Moodle

Inserindo Nat para o Moodle instalado no servidor com ip 10.10.50.10 , e operando na porta 80, com a opção **both** o nat será habilitado tando para o protocolo **TCP** quando para o protocolo **UDP**

```
config apps  add  Moodle enable  10.10.50.10 8181 80 both
```

Conferindo a nova regra

```
config apps show
```

A nova regra entra com o último ID da tabela nat veja o exemplo:

```
#26     Moodle  Enabled Any	10.10.50.10	8181	80	TCP/UDP
```

## NAT Para o Helpdesk

Inserindo Nat para o Helpdesk em execução no servidor com ip 10.10.50.10

```
config apps  add  helpdesk  enable  10.10.50.8 88 80 both
```

## Conferindo a nova regra:

```
config apps show
#27     helpdesk        Enabled Any     10.10.50.8      88      80      TCP/UDP
```

## Deletando regas NAT

Para deletar uma regra é bem simples a sintaxe é

```
config apps  del  [Index]
```

Onde index é o número da regra, suponhamos que eu queira deletar a regra recém criada para o helpdek, bastaria eu obter o id, que nesse caso é 27 e colocar após o del.*Exemplo:*

```
config apps del 22
```

Se ainda estiver em dúvida sobre a sintaxe do nat para esse roteador basta digitar

```
config apps help
```

Para obter ajuda.

Fico por aqui !