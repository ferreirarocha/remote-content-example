Criar email de destinatário no GLPI é uma tarefa muito simples, porém  devemos nos atentar as configurações de nosso servidor de email, em  nosso ambiente estamos utilizando o GMail, possuindo as seguinte  configuração.

## Configuração minímas para o configurar Gmail

| Configirações do GMail | Valores                                                      |
| ---------------------- | ------------------------------------------------------------ |
| Servidor               | [imap.googlemail.com](http://imap.googlemail.com/)           |
| Opções de conexão      | IMAP SSL                                                     |
| Porta (opcional)       | [imap.googlemail.com](http://imap.googlemail.com/):993/imap/ssl |
| Cadeia de conexão      | 993                                                          |

Para efeito de treinamento faça uma conta no GMail assim obteremos o  máximo de compatibilidade com o tutorial. Ao finalizar o essa etapa faça uma analise no seu servidor de email e substitua os valores.



## Configurando o Destinatário



**Configurar** >> **Destinatários** >> **Clique em adicionar**

| Configirações do Destinatário com GMail                 | Valores                                                      |
| ------------------------------------------------------- | ------------------------------------------------------------ |
| Ativo                                                   | Sim                                                          |
| Servidor                                                | [imap.googlemail.com](http://imap.googlemail.com/)           |
| Opções de conexão                                       | IMAP SSL                                                     |
| Pasta de e-mails recebidos (opcional, geralmente INBOX) |                                                              |
| Porta (opcional)                                        | [imap.googlemail.com](http://imap.googlemail.com/):993/imap/ssl |
| Cadeia de conexão                                       | 993                                                          |
| Usuário                                                 | [seu-email@gmail.com](mailto:seu-email@gmail.com)            |
| Senha                                                   | sua-senha                                                    |



Sua configuração deverá ficar próxima a representada abaixo.

![Captura%20de%20tela%20de%202018-01-23%2022-24-32](https://4.bp.blogspot.com/-JE0buJ01o34/WmvXyRRkzvI/AAAAAAAAB4Y/eV0H2xQLhVwHrYRtXG42l8Wx7PnC0dPCgCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-23%2B22-24-32.png)



Após configurar o email acesse a aba Ações e clique em **Obter email agora** se tudo estiver correto aparecerá uma informação no rodapé da página informando quantos e-email ou não tem para restaurar.



## Cadastrando email de para abertura de chamados

Acesse o gerenciamento de usuários em
**Administração** > **Usuários**
 Escolha um usuário e faça um cadastro de email valido na guia **Emails** ,
![Captura de tela de 2018-01-23 22-36-03](https://2.bp.blogspot.com/-TOT65RHKSeM/WmvXyevEDBI/AAAAAAAAB4c/ZpHiOsrpV9UK0kYvXIRAsSk_0i9oVf41wCLcBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-23%2B22-36-03.png)

------



Agora este usuário conseguirá abrir um chamado por email enviando para o destinatário cadastrados.
Fico por aqui e até o próximo 😃