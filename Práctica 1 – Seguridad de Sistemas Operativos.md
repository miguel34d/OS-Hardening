<div align="center">

# Práctica 1 - Seguridad de Sistemas Operativos

**Evidencia de implementación: Windows Server 2022 con Active Directory y cliente Windows 10**

![Instituto](https://img.shields.io/badge/Instituto-ITLA-0A3D91?style=for-the-badge)
![Carrera](https://img.shields.io/badge/Carrera-Seguridad_Inform%C3%A1tica-1F6FEB?style=for-the-badge)
![Matricula](https://img.shields.io/badge/Matr%C3%ADcula-20251367-2EA043?style=for-the-badge)

![Windows Server](https://img.shields.io/badge/Windows_Server-2022_Standard_Evaluation-0078D4?style=flat-square&logo=windows&logoColor=white)
![Windows 10](https://img.shields.io/badge/Cliente-Windows_10_Pro-00A4EF?style=flat-square&logo=windows10&logoColor=white)
![VMware](https://img.shields.io/badge/Hipervisor-VMware_Workstation_Pro_17-607078?style=flat-square&logo=vmware&logoColor=white)
![Active Directory](https://img.shields.io/badge/Servicio-Active_Directory_DS-F25022?style=flat-square)
![Dominio](https://img.shields.io/badge/Dominio-MiguelRM.local-5C2D91?style=flat-square)
![Estado](https://img.shields.io/badge/Estado-Completado-2EA043?style=flat-square)

</div>

---

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

---

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

---

## Resumen de objetivos cumplidos

| Punto | Objetivo | Estado |
|---|---|---|
| A | Instalación de Windows Server 2022 en VMware | ![Completado](https://img.shields.io/badge/-Completado-2EA043?style=flat-square) |
| B | Instalación de AD DS y creación del dominio | ![Completado](https://img.shields.io/badge/-Completado-2EA043?style=flat-square) |
| C | Unión del cliente Windows 10 al dominio | ![Completado](https://img.shields.io/badge/-Completado-2EA043?style=flat-square) |

---

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

---

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

---

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

---

## Conclusión

| Aspecto | Resultado |
|---|---|
| Servidor | Windows Server 2022 instalado y operativo en VMware |
| Dominio | `MiguelRM.local` creado mediante la promoción a controlador de dominio |
| Cliente | Windows 10 (WS0829) unido correctamente al dominio |
| Autenticación | Inicio de sesión de dominio MIGUELRM disponible en el cliente |

---

<div align="center">

![Autor](https://img.shields.io/badge/Autor-Miguel_Ramirez_Meli-0A3D91?style=flat-square)
![Matricula](https://img.shields.io/badge/Matr%C3%ADcula-20251367-2EA043?style=flat-square)
![Carrera](https://img.shields.io/badge/Seguridad_Inform%C3%A1tica-ITLA-1F6FEB?style=flat-square)

</div>
