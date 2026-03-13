# Exercícios — Tópico 101: Arquitetura do Sistema

## Exercícios Práticos

### 1. Explorando o Hardware

No terminal, execute os comandos abaixo e observe a saída:

```bash
# a) Liste todos os dispositivos PCI do seu sistema
lspci

# b) Liste os dispositivos USB conectados
lsusb

# c) Exiba os módulos do kernel carregados atualmente
lsmod

# d) Visualize informações sobre a CPU
cat /proc/cpuinfo | grep "model name" | head -1

# e) Verifique a quantidade de memória RAM disponível
cat /proc/meminfo | grep MemTotal
```

**Pergunta:** Quantas CPUs lógicas seu sistema possui? (Dica: `cat /proc/cpuinfo | grep processor | wc -l`)

---

### 2. Explorando o Boot

```bash
# a) Exiba os parâmetros com que o kernel foi iniciado
cat /proc/cmdline

# b) Visualize as mensagens de boot do kernel
dmesg | head -30

# c) Verifique o arquivo de configuração do GRUB
cat /etc/default/grub
```

---

### 3. Trabalhando com systemd

```bash
# a) Descubra qual é o target de boot padrão do seu sistema
systemctl get-default

# b) Liste todos os serviços ativos
systemctl list-units --type=service --state=active

# c) Verifique o status do serviço de rede
systemctl status NetworkManager
# ou
systemctl status networking
```

---

## Questões de Múltipla Escolha (Estilo LPIC-1)

**1.** Qual arquivo virtual contém informações sobre os módulos do kernel atualmente carregados?

- A) `/sys/modules`
- B) `/proc/modules`
- C) `/dev/modules`
- D) `/boot/modules`

---

**2.** Qual comando é usado para carregar um módulo do kernel dinamicamente?

- A) `insmod`
- B) `rmmod`
- C) `modprobe`
- D) `lsmod`

---

**3.** Em um sistema com systemd, qual comando define o target de boot padrão como multi-user (sem interface gráfica)?

- A) `init 3`
- B) `systemctl default multi-user.target`
- C) `systemctl set-default multi-user.target`
- D) `telinit 3`

---

**4.** Qual é a sequência correta do processo de boot em um sistema Linux moderno?

- A) Kernel → BIOS → GRUB → systemd
- B) BIOS → GRUB → Kernel → initramfs → systemd
- C) GRUB → BIOS → initramfs → Kernel → systemd
- D) BIOS → Kernel → GRUB → systemd

---

**5.** Qual arquivo você deve editar para alterar os parâmetros padrão do GRUB2?

- A) `/boot/grub2/grub.cfg`
- B) `/etc/grub.conf`
- C) `/etc/default/grub`
- D) `/boot/grub/menu.lst`

---

**6.** Qual runlevel SysVinit corresponde ao modo "desligar o sistema"?

- A) Runlevel 1
- B) Runlevel 0
- C) Runlevel 6
- D) Runlevel 3

---

**7.** O diretório `/dev` contém:

- A) Arquivos de configuração do sistema
- B) Logs do sistema
- C) Arquivos de dispositivos (disco, terminais, etc.)
- D) Módulos do kernel

---

## Respostas

| Questão | Resposta | Explicação |
|---------|----------|------------|
| 1 | **B** | `/proc/modules` lista os módulos carregados. O comando `lsmod` lê esse arquivo. |
| 2 | **C** | `modprobe` carrega módulos gerenciando dependências automaticamente. `insmod` também carrega, mas sem resolver dependências. |
| 3 | **C** | `systemctl set-default multi-user.target` define o target padrão. |
| 4 | **B** | A sequência correta é BIOS/UEFI → Bootloader (GRUB) → Kernel → initramfs → systemd. |
| 5 | **C** | `/etc/default/grub` é o arquivo editável. O `grub.cfg` é gerado automaticamente. |
| 6 | **B** | Runlevel 0 = desligar; Runlevel 6 = reiniciar. |
| 7 | **C** | `/dev` contém arquivos de dispositivos criados pelo udev. |
