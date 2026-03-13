# Tópico 101 — Arquitetura do Sistema

## Objetivos (LPIC-1)

- **101.1** Determinar e configurar definições de hardware
- **101.2** Inicializar o sistema (boot)
- **101.3** Alterar runlevels/boot targets e desligar ou reiniciar o sistema

---

## 101.1 — Determinar e Configurar Hardware

### Pseudossistemas de Arquivos

O Linux expõe informações de hardware através de sistemas de arquivos virtuais:

| Caminho | Descrição |
|---------|-----------|
| `/proc` | Informações sobre processos e hardware em tempo de execução |
| `/sys`  | Interface para o kernel expor informações de dispositivos |
| `/dev`  | Arquivos de dispositivos (discos, terminais, etc.) |

### Comandos Úteis

```bash
# Listar dispositivos PCI (placas de vídeo, rede, etc.)
lspci

# Listar dispositivos USB
lsusb

# Listar módulos do kernel carregados
lsmod

# Carregar um módulo do kernel
modprobe nome_do_modulo

# Remover um módulo do kernel
modprobe -r nome_do_modulo

# Exibir informações sobre um módulo
modinfo nome_do_modulo

# Verificar mensagens do kernel (útil para diagnóstico de hardware)
dmesg
dmesg | grep -i error
```

### Arquivos de Configuração Importantes

```bash
# Informações sobre a CPU
cat /proc/cpuinfo

# Informações sobre a memória RAM
cat /proc/meminfo

# Dispositivos de disco reconhecidos
cat /proc/partitions

# Interrupções de hardware (IRQs)
cat /proc/interrupts
```

### udev — Gerenciador de Dispositivos

O **udev** é responsável por criar dinamicamente os arquivos em `/dev` quando dispositivos são conectados.

```bash
# Listar todos os dispositivos de bloco (discos)
lsblk

# Listar dispositivos com informações detalhadas
udevadm info --query=all --name=/dev/sda
```

---

## 101.2 — Inicialização do Sistema (Boot)

### Sequência de Boot

```
1. BIOS/UEFI
      ↓
2. Bootloader (GRUB2)
      ↓
3. Kernel Linux
      ↓
4. initrd / initramfs
      ↓
5. Sistema de Init (systemd / SysVinit)
      ↓
6. Ambiente de usuário
```

### GRUB2 — Grand Unified Bootloader

O **GRUB2** é o bootloader padrão na maioria das distribuições modernas.

```bash
# Arquivo de configuração do GRUB2
/boot/grub2/grub.cfg        # gerado automaticamente
/etc/default/grub           # arquivo que você edita

# Regenerar o grub.cfg após editar /etc/default/grub
update-grub                 # Debian/Ubuntu
grub2-mkconfig -o /boot/grub2/grub.cfg  # RHEL/CentOS

# Instalar o GRUB no disco
grub-install /dev/sda
```

### Parâmetros do Kernel

Parâmetros passados ao kernel no momento do boot (configuráveis no GRUB):

```bash
# Ver parâmetros com que o kernel foi iniciado
cat /proc/cmdline
```

Exemplos de parâmetros comuns:
- `root=/dev/sda1` — define o dispositivo raiz
- `quiet` — reduz mensagens de boot
- `single` — inicia em modo single-user (manutenção)

### initrd / initramfs

O **initramfs** é um sistema de arquivos temporário carregado na memória durante o boot, usado para montar o filesystem raiz real.

```bash
# Ver o initramfs em uso
ls /boot/initrd*
ls /boot/initramfs*
```

---

## 101.3 — Runlevels e Boot Targets

### SysVinit — Runlevels Tradicionais

| Runlevel | Descrição |
|----------|-----------|
| 0 | Desligar |
| 1 | Single-user (manutenção) |
| 2 | Multi-user sem rede (Debian) |
| 3 | Multi-user com rede (modo texto) |
| 4 | Definido pelo usuário |
| 5 | Multi-user com interface gráfica |
| 6 | Reiniciar |

```bash
# Ver o runlevel atual
runlevel

# Mudar de runlevel
init 3
telinit 3
```

### systemd — Boot Targets

O **systemd** substituiu o SysVinit na maioria das distribuições modernas e usa **targets** em vez de runlevels:

| Target systemd | Equivalente SysVinit |
|----------------|----------------------|
| `poweroff.target` | Runlevel 0 |
| `rescue.target` | Runlevel 1 |
| `multi-user.target` | Runlevel 3 |
| `graphical.target` | Runlevel 5 |
| `reboot.target` | Runlevel 6 |

```bash
# Ver o target atual
systemctl get-default

# Mudar o target padrão
systemctl set-default multi-user.target

# Mudar o target imediatamente (sem reiniciar)
systemctl isolate multi-user.target

# Desligar o sistema
systemctl poweroff

# Reiniciar o sistema
systemctl reboot

# Suspender o sistema
systemctl suspend
```

### Comandos de Desligamento

```bash
# Desligar imediatamente
shutdown -h now

# Desligar em 10 minutos
shutdown -h +10

# Reiniciar imediatamente
shutdown -r now

# Cancelar um shutdown agendado
shutdown -c

# Equivalentes rápidos
halt       # desliga
poweroff   # desliga e corta energia
reboot     # reinicia
```

---

## 📌 Pontos-Chave para o Exame

- `/proc`, `/sys` e `/dev` são sistemas de arquivos virtuais (não existem em disco)
- `lspci`, `lsusb`, `lsmod`, `modprobe` são ferramentas para gerenciar hardware/módulos
- A sequência de boot: BIOS → GRUB2 → Kernel → initramfs → systemd
- O GRUB2 é configurado em `/etc/default/grub`; gerar a config com `update-grub` ou `grub2-mkconfig`
- systemd usa **targets** (não runlevels); o comando principal é `systemctl`
- `systemctl get-default` mostra o target padrão; `systemctl set-default` muda-o

---

## ▶️ Próximo Tópico

[Tópico 102 — Instalação e Gerenciamento de Pacotes](../topico-102-instalacao/README.md)
