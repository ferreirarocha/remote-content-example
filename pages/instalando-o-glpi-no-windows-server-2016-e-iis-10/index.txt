Neste post demonstrarei como instalar o GLPI no Windows Server 2016,   executando o Servidor Web  IIS 10, será instalado o PHP ManagerForIIS,   MYSQL e por fim ativaremos as extensões PHP  LDAP, Fileinfo e Opcache  necessárias para execução do GLPI.

**Um momento de reflexão**

![Batman Corrigindo o carinha da TI](https://lh3.googleusercontent.com/tGzBC1BefUhENzY4WbIrev8o55VtiCbkJ2MmDTa1_8x7r17KdaCcvO10g56FkR04F5GCnX5xjQb1nPSzH8u3_M1B7qBtVVXpigGLhIyAeK7tjf_Db4g7HSbax7x9nqPz3vjIXrdnLw=s600-no)



## 1-Baixando Dedendências

Aqui estão disponíveis os links das dependências necessárias para provisionarmos o GLPI.
[**Web  Plataform Installer  (WebPI)**](https://www.microsoft.com/web/downloads/platform.aspx)
**[PHPManagerForIIS](https://github.com/RonaldCarter/PHPManager/blob/master/bin/Release/PHPManagerForIIS.msi)**
**[GLPI](https://github.com/glpi-project/glpi/archive/9.2.1.zip)**
[**Dashboard GLPI (Opcional)**](https://forge.glpi-project.org/attachments/download/2216/GLPI-dashboard_plugin-0.9.0_GLPI-9.2.x.tar.gz)



## 2-Instalando  NET Framework 3.5

##### Instalar NET Framework via DISM

Particularmente prefiro fazer a instalação via DISM no prompt de comando, por ser mais rápido e objetivo.

```php
DISM /Online /Enable-Feature /FeatureName:NetFx3 /All
```



## 3-Instalando  o   Web PI, PHP e MySQL

Baixar **[WebPI](https://www.microsoft.com/web/downloads/platform.aspx)**
 Execute o **W**eb **P**lataform **I**nstaller  (**W**eb**PI**), e  na tela de administração escolha o  Mysql Windows 5.1 e  PHP 7.

##### **Instalando o Mysql Windows 5.1**

Marque o **MySQL Windows 5.1** para a instalação disponível em
*Web Plataform Installer > Products > Database > MySQL Windows 5.1*

##### **Instalando o PHP 7**

Marque o **PHP7** para a instalação disponível em bnb  hjmku09-0
*Web Plataform Installer > Products > Frameworks > PHP 7.0.21 (x64)*
​   
Desmarque a  opção php manager for iis, isso é necessário pois a versão disponível no WPI não é compatível com o IIS 10

- Clique em Install
  
- Cadastre uma senha para o user root no banco de dados
  
- Aceite Termo de Licença
  
- Aguarde finalizaçao do processo de instalação.
   ​
   

##### Instalar o PHPManagerForIIS.msi

Baixe e instale o PHPManagerForIIS.msi disponível no Github.
 Baixar **[PHPManagerForIIS](https://github.com/RonaldCarter/PHPManager/blob/master/bin/Release/PHPManagerForIIS.msi)**



## 4-Configurando o Path do MySQL

A instalação padrão   do [MySQL](https://draft.blogger.com/blogger.g?blogID=3985185191198013161) no Windows não cria o path do programa no  registro do sistema com isso, se você quiser  gerenciar o [MySQL](https://draft.blogger.com/blogger.g?blogID=3985185191198013161) via CMD  é necessário  inserir o caminho completo sempre que for  executar alguma tarefa, navegar ao local de instalação do programa, ou  abrir o arquivo pré configurado  para o CMD.
Mas isso pode ser resolvido  de forma  simples  inserindo o path
Abra o CMD  com usuario administrativo e insira esse comando.

```
setx /M MySQL "C:\Program Files\MySQL\MySQL Server 5.1\bin"
```



## 5-Criando Base de Dados no MySQL

Naturalmente  é  possível usar o usuário root para se conectar ao banco  de dados durante  a instalação do GLPI, porém isso não é considerado uma boa prática de segurança, sendo indicado criar um usuário específico  para a nova base de dados, abaixo apresentamos um meio simples de  criar um usuário e conceder  permissão a base de dados.
 Criando Banco de Dados glpi

```sql
mysql -u root -p"123456" -e "create database glpi character set utf8";
```

Criando Usuário user_glpi

```sql
mysql -u root -p"123456" -e "create user 'user_glpi'@'localhost' identified by '123456'";
```

Dando permissão adminsitrativa ao usuário user_glpi ao banco de dados glpi

```sql
mysql -u root -p"123456" -e "grant all on glpi.* to 'user_glpi'@'localhost'  with grant option"; 
```

Troque a senha 12345 pelo password criado durante a instalação do MySQL no tópico 3.



## 6-Baixando e pré configurando o GLPI

Baixe o [GLPI  9.2](https://github.com/glpi-project/glpi/archive/9.2.1.zip)
 Extraia o conteúdo e envie para o diretório

```
c:/inetpub/wwwroot/
```

Conceda permissões de modificação ao usuário IUSR  na pasta   C:\inetpub\wwwroot\glpi , é comum utilizar  a interface gráfica, mas   aqui vou apresentar um modo mais objetivo utilizando o ***icacls***
 Abra o cmd como administrator e execute o seguinte comando.

```
icacls "C:\inetpub\wwwroot\glpi"  /c /t  /grant IUSR:M
```

Nota:
 O   **:M** no final do IUSR indica que  ele terá permissão de moditicar o diretório e subdiretório glpi
 O modo `/c /t`  aplica a permissão nas subpastas.



## 7-Criando um  novo site para o GLPI no IIS

Obviamente essa etapa pode ser  realizada via interface gráfica do IIS, porém abordaremos sua gestão via  **appcmd**
 Abra o  cmd como administrador e insira o seguinte comando;

```
%windir%\system32\inetsrv/appcmd.exe add site /name:"Suporte GLPI" /physicalPath:C:\inetpub\wwwroot\glpi /bindings:http/*:80:suporte.empresa.int
```

Ele  criará um instância chamada Suporte GLPI com caminho físico  ***C:\inetpub\wwwroot\glpi***  escutando a ***porta 80*** no seguinte domínio  ***suporte.empresa.int***.

```
echo 127.0.0.1 suporte.empresa.int >> %windir%\System32\drivers\etc\hosts
```

Este comando  cria uma entrada **suporte.empresa.int**  no arquivo de hosts.



## 8-Ativando extensões via PHP ManagerForIIS

#### **Ativando e desativando exestensões PHP**

Execute **Windows + R** e insira o seguinte comando abaixo para  abrir o IIS ;

```
"C:\Windows\System32\inetsrv\iis.msc"
```

1. No painel Conexões, clique com o botão esquerdo do mouse no pool de  Sites
   
2. No lado equerdo do painel selecione o  PHP Manager
   

##### Em PHP Settings

Altere os valores em Set runtime limits, em nosso laboratório deixamos o ambiente coma seguinte configuração.

| Data Handling            |          |
| ------------------------ | -------- |
| Maximum POST size        | **200M** |
| Upload Maximum File Size | **100**  |
| **Resources Limits**     |          |
| Maximum Execution Time   | **300**  |
| Maximum Files To Upload  | **50**   |
| Maximum Imput Time       | **120**  |
| Memory Limit             | **512M** |

##### Ativando e desativando extensões

![PHP Extensions](https://lh3.googleusercontent.com/thamMwFm_xuP-eccuRKEvqpWLhiNswqYa8USgYzQ0gxlN-zcFhjTqmbVIp08kej5ns0JngfFXY1csOlS1_2WtHMnxMaaUrTGOwSetqR-a1ayRXV_LK6m6qKxeakwqaHmJOCSYS5heg=w649-h276-no)
 Acesse Enable or disable an extensions
Ative os seguintes extensões clicando sobre elas com o botão direito e marcando enable.

1. php-ldap
   
2. php-fileinfo
   

![Extensoes PHP](https://lh3.googleusercontent.com/XsBpTTEYuUgdOlUKxGUUgJscnCjRg6LERHDrPtFmLfV9QNlovGHiyKIx-TPHv-Ru6kD7eWGZHnyyyOwmLwIXVdce0SaLeJ2FkhTcMHuKlWshAElC4clPQB1YKJc6FXnhkf1NOGtBeg=w765-h342-no)

##### Editando o PHP.ini

Edite o php.ini para que o  instalador do GLPI reconheça a extensão Opcache.
 Cique com o botão direito e acione o Open php.ini
 No final do arquivo insira o seguinte conteúdo.

```ini
[opcache]
zend_extension=php_opcache.dll
; Determines if Zend OPCache is enabled
opcache.enable=On
opcache.cli_enable=On
```

Salve e feche o arrquivo php.ini



## 9-Instalando GLPI

Finalmente o ambiente Windows IIS está preparado para  executar o GLPI,  podemos agora  acessar a sua url local e darmos início a  instalação;
 Acesse
**http://localhost/glpi**   ou   **[http://suporte.empresa.int](http://suporte.empresa.int/)**
![01-selecionar-idioma](https://lh3.googleusercontent.com/sp_q-f8zLJz1kfj_NGxXJGqz946tLJh2n9hfXILFrFrXjxh0L4MWNwaOJAGYSBDc6yDAQzCMKz4f48Gmmp6mAaVvmfAktWGUGbgpR06q0wxd2DQgwQu6_AYzYeMjyPh5HXutxkLX0LsZfPtIo6Ky7ns0jF_ECfCf1SHxNM9oGl4UV9SdMX9A9NH2ZGVxujL0rIcDf8Yqonb0n6uQoW-pTt9OYXSrE-7kwsJdejMY3RtF2xVmr3j0xkpcg1g88fePODxtOiDJI0XmKNB_v80eioCMhtK0z4E-8VmLj586wcGumPbcDRJHaQ4nj3Ykt3ETqtF3OthVgw5GcuIdJo1SeEnEow_qMcHxa_JLhEa22kv500ORkZeqRlmmfRy4TYfrRMNq2t-acDyhSMumaBOdErUg66tR0us0H7JqNyoWkDWH-LjIZGCPubdyguqqh5OO0oCNlBghdh5e4rhgNOV60K7BwERfvi2VhwesMs1ftS5sJ9hmt0qYYo3ZpXqcgncJ-CmUO_Q5008Rh0ptRKw0x6Ff7S7fyHEmPwIO6awMb14s6iUmwJh0P54OeobDEY3Zfj1wm4CtEsYEZLvkVXNkQmQf-sUdGfImiJYOnTY=w765-h342-no)
![02-aceitar-licena](https://lh3.googleusercontent.com/RB_CjJ67iKRSF5PtvtcXQxx9W_6C_k0STOb-ALAIvTS-GM7jnxdpjd-cxzLIasYR4PijJzpLWjkaMuL_exYrc1VTGGziz8lcua8mn6M7tQLcX4LDTygz3zuGtg2ba70YbnAUDqXqCbn49IJUFWpWkR-PoOqZP-_dcrPmAF7kuCwyosmj2Qa1LDwdOj4G7xhJhhT1WiiY1oKQj-xFYVH3HYhfUOAqK8e3IvSC8sIfI5lsseWxFr7MlPyYD-gHaTFlu_LusogX8Z1zk08G6NtdcsqhxJVXbbwKvauketdMJvR0Wy80Tyd9Fz6WqsJ1qw99M4byrkiO5_PhyQv8bfO2DUFZJgKClhTAY_n_D9LJ8wdbg372F9llw9lqGc_D0x_u-taVNAdU0oWHR3FgFGcvl-EcffbTH_hpyDrZoaeq6Np9GAHCr2Cecxu1T2EE9GhknhfOlEsaB4Tr8cdoCqVGrmbleYIht1OlfFtWaazsP7dtLqqhyX_L-7eNGviyZdemGyDWa-fYuT1QZy8McYy8Jn_3jBTFGm6QSlSdrjh6fhhZwcC5vYo0sSFrkyxxnzVyrqYKWtEhnNJvYXvJO4Ma4SYNLlJ_V_RKris2xRg=w765-h342-no)
![03-selecionar-tarefa](https://lh3.googleusercontent.com/KLoJALV-8HhsnVNRUgptY_p02-pg2eYcFopnJ8E7xCmNn2qHH7nj-7abrIv0anIhMo3WDNjpGmO561F2gWdKnbo6aEczjGGl4iqhyrP6tsOXszVDik8cz0HY17ylejA-d8OUBJF4RfguK4Qpw1QCOLFt0R5GuoASEdviVbNler8GOif-flHZ1_zu_X19gQbroep_OaWnn5vka0Uy3-A7fHLYmEweBEoaZ3si0cIrZteXbYTpxlBk5nmrW_frH-1qn-Zrne5GPSyinslyVqU5T_KljQWDSmvfd9urBkou8Immbnb5r6Z5cC3UT_WzKHEKfy1EbFT6SLSHisk1c0_kNX_U14S_D1vxjoljWpiYnWet8YeEmoipBiw9QHv_qhC4q3RdHe0kmT7lGWSYGiqDgPON0OShSZm3IRUbR8LzVoE8DVA2AwTJe-aE9dU7dMyEggDpLoXpxYejovgvLBCeM2M1jF69T7OSLZA_jIB_iXKki3w0IZDYOUgv4E20JRfha9aXrEz7xU6ZApBg9-PXRm89rZZHeDFQj16vO5m4Cy2guSYZkmmrcnnzGiPu5KnjbnLKhIWkm3PuUmsuqrmFNsCcyef4H-Xl7olgknA=w765-h342-no)



**Aviso :** Como deve  ter notado o instalador identificou a falta da extensão  APCU, isso ocorre por termos optado pelo Wincache ele não está preparado para  reconhecê-lo  como alternativa ao apcu, dessa forma prossiga com a instalação normalmente.

---

![03-5-Dependencias](https://lh3.googleusercontent.com/t35lRVssPP2U_G-ARwGrzR1lnb5PYV_juMQ8U2jh4cFvdCE_VoYuXKBpyHZM-codwR9WzyWAVGqdTnFEiX6TeuEeWLrl3G7QtsIsTAfX8qsH50EcT6I8OIqCirG8mlgHCMbzXgfzirrnoyaQ7un9FgJWg1Kuj4z22lptJ99zhiuoAubIJ6VyQNwGfpBPHkcJXf5W_2kmxDLvipkg4oh0srllLdLzbg3d2pQnJP2U_EjrDB0du1iE9i9RUcl_YfpyGLv1rG7TBzTHv4z4wwcGvsAEWSZeTTvW4e5xIlMIlka_rMNAb9PhqisjisFijSsjmbbQKayzJwC1DlFMGZvdISU6pkYF1NQ7UezpZRHCWBT94Rnp212x598iiVqXhP8LODIlePsDNULT36rdzuhRvwXCcvopFkLZD8xfxo2Ps1R2rG-yUb-KuqqBrGRLb04QZzX2egODw2XcvnLrwgKiBHN_YOxUnkLF4lX90JW8r2R7Gjhfwx8iIaf_Q6I670DGdXe_0tH6smWhty64DXDV7fI_h0IvXx5rKLNeQhY4l6hha9fWwZGv27T9PeNkmJtr5-N_zT9hQIm2kL_WW7ZQneynSWrSSQLkusGarU4=w765-h813-no)
![04-config-database](https://lh3.googleusercontent.com/4jKDzbwNfhM-TMd5ei-THETh6r1lK80yETS0Tm2ShRJ9ZM--4318IVhg5XOLZId1MUrfqxn4Lt5sjfo_-mDnmKhKjq3XsaLGblqQZqg0BGOqtI_vm3O2rKhFNuGPRvyTBcGAPAxrtpil5KbJZJZCb8crm_1m05xxlKh9QJU5VJvWYBZdXaiKpJK4yMyA-q2QkmJdiAIaiKCadgZyicDn2UcMOfBbpzxvektIDu98b7qV4jDW_WoWe1JmBXXYFw5jRXfURxM7Hyq1pfeAGTvl51xqIzVs90lRC37IvPB-ngvw5SApbbFBc6C49A_ld6NEF2FTKys0c4-5EFg0t26QRTIqsBpKOm1fPb3N7b2Ko1ZJJlcLxjzVLxYxbKuYJHrnjJzzDlmSRcuRrVqxlNggctjbzYOY78GfGlyciHiZq61yYJl6fgfEdzM5uY7kCI0cct1wIPuHrXUcHTDJtS99TrWox2J8qp4ywYJznqX0m1ZUBp9ncRvMeh790bgDXlr2d66PfAjUpG4jXZgRh1K3h-KQwKrk9g8f6WFWyvtDiJqNogbQcJPsQNlCCuJU0zq2VwHIz5eZ-vHMELA1-9YMXLiGcvPGuJMXlN_nDPE=w765-h342-no)
![05-selecione-base-de-dados](https://lh3.googleusercontent.com/lVIxMMtMWcuq3Ar5nd_6LwPjj049zD89guYpMQBs93bmAH1h6gkDk5OC2yt5IDgFjsaHhrEMGWJv3jmM2bvzpq-t-N64OAeGZ8rm6UKzBXapaPY7EKRRWE3YdaNhmHD-xb1vNdEv-h44ykHllHXqCticLaoYTC_szp8ov_RDPgCetb0GrrbPSI5bvn6FB5CouKMm5wJFk2bOdBCk0YhA0u3tHJYDPqwDR36vC24DPgYo6-3oBCZZEWZKBdc6rC0OSA33SmGV3mv6Z0ZvNOjOxIgsTlKaKkR7Nqb0gGjeU7oE2QCrBGfzxVima8ZbILhynwH7WhfUL63YTPAoHUMhGzs9oi-TBB3r-yOOwFCt8KASB1M6e3i6mR-3HIFZzq77JXpfx9R4H5ATMA76CYjoy4XxvDk2d4eEWQMmiwaLHW_LbVcSLVEg2XFQtKuI-mUHCbxnEYfovhw8b9b_A_KbKWsY_5Jd4lQCU0lk0p3dNxMpje023rqSm4CfsN-QzMCwZG9QDXoIpZLZZ3o5U1omQLHew3BiqSj3P6CWqd2ZipM0zrHYBE_vm9wBXnMFyNzIY-fhR8sQoS0bC5UZBHkyp5fvTNJ32HgYtbJu-Ec=w765-h342-no)


--



![06-DumpConcluido](https://lh3.googleusercontent.com/gGQ9TiZ-x1PzbC9kgWKgmX_kk4P3fYVDNSHC1IQH9jpnLvvI4c8R3N3MjECdTu4xg3m06XktjXwAqBUbuEOyyDyCnlyNfLxsrEyW-6fW863jQnU-mP-QLQaSraDOBV89Vm9w9k2CZLaWQw-ThB9vOXUmzDrr-IIxW3ltES-EOACFIUKLDVjGgI3Iyw_ca8YncgBuxz5A6DH9oXTH5NlfmdaMAPSnXefGOyQorLH85RpLNFcVXKFE3ZzG4COr9QXHhGBKtUKpSS-MjiJbMrtiZ6q57TFbZbjkdi193u1iHAYA1FSlKEDihUIESUNBsbQrmfKz8eWg4gX_TbAYVBZBdo3NwruNggFcedBEyjr4-D6ywr-3cPm8WEuSIJvXfWe-e_9juW1CeDhC2gWo7j1VKzetSboXccq0qX7hi6c1seYfCcE4Bgq4ktZGIQMQbCxnc1xCI9Fvyb5QxMna-fJ_QVm5KKaTz5jg9HVEygNAiICNJ1GvKUQF9RaE8N0mXuoAc4QwIibBbW7RaRKM-G_h3CloePdjjxA0QtxJhaEE10inTqZK5VHXEhBW6i9WT_HNhs53MJS6tyatuoMZAgIOiXWRV564KjhI8JpOB2c=w765-h342-no)

![07-coleta-de-dados](https://lh3.googleusercontent.com/L2a4q9FkA_kY0L0rXHLBP0ePEQ9YDe4HFER5dyKUhEIuc-z5qMRvGEJc272lPGl3GleGXIM-anoRcKXtzY1B5NGmBDNajn3EVp4UDIVtW8ZUiZ7CNoCx73j7mzOB_G0PHSAJFDz1Mmj8-PPaA-wrCHklgLyhjMLeHuAWiuN7cE8N1-NP6YwH7QxPYMUD-XTYGJEHCS4ig7s0VQpjjQRSnZQemKzTQK-2fTHlIHx4PiadlUUjtcRJdwUWOh70pP-rGL5gQnKU3h40oh4Qr9tbd3D8TlQxVWMBcQYcas18Pfo629HsO7AiVqF1OtLVJr2DfNWR1rR7iGV5BVtmEn35WJomHZmygBCvjEB6yT2xMfG4BDI0HRIm4h5kkkniyGw1JF4iZZ_dz28IPTDldVOlSa0ytpUVKoiphmmEcD0MJF3rod1jzeIeSKl_P9ca1WTysR2KZ6fbOIhUTzwkfkHaMbFUJnAp-NkVSOrRtM-Kl6NNXe7ylWUX3F3qytumNb7JvpLrKYTT7HX-KfqjtODw2A35s03Ml3kXV5DfcahjjbUCz6xWZXcaCF2X917BEP2kVLEWAMpstCku9S7HV4t9r-ddQXFJHu-ysqE4piE=w765-h342-no) 








![08-Instalacao-Concluida](https://lh3.googleusercontent.com/mWLvi0Z4KdYTW9P_pSsyKJ3UfKUbl41_dA1i7arfTaUZdYK1tYL6hRIaK7C2jd9ZesCWrPgAn0J0PwOEzo-W9CeSxO3eyvE53lAZ7lrUyK-bztgIZqmA725jUmQJUrme6MEiMvH0kNRFEAVekL_9Qt5-wMBOT9-1LtxRTDwcSt9-yixz6mtuuqV6-kGfN83NKhgs2OtjhpN4BeQzEBeoHPngoT0AsLL4OKwXL3X1xNEUQa13XKP6G7iFSmjMcy1r_Py6RgrJZQdnOCmazTP-x-nbkhGIsv_NXLi21lnid2eiy8tGdpW4aCQLewav7XyfeytuTrgWETju0CxLSokRa-r7DCLGE_jUcu6HtR5VWVtCkUXmTf31CaIf_aKB4JtGb9cCgFfxO-wubEqsqbSsjAgmvU4N3-JijoTuOv3cfSAkyUAPTFA1GKAh-If3b4gXwcG7k15xI0HZPEwzgUlhFnWvNbGuj5AVjgmdlAaFWmTCTGVGKZslcdG96JLu-Yz6RHbtOTXCXcx-8mZ0qBFKwreVW1e9CAvNtMwWNo9nkdFBBtX6SnKHd1ufoT90XRkYdq5KCC_h-j3wTmpndFk8rjrVG-sPraTp5X3SMXg=w765-h342-no)

--


![09-Login-GLPI](https://lh3.googleusercontent.com/yvCsyQ56NPvwy5UIr3YrOOzl59JzxOJ7P7o9DTDo0IPr5lKXR5Za73lHUnl7M_fvXGfK_nLoud6u8joJWsxEagCk0m1JlSfOvLkgH4USNM9RxafnhDmc3gtjwUwrGHqsBvzzl6RqLu9lsZSrV452wUJLrdW76zWGmSzSxnWEeaziG2JGjm__xWFq77FY3lvSNGOIU6Hmh5fP3x7zY8GEdx5U__56xTZA4rfK_t5jDZxS7MaCujTDF-W3FY1GIqa7TulXyw4F3VJgD5jhqQGwBXHBJuVl-O_vgeoHu_pCVxcso0I1mlYto8wRaHXaQrcIa-OKCUlAAt3R4CmmDXNxBN-VKhmg3EwN6asAHe5yXXZFzK56hMLAlzooTvCnnt-Or4Zwvn9Z9xSGurRKdbbME-E-4b0eC0zawzh3MJrKB9hHHHbLGkFQ5MOBUmbyIk90A9Re8upvu1Syk4LBs5ErJGzkKJp9lAO74mqSpkFVfuGbcTlYXAgtR2s_So4j1_m-6PPX2gLEAfCff8kkTdST4CVe7eo92XsopCHWHYwNFvWqZjejmLS8EpYg7RhQqcZbWErG0snedipYHLbFXLPHBhojJcZmppfxBX8GCr4=w765-h342-no)


--


Concluímos o processo de instalação do GLPI no Windows Server 2016



## 10-Dicas Adicionais

##### **Ferramentas para gestão da base de dados**

Talvez você prefira usar um cliente gráfico para o administrar o mysql,  são muitas opções disponíveis com esse propósito entre elas o [phpMyAdmin](https://www.phpmyadmin.net/), [HeidiSQL](https://www.heidisql.com/) e o [MySQL Workbench](https://www.mysql.com/products/workbench/)  mantido pela própria mantenedora do MySQL, as três ferramentas citadas  possuiem  vários recursos que o ajudarão a aprimorar a manutenção e  desenvolvimento dos seus projetos envolvendo o MySQL.

##### Cadastrar o FQDN do  GLPI no servidor  DNS da Empresa.

Esta etapa é opcional porém de suma  importância se você deseja faciliar o acesso dos usuários à ferramenta de suporte da empresa. Cadastre o   [**FQDN**](https://pt.wikipedia.org/wiki/FQDN) do  GLPI no DNS da empresa e evite publicar o serviço por endereço  ip/glpi, isso facilita a vida do usuário criando um nome  familiar como  suporte.empresa crm.empresa, assim como para o administrador de rede que pode alterar o servidor  de forma transparente, trabalhar com um único  endereço tanto interno quanto externo, entre outros benefícios.

##### Uma descontração

Não leve a  tirinha do Batman a sério, ela foi utilizada para  criamos  um gancho e demonstrar a utilização do GLPI sob IIS e Windows Server. 😃

## 11-CONCLUSÃO

Instalar  o GLPI no Windows Server 2016 com IIS é uma tarefa simples,  assim como no  Linux, devendo ficar atento as suas depedências de  software e permissões de pastas, no windows podemos susbstituir a  extensão APCU que  ajusta o cache php e otimiza a aplicação.
Já a  performance não é tão eficiente quanto em ambientes *unix, apresentando  uma certa lentidão em tarefas simples como listar os chamados, o que  pode ser um problema quando o ambiente elevar a carga de operação, se   isto não for algo crucial em seu helpdesk, então nada impedirá sua  utilização nesses cenários.
Em última análise, instalar o GLPI nesse  ambiente é interessante enquanto a infraestrutura, natureza do negócio  ou até mesmo a política da empresa não  permita ambientes heterogêneos, o que é uma raridade levando em considaração o cenário tecnológico atual.