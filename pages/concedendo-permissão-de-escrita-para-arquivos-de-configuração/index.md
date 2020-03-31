Então você quer permitir a escrita de configuração na raiz do GLPI, mas o sistema acusa que não permissão ? Já deu chmod 777 e nada ?, pois bem antes de tudo o chmod 777 é extremamente errado nesses casos, pois não se trata de uma permissão de arquivos e sim permirir que o apache consiga gravar no diretório em questão, para conceder isso de forma segura, é usado o `AllowOverride`
Abaixo apresento os procedimentos.
Criando um arquivo de configuração.



```
echo -e "<Directory \"/var/www/html/glpi\">\n\tAllowOverride All\n</Directory>" > /etc/apache2/conf-available/glpi.conf
```

```
a2enconf glpi
```

```
systemctl restart apache2
```

