# Tópico 106 — Interfaces de Usuário e Desktops

## Objetivos (LPIC-1)

- **106.1** Instalar e configurar o X11
- **106.2** Desktops gráficos
- **106.3** Acessibilidade

---

## 106.1 — X11 (Sistema X Window)

O **X11** (ou X Window System) é o sistema de janelas padrão no Linux. Ele fornece a base para os ambientes gráficos (GNOME, KDE, etc.).

### Conceitos Fundamentais

- **X Server:** Gerencia o hardware gráfico (tela, teclado, mouse)
- **X Client:** Aplicação que desenha na tela (qualquer programa com janela)
- **Display Manager:** Tela de login gráfico (GDM, SDDM, LightDM)
- **Window Manager:** Gerencia o posicionamento e decoração das janelas

### Configuração do X11

```bash
# Arquivo de configuração principal (moderno — gerado automaticamente)
/etc/X11/xorg.conf
/etc/X11/xorg.conf.d/

# Gerar configuração básica do Xorg
Xorg -configure         # gera /root/xorg.conf.new

# Verificar logs do X server
cat /var/log/Xorg.0.log
```

### Variável de Display

```bash
# Ver o display atual
echo $DISPLAY
# Saída típica: :0 ou :0.0

# Executar aplicação em display remoto
DISPLAY=:0 xterm
export DISPLAY=192.168.1.100:0
```

### X11 Forwarding via SSH

```bash
# Conectar via SSH habilitando X11 forwarding
ssh -X usuario@servidor
ssh -Y usuario@servidor    # trusted forwarding (menos restritivo)

# Testar com um programa gráfico simples
xterm &
xclock &
```

---

## 106.2 — Desktops Gráficos

### Ambientes de Desktop Comuns

| Desktop | Descrição | Distribuições |
|---------|-----------|---------------|
| GNOME | Moderno, minimalista | Ubuntu, Fedora, Debian |
| KDE Plasma | Rico em recursos, customizável | openSUSE, Kubuntu |
| XFCE | Leve e eficiente | Xubuntu, Manjaro XFCE |
| LXDE / LXQt | Muito leve | Lubuntu |
| MATE | Fork do GNOME 2 | Ubuntu MATE |
| Cinnamon | Moderno com aparência clássica | Linux Mint |

### Display Managers (Gerenciadores de Login)

| Display Manager | Desktop Padrão |
|----------------|----------------|
| GDM | GNOME |
| SDDM | KDE Plasma |
| LightDM | XFCE, LXDE, MATE |
| XDM | X11 genérico |

```bash
# Ver qual display manager está em uso
systemctl status display-manager

# Alternar entre modo gráfico e texto
systemctl isolate multi-user.target    # modo texto
systemctl isolate graphical.target     # modo gráfico

# Definir modo padrão
systemctl set-default graphical.target
systemctl set-default multi-user.target
```

### Protocolos Modernos

- **Wayland:** Substituto moderno do X11, mais seguro e eficiente. É o padrão no Ubuntu e Fedora modernos.
- **XWayland:** Camada de compatibilidade para rodar aplicações X11 no Wayland.

```bash
# Verificar se está usando Wayland ou X11
echo $WAYLAND_DISPLAY    # se não vazio, está no Wayland
echo $XDG_SESSION_TYPE   # 'wayland' ou 'x11'
```

---

## 106.3 — Acessibilidade

O Linux oferece diversas ferramentas de acessibilidade:

| Ferramenta | Tipo | Descrição |
|------------|------|-----------|
| Orca | Leitor de tela | Para usuários com deficiência visual |
| Magnifier (GNOME) | Ampliação | Zoom de tela |
| onboard | Teclado virtual | Teclado em tela para mouse/toque |
| Brltty | Braille | Suporte a terminais braille |
| GOK | Teclado | Teclado na tela |

```bash
# Iniciar o leitor de tela Orca
orca

# Configurações de acessibilidade via linha de comando (GNOME)
gsettings set org.gnome.desktop.a11y.applications screen-reader-enabled true
```

---

## 📌 Pontos-Chave para o Exame

- X11 usa arquitetura cliente-servidor; o X Server controla o hardware gráfico
- A variável `$DISPLAY` indica o display em uso (ex: `:0`)
- `ssh -X` habilita X11 forwarding para executar aplicações gráficas remotamente
- Display managers (GDM, SDDM, LightDM) fornecem a tela de login gráfico
- Wayland é o sucessor moderno do X11
- `systemctl set-default graphical.target` define o boot em modo gráfico

---

## ▶️ Próximo Tópico

[Tópico 107 — Tarefas Administrativas](../topico-107-administrativo/README.md)
