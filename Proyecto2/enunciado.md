# Universidad San Carlos de Guatemala
## Facultad de Ingeniería — Ingeniería en Ciencias y Sistemas

**CYS | UBAR | FIUSAC | FACULTAD DE INGENIERÍA | UNIVERSIDAD DE SAN CARLOS DE GUATEMALA | Cococys**

---

# Proyecto 2: Telecom Uno, Redes Nacionales y Link Global: Conectando al país.

**PONDERACIÓN:** 25  
**Horas Aproximadas:** 30

---

## Índice

| Tema | Página |
|------|--------|
| 1. MARCO FORMATIVO | 3 |
| 1.1. Valor | 3 |
| 1.2. Competencia(s) | 3 |
| 1.3. Objetivo SMART | 3 |
| 2. Resumen Ejecutivo | 4 |
| 3. Enunciado del Proyecto | 4 |
| 3.1. Requerimientos importantes | 5 |
| 3.1.1. Interconexión de ISPs | 5 |
| 3.1.2. ISP 1: Telecom Uno | 5 |
| 3.1.3. ISP 2: Redes Nacionales | 6 |
| 3.1.4. ISP 3: Conexiones Futuras | 7 |
| 3.2. Reglas de comunicación | 8 |
| 3.3. Servidor WEB | 8 |
| 3.4. DHCP | 8 |
| 3.5. Presentación | 8 |
| 3.6. Restricciones | 9 |
| 3.8. Observaciones | 10 |
| 3.9. Entregables | 10 |
| 4. Material de apoyo | 11 |
| 5. Metodología | 11 |
| 7. Desarrollo de Habilidades Blandas | 12 |
| 8. Cronograma | 12 |
| 9. Rúbrica de Calificación | 13 |

---

## 1. MARCO FORMATIVO

### 1.1. Valor

| Nombre del valor | ¿Cómo se aplica en tu laboratorio? |
|------------------|-------------------------------------|
| Responsabilidad profesional | El estudiante debe garantizar que la red diseñada cumpla con los plazos establecidos, las especificaciones técnicas requeridas y considere el impacto social de su propuesta. La responsabilidad se demuestra mediante configuraciones robustas, documentación transparente y cumplimiento de fechas de entrega. |

### 1.2. Competencia(s)

| Tipo de Competencia | Descripción |
|---------------------|-------------|
| Competencia General | El estudiante configura la agregación de enlaces utilizando los protocolos LACP o PAgP para mejorar el ancho de banda y la tolerancia a fallos en redes Ethernet. |
| Competencia Específica | El estudiante identifica los principios de funcionamiento de los protocolos de enrutamiento dinámico mediante la comparación de sus características, ventajas y desventajas para utilizarlos en la implementación de redes de forma eficiente y segura. |

### 1.3. Objetivo SMART

| SMART | Objetivo redactado |
|-------|--------------------|
| Específico (¿Qué?) | Configurar 3-4 ISPs interconectados mediante BGP, cada uno con topología específica (árbol, jerárquica, hub-spoke, malla), protocolos de enrutamiento definidos (OSPF, EIGRP, RIP), servicios de DNS, DHCP, HTTP, y cumplir reglas de comunicación entre departamentos. |
| Medible (¿Cuánto?) / Alcanzable (¿Cómo?) | • Al menos 5 routers y 5 hosts por ISP  • 2 enlaces LACP/PAGP por ISP  • Comunicación funcional según tabla de reglas  • Servicios DNS, DHCP y HTTP operativos. Utilizando Cisco Packet Tracer, equipos virtuales Cisco y protocolos estándar de red. |
| Realista (¿Para qué?) / A Tiempo (¿Cuándo?) | Desarrolla competencias en redes WAN, BGP, enrutamiento interdominio, servicios de red empresariales y diseño de infraestructuras escalables. **Fecha de entrega: 02/05/2026** |

---

## 2. Resumen Ejecutivo

El proyecto consiste en diseñar e implementar una infraestructura nacional de telecomunicaciones que interconecte tres o cuatro proveedores de servicio (ISP). Cada ISP tiene requisitos específicos de topología, protocolos de enrutamiento interno (OSPF, EIGRP, RIP) y servicios críticos. La interconexión entre ISPs se realiza mediante BGP utilizando la red `192.168.X.0/16`. Se implementan reglas de comunicación específicas entre departamentos, servicios centralizados y una presentación ejecutiva para "vender" la solución a stakeholders.

---

## 3. Enunciado del Proyecto

