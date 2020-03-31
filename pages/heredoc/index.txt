Na computação, Here document , heredoc ,EOF (*sintaxe*) … é uma condição do sistema operacional que define os limites de um texto, ele pode ser utilizado em várias situações, e nesse caso aliado com o cat pode servir como um pequeno editor de texto, mas essa possibilidade eu acabo utilizando em scripts ou para alguma configuração automática de arquivos, por exemplo configurar um arquivos do apache.

No exemplo abaixo configuramos o virtualhost do apache via **EOF** e **CAT**.

```
cat << EOF >  /etc/apache2/conf-available/erp.empresa.conf

<VirtualHost *:80>	
ServerAdmin admin@erp.empresa.conf
	ServerName erp.empresa.conf
	ServerAlias erp.empresa.conf
	DocumentRoot /var/www/html/erp
	ErrorLog /error.log
	CustomLog /access.log combined

</VirtualHost>

EOF
```

Você pode usar o **>>** para adicionar um texto ao final do arquivo.

```
cat << DELIMITADOR >> meu-documento.txt
...
adicionando um pouco mais o trecho do arquivo....

DELIMITADOR
```

Como pode perceber substituímos o **EOF** por **DELIMITADOR**, fizemos isso para demonstrar que EOF é apenas uma sintaxe comum, mas pode ser substituído por qualquer palavra.

Caso queira saber um pouco mais sobre essa função acesse **[LDP](http://tldp.org/LDP/abs/html/here-docs.html)**