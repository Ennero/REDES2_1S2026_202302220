# Universidad San Carlos de Guatemala
## Facultad de Ingeniería. Ingeniería en Ciencias y Sistemas

**CYS UBAR FIUSAC**
**FACULTAD DE INGENIERÍA**
**UNIVERSIDAD DE SAN CARLOS DE GUATEMALA**
**Cococys**
**Proyecto 2: Telecom Uno, Redes Nacionales y Link Global: Conectando al país.**

---

### **PONDERACIÓN:** 25
### **Horas Aproximadas:** 30

---

## **Índice**
   **Tema**                          | **Página** |
 |-----------------------------------|------------|
 | 1. MARCO FORMATIVO                | 3          |
 | 1.1. Valor                        | 3          |
 | 1.2. Competencia(s)               | 3          |
 | 1.3. Objetivo SMART               | 3          |
 | 2. Resumen Ejecutivo              | 4          |
 | 3. Enunciado del Proyecto         | 4          |
 | 3.1. Requerimientos importantes   | 5          |
 | 3.1.1. Interconexión de ISPs      | 5          |
 | 3.1.2. ISP 1: Telecom Uno         | 5          |
 | 3.1.3. ISP 2: Redes Nacionales   | 6          |
 | 3.1.4. ISP 3: Conexiones Futuras  | 7          |
 | 3.2. Reglas de comunicación       | 8          |
 | 3.3. Servidor WEB                 | 8          |
 | 3.4. DHCP                         | 8          |
 | 3.5. Presentación                 | 8          |
 | 3.6. Restricciones                | 9          |
 | 3.8. Observaciones                | 10         |
 | 3.9. Entregables                  | 10         |
 | 4. Material de apoyo              | 11         |
 | 5. Metodología                    | 11         |
 | 7. Desarrollo de Habilidades Blandas | 12      |
 | 8. Cronograma                     | 12         |
 | 9. Rúbrica de Calificación        | 13         |

---

## **1. MARCO FORMATIVO**

### **1.1. Valor**
 | **Nombre del valor**            | **¿Cómo se aplica en tu laboratorio?**                                                                                     |
 |---------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
 | Responsabilidad profesional     | El estudiante debe garantizar que la red diseñada cumpla con los plazos establecidos, las especificaciones técnicas requeridas y considere el impacto social de su propuesta. La responsabilidad se demuestra mediante configuraciones robustas, documentación transparente y cumplimiento de fechas de entrega. |

### **1.2. Competencia(s)**
 | **Tipo de Competencia**      | **Descripción**                                                                                                             |
 |-------------------------------|-----------------------------------------------------------------------------------------------------------------------------|
 | **Competencia General**       | El estudiante configura la agregación de enlaces utilizando los protocolos LACP o PAgP para mejorar el ancho de banda y la tolerancia a fallos en redes Ethernet. |
 | **Competencia Específica**    | El estudiante identifica los principios de funcionamiento de los protocolos de enrutamiento dinámico mediante la comparación de sus características, ventajas y desventajas para utilizarlos en la implementación de redes de forma eficiente y segura. |

### **1.3. Objetivo SMART**
 | **SMART**         | **Objetivo redactado**                                                                                                      |
 |-------------------|-----------------------------------------------------------------------------------------------------------------------------|
 | **Específico**    | Configurar 3-4 ISPs interconectados mediante BGP, cada uno con topología específica (árbol, jerárquica, hub-spoke, malla), protocolos de enrutamiento definidos (OSPF, EIGRP, RIP), servicios de DNS, DHCP, HTTP, y cumplir reglas de comunicación entre departamentos. |
 | **Medible**       | • Al menos 5 routers y 5 hosts por ISP<br>• 2 enlaces LACP/PAGP por ISP<br>• Comunicación funcional según tabla de reglas<br>• Servicios DNS, DHCP y HTTP operativos |
 | **Alcanzable**    | Utilizando Cisco Packet Tracer, equipos virtuales Cisco y protocolos estándar de red.                                      |
 | **Realista**      | Desarrolla competencias en redes WAN, BGP, enrutamiento interdominio, servicios de red empresariales y diseño de infraestructuras escalables. |
 | **A Tiempo**      | Fecha de entrega: **02/05/2026**                                                                                           |