Un país con una población en rápido crecimiento y una demanda de telecomunicaciones en auge ha decidido modernizar y ampliar su infraestructura de red para satisfacer las necesidades actuales y futuras de sus ciudadanos. Tres de las principales empresas de telecomunicaciones han sido seleccionadas para colaborar en este ambicioso proyecto de expansión: **Telecom Uno**, **Redes Nacionales** y **Link Global**.

El gobierno, junto con estas empresas, ha creado un comité de expertos que se encargarán del diseño, simulación y presentación de una solución eficiente, escalable y económicamente viable. Dado su extensa experiencia en el campo, el comité ha decidido seleccionarlo a usted para llevar a cabo este desafío.

El país cuenta con tres empresas de telecomunicaciones interesadas en optimizar sus redes internas y mejorar la interconectividad entre ellas para ofrecer un mejor servicio a nivel nacional. Cada ISP tiene necesidades y requerimientos específicos para sus redes internas, y, además, se debe garantizar una conectividad óptima y segura entre las tres empresas a través de un protocolo de enrutamiento común.

### 3.1. Requerimientos importantes

#### 3.1.1. Interconexión de ISPs

- Se deben conectar los tres ISP: Telecom Uno, Redes Nacionales y Link Global.
- Configuración de BGP para interconectar los routers principales de los ISP.
- Para conectar los routers o switches de capa 3 se asigna la dirección de red `192.168.X.0/16` donde X son los últimos 2 dígitos del carné.
- El medio de conexión entre routers es **fibra óptica**.
- Dispositivos: 3650-24PS Multilayer Switch (x3)

#### 3.1.2. ISP 1: Telecom Uno

Red asignada: `172.16.1X.0/24` (X = último dígito del carné)

**Departamentos:**
- Administración
- Atención al cliente

El estudiante deberá realizar el proceso de subneteo de la dirección asignada para crear subredes que permitan la comunicación entre estos dos departamentos. El tamaño de las subredes queda a discreción del estudiante.

**Detalles:**
- Topología en **árbol**.
- Protocolo de enrutamiento: **OSPF**.
- Al menos 5 routers, 5 hosts y 2 enlaces LACP.
- Esta empresa provee el servicio **DNS y HTTP** a toda la topología.

#### 3.1.3. ISP 2: Redes Nacionales

Red asignada: `172.16.2X.0/24` (X = último dígito del carné)

**Departamentos:**
- Ventas
- Facturación

El estudiante deberá realizar el proceso de subneteo de la dirección asignada para crear subredes que permitan la comunicación entre estos dos departamentos. El tamaño de las subredes queda a discreción del estudiante.

**Detalles:**
- Topología de **modelo jerárquico**.
- Protocolo de enrutamiento: **OSPF**.
- Al menos 5 routers, 5 hosts y 2 enlaces LACP.
- Se requiere **redundancia de capa 3 (HSRP)** obligatoriamente.
- Esta empresa proporciona el direccionamiento IP por medio de **DHCP** a toda la topología.

#### 3.1.4. ISP 3: Conexiones Futuras

Red asignada: `172.16.3X.0/24` (X = primer dígito del carné)

**Departamentos:**
- Soporte
- Seguridad

El estudiante deberá realizar el proceso de subneteo de la dirección asignada para crear subredes que permitan la comunicación entre estos dos departamentos. El tamaño de las subredes queda a discreción del estudiante.

**Detalles:**
- Topología **hub and spoke**.
- Protocolo de enrutamiento: **EIGRP**.
- Al menos 5 routers, 5 hosts y 2 enlaces LACP.
- Debe tener al menos un **router inalámbrico** que proporcione servicio WiFi.

---

### 3.2. Reglas de comunicación

| Departamento | Debe existir tráfico de salida con: | Debe existir tráfico de entrada con: |
|--------------|--------------------------------------|---------------------------------------|
| Seguridad | Todos los deptos. | Ningún departamento. |
| Soporte | Todos los deptos. | Todos los deptos. |
| Administración | Todos los deptos. | Todos los deptos. |
| Atención al Cliente | Ventas. | Ventas. |
| Facturación | Ventas. | Ventas. |
| Ventas | Facturación y Atención al Cliente. | Facturación y Atención al Cliente. |

---

### 3.3. Servidor WEB

- Proporcionará direccionamiento DNS para todos los dispositivos finales.
- El dominio a configurar será: `www.proyecto2_#carné.com`
- Al ingresar el dominio en el buscador web deberá desplegarse una **página web estática** con los datos del estudiante e información básica del curso.

---

### 3.4. DHCP

Todos los dispositivos finales deben obtener direccionamiento IP por medio de **DHCP**.

---

### 3.5. Presentación

Durante este proyecto no solamente se debe configurar una topología de red, sino también vender la idea a los dirigentes. Se debe realizar una presentación en **PowerPoint, Canva o el software a elección** con información relevante como:

