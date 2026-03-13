# Tópico 103 — Comandos GNU e Unix

## Objetivos (LPIC-1)

- **103.1** Trabalhar na linha de comando
- **103.2** Processar fluxos de texto com filtros
- **103.3** Gerenciamento básico de arquivos
- **103.4** Usar streams, pipes e redirecionamentos
- **103.5** Criar, monitorar e encerrar processos
- **103.6** Modificar prioridades de execução de processos
- **103.7** Pesquisar arquivos de texto com expressões regulares
- **103.8** Edição básica de arquivos

---

## 103.1 — Trabalhando na Linha de Comando

### Comandos Essenciais

```bash
# Exibir variável de ambiente
echo $HOME
echo $PATH
echo $USER

# Definir uma variável de ambiente
export MINHA_VAR="valor"

# Exibir todas as variáveis de ambiente
env
printenv

# Ver o histórico de comandos
history
history | grep apt

# Executar um comando do histórico
!42        # executa o comando número 42
!!         # re-executa o último comando

# Obter ajuda de um comando
man ls
info bash
ls --help

# Descobrir onde está um executável
which python3
type ls
whereis bash

# Ver o manual de uma função
man 3 printf     # seção 3 = funções de biblioteca C
```

### Variáveis Especiais

| Variável | Significado |
|----------|-------------|
| `$HOME` | Diretório home do usuário |
| `$PATH` | Caminhos onde o shell busca executáveis |
| `$USER` | Nome do usuário atual |
| `$SHELL` | Shell em uso |
| `$PWD` | Diretório atual |
| `$?` | Código de retorno do último comando |

---

## 103.2 — Processamento de Texto (Filtros)

```bash
# cat — exibe o conteúdo de um arquivo
cat arquivo.txt
cat -n arquivo.txt     # com números de linha

# head / tail — primeiras/últimas linhas
head -5 arquivo.txt
tail -10 arquivo.txt
tail -f /var/log/syslog    # acompanha o arquivo em tempo real

# sort — ordena linhas
sort arquivo.txt
sort -r arquivo.txt        # ordem reversa
sort -n arquivo.txt        # ordem numérica
sort -k2 arquivo.txt       # ordena pela 2ª coluna

# uniq — remove linhas duplicadas consecutivas
sort arquivo.txt | uniq
sort arquivo.txt | uniq -c    # conta ocorrências

# cut — extrai colunas
cut -d: -f1 /etc/passwd       # campo 1 separado por ':'
cut -c1-10 arquivo.txt        # caracteres 1 a 10

# tr — substitui/apaga caracteres
echo "hello" | tr 'a-z' 'A-Z'     # converte para maiúsculas
echo "hello world" | tr -d 'l'    # remove o caractere 'l'

# wc — conta linhas, palavras e caracteres
wc -l arquivo.txt    # conta linhas
wc -w arquivo.txt    # conta palavras
wc -c arquivo.txt    # conta bytes

# paste — une arquivos lado a lado
paste arquivo1.txt arquivo2.txt

# split — divide um arquivo grande
split -l 100 arquivo.txt parte_    # 100 linhas por parte

# od — exibe arquivo em formato octal/hex
od -c arquivo.txt
od -x arquivo.txt

# nl — adiciona números de linha
nl arquivo.txt
```

---

## 103.3 — Gerenciamento Básico de Arquivos

```bash
# Navegar
ls -la              # lista detalhada com arquivos ocultos
ls -lh              # tamanhos legíveis por humanos
cd /home/usuario    # muda de diretório
pwd                 # exibe o diretório atual

# Criar / Copiar / Mover / Remover
touch arquivo.txt          # cria arquivo vazio ou atualiza timestamp
mkdir nova_pasta           # cria diretório
mkdir -p a/b/c             # cria diretórios aninhados

cp arquivo.txt destino/    # copia arquivo
cp -r pasta/ destino/      # copia diretório recursivamente

mv arquivo.txt novo_nome.txt   # renomeia/move arquivo
mv pasta/ /tmp/                # move diretório

rm arquivo.txt             # remove arquivo
rm -r pasta/               # remove diretório recursivamente
rm -f arquivo.txt          # força remoção sem confirmação

# Encontrar arquivos
find /home -name "*.txt"              # por nome
find /var -size +10M                  # maiores que 10 MB
find /tmp -mtime -1                   # modificados nas últimas 24h
find / -type f -name "passwd"         # arquivos com nome 'passwd'
find /home -user joao                 # pertencentes ao usuário joao

# Compactação / Arquivamento
tar -cvf arquivo.tar pasta/           # criar tar
tar -czvf arquivo.tar.gz pasta/       # criar tar compactado com gzip
tar -cjvf arquivo.tar.bz2 pasta/      # criar tar compactado com bzip2
tar -xzvf arquivo.tar.gz              # extrair tar.gz
tar -tvf arquivo.tar                  # listar conteúdo sem extrair

gzip arquivo.txt      # compactar (cria arquivo.txt.gz)
gunzip arquivo.txt.gz # descompactar

bzip2 arquivo.txt      # compactar com bzip2
bunzip2 arquivo.txt.bz2

xz arquivo.txt        # compactar com xz
unxz arquivo.txt.xz

zip arquivo.zip pasta/    # compactar em formato zip
unzip arquivo.zip         # descompactar zip
```

