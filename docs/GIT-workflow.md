# Git Workflow (Template práctico para todo el curso)

Este flujo simula un entorno profesional. Úsalo *siempre* que trabajes un módulo o laboratorio.

---

## 1) Convenciones rápidas

* **Ramas**

  * Por módulo: `module-01`, `module-02`, ...
  * Por lab dentro de un módulo (opcional): `module-01-lab-tcpdump`
  * Fixes puntuales: `fix/<breve-descripcion>`
* **Commits** (Convencional)

  * `feat:` funcionalidad nueva (labs, scripts, reglas)
  * `docs:` documentación (README, theory/practice)
  * `fix:` correcciones
  * `refactor:` reestructuración sin cambiar comportamiento
  * `chore:` tareas varias (estructura, .gitignore, formatos)
* **Pull Requests**

  * Siempre de la rama de trabajo → `main`
  * Descripción con **qué**, **por qué**, **evidencias** (capturas/PCAPs), y **cómo probar**

---

## 2) Flujo estándar por módulo

```bash
# 0) Estar en main actualizado
git checkout main
git pull

# 1) Crear la rama del módulo
git checkout -b module-01

# 2) Trabajar con commits atómicos
# ejemplo: crear esqueletos
mkdir -p modules/Module_01
cp templates/module/theory.md modules/Module_01/theory.md 2>/dev/null || true
cp templates/module/practice.md modules/Module_01/practice.md 2>/dev/null || true
cp templates/module/resources.md modules/Module_01/resources.md 2>/dev/null || true

git add modules/Module_01
git commit -m "feat(module-01): scaffolding theory/practice/resources"

# 3) Subir rama
git push -u origin module-01

# 4) (Opcional) Ramas de laboratorio específicas
git checkout -b module-01-lab-tcpdump
# ... trabajo del lab ...
git add .
git commit -m "feat(module-01): add tcpdump capture lab and notes"
git push -u origin module-01-lab-tcpdump

# 5) Merge de lab → rama del módulo
git checkout module-01
git merge --no-ff module-01-lab-tcpdump -m "merge: lab tcpdump into module-01"

# 6) Abrir Pull Request module-01 → main (desde GitHub)
# 7) Tras aprobar PR
git checkout main
git pull
# (Si mergiaste por GitHub, ya está)
```

---

## 3) Plantilla de Pull Request (`.github/pull_request_template.md`)

```markdown
## Resumen
- Módulo/Lab: [p.ej., Module_01 / Lab: tcpdump]
- Objetivo: [qué se añadió o modificó]

## Cambios
- [ ] Teoría: `modules/Module_01/theory.md`
- [ ] Práctica: `modules/Module_01/practice.md`
- [ ] Recursos: `modules/Module_01/resources.md`
- [ ] Evidencias en `assets/` (capturas, PCAPs, configs)

## Cómo probar
1. [pasos para reproducir el lab/resultado]
2. [comandos relevantes]

## Evidencias
- Capturas: `assets/module_01/lab_tcpdump/*.png`
- PCAP: `assets/module_01/lab_tcpdump/capture.pcap`

## Checklist
- [ ] Commits atómicos y mensajes claros
- [ ] Estructura de carpetas respetada
- [ ] README/Notas actualizadas si aplica
```

> Crea la carpeta `.github/` y sube ese archivo para que GitHub te ofrezca el template automáticamente.

---

## 4) Mensajes de commit (buenos ejemplos)

```text
feat(module-01): add tcpdump ring buffer capture script

docs(module-01): explain NIST IR cycle and SOC roles

fix(module-01): correct wireshark filter examples (tcp.flags.syn==1)

refactor(repo): move pcap assets into assets/module_01/

chore(ci): add pre-commit hook to lint markdown
```

---

## 5) Estructura recomendada de templates (opcional)

```
templates/
└── module/
    ├── theory.md
    ├── practice.md
    └── resources.md
```

**`theory.md` (esqueleto):**

```markdown
# Módulo X — Título
## Objetivos
-
## Contenidos clave
-
## Lecturas/Recursos
-
```

**`practice.md` (esqueleto):**

```markdown
# Módulo X — Laboratorios
## Lab 1: [nombre]
### Objetivo
### Pasos
### Resultados/Evidencias

## Lab 2: [nombre]
...
```

---

## 6) .gitignore básico (añádelo en la raíz)

```gitignore
# OS
.DS_Store
Thumbs.db

# Python
__pycache__/
*.pyc
.venv/
.env

# Capturas/PCAPs pesados (versiona solo muestras representativas)
assets/**/*.pcap
assets/**/*.pcapng
assets/**/*.zip
assets/**/*.7z

# Logs
*.log
logs/

# IDEs
.vscode/
.idea/
```

---

## 7) Etiquetar hitos (tags)

```bash
# tras mergear module-01 a main
git checkout main
git pull

git tag -a v0.1-module-01 -m "Hito: módulo 01 completado"
git push origin v0.1-module-01
```

---

## 8) Aliases útiles (opcional)

```bash
git config alias.st 'status -sb'
git config alias.co 'checkout'
git config alias.br 'branch'
git config alias.cm 'commit -m'
git config alias.last 'log -1 --stat'
```

---

## 9) Buenas prácticas

* Commits pequeños y descriptivos (1 idea por commit).
* PR con evidencias: capturas, PCAPs **pequeños** o muestras.
* Documenta comandos exactos y resultados esperados.
* Mantén `main` siempre estable.

---

## 10) Ejemplo de sesión real (resumen)

```bash
git checkout -b module-01
# trabajar teoría + práctica
vim modules/Module_01/theory.md
vim modules/Module_01/practice.md

git add modules/Module_01/theory.md
git commit -m "docs(module-01): SOC overview and NIST IR"

git add modules/Module_01/practice.md assets/module_01/lab_tcpdump/*
git commit -m "feat(module-01): add tcpdump lab with pcap and screenshots"

git push -u origin module-01
# abrir PR en GitHub con el template
```
