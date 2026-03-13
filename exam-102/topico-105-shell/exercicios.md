# Exercícios — Tópico 105: Shell e Shell Scripting

## Exercícios Práticos

### 1. Ambiente do Shell

```bash
# a) Crie um alias chamado 'limpar' para o comando 'clear'
alias limpar='clear'

# b) Crie um alias permanente para 'ls -la' chamado 'll'
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc

# c) Adicione /usr/local/bin ao seu PATH (se ainda não estiver)
echo 'export PATH=$PATH:/usr/local/bin' >> ~/.bashrc
source ~/.bashrc

# d) Veja todos os aliases definidos
alias
```

---

### 2. Scripts Básicos

**Script 1:** Crie o arquivo `~/ola_mundo.sh` com o conteúdo:

```bash
#!/bin/bash
echo "Olá, Mundo!"
echo "Usuário atual: $USER"
echo "Diretório atual: $PWD"
echo "Data e hora: $(date)"
```

Execute:
```bash
chmod +x ~/ola_mundo.sh
~/ola_mundo.sh
```

---

**Script 2:** Crie `~/verifica_arquivo.sh` que aceita um nome de arquivo como argumento:

```bash
#!/bin/bash

if [ $# -eq 0 ]; then
    echo "Uso: $0 <nome_do_arquivo>"
    exit 1
fi

ARQUIVO="$1"

if [ -f "$ARQUIVO" ]; then
    echo "'$ARQUIVO' é um arquivo regular."
    echo "Tamanho: $(du -sh "$ARQUIVO" | cut -f1)"
elif [ -d "$ARQUIVO" ]; then
    echo "'$ARQUIVO' é um diretório."
else
    echo "'$ARQUIVO' não existe."
    exit 1
fi
```

Teste com:
```bash
chmod +x ~/verifica_arquivo.sh
~/verifica_arquivo.sh /etc/passwd
~/verifica_arquivo.sh /home
~/verifica_arquivo.sh /arquivo_que_nao_existe
```

---

**Script 3:** Crie `~/conta_usuarios.sh` que conta e lista usuários com shell `/bin/bash`:

```bash
#!/bin/bash

SHELL_ALVO="/bin/bash"
USUARIOS=$(grep "$SHELL_ALVO" /etc/passwd | cut -d: -f1)
TOTAL=$(echo "$USUARIOS" | wc -l)

echo "Usuários com shell $SHELL_ALVO:"
echo "$USUARIOS"
echo ""
echo "Total: $TOTAL usuário(s)"
```

---

### 3. Loops e Condicionais

**Script 4:** Crie `~/tabuada.sh` que imprime a tabuada de um número:

```bash
#!/bin/bash

if [ $# -ne 1 ]; then
    echo "Uso: $0 <número>"
    exit 1
fi

NUMERO=$1

echo "=== Tabuada do $NUMERO ==="
for i in {1..10}; do
    RESULTADO=$((NUMERO * i))
    echo "$NUMERO x $i = $RESULTADO"
done
```

---

## Questões de Múltipla Escolha (Estilo LPIC-1)

**1.** Qual arquivo é lido pelo Bash quando um usuário abre um **terminal interativo** (não-login) no ambiente gráfico?

- A) `~/.bash_profile`
- B) `/etc/profile`
- C) `~/.bashrc`
- D) `~/.bash_login`

---

**2.** Qual é a saída do seguinte script?

```bash
#!/bin/bash
X=5
echo $((X * 2 + 3))
```

- A) `X * 2 + 3`
- B) `13`
- C) `10`
- D) Erro

---

**3.** Qual comando recarrega o arquivo `~/.bashrc` na sessão atual sem precisar fechar o terminal?

- A) `exec ~/.bashrc`
- B) `source ~/.bashrc`
- C) `bash ~/.bashrc`
- D) `load ~/.bashrc`

---

**4.** Em um script bash, qual variável especial contém o **número de argumentos** passados ao script?

- A) `$0`
- B) `$@`
- C) `$#`
- D) `$$`

---

**5.** Qual operador de teste verifica se um arquivo **existe e é um diretório**?

- A) `[ -f "$var" ]`
- B) `[ -e "$var" ]`
- C) `[ -d "$var" ]`
- D) `[ -r "$var" ]`

---

**6.** Qual construção executa o bloco de código enquanto a condição for **falsa**?

- A) `while`
- B) `until`
- C) `for`
- D) `if`

---

## Respostas

| Questão | Resposta | Explicação |
|---------|----------|------------|
| 1 | **C** | `~/.bashrc` é lido em shells interativos não-login (ex: ao abrir um terminal no ambiente gráfico). |
| 2 | **B** | `$((5 * 2 + 3))` = `$((10 + 3))` = `13`. |
| 3 | **B** | `source ~/.bashrc` (ou `. ~/.bashrc`) executa o arquivo no contexto do shell atual. |
| 4 | **C** | `$#` contém o número de argumentos passados ao script. |
| 5 | **C** | `-d` testa se o caminho existe e é um diretório. |
| 6 | **B** | `until` executa enquanto a condição for falsa (oposto do `while`). |