---

## 103.4 — Streams, Pipes e Redirecionamentos

### Streams Padrão

| Stream | Número | Descrição |
|--------|--------|-----------|
| stdin | 0 | Entrada padrão (teclado) |
| stdout | 1 | Saída padrão (tela) |
| stderr | 2 | Saída de erros (tela) |

```bash
# Redirecionar saída para arquivo (sobrescreve)
ls -la > lista.txt

# Redirecionar saída para arquivo (acrescenta)
echo "nova linha" >> lista.txt

# Redirecionar entrada (lê de arquivo)
sort < nomes.txt

# Redirecionar erros para arquivo
ls /nao_existe 2> erros.txt

# Redirecionar saída e erros para o mesmo arquivo
ls /nao_existe > tudo.txt 2>&1
ls /nao_existe &> tudo.txt    # forma abreviada (bash)

# Descartar saída
ls /tmp > /dev/null
ls /nao_existe 2> /dev/null

# Pipe — encadeia a saída de um comando na entrada de outro
ls -la | grep ".txt"
cat /etc/passwd | cut -d: -f1 | sort

# tee — divide a saída: exibe e salva simultaneamente
ls -la | tee lista.txt

# xargs — usa a saída como argumentos de outro comando
find /tmp -name "*.log" | xargs rm
echo "hello world" | xargs echo
```

---

## 103.5 — Processos

```bash
# Listar processos
ps aux                    # todos os processos
ps -ef                    # formato alternativo
ps aux | grep nginx       # filtrar por nome

# Monitor interativo de processos
top
htop    # versão melhorada (pode precisar instalar)

# Ver processos em árvore
pstree

# Encontrar PID de um processo
pgrep nginx
pidof nginx

# Enviar sinais a processos
kill PID              # envia SIGTERM (15) — encerramento gracioso
kill -9 PID           # envia SIGKILL — força encerramento
kill -HUP PID         # envia SIGHUP — recarregar configuração
killall nginx         # encerra todos os processos 'nginx'
pkill -f "python"     # encerra processos pelo padrão do nome

# Executar em background / foreground
comando &             # executa em background
jobs                  # lista jobs em background
fg %1                 # traz job 1 para foreground
bg %1                 # envia job para background
Ctrl+Z                # suspende o processo atual

# nohup — mantém o processo rodando após logout
nohup script.sh &

# Verificar uso de recursos
free -h               # memória RAM e swap
uptime                # tempo de atividade e carga do sistema
```

### Sinais Importantes

| Sinal | Número | Descrição |
|-------|--------|-----------|
| SIGTERM | 15 | Encerramento gracioso (padrão do `kill`) |
| SIGKILL | 9 | Encerramento forçado (não pode ser ignorado) |
| SIGHUP | 1 | Reiniciar/recarregar |
| SIGSTOP | 19 | Pausar processo |
| SIGCONT | 18 | Continuar processo pausado |

---

## 103.6 — Prioridades de Processo (nice)

O **nice value** vai de -20 (maior prioridade) a +19 (menor prioridade). O padrão é 0.

```bash
# Iniciar um processo com prioridade específica
nice -n 10 comando         # nice value = 10 (menor prioridade)
nice -n -5 comando         # nice value = -5 (maior prioridade — requer root)

# Alterar a prioridade de um processo já em execução
renice -n 15 -p PID        # altera o nice value do processo PID
renice -n 10 -u usuario    # altera para todos os processos do usuário

# Ver nice value nos processos
ps aux -o pid,ni,comm      # coluna NI = nice value
top                        # coluna NI
```

