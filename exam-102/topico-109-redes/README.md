# Tópico 109 — Fundamentos de Redes

## Objetivos (LPIC-1)

- **109.1** Fundamentos dos protocolos de internet
- **109.2** Configuração de rede persistente
- **109.3** Solução básica de problemas de rede
- **109.4** Configurar DNS no lado do cliente

---

## 109.1 — Fundamentos dos Protocolos de Internet

### Modelo TCP/IP

| Camada | Protocolos | Descrição |
|--------|------------|-----------|
| Aplicação | HTTP, FTP, SSH, DNS, SMTP | Comunicação de aplicações |
| Transporte | TCP, UDP | Controle de conexão e confiabilidade |
| Internet | IP, ICMP, ARP | Endereçamento e roteamento |
| Acesso à rede | Ethernet, Wi-Fi | Transmissão física |

### Endereçamento IPv4

```
Endereço IP: 192.168.1.100 / 24

Notação CIDR:
/24 = 255.255.255.0 = 256 endereços (254 hosts)
/16 = 255.255.0.0   = 65.536 endereços
/8  = 255.0.0.0     = 16.777.216 endereços

Endereços reservados para redes privadas:
10.0.0.0/8         (Classe A)
172.16.0.0/12      (Classe B)
192.168.0.0/16     (Classe C)

Loopback:
127.0.0.1 / ::1 (IPv6)
```

### Portas Comuns

| Porta | Protocolo | Serviço |
|-------|-----------|---------|
| 20/21 | TCP | FTP |
| 22 | TCP | SSH |
| 23 | TCP | Telnet |
| 25 | TCP | SMTP |
| 53 | TCP/UDP | DNS |
| 80 | TCP | HTTP |
| 110 | TCP | POP3 |
| 143 | TCP | IMAP |
| 443 | TCP | HTTPS |
| 993 | TCP | IMAPS |
| 3306 | TCP | MySQL |

### TCP vs UDP

| Característica | TCP | UDP |
|----------------|-----|-----|
| Conexão | Orientado a conexão | Sem conexão |
| Confiabilidade | Garantida (ACK) | Não garantida |
| Ordem | Mantém a ordem | Não garante ordem |
| Velocidade | Mais lento | Mais rápido |
| Uso | HTTP, SSH, FTP | DNS, streaming, VoIP |

---

## 109.2 — Configuração de Rede Persistente

### Interfaces de Rede

```bash
# Ver interfaces de rede
ip link show
ip addr show
ifconfig            # legado (pode precisar instalar)

# Nome das interfaces:
# eth0, eth1 — Ethernet (nomenclatura antiga)
# ens3, enp2s0 — Ethernet (nomenclatura predictable, moderna)
# wlan0, wlp2s0 — Wi-Fi
# lo — Loopback
```

### Configuração com ip (moderno)

```bash
# Ver endereços IP
ip addr show
ip addr show eth0

# Adicionar endereço IP (temporário)
ip addr add 192.168.1.100/24 dev eth0

# Remover endereço IP
ip addr del 192.168.1.100/24 dev eth0

# Ativar/desativar interface
ip link set eth0 up
ip link set eth0 down

# Ver tabela de roteamento
ip route show

# Adicionar rota padrão (gateway)
ip route add default via 192.168.1.1

# Adicionar rota estática
ip route add 10.0.0.0/8 via 192.168.1.1
```

### Configuração Persistente

#### Debian/Ubuntu — `/etc/network/interfaces`

```
# Interface de loopback
auto lo
iface lo inet loopback

# Interface com IP estático
auto eth0
iface eth0 inet static
    address 192.168.1.100
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4

# Interface com DHCP
auto eth0
iface eth0 inet dhcp
```

#### RHEL/CentOS — `/etc/sysconfig/network-scripts/ifcfg-eth0`

```
DEVICE=eth0
BOOTPROTO=static
IPADDR=192.168.1.100
NETMASK=255.255.255.0
GATEWAY=192.168.1.1
DNS1=8.8.8.8
ONBOOT=yes
```

#### NetworkManager

```bash
# Status do NetworkManager
systemctl status NetworkManager

# Gerenciar conexões com nmcli
nmcli connection show
nmcli device status

# Criar conexão com IP estático
nmcli connection add type ethernet ifname eth0 \
    ip4 192.168.1.100/24 gw4 192.168.1.1

# Ativar/desativar conexão
nmcli connection up nome_conexao
nmcli connection down nome_conexao
```

---

## 109.3 — Solução de Problemas de Rede

```bash
# Testar conectividade (ping)
ping google.com
ping -c 4 192.168.1.1      # enviar apenas 4 pacotes
ping6 ::1                   # IPv6

# Rastrear rota até um destino
traceroute google.com
tracepath google.com        # alternativa sem root

# Ver conexões ativas
ss -tuln                    # sockets em escuta
ss -tunp                    # com PID do processo
netstat -tuln               # legado

# Verificar portas abertas
ss -l
nmap localhost              # requer instalação

# Testar conexão em uma porta específica
nc -zv 192.168.1.1 22       # testa SSH no host
telnet google.com 80        # teste HTTP

# Ver estatísticas de rede
ip -s link show eth0

# Ver a tabela ARP
ip neigh show
arp -n                      # legado

# Renovar endereço DHCP
dhclient eth0
dhclient -r eth0 && dhclient eth0   # renovar
```

---

## 109.4 — Configuração de DNS no Cliente

### Arquivos de Configuração

```bash
# Servidores DNS
cat /etc/resolv.conf

# Exemplo de /etc/resolv.conf:
# nameserver 8.8.8.8
# nameserver 8.8.4.4
# search meu_dominio.local

# Ordem de resolução de nomes
cat /etc/nsswitch.conf | grep hosts
# hosts: files dns
# files = /etc/hosts é consultado primeiro, depois DNS

# Mapeamentos locais
cat /etc/hosts
# 127.0.0.1     localhost
# 192.168.1.10  meu_servidor  servidor
```

### Ferramentas de DNS

```bash
# Consultar DNS
host google.com
host 8.8.8.8                     # reverse lookup

nslookup google.com
nslookup google.com 8.8.8.8      # usando servidor específico

dig google.com
dig google.com MX                 # registros de e-mail
dig google.com AAAA               # IPv6
dig @8.8.8.8 google.com           # usando servidor específico
dig +short google.com             # saída resumida
```

---

## 📌 Pontos-Chave para o Exame

- Classes de endereços privados: 10.x.x.x/8, 172.16.x.x/12, 192.168.x.x/16
- Loopback: 127.0.0.1 (IPv4) e ::1 (IPv6)
- `ip addr`, `ip route`, `ip link` — comandos modernos de rede
- `ifconfig` e `netstat` são legados mas ainda cobrados no exame
- `/etc/resolv.conf` define os servidores DNS; `/etc/hosts` tem mapeamentos locais
- A ordem de resolução de nomes é definida em `/etc/nsswitch.conf`
- `ss` substitui `netstat` nos sistemas modernos
- Portas importantes: 22 (SSH), 25 (SMTP), 53 (DNS), 80 (HTTP), 443 (HTTPS)

---

## ▶️ Próximo Tópico

[Tópico 110 — Segurança](../topico-110-seguranca/README.md)
