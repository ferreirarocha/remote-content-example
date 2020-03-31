Redefinir senhas faz parte de todo processo de gerenciamento de uma  base de usuários, os motivos para sua utilização são vários, desde uma  simples política de segurança, ou até mesmo devido a troca do gestor da  base de dados, o que implica invariavelmente na redefinição de senhas de todos ou alguns consumidores de serviço de TI da instituição.
Ora e  por que isso ? um dos motivos parte do princípio do antigo gestor não  ter aceito a dispensa de forma amigável e o resto da estoria podemos ver no vale a pena ver de novo 🙈.
Como mantenedor de uma base de dados GLPI, saiba como se precaver diante desta hipótese real.

## Acesse o seu SGDB

```sql
mysql  -u root -p
```

## Selecione a base de dados utilizada pelo GLPI

```sql
use glpi;
```

## Avaliação

```sql
select password from glpi_users;
```

## Alterando a senha de todos os usuários

##### Opção 1:

Se você desejar alterar a senha de todos os usuário inclusive o glpi, você poder executar o comando abaixo.

```sql
update glpi_users set  password= MD5('nova-senha');
```

Nesse caso alteramos a senha de todos os usuário inclusive o glpi.

##### Opção 2:

Caso queira excluir o usuário glpi da lista de restauração de senhas execute esta segunda opção, altere-a caso sinta necessidade.

```sql
update glpi_users set  password= MD5('123456') where not name='glpi';
```

Nesse caso alteramos a senha de todos os usuário exceto o glpi.
Agora acesse a interface web do helpdesk e entre com a nova senha.
Informe aos usuários a importância de realizar a alteração da senha no próximo  logon. caso contrário o desastre será o mesmo que tentamos evitar na  introdução desse post.

Fico por aqui e até a próxima 👦