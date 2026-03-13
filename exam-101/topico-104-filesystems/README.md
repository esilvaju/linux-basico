# Tópico 104 — Dispositivos, Linux Filesystems e FHS

## Objetivos (LPIC-1)

- **104.1** Criar partições e sistemas de arquivos
- **104.2** Manter a integridade dos sistemas de arquivos
- **104.3** Controlar montagem e desmontagem de sistemas de arquivos
- **104.5** Gerenciar permissões e propriedade de arquivos
- **104.6** Criar e alterar links físicos e simbólicos
- **104.7** Localizar arquivos do sistema e colocar arquivos nos locais corretos (FHS)

---

## 104.1 — Criar Partições e Sistemas de Arquivos

### Sistemas de Arquivos Comuns

| Filesystem | Descrição |
|------------|-----------|
| ext4 | Padrão no Linux moderno (Debian, Ubuntu) |
| xfs | Alta performance, padrão no RHEL/CentOS 7+ |
| btrfs | Moderno, com snapshots e RAID integrado |
| vfat/FAT32 | Compatível com Windows; usado em /boot/efi |
| exFAT | Para dispositivos de armazenamento portáteis |
| swap | Memória virtual em disco |

### Criar e Formatar Partições

```bash
# Criar partições com fdisk
fdisk /dev/sdb
# Comandos interativos: n (nova), p (primária), w (gravar)

# Criar filesystem ext4
mkfs.ext4 /dev/sdb1
mkfs -t ext4 /dev/sdb1   # equivalente

# Criar filesystem xfs
mkfs.xfs /dev/sdb1

# Criar filesystem vfat
mkfs.vfat /dev/sdb1

# Criar área de swap
mkswap /dev/sdb2
swapon /dev/sdb2          # ativar
swapoff /dev/sdb2         # desativar

# Ver uso da swap
swapon --show
free -h
```

---

## 104.2 — Manutenção e Integridade dos Filesystems

```bash
# Verificar e reparar filesystem ext2/ext3/ext4 (requer desmontagem)
fsck /dev/sdb1
fsck.ext4 /dev/sdb1
e2fsck -f /dev/sdb1     # forçar verificação completa

# Verificar filesystem xfs (desmontado)
xfs_repair /dev/sdb1

# Verificar uso do disco
df -h                   # uso por sistema de arquivos
df -i                   # uso de inodes
du -sh /home/usuario    # tamanho de um diretório
du -sh /*               # tamanho de cada pasta raiz

# Informações sobre filesystem ext
tune2fs -l /dev/sda1    # exibir informações do superbloco
dumpe2fs /dev/sda1      # informações detalhadas

# Ajustar parâmetros do filesystem ext
tune2fs -c 20 /dev/sda1       # verificar a cada 20 montagens
tune2fs -i 1m /dev/sda1       # verificar a cada 1 mês
tune2fs -L "MINHA_PARTICAO" /dev/sda1   # definir label
```

---

## 104.3 — Montagem e Desmontagem de Filesystems

```bash
# Montar um filesystem
mount /dev/sdb1 /mnt/dados
mount -t ext4 /dev/sdb1 /mnt/dados   # especificando o tipo

# Montar com opções
mount -o ro /dev/sdb1 /mnt/dados         # somente leitura
mount -o remount,rw /dev/sdb1            # remontar com leitura/escrita

# Desmontar
umount /mnt/dados
umount /dev/sdb1

# Listar filesystems montados
mount
mount | grep sdb
cat /proc/mounts

# Arquivo /etc/fstab — montagem automática no boot
cat /etc/fstab
```

### Formato do /etc/fstab

```
# dispositivo   ponto_montagem  tipo    opções          dump  pass
/dev/sda1       /               ext4    defaults         0     1
/dev/sda2       /boot           ext4    defaults         0     2
/dev/sda3       swap            swap    sw               0     0
UUID=abc123     /home           ext4    defaults,noatime 0     2
```

Opções comuns do fstab:

| Opção | Descrição |
|-------|-----------|
| `defaults` | Opções padrão (rw, suid, dev, exec, auto, nouser, async) |
| `ro` / `rw` | Somente leitura / leitura e escrita |
| `noatime` | Não atualiza tempo de acesso (melhora performance) |
| `auto` / `noauto` | Montar/não montar automaticamente |
| `user` / `nouser` | Permite/proíbe usuário comum montar |
| `exec` / `noexec` | Permite/proíbe execução de programas |

```bash
# Montar tudo do /etc/fstab
mount -a

# Ver UUIDs dos discos
blkid
lsblk -f
```

---

## 104.5 — Permissões e Propriedade de Arquivos

### Permissões Básicas

```
-rwxr-xr--   1  usuario  grupo  1234  jan 01 12:00  arquivo.txt
│└─┬──┘└─┬──┘└─┬──┘
│  │     │     └── Outros (r=4, x=1)
│  │     └──────── Grupo (r=4, x=1)
│  └────────────── Dono (r=4, w=2, x=1)
└───────────────── Tipo: - arquivo, d diretório, l link
```

| Permissão | Valor Octal | Arquivo | Diretório |
|-----------|-------------|---------|-----------|
| r (read) | 4 | Ler conteúdo | Listar conteúdo |
| w (write) | 2 | Modificar/deletar | Criar/remover arquivos |
| x (execute) | 1 | Executar | Entrar no diretório (`cd`) |

