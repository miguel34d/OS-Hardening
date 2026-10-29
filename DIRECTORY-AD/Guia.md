<div align="center">

# Operating Systems Security - Practice 1 / Seguridad de Sistemas Operativos - Práctica 1

![Institute](https://img.shields.io/badge/Institute-ITLA-0A3D91?style=for-the-badge)
![Career](https://img.shields.io/badge/Career-Information_Security-1F6FEB?style=for-the-badge)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=for-the-badge)

![Windows Server](https://img.shields.io/badge/Windows_Server-2022_Standard_Evaluation-0078D4?style=flat-square&logo=windows&logoColor=white)
![Windows 10](https://img.shields.io/badge/Client-Windows_10_Pro-00A4EF?style=flat-square&logo=windows10&logoColor=white)
![VMware](https://img.shields.io/badge/Hypervisor-VMware_Workstation_Pro_17-607078?style=flat-square&logo=vmware&logoColor=white)
![Active Directory](https://img.shields.io/badge/Service-Active_Directory_DS-F25022?style=flat-square)
![Domain](https://img.shields.io/badge/Domain-MiguelRM.local-5C2D91?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-2EA043?style=flat-square)

[English](#english) | [Español](#espanol)

</div>

---

<a id="english"></a>

# English

**Implementation evidence: Windows Server 2022 with Active Directory and a Windows 10 client**

## General information

| Field | Detail |
|---|---|
| **Student** | Miguel Ramirez Meli |
| **Student ID** | 20251367 |
| **Career** | Information Security |
| **Institution** | Instituto Tecnológico de Las Américas (ITLA) |
| **Course** | Operating Systems Security |
| **Instructor** | Juan Alexander Ramírez |
| **Date** | 2026-01-25 |

## Environment summary

| Component | Value |
|---|---|
| **Hypervisor** | VMware Workstation Pro 17 |
| **Server** | Windows Server 2022 Standard Evaluation (Desktop Experience) |
| **Client** | Windows 10 Pro |
| **ISO image** | `SERVER_EVAL_x64FRE_es-es.iso` |
| **Server VM name** | MiguelRM |
| **Virtual disk** | 60 GB (split into multiple files) |
| **Domain** | `MiguelRM.local` |
| **NetBIOS** | MIGUELRM |
| **Forest and domain functional level** | Windows Server 2016 |
| **Client computer name** | WS0829 |
| **Client preferred DNS** | Server IP (e.g.: 192.168.0.10) |
| **Client alternate DNS** | 8.8.8.8 |

## Objectives summary

| Part | Objective | Status |
|---|---|---|
| A | Windows Server 2022 installation on VMware | ![Completed](https://img.shields.io/badge/-Completed-2EA043?style=flat-square) |
| B | AD DS installation and domain creation | ![Completed](https://img.shields.io/badge/-Completed-2EA043?style=flat-square) |
| C | Windows 10 client joined to the domain | ![Completed](https://img.shields.io/badge/-Completed-2EA043?style=flat-square) |

## Part A - Windows Server 2022 installation on VMware

![Part A](https://img.shields.io/badge/Part-A-0078D4?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-2EA043?style=flat-square)

### A.1 Virtual machine creation

| Parameter | Applied configuration |
|---|---|
| Wizard mode | Typical (recommended) |
| Installation source | Installer disc image file (ISO) |
| Image used | `SERVER_EVAL_x64FRE_es-es.iso` |
| VMware detection | Windows Server 2022 with Easy Install enabled |
| Product key | Not entered (warning accepted with Yes) |
| VM name | MiguelRM |
| Disk size | 60 GB |
| Disk type | Split virtual disk into multiple files |
| Additional adjustment | Floppy device removed in Edit virtual machine settings to avoid Easy Install errors |

### A.2 Operating system installation

| Parameter | Applied configuration |
|---|---|
| Installation language | Spanish |
| Edition | Windows Server 2022 Standard Evaluation - Desktop Experience |
| Installation type | Custom: install the operating system only |
| Partition | 60 GB unallocated |
| Restart | Automatic when the installation finished |
| Initial access | Administrator password set; sign-in using Right Ctrl + Right Alt + Insert |

### A.3 Computer rename

| Item | Detail |
|---|---|
| Location | Server Manager, Local Server, computer name, Change |
| Assigned name | MiguelRM |
| Subsequent action | Server restart |

## Part B - Active Directory activation and domain configuration

![Part B](https://img.shields.io/badge/Part-B-5C2D91?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-2EA043?style=flat-square)

### B.1 Active Directory Domain Services role installation

| Parameter | Applied configuration |
|---|---|
| Tool | Server Manager, Add Roles and Features |
| Installation type | Role-based or feature-based installation |
| Destination server | Local server |
| Selected role | Active Directory Domain Services |
| Dependencies | Additional features added |
| Restart | Restart the destination server automatically if required |

### B.2 Domain controller promotion

| Parameter | Applied configuration |
|---|---|
| Deployment operation | Add a new forest |
| Root domain name | `MiguelRM.local` |
| Forest functional level | Windows Server 2016 |
| Domain functional level | Windows Server 2016 |
| DSRM password | Set |
| DNS delegation | Not configured |
| NetBIOS name | MIGUELRM (assigned automatically) |
| Prerequisites check | Passed |
| Restart | Automatic after installation |

### B.3 Domain verification

| Check | Result |
|---|---|
| Location | Server Manager, Local Server |
| Domain displayed | `MiguelRM.local` |

## Part C - Joining a Windows 10 client to the domain

![Part C](https://img.shields.io/badge/Part-C-00A4EF?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-2EA043?style=flat-square)

### C.1 Client network configuration

| Parameter | Applied configuration |
|---|---|
| Path | Control Panel, Network and Internet, Network and Sharing Center, Change adapter settings |
| Adapter | Ethernet0 |
| Protocol | Internet Protocol Version 4 (TCP/IPv4) |
| Preferred DNS | Server IP (e.g.: 192.168.0.10, verified with `ipconfig`) |
| Alternate DNS | 8.8.8.8 |

### C.2 Domain join

| Parameter | Applied configuration |
|---|---|
| Path | This PC, Properties, Change settings, Change |
| Computer name | WS0829 |
| Member of | Domain: `MiguelRM.local` |
| Credentials | Domain Administrator |
| Result | Message confirming a successful join to `MiguelRM.local` |
| Subsequent action | Client computer restart |

### C.3 Sign-in verification

| Check | Result |
|---|---|
| Sign-in screen | Other user option available |
| Sign-in domain | MIGUELRM |

## Conclusion

| Aspect | Result |
|---|---|
| Server | Windows Server 2022 installed and operational on VMware |
| Domain | `MiguelRM.local` created through domain controller promotion |
| Client | Windows 10 (WS0829) successfully joined to the domain |
| Authentication | MIGUELRM domain sign-in available on the client |

---

<a id="espanol"></a>

# Español

**Evidencia de implementación: Windows Server 2022 con Active Directory y cliente Windows 10**

## Datos generales

| Campo | Detalle |
|---|---|
| **Estudiante** | Miguel Ramirez Meli |
| **Matrícula** | 20251367 |
| **Carrera** | Seguridad Informática |
| **Institución** | Instituto Tecnológico de Las Américas (ITLA) |
| **Materia** | Seguridad de Sistemas Operativos |
| **Instructor** | Juan Alexander Ramírez |
| **Fecha** | 25-01-2026 |

## Resumen del entorno

| Componente | Valor |
|---|---|
| **Hipervisor** | VMware Workstation Pro 17 |
| **Servidor** | Windows Server 2022 Standard Evaluation (Experiencia de escritorio) |
| **Cliente** | Windows 10 Pro |
| **Imagen ISO** | `SERVER_EVAL_x64FRE_es-es.iso` |
| **Nombre de la VM servidor** | MiguelRM |
| **Disco virtual** | 60 GB (dividido en múltiples archivos) |
| **Dominio** | `MiguelRM.local` |
| **NetBIOS** | MIGUELRM |
| **Nivel funcional (bosque y dominio)** | Windows Server 2016 |
| **Nombre del equipo cliente** | WS0829 |
| **DNS preferido del cliente** | IP del servidor (ej.: 192.168.0.10) |
| **DNS alternativo del cliente** | 8.8.8.8 |

## Resumen de objetivos cumplidos

| Punto | Objetivo | Estado |
|---|---|---|
| A | Instalación de Windows Server 2022 en VMware | ![Completado](https://img.shields.io/badge/-Completado-2EA043?style=flat-square) |
| B | Instalación de AD DS y creación del dominio | ![Completado](https://img.shields.io/badge/-Completado-2EA043?style=flat-square) |
| C | Unión del cliente Windows 10 al dominio | ![Completado](https://img.shields.io/badge/-Completado-2EA043?style=flat-square) |

## Punto A - Instalación de Windows Server 2022 en VMware

![Punto A](https://img.shields.io/badge/Punto-A-0078D4?style=flat-square)
![Estado](https://img.shields.io/badge/Estado-Completado-2EA043?style=flat-square)

### A.1 Creación de la máquina virtual

| Parámetro | Configuración aplicada |
|---|---|
| Modo del asistente | Typical (recommended) |
| Origen de instalación | Installer disc image file (ISO) |
| Imagen utilizada | `SERVER_EVAL_x64FRE_es-es.iso` |
| Detección de VMware | Windows Server 2022 con Easy Install activado |
| Clave de producto | No ingresada (se aceptó el aviso con Yes) |
| Nombre de la VM | MiguelRM |
| Tamaño de disco | 60 GB |
| Tipo de disco | Split virtual disk into multiple files |
| Ajuste adicional | Dispositivo Floppy eliminado en Edit virtual machine settings para evitar errores de Easy Install |

### A.2 Instalación del sistema operativo

| Parámetro | Configuración aplicada |
|---|---|
| Idioma de instalación | Español |
| Edición | Windows Server 2022 Standard Evaluation - Experiencia de escritorio |
| Tipo de instalación | Personalizada: instalar solo el sistema operativo |
| Partición | 60 GB sin asignar |
| Reinicio | Automático al finalizar la instalación |
| Acceso inicial | Contraseña de Administrador establecida; inicio de sesión con Ctrl derecho + Alt derecho + Insert |

### A.3 Cambio de nombre del equipo

| Elemento | Detalle |
|---|---|
| Ubicación | Administrador del servidor, Servidor local, nombre del equipo, Cambiar |
| Nombre asignado | MiguelRM |
| Acción posterior | Reinicio del servidor |

## Punto B - Activación de Active Directory y configuración del dominio

![Punto B](https://img.shields.io/badge/Punto-B-5C2D91?style=flat-square)
![Estado](https://img.shields.io/badge/Estado-Completado-2EA043?style=flat-square)

### B.1 Instalación del rol Servicios de dominio de Active Directory

| Parámetro | Configuración aplicada |
|---|---|
| Herramienta | Administrador del servidor, Agregar roles y características |
| Tipo de instalación | Instalación basada en características o en roles |
| Servidor de destino | Servidor local |
| Rol seleccionado | Servicios de dominio de Active Directory |
| Dependencias | Características adicionales agregadas |
| Reinicio | Reiniciar automáticamente el servidor de destino en caso necesario |

### B.2 Promoción a controlador de dominio

| Parámetro | Configuración aplicada |
|---|---|
| Operación de implementación | Agregar un nuevo bosque |
| Nombre de dominio raíz | `MiguelRM.local` |
| Nivel funcional del bosque | Windows Server 2016 |
| Nivel funcional del dominio | Windows Server 2016 |
| Contraseña DSRM | Establecida |
| Delegación DNS | No configurada |
| Nombre NetBIOS | MIGUELRM (asignado automáticamente) |
| Comprobación de requisitos previos | Superada |
| Reinicio | Automático tras la instalación |

### B.3 Verificación del dominio

| Verificación | Resultado |
|---|---|
| Ubicación | Administrador del servidor, Servidor local |
| Dominio mostrado | `MiguelRM.local` |

## Punto C - Unión de cliente Windows 10 al dominio

![Punto C](https://img.shields.io/badge/Punto-C-00A4EF?style=flat-square)
![Estado](https://img.shields.io/badge/Estado-Completado-2EA043?style=flat-square)

### C.1 Configuración de red del cliente

| Parámetro | Configuración aplicada |
|---|---|
| Ruta | Panel de Control, Redes e Internet, Centro de redes y recursos compartidos, Cambiar configuración del adaptador |
| Adaptador | Ethernet0 |
| Protocolo | Protocolo de Internet versión 4 (TCP/IPv4) |
| DNS preferido | IP del servidor (ej.: 192.168.0.10, verificada con `ipconfig`) |
| DNS alternativo | 8.8.8.8 |

### C.2 Unión al dominio

| Parámetro | Configuración aplicada |
|---|---|
| Ruta | Este equipo, Propiedades, Cambiar configuración, Cambiar |
| Nombre del equipo | WS0829 |
| Miembro de | Dominio: `MiguelRM.local` |
| Credenciales | Administrador del dominio |
| Resultado | Mensaje de unión correcta al dominio `MiguelRM.local` |
| Acción posterior | Reinicio del equipo cliente |

### C.3 Verificación del inicio de sesión

| Verificación | Resultado |
|---|---|
| Pantalla de inicio de sesión | Opción Otro usuario disponible |
| Dominio de inicio de sesión | MIGUELRM |

## Conclusión

| Aspecto | Resultado |
|---|---|
| Servidor | Windows Server 2022 instalado y operativo en VMware |
| Dominio | `MiguelRM.local` creado mediante la promoción a controlador de dominio |
| Cliente | Windows 10 (WS0829) unido correctamente al dominio |
| Autenticación | Inicio de sesión de dominio MIGUELRM disponible en el cliente |

---

<div align="center">

![Author](https://img.shields.io/badge/Author-Miguel_Ramirez_Meli-0A3D91?style=flat-square)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=flat-square)
![Career](https://img.shields.io/badge/Information_Security-ITLA-1F6FEB?style=flat-square)

</div>
