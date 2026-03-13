# Exercícios — Tópico 102: Instalação e Gerenciamento de Pacotes

## Exercícios Práticos

### 1. Gerenciamento de Pacotes Debian (apt/dpkg)

```bash
# a) Atualize a lista de pacotes
sudo apt update

# b) Instale o editor de texto 'nano' (se ainda não estiver instalado)
sudo apt install nano

# c) Veja as informações do pacote 'nano'
apt show nano

# d) Descubra qual pacote instalou o arquivo /bin/bash
dpkg -S /bin/bash

# e) Liste todos os arquivos instalados pelo pacote 'bash'
dpkg -L bash

# f) Liste todos os pacotes instalados no sistema
dpkg -l | head -20
```

---

### 2. Explorando Bibliotecas Compartilhadas

```bash
# a) Veja as bibliotecas usadas pelo comando 'ls'
ldd /bin/ls

# b) Veja as bibliotecas usadas pelo bash
ldd /bin/bash

# c) Verifique onde estão os arquivos de configuração de biblioteca
cat /etc/ld.so.conf
ls /etc/ld.so.conf.d/
```

---

### 3. Explorando Partições

```bash
# a) Liste todas as partições do sistema
lsblk

# b) Veja detalhes das partições com tamanhos
lsblk -f

# c) Exiba o uso do disco em formato legível
df -h
```

---

## Questões de Múltipla Escolha (Estilo LPIC-1)

**1.** Qual comando `dpkg` é usado para descobrir qual pacote instalou o arquivo `/usr/bin/vim`?

- A) `dpkg -l vim`
- B) `dpkg -L vim`
- C) `dpkg -S /usr/bin/vim`
- D) `dpkg -i /usr/bin/vim`

---

**2.** No formato de empacotamento RPM, qual opção do comando `rpm` lista todos os arquivos instalados por um pacote?

- A) `rpm -qi nome_pacote`
- B) `rpm -ql nome_pacote`
- C) `rpm -qf nome_pacote`
- D) `rpm -qa nome_pacote`

---

**3.** Qual arquivo contém a lista de repositórios de software no Debian/Ubuntu?

- A) `/etc/apt/preferences`
- B) `/etc/apt/sources.list`
- C) `/etc/dpkg/sources.conf`
- D) `/etc/apt/apt.conf`

---

**4.** Qual comando atualiza o cache de bibliotecas compartilhadas após adicionar uma nova biblioteca ao sistema?

- A) `ldd -update`
- B) `libcache -r`
- C) `ldconfig`
- D) `modprobe`

---

**5.** Uma das diferenças entre MBR e GPT é que:

- A) MBR suporta até 4 TB e GPT até 2 TB
- B) GPT suporta mais de 128 partições e MBR no máximo 4 partições primárias
- C) MBR é mais moderno que GPT
- D) GPT só funciona em sistemas de 32 bits

---

**6.** Qual opção do comando `apt` remove um pacote E seus arquivos de configuração?

- A) `apt remove`
- B) `apt delete`
- C) `apt purge`
- D) `apt clean`

---

**7.** Qual comando verifica se o sistema está rodando dentro de uma máquina virtual?

- A) `lsvm`
- B) `systemd-detect-virt`
- C) `virt-check`
- D) `uname -v`

---

## Respostas

| Questão | Resposta | Explicação |
|---------|----------|------------|
| 1 | **C** | `dpkg -S arquivo` procura qual pacote instalou determinado arquivo. |
| 2 | **B** | `rpm -ql pacote` lista os arquivos do pacote (QL = Query, List files). |
| 3 | **B** | `/etc/apt/sources.list` (e arquivos em `.d/`) define os repositórios. |
| 4 | **C** | `ldconfig` atualiza o cache `/etc/ld.so.cache` após adicionar bibliotecas. |
| 5 | **B** | GPT suporta até 128 partições; MBR suporta no máximo 4 primárias (ou 3 primárias + 1 estendida). |
| 6 | **C** | `apt purge` remove o pacote e seus arquivos de configuração. `apt remove` mantém as configurações. |
| 7 | **B** | `systemd-detect-virt` detecta o tipo de virtualização em uso. |