- Arquitectura seleccionada
- Despliegue de costos
- Dispositivos seleccionados
- Tecnologías utilizadas

Factores a considerar: eficiencia, costo, innovación, tanto a nivel de software como de hardware.

La exposición debe durar **mínimo 5 y máximo 10 minutos** durante la calificación.

---

### 3.6. Restricciones

- La práctica se realizará de forma **individual**.
- Se debe tener un claro conocimiento de la red propuesta.
- Las configuraciones deben realizarse desde la **CLI**.
- En el repositorio del Proyecto 1 debe crearse una carpeta llamada **"Proyecto 2"**.
- El archivo debe llamarse: `Proyecto2_#carné.pkt`
- La presentación debe llamarse: `Proyecto2_Presentacion_#carné.pdf`
- Durante la calificación solo habrá **un intento** para demostrar comunicación entre topologías mediante ping.
- Para tener derecho a calificación: el direccionamiento IP debe ser por **DHCP** (no se calificará con estático).
- Para tener derecho a calificación: debe demostrarse funcionalidad en **servidor DNS y HTTP**.
- Para tener derecho a calificación: debe demostrarse al menos una sección funcional con **HSRP**.

---

### 3.7. Penalizaciones

| Falta | Penalización |
|-------|--------------|
| Falta de seguimiento continuo (GitLab/GitHub) | -10% |
| Incumplimiento de instrucciones de entrega (nombre del repositorio) | -5% |
| No implementar la topología indicada | -60% de la nota obtenida |
| Impuntualidad en calificación (sin previo aviso) | -10% de la nota obtenida |
| Entrega 1-10 minutos tarde | -10% |
| Entrega 11-59 minutos tarde | -30% |
| Entrega con más de 60 minutos de retraso | Nota de 0, no se califica |
| Copias | Nota de 0 + sanción reglamentaria |

---

### 3.8. Observaciones

- **Software:** Cisco Packet Tracer.
- La entrega se realizará por medio de **UEDI**, utilizando el repositorio creado para el Proyecto 1.
- Se debe crear una carpeta con el nombre **Proyecto2**.

---

### 3.9. Entregables

1. Enlace al repositorio (UEDI).
2. Manual Técnico (Repositorio).
3. Archivo `.pkt` (Repositorio).

---

## 4. Material de apoyo

- Cisco Systems, Inc. (2023). *BGP Configuration Guide, Cisco IOS Release 15.2M&T*. Disponible en: https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_bgp/configuration/15-mt/irg-15-mt-book.html

- Cisco Systems, Inc. (2023). *OSPF Configuration Guide*. Disponible en: https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_ospf/configuration/15-mt/iro-15-mt-book/iro-cfg.html

- Cisco Systems, Inc. (2023). *EIGRP Configuration Guide*. Disponible en: https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_eigrp/configuration/15-mt/ire-15-mt-book/ire-cfq.html

- Cisco Systems, Inc. (2023). *RIP Configuration Guide*. Disponible en: https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_rip/configuration/15-mt/irr-15-mt-book/irr-cfq.html

---

## 5. Metodología

**Fase 1: Análisis y Planificación (Semana 1)**
- Análisis de requisitos por ISP
- Subneteo de redes asignadas (`172.16.x.0/24`)
- Diseño de topologías específicas
- Definición de reglas de comunicación

**Fase 2: Implementación Individual ISP (Semana 2)**
- Configuración de protocolos internos (OSPF, EIGRP, RIP)
- Implementación de servicios asignados (DNS, DHCP, HTTP)
- Configuración de redundancia (HSRP) y enlaces (LACP/PAGP)
- Setup de red inalámbrica (ISP 3)

**Fase 3: Integración BGP (Semana 3)**
- Configuración de BGP entre routers principales
- Establecimiento de vecinos BGP
- Verificación de rutas intercambiadas
- Pruebas de conectividad inter-ISP

**Fase 4: Validación y Documentación (Semana 3)**
- Pruebas integrales de comunicación
- Verificación de reglas de tráfico
- Documentación en manual técnico
- Preparación de presentación ejecutiva

---

## 6. Recursos y herramientas a utilizar

- **Software:** Packet Tracer
- **Plataformas:** UEDI, GitHub

---

## 7. Desarrollo de Habilidades Blandas

