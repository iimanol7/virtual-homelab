# 🌐 virtual-homelab

### Infraestructura de red y seguridad virtualizada.
***
Este proyecto es un entorno evolutivo diseñado para simular redes corporativas, probar configuraciones de sistemas, redes y nuevas tecnologias, e incluso crear futuros laboratorios de prueba para pentesting/auditorias de seguridad.

![Status: En Desarrollo](https://img.shields.io/badge/Estado%20actual-Fase%200:%20Core%20Infra-blue)

---

## 📍 Estado del Proyecto

Actualmente trabajando en la **Fase 0**. El objetivo es establecer el enrutamiento base y la salida segura a internet para el resto de la red.

| Fase | Hito | Estado |
| :--- | :--- | :--- |
| **Fase 0** | Core Infra (pfSense + Virtual Networking) | 🟢 **Completado** |
| **Fase 1** | Directorio Activo y Gestión de Identidad | 🟢 **Completado** |
| **Fase 2** | Hardening y Monitorización (SIEM) | ⏳ Pendiente |

> [!INFO]
> A medida que el proyecto se vaya desarrollando, se irán definiendo nuevas fases.

---

## 🏗️ Arquitectura de Red Actual


> Puedes encontrar los diagramas detallados en la carpeta `/diagrams`.

![diagrama-inicial](diagrams/diagrama-inicial.png)

---

## 📂 Estructura del Repositorio

- `docs/`: Documentación técnica detallada de cada servicio.
- `diagrams/`: Esquemas lógicos y físicos de la red.
- `configs/`: Archivos de configuración de sistemas y servicios (Router, Switch, pfSense, .conf, .ini...)
- `scripts/`: Automatizaciones en PowerShell, Bash o Python.

---

## 🛠️ Stack Tecnológico

- **Hipervisor:** VMware Workstation
- **Networking:** pfSense
- **Sistemas:** Windows Server 2022, FreeBSD, Linux
- **Seguridad:** 🔜Coming soon...

---