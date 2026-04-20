# Proyecto 2: Telecom Uno, Redes Nacionales y Link Global: Conectando al país.

**Universidad San Carlos de Guatemala**  
**Facultad de Ingeniería**  
**Ingeniería en Ciencias y Sistemas**  
**Redes de Computadoras 2**

**PONDERACIÓN: 25**  
**Horas Aproximadas: 30**

---

## Índice

- [Proyecto 2: Telecom Uno, Redes Nacionales y Link Global: Conectando al país.](#proyecto-2-telecom-uno-redes-nacionales-y-link-global-conectando-al-país)
  - [Índice](#índice)
  - [1. Marco Formativo](#1-marco-formativo)
    - [1.1. Valor](#11-valor)
    - [1.2. Competencias](#12-competencias)
    - [1.3. Objetivo SMART](#13-objetivo-smart)
  - [2. Resumen Ejecutivo](#2-resumen-ejecutivo)
  - [3. Enunciado del Proyecto](#3-enunciado-del-proyecto)
    - [3.1. Requerimientos Importantes](#31-requerimientos-importantes)
      - [3.1.1. Interconexión de ISPs](#311-interconexión-de-isps)
      - [3.1.2. ISP 1: Telecom Uno](#312-isp-1-telecom-uno)
      - [3.1.3. ISP 2: Redes Nacionales](#313-isp-2-redes-nacionales)
      - [3.1.4. ISP 3: Link Global](#314-isp-3-link-global)
    - [3.2. Reglas de Comunicación](#32-reglas-de-comunicación)
    - [3.3. Servidor WEB](#33-servidor-web)
    - [3.4. DHCP](#34-dhcp)
    - [3.5. Presentación](#35-presentación)
    - [3.6. Restricciones](#36-restricciones)
    - [3.7. Penalizaciones](#37-penalizaciones)
    - [3.8. Observaciones](#38-observaciones)
    - [3.9. Entregables](#39-entregables)
  - [4. Material de Apoyo](#4-material-de-apoyo)
  - [5. Metodología](#5-metodología)
  - [6. Recursos y Herramientas a Utilizar](#6-recursos-y-herramientas-a-utilizar)
  - [7. Desarrollo de Habilidades Blandas](#7-desarrollo-de-habilidades-blandas)
  - [8. Cronograma](#8-cronograma)
  - [9. Rúbrica de Calificación](#9-rúbrica-de-calificación)
    - [Requisitos previos para calificación](#requisitos-previos-para-calificación)
    - [Resumen de Puntuaciones](#resumen-de-puntuaciones)
    - [Penalizaciones](#penalizaciones)

---

## 1. Marco Formativo

### 1.1. Valor

| Nombre del valor | ¿Cómo se aplica en tu laboratorio? |
|---|---|
| Responsabilidad profesional | El estudiante debe garantizar que la red diseñada cumpla con los plazos establecidos, las especificaciones técnicas requeridas y considere el impacto social de su propuesta. La responsabilidad se demuestra mediante configuraciones robustas, documentación transparente y cumplimiento de fechas de entrega. |

### 1.2. Competencias

| Tipo de Competencia | Descripción |
|---|---|
| Competencia General | El estudiante configura la agregación de enlaces utilizando los protocolos LACP o PAgP para mejorar el ancho de banda y la tolerancia a fallos en redes Ethernet. |
| Competencia Específica | El estudiante identifica los principios de funcionamiento de los protocolos de enrutamiento dinámico mediante la comparación de sus características, ventajas y desventajas para utilizarlos en la implementación de redes de forma eficiente y segura. |

### 1.3. Objetivo SMART

| SMART | Objetivo redactado |
|---|---|
| **Específico (¿Qué?)** | Configurar 3-4 ISPs interconectados mediante BGP, cada uno con topología específica (árbol, jerárquica, hub-spoke, malla), protocolos de enrutamiento definidos (OSPF, EIGRP, RIP), servicios de DNS, DHCP, HTTP, y cumplir reglas de comunicación entre departamentos. |
| **Medible (¿Cuánto?)** | - Al menos 5 routers y 5 hosts por ISP<br>- 2 enlaces LACP/PAGP por ISP<br>- Comunicación funcional según tabla de reglas<br>- Servicios DNS, DHCP y HTTP operativos |
| **Alcanzable (¿Cómo?)** | Utilizando Cisco Packet Tracer, equipos virtuales Cisco y protocolos estándar de red. |
| **Realista (¿Para qué?)** | Desarrolla competencias en redes WAN, BGP, enrutamiento interdominio, servicios de red empresariales y diseño de infraestructuras escalables. |
| **A Tiempo (¿Cuándo?)** | Fecha de entrega: 02/05/2026 |

---

## 2. Resumen Ejecutivo

El proyecto consiste en diseñar e implementar una infraestructura nacional de telecomunicaciones que interconecte tres o cuatro proveedores de servicio (ISP). Cada ISP tiene requisitos específicos de topología, protocolos de enrutamiento interno (OSPF, EIGRP, RIP) y servicios críticos. La interconexión entre ISPs se realiza mediante BGP utilizando la red `192.168.X.0/16`.

Se implementan reglas de comunicación específicas entre departamentos, servicios centralizados y una presentación ejecutiva para "vender" la solución a stakeholders.

---

## 3. Enunciado del Proyecto

Un país con una población en rápido crecimiento y una demanda de telecomunicaciones en auge ha decidido modernizar y ampliar su infraestructura de red para satisfacer las necesidades actuales y futuras de sus ciudadanos. Tres de las principales empresas de telecomunicaciones han sido seleccionadas para colaborar en este ambicioso proyecto de expansión: **Telecom Uno**, **Redes Nacionales** y **Link Global**.

El gobierno, junto con estas empresas, ha creado un comité de expertos que se encargarán del diseño, simulación y presentación de una solución eficiente, escalable y económicamente viable. Dado su extensa experiencia en el campo, el comité ha decidido seleccionarlo a usted para llevar a cabo este desafío.

El país cuenta con tres empresas de telecomunicaciones interesadas en optimizar sus redes internas y mejorar la interconectividad entre ellas para ofrecer un mejor servicio a nivel nacional. Cada ISP tiene necesidades y requerimientos específicos para sus redes internas, y, además, se debe garantizar una conectividad óptima y segura entre las tres empresas a través de un protocolo de enrutamiento común.

---

### 3.1. Requerimientos Importantes

#### 3.1.1. Interconexión de ISPs

- Se deben conectar los tres ISP: Telecom Uno, Redes Nacionales y Link Global.
- **Configuración de BGP para interconectar los routers principales de los ISP.**
- Para conectar los routers o switches de capa 3 se le asigna la dirección de red `192.168.X.0/16` **donde X son los últimos 2 dígitos de su carné.**
- Utilizando la siguiente topología (tres multilayer switches 3650-24PS interconectados en triángulo).
- El medio de conexión entre routers es **fibra óptica**.

#### 3.1.2. ISP 1: Telecom Uno

Para llevar a cabo la red de Telecom Uno, se le otorga una única red `172.16.1X.0/24`, donde la X corresponde al **último** dígito de su carné. Telecom Uno contará con dos departamentos:

1. Administración
2. Atención al cliente

El estudiante deberá realizar el proceso de subneteo de la dirección asignada para crear subredes que permitan la comunicación entre estos dos departamentos. El tamaño de las subredes queda a discreción del estudiante.

**Detalles:**
- Se solicita una topología en **árbol**.
- Configuración del protocolo de enrutamiento **OSPF**.
- La red debe contar con al menos **5 routers, 5 hosts y 2 enlaces LACP**.
- Esta empresa es la que provee el servicio **DNS y HTTP** a toda la topología.

#### 3.1.3. ISP 2: Redes Nacionales

Para llevar a cabo la red de Redes Nacionales, se le otorga una única red `172.16.2X.0/24`, donde la X corresponde al **último** dígito de su carné. Redes Nacionales contará con dos departamentos:

1. Ventas
2. Facturación

El estudiante deberá realizar el proceso de subneteo de la dirección asignada para crear subredes que permitan la comunicación entre estos dos departamentos. El tamaño de las subredes queda a discreción del estudiante.

**Detalles:**
- Se requiere una topología de **modelo jerárquico**.
- Configuración del protocolo de enrutamiento **OSPF**.
- La red debe tener al menos **5 routers, 5 hosts y 2 enlaces LACP**.
- Debido a la importancia de los departamentos en esta empresa se solicita que haya **redundancia de capa 3 obligatoriamente (HSRP)** para no perder comunicación en ningún momento.
- Esta empresa es la que proporciona el direccionamiento IP por medio de **DHCP** a toda la topología.

#### 3.1.4. ISP 3: Link Global

Para llevar a cabo la red de Link Global, se le otorga una única red `172.16.3X.0/24`, donde la X corresponde al **primer** dígito de su carné. Link Global contará con dos departamentos:

1. Soporte
2. Seguridad

El estudiante deberá realizar el proceso de subneteo de la dirección asignada para crear subredes que permitan la comunicación entre estos dos departamentos. El tamaño de las subredes queda a discreción del estudiante.

**Detalles:**
- Debe implementarse una topología de **hub and spoke**.
- Configuración del protocolo de enrutamiento **EIGRP**.
- La red debe contar con al menos **5 routers, 5 hosts y 2 enlaces LACP**.
- Debido a que desde aquí se controlan las ventas al cliente final, se requiere que esta topología tenga al menos un **router inalámbrico** que proporcione servicio WiFi.

---

### 3.2. Reglas de Comunicación

La comunicación entre departamentos de ambas vías es la siguiente:

| Departamento | Debe existir tráfico de salida con: | Debe existir tráfico de entrada con: |
|---|---|---|
| **Seguridad** | Todos los deptos. | Ningún departamento. |
| **Soporte** | Todos los deptos. | Todos los deptos. |
| **Administración** | Todos los deptos. | Todos los deptos. |
| **Atención al Cliente** | Ventas. | Ventas. |
| **Facturación** | Ventas. | Ventas. |
| **Ventas** | Facturación y Atención al Cliente. | Facturación y Atención al Cliente. |

---

### 3.3. Servidor WEB

- Proporcionará direccionamiento DNS para todos los dispositivos finales.
- El dominio para configurar será `www.proyecto2_#carné.com`
- Al ingresar el dominio en el buscador web deberá desplegarse una página web estática y mostrar los datos del estudiante e información básica del curso.

---

### 3.4. DHCP

- Todos los dispositivos finales deben obtener direccionamiento IP por medio de **DHCP**.

---

### 3.5. Presentación

Durante este proyecto no solamente debe configurar una topología de red, sino por la naturaleza de esta deben vender su idea a los dirigentes. Por ello deben realizar una presentación en PowerPoint, Canva o en el software a su elección con la información que consideren importante para poder vender su idea a todos los gobernantes, tal como la arquitectura seleccionada, despliegue de costos, dispositivos seleccionados y tecnologías utilizadas. Para ello deben tomar en cuenta factores como eficiencia, costo, innovación, etc. tanto a nivel de software como de hardware.

Deberán exponer sus puntos en la calificación **(mínimo 5 y máximo 10 minutos)** para demostrar que la red propuesta cumple con la finalidad que el país requiere.

---

### 3.6. Restricciones

- La práctica se realizará de forma **individual**.
- Deberán tener un claro conocimiento de la red propuesta.
- Las configuraciones deben realizarlas desde la **CLI**.
- En el repositorio creado para el proyecto 1 debe crearse una carpeta con nombre **"Proyecto 2"** en la cual se irá desarrollando el proyecto.
- La implementación de la red debe realizarse en **Cisco Packet Tracer** y el nombre del archivo debe ser `Proyecto2_#carné.pkt`.
- La presentación que expondrán puede ser del formato que deseen, tomando en cuenta que el nombre sea **`Proyecto2_Presentacion_#carné.pdf`**.
- **Durante la calificación solo tendrán un intento para demostrar comunicación entre topologías por medio de un ping.**
- **Para tener derecho a calificación el direccionamiento IP debe ser por medio de DHCP, no se calificará proyecto con direccionamiento estático.**
- **Para tener derecho a calificación debe demostrar funcionalidad en servidor DNS y HTTP.**
- **Para tener derecho a calificación debe demostrarse al menos una sección funcional con HSRP.**

---

### 3.7. Penalizaciones

- Falta de seguimiento de desarrollo continuo por medio de Gitlab o GitHub tendrá una penalización del **10%**.
- Falta de seguimiento de instrucciones conforme al método de entrega (nombre del repositorio) tendrá una penalización del **5%**.
- No implementar la topología indicada tendrá una penalización del **60%** de la nota obtenida.
- Falta de puntualidad conforme a la calificación (sin previo aviso) tendrá una penalización del **10%** de la nota obtenida.
- Falta de puntualidad conforme a la entrega tendrá una penalización de la siguiente manera:
  - 1 – 10 minutos: **10%**
  - 11 – 59 minutos: **30%**
  - Pasados 60 minutos tendrá una nota de **0** y no se calificará.
- **Las copias serán penalizadas con una nota de 0 y serán sancionados según lo indique el reglamento.**

---

### 3.8. Observaciones

- Software para utilizar: **Cisco Packet Tracer**.
- La entrega se realizará por medio de **UEDI**, cada estudiante deberá utilizar el repositorio creado para el proyecto 1. Se debe crear una carpeta con el nombre **Proyecto2**.

---

### 3.9. Entregables

- Enlace al repositorio (UEDI).
- Manual Técnico (Repositorio).
- Archivo `.pkt` (Repositorio).

---

## 4. Material de Apoyo

- Cisco Systems, Inc. (2023). *BGP Configuration Guide, Cisco IOS Release 15.2M&T*. Disponible en: https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_bgp/configuration/15-mt/irg-15-mt-book.html

- Cisco Systems, Inc. (2023). *OSPF Configuration Guide*. Disponible en: https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_ospf/configuration/15-mt/iro-15-mt-book/iro-cfg.html

- Cisco Systems, Inc. (2023). *EIGRP Configuration Guide*. Disponible en: https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_eigrp/configuration/15-mt/ire-15-mt-book/ire-cfg.html

- Cisco Systems, Inc. (2023). *RIP Configuration Guide*. Disponible en: https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_rip/configuration/15-mt/irr-15-mt-book/irr-cfg.html

---

## 5. Metodología

1. **Fase 1: Análisis y Planificación (Semana 1)**
   - Análisis de requisitos por ISP
   - Subneteo de redes asignadas (172.16.X.0/24)
   - Diseño de topologías específicas
   - Definición de reglas de comunicación

2. **Fase 2: Implementación Individual ISP (Semana 2)**
   - Configuración de protocolos internos (OSPF, EIGRP, RIP)
   - Implementación de servicios asignados (DNS, DHCP, HTTP)
   - Configuración de redundancia (HSRP) y enlaces (LACP/PAGP)
   - Setup de red inalámbrica (ISP 3)

3. **Fase 3: Integración BGP (Semana 3)**
   - Configuración de BGP entre routers principales
   - Establecimiento de vecinos BGP
   - Verificación de rutas intercambiadas
   - Pruebas de conectividad inter-ISP

4. **Fase 4: Validación y Documentación (Semana 3)**
   - Pruebas integrales de comunicación
   - Verificación de reglas de tráfico
   - Documentación en manual técnico
   - Preparación de presentación ejecutiva

---

## 6. Recursos y Herramientas a Utilizar

- **Software:** Packet Tracer
- **Plataformas:** UEDI, GitHub

---

## 7. Desarrollo de Habilidades Blandas

1. **Comunicación efectiva:** Explicar tecnicalidades a stakeholders no técnicos en la presentación.
2. **Colaboración equitativa:** Distribución balanceada de tareas entre 3-4 integrantes.
3. **Pensamiento crítico:** Resolver problemas de interoperabilidad entre diferentes protocolos.
4. **Creatividad e innovación:** Diseñar soluciones escalables y económicamente viables.
5. **Gestión del tiempo:** Cumplir con el cronograma ajustado.
6. **Adaptabilidad:** Trabajar con múltiples tecnologías y requisitos simultáneos.
7. **Responsabilidad:** Garantizar que la infraestructura propuesta sea confiable y segura.

---

## 8. Cronograma

| Tipo | Fecha Inicio | Fecha Fin |
|---|---|---|
| Asignación de Proyecto | 11/04/2026 | 11/04/2026 |
| Elaboración | 11/04/2026 | 01/05/2026 |
| Calificación | 02/05/2026 | 04/05/2026 |

---

## 9. Rúbrica de Calificación

### Requisitos previos para calificación

| Tema | Descripción | Cumple (Sí/No) |
|---|---|---|
| Cumplimiento de la entrega | Entrega puntual en la plataforma UEDI. | |
| Direccionamiento IP | Debe ser por DHCP. | |

### Resumen de Puntuaciones

| Descripción de ponderación | Puntos totales | Puntos obtenidos |
|---|:---:|:---:|
| **Telecom Uno** | **21** | |
| Topología de árbol bien diseñada | 4 | |
| OSPF utilizado como protocolo de enrutamiento | 4 | |
| Configuración de al menos 2 enlaces LACP y utilización de la cantidad de dispositivos mínimos solicitados | 3 | |
| Dirección de red 172.16.1X.0/24 utilizada como base | 2 | |
| Subneteo de la dirección base para las 2 subredes (Administración y Atención al cliente) | 2 | |
| Configuración correcta de DNS para toda la topología | 4 | |
| Listas de Control de Acceso correctas | 2 | |
| **Redes Nacionales** | **23** | |
| Topología de modelo jerárquico bien diseñada | 4 | |
| OSPF utilizado como protocolo de enrutamiento | 4 | |
| Configuración de al menos 2 enlaces LACP y utilización de la cantidad de dispositivos mínimos solicitados | 3 | |
| Dirección de red 172.16.2X.0/24 utilizada como base | 2 | |
| Subneteo de la dirección base para las 2 subredes (Ventas y Facturación) | 2 | |
| Configuración e implementación correcta de HSRP | 4 | |
| Direccionamiento DHCP correctamente configurado para toda la topología | 3 | |
| Listas de Control de Acceso correctas | 1 | |
| **Link Global** | **21** | |
| Topología hub and spoke bien diseñada | 4 | |
| EIGRP utilizado como protocolo de enrutamiento | 4 | |
| Configuración de al menos 2 enlaces LACP y utilización de la cantidad de dispositivos mínimos solicitados | 3 | |
| Dirección de red 172.16.3X.0/24 utilizada como base | 2 | |
| Subneteo de la dirección base para las 2 subredes (Soporte y Seguridad) | 2 | |
| Implementación correcta de red inalámbrica | 4 | |
| Listas de control de acceso correctas | 2 | |
| **Conexión entre ISPs** | **10** | |
| Configuración de la topología indicada | 2 | |
| Configuración de BGP en cada uno de los routers o switches multicapa | 6 | |
| Subneteo de la red 192.168.X.0/16 para las interfaces, utilizando una máscara eficiente | 2 | |
| **Comunicación entre departamentos** | **10** | |
| Administración - Ventas | 3 | |
| Facturación - Administración | 3 | |
| Soporte - Ventas | 2 | |
| Atención al cliente – Facturación | 2 | |
| **Presentación** | **10** | |
| Descripción de las topologías utilizadas (1pt c/u) | 3 | |
| Descripción de los dispositivos utilizados | 3 | |
| Lista de precios | 2 | |
| Tecnologías y protocolos utilizados | 2 | |
| **Evaluación de conocimiento** | **5** | |
| Explicación de trabajo realizado | 5 | |

### Penalizaciones

| Penalización | Porcentaje |
|---|---|
| Topología diferente a la solicitada | -60% |
| Sin seguimiento por medio de Github o Gitlab | -10% |
| Nombre del repositorio o organización de carpetas errónea | -5% |
| Impuntualidad en la calificación | -10% |
| Entrega tarde (1–10 min) | -10% |
| Entrega tarde (11–59 min) | -30% |
| Entrega tarde (más de 60 min) | -100% (nota 0) |
| El estudiante no se presenta a la calificación | -10% |

| | |
|---|---|
| **TOTAL** | **100** |