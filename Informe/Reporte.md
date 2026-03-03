
# Pentest Report - Metasploitable 2

# English:

# Penetration Testing Report

**Date:** 23/02/2026  

---

## Objective

Evaluate the security posture of the internal server identified as **192.168.1.57** and determine potential compromise vectors that could allow unauthorized access.

---

## Scope

**Target Machine:** 192.168.1.57

---

## Test Type

Black Box Assessment

---

## Methodology

OWASP Testing Methodology / PTES (Lab-adapted)

---

## Reconnaissance

![](https://github.com/aliciablanc/Portfolio/blob/main/Evidencias/VirtualBox_kali-linux-2025.4-virtualbox-amd64_19_02_2026_19_01_25.png)

An active scan was performed against the target host (**192.168.1.57**) using the **Nmap** network reconnaissance tool in order to identify exposed services and associated versions.

Service version detection techniques and basic enumeration scripts were used to gather additional context about available services.

The scan revealed multiple open ports:

- **21/tcp – FTP (vsftpd 2.3.4)**
- **22/tcp – SSH (OpenSSH 4.7p1)**
- **80/tcp – HTTP (Apache 2.2.8)**
- **445/tcp – SMB**

The presence of outdated service versions suggests potential exposure to known vulnerabilities, justifying further service-specific enumeration.

---

## Enumeration

During the enumeration phase, it was identified that the FTP service exposed on port 21 allows authentication through the **anonymous** user without requiring valid credentials.

Although no sensitive files were discovered in the root directory during testing, enabling anonymous access increases the attack surface and could facilitate information enumeration or data exfiltration if content were stored on the server.

### Impact

Possible unauthorized file exposure and facilitation of further attacker reconnaissance.

### Recommendation

Disable anonymous access or restrict it strictly to controlled environments containing no sensitive information.

---

## Exploitation

During enumeration, the exposed FTP service was identified as running **vsftpd 2.3.4**, a version affected by a known vulnerability allowing remote command execution through a backdoor introduced in compromised software releases.

After validating the vulnerability, remote access to the target system was successfully established without valid authentication, obtaining a shell with elevated privileges.

![](https://github.com/aliciablanc/Portfolio/blob/main/Evidencias/VirtualBox_kali-linux-2025.4-virtualbox-amd64_23_02_2026_20_55_00.png)

### Evidence

- Identification of the vulnerable service through version scanning.
- Validation of the public exploit associated with the vulnerability.
- Successful remote access with root privileges.

### Impact

Successful exploitation allows an attacker to:

- Execute arbitrary commands.
- Fully compromise the system.
- Access, modify, or delete sensitive information.
- Use the system as a pivot point toward other internal assets.

![](https://github.com/aliciablanc/Portfolio/blob/main/Evidencias/VirtualBox_kali-linux-2025.4-virtualbox-amd64_23_02_2026_21_04_21.png)

### Severity

**Critical**

### Recommendation

- Immediately update the FTP service to a secure version.
- Remove unnecessary exposed services.
- Implement network segmentation and access controls.

---

## Executive Summary

Exploitation of the vulnerable FTP service allowed superuser privileges to be obtained on the target system. An attacker with this level of access could access all stored information, modify critical configurations, maintain persistence within the system, or use the server as a launch point for attacks against other internal network assets.

The achieved compromise demonstrates that an external attacker with network access could obtain full administrative control of the system with a low level of effort.

---

## Attack Path

1. Discovery of an active host within the internal network.
2. Identification of exposed services through Nmap scanning.
3. Enumeration of FTP service with anonymous access enabled.
4. Identification of vulnerable version vsftpd 2.3.4.
5. Remote backdoor exploitation.
6. Interactive shell obtained.
7. Root access confirmed.
8. Full system compromise.

---

## Lessons Learned

- Accurate service version enumeration is critical for identifying exploitable vulnerabilities.
- Seemingly low-priority services (such as FTP) can lead to full system compromise.
- Exposure of outdated software represents one of the most critical risks in corporate environments.

# Español

**Fecha:** 23/02/2026

**Objetivo:** Evaluar la postura de seguridad del servidor interno identificado como 192.168.1.57 y determinar posibles vectores de compromiso que permitan acceso no autorizado.

**Scope:** Máquina 192.168.1.57

**Tipo de prueba:** Caja negra

**Metodología:** OWASP Testing Methodology / PTES (adaptada a laboratorio)

## Reconocimiento

![](https://github.com/aliciablanc/Portfolio/blob/main/Evidencias/VirtualBox_kali-linux-2025.4-virtualbox-amd64_19_02_2026_19_01_25.png)

Se realizó un escaneo activo contra el host objetivo (192.168.1.57) utilizando la herramienta de reconocimiento de red Nmap, con el propósito de identificar servicios expuestos y versiones asociadas.

Se emplearon técnicas de detección de versiones y scripts básicos de enumeración para obtener mayor contexto sobre los servicios disponibles.

El escaneo reveló múltiples puertos abiertos, entre ellos:

21/tcp – FTP (vsftpd 2.3.4)

22/tcp – SSH (OpenSSH 4.7p1)

80/tcp – HTTP (Apache 2.2.8)

445/tcp – SMB

La presencia de versiones antiguas sugiere una posible exposición a vulnerabilidades conocidas, lo que justifica una fase posterior de enumeración específica por servicio.

## Enumeración

Durante la fase de enumeración se identificó que el servicio FTP expuesto en el puerto 21 permite autenticación mediante el usuario “anonymous” sin requerir credenciales válidas.

Si bien no se encontraron archivos sensibles accesibles en el directorio raíz durante la prueba, la habilitación de acceso anónimo incrementa la superficie de ataque y podría facilitar la enumeración de información o la exfiltración de datos en caso de existir contenido almacenado en el servidor.

**Impacto**

Posible exposición no autorizada de archivos y facilitación de reconocimiento adicional por parte de un atacante.

**Recomendación**

Deshabilitar el acceso anónimo o restringirlo únicamente a entornos controlados y sin información sensible.

## Explotación

Durante la fase de enumeración se identificó que el servicio FTP expuesto ejecutaba la versión vsftpd 2.3.4, la cual posee una vulnerabilidad conocida que permite la ejecución remota de comandos a través de un backdoor introducido en versiones comprometidas del software.

Tras validar la vulnerabilidad, se logró establecer acceso remoto al sistema objetivo sin autenticación válida, obteniendo una shell con privilegios elevados.

![](https://github.com/aliciablanc/Portfolio/blob/main/Evidencias/VirtualBox_kali-linux-2025.4-virtualbox-amd64_23_02_2026_20_55_00.png)

**Evidencia**

Identificación del servicio vulnerable mediante escaneo de versiones.

Validación del exploit público asociado a la vulnerabilidad.

Obtención de acceso remoto con privilegios de root.

**Impacto**

La explotación exitosa permite a un atacante:

Ejecutar comandos arbitrarios.

Comprometer completamente el sistema.

Acceder, modificar o eliminar información sensible.

Utilizar el sistema como punto de pivote hacia otros activos internos.

![](https://github.com/aliciablanc/Portfolio/blob/main/Evidencias/VirtualBox_kali-linux-2025.4-virtualbox-amd64_23_02_2026_21_04_21.png)

**Severidad**

Crítica

**Recomendación**

Actualizar inmediatamente el servicio FTP a una versión segura.

Eliminar servicios innecesarios expuestos.

Implementar segmentación de red y controles de acceso.

## Resumen

La explotación del servicio FTP vulnerable permitió obtener privilegios de superusuario en el sistema objetivo. Un atacante con este nivel de acceso podría acceder a toda la información almacenada, modificar configuraciones críticas, mantener persistencia en el sistema o utilizar el servidor como punto de ataque hacia otros activos de la red interna. El compromiso obtenido demuestra que un atacante externo con acceso a la red podría alcanzar control administrativo completo del sistema con un nivel de esfuerzo bajo.

## Attack Path

1. Descubrimiento del host activo en red interna.
2. Identificación de servicios expuestos mediante escaneo Nmap.
3. Enumeración del servicio FTP con acceso anónimo habilitado.
4. Identificación de versión vulnerable vsftpd 2.3.4.
5. Explotación de backdoor remoto.
6. Obtención de shell interactiva.
7. Acceso root confirmado.
8. Compromiso total del sistema.

## Lecciones Aprendidas

- La enumeración precisa de versiones de servicio es crítica para identificar vulnerabilidades explotables.
- Servicios aparentemente poco relevantes (FTP) pueden derivar en compromiso total del sistema.
- La exposición de software desactualizado representa uno de los riesgos más críticos en entornos corporativos.
