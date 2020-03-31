Para alterar a porta padrão do apache no centOS abra o arquivo httpd.conf, e altere a parâmetro Listen como

```
nano /etc/httpd/conf/httpd.conf
```

![photo_2018-01-29_15-31-11](https://3.bp.blogspot.com/-Y6pnqMnLAb0/WnA1Y01iBqI/AAAAAAAAB_Y/GnMRtLegZyctv0IZ4i8hsXDRC6oPBUS7QCKgBGAs/s1600/photo_2018-01-29_15-31-11.jpg)

Reinicie o apache

```
 systemctl  restart httpd
```

Confira se ele está disponível na nova porta configurada

```
sudo lsof -i | grep httpd
```

![Captura de tela de 2018-01-30 07-01-30](https://3.bp.blogspot.com/-RBK8qD3Wnxc/WnA1Y_xzdgI/AAAAAAAAB_Y/evUZBaeTlKIvsHo8wcVUwTnuh_dJC4XUgCKgBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-30%2B07-01-30.png)

Em seguida permita o acesso a nova porta no firewallD, por padrão é esse que vem no CentOS 7.

```
sudo firewall-cmd --zone=public --add-port=8084/tcp
```

Se voce tiver o SElinux ativado, desatíve-o ou faça as devidas configurações para permitir o acesso.

Confira se se está ativado com o comando

```
sestatus
```

Saída

```
SELinux status:                 enabled 
```

Altere o selinux enforcing para disable

![Captura de tela de 2018-01-30 06-53-22](https://2.bp.blogspot.com/-y48H0KW0qBI/WnA1Y1sDPVI/AAAAAAAAB_Y/Fu4maCR8seI1dppOJm1v2B4IWaoDeq3LgCKgBGAs/s1600/Captura%2Bde%2Btela%2Bde%2B2018-01-30%2B06-53-22.png)

Para desabilidat o selinux umediatamente execute o comando

```
setenforce 0
```

Fico por aqui e até uma próxima oportunidade 😃