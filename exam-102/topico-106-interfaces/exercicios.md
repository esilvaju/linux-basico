# Exercícios — Tópico 106: Interfaces de Usuário e Desktops

## Questões de Múltipla Escolha (Estilo LPIC-1)

**1.** Qual variável de ambiente indica o display X11 em uso?

- A) `$XDISPLAY`
- B) `$DISPLAY`
- C) `$X11_DISPLAY`
- D) `$SCREEN`

---

**2.** Qual opção do comando `ssh` habilita o X11 forwarding?

- A) `ssh -x`
- B) `ssh -G`
- C) `ssh -X`
- D) `ssh -F`

---

**3.** Qual dos seguintes é um **Display Manager** (gerenciador de login gráfico)?

- A) GNOME
- B) KDE Plasma
- C) GDM
- D) XFCE

---

**4.** Qual comando systemd muda o sistema para o modo gráfico imediatamente?

- A) `systemctl start graphical`
- B) `systemctl isolate graphical.target`
- C) `init 5`
- D) `startx`

---

**5.** Qual protocolo de servidor gráfico é considerado o sucessor moderno do X11?

- A) OpenGL
- B) Vulkan
- C) Wayland
- D) Mir

---

**6.** O arquivo de configuração do servidor X (Xorg) fica em qual diretório?

- A) `/etc/xorg/`
- B) `/usr/share/X11/`
- C) `/etc/X11/`
- D) `/var/lib/xorg/`

---

## Respostas

| Questão | Resposta | Explicação |
|---------|----------|------------|
| 1 | **B** | `$DISPLAY` define o display X11 (ex: `:0` para o primeiro display local). |
| 2 | **C** | `ssh -X` habilita X11 forwarding. `-Y` é o modo trusted. |
| 3 | **C** | GDM (GNOME Display Manager) é um display manager. GNOME, KDE e XFCE são ambientes de desktop. |
| 4 | **B** | `systemctl isolate graphical.target` muda para o target gráfico imediatamente. |
| 5 | **C** | Wayland é o protocolo moderno que está substituindo o X11. |
| 6 | **C** | O arquivo de configuração do Xorg fica em `/etc/X11/xorg.conf` (ou em `/etc/X11/xorg.conf.d/`). |