---

## 103.7 — Expressões Regulares (grep / sed / awk)

### grep

```bash
# Buscar padrão em arquivo
grep "erro" /var/log/syslog
grep -i "erro" arquivo.txt       # -i = case insensitive
grep -r "função" /home/usuario/  # -r = recursivo
grep -n "import" script.py       # -n = mostrar números de linha
grep -v "comentário" arquivo.txt # -v = inverso (exclui linhas com o padrão)
grep -c "erro" arquivo.txt       # -c = conta ocorrências
grep -l "TODO" *.py              # -l = lista apenas os arquivos

# Expressões regulares estendidas (-E ou egrep)
grep -E "^root|^daemon" /etc/passwd   # linhas que começam com root ou daemon
grep -E "[0-9]{3}" arquivo.txt        # 3 dígitos consecutivos
```

### Metacaracteres Básicos de Regex

| Símbolo | Significado |
|---------|-------------|
| `.` | Qualquer caractere (exceto nova linha) |
| `*` | Zero ou mais do anterior |
| `+` | Um ou mais do anterior |
| `?` | Zero ou um do anterior |
| `^` | Início da linha |
| `$` | Fim da linha |
| `[]` | Conjunto de caracteres |
| `[^]` | Negação do conjunto |
| `\` | Escape |

### sed — Editor de Fluxo

```bash
# Substituir texto
sed 's/antigo/novo/' arquivo.txt          # substitui primeira ocorrência por linha
sed 's/antigo/novo/g' arquivo.txt         # substitui todas as ocorrências
sed -i 's/antigo/novo/g' arquivo.txt      # modifica o arquivo diretamente

# Deletar linhas
sed '3d' arquivo.txt                      # deleta a linha 3
sed '/padrão/d' arquivo.txt               # deleta linhas com 'padrão'

# Exibir linhas específicas
sed -n '5,10p' arquivo.txt                # imprime linhas 5 a 10
```

### awk — Processamento de Texto

```bash
# Imprimir colunas específicas
awk '{print $1}' arquivo.txt              # primeira coluna
awk -F: '{print $1, $6}' /etc/passwd      # usando ':' como separador

# Com condição
awk '$3 > 100 {print $1}' arquivo.txt     # imprime col. 1 onde col. 3 > 100

# Contar linhas
awk 'END {print NR}' arquivo.txt
```

---

## 103.8 — Edição Básica de Arquivos (vi/vim)

### Modos do vim

| Modo | Descrição | Como entrar |
|------|-----------|-------------|
| Normal | Navegação e comandos | Padrão / `Esc` |
| Inserção | Digitar texto | `i`, `a`, `o` |
| Visual | Selecionar texto | `v` |
| Comando | Salvar, sair, etc. | `:` |

### Comandos Essenciais do vim

```
# Abrir arquivo
vim arquivo.txt

# Navegação (modo Normal)
h j k l    → esquerda, baixo, cima, direita
gg         → ir para o início do arquivo
G          → ir para o final do arquivo
:10        → ir para a linha 10

# Edição (entrar no modo Inserção)
i    → inserir antes do cursor
a    → inserir após o cursor
o    → nova linha abaixo
O    → nova linha acima

# Sair do modo Inserção
Esc

# Salvar e sair (modo Comando)
:w          → salvar
:q          → sair (só funciona se não houver alterações)
:wq         → salvar e sair
:q!         → sair sem salvar
:wq!        → forçar salvar e sair

# Busca
/palavra    → busca à frente
?palavra    → busca para trás
n           → próxima ocorrência
N           → ocorrência anterior

# Substituição
:%s/antigo/novo/g    → substituir em todo o arquivo
```

---

## 📌 Pontos-Chave para o Exame

- `find` com opções `-name`, `-type`, `-size`, `-mtime`, `-user` é muito cobrado
- Pipes `|` encadeiam comandos; redirecionamentos `>`, `>>`, `<`, `2>` controlam streams
- `kill -9 PID` = SIGKILL (forçado); `kill PID` = SIGTERM (gracioso)
- Nice value: -20 (mais prioridade) a +19 (menos prioridade)
- `grep -E` ou `egrep` para expressões regulares estendidas
- No vim: `Esc` para voltar ao modo Normal; `:wq` para salvar e sair

---

## ▶️ Próximo Tópico

[Tópico 104 — Dispositivos, Filesystems e FHS](../topico-104-filesystems/README.md)
