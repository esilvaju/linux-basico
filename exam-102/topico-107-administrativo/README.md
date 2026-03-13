# Tópico 107 — Tarefas Administrativas

## Objetivos (LPIC-1)

- **107.1** Gerenciar contas de usuário e grupo
- **107.2** Automatizar tarefas de administração com agendamento (cron e at)
- **107.3** Localização e internacionalização

---

## 107.1 — Gerenciamento de Usuários e Grupos

### Arquivos Essenciais

| Arquivo | Descrição |
|---------|-----------|
| `/etc/passwd` | Informações das contas de usuário |
| `/etc/shadow` | Senhas criptografadas dos usuários |
| `/etc/group` | Informações dos grupos |
| `/etc/gshadow` | Senhas dos grupos |
| `/etc/skel/` | Arquivos modelo copiados ao criar um usuário |

### Formato do /etc/passwd

```
nome:senha:UID:GID:comentario:home:shell
root:x:0:0:root:/root:/bin/bash
joao:x:1001:1001:João Silva:/home/joao:/bin/bash
```

### Gerenciamento de Usuários

```bash
# Criar usuário
useradd joao                          # cria o usuário
useradd -m joao                       # cria com diretório home
useradd -m -s /bin/bash -c "João Silva" joao
useradd -u 1500 -g 1000 -m joao       # UID e GID específicos

# Definir/alterar senha
passwd joao

# Modificar usuário
usermod -s /bin/zsh joao              # mudar shell
usermod -aG sudo joao                 # adicionar ao grupo sudo
usermod -l novo_nome joao             # mudar nome de login
usermod -L joao                       # bloquear conta
usermod -U joao                       # desbloquear conta

# Remover usuário
userdel joao                          # remove o usuário
userdel -r joao                       # remove com diretório home

# Ver informações do usuário
id joao
finger joao    # pode precisar instalar
who            # usuários logados
w              # usuários logados com detalhes
last           # histórico de logins
```

### Gerenciamento de Grupos

```bash
# Criar grupo
groupadd desenvolvedores
groupadd -g 1500 desenvolvedores      # com GID específico

# Modificar grupo
groupmod -n novo_nome desenvolvedores  # renomear
groupmod -g 1600 desenvolvedores       # mudar GID

# Remover grupo
groupdel desenvolvedores

# Adicionar usuário a um grupo
gpasswd -a joao desenvolvedores        # adiciona
gpasswd -d joao desenvolvedores        # remove
usermod -aG desenvolvedores joao       # adiciona (alternativo)

# Ver grupos de um usuário
groups joao
id joao
```

### Sudo

```bash
# Arquivo de configuração do sudo
visudo                  # editor seguro para /etc/sudoers
cat /etc/sudoers

# Dar acesso sudo a um usuário (adicionar ao grupo sudo ou wheel)
usermod -aG sudo joao       # Debian/Ubuntu
usermod -aG wheel joao      # RHEL/CentOS

# Executar comando como root
sudo comando
sudo -i                # abre shell como root
sudo -u outro_user comando   # executar como outro usuário

# Ver o que o usuário pode fazer com sudo
sudo -l
```

---

## 107.2 — Agendamento de Tarefas

### cron — Tarefas Recorrentes

```bash
# Editar a crontab do usuário atual
crontab -e

# Ver a crontab do usuário atual
crontab -l

# Remover a crontab do usuário atual
crontab -r

# Ver crontab de outro usuário (root)
crontab -u joao -l
```

#### Formato da Crontab

```
# minuto hora dia_do_mês mês dia_da_semana comando
  0      2    *          *   *             /usr/bin/backup.sh

# Explicação dos campos:
# minuto:       0-59
# hora:         0-23
# dia_do_mês:   1-31
# mês:          1-12
# dia_da_semana: 0-7 (0 e 7 = domingo)
# * = qualquer valor
# */5 = a cada 5
# 1,3,5 = 1, 3 e 5
# 1-5 = de 1 a 5
```

