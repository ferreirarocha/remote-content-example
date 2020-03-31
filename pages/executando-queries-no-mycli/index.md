O mycli  também possui o recurso de consultas favoritas, elas  são uma  maneira de salvar consultas usadas com frequência com um nome curto.

O seu uso é  bem simples veja;

| Nome | Sintaxe        | Função                              |
| ---- | -------------- | ----------------------------------- |
| \f   | \f [name]      | Lista ou executa queries favoritas. |
| \fd  | \fd [name]     | Deleta uma  query favorita.         |
| \fs  | \fs name query | Salva  uma query favorita.          |

### Acesse o mycli

```
mycli  -u root
```

## Salvando query

```
\fs users select user,host,plugin,password FROM mysql.user;
```

Esta  consulta de nome **users**  exibirá todos  os usuários da tabela  mysql.user

## Executando

Para executar essa consulta, basta executá-la da seguinte forma

```
\f users 
```

## Deletando query

Para deletá-la também é muito simples.

```
\fd users 
```

![sobre-as-funções](https://1.bp.blogspot.com/-FTA2ZDsNbMU/W6cAFmKLbCI/AAAAAAAACd4/9vaYAjSLUyQC7ofTATJ_oPTWmfKWz1ArwCLcBGAs/s1600/myclifavorites%2B%25284%2529.gif)

Marcadores: