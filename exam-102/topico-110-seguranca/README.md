# Tópico 110 — Segurança

## Objetivos (LPIC-1)

- **110.1** Realizar tarefas de administração de segurança
- **110.2** Configurar segurança do host
- **110.3** Proteger dados com criptografia (GPG e SSH)

---

## 110.1 — Tarefas de Administração de Segurança

### Verificação de Usuários e Permissões

```bash
# Ver quem está logado no sistema
who
w
last
lastb           # tentativas de login malsucedidas

# Verificar arquivos com SUID (permissão especial)
find / -perm -4000 -type f 2>/dev/null
find / -perm -4000 -ls 2>/dev/null

# Verificar arquivos com SGID
find / -perm -2000 -type f 2>/dev/null

# Verificar arquivos sem dono
find / -nouser 2>/dev/null
find / -nogroup 2>/dev/null

# Verificar arquivos graváveis por qualquer pessoa
find / -perm -o+w -type f 2>/dev/null
```

### Gerenciamento de Senhas

```bash
# Definir expiração de senha
chage -l joao                  # ver política de senha do usuário
chage -M 90 joao               # senha expira em 90 dias
chage -m 7 joao                # deve usar por no mínimo 7 dias
chage -E 2025-12-31 joao       # conta expira em 31/12/2025
chage -W 14 joao               # avisar 14 dias antes da expiração

# Bloquear/desbloquear conta
passwd -l joao     # bloquear (lock)
passwd -u joao     # desbloquear (unlock)

# Forçar mudança de senha no próximo login
passwd -e joao
chage -d 0 joao
```

### Limites de Recursos do Usuário

```bash
# Ver limites do usuário atual
ulimit -a

# Definir limite de processos (temporário)
ulimit -u 100

# Configuração persistente de limites
/etc/security/limits.conf
# Exemplo:
# @estudantes  hard  nproc  50
# joao         soft  nofile  100
```

### Verificação de Portas e Serviços

```bash
# Ver serviços em escuta
ss -tuln
netstat -tuln         # legado

# Ver qual processo usa uma porta
ss -tulnp
fuser 80/tcp          # qual processo usa a porta 80

# Desativar serviços desnecessários
systemctl disable nome_servico
systemctl stop nome_servico
```

---

## 110.2 — Segurança do Host

### TCP Wrappers

Os **TCP Wrappers** controlam o acesso a serviços de rede através dos arquivos `/etc/hosts.allow` e `/etc/hosts.deny`.

```bash
# /etc/hosts.allow — acesso permitido (verificado primeiro)
sshd: 192.168.1.0/255.255.255.0     # permite SSH da rede local
ALL: 127.0.0.1                       # permite tudo do loopback

# /etc/hosts.deny — acesso negado
sshd: ALL                            # nega SSH de todos os outros
ALL: ALL                             # nega tudo que não foi permitido

# Formato: serviço: host/rede
```

### Firewall com nftables / iptables

```bash
# Ver regras de firewall (iptables)
iptables -L
iptables -L -v -n

# Ver regras nftables (moderno)
nft list ruleset

# Exemplos com iptables:
# Bloquear acesso à porta 23 (telnet)
iptables -A INPUT -p tcp --dport 23 -j DROP

# Permitir SSH apenas da rede local
iptables -A INPUT -p tcp --dport 22 -s 192.168.1.0/24 -j ACCEPT
iptables -A INPUT -p tcp --dport 22 -j DROP

# Salvar regras (Debian)
iptables-save > /etc/iptables/rules.v4
```

### Verificação de Integridade — Nmap

```bash
# Verificar portas abertas no próprio host
nmap localhost
nmap -sV localhost        # com versão dos serviços
nmap -p 1-1000 localhost  # apenas portas 1-1000
```

---

## 110.3 — Criptografia: GPG e SSH

### GPG — GNU Privacy Guard

O **GPG** é usado para criptografar e assinar arquivos e e-mails.

