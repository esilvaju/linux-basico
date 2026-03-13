# Tópico 102 — Instalação e Gerenciamento de Pacotes

## Objetivos (LPIC-1)

- **102.1** Design do layout do disco rígido
- **102.2** Instalar um bootloader
- **102.3** Gerenciar bibliotecas compartilhadas
- **102.4** Usar o gerenciamento de pacotes Debian (dpkg/apt)
- **102.5** Usar o gerenciamento de pacotes RPM e YUM/DNF
- **102.6** Linux como convidado de virtualização

---

## 102.1 — Layout do Disco

### Partições Essenciais

| Partição | Ponto de Montagem | Descrição |
|----------|-------------------|-----------|
| Raiz | `/` | Sistema base (obrigatório) |
| Swap | — | Memória virtual em disco |
| Boot | `/boot` | Kernel e arquivos de boot (recomendado) |
| Home | `/home` | Dados dos usuários |
| Var | `/var` | Logs, spool, dados variáveis |
| Tmp | `/tmp` | Arquivos temporários |

### Tipos de Tabela de Partição

- **MBR (Master Boot Record):** Legado, suporta discos até 2 TB, máximo de 4 partições primárias
- **GPT (GUID Partition Table):** Moderno, suporta discos maiores, até 128 partições

### Ferramentas de Particionamento

```bash
# fdisk — para discos MBR (e GPT em versões recentes)
fdisk /dev/sda

# gdisk — para discos GPT
gdisk /dev/sda

# parted — ferramenta moderna (suporta MBR e GPT)
parted /dev/sda

# Listar partições
fdisk -l
lsblk
```

---

## 102.2 — Instalar um Bootloader (GRUB2)

```bash
# Instalar o GRUB no disco principal
grub-install /dev/sda           # Debian/Ubuntu
grub2-install /dev/sda          # RHEL/CentOS

# Gerar o arquivo de configuração
update-grub                     # Debian/Ubuntu
grub2-mkconfig -o /boot/grub2/grub.cfg  # RHEL/CentOS

# Editar configurações do GRUB
nano /etc/default/grub
```

---

## 102.3 — Bibliotecas Compartilhadas

No Linux, programas compartilham bibliotecas (`.so` = shared object) para economizar memória e disco.

```bash
# Listar as bibliotecas que um programa usa
ldd /bin/ls
ldd /usr/bin/python3

# Atualizar o cache de bibliotecas compartilhadas
ldconfig

# Arquivo de configuração de caminhos de bibliotecas
cat /etc/ld.so.conf
ls /etc/ld.so.conf.d/

# Variável de ambiente para caminhos adicionais de bibliotecas
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
```

---

## 102.4 — Gerenciamento de Pacotes Debian (dpkg / apt)

### dpkg — Gerenciador de Pacotes de Baixo Nível

```bash
# Instalar um pacote .deb local
dpkg -i pacote.deb

# Remover um pacote (mantém configurações)
dpkg -r nome_pacote

# Remover pacote e suas configurações
dpkg -P nome_pacote

# Listar pacotes instalados
dpkg -l
dpkg -l | grep nome_pacote

# Ver arquivos instalados por um pacote
dpkg -L nome_pacote

# Descobrir qual pacote instalou determinado arquivo
dpkg -S /usr/bin/vim

# Ver informações de um pacote instalado
dpkg -s nome_pacote

# Verificar integridade dos arquivos de um pacote
dpkg -V nome_pacote
```

### apt / apt-get — Gerenciador de Alto Nível

```bash
# Atualizar a lista de pacotes disponíveis
apt update

# Instalar um pacote
apt install nome_pacote

# Remover um pacote
apt remove nome_pacote

# Remover pacote e configurações
apt purge nome_pacote

# Atualizar todos os pacotes instalados
apt upgrade

# Atualização completa (pode remover pacotes)
apt full-upgrade

# Remover pacotes desnecessários
apt autoremove

# Buscar um pacote
apt search palavra_chave

# Exibir informações sobre um pacote
apt show nome_pacote

# Limpar o cache de pacotes baixados
apt clean
```

### Repositórios

```bash
# Arquivo de repositórios (Debian/Ubuntu)
/etc/apt/sources.list
/etc/apt/sources.list.d/

# Exemplo de linha no sources.list:
# deb http://archive.ubuntu.com/ubuntu focal main restricted universe
```

---

## 102.5 — Gerenciamento de Pacotes RPM / YUM / DNF

### rpm — Gerenciador de Pacotes de Baixo Nível (Red Hat)

```bash
# Instalar um pacote .rpm local
rpm -ivh pacote.rpm

# Atualizar um pacote
rpm -Uvh pacote.rpm

# Remover um pacote
rpm -e nome_pacote

# Listar todos os pacotes instalados
rpm -qa

# Ver arquivos instalados por um pacote
rpm -ql nome_pacote

# Descobrir qual pacote instalou um arquivo
rpm -qf /usr/bin/vim

# Ver informações de um pacote
rpm -qi nome_pacote

# Verificar integridade dos arquivos
rpm -V nome_pacote
```

### yum / dnf — Gerenciadores de Alto Nível (Red Hat / Fedora)

```bash
# Instalar um pacote
yum install nome_pacote
dnf install nome_pacote

# Remover um pacote
yum remove nome_pacote
dnf remove nome_pacote

# Atualizar todos os pacotes
yum update
dnf update

# Buscar um pacote
yum search palavra_chave
dnf search palavra_chave

# Exibir informações sobre um pacote
yum info nome_pacote
dnf info nome_pacote

# Listar pacotes instalados
yum list installed
dnf list installed

# Repositórios do yum/dnf
/etc/yum.repos.d/
```

---

## 102.6 — Linux como Convidado de Virtualização

O Linux pode ser executado como sistema operacional convidado em diversas plataformas de virtualização:

| Plataforma | Tipo | Descrição |
|------------|------|-----------|
| VirtualBox | Tipo 2 | Hypervisor para desktop, gratuito |
| VMware | Tipo 1/2 | Solução corporativa |
| KVM | Tipo 1 | Embutido no kernel Linux |
| Xen | Tipo 1 | Usado em servidores e nuvem |
| Docker | Contêiner | Não é VM, compartilha o kernel |

```bash
# Verificar se o sistema está rodando em uma VM
systemd-detect-virt

# Ver informações sobre a plataforma de virtualização
cat /proc/cpuinfo | grep hypervisor
```

---

## 📌 Pontos-Chave para o Exame

- MBR suporta até 2 TB e 4 partições primárias; GPT não tem essas limitações
- `dpkg` é o gerenciador de baixo nível no Debian; `apt` é o de alto nível
- `rpm` é o gerenciador de baixo nível no Red Hat; `yum`/`dnf` é o de alto nível
- `dpkg -S arquivo` e `rpm -qf arquivo` descobrem qual pacote instalou um arquivo
- `ldd` lista as bibliotecas compartilhadas de um executável
- `ldconfig` atualiza o cache de bibliotecas (`/etc/ld.so.cache`)

---

## ▶️ Próximo Tópico

[Tópico 103 — Comandos GNU e Unix](../topico-103-comandos/README.md)
