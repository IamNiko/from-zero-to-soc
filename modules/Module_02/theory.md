# Módulo 2 — Linux para SOC (operación y automatización básica)

## Objetivos
- Moverte con soltura en Linux para operar, inspeccionar y endurecer (hardening) sistemas.
- Entender permisos, usuarios/grupos, procesos/servicios y red.
- Conocer dónde viven los logs y cómo rotan.

## 1) Sistema de archivos & permisos
- Rutas clave: `/etc` (config), `/var/log` (logs), `/var/www` (apps), `/home/<user>` (perfiles).
- Permisos: `r=4, w=2, x=1`. `chmod`, `chown`, `chgrp`, `umask`.
- Bits especiales: `suid`, `sgid`, `sticky`.

## 2) Usuarios y grupos
- Gestión: `useradd/userdel/usermod`, `groupadd`, `passwd`, `chage`.
- Ficheros: `/etc/passwd`, `/etc/shadow`, `/etc/group`, `/etc/sudoers` (+ `visudo`).
- Buenas prácticas: usuarios sin shell interactiva para servicios; mínimo sudo.

## 3) Procesos y servicios
- Procesos: `ps aux`, `top/htop`, `nice/renice`, `kill/killall`.
- Systemd: `systemctl status|start|stop|enable|disable <servicio>`, `journalctl -u <servicio>`.
- Unidades: `.service`, `.timer`. Logs de systemd via `journalctl`.

## 4) Red en Linux
- Interfaces/IP: `ip -br addr`, `ip route`, `resolvectl status`.
- Conexiones/puertos: `ss -tulpen`, `netstat -tulpen` (si está).
- Firewall: `ufw` (frontal de `nftables`/`iptables`).

## 5) Logs y rotación
- `journalctl` (binario) vs `/var/log/*` (texto: `auth.log`, `syslog`, `kern.log`…).
- Rotación con `logrotate`: `/etc/logrotate.conf`, `/etc/logrotate.d/*`.
- Buenas prácticas SOC: timestamps, zonas horarias, persistencia de journald.

## 6) Automatización ligera
- Bash/Python para recolectar artefactos: usuarios, grupos, puertos, procesos, versión kernel, servicios activos, reglas UFW, últimos eventos `auth.log`.

## 7) Seguridad rápida (baseline)
- Usuario limitado para operaciones.
- SSH con claves, sin password si es posible.
- UFW por defecto deny incoming, allow salientes + aperturas explícitas (22/80/443 si aplica).
- Banner legal en SSH.

## Preguntas de entrevista (Tier 1)
- ¿Dónde verías intentos fallidos de login SSH? (Res: `journalctl -u ssh` o `/var/log/auth.log`)
- Diferencia entre `ss` y `netstat`. (Res: `ss` moderno/rápido; `netstat` legacy)
- ¿Cómo ver procesos escuchando en puertos? (Res: `ss -tulpen`)
- ¿Cómo habilitar/arrancar un servicio en systemd? (Res: `systemctl enable --now <svc>`)
