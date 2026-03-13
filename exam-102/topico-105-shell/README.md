# Tópico 105 — Shell e Shell Scripting

## Objetivos (LPIC-1)

- **105.1** Customizar e usar o ambiente do shell
- **105.2** Customizar ou escrever scripts simples em Bash

---

## 105.1 — Ambiente do Shell

### Arquivos de Inicialização do Bash

| Arquivo | Quando é lido |
|---------|---------------|
| `/etc/profile` | Login shell — lido por todos os usuários |
| `/etc/profile.d/*.sh` | Scripts adicionais de profile |
| `~/.bash_profile` | Login shell — específico do usuário |
| `~/.bashrc` | Shell interativo não-login |
| `~/.bash_logout` | Ao sair de um login shell |
| `/etc/bash.bashrc` | Shell interativo não-login — todos os usuários |

```bash
# Recarregar o .bashrc sem reiniciar o terminal
source ~/.bashrc
. ~/.bashrc    # equivalente

# Ver o shell atual
echo $SHELL
```

### Variáveis de Ambiente

```bash
# Definir uma variável (apenas na sessão atual)
MINHA_VAR="valor"

# Exportar para subprocessos
export MINHA_VAR="valor"

# Ver todas as variáveis de ambiente
env
printenv

# Ver o valor de uma variável
echo $MINHA_VAR
printenv MINHA_VAR

# Remover uma variável
unset MINHA_VAR

# Variáveis permanentes — adicione ao ~/.bashrc ou ~/.bash_profile
echo 'export MINHA_VAR="valor"' >> ~/.bashrc
```

### Aliases

```bash
# Criar um alias (temporário)
alias ll='ls -la'
alias atualizar='sudo apt update && sudo apt upgrade'

# Ver todos os aliases
alias

# Remover um alias
unalias ll

# Alias permanente — adicione ao ~/.bashrc
echo "alias ll='ls -la'" >> ~/.bashrc
```

### Funções no Shell

```bash
# Definir uma função
minha_funcao() {
    echo "Olá, $1!"
}

# Chamar a função
minha_funcao "Mundo"

# Ver funções definidas
declare -f
```

### PATH

```bash
# Ver o PATH atual
echo $PATH

# Adicionar um diretório ao PATH (temporário)
export PATH=$PATH:/meu/diretorio

# Adicionar permanentemente (no ~/.bashrc)
echo 'export PATH=$PATH:/meu/diretorio' >> ~/.bashrc
```

---

## 105.2 — Shell Scripting com Bash

### Estrutura Básica de um Script

```bash
#!/bin/bash
# Descrição: Script de exemplo

# Variáveis
NOME="Linux"
VERSAO=1

# Exibir mensagem
echo "Bem-vindo ao $NOME versão $VERSAO!"
```

```bash
# Tornar o script executável
chmod +x meu_script.sh

# Executar
./meu_script.sh
bash meu_script.sh
```

### Variáveis Especiais

| Variável | Significado |
|----------|-------------|
| `$0` | Nome do script |
| `$1`, `$2`, ... | Argumentos posicionais |
| `$#` | Número de argumentos |
| `$@` | Todos os argumentos (como lista) |
| `$*` | Todos os argumentos (como string) |
| `$?` | Código de retorno do último comando |
| `$$` | PID do script atual |
| `$!` | PID do último processo em background |

### Estruturas de Controle

#### if/else

```bash
#!/bin/bash

ARQUIVO="$1"

if [ -f "$ARQUIVO" ]; then
    echo "O arquivo '$ARQUIVO' existe."
elif [ -d "$ARQUIVO" ]; then
    echo "'$ARQUIVO' é um diretório."
else
    echo "'$ARQUIVO' não encontrado."
fi
```

#### Operadores de Teste

**Para strings:**
| Operador | Significado |
|----------|-------------|
| `[ -z "$var" ]` | String vazia |
| `[ -n "$var" ]` | String não vazia |
| `[ "$a" = "$b" ]` | Igualdade |
| `[ "$a" != "$b" ]` | Diferença |

