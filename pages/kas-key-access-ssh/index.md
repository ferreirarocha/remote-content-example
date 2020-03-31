# Sobre

O KAS Key Access SSH, automatiza a geração e inserção de chave pública em servidores ssh, ele também cria funções para cada conexão no shell corrente afim de facilitar o acesso futuro. Ele também pode exportar, importar, e criar novos pares de chave de conexão, saiba mais na documentação.

# Instalando o script

## Instalação automática

```
wget bit.ly/install-kas ; bash install-kas --install
```

Ou

```
curl -s https://raw.githubusercontent.com/ferreirarocha/Key-Access-SSH/master/kas | bash -s -- --install
```

## Instalação manual

```
wget https://raw.githubusercontent.com/ferreirarocha/Key-Access-SSH/master/kas
```

```
chmod +x kas
```
```
cp /usr/bin/
```

# Atualizando o script

Para atualizar o script ou

```
kas --update
```

# Desinstalando o script

```
kas --uninstall
```

# Menu de ajuda

```
Todas as opções;

-a --add             Adiciona um nova conexão
-c --conf            Lista os arquivos de configuração e diretórios do script
-e --edit            Edita o arquivo de  conexões
-h --help            Exibe o menu de ajuda
-i --import          Importa registro de conexões
-I --import-all      Importa  as chaves e as  conexões registradas
-l --list            Lista as conexões registradas
-n --new-key         Cria um novo par de chaves
-r --reset           Limpa registro de conexões
-s --save-key        Salva o par de chaves utilizadas  pelo script  na home do usuário corrente
   --set-editor      Configura o editor padrão
   --uninstall       Remove o script
-U --update          Atualiza o Key Access ssh (kas)
-x --export          Exporta o registro de conexões
-X --export-all      Salva as chaves e as  conexões registradas
```

# Adicionando registro de conexão

Para registrar uma nova conexão siga o exemplo;

```
kas -a servidor user@192.168.1.1
```

# Exportando registros de conexão

Para exportar todos os registros de conexões siga o exemplo;

```
kas -e nome-do-arquivo
```

ou

```
kas --export nome-do-arquivo
```

# Importando registro de conexão

Para importar todos os registros de conexões siga o exemplo;

```
kas -i nome-do-arquivo
```

ou

```
kas --import nome-do-arquivo
```

# Importando todos os registros e chaves de conexão

Para importar todos os registros de conexões e chave ssh siga o exemplo;

```
kas -I nome-do-arquivo
```

ou

```
kas --import-all nome-do-arquivo
```

# Exportando todos os registros e chaves de conexão

Para exportar todos os registros de conexões e chave ssh siga o exemplo;

```
kas -X nome-do-arquivo
```

ou

```
kas --export-all nome-do-arquivo
```

# Exibir diretório e arquivos de Configuração

Abaixo é apresentado o conjuto de diretórios e arquivos que compõem a aplicação;

```
access_functions     Armazena as conexões registradas
~/.local/share/acesso/       Armazena os arquivos de configuração da aplicação
~/.bashrc            Exportar o arquivo access_functions para o shell corrent
~/.zshrc             Exportar o arquivo access_functions para o shell corrente
~/.ssh/                      Diretório utilizado para  acessar o par de chaves
/usr/bin/            Diretório com  script executável
```

# Editando registros

```
kas -e
```

ou

```
kas --edit
```

# Alterando o editor padrão

```
kas  --set-editor
```

Exemplo

```
kas --set-editor  vim
```

# Salvando o par de chaves usada pelo script

```
kas -s
```

ou

```
kas --save-key
```

# Limpando registro de conexões

Para limpar o registro de conexões execute o comando

```
kas -r
```

ou

```
kas  --reset
```

# Listando as conexões

```
kas -l
```

ou

```
kas --list
```

# Criando um novo par de chaves

```
kas --new-key
```

ou

```bash
kas -n
```

