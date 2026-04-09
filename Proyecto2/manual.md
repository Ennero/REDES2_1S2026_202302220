# Manual Técnico - Proyecto 2: Telecom Uno, Redes Nacionales y Link Global

## Tabla de Contenidos
- [Manual Técnico - Proyecto 2](#manual-técnico---proyecto-2)
  - [1. Objetivos](#1-objetivos)
    - [1.1 Objetivo General](#11-objetivo-general)
    - [1.2 Objetivos Específicos](#12-objetivos-específicos)
  - [2. Alcance del Proyecto](#2-alcance-del-proyecto)
  - [3. Arquitectura y Topología de Red por ISP](#3-arquitectura-y-topología-de-red-por-isp)
    - [3.1 ISP 1: Telecom Uno (Topología en Árbol)](#31-isp-1-telecom-uno-topología-en-árbol)
    - [3.2 ISP 2: Redes Nacionales (Modelo Jerárquico)](#32-isp-2-redes-nacionales-modelo-jerárquico)
    - [3.3 ISP 3: Link Global (Hub and Spoke)](#33-isp-3-link-global-hub-and-spoke)
    - [3.4 Interconexión Inter-ISP (Protocolo BGP)](#34-interconexión-inter-isp-protocolo-bgp)
  - [4. Tabla de Conexiones de la Red](#4-tabla-de-conexiones-de-la-red)
  - [5. Tabla de Direccionamiento IP Detallada](#5-tabla-de-direccionamiento-ip-detallada)
  - [6. Subnetting y Diseño de Direccionamiento (VLSM)](#6-subnetting-y-diseño-de-direccionamiento-vlsm)
    - [6.1 ISP 1: Telecom Uno (172.16.10.0/24)](#61-isp-1-telecom-uno-1721610024)
    - [6.2 ISP 2: Redes Nacionales (172.16.20.0/24)](#62-isp-2-redes-nacionales-1721620024)
    - [6.3 ISP 3: Link Global (172.16.32.0/24)](#63-isp-3-link-global-1721632024)
    - [6.4 Enlaces Inter-ISP BGP (192.168.20.0/16)](#64-enlaces-inter-isp-bgp-19216820016)
  - [7. Servicios Centralizados de Red](#7-servicios-centralizados-de-red)
    - [7.1 Servidor DNS y HTTP (Telecom Uno)](#71-servidor-dns-y-http-telecom-uno)
    - [7.2 Servidor DHCP Global (Redes Nacionales)](#72-servidor-dhcp-global-redes-nacionales)
    - [7.3 Conectividad Inalámbrica (Link Global)](#73-conectividad-inalámbrica-link-global)
  - [8. Redundancia, Alta Disponibilidad y Agregación](#8-redundancia-alta-disponibilidad-y-agregación)
    - [8.1 EtherChannel con LACP (802.3ad)](#81-etherchannel-con-lacp-8023ad)
    - [8.2 Redundancia de Capa 3 con HSRP](#82-redundancia-de-capa-3-con-hsrp)
  - [9. Políticas de Seguridad y Listas de Control de Acceso (ACLs)](#9-políticas-de-seguridad-y-listas-de-control-de-acceso-acls)
  - [10. Configuraciones de Dispositivos (Interfaz de Línea de Comandos - CLI)](#10-configuraciones-de-dispositivos-interfaz-de-línea-de-comandos---cli)
  - [11. Despliegue de Costos, Hardware y Tecnologías](#11-despliegue-de-costos-hardware-y-tecnologías)

---

## 1. Objetivos

### 1.1 Objetivo General
Diseñar, simular y documentar una infraestructura de red nacional robusta que interconecte tres Proveedores de Servicios de Internet (ISPs) utilizando el protocolo de enrutamiento de borde BGP, garantizando la escalabilidad, alta disponibilidad y el cumplimiento estricto de políticas de comunicación inter-departamentales mediante tecnologías avanzadas de switching y routing.

### 1.2 Objetivos Específicos
* **Configurar** una red troncal de fibra óptica para interconectar los sistemas autónomos de Telecom Uno, Redes Nacionales y Link Global mediante BGP.
* **Implementar** protocolos de enrutamiento dinámico internos (OSPF y EIGRP) adaptados a las topologías específicas de cada ISP (Árbol, Jerárquico y Hub-and-Spoke).
* **Asegurar** la continuidad de los servicios críticos mediante la implementación de redundancia de puerta de enlace (HSRP) y agregación de enlaces (LACP).
* **Centralizar** la gestión de servicios mediante un servidor DHCP global en el ISP 2 y un servidor de nombres (DNS) y contenido (HTTP) en el ISP 1.
* **Establecer** un perímetro de seguridad interno mediante ACLs extendidas que regulen el flujo de datos entre departamentos de Administración, Ventas, Facturación, Seguridad y Soporte.

---

## 2. Alcance del Proyecto

El presente proyecto abarca el diseño técnico y la implementación lógica en el simulador **Cisco Packet Tracer**. La solución incluye la configuración de al menos 5 routers y 5 hosts por cada proveedor, garantizando la interoperabilidad entre diferentes protocolos de enrutamiento. No se incluye la conexión a redes WAN externas fuera de los tres ISPs simulados, ni la implementación de servicios de seguridad avanzados como Firewalls dedicados o VPNs, limitándose a las capacidades de las ACLs en dispositivos de Capa 3.

---

## 3. Arquitectura y Topología de Red por ISP

### 3.1 ISP 1: Telecom Uno (Topología en Árbol)
Este ISP adopta un diseño en **árbol**, ideal para una estructura jerárquica donde el tráfico fluye desde las hojas (departamentos de Administración y Atención al Cliente) hacia una raíz central (Core Router). Utiliza el protocolo **OSPF** para el enrutamiento interno debido a su eficiencia en infraestructuras con múltiples niveles de jerarquía.

### 3.2 ISP 2: Redes Nacionales (Modelo Jerárquico)
Basado en el modelo de diseño de Cisco, este ISP se divide en capas de **Core, Distribución y Acceso**. Esta separación permite una gestión de tráfico superior y es el entorno donde se implementa **HSRP** para garantizar que los departamentos de Ventas y Facturación nunca pierdan conectividad. También utiliza **OSPF** y es el núcleo de la asignación dinámica de IPs (DHCP) para toda la red nacional.

### 3.3 ISP 3: Link Global (Hub and Spoke)
Diseñado para conectar oficinas remotas o dependencias aisladas (Spokes) a un nodo central (Hub). Esta arquitectura minimiza los costos de infraestructura al concentrar la inteligencia en el Hub. Utiliza el protocolo **EIGRP** por su capacidad de métricas compuestas y facilidad de configuración en este tipo de topologías.

### 3.4 Interconexión Inter-ISP (Protocolo BGP)
La comunicación entre los tres proveedores se realiza mediante **eBGP** (External BGP). Cada ISP posee su propio Sistema Autónomo (AS), permitiendo que las redes internas de cada uno sean conocidas globalmente. La conexión física se realiza mediante **fibra óptica** para soportar el ancho de banda necesario para el intercambio de tablas de rutas nacionales.

---

## 6. Subnetting y Diseño de Direccionamiento (VLSM)

Basado en el carné **202302220**, se han realizado los siguientes cálculos de subredes para optimizar el espacio de direcciones:

### 6.1 ISP 1: Telecom Uno (172.16.10.0/24)
| Departamento | VLAN | Hosts Req. | ID Red | Máscara | Gateway |
|--------------|------|------------|--------|---------|---------|
| Administración | 10 | 60 | 172.16.10.0 | 255.255.255.192 | 172.16.10.1 |
| Atención al Cliente | 20 | 60 | 172.16.10.64 | 255.255.255.192 | 172.16.10.65 |
| Enlaces Internos | - | - | 172.16.10.128 | 255.255.255.224 | Subredes /30 |

### 6.2 ISP 2: Redes Nacionales (172.16.20.0/24)
| Departamento | VLAN | Hosts Req. | ID Red | Máscara | Gateway (VIP HSRP) |
|--------------|------|------------|--------|---------|---------------------|
| Ventas | 30 | 60 | 172.16.20.0 | 255.255.255.192 | 172.16.20.1 |
| Facturación | 40 | 60 | 172.16.20.64 | 255.255.255.192 | 172.16.20.65 |

### 6.3 ISP 3: Link Global (172.16.32.0/24)
| Departamento | VLAN | Hosts Req. | ID Red | Máscara | Gateway |
|--------------|------|------------|--------|---------|---------|
| Soporte | 50 | 60 | 172.16.32.0 | 255.255.255.192 | 172.16.32.1 |
| Seguridad | 60 | 60 | 172.16.32.64 | 255.255.255.192 | 172.16.32.65 |

### 6.4 Enlaces Inter-ISP BGP (192.168.20.0/16)
Se han asignado subredes punto a punto (/30) para la interconexión BGP:
* **Enlace ISP 1 - ISP 2:** 192.168.20.0/30
* **Enlace ISP 2 - ISP 3:** 192.168.20.4/30
* **Enlace ISP 3 - ISP 1:** 192.168.20.8/30

---

## 7. Servicios Centralizados de Red

### 7.1 Servidor DNS y HTTP (Telecom Uno)
Este servidor es el punto de referencia para toda la red.
* **DNS:** Resuelve el dominio `www.proyecto2_202302220.com` a la IP `172.16.10.10`.
* **HTTP:** Aloja la página web con el nombre del estudiante, carné e información del curso de Redes 2.

### 7.2 Servidor DHCP Global (Redes Nacionales)
Para cumplir con la restricción de que "todos los hosts deben obtener IP por DHCP", se ha configurado un servidor centralizado en el ISP 2.
* Contiene **Scopes/Pools** para cada una de las 6 subredes departamentales.
* Se requiere la configuración de `ip helper-address` en todas las interfaces SVI o subinterfaces de los gateways remotos.

### 7.3 Conectividad Inalámbrica (Link Global)
El departamento de Soporte en el ISP 3 cuenta con un **Router Inalámbrico** que proporciona movilidad a los técnicos. El router recibe IP por DHCP en su puerto Internet y asigna IPs en un segmento interno privado a los dispositivos móviles.

---

## 9. Políticas de Seguridad y Listas de Control de Acceso (ACLs)

Las políticas se han diseñado para cumplir con la matriz de tráfico solicitada:

1.  **Seguridad (VLAN 60):** Posee la política más restrictiva. Puede iniciar tráfico hacia cualquier destino, pero **ningún** dispositivo externo puede iniciar comunicación hacia esta red.
2.  **Ventas (VLAN 30):** Solo tiene permitido comunicarse con **Atención al Cliente** y **Facturación**. El tráfico hacia Administración o Soporte está denegado.
3.  **Soporte y Administración:** Tienen "pase libre" para comunicarse con cualquier segmento de la red nacional para tareas de gestión.

---

## 10. Configuraciones de Dispositivos (CLI)

> **Nota:** Se muestran las configuraciones clave de BGP y Redundancia.

### Configuración BGP Router ISP 2 (Redes Nacionales)
```bash
enable
configure terminal
router bgp 200
 bgp router-id 2.2.2.2
 neighbor 192.168.20.1 remote-as 100
 neighbor 192.168.20.6 remote-as 300
 network 172.16.20.0 mask 255.255.255.0
 redistribute ospf 1
 exit
```

### Configuración HSRP en Switch Multicapa (ISP 2)
```bash
interface Vlan30
 description Gateway Ventas
 ip address 172.16.20.2 255.255.255.192
 standby 30 ip 172.16.20.1
 standby 30 priority 120
 standby 30 preempt
 standby 30 track GigabitEthernet0/1
 exit
```

---

## 11. Despliegue de Costos, Hardware y Tecnologías

Para la presentación ejecutiva, se ha elaborado el siguiente presupuesto basado en hardware Cisco de grado empresarial:

| Dispositivo | Cantidad | Función | Costo Unitario | Total |
|-------------|----------|---------|----------------|-------|
| Catalyst 3650-24PS | 3 | Core Multilayer Switching | $3,850.00 | $11,550.00 |
| Router Cisco 2911 | 15 | Enrutamiento ISP y BGP | $2,150.00 | $32,250.00 |
| Router Inalámbrico WAP | 1 | Acceso WiFi ISP 3 | $120.00 | $120.00 |
| Módulos SFP Fibra | 6 | Enlaces Inter-ISP | $280.00 | $1,680.00 |
| Servidores Dell R740 | 2 | DNS, HTTP, DHCP | $4,500.00 | $9,000.00 |
| **TOTAL ESTIMADO** | | | | **$54,600.00** |

**Tecnologías Utilizadas:**
* **Capa 2:** VTP, STP, LACP.
* **Capa 3:** OSPF, EIGRP, BGP, HSRP.
* **Servicios:** DHCP, DNS, HTTP, ACLs Extendidas.
