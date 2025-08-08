# From Zero to SOC — Proyecto de Formación Intensiva

## Sobre mí

Llevo tiempo estudiando ciberseguridad y recientemente he completado la **Certificación Profesional de Ciberseguridad de Google**, además de numerosos desafíos en TryHackMe y cursos gratuitos sobre redes, Linux y otras áreas técnicas clave.
Este repositorio es parte de un plan intensivo de 8 semanas diseñado para reforzar mis habilidades técnicas y operativas como Analista SOC, con el objetivo de **incorporarme profesionalmente en los últimos meses de 2025**.

## Objetivo del proyecto

Desarrollar, documentar y versionar un curso práctico y progresivo que me permita:

* Dominar los fundamentos técnicos necesarios para un puesto **SOC Tier 1**.
* Construir un **portfolio real** con laboratorios, scripts y evidencias.
* Mejorar mis capacidades de análisis, respuesta ante incidentes y trabajo con herramientas clave (SIEM, IDS/IPS, análisis de red, gestión de logs).

## Entorno de laboratorio

El proyecto se ejecutará en un entorno real compuesto por tres equipos físicos:

1. **Parrot OS (equipo principal)**

   * Rol: estación de trabajo del analista y atacante.
   * Uso: desarrollo de laboratorios, escaneos, análisis de tráfico, scripting y administración del repositorio Git.

2. **Raspberry Pi (principalmente víctima con SO Linux)**

   * Rol: víctima principal con Linux, pudiendo actuar también como **router** para prácticas de firewall (UFW) y monitoreo de tráfico.
   * Uso: instalación de servicios, despliegue de Suricata, prácticas de hardening y generación de logs reales.

3. **MiniPC con Windows (víctima)**

   * Rol: víctima con Windows 10/11, configurable desde cero según las necesidades de cada laboratorio.
   * Uso: creación de usuarios, despliegue de servicios vulnerables, análisis de eventos y simulación de ataques orientados a entornos Windows.

Este enfoque permitirá realizar prácticas **ataque → detección → respuesta** en un entorno controlado, generando datos reales para análisis y documentación.

## Estructura del repositorio

```
from-zero-to-soc/
│
├── index.md               # Roadmap detallado del curso
├── README.md               # Presentación y objetivos del proyecto
├── modules/
│   ├── Module_01/
│   │   ├── theory.md
│   │   ├── practice.md
│   │   └── resources.md
│   ├── Module_02/
│   └── ...
└── assets/                 # Imágenes, diagramas, capturas, PCAPs
```

## Convenciones Git

* Ramas por módulo: `module-0X`
* Ramas de laboratorio: `lab-<nombre>`
* Prefijos de commit:

  * `feat:` nueva funcionalidad o laboratorio
  * `fix:` corrección de errores
  * `docs:` documentación
  * `chore:` tareas menores
  * `refactor:` mejoras sin cambio funcional

## Resultado esperado

Al finalizar, este repositorio servirá como **portfolio técnico** para presentar a reclutadores, demostrando no solo conocimientos teóricos, sino también **capacidad práctica y metodológica** en entornos SOC reales.
