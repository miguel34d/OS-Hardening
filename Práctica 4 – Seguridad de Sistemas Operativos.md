<div align="center">

# Operating Systems Security - Practice 4 / Seguridad de Sistemas Operativos - Práctica 4

![Institute](https://img.shields.io/badge/Institute-ITLA-0A3D91?style=for-the-badge)
![Career](https://img.shields.io/badge/Career-Information_Security-1F6FEB?style=for-the-badge)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=for-the-badge)

![Windows Server](https://img.shields.io/badge/Windows_Server-2022-0078D4?style=flat-square&logo=windows&logoColor=white)
![Windows 10](https://img.shields.io/badge/Client-Windows_10_Pro-00A4EF?style=flat-square&logo=windows10&logoColor=white)
![VMware](https://img.shields.io/badge/VMware-Workstation_Pro_17-607078?style=flat-square&logo=vmware&logoColor=white)
![DNSSEC](https://img.shields.io/badge/DNS-DNSSEC-F25022?style=flat-square)
![GPO](https://img.shields.io/badge/GPO-DNSSEC-5C2D91?style=flat-square)
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
| **Date** | 2026-02-22 |

## Legend

| Badge | Meaning |
|---|---|
| ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Action performed on Windows Server 2022 (Domain Controller) |
| ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Action performed on Windows 10 Pro (domain client) |

## Contents

| Part | Topic |
|---|---|
| [A](#part-a) | Enable DNSSEC on a lookup zone of the domain |
| [B](#part-b) | Propagate the DNSSEC configuration to the clients through a GPO |
| [Check](#check) | Verify that DNSSEC was applied correctly |

---

<a id="part-a"></a>

## Part A - Enable DNSSEC on a lookup zone of the domain

![Part A](https://img.shields.io/badge/Part-A-F25022?style=flat-square)

### A.1 Open DNS Manager and sign the zone

| Step | Where | Action |
|---|---|---|
| 1 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In **Server Manager**, go to **Tools > DNS**. |
| 2 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In DNS Manager, expand the server and then **Forward Lookup Zones**. Right-click the zone `MiguelRM.local`. |
| 3 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | If the **DNSSEC** option is grayed out, select **Refresh** in the same menu and try again. |
| 4 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click `MiguelRM.local` > **DNSSEC > Sign the Zone**. |
| 5 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | The **Zone Signing Wizard** opens. Select **Customize zone signing parameters** and click **Next**. |
| 6 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | On the **Key Master** page, select **The DNS server MIGUEL is the Key Master** and click **Next**. |

### A.2 Configure the Key Signing Key (KSK)

| Step | Where | Action |
|---|---|---|
| 7 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | On the KSK information page, click **Next**. |
| 8 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | On the **Key Signing Key (KSK) - Configure one or more KSKs** page, click **Add**. |
| 9 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In the form, set the values listed in the table below. |
| 10 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Change the **Rollover frequency (days)** from `755` to `100`. Click **OK**. |
| 11 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Verify that the KSK appears in the table and click **Next**. |

| Parameter | Default value | Value to use |
|---|---|---|
| Cryptographic algorithm | RSA/SHA-256 | RSA/SHA-256 (no change) |
| Key length (bits) | 2048 | 2048 (no change) |
| Rollover frequency (days) | 755 | **100** |

### A.3 Configure the Zone Signing Key (ZSK)

| Step | Where | Action |
|---|---|---|
| 12 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | On the ZSK information page, click **Next**. |
| 13 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | On the **Zone Signing Key (ZSK) - Configure one or more ZSKs** page, click **Add**. |
| 14 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In the form, set the values listed in the table below. |
| 15 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Change the **Key length** from `1024` to `2048` and the **Rollover frequency** from `90` to `100`. Click **OK**. |
| 16 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Verify that the ZSK appears in the table and click **Next**. |

| Parameter | Default value | Value to use |
|---|---|---|
| Key length (bits) | 1024 | **2048** |
| Rollover frequency (days) | 90 | **100** |

### A.4 Configure NSEC3

| Step | Where | Action |
|---|---|---|
| 17 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | On the **Next Secure (NSEC)** page, select **Use NSEC3** (selected by default) and keep the default values listed in the table below. |
| 18 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Click **Next >**. |

| Parameter | Value |
|---|---|
| Iterations | 50 |
| Generate and use a random salt of length | 8 |

### A.5 Configure Trust Anchors

| Step | Where | Action |
|---|---|---|
| 19 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | On the **Trust Anchors (TAs)** page, check both boxes: **Enable the distribution of trust anchors for this zone** and **Enable automatic update of trust anchors on key rollover (RFC 5011)**. |
| 20 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Click **Next >**. |

### A.6 Signing and Polling Parameters

| Step | Where | Action |
|---|---|---|
| 21 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | On the **Signing and Polling Parameters** page, keep all the default values listed in the table below. |
| 22 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Click **Next >**. |

| Parameter | Value |
|---|---|
| DS record generation algorithm | SHA-1 and SHA-256 |
| DS record TTL (seconds) | 3600 |
| DNSKEY record TTL (seconds) | 3600 |
| Secure delegation polling period (hours) | 12 |
| Signature inception (hours) | 1 |

### A.7 Finish the signing

| Step | Where | Action |
|---|---|---|
| 23 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Review the summary and click **Next >** to start the signing. |
| 24 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Wait for the wizard to complete. When the message **The zone was successfully signed** appears, click **Finish**. |
| 25 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In DNS Manager, right-click `MiguelRM.local` > **Refresh**. The new DNSSEC records (`RRSIG`, `DNSKEY`, `NSEC3`, `DS`) generated by the signing are displayed. |

| Result | Detail |
|---|---|
| Expected outcome | The `MiguelRM.local` zone is signed with DNSSEC. Its status changes from **Not Signed** to **Signed** in the zone list. |

---

<a id="part-b"></a>

## Part B - Propagate the DNSSEC configuration to the clients through a GPO

![Part B](https://img.shields.io/badge/Part-B-5C2D91?style=flat-square)

### B.1 Create the DNSSEC GPO

| Step | Where | Action |
|---|---|---|
| 1 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In **Server Manager**, go to **Tools > Group Policy Management**. |
| 2 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In the tree, expand the domain and locate the `Empresa` organizational unit. |
| 3 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click `Empresa` > **Create a GPO in this domain, and Link it here...**. |
| 4 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Name it `DNSSEC` and click **OK**. |
| 5 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Right-click the new GPO > **Edit**. |

### B.2 Configure the Name Resolution Policy

| Step | Where | Action |
|---|---|---|
| 6 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In the Group Policy Management Editor, navigate to: `Computer Configuration > Policies > Windows Settings > Name Resolution Policy`. |
| 7 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In the right pane, in the **Create Rules** section, set the fields listed in the table below. |
| 8 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In the **Certification authority (Optional)** field, click **Browse**. |
| 9 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | In the certificates box, select the available certificate and click **OK**. |
| 10 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Check **Enable DNSSEC in this rule**. |
| 11 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Under **DNSSEC configuration > Validation**, check **Require DNS clients to check that the name and address data has been validated by the DNS server**. |
| 12 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Under **IPsec**, check **Use IPsec in communication between the DNS client and the DNS server**. |
| 13 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Click **Apply** at the bottom of the pane. |

| Field | Value |
|---|---|
| To which part of the namespace does this rule apply? | **Suffix** |
| Suffix name | `MiguelRM.local` |

### B.3 Apply the GPO on the server

| Step | Where | Action |
|---|---|---|
| 14 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Open **Command Prompt (CMD)** as Administrator and run `gpupdate /force`. Wait for the policy update to complete. |

### B.4 Apply the GPO on the client

| Step | Where | Action |
|---|---|---|
| 15 | ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Open **Command Prompt (CMD)** as Administrator and run `gpupdate /force`. |
| 16 | ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Verify that the DNSSEC policy was applied by running `gpresult /r`. Confirm that `DNSSEC` appears in the list of applied computer policies. |

| Result | Detail |
|---|---|
| Expected outcome | The DNSSEC GPO is configured and deployed. Domain clients inside the `Empresa` OU validate DNS responses through DNSSEC signatures and use IPsec to secure the communication with the DNS server. |
| Requirement | The client computer must be inside the `Empresa` OU in Active Directory for the GPO to be applied correctly. |

---

<a id="check"></a>

## Check - Verify that DNSSEC was applied correctly

![Check](https://img.shields.io/badge/Check-DNSSEC-2EA043?style=flat-square)

### On the server - Verify the DNSSEC records

| Step | Where | Action |
|---|---|---|
| 1 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Open **DNS Manager** > **Forward Lookup Zones** > `MiguelRM.local`. |
| 2 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Verify that the record types listed in the table below exist in the zone. |
| 3 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | Verify that in the zone list the status of `MiguelRM.local` shows **Signed** in the DNSSEC Status column. |
| 4 | ![SERVER](https://img.shields.io/badge/-SERVER-0078D4?style=flat-square) | From **PowerShell** as Administrator, run `Resolve-DnsName -Name MiguelRM.local -Type DNSKEY -Server 127.0.0.1`. It must return the DNSKEY records of the signed zone. |

| Record | Description |
|---|---|
| `RRSIG` | Digital signature of the DNS records |
| `DNSKEY` | KSK and ZSK public keys |
| `NSEC3` | Authenticated proof of non-existence |
| `DS` | Signing delegation to the parent zone |

### On the client - Verify that the DNSSEC GPO was applied

| Step | Where | Action |
|---|---|---|
| 5 | ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Open **CMD** as Administrator and run `gpresult /r`. Confirm that `DNSSEC` appears under **Applied computer policies**. |
| 6 | ![CLIENT](https://img.shields.io/badge/-CLIENT-2EA043?style=flat-square) | Verify DNS resolution with DNSSEC validation by running in PowerShell `Resolve-DnsName -Name MiguelRM.local -Type A -DnssecOk`. If the response includes `RRSIG` records, DNSSEC is working correctly on the client. |

| Result | Detail |
|---|---|
| Expected outcome | DNSSEC is enabled, signed and correctly propagated to the clients of the `MiguelRM.local` domain. |

---

| Note | Detail |
|---|---|
| Environment | The whole practice was performed in a virtualized environment with VMware Workstation Pro 17, using Windows Server 2022 as the domain controller and Windows 10 Pro as the domain client. The client computer must be inside the `Empresa` OU in Active Directory for the GPOs to be applied correctly. |

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
| **Fecha** | 22-02-2026 |

## Leyenda

| Badge | Significado |
|---|---|
| ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Acción realizada en Windows Server 2022 (Controlador de Dominio) |
| ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Acción realizada en Windows 10 Pro (cliente del dominio) |

## Contenido

| Punto | Tema |
|---|---|
| [A](#punto-a) | Habilitar DNSSEC en una zona de búsqueda del dominio |
| [B](#punto-b) | Propagar la configuración de DNSSEC a los clientes mediante GPO |
| [Comprobación](#comprobacion) | Verificar que DNSSEC fue aplicado correctamente |

---

<a id="punto-a"></a>

## Punto A - Habilitar DNSSEC en una zona de búsqueda del dominio

![Punto A](https://img.shields.io/badge/Punto-A-F25022?style=flat-square)

### A.1 Abrir el Administrador de DNS y firmar la zona

| Paso | Dónde | Acción |
|---|---|---|
| 1 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el **Administrador del servidor**, ir a **Herramientas > DNS**. |
| 2 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el Administrador de DNS, expandir el servidor y luego **Forward Lookup Zones**. Clic derecho sobre la zona `MiguelRM.local`. |
| 3 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Si la opción **DNSSEC** aparece en gris, seleccionar **Actualizar** en el mismo menú y volver a intentarlo. |
| 4 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre `MiguelRM.local` > **DNSSEC > Firmar la zona**. |
| 5 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Se despliega el **Asistente para firmar zona**. Seleccionar **Customize zone signing parameters** y hacer clic en **Siguiente**. |
| 6 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En la pantalla **Key Master**, seleccionar **The DNS server MIGUEL is the Key Master** y hacer clic en **Siguiente**. |

### A.2 Configurar la Clave de Firma de Clave (KSK)

| Paso | Dónde | Acción |
|---|---|---|
| 7 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En la pantalla informativa de KSK, hacer clic en **Siguiente**. |
| 8 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En la pantalla **Key Signing Key (KSK) - Configure one or more KSKs**, hacer clic en **Add**. |
| 9 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el formulario, configurar los valores de la tabla siguiente. |
| 10 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Cambiar la **Frecuencia de sustitución (días)** de `755` a `100`. Hacer clic en **Aceptar**. |
| 11 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Verificar que la KSK aparezca en la tabla y hacer clic en **Siguiente**. |

| Parámetro | Valor por defecto | Valor a usar |
|---|---|---|
| Algoritmo criptográfico | RSA/SHA-256 | RSA/SHA-256 (sin cambio) |
| Longitud de clave (bits) | 2048 | 2048 (sin cambio) |
| Frecuencia de sustitución (días) | 755 | **100** |

### A.3 Configurar la Clave de Firma de Zona (ZSK)

| Paso | Dónde | Acción |
|---|---|---|
| 12 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En la pantalla informativa de ZSK, hacer clic en **Siguiente**. |
| 13 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En la pantalla **Zone Signing Key (ZSK) - Configure one or more ZSKs**, hacer clic en **Add**. |
| 14 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el formulario, configurar los valores de la tabla siguiente. |
| 15 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Cambiar la **Longitud de clave** de `1024` a `2048` y la **Frecuencia de sustitución** de `90` a `100`. Hacer clic en **Aceptar**. |
| 16 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Verificar que la ZSK aparezca en la tabla y hacer clic en **Siguiente**. |

| Parámetro | Valor por defecto | Valor a usar |
|---|---|---|
| Longitud de clave (bits) | 1024 | **2048** |
| Frecuencia de sustitución (días) | 90 | **100** |

### A.4 Configurar NSEC3

| Paso | Dónde | Acción |
|---|---|---|
| 17 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En la pantalla **Next Secure (NSEC)**, seleccionar **Use NSEC3** (ya seleccionado por defecto) y dejar los valores por defecto de la tabla siguiente. |
| 18 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Hacer clic en **Next >**. |

| Parámetro | Valor |
|---|---|
| Iterations | 50 |
| Generate and use a random salt of length | 8 |

### A.5 Configurar Trust Anchors

| Paso | Dónde | Acción |
|---|---|---|
| 19 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En la pantalla **Trust Anchors (TAs)**, marcar ambas casillas: **Enable the distribution of trust anchors for this zone** y **Enable automatic update of trust anchors on key rollover (RFC 5011)**. |
| 20 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Hacer clic en **Next >**. |

### A.6 Signing and Polling Parameters

| Paso | Dónde | Acción |
|---|---|---|
| 21 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En la pantalla **Signing and Polling Parameters**, dejar todos los valores por defecto de la tabla siguiente. |
| 22 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Hacer clic en **Next >**. |

| Parámetro | Valor |
|---|---|
| DS record generation algorithm | SHA-1 and SHA-256 |
| DS record TTL (seconds) | 3600 |
| DNSKEY record TTL (seconds) | 3600 |
| Secure delegation polling period (hours) | 12 |
| Signature inception (hours) | 1 |

### A.7 Finalizar la firma

| Paso | Dónde | Acción |
|---|---|---|
| 23 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Revisar el resumen y hacer clic en **Next >** para iniciar la firma. |
| 24 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Esperar a que el asistente complete el proceso. Cuando aparezca el mensaje **La zona se firmó correctamente**, hacer clic en **Finalizar**. |
| 25 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el Administrador de DNS, clic derecho sobre `MiguelRM.local` > **Actualizar**. Se visualizan los nuevos registros DNSSEC (`RRSIG`, `DNSKEY`, `NSEC3`, `DS`) generados por la firma. |

| Resultado | Detalle |
|---|---|
| Resultado esperado | La zona `MiguelRM.local` queda firmada con DNSSEC. El estado cambia de **Not Signed** a **Signed** en la lista de zonas. |

---

<a id="punto-b"></a>

## Punto B - Propagar la configuración de DNSSEC a los clientes mediante GPO

![Punto B](https://img.shields.io/badge/Punto-B-5C2D91?style=flat-square)

### B.1 Crear la GPO DNSSEC

| Paso | Dónde | Acción |
|---|---|---|
| 1 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el **Administrador del servidor**, ir a **Herramientas > Administración de directivas de grupo**. |
| 2 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el árbol, expandir el dominio y localizar la unidad organizativa `Empresa`. |
| 3 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre `Empresa` > **Crear un GPO en este dominio y vincularlo aquí...**. |
| 4 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Asignar el nombre `DNSSEC` y hacer clic en **Aceptar**. |
| 5 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Clic derecho sobre la GPO recién creada > **Editar**. |

### B.2 Configurar la Directiva de Resolución de Nombres

| Paso | Dónde | Acción |
|---|---|---|
| 6 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el Editor de administración de directivas de grupo, navegar hasta: `Configuración del equipo > Directivas > Configuración de Windows > Directiva de resolución de nombres`. |
| 7 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el panel derecho, en la sección **Crear reglas**, configurar los campos de la tabla siguiente. |
| 8 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el campo **Entidad de certificación (Opcional)**, hacer clic en **Examinar**. |
| 9 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | En el recuadro de certificados, seleccionar el certificado disponible y hacer clic en **Aceptar**. |
| 10 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Marcar la casilla **Habilitar DNSSEC en esta regla**. |
| 11 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Bajo **Configuración de DNSSEC > Validación**, marcar **Requerir a los clientes DNS que comprueben que el servidor DNS haya validado el nombre y la dirección**. |
| 12 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Bajo **IPsec**, marcar **Usar IPsec para la comunicación entre el cliente DNS y el servidor DNS**. |
| 13 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Hacer clic en **Aplicar** en la parte inferior del panel. |

| Campo | Valor |
|---|---|
| ¿A qué parte del espacio de nombres se aplica esta regla? | **Sufijo** |
| Nombre del sufijo | `MiguelRM.local` |

### B.3 Aplicar la GPO en el servidor

| Paso | Dónde | Acción |
|---|---|---|
| 14 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Abrir el **Símbolo del sistema (CMD)** como Administrador y ejecutar `gpupdate /force`. Esperar a que se complete la actualización de directivas. |

### B.4 Aplicar la GPO en el cliente

| Paso | Dónde | Acción |
|---|---|---|
| 15 | ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Abrir el **Símbolo del sistema (CMD)** como Administrador y ejecutar `gpupdate /force`. |
| 16 | ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Verificar que la directiva DNSSEC fue aplicada ejecutando `gpresult /r`. Confirmar que `DNSSEC` aparece en la lista de directivas de equipo aplicadas. |

| Resultado | Detalle |
|---|---|
| Resultado esperado | La GPO DNSSEC queda configurada y desplegada. Los clientes del dominio dentro de la OU `Empresa` validan las respuestas DNS mediante firmas DNSSEC y usan IPsec para proteger la comunicación con el servidor DNS. |
| Requisito | El equipo cliente debe estar dentro de la OU `Empresa` en Active Directory para que la GPO se aplique correctamente. |

---

<a id="comprobacion"></a>

## Comprobación - Verificar que DNSSEC fue aplicado correctamente

![Comprobación](https://img.shields.io/badge/Comprobaci%C3%B3n-DNSSEC-2EA043?style=flat-square)

### En el servidor - Verificar los registros DNSSEC

| Paso | Dónde | Acción |
|---|---|---|
| 1 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Abrir el **Administrador de DNS** > **Forward Lookup Zones** > `MiguelRM.local`. |
| 2 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Verificar que existan en la zona los tipos de registro de la tabla siguiente. |
| 3 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Verificar que en la lista de zonas el estado de `MiguelRM.local` muestre **Signed** en la columna DNSSEC Status. |
| 4 | ![SERVIDOR](https://img.shields.io/badge/-SERVIDOR-0078D4?style=flat-square) | Desde **PowerShell** como Administrador, ejecutar `Resolve-DnsName -Name MiguelRM.local -Type DNSKEY -Server 127.0.0.1`. Debe retornar los registros DNSKEY de la zona firmada. |

| Registro | Descripción |
|---|---|
| `RRSIG` | Firma digital de los registros DNS |
| `DNSKEY` | Claves públicas KSK y ZSK |
| `NSEC3` | Prueba de no existencia autenticada |
| `DS` | Delegación de firma hacia la zona padre |

### En el cliente - Verificar que la GPO DNSSEC fue aplicada

| Paso | Dónde | Acción |
|---|---|---|
| 5 | ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Abrir **CMD** como Administrador y ejecutar `gpresult /r`. Confirmar que `DNSSEC` aparece bajo **Directivas de equipo aplicadas**. |
| 6 | ![CLIENTE](https://img.shields.io/badge/-CLIENTE-2EA043?style=flat-square) | Verificar la resolución DNS con validación DNSSEC ejecutando en PowerShell `Resolve-DnsName -Name MiguelRM.local -Type A -DnssecOk`. Si la respuesta incluye registros `RRSIG`, DNSSEC está funcionando correctamente en el cliente. |

| Resultado | Detalle |
|---|---|
| Resultado esperado | DNSSEC queda habilitado, firmado y propagado correctamente a los clientes del dominio `MiguelRM.local`. |

---

| Nota | Detalle |
|---|---|
| Entorno | Toda la práctica fue realizada en un entorno virtualizado con VMware Workstation Pro 17, utilizando Windows Server 2022 como controlador de dominio y Windows 10 Pro como cliente del dominio. El equipo cliente debe estar dentro de la OU `Empresa` en Active Directory para que las GPOs sean aplicadas correctamente. |

---

<div align="center">

![Author](https://img.shields.io/badge/Author-Miguel_Ramirez_Meli-0A3D91?style=flat-square)
![Student ID](https://img.shields.io/badge/Student_ID-20251367-2EA043?style=flat-square)
![Career](https://img.shields.io/badge/Information_Security-ITLA-1F6FEB?style=flat-square)

</div>