# Exercícios — Tópico 108: Serviços Essenciais do Sistema

## Exercícios Práticos

### 1. Hora do Sistema

```bash
# a) Exiba a data e hora atual no formato ISO 8601
date +"%Y-%m-%dT%H:%M:%S"

# b) Exiba apenas o ano atual
date +"%Y"

# c) Verifique o status do serviço de sincronização de tempo
timedatectl status

# d) Veja o relógio de hardware
sudo hwclock --show
```

---

### 2. Logs do Sistema

```bash
# a) Veja as últimas 20 linhas do log do sistema
sudo tail -20 /var/log/syslog 2>/dev/null || sudo journalctl -n 20

# b) Acompanhe os logs em tempo real por 10 segundos (pressione Ctrl+C para parar)
sudo journalctl -f

# c) Veja todos os logs de autenticação
sudo cat /var/log/auth.log 2>/dev/null | tail -20
sudo journalctl -u ssh 2>/dev/null | tail -20

# d) Veja logs do boot atual
journalctl -b | head -30
```

---

### 3. Impressão

```bash
# a) Verifique se o CUPS está rodando
systemctl status cups

# b) Liste as impressoras configuradas
lpstat -p 2>/dev/null || echo "Nenhuma impressora configurada"

# c) Veja a fila de impressão
lpq
```

---

## Questões de Múltipla Escolha (Estilo LPIC-1)

**1.** Qual comando sincroniza o relógio do sistema a partir do relógio de hardware?

- A) `hwclock --systohc`
- B) `hwclock --hctosys`
- C) `date --sync`
- D) `ntpdate --hardware`

---

**2.** No sistema de logging syslog, qual nível de severidade é o mais crítico?

- A) `critical`
- B) `alert`
- C) `emerg`
- D) `error`

---

**3.** Qual comando exibe os logs do serviço `nginx` em tempo real?

- A) `journalctl -f nginx`
- B) `journalctl -u nginx -f`
- C) `tail -f /var/log/nginx`
- D) `syslog --service nginx --follow`

---

**4.** Qual arquivo contém os aliases de e-mail do sistema?

- A) `/etc/mail/aliases`
- B) `/etc/postfix/main.cf`
- C) `/etc/aliases`
- D) `/var/mail/aliases`

---

**5.** Qual comando cancela o trabalho de impressão número 42?

- A) `lp -c 42`
- B) `lprm 42`
- C) `cancel-print 42`
- D) `lpq -r 42`

---

**6.** Qual comando do `journalctl` exibe apenas mensagens com nível de severidade `error` ou superior?

- A) `journalctl --level=error`
- B) `journalctl -p err`
- C) `journalctl -s error`
- D) `journalctl --severity error`

---

## Respostas

| Questão | Resposta | Explicação |
|---------|----------|------------|
| 1 | **B** | `--hctosys` = Hardware Clock TO SYStem. O inverso (`--systohc`) sincroniza do sistema para o hardware. |
| 2 | **C** | A ordem do mais grave para o menos grave: emerg (0), alert (1), crit (2), err (3), warning (4), notice (5), info (6), debug (7). |
| 3 | **B** | `journalctl -u nginx -f` — `-u` filtra por unidade (serviço) e `-f` acompanha em tempo real. |
| 4 | **C** | `/etc/aliases` contém os aliases de e-mail. Após editar, execute `newaliases` para recarregar. |
| 5 | **B** | `lprm número_do_job` cancela o trabalho de impressão. `cancel` também funciona em sistemas com CUPS. |
| 6 | **B** | `journalctl -p err` filtra mensagens com prioridade `err` (3) ou superior (0, 1, 2). |