---

## **2. Resumen Ejecutivo**

El proyecto consiste en diseñar e implementar una infraestructura nacional de telecomunicaciones que interconecte tres o cuatro proveedores de servicio (ISP). Cada ISP tiene requisitos específicos de topología, protocolos de enrutamiento interno (OSPF, EIGRP, RIP) y servicios críticos. La interconexión entre ISPs se realiza mediante BGP utilizando la red **192.168.X.0/16**. Se implementan reglas de comunicación específicas entre departamentos, servicios centralizados y una presentación ejecutiva para "vender" la solución a stakeholders.

---
## **3. Enunciado del Proyecto**

Un país con una población en rápido crecimiento y una demanda de telecomunicaciones en auge ha decidido modernizar y ampliar su infraestructura de red para satisfacer las necesidades actuales y futuras de sus ciudadanos. Tres de las principales empresas de telecomunicaciones han sido seleccionadas para colaborar en este ambicioso proyecto de expansión: **Telecom Uno, Redes Nacionales y Link Global**.

El gobierno, junto con estas empresas, ha creado un comité de expertos que se encargarán del diseño, simulación y presentación de una solución eficiente, escalable y económicamente viable. Dado su extensa experiencia en el campo, el comité ha decidido seleccionarlo a usted para llevar a cabo este desafío.

El país cuenta con tres empresas de telecomunicaciones interesadas en optimizar sus redes internas y mejorar la interconectividad entre ellas para ofrecer un mejor servicio a nivel nacional. Cada ISP tiene necesidades y requerimientos específicos para sus redes internas, y, además, se debe garantizar una conectividad óptima y segura entre las tres empresas a través de un protocolo de enrutamiento común.

---

### **3.1. Requerimientos importantes**

#### **3.1.1. Interconexión de ISPs**
- Se deben conectar los tres ISP: **Telecom Uno, Redes Nacionales y Link Global**.
- Configuración de **BGP** para interconectar los routers principales de los ISP.
- Para conectar los routers o switches de capa 3 se asigna la dirección de red **192.168.X.0/16**, donde **X** son los últimos 2 dígitos de su carné.
- **Topología:** El medio de conexión entre routers es fibra óptica.
 | **Dispositivo**                     | **Cantidad** |
 |-------------------------------------|--------------|
 | 3650-24PS Multilayer Switch         | 23           |
 | 3650-24PS Multilayer Switch         | 12           |
 | 3650-24PS Multilayer Switch         | 10           |

---

#### **3.1.2. ISP 1: Telecom Uno**
- Red asignada: **172.16.1X.0/24** (X = último dígito del carné).
- Departamentos: **Administración y Atención al cliente**.
- **Topología:** Árbol.
- **Protocolo de enrutamiento:** OSPF.
- **Requisitos:**
  - Al menos 5 routers, 5 hosts y 2 enlaces LACP.
  - Esta empresa provee los servicios **DNS y HTTP** a toda la topología.

---
#### **3.1.3. ISP 2: Redes Nacionales**
- Red asignada: **172.16.2X.0/24** (X = último dígito del carné).
- Departamentos: **Ventas y Facturación**.
- **Topología:** Modelo jerárquico.
- **Protocolo de enrutamiento:** OSPF.
- **Requisitos:**
  - Al menos 5 routers, 5 hosts y 2 enlaces LACP.
  - **Redundancia de capa 3 obligatoria (HSRP)**.
  - Esta empresa proporciona direccionamiento IP por medio de **DHCP** a toda la topología.

---
#### **3.1.4. ISP 3: Conexiones Futuras**
- Red asignada: **172.16.3X.0/24** (X = primer dígito del carné).
- Departamentos: **Soporte y Seguridad**.
- **Topología:** Hub n Spoke.
- **Protocolo de enrutamiento:** EIGRP.
- **Requisitos:**
  - Al menos 5 routers, 5 hosts y 2 enlaces LACP.
  - **Router inalámbrico** para proporcionar servicio WiFi.

