# Módulo 1 — Laboratorio: Captura de tráfico base con tcpdump

## Objetivo

Configurar el entorno inicial y capturar tráfico de red base para establecer una línea de referencia (*baseline*) que servirá para comparar futuros laboratorios y detectar anomalías.

---

# Módulo 1 — Laboratorio: Captura de tráfico base con tcpdump


# Pasos iniciales de git 

##  Verificar o crear la rama de trabajo
```bash
git branch --show-current      # Verifica la rama actual
git checkout -b module-01      # Crea y cambia a module-01 si no existe
git checkout module-01         # Cambia a module-01 si ya existe
git pull origin module-01      # Sincroniza con remoto



## Pasos

### 1. Verificar interfaz de red en Parrot

Ejecuta:

```bash
ip a
```

Identifica el nombre de la interfaz conectada a internet (ej: `eth0`, `wlan0`).
Anótala, la usaremos en todos los comandos.

---

### 2. Instalar tcpdump si no está presente

```bash
sudo apt update && sudo apt install tcpdump -y
```

Este comando también instalará dependencias necesarias.

---

### 3. Capturar tráfico durante 5 minutos

```bash
sudo tcpdump -i eth0 -w assets/module_01/lab_tcpdump/baseline.pcap
```

> Sustituye `eth0` por tu interfaz real.

Mantén la captura activa durante **5 minutos** para obtener suficiente tráfico normal.

---

### 4. Detener la captura

Presiona `CTRL+C` para parar tcpdump.
Verás un resumen en pantalla del número de paquetes capturados.

---

### 5. Verificar el archivo de captura

```bash
ls -lh assets/module_01/lab_tcpdump/
capinfos assets/module_01/lab_tcpdump/baseline.pcap
```

Confirma:

* Tamaño razonable (>5 MB para 5 min de captura típica).
* Duración correcta de \~5 minutos.
* Presencia de tráfico TCP/UDP.

---

## Evidencias a subir

* Archivo `baseline.pcap` (o una versión recortada si pesa demasiado).
* Captura de pantalla mostrando la salida de `capinfos`.

---

## Git

* **Rama:** `module-01`
* **Commit sugerido:**

```bash
git add assets/module_01/lab_tcpdump/baseline.pcap
git commit -m "feat(module-01): add baseline tcpdump capture and verification"
git push -u origin module-01
```

---

## Validación

* Archivo PCAP > 5 MB.
* Contiene tráfico variado (DNS, HTTP, HTTPS).
* Registro completo de \~5 minutos de actividad.
