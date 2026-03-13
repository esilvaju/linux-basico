# Exercícios — Tópico 109: Fundamentos de Redes

## Exercícios Práticos

### 1. Explorando Interfaces de Rede

```bash
# a) Liste todas as interfaces de rede e seus endereços IP
ip addr show

# b) Mostre a tabela de roteamento
ip route show

# c) Veja as conexões ativas e portas em escuta
ss -tuln

# d) Veja a tabela ARP (hosts recentemente comunicados)
ip neigh show
```

---

### 2. Testes de Conectividade

```bash
# a) Teste a conectividade com o loopback
ping -c 3 127.0.0.1

# b) Teste a conectividade com o gateway padrão
GATEWAY=$(ip route | grep default | awk '{print $3}')
echo "Gateway: $GATEWAY"
ping -c 3 $GATEWAY

# c) Teste resolução de DNS
host google.com
dig +short google.com
```

---

### 3. DNS e Resolução de Nomes

```bash
# a) Veja a configuração atual de DNS
cat /etc/resolv.conf

# b) Adicione uma entrada local em /etc/hosts
echo "127.0.0.1 meu_servidor_teste" | sudo tee -a /etc/hosts
ping -c 1 meu_servidor_teste

# c) Faça uma consulta DNS reversa (IP → nome)
host 8.8.8.8

# d) Consulte registros MX de um domínio
dig gmail.com MX
```

---

## Questões de Múltipla Escolha (Estilo LPIC-1)

**1.** Qual faixa de endereços IP pertence à **Classe C** de endereços privados?

- A) `10.0.0.0/8`
- B) `172.16.0.0/12`
- C) `192.168.0.0/16`
- D) `169.254.0.0/16`

---

**2.** Qual arquivo define a **ordem de resolução de nomes** no Linux (se consultar `/etc/hosts` antes do DNS ou vice-versa)?

- A) `/etc/resolv.conf`
- B) `/etc/hosts`
- C) `/etc/nsswitch.conf`
- D) `/etc/network/dns.conf`

---

**3.** Qual comando moderno substitui o `ifconfig` para mostrar interfaces e endereços IP?

- A) `net show`
- B) `ipconfig`
- C) `ip addr show`
- D) `network list`

---

**4.** Qual porta TCP é usada pelo protocolo SSH?

- A) 21
- B) 22
- C) 23
- D) 25

---

**5.** Qual comando mostra as portas TCP e UDP em **escuta** (listening) no sistema?

- A) `netstat -r`
- B) `ss -tuln`
- C) `ip link show`
- D) `traceroute localhost`

---

**6.** Qual arquivo contém os servidores DNS usados pelo sistema?

- A) `/etc/hosts`
- B) `/etc/dns.conf`
- C) `/etc/nameservers`
- D) `/etc/resolv.conf`

---

**7.** Qual é o endereço de loopback IPv6?

- A) `fe80::1`
- B) `::1`
- C) `127.0.0.1`
- D) `0.0.0.0`

---

## Respostas

| Questão | Resposta | Explicação |
|---------|----------|------------|
| 1 | **C** | Os endereços privados Classe C são 192.168.0.0/16. Classe A = 10.0.0.0/8, Classe B = 172.16.0.0/12. |
| 2 | **C** | `/etc/nsswitch.conf` define a ordem. Linha típica: `hosts: files dns` (consulta `/etc/hosts` primeiro). |
| 3 | **C** | `ip addr show` é o comando moderno. `ifconfig` é legado. |
| 4 | **B** | SSH usa a porta TCP 22. FTP=21, Telnet=23, SMTP=25. |
| 5 | **B** | `ss -tuln`: `-t` TCP, `-u` UDP, `-l` listening (em escuta), `-n` numérico (não resolve nomes). |
| 6 | **D** | `/etc/resolv.conf` contém as linhas `nameserver IP` com os servidores DNS. |
| 7 | **B** | `::1` é o endereço de loopback IPv6, equivalente ao `127.0.0.1` do IPv4. |
