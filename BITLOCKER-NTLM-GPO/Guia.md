<div align="center">

# Operating Systems Security - Practice 2 / Seguridad de Sistemas Operativos - Práctica 2

![Institute](https://img.shields.io/badge/Institute-ITLA-0A3D91?style=for-the-badge)
![Career](https://img.shields.io/badge/Career-Information_Security-1F6FEB?style=for-the-badge)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=for-the-badge)

![Windows Server](https://img.shields.io/badge/Windows_Server-2022-0078D4?style=flat-square&logo=windows&logoColor=white)
![Windows 10](https://img.shields.io/badge/Client-Windows_10_Pro-00A4EF?style=flat-square&logo=windows10&logoColor=white)
![VMware](https://img.shields.io/badge/VMware-Workstation_Pro_17-607078?style=flat-square&logo=vmware&logoColor=white)
![BitLocker](https://img.shields.io/badge/GPO-BitLocker-F25022?style=flat-square)
![NTLM](https://img.shields.io/badge/GPO-NTLM-5C2D91?style=flat-square)
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
| **Date** | 2026-02-01 |

## Legend

| Badge | Meaning |
|---|---|
| ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Action performed on Windows Server 2022 (Domain Controller) |
| ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Action performed on Windows 10 Pro (domain-joined computer) |

## Contents

| Part | Topic |
|---|---|
| [A](#part-a) | BitLocker GPO: disk encryption with PIN and recovery backup in Active Directory |
| [B](#part-b) | NTLM GPO: block NTLM authentication in the domain |

---

<a id="part-a"></a>

## Part A - BitLocker GPO: disk encryption with PIN and recovery backup in Active Directory

![Part A](https://img.shields.io/badge/Part-A-F25022?style=flat-square)

### A.1 Install the BitLocker feature

| Step | Where | Action |
|---|---|---|
| 1 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In **Server Manager**, click **Manage > Add Roles and Features**. |
| 2 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Click **Next** until the **Features** page. Check **BitLocker Drive Encryption**. |
| 3 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | When the dependencies dialog appears, click **Add Features**. |
| 4 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Click **Next** until the Confirmation page. |
| 5 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Check **Restart the destination server automatically if required**, confirm with **Yes** and click **Install**. |
| 6 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Wait for the installation to finish. |

### A.2 Create and configure the BitLocker GPO

| Step | Where | Action |
|---|---|---|
| 7 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Go to **Tools > Group Policy Management**. |
| 8 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click the domain (`MiguelRM.local`) and select **Create a GPO in this domain, and Link it here**. |
| 9 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Name it `GPO BitLocker` and click **OK**. |
| 10 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Expand the domain, right-click the new GPO and select **Edit**. |
| 11 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Navigate to: `Computer Configuration > Policies > Administrative Templates > Windows Components > BitLocker Drive Encryption > Operating System Drives`. |
| 12 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Double-click **Require additional authentication at startup**, select **Enabled**, then **Apply > OK**. |
| 13 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Double-click **Allow enhanced PINs for startup**, select **Enabled**, then **Apply > OK**. |
| 14 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Double-click **Configure minimum PIN length for startup**, select **Enabled**, set the minimum to **4** characters, then **Apply > OK**. |
| 15 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Double-click **Configure TPM platform validation profile**, select **Enabled**, then **Apply > OK**. |
| 16 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Go one level up to the root of **BitLocker Drive Encryption** and enable **Store BitLocker recovery information in Active Directory Domain Services**, then **Apply > OK**. |

### A.3 Apply the GPO and encrypt the drive

| Step | Where | Action |
|---|---|---|
| 17 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Open **Command Prompt (CMD)** as Administrator and run `gpupdate /force`. Wait for the changes to apply. |
| 18 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | In **File Explorer**, create a folder on the desktop named `Key recovery` to store the recovery key. |
| 19 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Open **Disk Management** (right-click Start > **Disk Management**, or `Windows + R` > `diskmgmt.msc`, or **This PC > Manage > Disk Management**). Right-click the **C:** drive bar > **Shrink Volume**, set the size of the new partition and click **Shrink**. |
| 20 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | On the unallocated space, right-click > **New Simple Volume** and follow the wizard. |
| 21 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Right-click the new partition in File Explorer > **Turn on BitLocker**. |
| 22 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Select **Use a password to unlock the drive**, enter the password and click **Next**. |
| 23 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Choose **Save to a file**, browse to the `Key recovery` folder, then **Save > Next**. |
| 24 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Select **Encrypt entire drive** > **Next**. |
| 25 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Select **Compatible mode** (recommended for removable drives) > **Next**. |
| 26 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Click **Start encrypting** and wait for it to finish. |

| Result | Detail |
|---|---|
| Expected outcome | The drive is encrypted and a padlock icon appears on it in File Explorer. |

---

<a id="part-b"></a>

## Part B - NTLM GPO: block NTLM authentication in the domain

![Part B](https://img.shields.io/badge/Part-B-5C2D91?style=flat-square)

### B.1 Create the NTLM GPO

| Step | Where | Action |
|---|---|---|
| 1 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In **Server Manager**, click **Tools > Group Policy Management**. |
| 2 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click the domain (`MiguelRM.local`) > **Create a GPO in this domain, and Link it here**. |
| 3 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Name it `GPO NTLM` and click **OK**. |
| 4 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click the new GPO > **Edit**. |

### B.2 Configure the NTLM policies

| Step | Where | Action |
|---|---|---|
| 5 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Navigate to: `Computer Configuration > Policies > Windows Settings > Security Settings > Local Policies > Security Options`. |
| 6 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Find **Network security: Restrict NTLM: Audit NTLM authentication in this domain**. |
| 7 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Double-click it, check **Define this policy setting**, set the value to **Enable all**, then **Apply > OK**. |
| 8 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Find **Network security: Restrict NTLM: NTLM authentication in this domain**. |
| 9 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Double-click it, check **Define this policy setting**, set the value to **Deny all**, then **Apply > Yes > OK**. |

| Note | Detail |
|---|---|
| Effect of **Deny all** | Blocks every NTLM authentication attempt in the domain, preventing unauthorized access through network traffic. |

### B.3 Apply and verify the GPO

| Step | Where | Action |
|---|---|---|
| 10 | ![Client](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Open **Command Prompt (CMD)** as Administrator and run `gpupdate /force`. Wait for the update to complete. |
| 11 | ![Server](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In **Group Policy Management**, right-click **GPO NTLM** > **Update** to confirm that the policy is enabled and deployed. |

| Result | Detail |
|---|---|
| Expected outcome | The NTLM GPO is configured and deployed to all computers in the `MiguelRM.local` domain. |

---

| Note | Detail |
|---|---|
| Environment | The whole practice was performed in a virtualized environment with VMware Workstation Pro 17, using Windows Server 2022 as the domain controller and Windows 10 Pro as the domain client. |

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
| **Fecha** | 01-02-2026 |

## Leyenda

| Badge | Significado |
|---|---|
| ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Acción realizada en Windows Server 2022 (Controlador de Dominio) |
| ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Acción realizada en Windows 10 Pro (equipo unido al dominio) |

## Contenido

| Punto | Tema |
|---|---|
| [A](#punto-a) | GPO BitLocker: cifrado de disco con PIN y respaldo en Active Directory |
| [B](#punto-b) | GPO NTLM: bloquear autenticación NTLM en el dominio |

---

<a id="punto-a"></a>

## Punto A - GPO BitLocker: cifrado de disco con PIN y respaldo en Active Directory

![Punto A](https://img.shields.io/badge/Punto-A-F25022?style=flat-square)

### A.1 Instalar la característica BitLocker

| Paso | Dónde | Acción |
|---|---|---|
| 1 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el **Administrador del servidor**, hacer clic en **Administrar > Agregar roles y características**. |
| 2 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Hacer clic en **Siguiente** hasta la pantalla **Características**. Marcar **Cifrado de unidad BitLocker**. |
| 3 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Cuando aparezca el cuadro de dependencias, hacer clic en **Agregar características**. |
| 4 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Hacer clic en **Siguiente** hasta la pantalla de Confirmación. |
| 5 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Marcar **Reiniciar automáticamente el servidor de destino en caso necesario**, confirmar con **Sí** y hacer clic en **Instalar**. |
| 6 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Esperar a que finalice la instalación. |

### A.2 Crear y configurar la GPO de BitLocker

| Paso | Dónde | Acción |
|---|---|---|
| 7 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Ir a **Herramientas > Administración de directivas de grupo**. |
| 8 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre el dominio (`MiguelRM.local`) y seleccionar **Crear un GPO en este dominio y vincularlo aquí**. |
| 9 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Asignar el nombre `GPO BitLocker` y hacer clic en **Aceptar**. |
| 10 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Expandir el dominio, clic derecho sobre la GPO recién creada y seleccionar **Editar**. |
| 11 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Navegar hasta: `Configuración del equipo > Directivas > Plantillas administrativas > Componentes de Windows > Cifrado de unidad BitLocker > Unidades del sistema operativo`. |
| 12 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Doble clic en **Requerir autenticación adicional al iniciar**, seleccionar **Habilitada** y **Aplicar > Aceptar**. |
| 13 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Doble clic en **Permitir los PIN mejorados para el inicio**, seleccionar **Habilitada** y **Aplicar > Aceptar**. |
| 14 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Doble clic en **Configurar longitud mínima de PIN para el inicio**, seleccionar **Habilitada**, establecer un mínimo de **4** caracteres y **Aplicar > Aceptar**. |
| 15 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Doble clic en **Configurar el perfil de validación de plataforma del TPM**, seleccionar **Habilitada** y **Aplicar > Aceptar**. |
| 16 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Volver un nivel arriba, a la raíz de **Cifrado de unidad BitLocker**, y habilitar **Almacenar información de recuperación de BitLocker en los Servicios de dominio de Active Directory**, luego **Aplicar > Aceptar**. |

### A.3 Aplicar la GPO y cifrar el disco

| Paso | Dónde | Acción |
|---|---|---|
| 17 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Abrir el **Símbolo del sistema (CMD)** como Administrador y ejecutar `gpupdate /force`. Esperar a que se apliquen los cambios. |
| 18 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | En el **Explorador de archivos**, crear en el escritorio una carpeta llamada `Key recovery` para guardar la clave de recuperación. |
| 19 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Abrir **Administración de discos** (clic derecho en Inicio > **Disk Management**, o `Windows + R` > `diskmgmt.msc`, o **Este equipo > Administrar > Administración de discos**). Clic derecho en la barra del disco **C:** > **Shrink Volume**, definir el tamaño de la nueva partición y hacer clic en **Shrink**. |
| 20 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | En el espacio no asignado, clic derecho > **Nuevo volumen simple** y seguir el asistente. |
| 21 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Clic derecho sobre la nueva partición en el Explorador de archivos > **Activar BitLocker**. |
| 22 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Seleccionar **Usar una contraseña para desbloquear la unidad**, ingresar la contraseña y hacer clic en **Siguiente**. |
| 23 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Elegir **Guardar en un archivo**, navegar hasta la carpeta `Key recovery`, luego **Guardar > Siguiente**. |
| 24 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Seleccionar **Cifrar la unidad entera** > **Siguiente**. |
| 25 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Seleccionar **Modo Compatible** (recomendado para unidades extraíbles) > **Siguiente**. |
| 26 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Hacer clic en **Iniciar cifrado** y esperar a que finalice. |

| Resultado | Detalle |
|---|---|
| Resultado esperado | La unidad queda cifrada y el ícono del candado aparece sobre ella en el Explorador de archivos. |

---

<a id="punto-b"></a>

## Punto B - GPO NTLM: bloquear autenticación NTLM en el dominio

![Punto B](https://img.shields.io/badge/Punto-B-5C2D91?style=flat-square)

### B.1 Crear la GPO NTLM

| Paso | Dónde | Acción |
|---|---|---|
| 1 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el **Administrador del servidor**, hacer clic en **Herramientas > Administración de directivas de grupo**. |
| 2 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre el dominio (`MiguelRM.local`) > **Crear un GPO en este dominio y vincularlo aquí**. |
| 3 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Asignar el nombre `GPO NTLM` y hacer clic en **Aceptar**. |
| 4 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre la GPO recién creada > **Editar**. |

### B.2 Configurar las directivas NTLM

| Paso | Dónde | Acción |
|---|---|---|
| 5 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Navegar hasta: `Configuración del equipo > Directivas > Configuración de Windows > Configuración de seguridad > Directivas locales > Opciones de seguridad`. |
| 6 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Buscar **Seguridad de red: restringir NTLM: auditar la autenticación NTLM en este dominio**. |
| 7 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Doble clic, marcar **Definir esta configuración de directiva**, cambiar el valor a **Habilitar todo** y **Aplicar > Aceptar**. |
| 8 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Buscar **Seguridad de red: restringir NTLM: autenticación NTLM en este dominio**. |
| 9 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Doble clic, marcar **Definir esta configuración de directiva**, cambiar el valor a **Denegar todo** y **Aplicar > Sí > Aceptar**. |

| Nota | Detalle |
|---|---|
| Efecto de **Denegar todo** | Bloquea todos los intentos de autenticación NTLM en el dominio, previniendo accesos no autorizados a través del tráfico de red. |

### B.3 Aplicar y verificar la GPO

| Paso | Dónde | Acción |
|---|---|---|
| 10 | ![Cliente](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Abrir el **Símbolo del sistema (CMD)** como Administrador y ejecutar `gpupdate /force`. Esperar a que se complete la actualización. |
| 11 | ![Servidor](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En **Administración de directivas de grupo**, clic derecho sobre **GPO NTLM** > **Actualizar** para confirmar que la directiva está habilitada y desplegada. |

| Resultado | Detalle |
|---|---|
| Resultado esperado | La GPO NTLM queda configurada y desplegada en todos los equipos del dominio `MiguelRM.local`. |

---

| Nota | Detalle |
|---|---|
| Entorno | Toda la práctica fue realizada en un entorno virtualizado con VMware Workstation Pro 17, utilizando Windows Server 2022 como controlador de dominio y Windows 10 Pro como cliente del dominio. |

---

<div align="center">

![Author](https://img.shields.io/badge/Author-Miguel_Ramirez_Meli-0A3D91?style=flat-square)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=flat-square)
![Career](https://img.shields.io/badge/Information_Security-ITLA-1F6FEB?style=flat-square)

</div>
