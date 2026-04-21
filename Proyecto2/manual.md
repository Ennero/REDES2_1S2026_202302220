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
  - [4.1 Tabla de conexiones de la Red de Telecom](#41-tabla-de-conexiones-de-la-red-de-telecom)
  - [4.2 Tabla de conexiones de Redes Nacionales (Jerárquico)](#42-tabla-de-conexiones-de-redes-nacionales-jerárquico)
  - [4.3 Tabla de conexiones de Link Global (Hub and Spoke)](#43-tabla-de-conexiones-de-link-global-hub-and-spoke)
  - [5. Tabla de Direccionamiento IP](#5-tabla-de-direccionamiento-ip)
  - [6. Subnetting](#6-subnetting)
    - [6.1 VLANs de Departamentos (VLSM /26)](#61-vlans-de-departamentos-vlsm-26)
    - [6.2 Enlaces de Enrutamiento (Punto a Punto /30)](#62-enlaces-de-enrutamiento-punto-a-punto-30)
  - [7. Configuraciones de Dispositivos](#7-configuraciones-de-dispositivos)
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
    - [7.4 Configuraciones de Dispositivos de Redes Nacionales (ISP 2)](#74-configuraciones-de-dispositivos-de-redes-nacionales-isp-2)
      - [7.4.1 Configuración de Integración OSPF-BGP en MSW\_Nacionales](#741-configuración-de-integración-ospf-bgp-en-msw_nacionales)
      - [7.4.2 Configuración MSW\_Core\_RN (Core y DHCP Gateway)](#742-configuración-msw_core_rn-core-y-dhcp-gateway)
      - [7.4.3 Configuración MSW\_Dist\_1 (Distribución Primario)](#743-configuración-msw_dist_1-distribución-primario)
      - [7.4.4 Configuración MSW\_Dist\_2 (Distribución Secundario)](#744-configuración-msw_dist_2-distribución-secundario)
      - [7.4.5 Configuración de Switches de Acceso (SW\_Ventas y SW\_Facturacion)](#745-configuración-de-switches-de-acceso-sw_ventas-y-sw_facturacion)
        - [SW\_Ventas](#sw_ventas)
        - [SW\_Facturacion](#sw_facturacion)
      - [7.4.6 Configuración del Servidor DHCP (GUI)](#746-configuración-del-servidor-dhcp-gui)
    - [7.5 Configuraciones de Dispositivos de Link Global (ISP 3)](#75-configuraciones-de-dispositivos-de-link-global-isp-3)
      - [7.5.1 Configuración de Integración EIGRP-BGP en MSW\_Link](#751-configuración-de-integración-eigrp-bgp-en-msw_link)
      - [7.5.2 Configuración R\_Hub\_LG (Centro de la Estrella)](#752-configuración-r_hub_lg-centro-de-la-estrella)
      - [7.5.3 Configuración MSW\_Soporte\_1 (Spoke 1 - Inicio)](#753-configuración-msw_soporte_1-spoke-1---inicio)
      - [7.5.4 Configuración MSW\_Soporte\_2 (Spoke 1 - Final y Gateway)](#754-configuración-msw_soporte_2-spoke-1---final-y-gateway)
      - [7.5.5 Configuración R\_Seguridad (Spoke 2)](#755-configuración-r_seguridad-spoke-2)
      - [7.5.6 Configuración Router Inalámbrico y Actualización DHCP](#756-configuración-router-inalámbrico-y-actualización-dhcp)
  - [8. Listas de Control de Acceso (ACLs)](#8-listas-de-control-de-acceso-acls)
    - [8.1 ACL en Telecom Uno (Se aplica en MSW\_Dist\_T1)](#81-acl-en-telecom-uno-se-aplica-en-msw_dist_t1)
    - [8.2 ACLs en Redes Nacionales (Se aplica en MSW\_Dist\_1 y MSW\_Dist\_2)](#82-acls-en-redes-nacionales-se-aplica-en-msw_dist_1-y-msw_dist_2)
    - [8.3 ACL en Link Global (Se aplica en R\_Seguridad)](#83-acl-en-link-global-se-aplica-en-r_seguridad)
  - [9. Lista de Precios de Hardware (Cotización Estimada)](#9-lista-de-precios-de-hardware-cotización-estimada)
  - [10. Comandos de Verificación](#10-comandos-de-verificación)
    - [10.1 Verificación de Enrutamiento (BGP, OSPF, EIGRP)](#101-verificación-de-enrutamiento-bgp-ospf-eigrp)
    - [10.2 Verificación de Alta Disponibilidad y Agregación](#102-verificación-de-alta-disponibilidad-y-agregación)
    - [10.3 Verificación de Servicios y Seguridad](#103-verificación-de-servicios-y-seguridad)
    - [10.4 Pruebas de Extremo a Extremo](#104-pruebas-de-extremo-a-extremo)
- [10. Comandos de Verificación](#10-comandos-de-verificación-1)

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

> [Insertar Diagrama de Topología de Red Simulado]

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



## 4.1 Tabla de conexiones de la Red de Telecom

| Origen       | Puerto Origen          | Destino       | Puerto Destino         | Cable          |
|--------------|------------------------|---------------|------------------------|----------------|
| MSW_Telecom  | Gi1/0/24               | R_Raiz_T1     | Gi0/0/0                | Cobre Directo  |
| R_Raiz_T1    | Gi0/0/1                | MSW_Dist_T1   | Gi1/0/1                | Cobre Directo  |
| MSW_Dist_T1  | Gi1/0/2 y Gi1/0/3      | MSW_Admin     | Gi1/0/2 y Gi1/0/3      | Cobre Cruzado  |
| MSW_Dist_T1  | Gi1/0/4 y Gi1/0/5      | MSW_Atencion  | Gi1/0/4 y Gi1/0/5      | Cobre Cruzado  |
| MSW_Admin    | Gi1/0/10               | Servidor DNS  | Fa0                    | Cobre Directo  |
| MSW_Admin    | Gi1/0/11               | Servidor HTTP | Fa0                    | Cobre Directo  |
| **MSW_Admin** | Gi1/0/15 | **PC_Admin_1** | Fa0 | Cobre Directo |
| **MSW_Atencion** | Gi1/0/15 | **PC_Atencion_1** | Fa0 | Cobre Directo |
| **MSW_Atencion** | Gi1/0/16 | **PC_Atencion_2** | Fa0 | Cobre Directo |






## 4.2 Tabla de conexiones de Redes Nacionales (Jerárquico)


| Origen | Puerto Origen | Destino | Puerto Destino | Cable | Observación |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **MSW_Nacionales** | Gi1/0/24 | **MSW_Core_RN** | Gi1/0/1 | Cobre Cruzado | Conexión hacia ISP 2 |
| **MSW_Core_RN** | **Gi1/0/2 y Gi1/0/3** | **MSW_Dist_1** | **Gi1/0/2 y Gi1/0/3** | Cobre Cruzado | **Enlace LACP 1** |
| **MSW_Core_RN** | **Gi1/0/4 y Gi1/0/5** | **MSW_Dist_2** | **Gi1/0/4 y Gi1/0/5** | Cobre Cruzado | **Enlace LACP 2** |
| **MSW_Dist_1** | Gi1/0/10 | **SW_Ventas** | Gi0/1 | Cobre Directo | Enlace redundante 1 |
| **MSW_Dist_2** | Gi1/0/10 | **SW_Ventas** | Gi0/2 | Cobre Directo | Enlace redundante 2 |
| **MSW_Dist_1** | Gi1/0/11 | **SW_Facturacion** | Gi0/1 | Cobre Directo | Enlace redundante 1 |
| **MSW_Dist_2** | Gi1/0/11 | **SW_Facturacion** | Gi0/2 | Cobre Directo | Enlace redundante 2 |
| **MSW_Core_RN** | Gi1/0/20 | **Servidor DHCP** | Fa0 | Cobre Directo | Host de servicio DHCP |
| **SW_Ventas** | Fa0/1 | **PC0** | Fa0 | Cobre Directo | Host Ventas |
| **SW_Ventas** | Fa0/2 | **PC1** | Fa0 | Cobre Directo | Host Ventas |
| **SW_Ventas** | Fa0/3 | **Laptop1** | Fa0 | Cobre Directo | Host Ventas |
| **SW_Facturacion** | Fa0/1 | **PC2** | Fa0 | Cobre Directo | Host Facturación |
| **SW_Facturacion** | Fa0/2 | **Laptop0** | Fa0 | Cobre Directo | Host Facturación |

## 4.3 Tabla de conexiones de Link Global (Hub and Spoke)

| Origen | Puerto Origen | Destino | Puerto Destino | Cable | Observación |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **MSW_Link** | Gi1/0/24 | **R_Hub_LG** | Gi0/0/0 | Cobre Directo | Conexión hacia el ISP 3 |
| **R_Hub_LG** | Gi0/0/1 | **MSW_Soporte_1** | Gi1/0/1 | Cobre Directo | Enlace enrutado (Spoke 1) |
| **MSW_Soporte_1** | **Gi1/0/2 y Gi1/0/3** | **MSW_Soporte_2** | **Gi1/0/2 y Gi1/0/3** | Cobre Cruzado | **Enlace LACP 1** |
| **MSW_Soporte_1** | **Gi1/0/4 y Gi1/0/5** | **MSW_Soporte_2** | **Gi1/0/4 y Gi1/0/5** | Cobre Cruzado | **Enlace LACP 2** |
| **R_Hub_LG** | Gi0/0/2 | **R_Seguridad** | Gi0/0/0 | Cobre Cruzado | Enlace enrutado (Spoke 2) |
| **R_Seguridad** | Gi0/0/1 | **Router_WiFi** | Internet (WAN) | Cobre Directo | Conexión a red inalámbrica |
| **MSW_Soporte_2** | Gi1/0/10 | **PC3** | Fa0 | Cobre Directo | Host Soporte |
| **MSW_Soporte_2** | Gi1/0/11 | **PC4** | Fa0 | Cobre Directo | Host Soporte |

---

## 5. Tabla de Direccionamiento IP

| Dispositivo / Red | Interfaz | Dirección IP | Máscara | Gateway |
| :--- | :--- | :--- | :--- | :--- |
| **Core: T1 ↔ RN** | Enlace BGP (Gi1/1/1) | 192.168.20.1 | 255.255.255.252 | N/A |
| **Core: RN ↔ LG** | Enlace BGP (Gi1/1/2) | 192.168.20.5 | 255.255.255.252 | N/A |
| **Core: LG ↔ T1** | Enlace BGP (Gi1/1/2) | 192.168.20.9 | 255.255.255.252 | N/A |
| **MSW_Telecom ↔ R_Raiz_T1** | Gi1/0/24 ↔ Gi0/0/0 | 172.16.10.129 ↔ .130| 255.255.255.252 | N/A |
| **R_Raiz_T1 ↔ MSW_Dist_T1** | Gi0/0/1 ↔ Gi1/0/1 | 172.16.10.133 ↔ .134| 255.255.255.252 | N/A |
| **MSW_Core_RN ↔ MSW_Dist_1**| Po1 ↔ Po1 | 172.16.20.137 ↔ .138| 255.255.255.252 | N/A |
| **MSW_Core_RN ↔ MSW_Dist_2**| Po2 ↔ Po2 | 172.16.20.141 ↔ .142| 255.255.255.252 | N/A |
| **SVI: Admin (T1)** | VLAN 10 (MSW_Dist) | 172.16.10.1 | 255.255.255.192 | N/A |
| **SVI: Ventas (RN)** | VLAN 30 | 172.16.20.1 | 255.255.255.192 | 172.16.20.1 (VIP) |
| **SVI: Facturacion (RN)** | VLAN 40 | 172.16.20.65 | 255.255.255.192 | 172.16.20.65 (VIP) |
| **Servidor DHCP** | Fa0 | 172.16.20.134 | 255.255.255.252 | 172.16.20.133 |
| **MSW_Link ↔ R_Hub_LG** | Gi1/0/24 ↔ Gi0/0/0 | 172.16.32.129 ↔ .130| 255.255.255.252 | N/A |
| **R_Hub_LG ↔ MSW_Soporte_1** | Gi0/0/1 ↔ Gi1/0/1 | 172.16.32.133 ↔ .134| 255.255.255.252 | N/A |
| **R_Hub_LG ↔ R_Seguridad** | Gi0/0/2 ↔ Gi0/0/0 | 172.16.32.145 ↔ .146| 255.255.255.252 | N/A |
| **MSW_Soporte_1 ↔ MSW_Soporte_2**| Po1 ↔ Po1 | 172.16.32.137 ↔ .138| 255.255.255.252 | N/A |
| **MSW_Soporte_1 ↔ MSW_Soporte_2**| Po2 ↔ Po2 | 172.16.32.141 ↔ .142| 255.255.255.252 | N/A |
| **SVI: Soporte (LG)** | VLAN 50 | 172.16.32.1 | 255.255.255.192 | N/A |
| **Gateway Seguridad (LG)** | Gi0/0/1 (R_Seg) | 172.16.32.65 | 255.255.255.192 | N/A |
| **Router_WiFi (WAN)** | Internet | 172.16.32.66 | 255.255.255.192 | 172.16.32.65 |








---




## 6. Subnetting

El esquema de direccionamiento se calculó utilizando VLSM (Variable Length Subnet Mask) para optimizar el uso de los prefijos asignados en el proyecto. 

### 6.1 VLANs de Departamentos (VLSM /26)

Las redes base asignadas a cada ISP se dividieron para soportar un mínimo de 60 hosts por departamento, requiriendo una máscara `/26` (62 hosts utilizables).

| ISP | Red Base | VLAN / Departamento | ID Red | Máscara | Wildcard | Primer Host | Último Host | Broadcast |
|:---:|:---|:---|:---:|:---:|:---:|:---:|:---:|:---:|
| 1 | 172.16.10.0/24 | 10 - Administración | 172.16.10.0 | 255.255.255.192 | 0.0.0.63 | 172.16.10.1 | 172.16.10.62 | 172.16.10.63 |
| 1 | 172.16.10.0/24 | 20 - Atención al C. | 172.16.10.64 | 255.255.255.192 | 0.0.0.63 | 172.16.10.65 | 172.16.10.126 | 172.16.10.127 |
| 2 | 172.16.20.0/24 | 30 - Ventas | 172.16.20.0 | 255.255.255.192 | 0.0.0.63 | 172.16.20.1 | 172.16.20.62 | 172.16.20.63 |
| 2 | 172.16.20.0/24 | 40 - Facturación | 172.16.20.64 | 255.255.255.192 | 0.0.0.63 | 172.16.20.65 | 172.16.20.126 | 172.16.20.127 |
| 3 | 172.16.32.0/24 | 50 - Soporte | 172.16.32.0 | 255.255.255.192 | 0.0.0.63 | 172.16.32.1 | 172.16.32.62 | 172.16.32.63 |
| 3 | 172.16.32.0/24 | 60 - Seguridad | 172.16.32.64 | 255.255.255.192 | 0.0.0.63 | 172.16.32.65 | 172.16.32.126 | 172.16.32.127 |

### 6.2 Enlaces de Enrutamiento (Punto a Punto /30)

Se aprovecharon los remanentes de las redes `/24` de cada ISP, junto con la red central `192.168.20.0/16`, para subnetear enlaces con máscara `/30` (2 IPs útiles), erradicando el desperdicio de direcciones IP en las adyacencias de los protocolos de enrutamiento (OSPF, EIGRP, BGP).

| Dispositivo A | Dispositivo B | Protocolo | ID RED | MÁSCARA | PRIMER HOST | ÚLTIMO HOST | BROADCAST |
|:---|:---|:---:|:---:|:---:|:---:|:---:|:---:|
| MSW_Telecom | MSW_Nacionales | BGP | 192.168.20.0 | 255.255.255.252 | 192.168.20.1 | 192.168.20.2 | 192.168.20.3 |
| MSW_Nacionales | MSW_Link | BGP | 192.168.20.4 | 255.255.255.252 | 192.168.20.5 | 192.168.20.6 | 192.168.20.7 |
| MSW_Link | MSW_Telecom | BGP | 192.168.20.8 | 255.255.255.252 | 192.168.20.9 | 192.168.20.10 | 192.168.20.11 |
| MSW_Telecom | R_Raiz_T1 | OSPF | 172.16.10.128 | 255.255.255.252 | 172.16.10.129 | 172.16.10.130 | 172.16.10.131 |
| R_Raiz_T1 | MSW_Dist_T1 | OSPF | 172.16.10.132 | 255.255.255.252 | 172.16.10.133 | 172.16.10.134 | 172.16.10.135 |
| MSW_Nacionales | MSW_Core_RN | OSPF | 172.16.20.128 | 255.255.255.252 | 172.16.20.129 | 172.16.20.130 | 172.16.20.131 |
| MSW_Core_RN | Servidor DHCP | Estático | 172.16.20.132 | 255.255.255.252 | 172.16.20.133 | 172.16.20.134 | 172.16.20.135 |
| MSW_Core_RN | MSW_Dist_1 (LACP) | OSPF | 172.16.20.136 | 255.255.255.252 | 172.16.20.137 | 172.16.20.138 | 172.16.20.139 |
| MSW_Core_RN | MSW_Dist_2 (LACP) | OSPF | 172.16.20.140 | 255.255.255.252 | 172.16.20.141 | 172.16.20.142 | 172.16.20.143 |
| MSW_Link | R_Hub_LG | EIGRP | 172.16.32.128 | 255.255.255.252 | 172.16.32.129 | 172.16.32.130 | 172.16.32.131 |
| R_Hub_LG | MSW_Soporte_1 | EIGRP | 172.16.32.132 | 255.255.255.252 | 172.16.32.133 | 172.16.32.134 | 172.16.32.135 |
| R_Hub_LG | R_Seguridad | EIGRP | 172.16.32.144 | 255.255.255.252 | 172.16.32.145 | 172.16.32.146 | 172.16.32.147 |
| MSW_Sop_1 | MSW_Sop_2 (LACP 1)| EIGRP | 172.16.32.136 | 255.255.255.252 | 172.16.32.137 | 172.16.32.138 | 172.16.32.139 |
| MSW_Sop_1 | MSW_Sop_2 (LACP 2)| EIGRP | 172.16.32.140 | 255.255.255.252 | 172.16.32.141 | 172.16.32.142 | 172.16.32.143 |






---

## 7. Configuraciones de Dispositivos

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
> *Nota: Adición de enrutamiento OSPF al equipo con configuración BGP previa para anunciar rutas internas.*

```bash
enable
configure terminal

! 1. Configurar la interfaz que baja hacia el árbol
interface GigabitEthernet1/0/24
 no switchport
 ip address 172.16.10.129 255.255.255.252
 no shutdown
 exit

! 2. Levantar OSPF y Redistribuir BGP hacia adentro (Efecto Espejo)
router ospf 1
 network 172.16.10.128 0.0.0.3 area 0
 redistribute bgp 100 subnets
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
 ip helper-address 172.16.20.134
 no shutdown
 exit

interface Vlan20
 ip address 172.16.10.65 255.255.255.192
 ip helper-address 172.16.20.134
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

> *Nota: Estas configuraciones aplican en el entorno gráfico (GUI) para los dispositivos "Server-PT" implementados en la topología.*

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

> *Nota: Una vez finalizados estos pasos, cualquier host cliente (PC) en la topología con la IP y el servidor DNS configurados correctamente, podrá acceder a la página web ingresando a su navegador (Web Browser) mediante la URL: [http://www.proyecto2_202302220.com](http://www.proyecto2_202302220.com).*

---


### 7.4 Configuraciones de Dispositivos de Redes Nacionales (ISP 2)

#### 7.4.1 Configuración de Integración OSPF-BGP en MSW_Nacionales
> *Nota: Consolidación OSPF para publicar redes locales del esquema Jerárquico en las adyacencias BGP previamente configuradas.*

```bash
enable
configure terminal

! 1. Configurar la interfaz que baja hacia el Core de Redes Nacionales
interface GigabitEthernet1/0/24
 no switchport
 ip address 172.16.20.129 255.255.255.252
 no shutdown
 exit

! 2. Levantar OSPF y Redistribuir BGP hacia adentro (Efecto Espejo)
router ospf 1
 network 172.16.20.128 0.0.0.3 area 0
 redistribute bgp 200 subnets
 exit

! 3. Redistribuir las rutas de OSPF hacia BGP
router bgp 200
 redistribute ospf 1
 exit

end
write memory
```




#### 7.4.2 Configuración MSW_Core_RN (Core y DHCP Gateway)


```bash
enable
configure terminal
hostname MSW_Core_RN

! 1. Activar enrutamiento
ip routing

! 2. Interfaz hacia MSW_Nacionales (Arriba)
interface GigabitEthernet1/0/1
 no switchport
 ip address 172.16.20.130 255.255.255.252
 no shutdown
 exit

! 3. Interfaz hacia Servidor DHCP
interface GigabitEthernet1/0/20
 no switchport
 ip address 172.16.20.133 255.255.255.252
 no shutdown
 exit

! 4. LACP Hacia MSW_Dist_1 (Port-Channel 1 - Capa 3)
interface range GigabitEthernet1/0/2 - 3
 no switchport
 channel-protocol lacp
 channel-group 1 mode active
 exit

interface Port-channel 1
 ip address 172.16.20.137 255.255.255.252
 no shutdown
 exit

! 5. LACP Hacia MSW_Dist_2 (Port-Channel 2 - Capa 3)
interface range GigabitEthernet1/0/4 - 5
 no switchport
 channel-protocol lacp
 channel-group 2 mode active
 exit

interface Port-channel 2
 ip address 172.16.20.141 255.255.255.252
 no shutdown
 exit

! 6. Configuración OSPF
router ospf 1
 network 172.16.20.0 0.0.0.255 area 0
 exit

end
write memory
```





#### 7.4.3 Configuración MSW_Dist_1 (Distribución Primario)


```bash
enable
configure terminal
hostname MSW_Dist_1

! 1. Activar enrutamiento
ip routing

! 2. Crear VLANs
vlan 30
 name Ventas
vlan 40
 name Facturacion
 exit

! 3. LACP Hacia MSW_Core_RN (Port-Channel 1 - Capa 3)
interface range GigabitEthernet1/0/2 - 3
 no switchport
 channel-protocol lacp
 channel-group 1 mode passive
 exit

interface Port-channel 1
 ip address 172.16.20.138 255.255.255.252
 no shutdown
 exit

! 4. Enlaces troncales hacia switches de acceso
interface range GigabitEthernet1/0/10 - 11
 switchport mode trunk
 exit

! 5. Configurar SVIs y HSRP (Prioridad 110 = Activo)
interface Vlan30
 ip address 172.16.20.2 255.255.255.192
 ip helper-address 172.16.20.134
 standby 30 ip 172.16.20.1
 standby 30 priority 110
 standby 30 preempt
 no shutdown
 exit

interface Vlan40
 ip address 172.16.20.66 255.255.255.192
 ip helper-address 172.16.20.134
 standby 40 ip 172.16.20.65
 standby 40 priority 110
 standby 40 preempt
 no shutdown
 exit

! 6. OSPF
router ospf 1
 network 172.16.20.0 0.0.0.255 area 0
 exit

end
write memory
```

#### 7.4.4 Configuración MSW_Dist_2 (Distribución Secundario)


```bash
enable
configure terminal
hostname MSW_Dist_2

! 1. Activar enrutamiento
ip routing

! 2. Crear VLANs
vlan 30
 name Ventas
vlan 40
 name Facturacion
 exit

! 3. LACP Hacia MSW_Core_RN (Port-Channel 2 - Capa 3)
interface range GigabitEthernet1/0/4 - 5
 no switchport
 channel-protocol lacp
 channel-group 2 mode passive
 exit

interface Port-channel 2
 ip address 172.16.20.142 255.255.255.252
 no shutdown
 exit

! 4. Enlaces troncales hacia switches de acceso
interface range GigabitEthernet1/0/10 - 11
 switchport mode trunk
 exit

! 5. Configurar SVIs y HSRP (Prioridad 90 = Standby)
interface Vlan30
 ip address 172.16.20.3 255.255.255.192
 ip helper-address 172.16.20.134
 standby 30 ip 172.16.20.1
 standby 30 priority 90
 standby 30 preempt
 no shutdown
 exit

interface Vlan40
 ip address 172.16.20.67 255.255.255.192
 ip helper-address 172.16.20.134
 standby 40 ip 172.16.20.65
 standby 40 priority 90
 standby 40 preempt
 no shutdown
 exit

! 6. OSPF
router ospf 1
 network 172.16.20.0 0.0.0.255 area 0
 exit

end
write memory
```


#### 7.4.5 Configuración de Switches de Acceso (SW_Ventas y SW_Facturacion)

A continuación, la configuración respectiva para los switches de capa de acceso de cada departamento:

##### SW_Ventas

```bash
enable
configure terminal
hostname SW_Ventas

vlan 30
 name Ventas
 exit

interface range GigabitEthernet0/1 - 2
 switchport mode trunk
 exit

interface range FastEthernet0/1 - 3
 switchport mode access
 switchport access vlan 30
 exit

end
write memory

```



##### SW_Facturacion

```bash

enable
configure terminal
hostname SW_Facturacion

vlan 40
 name Facturacion
 exit

interface range GigabitEthernet0/1 - 2
 switchport mode trunk
 exit

interface range FastEthernet0/1 - 2
 switchport mode access
 switchport access vlan 40
 exit

end
write memory

```

#### 7.4.6 Configuración del Servidor DHCP (GUI)
1. Ve al Servidor DHCP. En la pestaña **Desktop -> IP Configuration**, ponle IP estática `172.16.20.134`, máscara `255.255.255.252` y Gateway `172.16.20.133`.
2. Ve a **Services -> DHCP**, asegúrate de que esté en **ON** y crea los siguientes pools (basados en tus subredes de la Sección 6):
   * **Pool Ventas:** Gateway `172.16.20.1`, DNS Server `172.16.10.2`, Start IP `172.16.20.4`, Mask `255.255.255.192`.
   * **Pool Facturación:** Gateway `172.16.20.65`, DNS Server `172.16.10.2`, Start IP `172.16.20.68`, Mask `255.255.255.192`.
   * **Pool Admin (T1):** Gateway `172.16.10.1`, DNS Server `172.16.10.2`, Start IP `172.16.10.4`, Mask `255.255.255.192`.
   * **Pool Atencion (T1):** Gateway `172.16.10.65`, DNS Server `172.16.10.2`, Start IP `172.16.10.66`, Mask `255.255.255.192`.

> *Nota: Corroborar la creación de los pools mediante el botón **Add** de la interfaz. Finalizado el proceso, configure las interfaces de red de los equipos clientes (PCs) para habilitar DHCP y corroborar la obtención dinámica de la dirección IP.*

### 7.5 Configuraciones de Dispositivos de Link Global (ISP 3)

#### 7.5.1 Configuración de Integración EIGRP-BGP en MSW_Link
> *Nota: Inyección de rutas de métrica EIGRP hacia el dominio externo BGP en este dispositivo frontera.*

```bash

enable
configure terminal

! 1. Configurar la interfaz que baja hacia el Hub
interface GigabitEthernet1/0/24
 no switchport
 ip address 172.16.32.129 255.255.255.252
 no shutdown
 exit

! 2. Levantar EIGRP y Redistribuir BGP hacia adentro con métricas semilla
router eigrp 1
 network 172.16.32.128 0.0.0.3
 redistribute bgp 300 metric 1000000 1 255 1 1500
 exit

! 3. Redistribuir EIGRP hacia BGP
router bgp 300
 redistribute eigrp 1
 exit

end
write memory

```


#### 7.5.2 Configuración R_Hub_LG (Centro de la Estrella)


```bash

enable
configure terminal
hostname R_Hub_LG

! 1. Interfaz hacia MSW_Link (Arriba)
interface GigabitEthernet0/0/0
 ip address 172.16.32.130 255.255.255.252
 no shutdown
 exit

! 2. Interfaz hacia Spoke 1 (MSW_Soporte_1)
interface GigabitEthernet0/0/1
 ip address 172.16.32.133 255.255.255.252
 no shutdown
 exit

! 3. Interfaz hacia Spoke 2 (R_Seguridad)
interface GigabitEthernet0/0/2
 ip address 172.16.32.145 255.255.255.252
 no shutdown
 exit

! 4. Configuración EIGRP
router eigrp 1
 network 172.16.32.128 0.0.0.3
 network 172.16.32.132 0.0.0.3
 network 172.16.32.144 0.0.0.3
 exit

end
write memory

```



#### 7.5.3 Configuración MSW_Soporte_1 (Spoke 1 - Inicio)


```bash

enable
configure terminal
hostname MSW_Soporte_1
ip routing

interface GigabitEthernet1/0/1
 no switchport
 ip address 172.16.32.134 255.255.255.252
 no shutdown
 exit

! LACP 1 (Capa 3)
interface range GigabitEthernet1/0/2 - 3
 no switchport
 channel-protocol lacp
 channel-group 1 mode active
 exit

interface Port-channel 1
 ip address 172.16.32.137 255.255.255.252
 no shutdown
 exit

! LACP 2 (Capa 3)
interface range GigabitEthernet1/0/4 - 5
 no switchport
 channel-protocol lacp
 channel-group 2 mode active
 exit

interface Port-channel 2
 ip address 172.16.32.141 255.255.255.252
 no shutdown
 exit

router eigrp 1
 network 172.16.32.132 0.0.0.3
 network 172.16.32.136 0.0.0.3
 network 172.16.32.140 0.0.0.3
 exit

end
write memory

```



#### 7.5.4 Configuración MSW_Soporte_2 (Spoke 1 - Final y Gateway)

```bash

enable
configure terminal
hostname MSW_Soporte_2
ip routing

! LACP 1 (Capa 3)
interface range GigabitEthernet1/0/2 - 3
 no switchport
 channel-protocol lacp
 channel-group 1 mode passive
 exit

interface Port-channel 1
 ip address 172.16.32.138 255.255.255.252
 no shutdown
 exit

! LACP 2 (Capa 3)
interface range GigabitEthernet1/0/4 - 5
 no switchport
 channel-protocol lacp
 channel-group 2 mode passive
 exit

interface Port-channel 2
 ip address 172.16.32.142 255.255.255.252
 no shutdown
 exit

! Gateway para PCs de Soporte y DHCP Relay
vlan 50
 name Soporte
 exit

interface Vlan50
 ip address 172.16.32.1 255.255.255.192
 ip helper-address 172.16.20.134
 no shutdown
 exit

! Puertos hacia las PCs de Soporte
interface range GigabitEthernet1/0/10 - 11
 switchport mode access
 switchport access vlan 50
 exit

router eigrp 1
 network 172.16.32.136 0.0.0.3
 network 172.16.32.140 0.0.0.3
 network 172.16.32.0 0.0.0.63
 exit

end
write memory

```




#### 7.5.5 Configuración R_Seguridad (Spoke 2)

```bash

enable
configure terminal
hostname R_Seguridad

! Hacia R_Hub_LG
interface GigabitEthernet0/0/0
 ip address 172.16.32.146 255.255.255.252
 no shutdown
 exit

! Hacia Router_WiFi
interface GigabitEthernet0/0/1
 ip address 172.16.32.65 255.255.255.192
 no shutdown
 exit

router eigrp 1
 network 172.16.32.144 0.0.0.3
 network 172.16.32.64 0.0.0.63
 exit

end
write memory
```



#### 7.5.6 Configuración Router Inalámbrico y Actualización DHCP

**1. Servidor DHCP Central (En Redes Nacionales):**
Configurar el direccionamiento del departamento "Soporte" insertando un nuevo `Pool` en el Servidor DHCP central (172.16.20.134):
- **Pool Name:** `PoolSoporte`
- **Default Gateway:** `172.16.32.1`
- **DNS Server:** `172.16.10.2`
- **Start IP:** `172.16.32.2`
- **Subnet Mask:** `255.255.255.192`

**2. Router Inalámbrico (Router_WiFi):**
Desde la pestaña **GUI** del dispositivo de interconexión WRT300N, aplicar la configuración base:

* **Internet Setup (WAN):** Tipo de conexión `Static IP`.
    * **IP Address:** `172.16.32.66`
    * **Subnet Mask:** `255.255.255.192`
    * **Default Gateway:** `172.16.32.65`
    * **Static DNS:** `172.16.10.2`
* **Network Setup (LAN):** Conservar la interfaz local por defecto (IP: `192.168.0.1`). Esto mantiene activo el NAT y de forma automática aísla el direccionamiento interno para seguridad en los clientes inalámbricos.
* **Guardar Cambios:** Navegar al inferior de las opciones y ejecutar *Save Settings*.

**3. Personalización de la Red Inalámbrica:**
Establecer identificadores y credenciales dedicadas para el SSID de Seguridad:
* Subsección **Wireless** -> **Basic Wireless Settings**:
    * **Network Name (SSID):** `Seguridad_202302220`.
    * Confirmar acción: *Save Settings*.
* Subsección **Wireless Security**:
    * **Security Mode:** `WPA2 Personal`
    * **Passphrase:** `redes2026`.
    * Confirmar acción: *Save Settings*.

> *Nota: Validar la conexión en terminales portátiles final (Desktop -> PC Wireless) seleccionando la red `Seguridad_202302220`.*



## 8. Listas de Control de Acceso (ACLs)

### 8.1 ACL en Telecom Uno (Se aplica en MSW_Dist_T1)
**Regla:** Atención al Cliente se comunica exclusivamente con Ventas.

```bash
enable
configure terminal

! Borrar lista anterior si existe
no ip access-list extended ACL_ATENCION

! Nueva ACL para Atención al Cliente (VLAN 20)
ip access-list extended ACL_ATENCION
 ! 1. Permitir DHCP y DNS/HTTP (Servicios vitales)
 permit udp any any eq bootps
 permit ip any host 172.16.10.2
 permit ip any host 172.16.10.3
 ! 2. Permitir responder a Admin (172.16.10.0/26)
 permit icmp 172.16.10.64 0.0.0.63 172.16.10.0 0.0.0.63 echo-reply
 permit tcp 172.16.10.64 0.0.0.63 172.16.10.0 0.0.0.63 established
 ! 3. Permitir responder a Soporte y Seguridad (Agrupados en 172.16.32.0/25)
 permit icmp 172.16.10.64 0.0.0.63 172.16.32.0 0.0.0.127 echo-reply
 permit tcp 172.16.10.64 0.0.0.63 172.16.32.0 0.0.0.127 established
 ! 4. Permitir comunicación completa hacia Ventas
 permit ip 172.16.10.64 0.0.0.63 172.16.20.0 0.0.0.63
 ! 5. Denegar tráfico hacia las demás subredes privadas
 deny ip 172.16.10.64 0.0.0.63 172.16.0.0 0.0.255.255
 permit ip any any
 exit

interface Vlan20
 ip access-group ACL_ATENCION in
 exit

end
write memory
```

---

### 8.2 ACLs en Redes Nacionales (Se aplica en MSW_Dist_1 y MSW_Dist_2)
**Regla 1:** Ventas se comunica con Facturación y Atención al Cliente.
**Regla 2:** Facturación se comunica exclusivamente con Ventas.

> *Nota: Garantizar la paridad de la configuración de acceso sobre ambos equipos en Capa de Distribución.*

```bash
enable
configure terminal

! Borrar listas anteriores
no ip access-list extended ACL_VENTAS
no ip access-list extended ACL_FACTURACION

! ACL para Ventas (VLAN 30)
ip access-list extended ACL_VENTAS
 permit udp any any eq bootps
 permit ip any host 172.16.10.2
 permit ip any host 172.16.10.3
 ! Respuestas a Admin, Soporte y Seguridad
 permit icmp 172.16.20.0 0.0.0.63 172.16.10.0 0.0.0.63 echo-reply
 permit tcp 172.16.20.0 0.0.0.63 172.16.10.0 0.0.0.63 established
 permit icmp 172.16.20.0 0.0.0.63 172.16.32.0 0.0.0.127 echo-reply
 permit tcp 172.16.20.0 0.0.0.63 172.16.32.0 0.0.0.127 established
 ! Permitir hacia Facturación y Atención
 permit ip 172.16.20.0 0.0.0.63 172.16.20.64 0.0.0.63
 permit ip 172.16.20.0 0.0.0.63 172.16.10.64 0.0.0.63
 ! Denegar resto
 deny ip 172.16.20.0 0.0.0.63 172.16.0.0 0.0.255.255
 permit ip any any
 exit

! ACL para Facturacion (VLAN 40)
ip access-list extended ACL_FACTURACION
 permit udp any any eq bootps
 permit ip any host 172.16.10.2
 permit ip any host 172.16.10.3
 ! Respuestas a Admin, Soporte y Seguridad
 permit icmp 172.16.20.64 0.0.0.63 172.16.10.0 0.0.0.63 echo-reply
 permit tcp 172.16.20.64 0.0.0.63 172.16.10.0 0.0.0.63 established
 permit icmp 172.16.20.64 0.0.0.63 172.16.32.0 0.0.0.127 echo-reply
 permit tcp 172.16.20.64 0.0.0.63 172.16.32.0 0.0.0.127 established
 ! Permitir hacia Ventas
 permit ip 172.16.20.64 0.0.0.63 172.16.20.0 0.0.0.63
 ! Denegar resto
 deny ip 172.16.20.64 0.0.0.63 172.16.0.0 0.0.255.255
 permit ip any any
 exit

! Aplicar en Gateways
interface Vlan30
 ip access-group ACL_VENTAS in
 exit
interface Vlan40
 ip access-group ACL_FACTURACION in
 exit

end
write memory
```

---

### 8.3 ACL en Link Global (Se aplica en R_Seguridad)
**Regla:** Bloqueo de entrada de cualquier red externa al departamento, permitiendo la capacidad nativa de consultar a redes externas por solicitudes originadas internamente.

```bash
enable
configure terminal

! Borrar lista anterior
no ip access-list extended ACL_NO_ENTRADA_SEG

! ACL de protección para Seguridad (172.16.32.64/26)
ip access-list extended ACL_NO_ENTRADA_SEG
 permit udp host 172.16.10.2 eq domain 172.16.32.64 0.0.0.63
 ! Permitir el regreso de conexiones TCP
 permit tcp any 172.16.32.64 0.0.0.63 established
 ! Permitir el regreso de PINGs iniciados por las laptops de Seguridad
 permit icmp any 172.16.32.64 0.0.0.63 echo-reply
 ! Bloquear cualquier intento malicioso de iniciar conexión hacia Seguridad
 deny ip 172.16.0.0 0.0.255.255 172.16.32.64 0.0.0.63
 permit ip any any
 exit

interface GigabitEthernet0/0/1
 ip access-group ACL_NO_ENTRADA_SEG out
 exit

end
write memory
```




## 9. Lista de Precios de Hardware (Cotización Estimada)

| Dispositivo | Cantidad | Precio Unitario (USD) | Subtotal (USD) | Subtotal (GTQ) |
|---|---|---|---|---|
| Switch Multicapa Cisco 3650-24PS | 11 | $1,200.00 | $13,200.00 | Q102,300.00 |
| Router Cisco ISR 4331 | 3 | $1,500.00 | $4,500.00 | Q34,875.00 |
| Switch Cisco 2960-24TT (Acceso) | 2 | $350.00 | $700.00 | Q5,425.00 |
| Transceptor SFP GLC-LH-SMD (Fibra) | 6 | $45.00 | $270.00 | Q2,092.50 |
| Router Inalámbrico Linksys WRT300N | 1 | $60.00 | $60.00 | Q465.00 |

---

## 10. Comandos de Verificación

A continuación, se presenta una lista de comandos útiles a ejecutar en la Interfaz de Línea de Comandos (CLI) de los dispositivos para verificar el correcto funcionamiento de las configuraciones implementadas:

### 10.1 Verificación de Enrutamiento (BGP, OSPF, EIGRP)
* `show ip route` - Muestra la tabla de enrutamiento completa. Validar la presencia de rutas marcadas con **B** (BGP), **O** (OSPF) o **D** (EIGRP).
* `show ip bgp summary` - Ejecutar en el Core para verificar que se haya establecido vecindad y adyacencia BGP con los ISPs vecinos.
* `show ip ospf neighbor` - Validar las adyacencias de OSPF en los routers interiores de Telecom Uno y Redes Nacionales.
* `show ip eigrp neighbors` - Validar las adyacencias de EIGRP en la topología Hub and Spoke de Link Global.

### 10.2 Verificación de Alta Disponibilidad y Agregación
* `show standby brief` - Ejecutar en MSW_Dist_1 y MSW_Dist_2 (Redes Nacionales) para verificar el estado **Active** o **Standby** del protocolo HSRP en las SVIs.
* `show etherchannel summary` - Verificar que los Port-Channels se encuentren con el flag **SU** (Layer 2) o **RU** (Layer 3) confirmando el funcionamiento de LACP.

### 10.3 Verificación de Servicios y Seguridad
* `show ip dhcp binding` - Ejecutar en el Servidor DHCP (interfaz GUI / Command Prompt) o Routers C3 configurados para validar el arrendamiento de direcciones IP a los hosts.
* `show access-lists` - Desplegar las ACLs configuradas y verificar el contador de *matches* cuando se realiza tráfico permitido o denegado.

### 10.4 Pruebas de Extremo a Extremo
* En la Command Prompt de cualquier PC final, usar el comando `ping [IP_Destino]` considerando las reglas de la matriz de comunicación permitida.
* Desde el *Web Browser* en terminales (Pestaña Desktop), acceder al URL proporcionado `http://www.proyecto2_202302220.com` para comprobar resolución DNS y servicio HTTP.
| Servidores Genéricos (Servicios) | 3 | $2,000.00 | $6,000.00 | Q46,500.00 |
| **TOTAL ESTIMADO** | | | **$24,730.00** | **Q191,657.50** |




# 10. Comandos de Verificación


Para demostrar el BGP (En MSW_Telecom):
show ip bgp summary -> Muestra que los vecinos están levantados.
show ip route bgp -> Demuestra que tu ISP está aprendiendo las rutas de todo el país.

Para demostrar la topología y redundancia LACP:
show etherchannel summary -> Úsalo en MSW_Core_RN o MSW_Soporte_1. Demuestra que los canales están en uso (bandera "U") y qué puertos físicos pertenecen a cada canal.

Para demostrar Alta Disponibilidad (HSRP):
show standby brief -> Úsalo en MSW_Dist_1. Deberá decir que su estado es "Active" y mostrará la IP virtual (VIP).

Para demostrar OSPF o EIGRP:
show ip protocols -> Un comando rápido que le dice al auxiliar qué protocolos están corriendo en el router actual y qué redes están inyectando.

