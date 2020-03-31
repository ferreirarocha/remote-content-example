Algumas vezes precisamos calcular a diferença entre datas de forma mais precisa,  no LibreOffice podemos utilizar as funções DATADIF + CONCATENAR, a junção dessas duas funções nos entrega exatamente isso, um cálculo de datas preciso, com Anos, Dias e Meses corridos entre a data inicial e a data final.

```
=CONCATENAR(DATADIF(B4;HOJE();"Y");" Anos, "; DATADIF(B4;HOJE();"YM");" Meses e "; DATADIF(B4;HOJE();"MD"); " Dias")
```

Os intervalos utilizados nessas fórmula foram os seguintes, **y**,**ym**, **md** , com suas descrições logo abaixo.

| "y"  | Número de anos inteiros entre a data inicial e a data final. |
| ---- | ------------------------------------------------------------ |
| "ym" | Número de meses inteiros ao subtrair os anos da diferença entre a data inicial e a data final. |
| "md" | Número de dias inteiros ao subtrair os anos e os meses da diferença entre a data inicial e a data final. |



[![img](https://3.bp.blogspot.com/-QKmtWY3CTBU/WPFSPC6RDJI/AAAAAAAAATY/nMavCH0_XJEHvk-GiC3MzvNZSATiP-CBwCLcB/s1600/Captura%2Bde%2Btela%2Bde%2B2017-04-14%2B19-48-26.png)](https://3.bp.blogspot.com/-QKmtWY3CTBU/WPFSPC6RDJI/AAAAAAAAATY/nMavCH0_XJEHvk-GiC3MzvNZSATiP-CBwCLcB/s1600/Captura%2Bde%2Btela%2Bde%2B2017-04-14%2B19-48-26.png)


[Planilha utilizada no post](https://drive.google.com/open?id=0B2KdodQ7iVQsTzN1VmRMdDB1MkU)

[Fonte Help LibreOffice](https://help.libreoffice.org/Calc/DATEDIF/pt-BR)