**Para números:**
| Operador | Significado |
|----------|-------------|
| `-eq` | Igual |
| `-ne` | Diferente |
| `-lt` | Menor que |
| `-le` | Menor ou igual |
| `-gt` | Maior que |
| `-ge` | Maior ou igual |

**Para arquivos:**
| Operador | Significado |
|----------|-------------|
| `-f arquivo` | É um arquivo regular |
| `-d arquivo` | É um diretório |
| `-e arquivo` | Existe |
| `-r arquivo` | Tem permissão de leitura |
| `-w arquivo` | Tem permissão de escrita |
| `-x arquivo` | Tem permissão de execução |
| `-s arquivo` | Não está vazio |

#### case

```bash
#!/bin/bash

case "$1" in
    start)
        echo "Iniciando serviço..."
        ;;
    stop)
        echo "Parando serviço..."
        ;;
    restart)
        echo "Reiniciando serviço..."
        ;;
    *)
        echo "Uso: $0 {start|stop|restart}"
        exit 1
        ;;
esac
```

#### Loops

```bash
# for — iterar sobre lista
for fruta in maçã banana laranja; do
    echo "Fruta: $fruta"
done

# for — iterar sobre arquivos
for arquivo in /tmp/*.txt; do
    echo "Processando: $arquivo"
done

# for — com range numérico (bash)
for i in {1..10}; do
    echo "Número: $i"
done

# while
CONTADOR=0
while [ $CONTADOR -lt 5 ]; do
    echo "Contador: $CONTADOR"
    CONTADOR=$((CONTADOR + 1))
done

# until (oposto do while)
until [ $CONTADOR -eq 0 ]; do
    echo "Contador: $CONTADOR"
    CONTADOR=$((CONTADOR - 1))
done
```

### Leitura de Input

```bash
# Ler entrada do usuário
read -p "Digite seu nome: " NOME
echo "Olá, $NOME!"

# Ler sem exibir (para senhas)
read -sp "Senha: " SENHA
echo ""
```

### Aritmética

```bash
# Usando $(( ))
RESULTADO=$((10 + 5))
echo $RESULTADO

# Usando expr
RESULTADO=$(expr 10 + 5)

# Incremento
CONTADOR=$((CONTADOR + 1))
((CONTADOR++))    # bash
```

### Funções em Scripts

```bash
#!/bin/bash

# Definir função
verificar_root() {
    if [ "$(id -u)" -ne 0 ]; then
        echo "Este script precisa ser executado como root."
        exit 1
    fi
}

# Chamar função
verificar_root

echo "Executando como root."
```

### Exemplo Prático — Script de Backup

```bash
#!/bin/bash
# Script de backup simples

ORIGEM="$HOME/documentos"
DESTINO="/backup"
DATA=$(date +%Y%m%d)
ARQUIVO_BACKUP="backup_$DATA.tar.gz"

# Verificar se o diretório de origem existe
if [ ! -d "$ORIGEM" ]; then
    echo "Erro: diretório de origem não encontrado."
    exit 1
fi

# Criar diretório de destino se não existir
mkdir -p "$DESTINO"

# Criar o backup
echo "Criando backup de $ORIGEM..."
tar -czvf "$DESTINO/$ARQUIVO_BACKUP" "$ORIGEM"

if [ $? -eq 0 ]; then
    echo "Backup criado com sucesso: $DESTINO/$ARQUIVO_BACKUP"
else
    echo "Erro ao criar o backup."
    exit 1
fi
```

---

## 📌 Pontos-Chave para o Exame

- `~/.bashrc` é lido em shells interativos não-login; `~/.bash_profile` em login shells
- `source arquivo` ou `. arquivo` executa o script no contexto atual do shell
- `export` torna a variável disponível para subprocessos
- `$?` retorna o código de saída do último comando (0 = sucesso)
- Use `[ ]` para testes; `[[ ]]` é específico do bash (mais poderoso)
- Sempre use aspas em variáveis: `"$VARIAVEL"` para evitar problemas com espaços

---

## ▶️ Próximo Tópico

[Tópico 106 — Interfaces de Usuário e Desktops](../topico-106-interfaces/README.md)
