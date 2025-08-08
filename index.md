# From Zero to SOC — Roadmap Intensivo (8 semanas)

> Objetivo: en 2 meses quedás listo para postularte a Analista SOC Nivel 1 (Tier 1) con portfolio de laboratorios, repositorio Git impecable y un kit de entrevista técnica.

---

## Estructura del curso
- **Repositorio**: `from-zero-to-soc/`
  - `index.md` (este archivo)
  - `README.md` (presentación del proyecto)
  - `modules/Module_0X/{theory.md, practice.md, resources.md}`
  - `assets/` (diagramas, imágenes, capturas)
- **Flujo Git**: ramas por módulo (`module-01`, `module-02`, …), PRs a `main`, commits atómicos.
- **Ritmo**: 5 días por semana, 2–4 h/día. Sábados: repaso + portfolio. Domingos: descanso.

---

## Semana 1 — Fundamentos SOC, IR y entorno de trabajo
**Objetivo**: entender el rol del SOC, ciclo NIST de respuesta a incidentes (IR) y montar el entorno.

**Teoría clave**
- ¿Qué es un SOC? Roles (Tier 1/2/3), flujos y KPIs.
- **NIST 800-61**: Preparación → Detección/Análisis → Contención/Erradicación/Recuperación → Lecciones.
- Amenazas, TTPs, MITRE ATT&CK (visión general).
- Concepto de *log*, *evento*, *alerta* y *incidente*.

**Práctica**
- Preparar entorno: Parrot/Kali + VM Ubuntu Server + Raspberry (si aplica).
- Instalación base: `tcpdump`, `wireshark`, `nmap`, `suricata`, `splunk`/`opensearch`, `ufw`.
- Primer laboratorio: capturar tráfico propio con `tcpdump` y describirlo.

**Entregables**
- `modules/Module_01/theory.md` (resumen SOC + NIST).
- `modules/Module_01/practice.md` (paso a paso del lab de captura y análisis inicial).
- 3 commits atómicos + PR `module-01` → `main`.

---

## Semana 2 — Linux para SOC (operación y automatización básica)
**Objetivo**: moverte con soltura en Linux para análisis y hardening básico.

**Teoría**
- Estructura del sistema, permisos, usuarios/grupos, servicios, `systemd`.
- Redes en Linux: `ip`, `ss`, `netstat`, `iptables`/`nft`, `ufw`.
- Logs: `journald`, `/var/log/*`, rotación, `logrotate`.

**Práctica**
- Playbook: crear usuario limitado, claves SSH, `ufw` básico, banners legales.
- Script Python/Bash: recolectar *artefactos* (red, procesos, puertos, logs) y exportar a `.json`.

**Entregables**
- Cheatsheet Linux SOC (`assets/linux-soc-cheatsheet.png/md`).
- Script `collect_baseline.{sh,py}` con README breve.

---

## Semana 3 — Redes para detección: TCP/IP, DNS, HTTP y análisis de tráfico
**Objetivo**: leer paquetes “a ojo” y detectar anomalías comunes.

**Teoría**
- TCP handshake, estados, retransmisiones, reset.
- DNS operativo para SOC (A/AAAA/CNAME/MX/TXT, TTL, NXDOMAIN, *tunneling*).
- HTTP/HTTPS en la práctica: verbos, cabeceras, códigos, SNI, JA3 (visión).

**Práctica**
- Wireshark: filtros, *follow TCP stream*, exportación.
- `tcpdump` avanzado: rotación y *ring buffers*.
- Laboratorio: detectar un *port scan*, credenciales en claro (lab controlado), y un 404 fuzzing básico.

**Entregables**
- PCAPs anotados en `assets/` + informe corto (`practice.md`).

---

## Semana 4 — Escaneo y Enumeración (Nmap/Masscan) + Baselines
**Objetivo**: diferenciar *ruido de ataque* vs *inventario legítimo*.

**Teoría**
- Tipos de escaneo (SYN/Connect/UDP/Version/Script), fingerprinting, evasión básica.
- Riesgos y ética del escaneo. Detección por IDS/IPS.

