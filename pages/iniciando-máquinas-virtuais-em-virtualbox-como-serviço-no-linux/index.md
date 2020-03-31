Para iniciar máquinas virtuais como serviço no Linux e usando o virtualbox, devemos criar um arquivo de serviço dentro do diretório /etc/systemd/system/ ,
Assim o systemd poderá controlar funções e recursos do próprio virtualbox, nesse arquivo usaremos o headless um modo de operação onde as maquina virtuais não tem dependências com o servidor X do sistema hospedeiro contudo fornecendo dados via VRDP. O serviço também será capaz de salvar o estado da vm quando o sistema hospedeiro for desligado.





**Criando o serviço:**

```
 sudo nano /etc/systemd/system/vboxvmservice@.service  
```





```
 [Unit]  
 Description=VBox Virtual Machine %i Service  
 Requires=systemd-modules-load.service  
 After=systemd-modules-load.service  
 [Service]  
 User=USUARIO  
 Group=vboxusers  
 ExecStart=/usr/bin/VBoxHeadless -s %i  
 ExecStop=/usr/bin/VBoxManage controlvm %i savestate  
 [Install]  
 WantedBy=multi-user.target  
```

No parâmetro **User,** costumo cadastrar o usuário root.

**Selecionando maquina virtual**


Liste suas máquinas virtuais com o seguinte comando:



```
 vboxmanage list vms  
```



```
 "Endian-Firewall-Lab" {b2b5a912-4b67-4315-a753-8e3123f93e0a}  
 "Ubuntu-Server" {86887c5f-7f76-4e16-a41d-1e3472924125}  
 "Lab1" {d4fb046a-8bb2-45b3-8698-60f9c1db4a7d}  
 "Lab2" {03789bd5-c3ea-448b-b6c5-80beff6fcddb}  
 "OpenSuse" {01311e7a-242a-4216-bf16-082124b35458}  
 "Webserver" {01758fc5-cf8d-4582-af14-3ce9216f38f9}  
```



Agora podemos utilizar o nome da VM no systemctl nas seguintes tarefas :



**Ativar a máquina virtual no boot:**

```
 sudo systemctl enable vboxvmservice@Ubuntu-Server  
```





**Iniciar a máquina virtual através do systemctl**





```
 sudo systemctl start vboxvmservice@Ubuntu-Server  
```





**Conferir status da VM**



```
 systemctl status  vboxvmservice@Ubuntu-Server.service   
```





**Desativar a VM no boot**



```
 systemctl disable  vboxvmservice@Ubuntu-Server  
```





**Parar a VM**





```
 systemctl stop  vboxvmservice@Ubuntu-Server  
```


**| Atenção:**



1. Substitua Ubuntu-Server pelo nome de sua máquina virtual escolhida.
2. Confira se o usuário configurado no arquivo de serviço possua acesso administrativo às máquinas virtuais, do contrário o serviço pode não funcionar adequadamente.



**| Ambientes indicados:**



VM's como webservers
VM's como firewalls
VM's labs



Tenha consciência de que a virtual machine apesar de estar em background ainda assim consumirá um bom recurso do seu pc. Então configure com atenção sua máquina virtual e também a quantidade de vm's durante o boot.