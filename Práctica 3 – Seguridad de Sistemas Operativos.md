<div align="center">

# Operating Systems Security - Practice 3 / Seguridad de Sistemas Operativos - Práctica 3

![Institute](https://img.shields.io/badge/Institute-ITLA-0A3D91?style=for-the-badge)
![Career](https://img.shields.io/badge/Career-Information_Security-1F6FEB?style=for-the-badge)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=for-the-badge)

![Windows Server](https://img.shields.io/badge/Windows_Server-2022-0078D4?style=flat-square&logo=windows&logoColor=white)
![Windows 10](https://img.shields.io/badge/Client-Windows_10_Pro-00A4EF?style=flat-square&logo=windows10&logoColor=white)
![VMware](https://img.shields.io/badge/VMware-Workstation_Pro_17-607078?style=flat-square&logo=vmware&logoColor=white)
![Credential Guard](https://img.shields.io/badge/GPO-Credential_Guard-F25022?style=flat-square)
![Block USB](https://img.shields.io/badge/GPO-Block_USB-5C2D91?style=flat-square)
![Domain](https://img.shields.io/badge/Domain-MiguelRM.local-5C2D91?style=flat-square)

[English](#english) | [Español](#espanol)

</div>

---

<a id="english"></a>

# English

## General information

| Field | Detail |
|---|---|
| **Course** | Operating Systems Security |
| **Institution** | Instituto Tecnológico de Las Américas (ITLA) |
| **Instructor** | Juan Alexander Ramírez |
| **Author** | Miguel Ramirez Meli |
| **Student ID** | 20251367 |
| **Date** | 2026-02-08 |

## Legend

| Badge | Meaning |
|---|---|
| ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Action performed on Windows Server 2022 (Domain Controller) |
| ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Action performed on Windows 10 Pro (domain client) |
| ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Action performed on VMware Workstation Pro 17 (physical host) |

## Contents

| Part | Topic |
|---|---|
| [A](#part-a) | Credential Guard GPO: enable virtualization-based security |
| [B](#part-b) | Client VM prerequisites for Credential Guard |
| [C](#part-c) | Block USB GPO: block USB storage devices |

---

<a id="part-a"></a>

## Part A - Credential Guard GPO: enable virtualization-based security

![Part A](https://img.shields.io/badge/Part-A-F25022?style=flat-square)

### A.1 Create the Organizational Unit and the GPO

| Step | Where | Action |
|---|---|---|
| 1 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In **Server Manager**, go to **Tools > Group Policy Management**. |
| 2 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In the left tree, click the expand button of the forest (`MiguelRM.local`) to show the domains. |
| 3 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click the domain `MiguelRM.local` > **New Organizational Unit**. |
| 4 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Enter the name `Empresa` and click **OK**. |
| 5 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click the new `Empresa` organizational unit > **Create a GPO in this domain, and Link it here...**. |
| 6 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Name it `GPO Credential Guard` and click **OK**. |
| 7 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click the new GPO > **Edit**. |

### A.2 Configure the Credential Guard policy

| Step | Where | Action |
|---|---|---|
| 8 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In the Group Policy Management Editor, navigate to: `Computer Configuration > Policies > Administrative Templates > System > Device Guard`. |
| 9 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Double-click **Turn On Virtualization Based Security**. |
| 10 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Select **Enabled** and set the options listed in the table below. |
| 11 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Click **Apply > OK**. |

| Option | Value |
|---|---|
| Select Platform Security Level | Secure Boot and DMA Protection |
| Virtualization Based Protection of Code Integrity | **Enabled with UEFI lock** |
| Credential Guard Configuration | **Enabled with UEFI lock** |

### A.3 Move the client computer into the Empresa OU

| Step | Where | Action |
|---|---|---|
| 12 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In **Server Manager**, go to **Tools > Active Directory Users and Computers**. |
| 13 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Expand `MiguelRM.local` and click the **Computers** container. Locate the client computer. |
| 14 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click the client computer > **Move...** > select **Empresa** > **OK** (dragging it also works). |

| Note | Detail |
|---|---|
| Why | The GPO uses Computer Configuration, so it only applies to computer objects inside the OU where it is linked. A user inside `Empresa` is not enough. By default, domain-joined computers stay in `Computers`, where this GPO does not apply. |

### A.4 Apply the GPO

| Step | Where | Action |
|---|---|---|
| 15 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Open **Command Prompt (CMD)** as Administrator on the server and run `gpupdate /force`. Wait for the policy changes to apply. |
| 16 | ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | On the client, open **PowerShell** as Administrator (right-click > **Run as administrator**), run `gpupdate /force` and restart the computer. |

### A.5 Verify the GPO on the client (Windows 10)

Verify that the policy was deployed (registry):

| Step | Where | Action |
|---|---|---|
| 17 | ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Open **PowerShell** as Administrator and run `reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows\DeviceGuard"`. |
| 18 | ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Verify the values listed in the table below. |

| Registry value | Expected | Meaning |
|---|---|---|
| `EnableVirtualizationBasedSecurity` | `0x1` | Virtualization-based security enabled |
| `RequirePlatformSecurityFeatures` | `0x3` | Secure Boot and DMA Protection |
| `HypervisorEnforcedCodeIntegrity` | `0x1` | Code integrity enabled with UEFI lock |
| `LsaCfgFlags` | `0x1` | Credential Guard enabled with UEFI lock |
| `ConfigureSystemGuardLaunch` | `0x1` | Secure Launch enabled |

| Note | Detail |
|---|---|
| Expected outcome | The Credential Guard GPO is deployed on the computers of the `MiguelRM.local` domain. |
| Error `Access denied` (for example in `gpresult /r /scope computer`) | PowerShell is not running as Administrator. |
| Registry key not found or `EnableVirtualizationBasedSecurity` equal to `0x0` | Check that the path is spelled `Policies`, that the client computer is inside `Empresa`, and run `gpupdate /force` and restart. |

---

<a id="part-b"></a>

## Part B - Client VM prerequisites for Credential Guard

![Part B](https://img.shields.io/badge/Part-B-5C2D91?style=flat-square)

This part documents the configuration needed in **VMware Workstation Pro 17** so that the Windows 10 Pro client VM is compatible with Credential Guard. Without these steps, Credential Guard cannot run on the client VM.

| Requirement | Detail |
|---|---|
| Firmware | **UEFI** with Secure Boot enabled |
| TPM | Virtual **TPM 2.0** |
| Virtualization | **Nested virtualization** enabled |

### B.1 Power off the VM and delete snapshots

| Step | Where | Action |
|---|---|---|
| 1 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Fully power off the Windows 10 client VM (do not suspend). |
| 2 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | In VMware, go to the menu **VM > Snapshots > Snapshot Manager**. |
| 3 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Delete all existing snapshots. |

| Note | Detail |
|---|---|
| Mandatory step | VM encryption cannot be enabled while snapshots exist. |

### B.2 Encrypt the VM

| Step | Where | Action |
|---|---|---|
| 4 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | With the VM powered off, open **Virtual Machine Settings > Options > Access Control**. |
| 5 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Click **Encrypt...** and set a strong password. |
| 6 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Confirm the operation. The VM is now encrypted. |

| Note | Detail |
|---|---|
| Why | The VMware virtual TPM requires the VM to be encrypted before it can be added. |

### B.3 Change the firmware to UEFI and enable Secure Boot

| Step | Where | Action |
|---|---|---|
| 7 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | In **Virtual Machine Settings > Options > Advanced**, change the firmware type to **UEFI**. |
| 8 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Check **Enable secure boot**. |
| 9 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Click **OK**. |

### B.4 Add the virtual TPM

| Step | Where | Action |
|---|---|---|
| 10 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | In **Virtual Machine Settings > Hardware**, click **Add...**. |
| 11 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Select **Trusted Platform Module** and click **Finish**. |

| Result | Detail |
|---|---|
| Expected outcome | The TPM module appears in the hardware list of the VM. |

### B.5 Enable nested virtualization

| Step | Where | Action |
|---|---|---|
| 12 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | In **Virtual Machine Settings > Hardware > Processors**, check **Virtualize Intel VT-x/EPT or AMD-V/RVI** and **Virtualize IOMMU (IO memory management unit)**. |
| 13 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Click **OK** and power on the VM. |

### B.6 Apply the GPO again on the client

| Step | Where | Action |
|---|---|---|
| 14 | ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | On the Windows 10 client, open **CMD** as Administrator and run `gpupdate /force`. |
| 15 | ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Restart the client computer. |
| 16 | ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | After the restart, verify again with PowerShell as Administrator: `Get-CimInstance -ClassName win32_DeviceGuard -Namespace root\Microsoft\Windows\DeviceGuard`. |

| Result | Detail |
|---|---|
| Expected outcome | The command returns the Device Guard properties and `AvailableSecurityProperties` shows the values `{5, 6}`. |

---

<a id="part-c"></a>

## Part C - Block USB GPO: block USB storage devices

![Part C](https://img.shields.io/badge/Part-C-00A4EF?style=flat-square)

### C.1 Create the GPO

| Step | Where | Action |
|---|---|---|
| 1 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In **Group Policy Management**, expand the domain and locate the `Empresa` organizational unit. |
| 2 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click `Empresa` > **Create a GPO in this domain, and Link it here...**. |
| 3 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Name it `GPO Block USB` and click **OK**. |
| 4 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click the new GPO > **Edit**. |

| Note | Detail |
|---|---|
| Requirement | The client computer must be inside the `Empresa` OU (see A.3), otherwise this GPO does not apply to it. |

### C.2 Configure the USB block

| Step | Where | Action |
|---|---|---|
| 5 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In the Group Policy Management Editor, navigate to: `Computer Configuration > Policies > Administrative Templates > System > Removable Storage Access`. |
| 6 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Double-click **All Removable Storage classes: Deny all access**. |
| 7 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Select **Enabled** and click **Apply > OK**. |

| Note | Detail |
|---|---|
| Priority | This policy takes precedence over any other individual removable storage setting. Once enabled, no USB device, removable disk or memory card can be read or written on the domain computers. |

### C.3 Apply the GPO

| Step | Where | Action |
|---|---|---|
| 8 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Open **Command Prompt (CMD)** as Administrator and run `gpupdate /force`. Wait for the policy update to complete. |

| Result | Detail |
|---|---|
| Expected outcome | The Block USB GPO is configured and deployed. Computers in the `Empresa` organizational unit of the `MiguelRM.local` domain cannot access removable storage devices. |

---

| Note | Detail |
|---|---|
| Environment | The whole practice was performed in a virtualized environment with VMware Workstation Pro 17, using Windows Server 2022 as the domain controller and Windows 10 Pro as the domain client. The client VM requires UEFI firmware, virtual TPM 2.0, Secure Boot and nested virtualization enabled for Credential Guard to work correctly. |

---

<a id="espanol"></a>

# Español

## Datos generales

| Campo | Detalle |
|---|---|
| **Materia** | Seguridad de Sistemas Operativos |
| **Institución** | Instituto Tecnológico de Las Américas (ITLA) |
| **Instructor** | Juan Alexander Ramírez |
| **Autor** | Miguel Ramirez Meli |
| **Matrícula** | 20251367 |
| **Fecha** | 08-02-2026 |

## Leyenda

| Badge | Significado |
|---|---|
| ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Acción realizada en Windows Server 2022 (Controlador de Dominio) |
| ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Acción realizada en Windows 10 Pro (cliente del dominio) |
| ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Acción realizada en VMware Workstation Pro 17 (host físico) |

## Contenido

| Punto | Tema |
|---|---|
| [A](#punto-a) | GPO Credential Guard: habilitar seguridad basada en virtualización |
| [B](#punto-b) | Prerrequisitos de la VM cliente para Credential Guard |
| [C](#punto-c) | GPO Block USB: bloquear dispositivos de almacenamiento USB |

---

<a id="punto-a"></a>

## Punto A - GPO Credential Guard: habilitar seguridad basada en virtualización

![Punto A](https://img.shields.io/badge/Punto-A-F25022?style=flat-square)

### A.1 Crear la Unidad Organizativa y la GPO

| Paso | Dónde | Acción |
|---|---|---|
| 1 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el **Administrador del servidor**, ir a **Herramientas > Administración de directivas de grupo**. |
| 2 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el árbol de la izquierda, hacer clic en el botón de expansión del bosque (`MiguelRM.local`) para desplegar los dominios. |
| 3 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre el dominio `MiguelRM.local` > **Nueva unidad organizativa**. |
| 4 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Ingresar el nombre `Empresa` y hacer clic en **Aceptar**. |
| 5 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre la unidad organizativa `Empresa` recién creada > **Crear un GPO en este dominio y vincularlo aquí...**. |
| 6 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Asignar el nombre `GPO Credential Guard` y hacer clic en **Aceptar**. |
| 7 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre la GPO recién creada > **Editar**. |

### A.2 Configurar la directiva de Credential Guard

| Paso | Dónde | Acción |
|---|---|---|
| 8 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el Editor de administración de directivas de grupo, navegar hasta: `Configuración del equipo > Directivas > Plantillas administrativas > Sistema > Device Guard`. |
| 9 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Doble clic en **Activar la seguridad basada en la virtualización**. |
| 10 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Seleccionar **Habilitada** y configurar las opciones de la tabla siguiente. |
| 11 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Hacer clic en **Aplicar > Aceptar**. |

| Opción | Valor |
|---|---|
| Nivel de seguridad de la plataforma | Arranque seguro y protección de DMA |
| Protección basada en virtualización de la integridad de código | **Habilitado con el bloqueo UEFI** |
| Configuración de Credential Guard | **Habilitado con el bloqueo UEFI** |

### A.3 Mover el equipo cliente a la unidad organizativa Empresa

| Paso | Dónde | Acción |
|---|---|---|
| 12 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el **Administrador del servidor**, ir a **Herramientas > Usuarios y equipos de Active Directory**. |
| 13 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Expandir `MiguelRM.local` y hacer clic en el contenedor **Computers**. Localizar el equipo cliente. |
| 14 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre el equipo cliente > **Mover...** > seleccionar **Empresa** > **Aceptar** (también funciona arrastrarlo). |

| Nota | Detalle |
|---|---|
| Motivo | La GPO usa Configuración del equipo, por lo que solo se aplica a los objetos de equipo que estén dentro de la unidad organizativa donde está vinculada. Un usuario dentro de `Empresa` no es suficiente. Por defecto, los equipos unidos al dominio quedan en `Computers`, donde esta GPO no se aplica. |

### A.4 Aplicar la GPO

| Paso | Dónde | Acción |
|---|---|---|
| 15 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Abrir el **Símbolo del sistema (CMD)** como Administrador en el servidor y ejecutar `gpupdate /force`. Esperar a que se apliquen los cambios de directiva. |
| 16 | ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | En el cliente, abrir **PowerShell** como Administrador (clic derecho > **Ejecutar como administrador**), ejecutar `gpupdate /force` y reiniciar el equipo. |

### A.5 Verificar la GPO en el cliente (Windows 10)

Verificar que la política fue desplegada (registro):

| Paso | Dónde | Acción |
|---|---|---|
| 17 | ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Abrir **PowerShell** como Administrador y ejecutar `reg query "HKLM\SOFTWARE\Policies\Microsoft\Windows\DeviceGuard"`. |
| 18 | ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Verificar los valores de la tabla siguiente. |

| Valor del registro | Esperado | Significado |
|---|---|---|
| `EnableVirtualizationBasedSecurity` | `0x1` | Seguridad basada en virtualización habilitada |
| `RequirePlatformSecurityFeatures` | `0x3` | Arranque seguro y protección de DMA |
| `HypervisorEnforcedCodeIntegrity` | `0x1` | Integridad de código habilitada con bloqueo UEFI |
| `LsaCfgFlags` | `0x1` | Credential Guard habilitado con bloqueo UEFI |
| `ConfigureSystemGuardLaunch` | `0x1` | Secure Launch habilitado |

| Nota | Detalle |
|---|---|
| Resultado esperado | La GPO de Credential Guard queda desplegada en los equipos del dominio `MiguelRM.local`. |
| Error `Acceso denegado` (por ejemplo en `gpresult /r /scope computer`) | PowerShell no está abierto como Administrador. |
| Clave de registro no encontrada o `EnableVirtualizationBasedSecurity` en `0x0` | Verificar que la ruta esté escrita `Policies`, que el equipo cliente esté dentro de `Empresa`, y ejecutar `gpupdate /force` y reiniciar. |

---

<a id="punto-b"></a>

## Punto B - Prerrequisitos de la VM cliente para Credential Guard

![Punto B](https://img.shields.io/badge/Punto-B-5C2D91?style=flat-square)

Este punto documenta la configuración necesaria en **VMware Workstation Pro 17** para que la VM cliente con Windows 10 Pro sea compatible con Credential Guard. Sin estos pasos, Credential Guard no puede ejecutarse en la VM cliente.

| Requisito | Detalle |
|---|---|
| Firmware | **UEFI** con Secure Boot habilitado |
| TPM | **TPM 2.0** virtual |
| Virtualización | **Virtualización anidada** (Nested Virtualization) habilitada |

### B.1 Apagar la VM y eliminar snapshots

| Paso | Dónde | Acción |
|---|---|---|
| 1 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Apagar completamente la VM cliente de Windows 10 (no suspender). |
| 2 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | En VMware, ir al menú **VM > Snapshots > Snapshot Manager**. |
| 3 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Eliminar todos los snapshots existentes. |

| Nota | Detalle |
|---|---|
| Paso obligatorio | La encriptación de la VM no puede activarse mientras existan snapshots. |

### B.2 Encriptar la VM

| Paso | Dónde | Acción |
|---|---|---|
| 4 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Con la VM apagada, abrir **Virtual Machine Settings > Options > Access Control**. |
| 5 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Hacer clic en **Encrypt...** y asignar una contraseña segura. |
| 6 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Confirmar la operación. La VM queda cifrada. |

| Nota | Detalle |
|---|---|
| Motivo | El TPM virtual de VMware requiere que la VM esté encriptada para poder ser agregado. |

### B.3 Cambiar el firmware a UEFI y habilitar Secure Boot

| Paso | Dónde | Acción |
|---|---|---|
| 7 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | En **Virtual Machine Settings > Options > Advanced**, cambiar el tipo de firmware a **UEFI**. |
| 8 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Marcar la opción **Enable secure boot**. |
| 9 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Hacer clic en **Aceptar**. |

### B.4 Agregar el TPM virtual

| Paso | Dónde | Acción |
|---|---|---|
| 10 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | En **Virtual Machine Settings > Hardware**, hacer clic en **Add...**. |
| 11 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Seleccionar **Trusted Platform Module** y hacer clic en **Finish**. |

| Resultado | Detalle |
|---|---|
| Resultado esperado | El módulo TPM aparece listado en el hardware de la VM. |

### B.5 Habilitar virtualización anidada

| Paso | Dónde | Acción |
|---|---|---|
| 12 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | En **Virtual Machine Settings > Hardware > Processors**, marcar **Virtualize Intel VT-x/EPT or AMD-V/RVI** y **Virtualize IOMMU (IO memory management unit)**. |
| 13 | ![HOST](https://img.shields.io/badge/-HOST-607078?style=flat-square) | Hacer clic en **Aceptar** y encender la VM. |

### B.6 Volver a aplicar la GPO en el cliente

| Paso | Dónde | Acción |
|---|---|---|
| 14 | ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | En el cliente Windows 10, abrir **CMD** como Administrador y ejecutar `gpupdate /force`. |
| 15 | ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Reiniciar el equipo cliente. |
| 16 | ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Luego del reinicio, verificar nuevamente con PowerShell como Administrador: `Get-CimInstance -ClassName win32_DeviceGuard -Namespace root\Microsoft\Windows\DeviceGuard`. |

| Resultado | Detalle |
|---|---|
| Resultado esperado | El comando retorna las propiedades de Device Guard y `AvailableSecurityProperties` muestra los valores `{5, 6}`. |

---

<a id="punto-c"></a>

## Punto C - GPO Block USB: bloquear dispositivos de almacenamiento USB

![Punto C](https://img.shields.io/badge/Punto-C-00A4EF?style=flat-square)

### C.1 Crear la GPO

| Paso | Dónde | Acción |
|---|---|---|
| 1 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En **Administración de directivas de grupo**, expandir el dominio y localizar la unidad organizativa `Empresa`. |
| 2 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre `Empresa` > **Crear un GPO en este dominio y vincularlo aquí...**. |
| 3 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Asignar el nombre `GPO Block USB` y hacer clic en **Aceptar**. |
| 4 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre la GPO recién creada > **Editar**. |

| Nota | Detalle |
|---|---|
| Requisito | El equipo cliente debe estar dentro de la unidad organizativa `Empresa` (ver A.3), de lo contrario esta GPO no se le aplica. |

### C.2 Configurar el bloqueo de USB

| Paso | Dónde | Acción |
|---|---|---|
| 5 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el Editor de administración de directivas de grupo, navegar hasta: `Configuración del equipo > Directivas > Plantillas administrativas > Sistema > Acceso de almacenamiento extraíble`. |
| 6 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Doble clic en **Todas las clases de almacenamiento extraíble: denegar acceso a todo**. |
| 7 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Seleccionar **Habilitada** y hacer clic en **Aplicar > Aceptar**. |

| Nota | Detalle |
|---|---|
| Prioridad | Esta directiva tiene prioridad sobre cualquier otra configuración individual de almacenamiento extraíble. Una vez habilitada, ningún dispositivo USB, disco extraíble ni tarjeta de memoria podrá ser leído ni escrito en los equipos del dominio. |

### C.3 Aplicar la GPO

| Paso | Dónde | Acción |
|---|---|---|
| 8 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Abrir el **Símbolo del sistema (CMD)** como Administrador y ejecutar `gpupdate /force`. Esperar a que se complete la actualización de directivas. |

| Resultado | Detalle |
|---|---|
| Resultado esperado | La GPO de bloqueo USB queda configurada y desplegada. Los equipos de la unidad organizativa `Empresa` dentro del dominio `MiguelRM.local` no podrán acceder a dispositivos de almacenamiento extraíble. |

---

| Nota | Detalle |
|---|---|
| Entorno | Toda la práctica fue realizada en un entorno virtualizado con VMware Workstation Pro 17, utilizando Windows Server 2022 como controlador de dominio y Windows 10 Pro como cliente del dominio. La VM cliente requiere firmware UEFI, TPM 2.0 virtual, Secure Boot y virtualización anidada habilitados para que Credential Guard funcione correctamente. |

---

<div align="center">

![Author](https://img.shields.io/badge/Author-Miguel_Ramirez_Meli-0A3D91?style=flat-square)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=flat-square)
![Career](https://img.shields.io/badge/Information_Security-ITLA-1F6FEB?style=flat-square)

</div>
