# Exercícios — Tópico 107: Tarefas Administrativas

## Exercícios Práticos

### 1. Gerenciamento de Usuários

```bash
# a) Crie um usuário chamado 'estudante' com diretório home e shell bash
sudo useradd -m -s /bin/bash -c "Usuário Estudante" estudante

# b) Defina uma senha para o usuário
sudo passwd estudante

# c) Veja as informações do usuário criado
id estudante
grep estudante /etc/passwd

# d) Crie um grupo chamado 'turma'
sudo groupadd turma

# e) Adicione 'estudante' ao grupo 'turma'
sudo usermod -aG turma estudante

# f) Verifique os grupos do usuário
groups estudante

# g) (Limpeza) Remova o usuário e seu home
sudo userdel -r estudante
sudo groupdel turma
```

---

### 2. Crontab

```bash
# a) Crie uma crontab para executar um script todos os dias às 6h da manhã
crontab -e
# Adicione a linha:
# 0 6 * * * /home/SEU_USUARIO/backup.sh

# b) Liste a crontab atual
crontab -l

# c) Crie uma tarefa que escreve a data atual em um arquivo a cada 5 minutos
crontab -e
# Adicione:
# */5 * * * * echo $(date) >> /tmp/registro_data.txt
```

---

### 3. Localização

```bash
# a) Veja o locale atual do seu sistema
locale

# b) Veja o timezone configurado
timedatectl status

# c) Liste os timezones do Brasil
timedatectl list-timezones | grep Brazil
timedatectl list-timezones | grep America/Sao
```

---

## Questões de Múltipla Escolha (Estilo LPIC-1)

**1.** Qual arquivo contém as senhas criptografadas dos usuários do sistema?

- A) `/etc/passwd`
- B) `/etc/group`
- C) `/etc/shadow`
- D) `/etc/security`

---

**2.** Qual comando adiciona o usuário `maria` ao grupo `sudo` sem remover seus grupos existentes?

- A) `usermod -g sudo maria`
- B) `usermod -aG sudo maria`
- C) `groupmod -a maria sudo`
- D) `gpasswd sudo maria`

---

**3.** O seguinte registro de crontab executa em qual momento?

```
30 8 * * 1 /usr/bin/relatorio.sh
```

- A) Às 8h30 do dia 1 de todo mês
- B) Às 8h30 de toda segunda-feira
- C) Às 30 minutos de todo dia, às 8h
- D) Às 8h30 de todo dia

---

**4.** Qual comando agenda uma tarefa para ser executada **uma única vez** às 15h de amanhã?

- A) `crontab -e`
- B) `at 15:00 tomorrow`
- C) `schedule 15:00 +1day`
- D) `cron "15:00 tomorrow"`

---

**5.** Para que serve o diretório `/etc/skel`?

- A) Armazenar scripts de manutenção do sistema
- B) Fornecer arquivos modelo que são copiados para o home de novos usuários
- C) Armazenar configurações de segurança
- D) Guardar backups dos arquivos de configuração dos usuários

---

**6.** Qual variável de localização define **todas** as configurações de locale de uma vez?

- A) `$LANG`
- B) `$LANGUAGE`
- C) `$LOCALE`
- D) `$LC_ALL`

---

**7.** Qual é o UID do usuário root?

- A) 0
- B) 1
- C) 100
- D) 65534

---

## Respostas

| Questão | Resposta | Explicação |
|---------|----------|------------|
| 1 | **C** | `/etc/shadow` armazena as senhas criptografadas, acessível apenas pelo root. `/etc/passwd` tem o 'x' no lugar da senha. |
| 2 | **B** | `usermod -aG` (append to Group) adiciona ao grupo sem remover outros. Sem `-a`, substitui os grupos. |
| 3 | **B** | Os campos são: 30 (minuto), 8 (hora), * (qualquer dia), * (qualquer mês), 1 (segunda-feira). |
| 4 | **B** | `at` agenda tarefas únicas. `crontab` é para tarefas recorrentes. |
| 5 | **B** | `/etc/skel` contém arquivos como `.bashrc`, `.profile` que são copiados para o home de cada novo usuário criado com `useradd -m`. |
| 6 | **D** | `LC_ALL` substitui todas as outras variáveis de locale quando definida. |
| 7 | **A** | O root sempre tem UID 0. Esse é o critério que o kernel usa para dar privilégios administrativos. |
