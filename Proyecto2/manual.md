# Manual Técnico - Proyecto 2

## Tabla de Contenidos
- [Manual Técnico - Proyecto 2](#manual-técnico---proyecto-2)
  - [Tabla de Contenidos](#tabla-de-contenidos)
  - [1. Objetivos](#1-objetivos)
    - [1.1 Objetivo General](#11-objetivo-general)
    - [1.2 Objetivos Específicos](#12-objetivos-específicos)
  - [2. Alcance del Proyecto](#2-alcance-del-proyecto)
    - [2.1 Inclusiones](#21-inclusiones)
  - [3. Topología de Red](#3-topología-de-red)
    - [3.1 Selección de Hardware (Simulado)](#31-selección-de-hardware-simulado)
  - [4. Tabla de Conexiones de la Red](#4-tabla-de-conexiones-de-la-red)
  - [4.1. Tabla de conexiones de la Red de Telecom](#41-tabla-de-conexiones-de-la-red-de-telecom)
  - [5. Tabla de Direccionamiento IP](#5-tabla-de-direccionamiento-ip)
  - [6. Subnetting](#6-subnetting)
    - [6.1 ISP 1: Telecom Uno (`172.16.10.0/24`)](#61-isp-1-telecom-uno-1721610024)
    - [6.2 ISP 2: Redes Nacionales (`172.16.20.0/24`)](#62-isp-2-redes-nacionales-1721620024)
    - [6.3 ISP 3: Link Global (`172.16.32.0/24`)](#63-isp-3-link-global-1721632024)
    - [6.4 Core BGP (`192.168.20.0/16`)](#64-core-bgp-19216820016)
    - [7.1 Configuración de Enrutamiento BGP en el Núcleo (Core)](#71-configuración-de-enrutamiento-bgp-en-el-núcleo-core)
      - [7.1.1 Configuración MSW\_Telecom (AS 100)](#711-configuración-msw_telecom-as-100)
      - [7.1.2 Configuración MSW\_Nacionales (AS 200)](#712-configuración-msw_nacionales-as-200)
      - [7.1.3 Configuración MSW\_Link (AS 300)](#713-configuración-msw_link-as-300)
    - [7.2 Configuraciones de Dispositivos de Telecom Uno (ISP 1)](#72-configuraciones-de-dispositivos-de-telecom-uno-isp-1)
      - [7.2.1 Configuración de Integración OSPF-BGP en MSW\_Telecom](#721-configuración-de-integración-ospf-bgp-en-msw_telecom)
      - [7.2.2 Configuración R\_Raiz\_T1](#722-configuración-r_raiz_t1)
      - [7.2.3 Configuración MSW\_Dist\_T1 (Distribución y Gateway)](#723-configuración-msw_dist_t1-distribución-y-gateway)
      - [7.2.4 Configuración MSW\_Admin (Capa 2 y Servidores)](#724-configuración-msw_admin-capa-2-y-servidores)
      - [7.2.5 Configuración MSW\_Atencion (Capa 2)](#725-configuración-msw_atencion-capa-2)
    - [7.3 Configuración de Servicios en Telecom Uno (DNS y HTTP)](#73-configuración-de-servicios-en-telecom-uno-dns-y-http)
      - [7.3.1 Configuración de IP Estática (Ambos Servidores)](#731-configuración-de-ip-estática-ambos-servidores)
      - [7.3.2 Configuración del Servicio HTTP](#732-configuración-del-servicio-http)
      - [7.3.3 Configuración del Servicio DNS](#733-configuración-del-servicio-dns)

---

## 1. Objetivos

### 1.1 Objetivo General
Diseñar e implementar una infraestructura nacional de telecomunicaciones que interconecte tres proveedores de servicios (ISP) mediante el protocolo BGP, garantizando una red escalable, redundante y segura que cumpla con los requerimientos específicos de enrutamiento interno (OSPF y EIGRP) y servicios críticos (DNS, HTTP, DHCP).

### 1.2 Objetivos Específicos
- Configurar la interconexión interdominio entre los tres ISPs utilizando BGP sobre un núcleo de switches multicapa con enlaces de fibra óptica.
- Implementar protocolos de enrutamiento dinámico interno: OSPF para Telecom Uno y Redes Nacionales, y EIGRP para Link Global.
- Garantizar alta disponibilidad en la capa de distribución mediante HSRP en el ISP de Redes Nacionales.
- Establecer políticas de seguridad mediante Listas de Control de Acceso (ACL) para regular el tráfico entre los departamentos según la matriz de comunicación permitida.
- Centralizar el servicio de DHCP en Redes Nacionales para proveer direccionamiento dinámico a toda la infraestructura mediante el uso de DHCP Relay.

---

## 2. Alcance del Proyecto

### 2.1 Inclusiones
- Simulación completa en Cisco Packet Tracer utilizando la Interfaz de Línea de Comandos (CLI).
- Configuración de servicios de red: DNS y HTTP (en Telecom Uno) y DHCP (en Redes Nacionales).
- Implementación de topologías específicas por ISP: Árbol, Jerárquico y Hub and Spoke.
- Configuración de agregación de enlaces mediante LACP en todos los ISPs para aumentar el ancho de banda y la redundancia.
- Despliegue de una red inalámbrica en el ISP Link Global para acceso WiFi.

---

## 3. Topología de Red
La infraestructura se divide en cuatro sistemas autónomos principales interconectados en un triángulo central (Core BGP) mediante switches multicapa 3650-24PS utilizando fibra óptica.

> _(Aquí deberás insertar el screenshot de tu topología de Packet Tracer)_

---


### 3.1 Selección de Hardware (Simulado)
Para satisfacer los requerimientos de enrutamiento dinámico (OSPF/EIGRP/BGP), alta disponibilidad (HSRP) y agregación de enlaces (LACP), se seleccionaron los siguientes equipos en Cisco Packet Tracer:

* **Switch Multicapa Cisco 3650-24PS:** Utilizado en el núcleo de interconexión (Core BGP) con módulos de fibra SFP `GLC-LH-SMD`. También se utiliza como dispositivo principal de capa de distribución en los ISPs, ya que soporta enrutamiento de capa 3 nativo (`ip routing`) y facilita enormemente la configuración de LACP y HSRP. Cumplen el rol de "Routers" dentro de las topologías.
* **Switch Cisco 2960-24TT:** Utilizado en la capa de acceso para la conexión de dispositivos finales, configuración de VLANs por departamento y troncalización. Cuenta con puertos FastEthernet y GigabitEthernet ideales para los enlaces LACP hacia la capa de distribución.
* **Router Cisco ISR 4331:** Implementados en las ramas "Spoke" de Link Global y puntos terminales de la topología de árbol para diversificar el hardware y manejar enlaces punto a punto gracias a sus interfaces Gigabit integradas.
* **Router Inalámbrico Linksys WRT300N:** Utilizado en el ISP Link Global para satisfacer la restricción de red inalámbrica, proporcionando un dominio de colisión aislado y DHCP local para los clientes WiFi.

---


## 4. Tabla de Conexiones de la Red

| Dispositivo Origen | Interfaz  | Dispositivo Destino | Interfaz  | Cable          | Módulo SFP  |
|--------------------|-----------|---------------------|-----------|----------------|-------------|
| MSW_Telecom        | Gi1/1/1   | MSW_Nacionales      | Gi1/1/1   | Fibra Óptica   | GLC-LH-SMD  |
| MSW_Nacionales     | Gi1/1/2   | MSW_Link            | Gi1/1/1   | Fibra Óptica   | GLC-LH-SMD  |
| MSW_Link           | Gi1/1/2   | MSW_Telecom         | Gi1/1/2   | Fibra Óptica   | GLC-LH-SMD  |
| MSW_Telecom        | Gi1/0/24  | R_Raiz_T1           | Gi0/0/0   | Cobre Directo  | N/A         |
| MSW_Nacionales     | Gi1/0/24  | MSW_Core_RN         | Gi1/0/1   | Cobre Cruzado  | N/A         |
| MSW_Link           | Gi1/0/24  | R_Hub_LG            | Gi0/0/0   | Cobre Directo  | N/A         |



## 4.1. Tabla de conexiones de la Red de Telecom

| Origen       | Puerto Origen          | Destino       | Puerto Destino         | Cable          |
|--------------|------------------------|---------------|------------------------|----------------|
| MSW_Telecom  | Gi1/0/24               | R_Raiz_T1     | Gi0/0/0                | Cobre Directo  |
| R_Raiz_T1    | Gi0/0/1                | MSW_Dist_T1   | Gi1/0/1                | Cobre Directo  |
| MSW_Dist_T1  | Gi1/0/2 y Gi1/0/3      | MSW_Admin     | Gi1/0/2 y Gi1/0/3      | Cobre Cruzado  |
| MSW_Dist_T1  | Gi1/0/4 y Gi1/0/5      | MSW_Atencion  | Gi1/0/4 y Gi1/0/5      | Cobre Cruzado  |
| MSW_Admin    | Gi1/0/10               | Servidor DNS  | Fa0                    | Cobre Directo  |
| MSW_Admin    | Gi1/0/11               | Servidor HTTP | Fa0                    | Cobre Directo  |

---

## 5. Tabla de Direccionamiento IP

| Dispositivo / Red | Interfaz | Dirección IP | Máscara | Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **Core: T1 ↔ RN** | Enlace BGP (Gi1/1/1) | 192.168.20.1 | 255.255.255.252 | N/A |
| **Core: RN ↔ LG** | Enlace BGP (Gi1/1/2) | 192.168.20.5 | 255.255.255.252 | N/A |
| **Core: LG ↔ T1** | Enlace BGP (Gi1/1/2) | 192.168.20.9 | 255.255.255.252 | N/A |
| **MSW_Telecom ↔ R_Raiz_T1** | Gi1/0/24 ↔ Gi0/0/0 | 172.16.10.129 ↔ .130| 255.255.255.252 | N/A |
| **R_Raiz_T1 ↔ MSW_Dist_T1** | Gi0/0/1 ↔ Gi1/0/1 | 172.16.10.133 ↔ .134| 255.255.255.252 | N/A |
| **SVI: Admin (T1)** | VLAN 10 (MSW_Dist) | 172.16.10.1 | 255.255.255.192 | N/A |
| **SVI: Atencion (T1)**| VLAN 20 (MSW_Dist) | 172.16.10.65 | 255.255.255.192 | N/A |
| **Servidor DNS** | Fa0 | 172.16.10.2 | 255.255.255.192 | 172.16.10.1 |
| **Servidor HTTP** | Fa0 | 172.16.10.3 | 255.255.255.192 | 172.16.10.1 |
| **SVI: Ventas (RN)** | VLAN 30 | 172.16.20.1 | 255.255.255.192 | 172.16.20.1 (VIP) |
| **SVI: Seguridad (LG)**| VLAN 60 | 172.16.32.65 | 255.255.255.192 | N/A |

---

## 6. Subnetting
El esquema de direccionamiento se calculó utilizando VLSM para optimizar el uso de los prefijos asignados según el número de carné 202302220.

### 6.1 ISP 1: Telecom Uno (`172.16.10.0/24`)
Se divide en subredes para los departamentos y enlaces internos:
- **Administración:** 172.16.10.0/26 (62 hosts disponibles)
- **Atención al Cliente:** 172.16.10.64/26 (62 hosts disponibles)
- **Enlaces OSPF:** 172.16.10.128/30 (Subneteo de remanente)

### 6.2 ISP 2: Redes Nacionales (`172.16.20.0/24`)
- **Ventas:** 172.16.20.0/26 (62 hosts disponibles)
- **Facturación:** 172.16.20.64/26 (62 hosts disponibles)
- **Redundancia (VIP HSRP):** Primera IP útil de cada segmento.

### 6.3 ISP 3: Link Global (`172.16.32.0/24`)
- **Soporte:** 172.16.32.0/26 (62 hosts disponibles)
- **Seguridad:** 172.16.32.64/26 (62 hosts disponibles)

### 6.4 Core BGP (`192.168.20.0/16`)
Se utiliza una máscara /30 para los enlaces punto a punto entre los switches multicapa del núcleo:
- **Enlace 1 (T1-RN):** 192.168.20.0/30
- **Enlace 2 (RN-LG):** 192.168.20.4/30
- **Enlace 3 (LG-T1):** 192.168.20.8/30



### 7.1 Configuración de Enrutamiento BGP en el Núcleo (Core)

#### 7.1.1 Configuración MSW_Telecom (AS 100)

```bash
enable
configure terminal
hostname MSW_Telecom

! 1. Activar funciones de ruteo en el switch multicapa
ip routing

! 2. Interfaz hacia MSW_Nacionales (Abajo)
interface GigabitEthernet1/1/1
 no switchport
 ip address 192.168.20.1 255.255.255.252
 no shutdown
 exit

! 3. Interfaz hacia MSW_Link (Derecha)
interface GigabitEthernet1/1/2
 no switchport
 ip address 192.168.20.10 255.255.255.252
 no shutdown
 exit

! 4. Configuración BGP
router bgp 100
 bgp log-neighbor-changes
 neighbor 192.168.20.2 remote-as 200
 neighbor 192.168.20.9 remote-as 300
 exit

end
write memory
```

#### 7.1.2 Configuración MSW_Nacionales (AS 200)

```bash
enable
configure terminal
hostname MSW_Nacionales

! 1. Activar funciones de ruteo
ip routing

! 2. Interfaz hacia MSW_Telecom (Arriba Izquierda)
interface GigabitEthernet1/1/1
 no switchport
 ip address 192.168.20.2 255.255.255.252
 no shutdown
 exit

! 3. Interfaz hacia MSW_Link (Arriba Derecha)
interface GigabitEthernet1/1/2
 no switchport
 ip address 192.168.20.5 255.255.255.252
 no shutdown
 exit

! 4. Configuración BGP
router bgp 200
 bgp log-neighbor-changes
 neighbor 192.168.20.1 remote-as 100
 neighbor 192.168.20.6 remote-as 300
 exit

end
write memory
```

#### 7.1.3 Configuración MSW_Link (AS 300)

```bash
enable
configure terminal
hostname MSW_Link

! 1. Activar funciones de ruteo
ip routing

! 2. Interfaz hacia MSW_Nacionales (Abajo)
interface GigabitEthernet1/1/1
 no switchport
 ip address 192.168.20.6 255.255.255.252
 no shutdown
 exit

! 3. Interfaz hacia MSW_Telecom (Izquierda)
interface GigabitEthernet1/1/2
 no switchport
 ip address 192.168.20.9 255.255.255.252
 no shutdown
 exit

! 4. Configuración BGP
router bgp 300
 bgp log-neighbor-changes
 neighbor 192.168.20.10 remote-as 100
 neighbor 192.168.20.5 remote-as 200
 exit

end
write memory
```

### 7.2 Configuraciones de Dispositivos de Telecom Uno (ISP 1)

#### 7.2.1 Configuración de Integración OSPF-BGP en MSW_Telecom
*Nota: Siguiendo el orden de este manual, este equipo ya tiene configurado BGP, ahora le agregaremos OSPF para que conozca su red interna.*

```bash
enable
configure terminal

! 1. Configurar la interfaz que baja hacia el árbol
interface GigabitEthernet1/0/24
 no switchport
 ip address 172.16.10.129 255.255.255.252
 no shutdown
 exit

! 2. Levantar OSPF
router ospf 1
 network 172.16.10.128 0.0.0.3 area 0
 exit

! 3. Redistribuir las rutas de OSPF hacia BGP para que el país conozca a Telecom Uno
router bgp 100
 redistribute ospf 1
 exit

end
write memory
```

#### 7.2.2 Configuración R_Raiz_T1

```bash
enable
configure terminal
hostname R_Raiz_T1

! 1. Interfaz hacia MSW_Telecom (Arriba)
interface GigabitEthernet0/0/0
 ip address 172.16.10.130 255.255.255.252
 no shutdown
 exit

! 2. Interfaz hacia MSW_Dist_T1 (Abajo)
interface GigabitEthernet0/0/1
 ip address 172.16.10.133 255.255.255.252
 no shutdown
 exit

! 3. Configuración OSPF
router ospf 1
 network 172.16.10.128 0.0.0.3 area 0
 network 172.16.10.132 0.0.0.3 area 0
 exit

end
write memory
```

#### 7.2.3 Configuración MSW_Dist_T1 (Distribución y Gateway)

```bash
enable
configure terminal
hostname MSW_Dist_T1

! 1. Activar enrutamiento
ip routing

! 2. Interfaz enrutada hacia R_Raiz_T1 (Arriba)
interface GigabitEthernet1/0/1
 no switchport
 ip address 172.16.10.134 255.255.255.252
 no shutdown
 exit

! 3. Crear las VLANs de los departamentos
vlan 10
 name Administracion
vlan 20
 name Atencion_Cliente
 exit

! 4. Crear los Gateways (SVIs) para cada departamento
interface Vlan10
 ip address 172.16.10.1 255.255.255.192
 no shutdown
 exit

interface Vlan20
 ip address 172.16.10.65 255.255.255.192
 no shutdown
 exit

! 5. LACP Hacia MSW_Admin (Port-Channel 1)
interface range GigabitEthernet1/0/2 - 3
 channel-protocol lacp
 channel-group 1 mode active
 switchport mode trunk
 exit

! 6. LACP Hacia MSW_Atencion (Port-Channel 2)
interface range GigabitEthernet1/0/4 - 5
 channel-protocol lacp
 channel-group 2 mode active
 switchport mode trunk
 exit

! 7. Configuración OSPF (Anunciamos toda la red de Telecom Uno)
router ospf 1
 network 172.16.10.0 0.0.0.255 area 0
 exit

end
write memory
```

#### 7.2.4 Configuración MSW_Admin (Capa 2 y Servidores)

```bash
enable
configure terminal
hostname MSW_Admin

! 1. Crear la VLAN localmente
vlan 10
 name Administracion
 exit

! 2. LACP Hacia MSW_Dist_T1 (Port-Channel 1)
interface range GigabitEthernet1/0/2 - 3
 channel-protocol lacp
 channel-group 1 mode passive
 switchport mode trunk
 exit

! 3. Puertos de acceso para Servidores DNS y HTTP
interface range GigabitEthernet1/0/10 - 11
 switchport mode access
 switchport access vlan 10
 exit

! 4. Puertos de acceso para PCs (Ej. Puertos 15 al 20)
interface range GigabitEthernet1/0/15 - 20
 switchport mode access
 switchport access vlan 10
 exit

end
write memory
```

#### 7.2.5 Configuración MSW_Atencion (Capa 2)

```bash
enable
configure terminal
hostname MSW_Atencion

! 1. Crear la VLAN localmente
vlan 20
 name Atencion_Cliente
 exit

! 2. LACP Hacia MSW_Dist_T1 (Port-Channel 2)
interface range GigabitEthernet1/0/4 - 5
 channel-protocol lacp
 channel-group 2 mode passive
 switchport mode trunk
 exit

! 3. Puertos de acceso para PCs (Ej. Puertos 15 al 20)
interface range GigabitEthernet1/0/15 - 20
 switchport mode access
 switchport access vlan 20
 exit

end
write memory
```

---


### 7.3 Configuración de Servicios en Telecom Uno (DNS y HTTP)

> *Nota: Estas configuraciones se realizan a través de la Interfaz Gráfica (GUI) de los dispositivos genéricos "Server-PT" en Cisco Packet Tracer.*

#### 7.3.1 Configuración de IP Estática (Ambos Servidores)
Debido a que estos servidores proveerán servicios críticos a toda la topología, requieren direccionamiento IP estático dentro de la VLAN 10 (Administración).

**Ruta:** Pestaña `Desktop` -> `IP Configuration`.

**Servidor DNS:**
- **IPv4 Address:** `172.16.10.2`
- **Subnet Mask:** `255.255.255.192`
- **Default Gateway:** `172.16.10.1` (IP de la SVI Vlan 10 en MSW_Dist_T1)

**Servidor HTTP:**
- **IPv4 Address:** `172.16.10.3`
- **Subnet Mask:** `255.255.255.192`
- **Default Gateway:** `172.16.10.1`

---

#### 7.3.2 Configuración del Servicio HTTP
El servidor HTTP alojará la página web solicitada en los requerimientos del proyecto.

1. Ir a la pestaña `Services` -> `HTTP`.
2. Asegurarse de que tanto **HTTP** como **HTTPS** estén en estado **ON**.
3. En el archivo `index.html`, hacer clic en **(edit)** e insertar el siguiente código:

```html
<html>
<center>
  <h1>Proyecto 2: Redes de Computadoras 2</h1>
  <h2>Estudiante: Enner Esaí Mendizabal Castro</h2>
  <h3>Carné: 202302220</h3>
  <p>Bienvenidos a la intranet de Telecom Uno, Redes Nacionales y Link Global.</p>
</center>
</html>
```

4. Clic en **Save** y confirmar la sobreescritura.

---

#### 7.3.3 Configuración del Servicio DNS
El servidor DNS resolverá el dominio solicitado para toda la red hacia la IP del servidor web.

1. Ir a la pestaña `Services` -> `DNS`.
2. Encender el servicio (**DNS Service: ON**).
3. Configurar el registro **A Record**:
   - **Name:** `www.proyecto2_202302220.com`
   - **Type:** A Record
   - **Address:** `172.16.10.3` (Dirección del Servidor HTTP)
4. Clic en **Add** para guardar el registro.

---