**Práctica**
- Crear *baseline* de servicios internos (tu lab) y compararlo con un día “anómalo”.
- Nmap NSE scripts clave para SOC.
- Mini-tool Python: parsear `nmap -oX` → CSV/JSON y *diff* de puertos por host.

**Entregables**
- `nmap-baseline.json` + script `nmap_diff.py` con README.

---

## Semana 5 — Logs y SIEM (Splunk/OpenSearch) + Normalización
**Objetivo**: ingerir, normalizar, buscar e *investigar*.

**Teoría**
- Taxonomía de logs: sistema, red, aplicación, seguridad.
- Pipelines: ingestión → *parsing* → enriquecimiento → reglas/alertas → tablero.
- SPL/KQL (básicos), *data models*, *field extractions*.

**Práctica**
- Ingestar: syslog de tu Linux + Suricata EVE JSON + logs web (`apache/nginx`).
- Consultas típicas: Top talkers, 401/403/404 anómalos, picos de DNS, intentos SSH.
- Dashboard mínimo con 4 paneles SOC.

**Entregables**
- `modules/Module_05/resources.md` con saved searches y snapshots del dashboard.

---

## Semana 6 — IDS/IPS con Suricata: reglas, EVE y tuning
**Objetivo**: detectar comportamientos clave y reducir *false positives*.

**Teoría**
- Arquitectura, *runmodes*, *af-packet*, *nfqueue* (visión), EVE JSON.
- Reglas: sintaxis, clasificaciones, *thresholds*, *suppression*.

**Práctica**
- Implantar Suricata en *mirror* o VM. Cargar Emerging Rules.
- Escribir 3 reglas propias (scan, login SSH sospechoso, exfil DNS). Tuning.
- Exportar alertas al SIEM y correlacionar con logs del host.

**Entregables**
- `custom.rules` + `practice.md` con hallazgos y *before/after* del tuning.

---

## Semana 7 — Detección de amenazas y casos reales (ATT&CK)
**Objetivo**: pensar en TTPs, no solo en *IOCs*.

**Teoría**
- MITRE ATT&CK: tácticas principales y mapeo a telemetría disponible.
- Detecciones basadas en comportamiento vs firmas.

**Práctica**
- Simular 3 escenarios: (1) fuerza bruta SSH, (2) *web recon* + 404 spike, (3) DNS exfil baja y lenta.
- Crear *runbooks* de investigación y *playbooks* de contención.

**Entregables**
- Carpeta `cases/` con guías paso a paso y consultas SIEM asociadas.

---

## Semana 8 — Portfolio, informes y entrevistas
**Objetivo**: quedar contratables.

**Teoría**
- Informe SOC eficaz (ejecutivo + técnico + anexos). STAR para entrevistas.
- Preguntas típicas Tier 1 y *hands-on*.

**Práctica**
- Compilar portfolio: 3 labs estrella con capturas, queries y conclusiones.
- Simulacro de entrevista técnica + roleplay analista-reclutador.

**Entregables**
- `assets/portfolio.pdf` + `assets/cv_soc.pdf` + `resources.md` con bancos de preguntas.

---

## Hitos por módulo (checklist)
- [ ] Ramas por módulo creadas y PRs a `main`.
- [ ] Evidencias subidas (`assets/`): capturas, PCAPs, configs.
- [ ] README por herramienta con comandos y gotchas.
- [ ] Scripts con `--help` y ejemplos.
- [ ] Informe corto por laboratorio (qué, cómo, hallazgos, lecciones).

---

## Convenciones Git
- Convención de ramas: `module-0X`, `lab-<nombre>`, `fix/<cosa>`.
- Commits: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`.
- PR template (resumen, evidencias, checklist de pruebas).

---

## Recursos iniciales (añadir enlaces y notas)
- MITRE ATT&CK Navigator (mapear detecciones del lab).
- Documentación `tcpdump`, Wireshark Display Filters, Nmap NSE docs.
- Suricata User Guide, Splunk Search Reference / OpenSearch Dashboards.

---
