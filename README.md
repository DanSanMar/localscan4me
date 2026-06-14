# lscan4me 🏠

**lscan4me** es una herramienta Bash automática pensada para redes locales domésticas. Descubre hosts activos, revisa servicios expuestos y recoge un informe simple de posibles vulnerabilidades conocidas, todo en un único flujo automático.


Flujo actual de la herramienta:

1. Detecta la interfaz de red activa y la subred local.
2. Descubre hosts vivos con ARP Scan.
3. Ejecuta un análisis rápido de servicios y vulnerabilidades conocidas con Nmap.
4. Guarda un reporte automático en tu escritorio para revisión.

#### 1. Módulo de Reconocimiento Inteligente
A diferencia de otros scripts que requieren la entrada manual del rango de red, ALL4ME utiliza el comando `ip route` para identificar la **interfaz de red activa** y calcular automáticamente la **subred CIDR**. Esto minimiza errores humanos y acelera el despliegue en entornos desconocidos.

#### 2. Motores de Descubrimiento de Hosts (Capa 2 y 3)
El script ofrece tres metodologías distintas para encontrar dispositivos vivos:
* **ARP-Scan (Capa 2):** Ideal para redes locales donde se busca rapidez. Incluye un modo **Sigiloso** que introduce retardos de 250ms entre paquetes para evitar saturar las tablas ARP de los switches y pasar desapercibido ante sistemas de monitoreo básicos.
* **Netdiscover (Modo Pasivo):** Una de las funciones más potentes. En este modo, el script no envía tráfico; simplemente pone la interfaz en modo escucha para capturar peticiones ARP, permitiendo identificar hosts sin generar una sola alerta en la red.
* **Nmap Stealth (Capa 3):** Utiliza escaneos de tipo **SYN Scan (-sS)**. Al no completar el apretón de manos TCP (Three-way handshake), el rastro dejado en los logs de las aplicaciones del objetivo es mínimo.

#### 3. Motor de Análisis de Vulnerabilidades
Para el análisis profundo, utiliza el motor de scripts de Nmap (NSE). El script lanza una batería de pruebas que incluyen:
* Detección exacta de **versiones de servicios** (`-sV`).
* Ejecución de scripts predeterminados de seguridad (`-sC`).
* Escaneo específico de **vulnerabilidades conocidas** mediante la categoría `vuln`, que busca CVEs críticos de forma automatizada.

#### 4. Gestión Inteligente de Reportes
Una característica distintiva es su sistema de salida. Aunque el script se ejecute con privilegios de `root` (necesarios para el manejo de interfaces), detecta quién es el **usuario real** tras la sesión de `sudo`. 
* **Auto-detección de ruta:** Localiza carpetas de "Desktop" o "Escritorio" dinámicamente.
* **Gestión de Permisos:** Al finalizar, aplica un `chown` automático para que el reporte pertenezca al usuario normal, eliminando la necesidad de cambiar permisos manualmente para leer o mover el archivo.

---

### ⚙️ Especificaciones Técnicas
* **Lenguaje:** Bash.
* **Compatibilidad:** Sistemas Linux basados en Debian (Ubuntu, Kali, Parrot, etc.).
* **Herramientas usadas:** `arp-scan` y `nmap`.

---

## ✨ Características principales

* 🔍 **Detección Automática:** Identifica interfaces de red y rangos CIDR sin configuración manual.
* 📡 **Descubrimiento Inteligente:** Incluye modos sigilosos, agresivos y pasivos (escucha).
* 🛡️ **Análisis de Vulnerabilidades:** Implementa scripts de Nmap para detectar fallos conocidos.
* 📄 **Generación de Reportes:** Guarda resultados en el Escritorio con gestión de permisos.

---

## 🛠️ Requisitos previos

El script requiere estas herramientas instaladas:

```bash
sudo apt update
sudo apt install nmap arp-scan -y
```

---

## 🚀 Instalación y Uso

Ejecuta el script directamente con sudo:

```bash
sudo ./lscan4me
```

No requiere interacción manual: detecta la red, escanea la red local y genera un reporte automáticamente.

---

## 📌 Uso recomendado

Esta herramienta está pensada para:
* revisar tu propia red doméstica,
* encontrar dispositivos activos,
* detectar servicios expuestos,
* recopilar evidencia para análisis personal o de seguridad.

---

## ⚠️ Aviso

Usa esta herramienta solo en redes que controles o en las que tengas autorización explícita.

---


1. **Entra a la carpeta del proyecto:**
   ```bash
   cd /ruta/del/proyecto
   ```

2. **Dale permisos de ejecución:**
   ```bash
   chmod +x lscan4me
   ```

3. **Ejecuta la herramienta:**
   ```bash
   sudo ./lscan4me
   ```

---

## 📋 Qué hace la herramienta

| Paso | Comando base | Propósito |
| :--- | :--- | :--- |
| Detección de red | `ip route` | Identificar la interfaz activa y la subred local. |
| Descubrimiento de hosts | `arp-scan --localnet` | Encontrar dispositivos activos en tu red doméstica. |
| Análisis de vulnerabilidades | `nmap -Pn -sV --script vuln` | Revisar servicios y fallos conocidos. |

---

## 📁 Gestión de Reportes

El script guarda el reporte en:
* `~/Desktop/Auditoria_Hogar_...txt` (o `/Escritorio/`).

El archivo se crea con permisos para tu usuario normal para que puedas abrirlo fácilmente.

---

## ⚠️ Descargo de Responsabilidad (Disclaimer)

Este script ha sido creado exclusivamente con fines **educativos y de auditoría doméstica**. El uso de estas herramientas contra infraestructuras sin autorización previa es ilegal. El autor no se hace responsable del mal uso de este software.

---
**Desarrollado por DanSanMar**