#### Exemplos de Crontab

```
# Executar a cada minuto
* * * * * /script.sh

# Executar às 2h da manhã todos os dias
0 2 * * * /backup.sh

# Executar às 9h de segunda a sexta
0 9 * * 1-5 /relatorio.sh

# Executar a cada 15 minutos
*/15 * * * * /verificar.sh

# Executar às 0h do primeiro dia de cada mês
0 0 1 * * /backup_mensal.sh

# Executar às 20h nas sextas-feiras
0 20 * * 5 /relatorio_semanal.sh
```

#### Diretórios de cron do Sistema

```bash
# Scripts em pastas especiais são executados automaticamente
/etc/cron.hourly/      # a cada hora
/etc/cron.daily/       # diariamente
/etc/cron.weekly/      # semanalmente
/etc/cron.monthly/     # mensalmente

# Crontab do sistema
/etc/crontab
/etc/cron.d/

# Controle de acesso ao cron
/etc/cron.allow        # usuários permitidos
/etc/cron.deny         # usuários bloqueados
```

### at — Tarefas Únicas Agendadas

```bash
# Agendar um comando para executar em um horário específico
at 22:00
at 22:00 tomorrow
at noon
at "10:00 AM Jan 15"
# Depois de digitar o comando, pressione Ctrl+D para confirmar

# Usar com pipe
echo "/usr/bin/backup.sh" | at 3:00 AM

# Listar tarefas agendadas
atq

# Remover tarefa agendada (pelo número do job)
atrm 3

# Controle de acesso ao at
/etc/at.allow
/etc/at.deny
```

---

## 107.3 — Localização e Internacionalização

### Locale

```bash
# Ver o locale atual
locale

# Listar locales disponíveis
locale -a

# Definir locale temporariamente
export LANG=pt_BR.UTF-8
export LC_ALL=pt_BR.UTF-8

# Variáveis de locale
# LANG — padrão geral
# LC_CTYPE — classificação de caracteres
# LC_COLLATE — ordenação
# LC_TIME — formato de data/hora
# LC_NUMERIC — formato de números
# LC_MONETARY — formato monetário
# LC_ALL — substitui todas as outras

# Gerar locales (Debian/Ubuntu)
locale-gen pt_BR.UTF-8
dpkg-reconfigure locales

# Configurar locale no sistema (systemd)
localectl status
localectl set-locale LANG=pt_BR.UTF-8
```

### Timezone

```bash
# Ver o timezone atual
timedatectl status
date

# Listar timezones disponíveis
timedatectl list-timezones
timedatectl list-timezones | grep America

# Definir timezone
timedatectl set-timezone America/Sao_Paulo

# Via link simbólico (método legado)
ln -sf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime

# Arquivo de configuração de timezone
cat /etc/timezone
```

### Codificação de Caracteres

```bash
# Ver a codificação de um arquivo
file arquivo.txt

# Converter codificação
iconv -f latin1 -t utf-8 arquivo_antigo.txt -o arquivo_novo.txt
iconv -f ISO-8859-1 -t UTF-8 entrada.txt > saida.txt

# Listar codificações disponíveis
iconv -l
```

---

## 📌 Pontos-Chave para o Exame

- `/etc/passwd` tem a estrutura: `nome:x:UID:GID:comentario:home:shell`
- `useradd -m` cria o diretório home; `userdel -r` remove o usuário e o home
- `usermod -aG grupo usuario` adiciona o usuário a um grupo sem remover dos outros
- Crontab: 5 campos de tempo (minuto hora dia mês dia_semana) + comando
- `crontab -e` edita; `crontab -l` lista; `crontab -r` remove
- `at` agenda tarefas únicas; `atq` lista; `atrm` cancela
- `timedatectl` gerencia data, hora e timezone em sistemas com systemd

---

## ▶️ Próximo Tópico

[Tópico 108 — Serviços Essenciais do Sistema](../topico-108-servicos/README.md)
