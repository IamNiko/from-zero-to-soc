modules/Module_02/practice.md

# Módulo 2 — Práctica: hardening rápido y baseline del host

## Objetivo
Aplicar un mínimo hardening en Linux (usuario limitado, SSH por claves, UFW básico, banner legal) y generar un **baseline** del host con un script.

---

## Pasos iniciales de Git
```bash
git branch --show-current
git checkout -b module-02      # si no existe
git checkout module-02         # si ya existe
git pull origin module-02

1) Crear usuario limitado (sin privilegios por defecto)

sudo useradd -m -s /bin/bash analyst
sudo passwd analyst
# (opcional) añadir a sudo sólo si lo necesitas:
# sudo usermod -aG sudo analyst

Validación

id analyst && getent passwd analyst

2) Acceso SSH con clave (desactivar password opcional)

En tu máquina cliente, generar clave si no la tienes:

ssh-keygen -t ed25519 -C "analyst@lab"
ssh-copy-id analyst@<IP_PI_o_Linux>

Opcional endurecer /etc/ssh/sshd_config:

PasswordAuthentication no
PermitRootLogin no
ClientAliveInterval 300
ClientAliveCountMax 2

Reinicia SSH:

sudo systemctl restart ssh

Validación

ssh analyst@<IP> 'echo OK'

3) UFW: política por defecto y reglas básicas

sudo apt update && sudo apt install ufw -y
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw allow 22/tcp           # acceso SSH
# (si montarás web) sudo ufw allow 80/tcp
# (si TLS)          sudo ufw allow 443/tcp
sudo ufw enable
sudo ufw status numbered

Validación: sudo ufw status debe mostrar reglas esperadas.
4) Banner legal

echo "ATENCIÓN: Acceso exclusivo. Toda actividad puede ser monitoreada." | sudo tee /etc/issue.net
# En /etc/ssh/sshd_config habilitar:
# Banner /etc/issue.net
sudo systemctl restart ssh

Validación: Reconexión SSH muestra el banner.
5) Script de baseline (recolector de artefactos)

Crea scripts/collect_baseline.sh:

mkdir -p scripts outputs/module_02
cat > scripts/collect_baseline.sh << 'EOF'
#!/usr/bin/env bash
set -euo pipefail

OUT="outputs/module_02/baseline_$(hostname)_$(date +%Y%m%d_%H%M%S).txt"

{
  echo "# BASELINE - $(date)"
  echo "## Sistema"
  uname -a
  lsb_release -a 2>/dev/null || cat /etc/os-release
  echo

  echo "## Usuarios y grupos (top 10)"
  getent passwd | head -n 10
  getent group | head -n 10
  echo

  echo "## Red"
  ip -br addr
  ip route
  echo
  echo "## Puertos en escucha"
  ss -tulpen
  echo

  echo "## Procesos top (CPU/mem)"
  ps aux --sort=-%cpu | head -n 10
  ps aux --sort=-%mem | head -n 10
  echo

  echo "## Servicios activos (systemd)"
  systemctl list-units --type=service --state=running | head -n 50
  echo

  echo "## UFW"
  sudo ufw status verbose || true
  echo

  echo "## Últimos 50 eventos de auth"
  journalctl -u ssh -n 50 --no-pager || sudo tail -n 50 /var/log/auth.log
} > "$OUT"

echo "Baseline escrito en: $OUT"
EOF

chmod +x scripts/collect_baseline.sh

Ejecución

./scripts/collect_baseline.sh

Validación: se genera outputs/module_02/baseline_<host>_<timestamp>.txt con todas las secciones.
6) Evidencias a subir

    outputs/module_02/baseline_*.txt

    Captura de sudo ufw status y del banner SSH (si puedes, PNG en assets/module_02/).

Git — Commit y push

git add scripts/collect_baseline.sh outputs/module_02/ assets/module_02/ modules/Module_02/{theory.md,practice.md,resources.md}
git commit -m "feat(module-02): baseline script, ufw+banners, linux ops"
git push -u origin module-02

Checklist de Validación

    Usuario analyst creado y acceso SSH por clave funcionando.

    UFW activo, incoming deny, 22/tcp permitido (+80/443 si aplica).

    Banner legal visible en conexión SSH.

    Archivo de baseline generado con todas las secciones.


---

### `modules/Module_02/resources.md`
```markdown
# Módulo 2 — Recursos

## Documentación oficial
- Permisos y atributos: `man chmod`, `man chown`, `man chgrp`
- Usuarios/grupos: `man useradd`, `man usermod`, `man groupadd`, `man passwd`, `man sudoers` (usar `visudo`)
- Procesos/servicios: `man ps`, `man systemctl`, `man journalctl`
- Red: `man ip`, `man ss`, `man resolvectl`
- UFW: `man ufw`
- Logrotate: `man logrotate`

## Lecturas recomendadas
- Systemd services 101 (guías introductorias)
- Linux Filesystem Hierarchy (FHS) — resúmenes y cheatsheets
- UFW Essentials — ejemplos de reglas comunes

## Prácticas extra (opcionales)
1. **Forzar journald persistente**  
   Edita `/etc/systemd/journald.conf`:

Storage=persistent
SystemMaxUse=200M

Reinicia: `sudo systemctl restart systemd-journald`

2. **Crear un servicio simple**  
Archivo `/etc/systemd/system/hello.service`:

[Unit]
Description=Hello demo

[Service]
ExecStart=/bin/echo 'hello'
Type=oneshot

[Install]
WantedBy=multi-user.target

```bash
sudo systemctl daemon-reload
sudo systemctl start hello.service
sudo systemctl enable hello.service

    Logrotate personalizado
    Crea /etc/logrotate.d/myapp para rotar /var/log/myapp/*.log semanal y conservar 4.

Git — recordatorio

git add modules/Module_02/resources.md
git commit -m "docs(module-02): add resources and extra practices"
git push


---

## Prácticas extra (opcionales)
1. **Forzar journald persistente**  
   Edita `/etc/systemd/journald.conf`:

Storage=persistent
SystemMaxUse=200M

Reinicia:  
```bash
sudo systemctl restart systemd-journald

    Crear un servicio simple
    Archivo /etc/systemd/system/hello.service:

[Unit]
Description=Hello demo

[Service]
ExecStart=/bin/echo 'hello'
Type=oneshot

[Install]
WantedBy=multi-user.target

    sudo systemctl daemon-reload
    sudo systemctl start hello.service
    sudo systemctl enable hello.service

    Logrotate personalizado
    Crea /etc/logrotate.d/myapp para rotar /var/log/myapp/*.log semanal y conservar 4 rotaciones.

Git — recordatorio

git add modules/Module_02/resources.md
git commit -m "docs(module-02): add resources and extra practices"
git push


Cuando lo tengas listo y subido, hacemos el **merge a `main` y el tag `v1.0-module-02`** igual que en el Módulo 1 para que quede publicado en GitHub.




