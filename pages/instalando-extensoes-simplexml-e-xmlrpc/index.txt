
```
apt update
```

```
apt install -y  php-xml   php-xmlrpc
```



```
systemctl  restart apache2
```

Se você percebeu eu não cheguei a definir a versão exata do php para as extensões, isso porque estamos utilizando o php disponível na politica padrão do ubuntu, você pode conferir isso em

```
apt  policy php-xmlrpc
```



```
apt  policy php-xml
```