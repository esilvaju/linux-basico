# Tópico 108 — Serviços Essenciais do Sistema

## Objetivos (LPIC-1)

- **108.1** Manter a hora do sistema
- **108.2** Logging do sistema
- **108.3** Fundamentos de MTA (Mail Transfer Agent)
- **108.4** Gerenciar impressoras e impressão

---

## 108.1 — Hora do Sistema

### Relógios no Linux

O Linux tem dois relógios:
- **Relógio de hardware (RTC):** Mantido pela placa-mãe, continua rodando com o computador desligado
- **Relógio do sistema:** Mantido pelo kernel em memória durante o funcionamento

```bash
# Ver data e hora atual
date
date +"%Y-%m-%d %H:%M:%S"    # formato personalizado

# Definir data e hora manualmente (requer root)
date -s "2025-01-15 10:30:00"

# Ver/sincronizar relógio de hardware
hwclock
hwclock --show                # exibir o relógio de hardware
hwclock --hctosys             # sincronizar hardware → sistema
hwclock --systohc             # sincronizar sistema → hardware
```

### NTP — Network Time Protocol

```bash
# Com systemd-timesyncd (padrão em sistemas modernos)
timedatectl status
timedatectl set-ntp true          # habilitar sincronização NTP

# Com ntpd (cliente NTP tradicional)
ntpq -p                          # ver servidores NTP configurados
ntpdate pool.ntp.org             # sincronizar manualmente

# Com chrony (recomendado em RHEL/CentOS modernos)
chronyc tracking                 # ver status da sincronização
chronyc sources                  # ver servidores NTP
```

---

## 108.2 — Logging do Sistema

### rsyslog / syslog

O **rsyslog** é o daemon de log padrão em muitas distribuições.

```bash
# Arquivo de configuração
/etc/rsyslog.conf
/etc/rsyslog.d/

# Restart do serviço de log
systemctl restart rsyslog
```

#### Facilidades (Facility) e Severidades (Severity)

**Facilidades:**

| Código | Facilidade | Descrição |
|--------|------------|-----------|
| kern | kernel | Mensagens do kernel |
| user | user | Mensagens de processos de usuário |
| mail | mail | Sistema de e-mail |
| daemon | daemon | Daemons de sistema |
| auth | auth/security | Autenticação |
| syslog | syslog | Mensagens internas do syslogd |
| cron | cron | Cron daemon |
| local0-7 | local | Uso local |

**Severidades (do mais grave ao menos grave):**

| Código | Nome | Descrição |
|--------|------|-----------|
| 0 | emerg | Sistema inutilizável |
| 1 | alert | Ação imediata necessária |
| 2 | crit | Condição crítica |
| 3 | err | Condição de erro |
| 4 | warning | Condição de aviso |
| 5 | notice | Condição normal mas significativa |
| 6 | info | Mensagem informativa |
| 7 | debug | Mensagem de depuração |

```bash
# Formato no rsyslog.conf:
# facilidade.severidade   destino
auth.info                 /var/log/auth.log
kern.err                  /var/log/kernel_errors.log
*.emerg                   :omusrmsg:*    # envia para todos os usuários
```

### Logs com journald (systemd)

```bash
# Ver logs do sistema (todos)
journalctl

# Ver logs do boot atual
journalctl -b

# Ver logs do boot anterior
journalctl -b -1

# Filtrar por serviço
journalctl -u nginx
journalctl -u sshd --since "1 hour ago"

# Acompanhar logs em tempo real
journalctl -f

# Ver logs de um período
journalctl --since "2025-01-01" --until "2025-01-02"

# Ver logs com nível de severidade
journalctl -p err        # apenas erros e acima

# Configuração do journald
/etc/systemd/journald.conf
```

### Arquivos de Log Comuns

| Arquivo | Conteúdo |
|---------|----------|
| `/var/log/syslog` | Log geral do sistema (Debian) |
| `/var/log/messages` | Log geral do sistema (RHEL) |
| `/var/log/auth.log` | Autenticação e segurança |
| `/var/log/kern.log` | Mensagens do kernel |
| `/var/log/dmesg` | Mensagens de boot do kernel |
| `/var/log/apt/` | Logs do gerenciador de pacotes apt |
| `/var/log/nginx/` | Logs do servidor web nginx |
| `/var/log/mysql/` | Logs do MySQL |

### logrotate — Rotação de Logs

```bash
# Configuração do logrotate
/etc/logrotate.conf
/etc/logrotate.d/

# Executar logrotate manualmente
logrotate -f /etc/logrotate.conf

# Exemplo de configuração em /etc/logrotate.d/meu_servico:
# /var/log/meu_servico.log {
#     daily
#     rotate 7
#     compress
#     missingok
#     notifempty
# }
```

---

## 108.3 — MTA (Mail Transfer Agent)

O **MTA** é responsável por enviar e receber e-mails entre servidores.

### MTAs Comuns

| MTA | Descrição |
|-----|-----------|
| Postfix | Moderno, seguro, padrão no Ubuntu/Debian |
| Sendmail | Clássico, configuração complexa |
| Exim | Flexível, padrão no Debian |
| qmail | Seguro, mas descontinuado |

### Comandos de E-mail

```bash
# Ver fila de e-mails
mailq
postqueue -p    # Postfix

# Enviar e-mail via linha de comando
echo "Corpo do e-mail" | mail -s "Assunto" destinatario@exemplo.com

# Ver e-mails recebidos (caixa de entrada local)
mail

# Aliases de e-mail
/etc/aliases
newaliases    # recarregar aliases após editar

# Ver e-mail local do usuário
cat /var/mail/$(whoami)
```

---

## 108.4 — Impressão (CUPS)

O **CUPS** (Common Unix Printing System) é o sistema de impressão padrão no Linux.

```bash
# Verificar o status do CUPS
systemctl status cups

# Interface web do CUPS
# Acesse: http://localhost:631

# Listar impressoras configuradas
lpstat -p
lpstat -a

# Enviar arquivo para impressão
lp arquivo.txt
lp -d nome_impressora arquivo.txt
lpr arquivo.txt                    # alternativo

# Listar trabalhos na fila de impressão
lpq
lpstat -o

# Cancelar trabalho de impressão
lprm job_id
cancel job_id

# Configuração do CUPS
/etc/cups/cupsd.conf
/etc/cups/printers.conf
```

---

## 📌 Pontos-Chave para o Exame

- `hwclock --hctosys` sincroniza do hardware para o sistema; `--systohc` o contrário
- `timedatectl set-ntp true` habilita sincronização automática via NTP
- Severidades do syslog (do mais para o menos grave): emerg, alert, crit, err, warning, notice, info, debug
- `journalctl -u serviço` filtra logs por serviço; `-f` acompanha em tempo real; `-b` exibe boot atual
- `/var/log/auth.log` registra tentativas de autenticação
- `mailq` mostra a fila de e-mails; `newaliases` recarrega `/etc/aliases`
- `lp` e `lpr` enviam para impressão; `lpq` lista a fila; `lprm` cancela

---

## ▶️ Próximo Tópico

[Tópico 109 — Fundamentos de Redes](../topico-109-redes/README.md)
