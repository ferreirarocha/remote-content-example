Esta inconsistência ocorre devido a varias situações entre as mais comuns são desligados bruscos, má atualizaçao do sistema ou até mesmo o seu disco dando sinal que seu tempo de vida útil está chegando ao fim.

Para corrigir o erro de Inconsistency no disco/partição execute o comando;



```
fsck /dev/sda1
```

Onde o /dev/sda1 indica a partição sda1 do disco sda

Confirme o processo e no final reinicilize o sistema. Se tudo ocorreu como esperado o seu Linux estará pronto para operação novamente.

