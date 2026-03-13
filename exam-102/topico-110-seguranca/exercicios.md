# Exercícios — Tópico 110: Segurança

## Exercícios Práticos

### 1. Verificações de Segurança

```bash
# a) Encontre todos os arquivos com SUID no sistema
sudo find / -perm -4000 -type f 2>/dev/null

# b) Veja as tentativas de login malsucedidas
sudo lastb | head -20

# c) Veja os usuários atualmente logados e o que estão fazendo
w

# d) Verifique a política de senha do seu usuário
chage -l $USER
```

---

### 2. Chaves SSH

```bash
# a) Gere um par de chaves SSH Ed25519 (se ainda não tiver)
ssh-keygen -t ed25519 -C "meu_email@exemplo.com"

# b) Liste as chaves na pasta ~/.ssh
ls -la ~/.ssh/

# c) Veja o conteúdo da chave pública gerada
cat ~/.ssh/id_ed25519.pub
```

---

### 3. GPG

```bash
# a) Liste as chaves GPG disponíveis
gpg --list-keys

# b) (Opcional) Gere um novo par de chaves GPG
gpg --full-generate-key

# c) Crie um arquivo de teste e assine-o
echo "Conteúdo de teste" > /tmp/teste_gpg.txt
gpg --clearsign /tmp/teste_gpg.txt
cat /tmp/teste_gpg.txt.asc
```

---

## Questões de Múltipla Escolha (Estilo LPIC-1)

**1.** Qual comando define que a senha do usuário `joao` deve expirar em 60 dias?

- A) `passwd -e 60 joao`
- B) `chage -M 60 joao`
- C) `usermod --maxdays 60 joao`
- D) `chage -d 60 joao`

---

**2.** Em qual arquivo são definidos os **hosts permitidos** para acessar serviços via TCP Wrappers?

- A) `/etc/hosts.deny`
- B) `/etc/tcp.allow`
- C) `/etc/hosts.allow`
- D) `/etc/security/hosts`

---

**3.** Qual comando GPG é usado para **criptografar** um arquivo para o destinatário `alice@exemplo.com`?

- A) `gpg --sign -r alice@exemplo.com arquivo.txt`
- B) `gpg --encrypt -r alice@exemplo.com arquivo.txt`
- C) `gpg --cipher -r alice@exemplo.com arquivo.txt`
- D) `gpg --protect arquivo.txt alice@exemplo.com`

---

**4.** Onde deve ser colocada a **chave pública SSH** para permitir acesso sem senha ao servidor?

- A) `~/.ssh/id_rsa.pub` no servidor
- B) `~/.ssh/known_hosts` no servidor
- C) `~/.ssh/authorized_keys` no servidor
- D) `/etc/ssh/authorized_keys`

---

**5.** Qual configuração no `/etc/ssh/sshd_config` desabilita o login direto do root via SSH?

- A) `AllowRoot no`
- B) `RootAccess disabled`
- C) `PermitRootLogin no`
- D) `DenyRoot yes`

---

**6.** Qual comando lista os **processos que estão utilizando a porta 443**?

- A) `port 443`
- B) `ss -tlnp`
- C) `fuser 443/tcp`
- D) `netstat -find 443`

---

**7.** Qual tipo de chave SSH é considerado mais **seguro e moderno** que RSA?

- A) DSA
- B) Ed25519
- C) ECDSA de 256 bits
- D) RSA de 1024 bits

---

## Respostas

| Questão | Resposta | Explicação |
|---------|----------|------------|
| 1 | **B** | `chage -M 60 joao` define o número máximo de dias (-M = Maximum) antes da senha expirar. |
| 2 | **C** | `/etc/hosts.allow` é verificado primeiro. Se o host estiver listado, o acesso é permitido. |
| 3 | **B** | `gpg --encrypt -r destinatario arquivo` criptografa o arquivo. O `-r` (recipient) define o destinatário. |
| 4 | **C** | `~/.ssh/authorized_keys` no servidor contém as chaves públicas dos clientes autorizados. |
| 5 | **C** | `PermitRootLogin no` no `sshd_config` impede login SSH direto como root. |
| 6 | **C** | `fuser 443/tcp` mostra os PIDs dos processos usando a porta 443. `ss -tlnp` também funciona. |
| 7 | **B** | Ed25519 é baseado em criptografia de curva elíptica, mais rápido e seguro que RSA para tamanhos equivalentes. |
