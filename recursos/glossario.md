# 📖 Glossário de Termos Linux

## A

**alias** — Atalho de linha de comando que substitui um comando por outro. Ex: `alias ll='ls -la'`

**APT** (Advanced Package Tool) — Sistema de gerenciamento de pacotes usado no Debian/Ubuntu. Inclui `apt`, `apt-get`, `apt-cache`.

**arquivo de dispositivo** — Arquivo especial em `/dev` que representa um hardware ou interface. Ex: `/dev/sda` para disco, `/dev/tty` para terminal.

---

## B

**bash** — Bourne Again Shell. Shell padrão na maioria das distribuições Linux.

**bootloader** — Programa que inicia o kernel Linux após o BIOS/UEFI. Ex: GRUB2.

---

## C

**cron** — Serviço que agenda tarefas para execução em intervalos regulares.

**crontab** — Arquivo de configuração do cron. Editado com `crontab -e`.

**CLI** (Command Line Interface) — Interface de linha de comando.

---

## D

**daemon** — Processo que roda em segundo plano sem interação do usuário. Ex: `sshd`, `httpd`.

**DHCP** (Dynamic Host Configuration Protocol) — Protocolo que atribui endereços IP automaticamente.

**DNS** (Domain Name System) — Sistema que traduz nomes de domínio em endereços IP.

**dpkg** — Gerenciador de pacotes de baixo nível do Debian/Ubuntu.

---

## E

**ext4** — Sistema de arquivos mais usado no Linux (quarta versão do Extended Filesystem).

---

## F

**FHS** (Filesystem Hierarchy Standard) — Padrão que define a estrutura de diretórios no Linux.

**firewall** — Barreira de segurança que controla o tráfego de rede. No Linux, usa-se iptables ou nftables.

---

## G

**GPG** (GNU Privacy Guard) — Software para criptografia e assinatura digital.

**GRUB** (Grand Unified Bootloader) — Bootloader padrão na maioria das distribuições Linux.

**grupo** — Conjunto de usuários com permissões compartilhadas. Gerenciado via `groupadd`, `groupmod`, `gpasswd`.

---

## H

**hard link** — Referência direta a um inode (arquivo). Múltiplos nomes para o mesmo arquivo.

**home directory** — Diretório pessoal do usuário, geralmente em `/home/usuario`.

---

## I

**inode** — Estrutura de dados que armazena metadados de um arquivo (permissões, dono, tamanho, localização). Não contém o nome do arquivo.

**initramfs / initrd** — Sistema de arquivos temporário carregado na memória durante o boot para montar o filesystem raiz.

**iptables** — Ferramenta de firewall do Linux para controlar tráfego de rede.

---

## J

**journald** — Serviço de logging do systemd. Logs acessados via `journalctl`.

---

## K

**kernel** — Núcleo do sistema operacional Linux. Gerencia hardware, processos e recursos.

---

## L

**link simbólico** (symlink / soft link) — Arquivo especial que aponta para outro arquivo ou diretório.

**locale** — Configuração regional do sistema (idioma, formato de data, moeda).

**log** — Registro de eventos do sistema, geralmente em `/var/log/`.

---

## M

**man** — Manual do sistema. `man ls` mostra o manual do comando `ls`.

**MBR** (Master Boot Record) — Tipo de tabela de partição legada. Suporta até 2 TB e 4 partições primárias.

**mount** — Montar um filesystem (torná-lo acessível em um ponto de montagem).

**MTA** (Mail Transfer Agent) — Software que envia/recebe e-mails. Ex: Postfix, Sendmail.

---

## N

**NFS** (Network File System) — Protocolo para compartilhar sistemas de arquivos via rede.

**NTP** (Network Time Protocol) — Protocolo de sincronização de hora via rede.

---

## P

**PATH** — Variável de ambiente que contém os diretórios onde o shell procura por executáveis.

**permissão** — Controle de acesso a arquivos e diretórios: leitura (r), escrita (w), execução (x).

**PID** (Process ID) — Número único que identifica um processo em execução.

**pipe** — Mecanismo que conecta a saída de um comando à entrada de outro. Símbolo: `|`

---

## R

**redirecionamento** — Redirecionar a entrada/saída de comandos: `>` (stdout), `>>` (append), `<` (stdin), `2>` (stderr).

**root** — Superusuário com UID 0. Tem permissão total no sistema.

**rpm** — Formato de pacote do Red Hat e gerenciador de pacotes de baixo nível.

**runlevel** — Modo de operação do sistema (SysVinit). Substituído por "targets" no systemd.

---

## S

**SGID** (Set Group ID) — Permissão especial que faz executáveis rodarem com o GID do grupo dono.

**shell** — Interpretador de comandos. Exemplos: bash, zsh, sh, fish.

**SUID** (Set User ID) — Permissão especial que faz executáveis rodarem com o UID do dono do arquivo.

**SSH** (Secure Shell) — Protocolo de acesso remoto seguro via criptografia.

**stderr** — Saída de erros padrão (stream 2).

**stdin** — Entrada padrão (stream 0).

**stdout** — Saída padrão (stream 1).

**sticky bit** — Permissão especial em diretórios que permite apenas ao dono apagar seus arquivos.

**swap** — Área de disco usada como extensão da memória RAM.

**symlink** — Ver *link simbólico*.

**systemd** — Sistema de init moderno usado na maioria das distribuições Linux atuais.

---

## T

**target** — Modo de operação no systemd (equivalente ao runlevel). Ex: `multi-user.target`, `graphical.target`.

**TCP** (Transmission Control Protocol) — Protocolo de transporte orientado a conexão e confiável.

**timezone** — Fuso horário configurado no sistema.

---

## U

**UDP** (User Datagram Protocol) — Protocolo de transporte sem conexão e sem garantia de entrega.

**UID** (User ID) — Número único que identifica um usuário no sistema.

**umask** — Máscara que define as permissões padrão de novos arquivos e diretórios.

---

## V

**variável de ambiente** — Variável disponível para processos do sistema. Ex: `$HOME`, `$PATH`, `$USER`.

**virtual filesystem** — Sistema de arquivos que não existe em disco, apenas em memória. Ex: `/proc`, `/sys`, `/dev`.

---

## W

**Wayland** — Protocolo moderno de servidor gráfico, substituto do X11.

---

## X

**X11 / X Window System** — Sistema de janelas gráfico tradicional do Linux.

**XFS** — Sistema de arquivos de alta performance, padrão no RHEL/CentOS.

---

## Y

**yum / dnf** — Gerenciador de pacotes de alto nível para sistemas Red Hat/CentOS/Fedora.

---

## Z

**zombie** — Processo que terminou mas cujo registro ainda existe na tabela de processos porque o processo pai não coletou seu status.
