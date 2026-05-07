# Índice

- [Índice](#índice)
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

---

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

- **Configuración Dispositivo Inalámbrico:** Para que la `Laptop_Inv1` se conecte al `AP_Wireless`, en Packet Tracer se debe apagar físicamente la laptop, retirar el módulo Ethernet por defecto (FastEthernet) y colocar el módulo inalámbrico (`WPC300N`). Posteriormente, encender el equipo y configurar el mismo SSID en ambos dispositivos (Ejemplo: `USAC_WIFI`).








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

! Protección de Puertos No Utilizados
interface range FastEthernet 0/2 - 24, GigabitEthernet 0/2
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