---
### **3.2. Reglas de comunicación**
 | **Departamento**      | **Tráfico de salida con:**               | **Tráfico de entrada con:**              |
 |-----------------------|------------------------------------------|------------------------------------------|
 | Seguridad             | Todos los deptos.                        | Ningún departamento.                    |
 | Soporte               | Todos los deptos.                        | Todos los deptos.                        |
 | Administración        | Todos los deptos.                        | Todos los deptos.                        |
 | Atención al Cliente   | Ventas.                                 | Ventas.                                 |
 | Ventas               | Facturación y Atención al Cliente.      | Administración y Atención al Cliente.   |
 | Facturación           | Ventas.                                 | Ventas.                                 |

---
### **3.3. Servidor WEB**
- Proporcionará direccionamiento **DNS** para todos los dispositivos finales.
- **Dominio:** `www.proyecto2_#carné.com`
- Al ingresar el dominio en el buscador web, se debe desplegar una **página web estática** con los datos del estudiante e información básica del curso.

---
### **3.4. DHCP**
Todos los dispositivos finales deben obtener direccionamiento IP por medio de **DHCP**.

---
### **3.5. Presentación**
Durante este proyecto, no solo debe configurar una topología de red, sino también **vender su idea a los dirigentes**. Debe realizar una presentación en **PowerPoint, Canva o el software de su elección** con la información relevante para convencer a los gobernantes, incluyendo:
- Arquitectura seleccionada.
- Despliegue de costos.
- Dispositivos seleccionados.
- Tecnologías utilizadas.
- Factores como eficiencia, costo e innovación.

**Duración de la exposición:** Mínimo 5 y máximo 10 minutos.

---
### **3.6. Restricciones**
- La práctica se realizará de forma **individual**.
- Debe tener un **claro conocimiento de la red propuesta**.
- Las configuraciones deben realizarse desde la **CLI**.
- En el repositorio creado para el **Proyecto 1**, debe crearse una carpeta llamada **"Proyecto 2"**.
- La implementación de la red debe realizarse en **Cisco Packet Tracer** (nombre del archivo: `Proyecto2_#carné.pkt`).
- La presentación debe tener el nombre: `Proyecto2_Presentacion_#carné.pdf`.
- Solo tendrán **un intento** para demostrar comunicación entre topologías mediante un **ping**.
- **Derecho a calificación:**
  - Direccionamiento IP por DHCP.
  - Funcionalidad en servidor **DNS y HTTP**.
  - Al menos una sección funcional con **HSRP**.

---
### **3.8. Observaciones**
- **Software a utilizar:** Cisco Packet Tracer.
- **Entrega:** Por medio de **UEDI**, en el repositorio creado para el Proyecto 1 (carpeta "Proyecto 2").

---
### **3.9. Entregables**
- Enlace al repositorio (UEDI).
- Manual Técnico (en el repositorio).
- Archivo `.pkt` (en el repositorio).

---
## **4. Material de apoyo**
- [Cisco BGP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_bgp/configuration/15-mt/irg-15-mt-book.html)
- [Cisco OSPF Configuration Guide](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_ospf/configuration/15-mt/iro-15-mt-book/iro-cfg.html)
- [Cisco EIGRP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_eigrp/configuration/15-mt/ire-15-mt-book/ire-cfq.html)
- [Cisco RIP Configuration Guide](https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/iproute_rip/configuration/15-mt/irr-15-mt-book/irr-cfq.html)