```bash
# Gerar um par de chaves GPG
gpg --gen-key
gpg --full-generate-key    # com mais opções

# Listar chaves
gpg --list-keys            # chaves públicas
gpg --list-secret-keys     # chaves privadas

# Exportar chave pública
gpg --export -a "Nome do Usuário" > chave_publica.asc
gpg --export --armor nome@email.com > chave_publica.asc

# Importar chave pública de outra pessoa
gpg --import chave_publica.asc

# Criptografar um arquivo
gpg --encrypt --recipient nome@email.com arquivo.txt
gpg -e -r nome@email.com arquivo.txt    # versão curta

# Descriptografar um arquivo
gpg --decrypt arquivo.txt.gpg
gpg -d arquivo.txt.gpg

# Assinar um arquivo
gpg --sign arquivo.txt                  # assinatura binária
gpg --clearsign arquivo.txt             # assinatura embutida no texto
gpg --detach-sign arquivo.txt           # assinatura separada

# Verificar assinatura
gpg --verify arquivo.txt.sig arquivo.txt
```

### SSH — Secure Shell

O **SSH** é o protocolo padrão para acesso remoto seguro.

```bash
# Conectar a um servidor
ssh usuario@servidor
ssh -p 2222 usuario@servidor        # porta diferente

# Copiar arquivo com SCP
scp arquivo.txt usuario@servidor:/tmp/
scp -r pasta/ usuario@servidor:/home/usuario/

# Sincronizar com rsync via SSH
rsync -avz pasta/ usuario@servidor:/destino/
```

#### Chaves SSH

```bash
# Gerar par de chaves SSH
ssh-keygen -t rsa -b 4096             # RSA 4096 bits
ssh-keygen -t ed25519                  # Ed25519 (recomendado, mais moderno)

# As chaves são criadas em:
~/.ssh/id_rsa           # chave privada (NUNCA compartilhe!)
~/.ssh/id_rsa.pub       # chave pública (pode compartilhar)
~/.ssh/id_ed25519
~/.ssh/id_ed25519.pub

# Copiar chave pública para o servidor
ssh-copy-id usuario@servidor
# Manualmente: adicionar o conteúdo de ~/.ssh/id_rsa.pub ao
# arquivo ~/.ssh/authorized_keys no servidor

# Agente SSH — gerencia as chaves
eval $(ssh-agent)
ssh-add ~/.ssh/id_rsa
ssh-add -l              # listar chaves no agente
```

#### Configuração do SSH

```bash
# Arquivo de configuração do servidor SSH
/etc/ssh/sshd_config

# Configurações importantes de segurança:
# PermitRootLogin no          # desabilitar login root direto
# PasswordAuthentication no   # desabilitar login por senha
# PubkeyAuthentication yes    # habilitar login por chave

# Reiniciar após mudanças
systemctl restart sshd

# Arquivo de configuração do cliente
~/.ssh/config
/etc/ssh/ssh_config

# Exemplo de ~/.ssh/config:
# Host servidor_prod
#     HostName 192.168.1.100
#     User deploy
#     Port 2222
#     IdentityFile ~/.ssh/id_ed25519
```

---

## 📌 Pontos-Chave para o Exame

- `find / -perm -4000` localiza arquivos com SUID (potencial risco de segurança)
- `chage` gerencia políticas de expiração de senhas; `passwd -l` bloqueia uma conta
- TCP Wrappers: `/etc/hosts.allow` é verificado primeiro, depois `/etc/hosts.deny`
- `iptables -L` lista regras de firewall; `-A INPUT` adiciona regra de entrada
- GPG: `--encrypt` criptografa, `--decrypt` descriptografa, `--sign` assina, `--verify` verifica
- `ssh-keygen` gera par de chaves; `ssh-copy-id` instala a chave pública no servidor
- A chave privada SSH nunca deve ser compartilhada; a pública vai em `authorized_keys`
- `/etc/ssh/sshd_config`: `PermitRootLogin no` e `PasswordAuthentication no` aumentam a segurança

---

## 🎉 Parabéns!

Você concluiu todo o conteúdo do LPIC-1! 

**Próximos passos:**
1. Revise os pontos-chave de cada tópico
2. Pratique todos os exercícios em um sistema Linux real
3. Faça simulados de exame (veja os [recursos adicionais](../../recursos/links-uteis.md))
4. Agende seu exame no [site do LPI](https://www.lpi.org/pt-br/)
