
# Pentest Report - Metasploitable 2

**Fecha:** 23/02/2026
**Objetivo:** Evaluar la postura de seguridad del servidor interno identificado como 192.168.1.57 y determinar posibles vectores de compromiso que permitan acceso no autorizado.
**Scope:** Máquina 192.168.1.57
**Tipo de prueba:** Caja negra
**Metodología:** OWASP Testing Methodology / PTES (adaptada a laboratorio)
## Reconocimiento

![[VirtualBox_kali-linux-2025.4-virtualbox-amd64_19_02_2026_19_01_25.png]]

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

![[VirtualBox_kali-linux-2025.4-virtualbox-amd64_23_02_2026_20_55_00.png]]

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

![[VirtualBox_kali-linux-2025.4-virtualbox-amd64_23_02_2026_21_04_21.png]]

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