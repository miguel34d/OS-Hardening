<div align="center">

# Seguridad de Sistemas Operativos - Práctica 3

![Institute](https://img.shields.io/badge/Institute-ITLA-0A3D91?style=for-the-badge)
![Career](https://img.shields.io/badge/Career-Information_Security-1F6FEB?style=for-the-badge)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=for-the-badge)

![Windows Server](https://img.shields.io/badge/Windows_Server-2022-0078D4?style=flat-square&logo=windows&logoColor=white)
![Windows 10](https://img.shields.io/badge/Client-Windows_10_Pro-00A4EF?style=flat-square&logo=windows10&logoColor=white)
![VMware](https://img.shields.io/badge/VMware-Workstation_Pro_17-607078?style=flat-square&logo=vmware&logoColor=white)
![Credential Guard](https://img.shields.io/badge/GPO-Credential_Guard-F25022?style=flat-square)
![Block USB](https://img.shields.io/badge/GPO-Block_USB-5C2D91?style=flat-square)
![Domain](https://img.shields.io/badge/Domain-miguelrm.local-5C2D91?style=flat-square)

</div>

---

## Evidencia de implementación

| Campo | Detalle |
|---|---|
| **Estudiante** | Miguel Ramirez Meli |
| **Matrícula** | 20251367 |
| **Carrera** | Seguridad Informática |
| **Institución** | Instituto Tecnológico de Las Américas (ITLA) |
| **Materia** | Seguridad de Sistemas Operativos |
| **Instructor** | Juan Alexander Ramírez |
| **Fecha** | 08-02-2026 |

## Índice de evidencias

| N.º | Evidencia | Punto | Estado |
|---|---|---|---|
| 1 | Directiva de seguridad basada en virtualización habilitada | A | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 2 | GPO Credential Guard y GPO Block USB vinculadas a la unidad organizativa Empresa | A, C | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 3 | Valores de Device Guard aplicados en el registro del cliente | A | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 4 | Directiva de almacenamiento extraíble habilitada | C | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 5 | Dispositivo USB bloqueado en el cliente | C | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |

---

## Punto A - GPO Credential Guard

![Part A](https://img.shields.io/badge/Part-A-F25022?style=flat-square)

### Evidencia 1 - Seguridad basada en virtualización

La directiva "Turn On Virtualization Based Security" está en Enabled, con nivel de plataforma Secure Boot and DMA Protection, protección de integridad de código y Credential Guard en "Enabled with UEFI lock", y Secure Launch en Enabled.

![Directiva de seguridad basada en virtualización](images/A1_directiva_vbs.png)

### Evidencia 2 - GPO vinculadas a la unidad organizativa Empresa

Administración de directivas de grupo muestra GPO Credential Guard y GPO Block USB vinculadas a la unidad organizativa `Empresa` del dominio `miguelrm.local`, con el enlace habilitado y el estado de la GPO en Enabled.

![GPO vinculadas a Empresa](images/AC2_gpos_vinculadas_empresa.png)

### Evidencia 3 - Despliegue de la política en el cliente

La consulta `reg query` sobre la clave `HKLM\SOFTWARE\Policies\Microsoft\Windows\DeviceGuard` del cliente muestra `EnableVirtualizationBasedSecurity` en 0x1, `RequirePlatformSecurityFeatures` en 0x3, `HypervisorEnforcedCodeIntegrity` en 0x1, `LsaCfgFlags` en 0x1 y `ConfigureSystemGuardLaunch` en 0x1.

![Valores de Device Guard en el registro](images/A3_registro_deviceguard.png)

---

## Punto C - GPO Block USB

![Part C](https://img.shields.io/badge/Part-C-00A4EF?style=flat-square)

### Evidencia 4 - Directiva de almacenamiento extraíble

La directiva "All Removable Storage classes: Deny all access" está en Enabled dentro de Removable Storage Access.

![Directiva de almacenamiento extraíble](images/C1_politica_almacenamiento_extraible.png)

### Evidencia 5 - USB bloqueado en el cliente

El cliente muestra la notificación "Acción de directiva de grupo: Acceso denegado a dispositivo USB", que indica que el dispositivo de almacenamiento fue detectado pero su uso está restringido por la directiva de seguridad de la organización.

![USB bloqueado en el cliente](images/C2_usb_bloqueado.png)

---

<div align="center">

![Author](https://img.shields.io/badge/Author-Miguel_Ramirez_Meli-0A3D91?style=flat-square)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=flat-square)
![Career](https://img.shields.io/badge/Information_Security-ITLA-1F6FEB?style=flat-square)

</div>