- **Comunicación efectiva:** Explicar tecnicidades a stakeholders no técnicos en la presentación.
- **Colaboración equitativa:** Distribución balanceada de tareas.
- **Pensamiento crítico:** Resolver problemas de interoperabilidad entre diferentes protocolos.
- **Creatividad e innovación:** Diseñar soluciones escalables y económicamente viables.
- **Gestión del tiempo:** Cumplir con el cronograma ajustado.
- **Adaptabilidad:** Trabajar con múltiples tecnologías y requisitos simultáneos.
- **Responsabilidad:** Garantizar que la infraestructura propuesta sea confiable y segura.

---

## 8. Cronograma

| Tipo | Fecha Inicio | Fecha Fin |
|------|-------------|-----------|
| Asignación de Proyecto | 11/04/2026 | 11/04/2026 |
| Elaboración | 11/04/2026 | 01/05/2026 |
| Calificación | 02/05/2026 | 04/05/2026 |

---

## 9. Rúbrica de Calificación

La práctica se realizará de manera **individual**. Las configuraciones deben realizarse desde la **CLI** (exceptuando configuraciones en dispositivos que no cuenten con CLI).

**Requisitos para calificación:**
- Direccionamiento IP por **DHCP** (no se califica con estático).
- Funcionalidad en **servidor DNS y HTTP**.
- Al menos una sección funcional con **HSRP**.
- Un solo intento de ping para demostrar comunicación entre topologías.

| Tema | Descripción | Cumple (Sí/No) |
|------|-------------|----------------|
| Cumplimiento de la entrega | Entrega puntual en la plataforma UEDI. | |
| Direccionamiento IP | Debe ser por DHCP. | |

---

### Resumen de puntuaciones

| Descripción de ponderación | Puntos totales | Puntos obtenidos |
|----------------------------|---------------|-----------------|
| **Telecom Uno** | **21** | |
| Topología de árbol bien diseñada | 4 | |
| OSPF utilizado como protocolo de enrutamiento | 4 | |
| Configuración de al menos 2 enlaces LACP y cantidad mínima de dispositivos | 3 | |
| Dirección de red `172.16.1X.0/24` utilizada como base | 2 | |
| Subneteo de la dirección base para las 2 subredes (Administración y Atención al cliente) | 2 | |
| Configuración correcta de DNS para toda la topología | 4 | |
| Listas de Control de Acceso correctas | 2 | |
| **Redes Nacionales** | **23** | |
| Topología de modelo jerárquico bien diseñada | 4 | |
| OSPF utilizado como protocolo de enrutamiento | 4 | |
| Configuración de al menos 2 enlaces LACP y cantidad mínima de dispositivos | 3 | |
| Dirección de red `172.16.2X.0/24` utilizada como base | 2 | |
| Subneteo de la dirección base para las 2 subredes (Ventas y Facturación) | 2 | |
| Configuración e implementación correcta de HSRP | 4 | |
| Direccionamiento DHCP correctamente configurado para toda la topología | 3 | |
| Listas de Control de Acceso correctas | 1 | |
| **Conexiones Futuras** | **21** | |
| Topología hub and spoke bien diseñada | 4 | |
| EIGRP utilizado como protocolo de enrutamiento | 4 | |
| Configuración de al menos 2 enlaces LACP y cantidad mínima de dispositivos | 3 | |
| Dirección de red `172.16.3X.0/24` utilizada como base | 2 | |
| Subneteo de la dirección base para las 2 subredes (Soporte y Seguridad) | 2 | |
| Implementación correcta de red inalámbrica | 4 | |
| Listas de control de acceso correctas | 2 | |
| **Conexión entre ISPs** | **10** | |
| Configuración de la topología indicada | 2 | |
| Configuración de BGP en cada uno de los routers o switches multicapa | 6 | |
| Subneteo de la red `192.168.X.0/16` para las interfaces con máscara eficiente | 2 | |
| **Comunicación entre departamentos** | **10** | |
| Administración - Ventas | 3 | |
| Facturación - Administración | 3 | |
| Soporte - Ventas | 2 | |
| Atención al cliente - Facturación | 2 | |
| **Presentación** | **10** | |
| Descripción de las topologías utilizadas (1pt c/u) | 3 | |
| Descripción de los dispositivos utilizados | 3 | |
| Lista de precios | 2 | |
| Tecnologías y protocolos utilizados | 2 | |
| **Evaluación de conocimiento** | **5** | |
| Explicación de trabajo realizado | 5 | |

---

### Penalizaciones

| Infracción | Penalización |
|------------|--------------|
| Topología diferente a la solicitada | -60% |
| Sin seguimiento por medio de GitHub o GitLab | -10% |
| Nombre del repositorio u organización de carpetas errónea | -5% |
| Impuntualidad en la calificación | -10% |
| Entrega tarde | -10% al 100% |
| El estudiante no se presenta a la calificación | -10% |

| | **TOTAL** | **100** | |