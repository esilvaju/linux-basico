# Exercícios — Tópico 104: Dispositivos, Filesystems e FHS

## Exercícios Práticos

### 1. Explorando o Sistema de Arquivos

```bash
# a) Veja o uso do disco em formato legível
df -h

# b) Veja o uso de inodes
df -i

# c) Descubra quanto espaço o diretório /var ocupa
du -sh /var

# d) Liste as 5 pastas que mais ocupam espaço em /var
du -sh /var/* 2>/dev/null | sort -rh | head -5

# e) Veja os UUIDs dos dispositivos montados
blkid
```

---

### 2. Permissões de Arquivos

```bash
# a) Crie um diretório de teste
mkdir /tmp/perm_teste

# b) Crie um arquivo dentro dele
touch /tmp/perm_teste/arquivo.txt

# c) Defina as permissões: dono=rw, grupo=r, outros=nada
chmod 640 /tmp/perm_teste/arquivo.txt
ls -la /tmp/perm_teste/arquivo.txt

# d) Adicione permissão de execução apenas para o dono
chmod u+x /tmp/perm_teste/arquivo.txt
ls -la /tmp/perm_teste/arquivo.txt

# e) Defina o sticky bit no diretório
chmod +t /tmp/perm_teste
ls -la /tmp/ | grep perm_teste
```

---

### 3. Links

```bash
# a) Crie um arquivo de teste
echo "conteúdo original" > /tmp/original.txt

# b) Crie um hard link
ln /tmp/original.txt /tmp/hardlink.txt

# c) Crie um soft link (simbólico)
ln -s /tmp/original.txt /tmp/softlink.txt

# d) Compare os inodes dos arquivos
ls -lai /tmp/original.txt /tmp/hardlink.txt /tmp/softlink.txt

# e) Modifique o arquivo original e veja nos links
echo "nova linha" >> /tmp/original.txt
cat /tmp/hardlink.txt
cat /tmp/softlink.txt
```

---

### 4. Identificação de Locais no FHS

Com base no FHS, responda: onde os seguintes arquivos deveriam estar?

- Arquivo de configuração do servidor web nginx
- Logs do sistema
- Executável do comando `ls`
- Diretório home do usuário "alice"
- Arquivos temporários criados por aplicações

---

## Questões de Múltipla Escolha (Estilo LPIC-1)

**1.** Qual comando verifica e repara um filesystem ext4 que está **desmontado**?

- A) `mkfs.ext4 /dev/sdb1`
- B) `tune2fs -f /dev/sdb1`
- C) `e2fsck -f /dev/sdb1`
- D) `mount -o repair /dev/sdb1`

---

**2.** O arquivo `/etc/fstab` contém a linha abaixo. O que o campo `2` no final significa?

```
/dev/sda2   /boot   ext4   defaults   0   2
```

- A) Número de cópias do filesystem
- B) Ordem de verificação pelo fsck na inicialização
- C) Nível de compressão
- D) Prioridade de montagem

---

**3.** Qual é a permissão octal equivalente a `rwxr-x---`?

- A) 775
- B) 754
- C) 750
- D) 755

---

**4.** Qual das afirmações sobre hard links é **verdadeira**?

- A) Hard links podem apontar para diretórios
- B) Hard links podem cruzar diferentes sistemas de arquivos
- C) Hard links e o arquivo original compartilham o mesmo inode
- D) Hard links ficam quebrados se o arquivo original for removido

---

**5.** Qual diretório FHS é destinado a **dados variáveis** como logs e filas de impressão?

- A) `/tmp`
- B) `/srv`
- C) `/var`
- D) `/run`

---

**6.** O comando `chmod 4755 /usr/bin/meu_programa` aplica qual permissão especial?

- A) Sticky bit
- B) SGID
- C) SUID
- D) immutable flag

---

**7.** Qual comando mostra as informações do superbloco de uma partição ext4?

- A) `fsck -l /dev/sda1`
- B) `tune2fs -l /dev/sda1`
- C) `mkfs.ext4 -l /dev/sda1`
- D) `e2fsck -l /dev/sda1`

---

**8.** Onde ficam os arquivos executáveis instalados localmente por um administrador (não pelo gerenciador de pacotes)?

- A) `/bin`
- B) `/opt`
- C) `/usr/local/bin`
- D) `/sbin`

---

## Respostas

| Questão | Resposta | Explicação |
|---------|----------|------------|
| 1 | **C** | `e2fsck -f` força a verificação completa em filesystems ext. O filesystem deve estar desmontado. |
| 2 | **B** | O último campo define a ordem de verificação do `fsck` (0=não verificar, 1=primeiro, 2=depois). |
| 3 | **C** | rwx=7, r-x=5, ---=0. Portanto 750. |
| 4 | **C** | Hard links compartilham o mesmo inode (número de identificação do arquivo no filesystem). |
| 5 | **C** | `/var` é para dados variáveis: logs (`/var/log`), spool (`/var/spool`), cache (`/var/cache`). |
| 6 | **C** | O dígito 4 em 4755 indica SUID. O programa executará com as permissões do **dono** do arquivo. |
| 7 | **B** | `tune2fs -l` exibe informações do superbloco como contagem de montagens, data da última verificação, etc. |
| 8 | **C** | `/usr/local/bin` é o local padrão para executáveis instalados localmente fora do gerenciador de pacotes. |

---

## Respostas da Questão 4 (FHS)

| Item | Local Correto |
|------|---------------|
| Configuração do nginx | `/etc/nginx/nginx.conf` |
| Logs do sistema | `/var/log/` |
| Executável do `ls` | `/bin/ls` ou `/usr/bin/ls` |
| Home do usuário alice | `/home/alice/` |
| Arquivos temporários | `/tmp/` |
