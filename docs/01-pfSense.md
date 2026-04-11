# Configuración y gestión de pfSense CE

## Índice

1. [Fase 0: Infraestructura Core (Red Virtual y Firewall)](#1-fase-0-infraestructura-core-red-virtual-y-firewall)
    - [Introducción](#introducción)
    - [Especificaciones de Hardware (VMware)](#especificaciones-de-hardware-vmware)
    - [Arquitectura de Red](#arquitectura-de-red)
      - [Adaptador 1: WAN (Lado Público)](#adaptador-1-wan-lado-público)
      - [Adaptador 2: LAN (Lado Privado)](#adaptador-2-lan-lado-privado)
    - [Servicios Configurados](#servicios-configurados)
    - [Decisiones de Diseño y Notas Técnicas](#decisiones-de-diseño-y-notas-técnicas)
    - [Verificaciones](#verificaciones)
      - [A. Asignación de Interfaces e IP](#a-asignación-de-interfaces-e-ip)
      - [B. Conectividad Exterior (WAN)](#b-conectividad-exterior-wan)
      - [C. Aislamiento y Segmentación (VMware)](#c-aislamiento-y-segmentación-vmware)

***

## 1. Fase 0: Infraestructura Core (Red Virtual y Firewall)

### Introducción
En esta fase se ha realizado el despliegue e instalación del firewall **pfSense Community Edition (CE) v2.8.1** sobre un entorno virtualizado en VMware Workstation. Este componente actuará como el "puerto de entrada" (Gateway) y principal defensor de la infraestructura del laboratorio, separando la red doméstica (WAN) de la red privada del HomeLab.

---

### Especificaciones de Hardware (VMware)
Para garantizar un rendimiento óptimo, se han asignado los siguientes recursos a la VM:

| Recurso           | Configuración       | Justificación Técnica                                      |
|:------------------|:--------------------|:-----------------------------------------------------------|
| **CPU** | 2 vCPUs             | Suficiente para procesar tráfico y servicios de red.       |
| **RAM** | 2 GB                | Equilibrio entre rendimiento del sistema y consumo del host.|
| **Almacenamiento**| 16 GB NVMe          | Uso de controlador NVMe para minimizar latencia en logs.   |
| **Formato Disco** | Single File         | Optimización de E/S y facilidad de gestión de archivos.    |
| **Sistema Arch.** | **ZFS** | Resiliencia de datos y capacidad de snapshots avanzada.    |

---

### Arquitectura de Red
El firewall se ha configurado con una topología de "doble pata" para aislar el tráfico:

#### Adaptador 1: WAN (Lado Público)
- **Modo:** Puente (Bridged).
- **Configuración:** DHCP (Client).
- **Función:** Obtener conectividad a Internet directamente desde el router físico, replicando el estado de la conexión física para mayor realismo.

#### Adaptador 2: LAN (Lado Privado)
- **Modo:** Segmento de LAN (`homelab`).
- **Configuración:** IP Estática `10.0.1.1/24`.
- **Función:** Actuar como Gateway predeterminado para todas las máquinas virtuales del laboratorio, garantizando aislamiento total del host.

---

### Servicios Configurados
Durante el asistente de instalación, se han levantado los siguientes servicios críticos:

* **Servidor DHCP:** * Rango: `10.0.1.100` - `10.0.1.200`.
    * Propósito: Asignación automática de IPs a clientes y máquinas de gestión, reservando las primeras 99 direcciones para equipos que precisen IP fija.
* **WebGUI:** Acceso administrativo vía HTTPS (restringido inicialmente a la interfaz LAN).

---

### Decisiones de Diseño y Notas Técnicas
> [!TIP]
> **Aislamiento de Red:** Se ha optado por un **Segmento de LAN** en lugar de una red VMnet convencional para evitar interferencias con el servidor DHCP nativo de VMware y asegurar que el tráfico de la red interna sea 100% invisible para el sistema operativo host.

> [!NOTE]
> **Seguridad:** Se mantiene bloqueado el acceso a la interfaz web desde la WAN por defecto, siguiendo el principio de "mínimo privilegio". La gestión se realizará exclusivamente desde un equipo de administración situado dentro de la red `10.0.1.0/24`.

---

### Verificaciones

#### A. Asignación de Interfaces e IP
- [x] **Estado:** Las interfaces se han mapeado correctamente (em0 para WAN y em1 para LAN).
> **Evidencia:**
> ![Captura de la consola de pfSense mostrando las IPs asignadas](ruta-a-tu-captura-consola.png)

#### B. Conectividad Exterior (WAN)
- [x] **Estado:** El firewall resuelve DNS y tiene salida a internet.
> **Evidencia:**
> ![Captura de un ping exitoso a 8.8.8.8 desde la shell de pfSense](ruta-a-tu-captura-ping.png)

#### C. Aislamiento y Segmentación (VMware)
- [x] **Estado:** El adaptador LAN está vinculado exclusivamente al segmento `homelab`.
> **Evidencia:**
> ![Captura de los ajustes de red de VMware Workstation](ruta-a-tu-captura-ajustes-vmware.png)

---