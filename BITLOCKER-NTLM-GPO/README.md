<div align="center">

# Seguridad de Sistemas Operativos - Práctica 2

![Institute](https://img.shields.io/badge/Institute-ITLA-0A3D91?style=for-the-badge)
![Career](https://img.shields.io/badge/Career-Information_Security-1F6FEB?style=for-the-badge)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=for-the-badge)

![Windows Server](https://img.shields.io/badge/Windows_Server-2022-0078D4?style=flat-square&logo=windows&logoColor=white)
![Windows 10](https://img.shields.io/badge/Client-Windows_10-00A4EF?style=flat-square&logo=windows10&logoColor=white)
![VMware](https://img.shields.io/badge/VMware-Workstation_Pro_17-607078?style=flat-square&logo=vmware&logoColor=white)
![BitLocker](https://img.shields.io/badge/GPO-BitLocker-F25022?style=flat-square)
![NTLM](https://img.shields.io/badge/GPO-NTLM-5C2D91?style=flat-square)
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
| **Fecha** | 01-02-2026 |

## Índice de evidencias

| N.º | Evidencia | Punto | Estado |
|---|---|---|---|
| 1 | Característica Cifrado de unidad BitLocker instalada en el servidor | A | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 2 | Directivas de BitLocker para unidades del sistema operativo | A | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 3 | Información de recuperación almacenada en Active Directory | A | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 4 | GPO BitLocker y GPO NTLM vinculadas al dominio | A, B | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 5 | Directiva de grupo actualizada en el cliente | A, B | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 6 | Unidad protegida con BitLocker (bloqueada) | A | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 7 | Estado de BitLocker de la unidad (`manage-bde`) | A | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |
| 8 | Directivas de restricción de NTLM | B | ![Verified](https://img.shields.io/badge/-Verified-2EA043?style=flat-square) |

---

## Punto A - GPO BitLocker

![Part A](https://img.shields.io/badge/Part-A-F25022?style=flat-square)

### Evidencia 1 - Característica BitLocker instalada

Cifrado de unidad BitLocker aparece como instalado en el asistente de roles y características del servidor.

![Característica BitLocker](images/A1_bitlocker_feature.png)

### Evidencia 2 - Directivas de BitLocker para unidades del sistema operativo

En el editor de la GPO, "Require additional authentication at startup", "Allow enhanced PINs for startup", "Configure minimum PIN length for startup" y el perfil de validación de plataforma del TPM están en Enabled.

![Directivas de unidades del sistema operativo](images/A2_politicas_unidades_so.png)

### Evidencia 3 - Información de recuperación en Active Directory

"Store BitLocker recovery information in Active Directory Domain Services" está en Enabled.

![Respaldo de recuperación en Active Directory](images/A3_respaldo_ad.png)

### Evidencia 4 - GPO vinculadas al dominio

Administración de directivas de grupo muestra GPO BitLocker y GPO NTLM creadas y vinculadas al dominio `miguelrm.local`.

![GPO vinculadas al dominio](images/AB4_gpos_vinculadas.png)

### Evidencia 5 - Actualización de directivas en el cliente

`gpupdate /force` se completó correctamente en el equipo cliente.

![gpupdate en el cliente](images/AB5_gpupdate_cliente.png)

### Evidencia 6 - Unidad protegida con BitLocker

La unidad J: aparece con el candado cerrado, lo que indica que está protegida y bloqueada por BitLocker.

![Unidad bloqueada](images/A6_unidad_bloqueada.png)

### Evidencia 7 - Estado de BitLocker

`manage-bde -status J:` reporta BitLocker versión 2.0, cifrado AES 128, estado de bloqueo Bloqueado y los protectores de clave Contraseña y Contraseña numérica.

![Estado con manage-bde](images/A7_manage_bde_status.png)

---

## Punto B - GPO NTLM

![Part B](https://img.shields.io/badge/Part-B-5C2D91?style=flat-square)

### Evidencia 8 - Directivas de restricción de NTLM

"Restrict NTLM: Audit NTLM authentication in this domain" está en Enable all, y "Restrict NTLM: NTLM authentication in this domain" está en Deny all.

![Directivas NTLM](images/B1_politicas_ntlm.png)

---

<div align="center">

![Author](https://img.shields.io/badge/Author-Miguel_Ramirez_Meli-0A3D91?style=flat-square)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=flat-square)
![Career](https://img.shields.io/badge/Information_Security-ITLA-1F6FEB?style=flat-square)

</div>
