Alguns departamentos possuem subdiretórios como o Suporte e a Diretoria,  queremos também definir uma permissão padrão para os novos diretórios,  então utilizaremos a opção -m do mkdir. veja como ficaria a sintaxe  desse comando.



```javascript
mkdir -p -m 755 ~/Arquivologia/conta{1..500}/{finaneiro,rh,dp,suporte/{usuario/{linux/{nivel-1,nivel-2},windows/{nivel-1,nivel-2}},infraestrutura},almoxarifado,diretoria/{diretor,vice-diretor},administrativo,zeladoria}
```

[![img](https://4.bp.blogspot.com/-9IIAKEPfcBA/WNV_RlTvqtI/AAAAAAAATWs/DIRyvPWyGgs0rIYyUMQBl1r9RCBgm2P-QCPcB/s640/estrutura-de-diretrio-no-linux.png)](https://4.bp.blogspot.com/-9IIAKEPfcBA/WNV_RlTvqtI/AAAAAAAATWs/DIRyvPWyGgs0rIYyUMQBl1r9RCBgm2P-QCPcB/s1600/estrutura-de-diretrio-no-linux.png)



> 