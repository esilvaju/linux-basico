# Exercícios — Tópico 103: Comandos GNU e Unix

## Exercícios Práticos

### 1. Linha de Comando Básica

```bash
# a) Exiba seu diretório home
echo $HOME

# b) Veja o conteúdo do PATH
echo $PATH

# c) Mostre o código de retorno do último comando executado
ls /tmp
echo $?

# d) Execute um comando inexistente e observe o código de retorno
ls /diretorio_que_nao_existe
echo $?
```

**Pergunta:** Qual é o código de retorno de um comando bem-sucedido?

---

### 2. Filtros de Texto

```bash
# a) Conte o número de usuários no sistema (linhas do /etc/passwd)
wc -l /etc/passwd

# b) Exiba apenas os nomes de usuário (primeiro campo) de /etc/passwd
cut -d: -f1 /etc/passwd

# c) Ordene os nomes de usuário em ordem alfabética
cut -d: -f1 /etc/passwd | sort

# d) Mostre as 5 primeiras linhas de /etc/passwd
head -5 /etc/passwd

# e) Mostre as 5 últimas linhas de /etc/passwd
tail -5 /etc/passwd

# f) Converta os nomes de usuário para maiúsculas
cut -d: -f1 /etc/passwd | tr 'a-z' 'A-Z'
```

---

### 3. Gerenciamento de Arquivos

```bash
# a) Crie uma estrutura de diretórios aninhada
mkdir -p /tmp/teste/subpasta1/subpasta2

# b) Crie alguns arquivos de teste
touch /tmp/teste/arquivo1.txt /tmp/teste/arquivo2.log

# c) Copie todos os arquivos .txt para uma subpasta
cp /tmp/teste/*.txt /tmp/teste/subpasta1/

# d) Encontre todos os arquivos .txt na pasta /tmp/teste
find /tmp/teste -name "*.txt"

# e) Compacte a pasta /tmp/teste em um arquivo .tar.gz
tar -czvf /tmp/teste_backup.tar.gz /tmp/teste/

# f) Liste o conteúdo do arquivo compactado sem extrair
tar -tvf /tmp/teste_backup.tar.gz
```

---

### 4. Processos e Sinais

```bash
# a) Execute um processo em background
sleep 300 &

# b) Veja os jobs em background
jobs

# c) Encontre o PID do processo sleep
pgrep sleep

# d) Encerre o processo usando SIGTERM
kill $(pgrep sleep)

# e) Verifique que o processo foi encerrado
pgrep sleep
echo $?
```

---

### 5. Expressões Regulares

```bash
# a) Encontre todas as linhas de /etc/passwd que começam com 'root'
grep "^root" /etc/passwd

# b) Encontre linhas que terminam com '/bash'
grep "/bash$" /etc/passwd

# c) Encontre linhas que contêm um número entre 0 e 9
grep -E "[0-9]" /etc/passwd

# d) Conte quantos usuários usam /bin/bash como shell
grep -c "/bin/bash$" /etc/passwd
```

---

## Questões de Múltipla Escolha (Estilo LPIC-1)

**1.** Qual comando exibe as últimas 20 linhas de um arquivo e continua acompanhando novas linhas em tempo real?

- A) `tail -20 arquivo.log`
- B) `tail -f -n 20 arquivo.log`
- C) `head -f arquivo.log`
- D) `cat -f arquivo.log`

---

**2.** O comando `find /var -size +50M -type f` encontra:

- A) Arquivos com exatamente 50 MB em /var
- B) Diretórios maiores que 50 MB em /var
- C) Arquivos regulares maiores que 50 MB em /var
- D) Arquivos com menos de 50 MB em /var

---

**3.** Qual redirecionamento redireciona a saída de erros (stderr) para o mesmo lugar que stdout?

- A) `comando > saida.txt 1>&2`
- B) `comando 2>&1 > saida.txt`
- C) `comando > saida.txt 2>&1`
- D) `comando >> saida.txt 2>saida.txt`

---

**4.** Qual é o nice value padrão de um processo ao ser iniciado normalmente?

- A) -20
- B) 0
- C) 10
- D) 19

---

**5.** Qual sinal o comando `kill PID` envia por padrão?

- A) SIGKILL (9)
- B) SIGHUP (1)
- C) SIGSTOP (19)
- D) SIGTERM (15)

---

**6.** O comando `grep -v "root" /etc/passwd` exibe:

- A) Apenas as linhas que contêm "root"
- B) O número de linhas que contêm "root"
- C) Todas as linhas que **não** contêm "root"
- D) Os arquivos que contêm "root"

---

**7.** No vim, qual sequência de teclas salva o arquivo e sai do editor?

- A) `Ctrl+S` e depois `Ctrl+Q`
- B) `Esc` e depois `:wq`
- C) `Esc` e depois `:q!`
- D) `i` e depois `:wq`

---

**8.** Qual comando substitui "foo" por "bar" em todas as linhas de um arquivo, modificando-o diretamente?

- A) `sed 's/foo/bar/' arquivo.txt`
- B) `sed -i 's/foo/bar/g' arquivo.txt`
- C) `grep -r 's/foo/bar/g' arquivo.txt`
- D) `awk '{s/foo/bar/g}' arquivo.txt`

---

## Respostas

| Questão | Resposta | Explicação |
|---------|----------|------------|
| 1 | **B** | `tail -f -n 20` exibe as últimas 20 linhas e continua monitorando. |
| 2 | **C** | `-type f` = arquivo regular; `-size +50M` = maior que 50 MB. |
| 3 | **C** | `> saida.txt 2>&1` redireciona stdout para o arquivo e então stderr para stdout (que já aponta para o arquivo). A ordem importa! |
| 4 | **B** | O nice value padrão é 0. |
| 5 | **D** | O padrão é SIGTERM (15), que permite ao processo encerrar adequadamente. |
| 6 | **C** | A opção `-v` inverte a busca, exibindo linhas que NÃO correspondem ao padrão. |
| 7 | **B** | `Esc` sai do modo inserção, `:wq` salva (w = write) e sai (q = quit). |
| 8 | **B** | `sed -i` modifica o arquivo diretamente; o flag `g` substitui todas as ocorrências na linha. |
