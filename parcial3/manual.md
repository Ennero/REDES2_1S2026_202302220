# Índice

- [Índice](#índice)
- [1.1. Plantilla de Configuración Base (Cisco IOS)](#11-plantilla-de-configuración-base-cisco-ios)
  - [1.2. Particularidad en Firewall ASA 5506-X](#12-particularidad-en-firewall-asa-5506-x)
- [2. Direccionamiento IP y Subneteo (VLSM)](#2-direccionamiento-ip-y-subneteo-vlsm)
  - [2.1. Tabla de Subneteo (VLSM) Red Interna](#21-tabla-de-subneteo-vlsm-red-interna)
  - [2.2. Enlaces Punto a Punto (P2P) y Salida a Internet](#22-enlaces-punto-a-punto-p2p-y-salida-a-internet)
  - [2.3. Tabla General de Asignación de Interfaces (Capa 3 y Servicios)](#23-tabla-general-de-asignación-de-interfaces-capa-3-y-servicios)
- [3. Topología de Red y Conexiones Físicas](#3-topología-de-red-y-conexiones-físicas)
  - [3.1. Inventario de Dispositivos Sugeridos (Packet Tracer)](#31-inventario-de-dispositivos-sugeridos-packet-tracer)
  - [3.2. Tabla de Conexiones (Matriz de Cableado)](#32-tabla-de-conexiones-matriz-de-cableado)
    - [Núcleo y Salida a Internet](#núcleo-y-salida-a-internet)
    - [Distribución (Core a Edificios)](#distribución-core-a-edificios)
    - [Edificio Ingeniería](#edificio-ingeniería)
    - [Edificio Administrativo](#edificio-administrativo)
    - [Edificio Biblioteca](#edificio-biblioteca)
    - [Red Inalámbrica (Invitados)](#red-inalámbrica-invitados)
  - [3.3. Notas Importantes para el Ensamblaje en Packet Tracer](#33-notas-importantes-para-el-ensamblaje-en-packet-tracer)
- [4. Configuración de Capa 2 (VLANs, Troncales y Port Security)](#4-configuración-de-capa-2-vlans-troncales-y-port-security)
  - [4.1. Creación de VLANs (En Core Switch y Switches de Acceso)](#41-creación-de-vlans-en-core-switch-y-switches-de-acceso)
  - [4.2. Configuración de Enlaces Troncales (Trunks)](#42-configuración-de-enlaces-troncales-trunks)
  - [4.3. Asignación de Puertos y Port Security Obligatorio](#43-asignación-de-puertos-y-port-security-obligatorio)
    - [Configuración en SW\_Ingenieria](#configuración-en-sw_ingenieria)
    - [Protección de Puertos No Utilizados (Buenas Prácticas)](#protección-de-puertos-no-utilizados-buenas-prácticas)
    - [Configuración en SW\_Admin](#configuración-en-sw_admin)
    - [Configuración en SW\_Biblioteca](#configuración-en-sw_biblioteca)
    - [Configuración de puerto de servidor en Core Switch](#configuración-de-puerto-de-servidor-en-core-switch)
- [5. Configuración de Capa 3 (Enrutamiento Inter-VLAN y OSPF)](#5-configuración-de-capa-3-enrutamiento-inter-vlan-y-ospf)
  - [5.1. Activación de Enrutamiento e Interfaces SVI (Core Switch)](#51-activación-de-enrutamiento-e-interfaces-svi-core-switch)
  - [5.2. Enlace Punto a Punto (Core Switch hacia Firewall)](#52-enlace-punto-a-punto-core-switch-hacia-firewall)
  - [5.3. Configuración del Protocolo OSPF](#53-configuración-del-protocolo-ospf)
- [6. Seguridad Perimetral y Acceso a Internet](#6-seguridad-perimetral-y-acceso-a-internet)
  - [6.1. Configuración del Router ISP (Internet Simulado)](#61-configuración-del-router-isp-internet-simulado)
  - [6.2. Configuración de Interfaces en Firewall ASA](#62-configuración-de-interfaces-en-firewall-asa)
  - [6.3. Enrutamiento y Propagación OSPF en el Firewall](#63-enrutamiento-y-propagación-ospf-en-el-firewall)
  - [6.4. Traducción de Direcciones (NAT/PAT) e Inspección ICMP](#64-traducción-de-direcciones-natpat-e-inspección-icmp)
- [7. Servicios Internos Requeridos](#7-servicios-internos-requeridos)
  - [7.1. Servidor Web Institucional](#71-servidor-web-institucional)
  - [7.2. Servidor FTP](#72-servidor-ftp)
  - [7.3. Servidor DHCP y DHCP Relay (IP Helper)](#73-servidor-dhcp-y-dhcp-relay-ip-helper)
    - [1. Configuración de Pools en Servidor DHCP (IP: `172.16.31.170`)](#1-configuración-de-pools-en-servidor-dhcp-ip-1721631170)
    - [2. Comandos de Enrutamiento DHCP (Core Switch)](#2-comandos-de-enrutamiento-dhcp-core-switch)

---

# 1.1. Plantilla de Configuración Base (Cisco IOS)

A continuación, se detalla el bloque de comandos genérico utilizado. El parámetro `[NOMBRE_DEL_DISPOSITIVO]` fue reemplazado individualmente por el nombre asignado en la topología (ej. `Core_Switch`, `SW_Ingenieria`, `Router_ISP`).

**Comandos aplicados en Switches y Routers:**

```bash
enable
configure terminal

! 1. Desactivacion de busqueda DNS para evitar bloqueos por comandos erroneos
no ip domain-lookup

! 2. Asignacion de nombre de host
hostname [NOMBRE_DEL_DISPOSITIVO]

! 3. Proteccion del modo privilegiado con encriptacion fuerte (MD5/SHA)
enable secret cisco123

! 4. Seguridad y control del puerto fisico de Consola
line console 0
 password cisco123
 login
 logging synchronous
 exit

! 5. Seguridad para lineas de terminal virtual (Accesos remotos)
line vty 0 15
 password cisco123
 login
 exit

! 6. Encriptacion global de contraseñas almacenadas en texto plano
service password-encryption

! 7. Mensaje del dia (MOTD) para advertencia legal
banner motd #
================================================================
          ADVERTENCIA: ACCESO RESTRINGIDO - RED USAC
 Este sistema es de uso exclusivo para personal autorizado.
 Todo intento de acceso no autorizado sera monitoreado y
 penalizado conforme a las normativas de la institucion.
================================================================
#
exit
```

---

## 1.2. Particularidad en Firewall ASA 5506-X

Debido a la naturaleza del sistema operativo del Cisco ASA, la configuración básica difiere ligeramente del IOS tradicional. En el dispositivo perimetral (`FW_ASA`), se aplicaron las credenciales y el nombre de host mediante la siguiente sintaxis:

```bash
enable
configure terminal
hostname FW_ASA
enable password cisco123
banner motd # ACCESO RESTRINGIDO - FIREWALL PERIMETRAL USAC #
exit
```

> **Justificación Técnica:** La implementación del `enable secret` en lugar del `enable password` tradicional garantiza que la credencial de modo privilegiado se almacene como un **hash criptográfico** en la NVRAM, mitigando ataques de lectura del archivo de configuración. Asimismo, el `banner motd` cumple con la normativa legal de disuasión y advertencia previa a la autenticación requerida en auditorías de red.



# 2. Direccionamiento IP y Subneteo (VLSM)

Para la implementación de la red institucional del campus central de la Universidad de San Carlos de Guatemala, se ha utilizado la técnica de **Máscara de Subred de Longitud Variable (VLSM)** partiendo de la dirección de red principal `172.16.30.0/16`.

El proceso de subneteo se realizó ordenando los requerimientos de hosts de mayor a menor, garantizando un uso eficiente del espacio de direcciones y dejando margen para la creación de enlaces punto a punto (P2P) entre los equipos de infraestructura de Capa 3 (Core Switch, Routers y Firewall).

---

## 2.1. Tabla de Subneteo (VLSM) Red Interna

A continuación, se detalla la segmentación de la red principal para cubrir los requerimientos de las subredes de los edificios de Ingeniería, Administrativo, Biblioteca y redes adicionales. Se ha establecido la primera dirección IP utilizable de cada rango como la puerta de enlace (Gateway) predeterminada para dicha subred.

| Edificio / Área | Subred | Hosts Req. | Hosts Disp. | Dirección de Red | Máscara | Rango Utilizable | Gateway (1ra IP) | Dirección de Broadcast |
|---|---|---|---|---|---|---|---|---|
| Biblioteca | Subred F | 100 | 126 | 172.16.30.0/25 | 255.255.255.128 | .1 - .126 | 172.16.30.1 | 172.16.30.127 |
| Invitados | Subred L (Wireless) | 100 | 126 | 172.16.30.128/25 | 255.255.255.128 | .129 - .254 | 172.16.30.129 | 172.16.30.255 |
| Ingeniería | Subred A | 50 | 62 | 172.16.31.0/26 | 255.255.255.192 | .1 - .62 | 172.16.31.1 | 172.16.31.63 |
| Ingeniería | Subred B | 25 | 30 | 172.16.31.64/27 | 255.255.255.224 | .65 - .94 | 172.16.31.65 | 172.16.31.95 |
| Administrativo | Red de Admin. | 25 | 30 | 172.16.31.96/27 | 255.255.255.224 | .97 - .126 | 172.16.31.97 | 172.16.31.127 |
| Adicional | Subred K | 25 | 30 | 172.16.31.128/27 | 255.255.255.224 | .129 - .158 | 172.16.31.129 | 172.16.31.159 |
| Biblioteca | Subred G | 20 | 30 | 172.16.31.160/27 | 255.255.255.224 | .161 - .190 | 172.16.31.161 | 172.16.31.191 |
| Adicional | Subred I | 10 | 14 | 172.16.31.192/28 | 255.255.255.240 | .193 - .206 | 172.16.31.193 | 172.16.31.207 |

---

## 2.2. Enlaces Punto a Punto (P2P) y Salida a Internet

Para la comunicación entre dispositivos de enrutamiento (ej. Core Switch hacia Routers de Distribución o Firewall), se ha continuado el VLSM utilizando máscaras `/30` a partir de la dirección `172.16.31.208`. El enlace con el Proveedor de Servicios de Internet (ISP) utiliza direccionamiento público estático provisto por el enunciado.

| Conexión | Red Asignada | Máscara | Rango Utilizable | IP Interfaz A | IP Interfaz B |
|---|---|---|---|---|---|
| Enlace P2P 1 (Ej: Core a FW) | 172.16.31.208/30 | 255.255.255.252 | .209 - .210 | 172.16.31.209 | 172.16.31.210 |
| Enlace P2P 2 (Ej: Core a R1) | 172.16.31.212/30 | 255.255.255.252 | .213 - .214 | 172.16.31.213 | 172.16.31.214 |
| Enlace P2P 3 (Ej: Core a R2) | 172.16.31.216/30 | 255.255.255.252 | .217 - .218 | 172.16.31.217 | 172.16.31.218 |
| Conexión ISP - USAC | 210.101.100.16/30 | 255.255.255.252 | .17 - .18 | 210.101.100.17 (USAC/FW) | 210.101.100.18 (ISP) |

---

## 2.3. Tabla General de Asignación de Interfaces (Capa 3 y Servicios)

> **Nota:** Las interfaces exactas (GigabitEthernet, FastEthernet o VLAN) dependerán de las conexiones físicas finales establecidas en Packet Tracer. Esta tabla funciona como la guía de configuración base.

| Dispositivo | Interfaz / VLAN | Dirección IP | Máscara de Subred | Gateway por Defecto |
|---|---|---|---|---|
| Core_Switch_L3 | VLAN F (Biblio) | 172.16.30.1 | 255.255.255.128 | N/A (Enruta) |
| Core_Switch_L3 | VLAN L (Wireless) | 172.16.30.129 | 255.255.255.128 | N/A (Enruta) |
| Core_Switch_L3 | VLAN A (Ing.) | 172.16.31.1 | 255.255.255.192 | N/A (Enruta) |
| Core_Switch_L3 | VLAN B (Ing.) | 172.16.31.65 | 255.255.255.224 | N/A (Enruta) |
| Core_Switch_L3 | VLAN Admin | 172.16.31.97 | 255.255.255.224 | N/A (Enruta) |
| Core_Switch_L3 | VLAN K | 172.16.31.129 | 255.255.255.224 | N/A (Enruta) |
| Core_Switch_L3 | VLAN G (Biblio) | 172.16.31.161 | 255.255.255.224 | N/A (Enruta) |
| Core_Switch_L3 | VLAN I | 172.16.31.193 | 255.255.255.240 | N/A (Enruta) |
| Core_Switch_L3 | G1/0/1 (Hacia FW) | 172.16.31.209 | 255.255.255.252 | Ruta OSPF/Default |
| FW_Perimetral_ASA | G0/0 (Inside) | 172.16.31.210 | 255.255.255.252 | N/A |
| FW_Perimetral_ASA | G0/1 (Outside) | 210.101.100.17 | 255.255.255.252 | 210.101.100.18 |
| Router_ISP | G0/0 (Hacia USAC) | 210.101.100.18 | 255.255.255.252 | N/A |
| Router_ISP | Loopback0 (Internet) | 8.8.8.8 (Ejemplo) | 255.255.255.255 | N/A |
| Servidor_Web | Fa0 (Directo al Core) | 172.16.31.130 | 255.255.255.224 | 172.16.31.129 |
| Servidor_FTP | Fa0 (En Subred F) | 172.16.30.10 | 255.255.255.128 | 172.16.30.1 |
| Servidor_DHCP | Fa0 (En Subred G) | 172.16.31.170 | 255.255.255.224 | 172.16.31.161 |





# 3. Topología de Red y Conexiones Físicas

Para el despliegue en Cisco Packet Tracer, se ha seleccionado hardware estándar empresarial que soporta las características requeridas de Capa 3 (OSPF, VLANs), seguridad perimetral (Firewall) y políticas de acceso (Port Security).

---

## 3.1. Inventario de Dispositivos Sugeridos (Packet Tracer)

- **Core Switch:** 1x Multilayer Switch (Modelo `3650-24PS` o `3560-24PS`) para concentración de VLANs y enrutamiento OSPF.
- **Switches de Acceso:** 3x Switches Capa 2 (Modelo `2960-24TT`) para los edificios de Ingeniería, Administrativo y Biblioteca.
- **Firewall Perimetral:** 1x Cisco ASA (Modelo `ASA 5506-X` o `5505`).
- **Router ISP:** 1x Router (Modelo `4331` o `1941`) para simular la salida a Internet y aplicar NAT/PAT en conjunto con el Firewall.
- **Dispositivos Inalámbricos:** 1x Access Point (Modelo `AP-PT` o WLC/Wireless Router) para la red de invitados.
- **Servidores y Endpoints:** Servidores genéricos de Packet Tracer (`Server-PT`) y PCs (`PC-PT`, `Laptop-PT`).

---

## 3.2. Tabla de Conexiones (Matriz de Cableado)

La siguiente tabla describe puerto a puerto cómo deben conectarse los dispositivos. Se especifican los enlaces troncales (Trunks) y los puertos de acceso.

> **Nota sobre cables:** En Packet Tracer, la conexión entre Switches se realiza convencionalmente con **Cable de Cobre Cruzado (Copper Cross-Over)**, mientras que las conexiones de Switch a PC/Servidor o Router utilizan **Cable de Cobre Directo (Copper Straight-Through)**.

### Núcleo y Salida a Internet

| Dispositivo Origen | Tipo / Modelo | Interfaz Origen | Dispositivo Destino | Interfaz Destino | Tipo de Cable | Función Lógica |
|---|---|---|---|---|---|---|
| Router_ISP | Router 4331 | G0/0/0 | FW_ASA | G1/1 (Outside) | Cobre Directo | Enlace P2P (Red Pública) |
| FW_ASA | ASA 5506-X | G1/2 (Inside) | Core_Switch | G1/0/1 | Cobre Directo | Enlace P2P (Red Interna) |

### Distribución (Core a Edificios)

| Dispositivo Origen | Tipo / Modelo | Interfaz Origen | Dispositivo Destino | Interfaz Destino | Tipo de Cable | Función Lógica |
|---|---|---|---|---|---|---|
| Core_Switch | Switch L3 3650 | G1/0/2 | SW_Ingenieria | G0/1 | Cobre Cruzado | Enlace Troncal (Trunk 802.1Q) |
| Core_Switch | Switch L3 3650 | G1/0/3 | SW_Admin | G0/1 | Cobre Cruzado | Enlace Troncal (Trunk 802.1Q) |
| Core_Switch | Switch L3 3650 | G1/0/4 | SW_Biblioteca | G0/1 | Cobre Cruzado | Enlace Troncal (Trunk 802.1Q) |

### Edificio Ingeniería

| Dispositivo Origen | Tipo / Modelo | Interfaz Origen | Dispositivo Destino | Interfaz Destino | Tipo de Cable | Función Lógica |
|---|---|---|---|---|---|---|
| SW_Ingenieria | Switch 2960 | F0/1 | PC_Ing_A1 | Fa0 | Cobre Directo | Acceso (VLAN A) + Port Security |
| SW_Ingenieria | Switch 2960 | F0/10 | PC_Ing_B1 | Fa0 | Cobre Directo | Acceso (VLAN B) + Port Security |

### Edificio Administrativo

| Dispositivo Origen | Tipo / Modelo | Interfaz Origen | Dispositivo Destino | Interfaz Destino | Tipo de Cable | Función Lógica |
|---|---|---|---|---|---|---|
| SW_Admin | Switch 2960 | F0/1 | PC_Admin1 | Fa0 | Cobre Directo | Acceso (VLAN Admin) + Port Security |
| SW_Admin | Switch 2960 | F0/10 | PC_Adicional_I1 | Fa0 | Cobre Directo | Acceso (VLAN I - Subred 10 hosts) + Port Security |
| Core_Switch | Switch L3 3650 | G1/0/24 | SVR_Web | Fa0 | Cobre Directo | Acceso (VLAN 70 - Servicios) |

### Edificio Biblioteca

| Dispositivo Origen | Tipo / Modelo | Interfaz Origen | Dispositivo Destino | Interfaz Destino | Tipo de Cable | Función Lógica |
|---|---|---|---|---|---|---|
| SW_Biblioteca | Switch 2960 | F0/1 | PC_Biblio_F1 | Fa0 | Cobre Directo | Acceso (VLAN F) + Port Security |
| SW_Biblioteca | Switch 2960 | F0/10 | PC_Biblio_G1 | Fa0 | Cobre Directo | Acceso (VLAN G) + Port Security |
| SW_Biblioteca | Switch 2960 | F0/20 | SVR_FTP | Fa0 | Cobre Directo | Acceso (VLAN F) |
| SW_Biblioteca | Switch 2960 | F0/21 | SVR_DHCP | Fa0 | Cobre Directo | Acceso (VLAN F/G) |
| SW_Biblioteca | Switch 2960 | F0/24 | AP_Wireless | Port 0 | Cobre Directo | Acceso (VLAN L - Invitados) |

### Red Inalámbrica (Invitados)

| Dispositivo Origen | Tipo / Modelo | Interfaz Origen | Dispositivo Destino | Interfaz Destino | Tipo de Cable | Función Lógica |
|---|---|---|---|---|---|---|
| AP_Wireless | Access Point | WiFi | Laptop_Inv1 | Wlan0 | Inalámbrico | Conexión SSID Invitados |

---

## 3.3. Notas Importantes para el Ensamblaje en Packet Tracer

- **Interfaces Libres y Seguridad:** El enunciado exige que los puertos no utilizados queden documentados y protegidos. Una vez armada la topología, se deben seleccionar los rangos de puertos que no estén en uso (por ejemplo, del `F0/2` al `F0/9` en los switches) y aplicarles el comando `shutdown`.

- **Red Administrativa:** La `PC_Admin1` no es un usuario final. Esta PC será la única autorizada (mediante Listas de Control de Acceso) para acceder por SSH a la consola del Core Switch, Firewall y Routers.

- **Simulación de Internet:** Para el `Router_ISP`, además de la interfaz física conectada al Firewall, se debe crear una interfaz virtual (ej. `interface loopback 0`) con una IP pública genérica (como `8.8.8.8`) para demostrar que el enrutamiento y el NAT están funcionando correctamente.

- **Configuración Dispositivo Inalámbrico:** Para que la `Laptop_Inv1` se conecte al `AP_Wireless`, en Packet Tracer se debe apagar físicamente la laptop, retirar el módulo Ethernet por defecto (FastEthernet) y colocar el módulo inalámbrico (`WPC300N`). Posteriormente, encender el equipo y configurar el mismo SSID en ambos dispositivos (`USAC_WIFI`).








# 4. Configuración de Capa 2 (VLANs, Troncales y Port Security)

En esta fase se establece la segmentación lógica de la red. El diseño exige que el Core Switch concentre las VLANs y que los switches de acceso apliquen políticas de seguridad en los puertos de usuario final.

---

## 4.1. Creación de VLANs (En Core Switch y Switches de Acceso)

Las VLANs deben crearse en la base de datos de todos los switches involucrados (Core, Ingeniería, Administrativo y Biblioteca).

**Comandos (Aplicar en TODOS los switches):**

```bash
enable
configure terminal
vlan 10
 name INGENIERIA_A
vlan 20
 name INGENIERIA_B
vlan 30
 name ADMINISTRATIVO
vlan 40
 name BIBLIOTECA_F
vlan 50
 name BIBLIOTECA_G
vlan 60
 name INVITADOS_WIFI
vlan 70
 name SERVICIOS_WEB
vlan 80
 name ADICIONAL_I
vlan 99
 name NATIVA_ADMIN
exit
```

> **Justificación Técnica:** Se ha creado una VLAN dedicada para cada subred solicitada en el diseño VLSM con el fin de reducir los dominios de broadcast y segmentar el tráfico lógicamente. Se incluye una **VLAN 99 Nativa** para mejorar la seguridad de los enlaces troncales.

---

## 4.2. Configuración de Enlaces Troncales (Trunks)

Los enlaces entre el Core Switch y los Switches de acceso deben transportar el tráfico de todas las VLANs.

**Comandos en el Core Switch (3650):**

> **Nota:** En los switches multicapa 3650 de Packet Tracer, es obligatorio especificar la encapsulación `dot1q` antes de convertir el puerto en troncal.

```bash
configure terminal
interface range GigabitEthernet 1/0/2 - 4
 switchport trunk encapsulation dot1q
 switchport mode trunk
 switchport trunk native vlan 99
 exit
```

**Comandos en los Switches de Acceso (2960):**

```bash
configure terminal
interface GigabitEthernet 0/1
 switchport mode trunk
 switchport trunk native vlan 99
 exit
```

---

## 4.3. Asignación de Puertos y Port Security Obligatorio

El proyecto exige configurar **Port Security** en los puertos de acceso, limitando la cantidad de MACs y definiendo una acción de violación. Se ha elegido la acción `restrict` porque bloquea el tráfico no autorizado y genera un registro (log) sin apagar el puerto físicamente, lo cual es ideal en un entorno universitario.

### Configuración en SW_Ingenieria

**Para la Subred A (F0/1 — VLAN 10):**

```bash
configure terminal
interface FastEthernet 0/1
 switchport mode access
 switchport access vlan 10

 ! Configuración de Port Security
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict

 description PC_Usuario_Ing_A
 exit
```

**Para la Subred B (F0/10 — VLAN 20):**

```bash
interface FastEthernet 0/10
 switchport mode access
 switchport access vlan 20
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict
 exit
```

### Protección de Puertos No Utilizados (Buenas Prácticas)

Para cumplir con la documentación de puertos no utilizados, se deben apagar lógicamente. En el switch de Ingeniería, asumiendo que solo se usan `F0/1` y `F0/10`:

```bash
interface range FastEthernet 0/2 - 9, FastEthernet 0/11 - 24, GigabitEthernet 0/2
 shutdown
 description PUERTO_DESHABILITADO
 exit
```

### Configuración en SW_Admin

En este switch, al haber movido el Servidor Web al Core, solo queda la PC administrativa. Se deben apagar todos los demás puertos para cumplir con las políticas de seguridad.

```bash
configure terminal
! Para la Subred Administrativa (F0/1 — VLAN 30)
interface FastEthernet 0/1
 switchport mode access
 switchport access vlan 30

 ! Configuración de Port Security
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict
 description PC_Admin1
 exit

! Para la Subred I (F0/10 — VLAN 80)
interface FastEthernet 0/10
 switchport mode access
 switchport access vlan 80
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict
 description PC_Adicional_I1
 exit

! Protección de Puertos No Utilizados
interface range FastEthernet 0/2 - 9, FastEthernet 0/11 - 24, GigabitEthernet 0/2
 shutdown
 description PUERTO_DESHABILITADO
 exit
```

### Configuración en SW_Biblioteca

Este switch concentra las dos subredes de estudiantes (F y G), los servidores institucionales y el Access Point.

> **Nota de Diseño:** No se aplica Port Security con un máximo de 1 MAC en el puerto del Access Point (`F0/24`), ya que por ahí pasará el tráfico de todos los dispositivos invitados que se conecten al WiFi. Limitarlo a 1 bloquearía la red inalámbrica. Tampoco es indispensable en puertos de servidores fijos.

```bash
configure terminal
! Para la Subred F (F0/1 — VLAN 40)
interface FastEthernet 0/1
 switchport mode access
 switchport access vlan 40
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict
 description PC_Biblio_F1
 exit

! Para la Subred G (F0/10 — VLAN 50)
interface FastEthernet 0/10
 switchport mode access
 switchport access vlan 50
 switchport port-security
 switchport port-security maximum 1
 switchport port-security mac-address sticky
 switchport port-security violation restrict
 description PC_Biblio_G1
 exit

! Para el Servidor FTP (F0/20 — VLAN 40)
interface FastEthernet 0/20
 switchport mode access
 switchport access vlan 40
 description ACCESO_SVR_FTP
 exit

! Para el Servidor DHCP (F0/21 — VLAN 50)
interface FastEthernet 0/21
 switchport mode access
 switchport access vlan 50
 description ACCESO_SVR_DHCP
 exit

! Para el Access Point de Invitados (F0/24 — VLAN 60)
interface FastEthernet 0/24
 switchport mode access
 switchport access vlan 60
 description ACCESO_AP_WIFI
 exit

! Protección de Puertos No Utilizados
interface range FastEthernet 0/2 - 9, FastEthernet 0/11 - 19, FastEthernet 0/22 - 23, GigabitEthernet 0/2
 shutdown
 description PUERTO_DESHABILITADO
 exit
```

### Configuración de puerto de servidor en Core Switch

```bash
configure terminal
interface GigabitEthernet 1/0/24
 switchport mode access
 switchport access vlan 70
 description ACCESO_SERVIDOR_WEB
 exit
```



# 5. Configuración de Capa 3 (Enrutamiento Inter-VLAN y OSPF)

Para permitir la comunicación entre las diferentes subredes del campus central, el **Core Switch (modelo 3650)** asume el rol de concentrador de Capa 3. Se configuran **Interfaces Virtuales de Switch (SVI)** que actúan como la puerta de enlace (Gateway) predeterminada para cada VLAN.

Adicionalmente, se implementa el protocolo de enrutamiento dinámico **OSPF** de forma obligatoria para el intercambio de rutas internas.

---

## 5.1. Activación de Enrutamiento e Interfaces SVI (Core Switch)

Para que el switch multicapa pueda enrutar paquetes IP, primero se debe habilitar el servicio de enrutamiento globalmente. Luego, se le asigna la primera IP utilizable de cada subred (según la tabla VLSM) a su respectiva interfaz VLAN.

**Comandos en el Core Switch:**

```bash
enable
configure terminal

! Habilitar el enrutamiento IPv4 en el switch
ip routing

! SVI para Subred A (Ingeniería)
interface vlan 10
 ip address 172.16.31.1 255.255.255.192
 no shutdown
 exit

! SVI para Subred B (Ingeniería)
interface vlan 20
 ip address 172.16.31.65 255.255.255.224
 no shutdown
 exit

! SVI para Red Administrativa
interface vlan 30
 ip address 172.16.31.97 255.255.255.224
 no shutdown
 exit

! SVI para Subred F (Biblioteca)
interface vlan 40
 ip address 172.16.30.1 255.255.255.128
 no shutdown
 exit

! SVI para Subred G (Biblioteca)
interface vlan 50
 ip address 172.16.31.161 255.255.255.224
 no shutdown
 exit

! SVI para Invitados WiFi
interface vlan 60
 ip address 172.16.30.129 255.255.255.128
 no shutdown
 exit

! SVI para Servicios Web (Subred K)
interface vlan 70
 ip address 172.16.31.129 255.255.255.224
 no shutdown
 exit

! SVI para Subred I (Adicional)
interface vlan 80
 ip address 172.16.31.193 255.255.255.240
 no shutdown
 exit
```

---

## 5.2. Enlace Punto a Punto (Core Switch hacia Firewall)

El puerto `GigabitEthernet 1/0/1` del Core Switch se conecta al Firewall Perimetral (ASA). Para que este puerto funcione como un enlace de Capa 3 puro (sin usar VLANs), se convierte en un puerto ruteado mediante el comando `no switchport`.

```bash
interface GigabitEthernet 1/0/1
 no switchport
 description ENLACE_P2P_HACIA_FIREWALL_ASA
 ip address 172.16.31.209 255.255.255.252
 no shutdown
 exit
```

---

## 5.3. Configuración del Protocolo OSPF

Se configura OSPF con el ID de proceso `1`. Se declaran todas las redes directamente conectadas al Core Switch utilizando sus respectivas **Wildcard Masks** (el inverso de la máscara de subred) y asignándolas al área `0`.

```bash
router ospf 1
 router-id 1.1.1.1
 ! Declaración de las subredes internas
 network 172.16.31.0 0.0.0.63 area 0      ! VLAN 10
 network 172.16.31.64 0.0.0.31 area 0     ! VLAN 20
 network 172.16.31.96 0.0.0.31 area 0     ! VLAN 30
 network 172.16.30.0 0.0.0.127 area 0     ! VLAN 40
 network 172.16.31.160 0.0.0.31 area 0    ! VLAN 50
 network 172.16.30.128 0.0.0.127 area 0   ! VLAN 60
 network 172.16.31.128 0.0.0.31 area 0    ! VLAN 70
 network 172.16.31.192 0.0.0.15 area 0    ! VLAN 80

 ! Declaración del enlace P2P hacia el Firewall
 network 172.16.31.208 0.0.0.3 area 0
 exit
```

> **Justificación Técnica:** El diseño requiere OSPF como protocolo principal. Al configurar las redes internas en el proceso OSPF del Core Switch, se garantiza que cualquier ruta aprendida — como la ruta por defecto hacia Internet que inyectará el Firewall — sea conocida por toda la infraestructura interna.






# 6. Seguridad Perimetral y Acceso a Internet

El diseño exige la separación de la red interna y la red del Proveedor de Servicios de Internet (ISP) mediante un **firewall perimetral (Cisco ASA 5506-X)**. Se implementan políticas de NAT/PAT para permitir que los usuarios internos accedan a un Internet simulado de manera segura.

---

## 6.1. Configuración del Router ISP (Internet Simulado)

El router del ISP proporciona la conexión pública y simula la existencia de Internet mediante una interfaz Loopback (ej. `8.8.8.8`). No necesita conocer las subredes internas (`172.16.x.x`), ya que todo el tráfico llegará enmascarado por el Firewall.

**Comandos en Router_ISP:**

```bash
enable
configure terminal
hostname Router_ISP

! Interfaz física conectada al Firewall USAC
interface GigabitEthernet 0/0/0
 ip address 210.101.100.18 255.255.255.252
 no shutdown
 exit

! Interfaz virtual para simular un servidor en Internet (Ej. Google DNS)
interface loopback 0
 ip address 8.8.8.8 255.255.255.255
 exit
```

---

## 6.2. Configuración de Interfaces en Firewall ASA

A diferencia de los routers IOS, el Firewall ASA basa su seguridad en niveles (`security-level`). La interfaz interna se configura con el nivel máximo de confianza (`100`) y la externa con el mínimo (`0`).

**Comandos en FW_ASA:**

```bash
enable
configure terminal
hostname FW_ASA

! Interfaz hacia el ISP (Exterior)
interface GigabitEthernet 1/1
 nameif outside
 security-level 0
 ip address 210.101.100.17 255.255.255.252
 no shutdown
 exit

! Interfaz hacia el Core Switch (Interior)
interface GigabitEthernet 1/2
 nameif inside
 security-level 100
 ip address 172.16.31.210 255.255.255.252
 no shutdown
 exit
```

---

## 6.3. Enrutamiento y Propagación OSPF en el Firewall

El Firewall debe conocer cómo enviar tráfico a Internet mediante una ruta por defecto estática, e inyectar esta ruta al Core Switch a través de OSPF.

> **Nota Técnica:** En el Firewall ASA, la declaración de redes en OSPF utiliza la **máscara de subred tradicional** en lugar de la Wildcard Mask utilizada en los switches o routers IOS.

**Comandos en FW_ASA:**

```bash
! Ruta estática por defecto hacia el ISP
route outside 0.0.0.0 0.0.0.0 210.101.100.18

! Proceso OSPF para crear adyacencia con el Core Switch
router ospf 1
 network 172.16.31.208 255.255.255.252 area 0
 default-information originate
 exit
```

---

## 6.4. Traducción de Direcciones (NAT/PAT) e Inspección ICMP

Para que las direcciones privadas del campus salgan a Internet, se agrupa la red principal `172.16.0.0/16` en un objeto de red y se le aplica un **NAT dinámico (PAT)** contra la interfaz externa. Finalmente, se habilita la inspección de paquetes ICMP para permitir que las pruebas de `ping` desde el interior regresen a través del firewall.

**Comandos en FW_ASA:**

```bash
! Creación de Objeto de Red para PAT
object network OBJ_RED_INTERNA
 subnet 172.16.0.0 255.255.0.0
 nat (inside,outside) dynamic interface
 exit

! Permitir inspección de PINGs a través del firewall
policy-map global_policy
 class inspection_default
  inspect icmp
  exit
```

> **Justificación Técnica:** El uso de `dynamic interface` (PAT) permite que todas las subredes del campus central compartan una única dirección IP pública (`210.101.100.17`), ahorrando espacio de direccionamiento IP público tal como lo requiere el enunciado.







# 7. Servicios Internos Requeridos

El proyecto contempla la implementación de servicios base para la operación de la red institucional: servidor **Web**, **FTP** y **DHCP**. Dado que estos servicios son simulados mediante dispositivos genéricos de Packet Tracer, la mayor parte de su configuración se realiza a través de la interfaz gráfica (GUI).

---

## 7.1. Servidor Web Institucional

Ubicado en el Core Switch (Subred K), este servidor aloja la página web de la USAC solicitada en los requerimientos. Al ubicarse conectado directamente al núcleo, se garantiza un acceso centralizado.

- **Dirección IP estática:** `172.16.31.130`
- **Máscara de subred:** `255.255.255.224`
- **Default Gateway:** `172.16.31.129`
- **Configuración (Packet Tracer):** En la pestaña **Services → HTTP**, se verificó que HTTP y HTTPS estén en `ON`. Se editó el archivo `index.html` con el siguiente código para mostrar los datos del estudiante:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Portal USAC - Campus Central</title>
    <style>
        body { font-family: Arial, sans-serif; background-color: #f4f4f9; color: #333; text-align: center; padding: 50px; }
        .container { background-color: white; padding: 30px; border-radius: 10px; box-shadow: 0px 0px 10px rgba(0,0,0,0.1); display: inline-block; }
        h1 { color: #003366; }
        h2 { color: #cc0000; }
        p { font-size: 18px; }
    </style>
</head>
<body>
    <div class="container">
        <h1>Universidad de San Carlos de Guatemala</h1>
        <h2>Facultad de Ingeniería - Redes de Computadoras</h2>
        <hr>
        <p><strong>Proyecto Final - Tercer Examen Parcial</strong></p>
        <p><strong>Estudiante:</strong> Enner Esaí Mendizabal Castro</p>
        <p><strong>Carnet:</strong> 202302220</p>
        <br>
        <p><em>Servidor Web Institucional Operativo</em></p>
    </div>
</body>
</html>
```

---

## 7.2. Servidor FTP

Ubicado en el edificio de la Biblioteca (Subred F), este servidor actúa como un repositorio de archivos centralizado para respaldos de configuración de red e intercambio de material.

- **Dirección IP estática:** `172.16.30.10`
- **Máscara de subred:** `255.255.255.128`
- **Default Gateway:** `172.16.30.1`
- **Configuración (Packet Tracer):** En la pestaña **Services → FTP**, se validó que el servicio esté en `ON`. Se creó el usuario administrador (`admin_usac` con contraseña `cisco123`) otorgándole permisos totales: **Read, Write, Delete, Rename, List**.

---

## 7.3. Servidor DHCP y DHCP Relay (IP Helper)

El servidor DHCP centralizado se ubica en la Biblioteca (Subred G) y se encarga de proveer direccionamiento dinámico a las subredes de usuarios finales.

Debido a que los paquetes de solicitud (Broadcast) de DHCP no atraviesan las fronteras de red (Capa 3), se configuró el **DHCP Relay** mediante el comando `ip helper-address` en las interfaces SVI del Core Switch. Esto permite que el switch reciba las solicitudes de otras VLANs y las reenvíe directamente al servidor.

### 1. Configuración de Pools en Servidor DHCP (IP: `172.16.31.170`)

Se crearon pools dedicados para cada subred de usuarios finales, excluyendo las redes de administración y servidores que utilizan direccionamiento estático:

| Nombre del Pool | Default Gateway | Start IP | Subnet Mask | Usuarios |
|---|---|---|---|---|
| Pool_Ingenieria_A | 172.16.31.1 | 172.16.31.5 | 255.255.255.192 | 50 |
| Pool_Ingenieria_B | 172.16.31.65 | 172.16.31.70 | 255.255.255.224 | 25 |
| Pool_Biblioteca_F | 172.16.30.1 | 172.16.30.5 | 255.255.255.128 | 100 |
| Pool_Biblioteca_G | 172.16.31.161 | 172.16.31.165 | 255.255.255.224 | 20 |
| Pool_Invitados_WIFI | 172.16.30.129 | 172.16.30.135 | 255.255.255.128 | 100 |
| Pool_Adicional_I | 172.16.31.193 | 172.16.31.195 | 255.255.255.240 | 10 |

### 2. Comandos de Enrutamiento DHCP (Core Switch)

Para habilitar el agente de retransmisión, se aplicó la dirección IP del servidor DHCP a las interfaces VLAN requeridas:

```bash
configure terminal
! Reenvío DHCP para Subred A (Ingeniería)
interface vlan 10
 ip helper-address 172.16.31.170
 exit

! Reenvío DHCP para Subred B (Ingeniería)
interface vlan 20
 ip helper-address 172.16.31.170
 exit

! Reenvío DHCP para Subred F (Biblioteca)
interface vlan 40
 ip helper-address 172.16.31.170
 exit

! Reenvío DHCP para Invitados WiFi
interface vlan 60
 ip helper-address 172.16.31.170
 exit

! Reenvío DHCP para Subred I (Adicional)
interface vlan 80
 ip helper-address 172.16.31.170
 exit
```

> **Justificación Técnica:** Centralizar el servicio DHCP optimiza los recursos de la red al no requerir un servidor dedicado por edificio. La **Subred G (VLAN 50)** no requiere `ip helper-address` ya que el servidor se encuentra físicamente conectado a ese mismo segmento de red, resolviendo las solicitudes de manera local.