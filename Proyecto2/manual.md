# Manual Técnico - Proyecto 2: Telecom Uno, Redes Nacionales y Link Global

## Tabla de Contenidos
- [Manual Técnico - Proyecto 2](#manual-técnico---proyecto-2)
  - [1. Objetivos](#1-objetivos)
    - [1.1 Objetivo General](#11-objetivo-general)
    - [1.2 Objetivos Específicos](#12-objetivos-específicos)
  - [2. Alcance del Proyecto](#2-alcance-del-proyecto)
  - [3. Arquitectura y Topología de Red](#3-arquitectura-y-topología-de-red)
    - [3.1 ISP 1: Telecom Uno (Topología en Árbol)](#31-isp-1-telecom-uno)
    - [3.2 ISP 2: Redes Nacionales (Modelo Jerárquico)](#32-isp-2-redes-nacionales)
    - [3.3 ISP 3: Conexiones Futuras (Hub and Spoke)](#33-isp-3-conexiones-futuras)
    - [3.4 Interconexión BGP](#34-interconexión-bgp)
  - [4. Tabla de Conexiones](#4-tabla-de-conexiones)
  - [5. Tabla de Direccionamiento IP](#5-tabla-de-direccionamiento-ip)
  - [6. Subnetting (VLSM)](#6-subnetting-vlsm)
    - [6.1 ISP 1: Telecom Uno (172.16.10.0/24)](#61-isp-1-telecom-uno)
    - [6.2 ISP 2: Redes Nacionales (172.16.20.0/24)](#62-isp-2-redes-nacionales)
    - [6.3 ISP 3: Conexiones Futuras (172.16.32.0/24)](#63-isp-3-conexiones-futuras)
    - [6.4 Enlaces Inter-ISP BGP (192.168.20.0/16)](#64-enlaces-inter-isp-bgp)
  - [7. Servicios de Red](#7-servicios-de-red)
  - [8. Redundancia y Alta Disponibilidad (HSRP, LACP)](#8-redundancia-y-alta-disponibilidad)
  - [9. Listas de Control de Acceso (ACLs)](#9-listas-de-control-de-acceso-acls)

---

## 1. Objetivos

### 1.1 Objetivo General
Diseñar e implementar una infraestructura de red nacional escalable que interconecte tres Proveedores de Servicios de Internet (ISPs) mediante el protocolo BGP, garantizando la convergencia de protocolos de enrutamiento internos (OSPF, EIGRP), alta disponibilidad mediante redundancia de Capa 3 (HSRP) y el cumplimiento de políticas de seguridad inter-departamentales.

### 1.2 Objetivos Específicos
* **Configurar** la interconexión BGP entre Telecom Uno, Redes Nacionales y Conexiones Futuras utilizando fibra óptica y el segmento 192.168.20.0/16.
* **Implementar** una topología en árbol para ISP 1, utilizando OSPF y proveyendo servicios centralizados de DNS y HTTP.
* **Establecer** un modelo jerárquico en ISP 2 con redundancia HSRP y asignación dinámica de direcciones mediante un servidor DHCP global.
* **Diseñar** una arquitectura Hub-and-Spoke en ISP 3 utilizando EIGRP y conectividad inalámbrica.
* **Aplicar** Listas de Control de Acceso (ACLs) para restringir el tráfico según la matriz de comunicación (Seguridad, Soporte, Administración, etc.).

---

## 2. Alcance del Proyecto

El proyecto contempla la simulación completa en **Cisco Packet Tracer**, abarcando:
* Interconexión de 3 ISPs con al menos 5 routers y 5 hosts cada uno.
* Implementación de agregación de enlaces **LACP** (mínimo 2 por ISP).
* Configuración de servicios **DNS, HTTP y DHCP** operativos para toda la red.
* Validación de redundancia mediante **HSRP** en el ISP 2.
* Configuración de un **Router Inalámbrico** en el ISP 3 para dispositivos móviles.
* Todas las configuraciones se realizan estrictamente vía **CLI**.

---

## 3. Arquitectura y Topología de Red

### 3.1 ISP 1: Telecom Uno (Topología en Árbol)
Diseñada para escalabilidad vertical, conectando los departamentos de Administración y Atención al Cliente hacia un Core central que gestiona el enrutamiento OSPF y los servicios de aplicación (HTTP/DNS).

### 3.2 ISP 2: Redes Nacionales (Modelo Jerárquico)
Implementa las capas de Core, Distribución y Acceso. Esta arquitectura es crítica ya que aloja el servidor DHCP global y requiere redundancia HSRP en la capa de distribución/core para asegurar la disponibilidad de los departamentos de Ventas y Facturación.

### 3.3 ISP 3: Conexiones Futuras (Hub and Spoke)
Centraliza el tráfico en un nodo principal (Hub) que se comunica con nodos remotos (Spokes) de Soporte y Seguridad. Utiliza EIGRP por su rápida convergencia y soporte nativo para esta topología.

---

## 6. Subnetting (VLSM)

Basado en el carné **202302220**, se definen los siguientes segmentos base:

### 6.1 ISP 1: Telecom Uno (172.16.10.0/24)
| Departamento | Hosts Est. | ID Red | Máscara | Gateway |
|--------------|------------|--------|---------|---------|
| Administración | 60 | 172.16.10.0 | 255.255.255.192 | 172.16.10.1 |
| Atención al Cliente | 60 | 172.16.10.64 | 255.255.255.192 | 172.16.10.65 |
| Enlaces OSPF | - | 172.16.10.128 | 255.255.255.224 | Subredes /30 |

### 6.2 ISP 2: Redes Nacionales (172.16.20.0/24)
| Departamento | Hosts Est. | ID Red | Máscara | Gateway (VIP HSRP) |
|--------------|------------|--------|---------|---------------------|
| Ventas | 60 | 172.16.20.0 | 255.255.255.192 | 172.16.20.1 |
| Facturación | 60 | 172.16.20.64 | 255.255.255.192 | 172.16.20.65 |

### 6.3 ISP 3: Conexiones Futuras (172.16.32.0/24)
| Departamento | Hosts Est. | ID Red | Máscara | Gateway |
|--------------|------------|--------|---------|---------|
| Soporte | 60 | 172.16.32.0 | 255.255.255.192 | 172.16.32.1 |
| Seguridad | 60 | 172.16.32.64 | 255.255.255.192 | 172.16.32.65 |

### 6.4 Enlaces Inter-ISP BGP (192.168.20.0/16)
Se utilizan subredes /30 para los enlaces de fibra óptica:

| Enlace | Red | IP Router A | IP Router B |
|--------|-----|-------------|-------------|
| ISP 1 <-> ISP 2 | 192.168.20.0/30 | 192.168.20.1 | 192.168.20.2 |
| ISP 2 <-> ISP 3 | 192.168.20.4/30 | 192.168.20.5 | 192.168.20.6 |
| ISP 3 <-> ISP 1 | 192.168.20.8/30 | 192.168.20.9 | 192.168.20.10 |

---

## 7. Servicios de Red

### 7.1 DNS y HTTP (Hospedado en ISP 1)
* **Dominio:** `www.proyecto2_202302220.com`
* **Servidor Web:** Página estática con datos del estudiante y del curso.

### 7.2 DHCP Global (Hospedado en ISP 2)
* El ISP 2 centraliza la asignación de IPs.
* Se requiere configurar `ip helper-address` en todos los gateways de la red.

### 7.3 Conectividad Inalámbrica (ISP 3)
* Implementación de un Router Inalámbrico para el área de Soporte.

---

## 8. Redundancia y Alta Disponibilidad

### 8.1 EtherChannel (LACP)
* Agregación de enlaces entre switches para redundancia de Capa 2.

### 8.2 HSRP (Hot Standby Router Protocol) en ISP 2
* Configuración de Virtual IP para garantizar gateway siempre activo en Ventas/Facturación.

---

## 9. Listas de Control de Acceso (ACLs)

| Origen | Destino | Regla |
|--------|---------|-------|
| Seguridad | Todos | Permitir (Salida) |
| Todos | Seguridad | **Bloquear** |
| Soporte | Todos | Permitir |
| Administración | Todos | Permitir |
| Atención Cliente | Ventas | Permitir |
| Facturación | Ventas | Permitir |
| Ventas | Atención/Facturación | Permitir |

---

## 10. Configuraciones de Dispositivos (CLI)

> **Nota:** Se presentan los extractos de configuración más relevantes para cumplir con los requerimientos de la rúbrica de calificación.

### 10.1 ISP 1: Telecom Uno (OSPF y BGP)
**Router Borde (R1_ISP1)**
```bash
enable
configure terminal
hostname R1_ISP1

! --- Configuración de Interfaces Internas (Árbol) ---
interface GigabitEthernet0/0
 ip address 172.16.10.129 255.255.255.252
 no shutdown
 exit

! --- Enrutamiento OSPF ---
router ospf 1
 network 172.16.10.0 0.0.0.255 area 0
 exit

! --- Interfaces Externas y BGP ---
interface GigabitEthernet0/1
 description Hacia ISP 2
 ip address 192.168.20.1 255.255.255.252
 no shutdown
 exit

interface GigabitEthernet0/2
 description Hacia ISP 3
 ip address 192.168.20.9 255.255.255.252
 no shutdown
 exit

router bgp 100
 bgp router-id 1.1.1.1
 neighbor 192.168.20.2 remote-as 200
 neighbor 192.168.20.10 remote-as 300
 network 172.16.10.0 mask 255.255.255.0
 exit
end
```

### 10.2 ISP 2: Redes Nacionales (OSPF, HSRP, DHCP y BGP)
**Switch Multicapa Distribución (DSW1_ISP2) - HSRP y LACP**
```bash
enable
configure terminal
hostname DSW1_ISP2
ip routing

! --- LACP Hacia DSW2 (Port-Channel 1) ---
interface range FastEthernet0/1 - 2
 channel-protocol lacp
 channel-group 1 mode active
 switchport trunk encapsulation dot1q
 switchport mode trunk
 exit

! --- VLANs y HSRP ---
vlan 20
 name Ventas
vlan 30
 name Facturacion
exit

interface Vlan20
 ip address 172.16.20.2 255.255.255.192
 standby 20 ip 172.16.20.1
 standby 20 priority 110
 standby 20 preempt
 ! Apuntando al Server DHCP
 ip helper-address 172.16.20.254 
 no shutdown
 exit

interface Vlan30
 ip address 172.16.20.66 255.255.255.192
 standby 30 ip 172.16.20.65
 standby 30 priority 110
 standby 30 preempt
 ip helper-address 172.16.20.254
 no shutdown
 exit

! --- Enrutamiento OSPF ---
router ospf 1
 network 172.16.20.0 0.0.0.255 area 0
 exit
end
```

**Router Borde (R2_ISP2)**
```bash
enable
configure terminal
hostname R2_ISP2

! --- BGP ---
router bgp 200
 bgp router-id 2.2.2.2
 neighbor 192.168.20.1 remote-as 100
 neighbor 192.168.20.5 remote-as 300
 network 172.16.20.0 mask 255.255.255.0
 exit
end
```

### 10.3 ISP 3: Conexiones Futuras (EIGRP y BGP)
**Router Borde (R3_ISP3) - Hub**
```bash
enable
configure terminal
hostname R3_ISP3

! --- Enrutamiento EIGRP (Hub) ---
router eigrp 1
 network 172.16.32.0 0.0.0.255
 no auto-summary
 exit

! --- BGP ---
router bgp 300
 bgp router-id 3.3.3.3
 neighbor 192.168.20.6 remote-as 200
 neighbor 192.168.20.10 remote-as 100
 network 172.16.32.0 mask 255.255.255.0
 redistribute eigrp 1
 exit
end
```

## 11. Implementación de ACLs

Las ACLs se aplican para cumplir la estricta matriz de comunicación exigida en el proyecto:

```bash
enable
configure terminal

! --- ACL BLOQUEO SEGURIDAD ---
! Bloquea todo el trafico entrante a la red de Seguridad (172.16.32.64/26) 
! desde las otras redes, pero permite respuestas a tráfico iniciado por Seguridad.
ip access-list extended ACL_SEGURIDAD_IN
 permit tcp any 172.16.32.64 0.0.0.63 established
 permit icmp any 172.16.32.64 0.0.0.63 echo-reply
 deny ip any 172.16.32.64 0.0.0.63
 permit ip any any
 exit

! --- ACL VENTAS ---
! Permitir Atencion al Cliente y Facturacion, Soporte y Admin.
ip access-list extended ACL_VENTAS_IN
 permit ip 172.16.10.64 0.0.0.63 any ! Atencion al Cliente
 permit ip 172.16.20.64 0.0.0.63 any ! Facturacion
 permit ip 172.16.10.0 0.0.0.63 any  ! Admin
 permit ip 172.16.32.0 0.0.0.63 any  ! Soporte
 permit ip 172.16.32.64 0.0.0.63 any ! Seguridad (permitir entrada de respuestas)
 deny ip 172.16.0.0 0.0.255.255 any  ! Bloquear otras subredes internas no autorizadas
 permit ip any any
 exit
end
```