```bash
# Alterar permissões
chmod 755 arquivo        # rwxr-xr-x (octal)
chmod u+x arquivo        # adiciona executável ao dono
chmod g-w arquivo        # remove escrita do grupo
chmod o=r arquivo        # define outros como somente leitura
chmod a+r arquivo        # adiciona leitura para todos (all)
chmod -R 644 pasta/      # recursivo

# Alterar proprietário
chown usuario arquivo
chown usuario:grupo arquivo
chown -R usuario:grupo pasta/

# Alterar grupo
chgrp grupo arquivo
chgrp -R grupo pasta/

# Ver permissões
ls -la
stat arquivo.txt
```

### Permissões Especiais

| Permissão | Octal | Efeito em Arquivo | Efeito em Diretório |
|-----------|-------|-------------------|---------------------|
| SUID | 4000 | Executa com permissões do dono | — |
| SGID | 2000 | Executa com permissões do grupo | Novos arquivos herdam o grupo |
| Sticky bit | 1000 | — | Só o dono pode apagar seus arquivos |

```bash
# Definir SUID
chmod u+s arquivo
chmod 4755 arquivo

# Definir SGID
chmod g+s diretorio
chmod 2755 diretorio

# Sticky bit (comum em /tmp)
chmod +t /tmp
chmod 1777 /tmp

# Verificar
ls -la /tmp      # verifica o 't' ao final das permissões
ls -la /usr/bin/passwd  # verifica o 's' no lugar do 'x' do dono
```

### umask — Máscara Padrão de Permissões

```bash
# Ver a umask atual
umask

# Alterar a umask (temporariamente)
umask 022      # arquivos criados com 644, dirs com 755
umask 027      # arquivos criados com 640, dirs com 750
```

---

## 104.6 — Links Físicos e Simbólicos

```bash
# Link físico (hard link) — aponta para o mesmo inode
ln arquivo.txt link_fisico.txt

# Link simbólico (soft link / symlink) — aponta para o caminho
ln -s arquivo.txt link_simbolico.txt
ln -s /caminho/absoluto /outro/local/link

# Ver links
ls -la           # links simbólicos mostram o destino com '->'
stat arquivo.txt # mostra o número de hard links

# Diferenças entre hard link e soft link:
# Hard link: mesmo inode, não funciona entre filesystems diferentes
# Soft link: arquivo separado, funciona entre filesystems, pode apontar para diretórios
```

---

## 104.7 — FHS (Filesystem Hierarchy Standard)

O **FHS** define a estrutura padrão de diretórios no Linux:

| Diretório | Descrição |
|-----------|-----------|
| `/` | Raiz do sistema |
| `/bin` | Executáveis essenciais para todos os usuários |
| `/sbin` | Executáveis do sistema (para root) |
| `/boot` | Arquivos de boot (kernel, initramfs, GRUB) |
| `/dev` | Arquivos de dispositivos |
| `/etc` | Arquivos de configuração |
| `/home` | Diretórios home dos usuários |
| `/lib` / `/lib64` | Bibliotecas essenciais |
| `/media` | Pontos de montagem para mídias removíveis |
| `/mnt` | Pontos de montagem temporários |
| `/opt` | Softwares opcionais de terceiros |
| `/proc` | Filesystem virtual — informações do kernel |
| `/root` | Home do usuário root |
| `/run` | Dados de runtime (PIDs, sockets) |
| `/srv` | Dados de serviços (web, ftp) |
| `/sys` | Filesystem virtual — informações de hardware |
| `/tmp` | Arquivos temporários (apagados no boot) |
| `/usr` | Hierarquia secundária (programas, bibliotecas) |
| `/usr/bin` | Executáveis dos usuários (não essenciais) |
| `/usr/local` | Softwares instalados localmente |
| `/var` | Dados variáveis (logs, spool, banco de dados) |
| `/var/log` | Arquivos de log |

```bash
# Comandos para localizar arquivos no sistema
which ls          # localiza o executável no PATH
whereis bash      # localiza binário, fonte e manual
locate arquivo    # busca em banco de dados (rápido)
updatedb          # atualiza o banco de dados do locate
find / -name "arquivo" 2>/dev/null   # busca em tempo real
```

---

## 📌 Pontos-Chave para o Exame

- `mkfs.ext4`, `mkfs.xfs`, `mkswap` — criação de filesystems
- `fsck` verifica integridade; **requer filesystem desmontado**
- `/etc/fstab` controla montagem automática no boot; `mount -a` aplica
- Permissões octal: r=4, w=2, x=1. Exemplo: 755 = rwxr-xr-x
- `chmod`, `chown`, `chgrp` — alteram permissões e propriedade
- SUID (4), SGID (2), Sticky (1) — permissões especiais
- Hard link: mesmo inode, mesmo filesystem. Soft link: pode cruzar filesystems, pode apontar para diretórios
- FHS: `/etc` = configurações, `/var/log` = logs, `/tmp` = temporários

---

## ▶️ Próximo Exame

[Exame 102 — Visão Geral](../../exam-102/README.md)