---
## **5. Metodología**
 | **Fase**                          | **Descripción**                                                                                     |
 |-----------------------------------|-----------------------------------------------------------------------------------------------------|
 | **Fase 1: Análisis y Planificación** (Semana 1) | Análisis de requisitos por ISP, subneteo de redes, diseño de topologías y definición de reglas de comunicación. |
 | **Fase 2: Implementación Individual ISP** (Semana 2) | Configuración de protocolos internos, implementación de servicios asignados, configuración de redundancia y enlaces. |
 | **Fase 3: Integración BGP** (Semana 3) | Configuración de BGP entre routers principales, establecimiento de vecinos BGP y verificación de rutas. |
 | **Fase 4: Validación y Documentación** (Semana 3) | Pruebas integrales de comunicación, documentación en manual técnico y preparación de presentación ejecutiva. |

---
## **6. Recursos y herramientas a utilizar**
- **Software:** Packet Tracer.
- **Plataformas:** UEDI, GitHub.

---
## **7. Desarrollo de Habilidades Blandas**
 | **Habilidad**            | **Descripción**                                                                                     |
 |---------------------------|-----------------------------------------------------------------------------------------------------|
 | Comunicación efectiva     | Explicar tecnicalidades a stakeholders no técnicos en la presentación.                            |
 | Colaboración equitativa   | Distribución balanceada de tareas entre 3-4 integrantes.                                          |
 | Pensamiento crítico       | Resolver problemas de interoperabilidad entre diferentes protocolos.                              |
 | Creatividad e innovación  | Diseñar soluciones escalables y económicamente viables.                                            |
 | Gestión del tiempo        | Cumplir con el cronograma ajustado.                                                                |
 | Adaptabilidad             | Trabajar con múltiples tecnologías y requisitos simultáneos.                                       |
 | Responsabilidad           | Garantizar que la infraestructura propuesta sea confiable y segura.                                |

---
## **8. Cronograma**
 | **Tipo**                  | **Fecha Inicio** | **Fecha Fin**   | **Asignación**          |
 |---------------------------|------------------|-----------------|-------------------------|
 | Asignación de Proyecto   | 11/04/2026       | 11/04/2026      |                         |
 | Elaboración               | 11/04/2026       | 01/05/2026      |                         |
 | Calificación              | 02/05/2026       | 04/05/2026      |                         |

---
## **9. Rúbrica de Calificación**
 | **Tema**                          | **Descripción**                                                                                     | **Cumple (Sí/No)** | **Puntos totales** |
 |-----------------------------------|-----------------------------------------------------------------------------------------------------|---------------------|--------------------|
 | **Cumplimiento de la entrega**   | Entrega puntual en la plataforma UEDI.                                                             |                     |                    |
 | **Direccionamiento IP**           | Debe ser por DHCP.                                                                                 |                     |                    |
 | **Telecom Uno**                   | Topología de árbol bien diseñada, OSPF, enlaces LACP, subneteo, DNS, ACLs.                          |                     | 21                 |
 | **Redes Nacionales**              | Topología jerárquica, OSPF, enlaces LACP, subneteo, HSRP, DHCP, ACLs.                              |                     | 23                 |
 | **Conexiones Futuras**            | Topología hub n spoke, EIGRP, enlaces LACP, subneteo, red inalámbrica, ACLs.                        |                     | 21                 |
 | **Conexión entre ISPs**           | Configuración de la topología, BGP, subneteo de 172.X.0.0/16.                                      |                     | 10                 |
 | **Comunicación entre departamentos** | Administración-Ventas, Facturación-Administración, Soporte-Ventas, Atención al Cliente-Facturación. |                     | 10                 |
 | **Presentación**                  | Descripción de topologías, dispositivos, lista de precios, tecnologías y evaluación de conocimiento. |                     | 10                 |

---
### **PENALIZACIONES**
- Topología diferente a la solicitada: **-60%**
- Sin seguimiento por GitHub/GitLab: **-10%**
- Nombre del repositorio erróneo: **-5%**
- Impuntualidad en calificación: **-10%**
- Entrega tarde:
  - 1-10 minutos: **-10%**
  - 11-59 minutos: **-30%**
  - Más de 60 minutos: **Nota 0**
- No presentarse a la calificación: **-10%**
- Copias: **Nota 0 y sanción según reglamento**

---
**TOTAL:** 100 puntos