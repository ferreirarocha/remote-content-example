Se você está tentando instalar o GLPI e recebeu esse tipo de aviso,  saiba que a resolução é simples, basta instalar  o php-peas-CAS  e em  seguinda  restartar o apache

```
apt install -y  php-cas
```



```
systemctl restart apache2
```