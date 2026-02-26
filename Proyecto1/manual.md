# Manual Técnico - Proyecto 1

## Tabla de Contenidos
- [Manual Técnico - Proyecto 1](#manual-técnico---proyecto-1)
  - [Tabla de Contenidos](#tabla-de-contenidos)
  - [1. Objetivos](#1-objetivos)
    - [1.1 Objetivo General](#11-objetivo-general)
    - [1.2 Objetivos Específicos](#12-objetivos-específicos)
  - [2. Alcance del Proyecto](#2-alcance-del-proyecto)
    - [2.1 Inclusiones](#21-inclusiones)
    - [2.2 Exclusiones](#22-exclusiones)
  - [3. Topología de Red](#3-topología-de-red)
    - [3.1 Red General (MAN)](#31-red-general-man)
    - [3.2 Edificio Izquierdo](#32-edificio-izquierdo)
    - [3.3 Edificio Derecho](#33-edificio-derecho)
    - [3.4 Edificio Administración](#34-edificio-administración)
- [4. Tabla de Conexiones de la Red](#4-tabla-de-conexiones-de-la-red)
  - [4. Tabla de Direccionamiento IP](#4-tabla-de-direccionamiento-ip)
    - [4.1 Direcciones de Dispositivos](#41-direcciones-de-dispositivos)
      - [4.1.1 Edificio Izquierdo](#411-edificio-izquierdo)
      - [4.1.2 Edificio Derecho](#412-edificio-derecho)
      - [4.1.3 Edificio Administración](#413-edificio-administración)
    - [4.2 Direcciones de Puertas de Enlace](#42-direcciones-de-puertas-de-enlace)
      - [4.2.1 Edificio Izquierdo](#421-edificio-izquierdo)
      - [4.2.2 Edificio Derecho](#422-edificio-derecho)
      - [4.2.3 Edificio Administración](#423-edificio-administración)
      - [4.2.4 Enlaces de Enrutamiento (MAN)](#424-enlaces-de-enrutamiento-man)
  - [5. Subnetting](#5-subnetting)
    - [5.1 VLANs (192.188.20.0/24 - VLSM)](#51-vlans-19218820024---vlsm)
    - [5.2 Enlaces de Enrutamiento (10.4.20.0/24 - FLSM /30)](#52-enlaces-de-enrutamiento-10420024---flsm-30)
  - [6. Configuraciones de Dispositivos](#6-configuraciones-de-dispositivos)

---

## 1. Objetivos

### 1.1 Objetivo General
Diseñar, configurar y simular una infraestructura de red empresarial multi-edificio para la organización Chapin Red utilizando Cisco Packet Tracer. El diseño busca garantizar una comunicación segura, eficiente y redundante entre los cuatro edificios de la organización, implementando segmentación de red mediante VLANs, protocolos de enrutamiento dinámico, alta disponibilidad mediante agregación de enlaces y políticas de control de acceso.

### 1.2 Objetivos Específicos
* **Segmentar** la red de cada edificio en VLANs diferenciadas por departamento (VLAN Naranja, VLAN Verde y VLAN ADMIN) para separar el tráfico y mejorar la seguridad.
* **Implementar** un esquema de direccionamiento IP jerárquico utilizando VLSM sobre la red 192.188.20.0/24 para las VLANs y FLSM con subredes /30 sobre la red 10.4.20.0/24 para los enlaces de enrutamiento.
* **Configurar** enrutamiento inter-VLAN e inter-edificios mediante el protocolo OSPF para garantizar la comunicación entre todas las sedes.
* **Establecer** una arquitectura jerárquica de tres capas (Core, Distribución y Acceso) en el edificio izquierdo, garantizando escalabilidad y rendimiento.
* **Implementar** agregación de enlaces con LACP (5 enlaces en el edificio izquierdo) y PAgP (3 enlaces en el edificio derecho) para alta disponibilidad y redundancia.
* **Configurar** el protocolo VTP para la sincronización de VLANs entre switches, designando roles de servidor y cliente según la topología.
* **Implementar** Spanning Tree Protocol (STP) en todos los dispositivos de capa 2 para prevenir bucles de red.
* **Configurar** servidores DHCP centralizados (uno por edificio) con DHCP Relay para la asignación dinámica de direcciones IP a todos los dispositivos finales.
* **Aplicar** políticas de control de acceso mediante ACLs para restringir la comunicación entre VLANs según los requerimientos de seguridad de la organización.
* **Validar** la tolerancia a fallos desconectando puertos de los canales agregados y verificando que no se produzca pérdida de paquetes.

---

## 2. Alcance del Proyecto

### 2.1 Inclusiones
* El diseño, configuración y validación se realizan en el simulador **Cisco Packet Tracer**.
* La topología abarca la interconexión de **cuatro edificios** de Chapin Red mediante una red de área metropolitana (MAN) con fibra óptica.
* Configuración de tecnologías de Capa 2 y Capa 3, incluyendo VLANs, VTP, STP, enrutamiento inter-VLAN, OSPF, LACP, PAgP y ACLs.
* Implementación de una **arquitectura jerárquica de tres capas** (Core, Distribución y Acceso) en el edificio principal izquierdo.
* Asignación de direccionamiento IP y configuración de todos los dispositivos mediante la **Interfaz de Línea de Comandos (CLI)**.
* Configuración de **servidores DHCP** para asignación dinámica de IPs a dispositivos finales (PCs y laptops).
* Implementación de **DHCP Relay** (ip helper-address) en los switches multicapa para reenvío de solicitudes DHCP entre subredes.
* Pruebas de **tolerancia a fallos** en los canales de agregación LACP y PAgP.

### 2.2 Exclusiones
* La conexión de la red con redes externas como WAN o Internet.
* La implementación de mecanismos de seguridad avanzados más allá de las ACLs estándar/extendidas requeridas.
* El uso de direcciones IP estáticas en dispositivos finales (PCs y laptops).
* El uso de interfaces gráficas de usuario (GUI) para la configuración de los dispositivos.
* Configuración de servicios adicionales como DNS o servidores web.

---

## 3. Topología de Red
A continuación se presenta el diagrama de la topología de red implementada en Cisco Packet Tracer para el proyecto Chapin Red. La red interconecta cuatro edificios mediante una red MAN con fibra óptica (módulos Gigabit), con un switch multicapa Cisco 3650 como punto de interconexión central. El enrutamiento inter-edificios se realiza mediante el protocolo OSPF (carné par: 202302220).

### 3.1 Red General (MAN)

![Topología General](./imgs/topologia_general.png)

### 3.2 Edificio Izquierdo

![Edificio Izquierdo](./imgs/edificio_izquierdo.png)

El edificio izquierdo implementa una arquitectura jerárquica completa de tres capas:
- **Capa Core:** MSW Cisco 3650 (Switch5 y Switch0) – switches multicapa de alto rendimiento.
- **Capa Distribución:** MSW Cisco 3650 (Switch6) – switch multicapa para enrutamiento inter-VLAN.
- **Capa Acceso:** Cisco 2960 (Switch0 y Switch1) – conexión de dispositivos finales.

### 3.3 Edificio Derecho

![Edificio Derecho](./imgs/edificio_derecho.png)

### 3.4 Edificio Administración

![Edificio Administración](./imgs/edificio_admin.png)

---

# 4. Tabla de Conexiones de la Red

|  Switch      |Interfaz1|  Switch/PC 1  |Interfaz2|
|--------------|---------|---------------|---------|
| MS1          | Gi1/0/1 | DHCP1         | fa0     |
| MS1          | Gi1/0/2 | DHCP2         | fa0     |
| MS1          | Gi1/1/1 | MS7           | Gi1/1/1 |
| MS1          | Gi1/1/4 | MS2           | Gi1/1/4 |
| MS6          | Gi1/1/2 | MS7           | Gi1/1/2 |
| MS6          | Gi1/1/3 | MS2           | Gi1/1/3 |
| MS6          | Gi1/0/1 | PC0           | fa0     |


Aqui es donde yo deberia de ir conectando más cositas

## 4. Tabla de Direccionamiento IP

### 4.1 Direcciones de Dispositivos

#### 4.1.1 Edificio Izquierdo
| Nombre                          | VLAN | Dirección IP  | Máscara de Red | Puerta de Enlace |
|---------------------------------|------|---------------|----------------|------------------|
| PC0                             | 10   |               |                |                  |
| PC1                             | 10   |               |                |                  |
| Laptop0                         | 20   |               |                |                  |
| Laptop1                         | 20   |               |                |                  |
| SVI Multilayer Switch6 (Dist.)  | 10   |               |                |                  |
| SVI Multilayer Switch6 (Dist.)  | 20   |               |                |                  |
| SVI Multilayer Switch5 (Core)   | 10   |               |                |                  |
| SVI Multilayer Switch5 (Core)   | 20   |               |                |                  |
| SVI Multilayer Switch0 (Core)   | 10   |               |                |                  |
| SVI Multilayer Switch0 (Core)   | 20   |               |                |                  |

#### 4.1.2 Edificio Derecho
| Nombre                          | VLAN | Dirección IP  | Máscara de Red | Puerta de Enlace |
|---------------------------------|------|---------------|----------------|------------------|
| Laptop2                         | 30   |               |                |                  |
| PC3                             | 30   |               |                |                  |
| Laptop3                         | 40   |               |                |                  |
| PC4                             | 40   |               |                |                  |
| SVI Multilayer Switch3          | 30   |               |                |                  |
| SVI Multilayer Switch3          | 40   |               |                |                  |

#### 4.1.3 Edificio Administración
| Nombre                          | VLAN | Dirección IP  | Máscara de Red | Puerta de Enlace |
|---------------------------------|------|---------------|----------------|------------------|
| PC-P1                           | 99   |               |                |                  |
| PC4 (Admin)                     | 99   |               |                |                  |
| SVI Multilayer Switch2          | 99   |               |                |                  |

### 4.2 Direcciones de Puertas de Enlace

#### 4.2.1 Edificio Izquierdo
| Nombre          | VLAN | Dirección IP | Máscara de Red |
|-----------------|------|--------------|----------------|
| VLAN Naranja    | 10   |              |                |
| VLAN Verde      | 20   |              |                |

#### 4.2.2 Edificio Derecho
| Nombre          | VLAN | Dirección IP | Máscara de Red |
|-----------------|------|--------------|----------------|
| VLAN Naranja    | 30   |              |                |
| VLAN Verde      | 40   |              |                |

#### 4.2.3 Edificio Administración
| Nombre          | VLAN | Dirección IP | Máscara de Red |
|-----------------|------|--------------|----------------|
| VLAN ADMIN      | 99   |              |                |

#### 4.2.4 Enlaces de Enrutamiento (MAN)
| Dispositivo A          | Dispositivo B          | Interfaz A | Interfaz B | IP Dispositivo A | IP Dispositivo B | Máscara         |
|------------------------|------------------------|------------|------------|------------------|------------------|-----------------|
| Multilayer Switch4     | Multilayer Switch1     |            |            |                  |                  | 255.255.255.252 |
| Multilayer Switch4     | Multilayer Switch3     |            |            |                  |                  | 255.255.255.252 |
| Multilayer Switch1     | Multilayer Switch3     |            |            |                  |                  | 255.255.255.252 |
| Multilayer Switch1     | Multilayer Switch2     |            |            |                  |                  | 255.255.255.252 |
| Multilayer Switch3     | Multilayer Switch2     |            |            |                  |                  | 255.255.255.252 |

---

## 5. Subnetting

### 5.1 VLANs (192.188.20.0/24 - VLSM)

La red **192.188.20.0/24** se divide en cinco subredes mediante VLSM, una por cada VLAN:

| HOSTS NECESARIOS | VLAN | DEPARTAMENTO               | ID RED | MÁSCARA | WILDCARD | PRIMER HOST | ÚLTIMO HOST | BROADCAST | HOSTS UTILIZABLES |
|-----------------|------|----------------------------|--------|---------|----------|-------------|-------------|-----------|------------------|
|                 | 10   | Naranja – Edificio Izq.    |        |         |          |             |             |           |                  |
|                 | 20   | Verde – Edificio Izq.      |        |         |          |             |             |           |                  |
|                 | 30   | Naranja – Edificio Der.    |        |         |          |             |             |           |                  |
|                 | 40   | Verde – Edificio Der.      |        |         |          |             |             |           |                  |
|                 | 99   | ADMIN – Edificio Admin.    |        |         |          |             |             |           |                  |

### 5.2 Enlaces de Enrutamiento (10.4.20.0/24 - FLSM /30)

La red **10.4.20.0/24** se divide en subredes /30 (4 IPs por enlace) para los enlaces punto a punto entre switches multicapa:

| HOSTS NECESARIOS | Dispositivo A      | Dispositivo B      | ID RED | MÁSCARA         | WILDCARD  | PRIMER HOST | ÚLTIMO HOST | BROADCAST | HOSTS UTILIZABLES | HOSTS UTILIZADOS | DESPERDICIO |
|-----------------|--------------------|--------------------|--------|-----------------|-----------|-------------|-------------|-----------|------------------|-----------------|-------------|
| 2               | Multilayer Switch4 | Multilayer Switch1 |        | 255.255.255.252 | 0.0.0.3   |             |             |           | 2                | 2               | 0           |
| 2               | Multilayer Switch4 | Multilayer Switch3 |        | 255.255.255.252 | 0.0.0.3   |             |             |           | 2                | 2               | 0           |
| 2               | Multilayer Switch1 | Multilayer Switch3 |        | 255.255.255.252 | 0.0.0.3   |             |             |           | 2                | 2               | 0           |
| 2               | Multilayer Switch1 | Multilayer Switch2 |        | 255.255.255.252 | 0.0.0.3   |             |             |           | 2                | 2               | 0           |
| 2               | Multilayer Switch3 | Multilayer Switch2 |        | 255.255.255.252 | 0.0.0.3   |             |             |           | 2                | 2               | 0           |
| 2               | Multilayer Switch1 | Multilayer Switch5 |        | 255.255.255.252 | 0.0.0.3   |             |             |           | 2                | 2               | 0           |
| 2               | Multilayer Switch1 | Multilayer Switch0 |        | 255.255.255.252 | 0.0.0.3   |             |             |           | 2                | 2               | 0           |

---

## 6. Configuraciones de Dispositivos

