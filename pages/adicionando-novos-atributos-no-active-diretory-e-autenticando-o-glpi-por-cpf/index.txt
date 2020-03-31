O GLPI provê vários meios de autenticação incluindo Diretórios LDAP,  Servidores de e-mail, Servidor CAS, Autenticação por certificado x509    e autenticação enviada na requisição HTTP , as opções são várias, com a possibilidade de estendê-las alterando o **campo de login** nas  configurações do Diretórios LDAP, podemos utilizar atributo mail  cadastrado nos usuários do AD ou LDAP para ser o atributo de  identificação, esse email pode ser apenas um recurso de rede local,  dessa forma não representando um custo financeiro para a empresa, em  contra partida ele não será válido para comunicação externa.
Com esta estratégia podemos utilizar a autenticação por Email, CPF, Matrícula ou qualquer outro Identificador que não seja suscetível a homônimos, ou  qualquer outro identificador em duplicata.
Veja este artigo interessante sobre [Como diferenciar pessoas homonimas.](http://www.certisignexplica.com.br/como-diferenciar-a-identidade-pessoas-homonimas/)
Abaixo apresentamos o portal do IBGE que mostra quantas pessoas tem o seu nome e os mais comuns. 



[![img](https://2.bp.blogspot.com/-kxIE1-DqjCY/WfdKVA7a1vI/AAAAAAAABBk/VF0QeunPLm0-XzqrZSjEDwHGJrvbzYWswCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-30%2B13-47-57.png)](https://2.bp.blogspot.com/-kxIE1-DqjCY/WfdKVA7a1vI/AAAAAAAABBk/VF0QeunPLm0-XzqrZSjEDwHGJrvbzYWswCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-30%2B13-47-57.png)


Optamos pela autenticação **CPF** e **Senha,** agora adicionaremos o novo atributo **cpf** à(s) classes do Active Diretory.

###  

### Etapa 1 ) Instalando o Active Directory Scheme 

#### **1.1 ) O que isso faz ?**

O Active Directory Schema é uma ferramenta administrativa que pode ser  usada para atualizar e alterar os schemas do Active Directory.



#### 1.2 ) Instalando schmmgmt 

Para instalá-lo, você precisa executar o comando regsvr32 schmmgmt.dll usando um prompt como administrador.

```
C:\Users\Administrator> regsvr32 schmmgmt.dll 
```

### Etapa 2 ) Criando um novo Atributo do Active Directory

Uma vez que a ferramenta administrativa do Esquema do Active Directory esteja instalada, ela estará disponível no MMC. 
Aproveite que seu prompt de comando está aberto e abra o **MMC**



[![img](https://1.bp.blogspot.com/-dqew0_EmamE/WfYHYdpVFXI/AAAAAAAAA7E/PxP25k6b8jM4HwwF7bOxy1onVWWTKcr1ACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B14-50-06.png)](https://1.bp.blogspot.com/-dqew0_EmamE/WfYHYdpVFXI/AAAAAAAAA7E/PxP25k6b8jM4HwwF7bOxy1onVWWTKcr1ACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B14-50-06.png)


Adicione o Active Directory Scheme ao console MMC



[![img](https://3.bp.blogspot.com/-RvY49mla4jc/WfdXTBqTm_I/AAAAAAAABCk/rqLY0chMJ_4hugVaTbnTK8WiGMQqdkqRQCLcBGAs/s1600/mmc.png)](https://3.bp.blogspot.com/-RvY49mla4jc/WfdXTBqTm_I/AAAAAAAABCk/rqLY0chMJ_4hugVaTbnTK8WiGMQqdkqRQCLcBGAs/s1600/mmc.png)

Para criar um novo atributo, proceda da seguinte maneira:

Na ferramenta administrativa Active Directory Scheme, clique com o botão  direito do mouse em Atributos e selecione Criar Atributo.



[![img](https://4.bp.blogspot.com/-3i9UfmGEz78/WfYJGHUF6WI/AAAAAAAAA7U/zenD2CHDAJopm4uOQo_88yGZRZisnxeOwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B14-57-06.png)](https://4.bp.blogspot.com/-3i9UfmGEz78/WfYJGHUF6WI/AAAAAAAAA7U/zenD2CHDAJopm4uOQo_88yGZRZisnxeOwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B14-57-06.png)



Clique em Continuar (O aviso que é exibido é informar que a criação de um novo atributo do  Active Directory não é uma operação reversível e que não pode ser  removida uma vez feita).



#### Preencha as seguintes informações:

**Common Name** (Nome comum )
**LDAP Display Name** ( Nome de exibição LDAP )
**Unique X500 Object ID** ( OID Gerado Pelo Script VBS)
**Description** (Opcional)
**Syntax** (http://technet.microsoft.com/en-us/library/cc961740.aspx Jump )
**Minimum** (Opcional)
**Maximum** (Opcional)
**Multi-Valued** (Utilize-o apenas se você quiser ter um atributo de valor múltiplo)

Para a criação de um novo atributo é necessário que saibamos qual OID está sendo utilizado pelo ambiente e registrá-lo em **Unique X500 Object ID**.
Para isso copie o código vbs abaixo no notepad e salve-o no desktop como oid.vbs, logo após executar o VBS.

Este mesmo script está diposnível em 
https://gallery.technet.microsoft.com/scriptcenter/56b78004-40d0-41cf-b95e-6e795b2e8a06

```
' oidgen.vbs 
'  
' THIS CODE AND INFORMATION IS PROVIDED "AS IS" WITHOUT WARRANTY OF ANY KIND, EITHER EXPRESSED  
' OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE IMPLIED WARRANTIES OF MERCHANTABILITY AND/OR  
' FITNESS FOR A PARTICULAR PURPOSE. 
' 
' Copyright (c) Microsoft Corporation. All rights reserved 
' 
' This script is not supported under any Microsoft standard support program or service.  
' The script is provided AS IS without warranty of any kind. Microsoft further disclaims all 
' implied warranties including, without limitation, any implied warranties of merchantability 
' or of fitness for a particular purpose. The entire risk arising out of the use or performance 
' of the scripts and documentation remains with you. In no event shall Microsoft, its authors, 
' or anyone else involved in the creation, production, or delivery of the script be liable for  
' any damages whatsoever (including, without limitation, damages for loss of business profits,  
' business interruption, loss of business information, or other pecuniary loss) arising out of  
' the use of or inability to use the script or documentation, even if Microsoft has been advised  
' of the possibility of such damages. 
' ---------------------------------------------------------------------- 
Function GenerateOID() 
    'Initializing Variables 
    Dim guidString, oidPrefix 
    Dim guidPart0, guidPart1, guidPart2, guidPart3, guidPart4, guidPart5, guidPart6 
    Dim oidPart0, oidPart1, oidPart2, oidPart3, oidPart4, oidPart5, oidPart6 
    On Error Resume Next 
    'Generate GUID 
    Set TypeLib = CreateObject("Scriptlet.TypeLib") 
    guidString = TypeLib.Guid 
    'If no network card is available on the machine then generating GUID can result with an error. 
    If Err.Number <> 0 Then 
        Wscript.Echo "ERROR: Guid could not be generated, please ensure machine has a network card." 
        Err.Clear 
        WScript.Quit 
    End If 
    'Stop Error Resume Next 
    On Error GoTo 0 
    'The Microsoft OID Prefix used for the automated OID Generator 
    oidPrefix = "1.2.840.113556.1.8000.2554" 
    'Split GUID into 6 hexadecimal numbers 
    guidPart0 = Trim(Mid(guidString, 2, 4)) 
    guidPart1 = Trim(Mid(guidString, 6, 4)) 
    guidPart2 = Trim(Mid(guidString, 11, 4)) 
    guidPart3 = Trim(Mid(guidString, 16, 4)) 
    guidPart4 = Trim(Mid(guidString, 21, 4)) 
    guidPart5 = Trim(Mid(guidString, 26, 6)) 
    guidPart6 = Trim(Mid(guidString, 32, 6)) 
    'Convert the hexadecimal to decimal 
    oidPart0 = CLng("&H" & guidPart0) 
    oidPart1 = CLng("&H" & guidPart1) 
    oidPart2 = CLng("&H" & guidPart2) 
    oidPart3 = CLng("&H" & guidPart3) 
    oidPart4 = CLng("&H" & guidPart4) 
    oidPart5 = CLng("&H" & guidPart5) 
    oidPart6 = CLng("&H" & guidPart6) 
    'Concatenate all the generated OIDs together with the assigned Microsoft prefix and return 
    GenerateOID = oidPrefix & "." & oidPart0 & "." & oidPart1 & "." & oidPart2 & "." & oidPart3 & _ 
        "." & oidPart4 & "." & oidPart5 & "." & oidPart6 
End Function 
'Output the resulted OID with best practice info 
Wscript.Echo "Your root OID is: " & VBCRLF & GenerateOID & VBCRLF & VBCRLF & VBCRLF & _ 
    "This prefix should be used to name your schema attributes and classes. For example: " & _ 
    "if your prefix is ""Microsoft"", you should name schema elements like ""microsoft-Employee-ShoeSize"". " & _ 
    "For more information on the prefix, view the Schema Naming Rules in the server " & _  
    "Application Specification (http://www.microsoft.com/windowsserver2003/partners/isvs/appspec.mspx)." & _ 
    VBCRLF & VBCRLF & _ 
    "You can create subsequent OIDs for new schema classes and attributes by appending a .X to the OID where X may " & _ 
    "be any number that you choose.  A common schema extension scheme generally uses the following structure:" & VBCRLF & _ 
    "If your assigned OID was: 1.2.840.113556.1.8000.2554.999999" & VBCRLF & VBCRLF & _ 
    "then classes could be under: 1.2.840.113556.1.8000.2554.999999.1 " & VBCRLF & _  
    "which makes the first class OID: 1.2.840.113556.1.8000.2554.999999.1.1" & VBCRLF & _ 
    "the second class OID: 1.2.840.113556.1.8000.2554.999999.1.2     etc..." & VBCRLF & VBCRLF & _ 
    "Using this example attributes could be under: 1.2.840.113556.1.8000.2554.999999.2 " & VBCRLF & _ 
    "which makes the first attribute OID: 1.2.840.113556.1.8000.2554.999999.2.1 " & VBCRLF & _ 
    "the second attribute OID: 1.2.840.113556.1.8000.2554.999999.2.2     etc..." & VBCRLF & VBCRLF & _ 
     "Here are some other useful links regarding AD schema:" & VBCRLF & _ 
    "Understanding AD Schema" & VBCRLF & _ 
    "http://technet2.microsoft.com/WindowsServer/en/Library/b7b5b74f-e6df-42f6-a928-e52979a512011033.mspx " & _ 
    VBCRLF & VBCRLF & _ 
    "Developer documentation on AD Schema:" & VBCRLF & _ 
    "http://msdn2.microsoft.com/en-us/library/ms675085.aspx " & VBCRLF & VBCRLF & _ 
    "Extending the Schema" & VBCRLF & _ 
    "http://msdn2.microsoft.com/en-us/library/ms676900.aspx " & VBCRLF & VBCRLF & _ 
    "Step-by-Step Guide to Using Active Directory Schema and Display Specifiers " & VBCRLF & _ 
    "http://www.microsoft.com/technet/prodtechnol/windows2000serv/technologies/activedirectory/howto/adschema.mspx " & _ 
    VBCRLF & VBCRLF & _ 
    "Troubleshooting AD Schema " & VBCR & _ 
    "http://technet2.microsoft.com/WindowsServer/en/Library/6008f7bf-80de-4fc0-ae3e-51eda0d7ab651033.mspx  " & _ 
    VBCRLF & VBCRLF 
```

*OBS: Você pode dár dois cliques para executar o VBS ou executar o comando*

```
C:\Users\Administrator>  cscript.exe Desktop\oid.vbs
```

O script tem uma saida bem poluída mas o que nos interessa é o OID logo  abaixo de "Your root OID is:", copie este código e insira em Unique  X500 Object ID



[![img](https://3.bp.blogspot.com/-DouywV7uIcM/WfYPitaUMDI/AAAAAAAAA78/NJ9m02VOBW8yYNl2cYyAlh2FLUCoD7--ACLcBGAs/s1600/saidaOID.png)](https://3.bp.blogspot.com/-DouywV7uIcM/WfYPitaUMDI/AAAAAAAAA78/NJ9m02VOBW8yYNl2cYyAlh2FLUCoD7--ACLcBGAs/s1600/saidaOID.png)


Insira uma descrição para o novo atributo, apesar de não ser necessário isso  nos ajuda ao longo do tempo, quando realizamos várias customizações nas  classes.



[![img](https://3.bp.blogspot.com/-6m2w11g9EYE/WfYNyM1gIWI/AAAAAAAAA7w/hUT_RD8NNR0lwqFxw-NPlJLrg5dSwELKQCEwYBhgL/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-17-20.png)](https://3.bp.blogspot.com/-6m2w11g9EYE/WfYNyM1gIWI/AAAAAAAAA7w/hUT_RD8NNR0lwqFxw-NPlJLrg5dSwELKQCEwYBhgL/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-17-20.png)


Em sintaxe escolha a que se encaixe as suas necessidade.
Utilizamos  String(Numeric) indicado para uma sequência de dígitos.
Como dica segue o link apresentando a melhor forma de utilizá-las 
https://technet.microsoft.com/en-us/library/cc961740.aspx



### Etapa 3) Adicionando o atributo a uma classe

O atributo criado pode ser adicionado a uma ou mais classes do Active Directory. Para fazer isso, proceda da seguinte maneira:
Na ferramenta administrativa do Schema do Active Directory, vá para  Classes, selecione a classe para atualizar, então vá para suas  propriedades:



[![img](https://2.bp.blogspot.com/-SrfHcfFRhXM/WfYQj-a3wkI/AAAAAAAAA8M/2eO414lkDZ0crRZe5UbWT1bTE3-W05M1gCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-28-56.png)](https://2.bp.blogspot.com/-SrfHcfFRhXM/WfYQj-a3wkI/AAAAAAAAA8M/2eO414lkDZ0crRZe5UbWT1bTE3-W05M1gCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-28-56.png)


Abra a guia Atributos, clique em Adicionar .... Uma vez feito, selecione o atributo desejado e clique em OK.

[![img](https://4.bp.blogspot.com/-l3SNhZRLrfI/WfcooCeCSjI/AAAAAAAABAY/D6cwEkkIMiQ_msDd20QuqyLwepxzQCbKwCLcBGAs/s1600/adicionandoAtribuoto.png)](https://4.bp.blogspot.com/-l3SNhZRLrfI/WfcooCeCSjI/AAAAAAAABAY/D6cwEkkIMiQ_msDd20QuqyLwepxzQCbKwCLcBGAs/s1600/adicionandoAtribuoto.png)





[![img](https://4.bp.blogspot.com/-9w2w0IQDKTk/WfYRFZBijMI/AAAAAAAAA8c/9JPRTfPU4sgdEuE9nP9tOB4gRJwDrxI7gCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-31-37.png)](https://4.bp.blogspot.com/-9w2w0IQDKTk/WfYRFZBijMI/AAAAAAAAA8c/9JPRTfPU4sgdEuE9nP9tOB4gRJwDrxI7gCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-31-37.png)



Acione ***windows + R\***

***[![img](https://4.bp.blogspot.com/-sfKrQgSf-eA/WfdbOUR3NyI/AAAAAAAABC8/FB7z1kBGqRMQ5QThYQ_I_LkPZ6cTZvVggCLcBGAs/s1600/windowsR.png)](https://4.bp.blogspot.com/-sfKrQgSf-eA/WfdbOUR3NyI/AAAAAAAABC8/FB7z1kBGqRMQ5QThYQ_I_LkPZ6cTZvVggCLcBGAs/s1600/windowsR.png)\***



Execute o **services.msc** e reinicie o serviço **Active Directory Domain Services**

[![img](https://4.bp.blogspot.com/-BgvQoO6QY7w/WfdbjKLCzBI/AAAAAAAABDA/3g2mUD6LyiMMmsTS43VrvMCfn8dGYO2TwCLcBGAs/s1600/servidowsAD.png)](https://4.bp.blogspot.com/-BgvQoO6QY7w/WfdbjKLCzBI/AAAAAAAABDA/3g2mUD6LyiMMmsTS43VrvMCfn8dGYO2TwCLcBGAs/s1600/servidowsAD.png)





#### Etapa 4 ) Utilizando os novos atributo

**4.1** ) Abra o Active Directory Users and Computers, selecione um usuário.
**4.2** ) Navege em View e habilite a Advanceds Features

[![img](https://2.bp.blogspot.com/-06CN_EI2hd0/WfYTjRpkIzI/AAAAAAAAA84/3iM_Xkv5W5EpAYrUeuceMQayBK9OoY7igCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-42-05.png)](https://2.bp.blogspot.com/-06CN_EI2hd0/WfYTjRpkIzI/AAAAAAAAA84/3iM_Xkv5W5EpAYrUeuceMQayBK9OoY7igCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-42-05.png)


**4.3** ) Agora escolha um usuário, clique com o botão direito e abra o opção Propriedades

[![img](https://2.bp.blogspot.com/-miYptMzJipA/WfYT0yOHI4I/AAAAAAAAA88/33xfkpVooS80Q53p5HE7EuIeVeV93jJfgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-42-48.png)](https://2.bp.blogspot.com/-miYptMzJipA/WfYT0yOHI4I/AAAAAAAAA88/33xfkpVooS80Q53p5HE7EuIeVeV93jJfgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-42-48.png)


**4.4** ) Selecione Atribute Editor e localize o Atributo CPF

[![img](https://2.bp.blogspot.com/-NQ080lvfhII/WfYUcqkV2II/AAAAAAAAA9I/N4gV2nq4G5wPPTwW1zZnILqUFV3tIWYzwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-45-37.png)](https://2.bp.blogspot.com/-NQ080lvfhII/WfYUcqkV2II/AAAAAAAAA9I/N4gV2nq4G5wPPTwW1zZnILqUFV3tIWYzwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-45-37.png)


**4.5** ) Dê um duplo clique sobre ele 
**4.6** ) Em Value insira o CPF do Usuário

[![img](https://1.bp.blogspot.com/-wN9ro8VEuUA/WfYU0I6VyEI/AAAAAAAAA9M/OMKaLhHIamoeFvg4_FrHxwnyuC3kYhKiACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-47-18.png)](https://1.bp.blogspot.com/-wN9ro8VEuUA/WfYU0I6VyEI/AAAAAAAAA9M/OMKaLhHIamoeFvg4_FrHxwnyuC3kYhKiACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-47-18.png)


Repita os processos **4.3** ao **4.6** em quantos usuário julgar necessário.



#### 5 ) Configurando o atributo CPF no Servidor LDAP do GLPI

Acesse o GLPI navegue até a opção Diretórios LDAP

**5.1** ) **Configurar** > **Altenticação** > **Diretórios LDAP**
**5.2** ) Clique em Adicionar



[![img](https://1.bp.blogspot.com/-RFy2nUWUpHk/WfYV8dZbFfI/AAAAAAAAA9Y/GRIKjHLyaFcbVhs-EHlg2N9Pk0VpDpTwgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-51-18.png)](https://1.bp.blogspot.com/-RFy2nUWUpHk/WfYV8dZbFfI/AAAAAAAAA9Y/GRIKjHLyaFcbVhs-EHlg2N9Pk0VpDpTwgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-51-18.png)


**5.3** ) Clique em Active Diretory

[![img](https://2.bp.blogspot.com/-NALUgiwgV4s/WfYWeSpRNgI/AAAAAAAAA9c/P6n6aLZmyLA_aaecYF5Pif4I9JhAr6ycgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-54-25.png)](https://2.bp.blogspot.com/-NALUgiwgV4s/WfYWeSpRNgI/AAAAAAAAA9c/P6n6aLZmyLA_aaecYF5Pif4I9JhAr6ycgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B15-54-25.png)


**5.4** ) Assim como na figura preencha todos os campos necessários altere o campo samaccountname para cpf Salve as configurações.
[![img](https://4.bp.blogspot.com/-WxUah_B_wmE/WfYW1aK60tI/AAAAAAAAA9o/452p8aN93KoQRWRDDJPtXSFlpCq2SR6pgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B14-12-28.png)](https://4.bp.blogspot.com/-WxUah_B_wmE/WfYW1aK60tI/AAAAAAAAA9o/452p8aN93KoQRWRDDJPtXSFlpCq2SR6pgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B14-12-28.png)**5.5** ) Clique em testar para conferir se as configurações estão corretas.



[![img](https://3.bp.blogspot.com/-wpz_IYvm1cQ/WfYW8qmtMPI/AAAAAAAAA9s/m2DoQXYGBoUd0Y-Nf3WgIZ4UMFT8omX0gCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B14-15-34.png)](https://3.bp.blogspot.com/-wpz_IYvm1cQ/WfYW8qmtMPI/AAAAAAAAA9s/m2DoQXYGBoUd0Y-Nf3WgIZ4UMFT8omX0gCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B14-15-34.png)



#### 6 ) Acessando o GLPI com CPF


**6.1** ) Faça logoff do GLPI e insira algum CPF cadastrado no nos atributos do usuario no active directory

**Exemplo de CPF : 559.554.867-54**
Senha Cadastrada no active diretory

[![img](https://2.bp.blogspot.com/-lUu2KthSBgM/WfYb6uB6TEI/AAAAAAAAA-A/Nfq_49Szilk0GiOi50rT4cnUKRYrMGsFgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B16-17-36.png)](https://2.bp.blogspot.com/-lUu2KthSBgM/WfYb6uB6TEI/AAAAAAAAA-A/Nfq_49Szilk0GiOi50rT4cnUKRYrMGsFgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B16-17-36.png)

 [![img](https://2.bp.blogspot.com/-qcNJWGbu3Og/WfYcAMX5P2I/AAAAAAAAA-E/qP7XM692UewY7E4LjZml3GJIAHJTVCw6ACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B16-17-51.png)](https://2.bp.blogspot.com/-qcNJWGbu3Og/WfYcAMX5P2I/AAAAAAAAA-E/qP7XM692UewY7E4LjZml3GJIAHJTVCw6ACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-29%2B16-17-51.png)

Também fizemos um vídeo bem rápido sobre o assunto se quiser dar uma  olhadinha, fique a vontade. são apenas 3:53 divita-se e até a próxima :)



<iframe allowfullscreen="" mozallowfullscreen="" src="https://www.dailymotion.com/embed/video/x66wod4" webkitallowfullscreen="" frameborder="0"></iframe>

*OBS: Não há necessidade de cadastrar o CPF com pontos e traços mas caso queira manter esse modelo não há problema algum*
Até a próxima ! 👦 