**T**rabalhando com entrega e gerenciamento de serviços, é comum a  necessidade de prover diferentes meios de autenticação em nosso  helpdesk, o GLPI há bastante tempo oferece nativamente a opção de  autenticação via e-mail, para que isso ocorra precisamos instalar a  dependência php-imap que provê a coleta de e-mail e a autenticação de  usuários.

 Instale-a para prosseguirmos com passso a passo abaixo.



### 1 - Logar no GLPI como usuário administrador



[![img](https://4.bp.blogspot.com/-J6uY4gc2R0c/WfG0yQGbm3I/AAAAAAAAA5Q/9J5WxoWixE8HXH_JCpaSMK1NwMpGwxzGwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B07-42-18.png)](https://4.bp.blogspot.com/-J6uY4gc2R0c/WfG0yQGbm3I/AAAAAAAAA5Q/9J5WxoWixE8HXH_JCpaSMK1NwMpGwxzGwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B07-42-18.png)





### 2 - Configurar servidores de email

Acesse a função  **Servidores de Email,** localizada em 
**Configurar** > **Autenticação** > **Servidores de Email**



[![img](https://1.bp.blogspot.com/-awrXU0HMzL0/WfG33rbGRcI/AAAAAAAAA6I/WQXyaw9PCbox3AxuD6xY9KRXaCQJj2ZuQCLcBGAs/s320/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B08-21-53.png)](https://1.bp.blogspot.com/-awrXU0HMzL0/WfG33rbGRcI/AAAAAAAAA6I/WQXyaw9PCbox3AxuD6xY9KRXaCQJj2ZuQCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B08-21-53.png)





### 3 - Clique em Adicionar servidores de email



[![img](https://2.bp.blogspot.com/-N_2eZyKQHdE/WfG07ewdtaI/AAAAAAAAA5U/bWItECVA2nUqlCiStGpHuVAGOcOwzxGOgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B07-44-06.png)](https://2.bp.blogspot.com/-N_2eZyKQHdE/WfG07ewdtaI/AAAAAAAAA5U/bWItECVA2nUqlCiStGpHuVAGOcOwzxGOgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B07-44-06.png)





### 4 - Configurar serviço

Configure o seu serviço de com os dados disponíveis em seu provedor 
 Em nosso exemplo utilizamos o serviço de email da Godady



[![img](https://4.bp.blogspot.com/-rkErVozrCYs/WfG1FGFkqMI/AAAAAAAAA5Y/PkOUrAKqtnEwpKz2Bn8VCx2isZzfWuMlwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B07-52-43.png)](https://4.bp.blogspot.com/-rkErVozrCYs/WfG1FGFkqMI/AAAAAAAAA5Y/PkOUrAKqtnEwpKz2Bn8VCx2isZzfWuMlwCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B07-52-43.png)



 Os campos abaixo são obrigatórios;

**Nome** , **Ativo** , **Nome do domínio do e-mail** , **Servidor**   , **Opções de conexão**.   . 

 Há casos onde ativar a opção TLS é necessária, porém em nosso caso não houve a necessidade.

 Veja como ficou nossa configuração.



| [![img](https://3.bp.blogspot.com/-q0ElZ_6OpbE/WfG1faEg71I/AAAAAAAAA5g/OjHE1YNYSXcS8G17BBw1YK0dRUpKqRHfACEwYBhgL/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B07-52-01.png)](https://3.bp.blogspot.com/-q0ElZ_6OpbE/WfG1faEg71I/AAAAAAAAA5g/OjHE1YNYSXcS8G17BBw1YK0dRUpKqRHfACEwYBhgL/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B07-52-01.png) |
| ------------------------------------------------------------ |
| Config de servidor de mail                                   |



### 5 - Clique em testar para checar o serviço de autenticação

Insira uma conta de email e senha válidos presentes no serviço de e-mail.



[![img](https://1.bp.blogspot.com/-xNGPGU6Yg7o/WfG16AcYJ_I/AAAAAAAAA5k/31ibMk0pD6sAUE46XqRUjsaXwWLF42_-gCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B07-57-11.png)](https://1.bp.blogspot.com/-xNGPGU6Yg7o/WfG16AcYJ_I/AAAAAAAAA5k/31ibMk0pD6sAUE46XqRUjsaXwWLF42_-gCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B07-57-11.png)


 Se tudo estive correto a mensagem de Testado com Sucesso surgirá, 





[![img](https://2.bp.blogspot.com/-PwoV2YDHm7w/WfG2o3ZWiLI/AAAAAAAAA5w/x1I8JrMTL9ECaiFaRnAgWvb8cR6iQL8gACLcBGAs/s200/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B08-15-49.png)](https://2.bp.blogspot.com/-PwoV2YDHm7w/WfG2o3ZWiLI/AAAAAAAAA5w/x1I8JrMTL9ECaiFaRnAgWvb8cR6iQL8gACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B08-15-49.png)

[![img](https://3.bp.blogspot.com/-C1uZt33lyS4/WfG2o4x2IFI/AAAAAAAAA50/_vBkDhy837kWhRLuRYTlZq-_ziAzCZhmACLcBGAs/s200/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B08-15-59.png)](https://3.bp.blogspot.com/-C1uZt33lyS4/WfG2o4x2IFI/AAAAAAAAA50/_vBkDhy837kWhRLuRYTlZq-_ziAzCZhmACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B08-15-59.png)


 Caso haja alguma incoformidade a mensagem 

 "Erro Teste Falhou" será exibida

 Então analise e teste atentamente  todas as opções de configuração no servidor de e-mail.





### 6 - Faça logoff do GLPI e entre com uma conta de e-mail válida



| [![img](https://3.bp.blogspot.com/-EZEYa0r-nsc/WfG25EoTtJI/AAAAAAAAA54/lPhZxYAk4f8AGcVp3NbTXEw164HIGmgIACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B08-04-39.png)](https://3.bp.blogspot.com/-EZEYa0r-nsc/WfG25EoTtJI/AAAAAAAAA54/lPhZxYAk4f8AGcVp3NbTXEw164HIGmgIACLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B08-04-39.png) |
| ------------------------------------------------------------ |
| Logon Email                                                  |





| [![img](https://3.bp.blogspot.com/-QOQxeRkr2qs/WfG3Ee_GaqI/AAAAAAAAA58/ZRvKNEEFiJYq_Y-78NfyrLMZxIgQp-fwgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B08-05-27.png)](https://3.bp.blogspot.com/-QOQxeRkr2qs/WfG3Ee_GaqI/AAAAAAAAA58/ZRvKNEEFiJYq_Y-78NfyrLMZxIgQp-fwgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2017-10-26%2B08-05-27.png) |
| ------------------------------------------------------------ |
| Acesso por email                                             |



### Preparamos um vídeo com o passo a passo descrito nessse post. 



<iframe allowfullscreen="" mozallowfullscreen="" src="https://www.dailymotion.com/embed/video/x667lef" webkitallowfullscreen="" frameborder="0"></iframe>