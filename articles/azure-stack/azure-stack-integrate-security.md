---
title: Reenvío de syslog de Azure Stack
description: Aprenda a integrar Azure Stack con soluciones de supervisión mediante el reenvío de syslog
services: azure-stack
author: PatAltimore
manager: femila
ms.service: azure-stack
ms.topic: article
ms.date: 01/28/2019
ms.author: patricka
ms.reviewer: fiseraci
ms.lastreviewed: 01/28/2019
keywords: ''
ms.openlocfilehash: 7dff82538448b27f14dd81e2862cd63d4dd56a9b
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2019
ms.locfileid: "55247109"
---
# <a name="azure-stack-datacenter-integration---syslog-forwarding"></a>Integración del centro de datos de Azure Stack: reenvío de syslog

Este artículo muestra cómo usar syslog para integrar la infraestructura de Azure Stack con soluciones de seguridad externas ya implementadas en su centro de datos. Por ejemplo, un sistema de administración de eventos de información de seguridad (SIEM). El canal de syslog expone las auditorías, alertas y registros de seguridad de todos los componentes de la infraestructura de Azure Stack. Use el reenvío de syslog para integrar con soluciones de supervisión de seguridad o para recuperar todas las auditorías, alertas y registros de seguridad a fin de almacenarlos para conservarlos.

A partir de la actualización 1809, Azure Stack tiene un cliente de syslog integrado que, una vez configurado, emite mensajes de syslog con la carga de Common Event Format (CEF).

En el diagrama siguiente, se describe la integración de Azure Stack con un sistema de SIEM externo. Hay dos patrones de integración que deben tenerse en cuenta: el primero (de color azul) es la infraestructura de Azure Stack que abarca los nodos de Hyper-V y las máquinas virtuales de la infraestructura. Todas las auditorías, los registros de seguridad y las alertas de los componentes se recopilan y exponen de forma centralizada mediante syslog con la carga de CEF. Este patrón de integración se describe en esta página del documento.
El segundo patrón de integración es el que aparece representado en color naranja y engloba a los controladores de administración de placa base (BMC), al host de ciclo de vida de hardware (HLH), a las máquinas virtuales o dispositivos virtuales que ejecutan el software de administración y supervisión de asociados de hardware y a los conmutadores de la parte superior del bastidor (TOR). Como estos componentes son específicos de los asociados del hardware, póngase en contacto con ellos para obtener documentación acerca de cómo integrarlos con un sistema de SIEM externo.

![Diagrama de reenvío de syslog](media/azure-stack-integrate-security/syslog-forwarding.png)

## <a name="configuring-syslog-forwarding"></a>Configuración del reenvío de syslog

El cliente de syslog en Azure Stack admite las siguientes configuraciones:

1. **Syslog a través de TCP, con autenticación mutua (cliente y servidor) y cifrado TLS 1.2:** en esta configuración, tanto el servidor como el cliente de syslog pueden comprobar la identidad del otro mediante certificados. Los mensajes se envían a través de un canal cifrado TLS 1.2.

2. **Syslog a través de TCP, con autenticación de servidor y cifrado TLS 1.2:** en esta configuración, el cliente de syslog puede comprobar la identidad del servidor de syslog mediante un certificado. Los mensajes se envían a través de un canal cifrado TLS 1.2.

3. **Syslog a través de TCP, sin cifrado:** en esta configuración, el cliente de syslog y las identidades de servidor de syslog no se comprueban. Los mensajes se envían en texto sin cifrar a través de TCP.

4. **Syslog a través de UDP, sin cifrado:** en esta configuración, el cliente de syslog y las identidades de servidor de syslog no se comprueban. Los mensajes se envían en texto sin cifrar a través de UDP.

> [!IMPORTANT]
> Microsoft recomienda encarecidamente utilizar TCP con autenticación y cifrado (configuración n.º 1 o, como mínimo, la n.º 2) en entornos de producción a fin de proteger contra ataques de tipo "Man-in-the-middle" y de la interceptación de mensajes.

### <a name="cmdlets-to-configure-syslog-forwarding"></a>Cmdlets para configurar el reenvío de syslog
Configurar el reenvío de syslog requiere acceso al punto de conexión con privilegios (PEP). Se han agregado dos cmdlets de PowerShell al PEP para configurar el reenvío de syslog:


```powershell
### cmdlet to pass the syslog server information to the client and to configure the transport protocol, the encryption and the authentication between the client and the server

Set-SyslogServer [-ServerName <String>] [-ServerPort <String>] [-NoEncryption] [-SkipCertificateCheck] [-SkipCNCheck] [-UseUDP] [-Remove]

### cmdlet to configure the certificate for the syslog client to authenticate with the server

Set-SyslogClient [-pfxBinary <Byte[]>] [-CertPassword <SecureString>] [-RemoveCertificate] 
```
#### <a name="cmdlets-parameters"></a>Parámetros de los cmdlets

Parámetros del cmdlet *Set-SyslogServer*:

| Parámetro | DESCRIPCIÓN | Type | Obligatorio |
|---------|---------|---------|---------|
|*ServerName* | Dirección IP o FQDN del servidor de syslog | string | Sí|
|*ServerPort* | Número de puerto de escucha del servidor de syslog | string | Sí|
|*NoEncryption*| Obliga al cliente a enviar los mensajes de syslog como texto sin cifrar | Marca | no|
|*SkipCertificateCheck*| Se omite la validación del certificado proporcionado por el servidor de syslog durante el protocolo de enlace TLS inicial | Marca | no|
|*SkipCNCheck*| Se omite la validación del valor de Nombre común del certificado proporcionado por el servidor de syslog durante el protocolo de enlace TLS inicial | Marca | no|
|*UseUDP*| Se usa syslog con UDP como protocolo de transporte |Marca | no|
|*Remove*| Se quita la configuración del servidor desde el cliente y se detiene el reenvío de syslog| Marca | no|

Parámetros del cmdlet *Set-SyslogClient*:
| Parámetro | DESCRIPCIÓN | Type |
|---------|---------| ---------|
| *pfxBinary* | Archivo PFX que contiene el certificado que usará el cliente como identidad para autenticarse en el servidor de syslog  | Byte[] |
| *CertPassword* |  Contraseña para importar la clave privada que está asociada con el archivo PFX | SecureString |
|*RemoveCertificate* | Se quita el certificado del cliente | Marca|

### <a name="configuring-syslog-forwarding-with-tcp-mutual-authentication-and-tls-12-encryption"></a>Configuración del reenvío de syslog con TCP, autenticación mutua y cifrado TLS 1.2

En esta configuración, el cliente de syslog en Azure Stack reenvía los mensajes al servidor de syslog a través de TCP, con el cifrado TLS 1.2. Durante el protocolo de enlace inicial, el cliente comprueba que el servidor proporciona un certificado válido y de confianza; de forma similar, el cliente también proporciona un certificado al servidor como prueba de su identidad. Esta configuración es la más segura, ya que proporciona una validación completa de la identidad tanto del cliente como del servidor y envía los mensajes a través de un canal cifrado. 

> [!IMPORTANT]
> Microsoft recomienda encarecidamente usar esta configuración para entornos de producción. 

Para configurar el reenvío de syslog con TCP, autenticación mutua y cifrado TLS 1.2, ejecute estos dos cmdlets en una sesión PEP:

```powershell
# Configure the server
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>

# Provide certificate to the client to authenticate against the server
Set-SyslogClient -pfxBinary <Byte[] of pfx file> -CertPassword <SecureString, password for accessing the pfx file>
```

El certificado de cliente debe tener la misma raíz que el proporcionado durante la implementación de Azure Stack. También debe contener una clave privada.

```powershell
##Example on how to set your syslog client with the certificate for mutual authentication.
##This example script must be run from your hardware lifecycle host or privileged access workstation.

$ErcsNodeName = "<yourPEP>"
$password = ConvertTo-SecureString -String "<your cloudAdmin account password" -AsPlainText -Force
 
$cloudAdmin = "<your cloudAdmin account name>"
$CloudAdminCred = New-Object System.Management.Automation.PSCredential ($cloudAdmin, $password)
 
$certPassword = $password
$certContent = Get-Content -Path C:\cert\<yourClientCertificate>.pfx -Encoding Byte
 
$params = @{ 
    ComputerName = $ErcsNodeName 
    Credential = $CloudAdminCred 
    ConfigurationName = "PrivilegedEndpoint" 
}

$session = New-PSSession @params
 
$params = @{ 
    Session = $session 
    ArgumentList = @($certContent, $certPassword) 
}
Write-Verbose "Invoking cmdlet to set syslog client certificate..." -Verbose 
Invoke-Command @params -ScriptBlock { 
    param($CertContent, $CertPassword) 
    Set-SyslogClient -PfxBinary $CertContent -CertPassword $CertPassword }
```

### <a name="configuring-syslog-forwarding-with-tcp-server-authentication-and-tls-12-encryption"></a>Configuración del reenvío de syslog con TCP, autenticación de servidor y cifrado TLS 1.2

En esta configuración, el cliente de syslog en Azure Stack reenvía los mensajes al servidor de syslog a través de TCP, con el cifrado TLS 1.2. Durante el protocolo de enlace inicial, el cliente también comprueba que el servidor proporciona un certificado válido y de confianza. Con esta configuración se evita que el cliente envíe mensajes a destinos que no sean de confianza.
TCP con autenticación y cifrado es la configuración predeterminada, y representa el nivel mínimo de seguridad que Microsoft recomienda para un entorno de producción. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
```

En caso de que desee probar la integración del servidor de syslog con el cliente de Azure Stack mediante el uso de un certificado autofirmado o que no sea de confianza, puede usar estas marcas para omitir la validación del servidor realizada por el cliente durante el protocolo de enlace inicial.

```powershell
 #Skip validation of the Common Name value in the server certificate. Use this flag if you provide an IP address for your syslog server
 Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
 -SkipCNCheck

 #Skip entirely the server certificate validation
 Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on>
 -SkipCertificateCheck
```

> [!IMPORTANT]
> Microsoft no recomienda el uso de la marca -SkipCertificateCheck para entornos de producción. 

### <a name="configuring-syslog-forwarding-with-tcp-and-no-encryption"></a>Configuración del reenvío de syslog con TCP y sin cifrado

En esta configuración, el cliente de syslog en Azure Stack reenvía los mensajes al servidor de syslog a través de TCP, sin cifrado. El cliente no comprueba la identidad del servidor ni proporciona su propia identidad en el servidor para la comprobación. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on> -NoEncryption
```

> [!IMPORTANT]
> Microsoft recomienda no usar esta configuración para entornos de producción. 


### <a name="configuring-syslog-forwarding-with-udp-and-no-encryption"></a>Configuración del reenvío de syslog con UDP y sin cifrado

En esta configuración, el cliente de syslog en Azure Stack reenvía los mensajes al servidor de syslog a través de UDP, sin cifrado. El cliente no comprueba la identidad del servidor ni proporciona su propia identidad en el servidor para la comprobación. 

```powershell
Set-SyslogServer -ServerName <FQDN or ip address of syslog server> -ServerPort <Port number on which the syslog server is listening on> -UseUDP
```

Aunque UDP sin cifrado es la configuración más fácil, no proporciona ninguna protección frente a ataques de tipo "Man-in-the-middle" ni frente a la interceptación de los mensajes. 

> [!IMPORTANT]
> Microsoft recomienda no usar esta configuración para entornos de producción. 


## <a name="removing-syslog-forwarding-configuration"></a>Eliminación de la configuración del reenvío de syslog

Para quitar la configuración del servidor de syslog por completo y detener el reenvío de syslog:

**Quite la configuración del servidor de syslog desde el cliente**

```PowerShell  
Set-SyslogServer -Remove
```

**Quite el certificado del cliente desde el cliente**

```PowerShell  
Set-SyslogClient -RemoveCertificate
```

## <a name="verifying-the-syslog-setup"></a>Comprobación de la configuración de syslog

Si conectó correctamente el cliente de syslog al servidor de syslog, pronto debería comenzar a recibir eventos. Si no ve ningún evento, ejecute los siguientes cmdlets para comprobar la configuración de su cliente de syslog:

**Compruebe la configuración del servidor en el cliente de syslog**

```PowerShell  
Get-SyslogServer
```

**Compruebe la configuración del certificado en el cliente de syslog**

```PowerShell  
Get-SyslogClient
```

## <a name="syslog-message-schema"></a>Esquema de los mensajes de Syslog

El reenvío de syslog de la infraestructura de Azure Stack envía los mensajes con el formato Common Event Format (CEF).
Cada mensaje de syslog se estructura según este esquema: 

```Syslog
<Time> <Host> <CEF payload>
```

La carga de CEF se basa en la siguiente estructura, pero la asignación para cada campo varía según el tipo de mensaje (eventos de Windows, alerta creada, alerta cerrada).

```CEF
# Common Event Format schema
CEF: <Version>|<Device Vendor>|<Device Product>|<Device Version>|<Signature ID>|<Name>|<Severity>|<Extensions>
* Version: 0.0
* Device Vendor: Microsoft
* Device Product: Microsoft Azure Stack
* Device Version: 1.0
```

### <a name="cef-mapping-for-privileged-endpoint-events"></a>Asignación de CEF para eventos de punto de conexión con privilegios

```
Prefix fields
* Signature ID: Microsoft-AzureStack-PrivilegedEndpoint: <PEP Event ID>
* Name: <PEP Task Name>
* Severity: mapped from PEP Level (details see the PEP Severity table below)
```

Tabla de eventos para el punto de conexión con privilegios:

| Evento | Id. del evento de PEP | Nombre de tarea de PEP | Gravedad |
|-------|--------------| --------------|----------|
|PrivilegedEndpointAccessed|1000|PrivilegedEndpointAccessedEvent|5|
|SupportSessionTokenRequested |1001|SupportSessionTokenRequestedEvent|5|
|SupportSessionDevelopmentTokenRequested |1002|SupportSessionDevelopmentTokenRequestedEvent|5|
|SupportSessionUnlocked |1003|SupportSessionUnlockedEvent|10|
|SupportSessionFailedToUnlock |1004|SupportSessionFailedToUnlockEvent|10|
|PrivilegedEndpointClosed |1005|PrivilegedEndpointClosedEvent|5|
|NewCloudAdminUser |1006|NewCloudAdminUserEvent|10|
|RemoveCloudAdminUser |1007|RemoveCloudAdminUserEvent|10|
|SetCloudAdminUserPassword |1008|SetCloudAdminUserPasswordEvent|5|
|GetCloudAdminPasswordRecoveryToken |1009|GetCloudAdminPasswordRecoveryTokenEvent|10|
|ResetCloudAdminPassword |1010|ResetCloudAdminPasswordEvent|10|

Tabla de gravedad de PEP:

| Gravedad | Nivel | Valor numérico |
|----------|-------| ----------------|
|0|Undefined|Valor: 0. Indica los registros en todos los niveles|
|10|Crítico|Valor: 1. Indica los registros de una alerta crítica|
|8|Error| Valor: 2. Indica los registros de error|
|5|Advertencia|Valor: 3. Indica los registros de advertencia|
|2|Información|Valor: 4. Indica los registros de un mensaje informativo|
|0|Detallado|Valor: 5. Indica los registros en todos los niveles|

### <a name="cef-mapping-for-recovery-endpoint-events"></a>Asignación de CEF para eventos de punto de conexión de recuperación

```
Prefix fields
* Signature ID: Microsoft-AzureStack-PrivilegedEndpoint: <REP Event ID>
* Name: <REP Task Name>
* Severity: mapped from REP Level (details see the REP Severity table below)
```

Tabla de eventos del punto de conexión de recuperación:

| Evento | Id. del evento de REP | Nombre de tarea de REP | Gravedad |
|-------|--------------| --------------|----------|
|RecoveryEndpointAccessed |1011|RecoveryEndpointAccessedEvent|5|
|RecoverySessionTokenRequested |1012|RecoverySessionTokenRequestedEvent |5|
|RecoverySessionDevelopmentTokenRequested |1013|RecoverySessionDevelopmentTokenRequestedEvent|5|
|RecoverySessionUnlocked |1014|RecoverySessionUnlockedEvent |10|
|RecoverySessionFailedToUnlock |1015|RecoverySessionFailedToUnlockEvent|10|
|RecoveryEndpointClosed |1016|RecoveryEndpointClosedEvent|5|

Tabla de gravedad de REP:
| Gravedad | Nivel | Valor numérico |
|----------|-------| ----------------|
|0|Undefined|Valor: 0. Indica los registros en todos los niveles|
|10|Crítico|Valor: 1. Indica los registros de una alerta crítica|
|8|Error| Valor: 2. Indica los registros de error|
|5|Advertencia|Valor: 3. Indica los registros de advertencia|
|2|Información|Valor: 4. Indica los registros de un mensaje informativo|
|0|Detallado|Valor: 5. Indica los registros en todos los niveles|

### <a name="cef-mapping-for-windows-events"></a>Asignación de CEF para los eventos de Windows

```
* Signature ID: ProviderName:EventID
* Name: TaskName
* Severity: Level (for details, see the severity table below)
* Extension: Custom Extension Name (for details, see the Custom Extension table below)
```

Tabla de gravedad para los eventos de Windows:
| Valor de gravedad de CEF | Nivel de eventos de Windows | Valor numérico |
|--------------------|---------------------| ----------------|
|0|Undefined|Valor: 0. Indica los registros en todos los niveles|
|10|Crítico|Valor: 1. Indica los registros de una alerta crítica|
|8|Error| Valor: 2. Indica los registros de error|
|5|Advertencia|Valor: 3. Indica los registros de advertencia|
|2|Información|Valor: 4. Indica los registros de un mensaje informativo|
|0|Detallado|Valor: 5. Indica los registros en todos los niveles|

Tabla de extensión personalizada para los eventos de Windows creados en Azure Stack:
| Nombre de la extensión personalizada | Ejemplo de evento de Windows | 
|-----------------------|---------|
|MasChannel | Sistema|
|MasComputer | test.azurestack.contoso.com|
|MasCorrelationActivityID| C8F40D7C-3764-423B-A4FA-C994442238AF|
|MasCorrelationRelatedActivityID| C8F40D7C-3764-423B-A4FA-C994442238AF|
|MasEventData| svchost!!4132,G,0!!!!EseDiskFlushConsistency!!ESENT!!0x800000|
|MasEventDescription| La configuración de directiva de grupo para el usuario se ha procesado correctamente. No se detectaron cambios desde el último procesamiento correcto de la directiva de grupo.|
|MasEventID|1501|
|MasEventRecordID|26637|
|MasExecutionProcessID | 29380|
|MasExecutionThreadID |25480|
|MasKeywords |0x8000000000000000|
|MasKeywordName |Auditoría correcta|
|MasLevel |4|
|MasOpcode |1|
|MasOpcodeName |info|
|MasProviderEventSourceName ||
|MasProviderGuid |AEA1B4FA-97D1-45F2-A64C-4D69FFFD92C9|
|MasProviderName |Microsoft-Windows-GroupPolicy|
|MasSecurityUserId |\<SID de Windows\> |
|MasTask |0|
|MasTaskCategory| Creación de un proceso|
|MasUserData|KB4093112!!5112!!Installed!!0x0!!WindowsUpdateAgent Xpath: /Event/UserData/*|
|MasVersion|0|

### <a name="cef-mapping-for-alerts-created"></a>Asignación de CEF para las alertas creadas

```
* Signature ID: Microsoft Azure Stack Alert Creation : FaultTypeId
* Name: FaultTypeId : AlertId
* Severity: Alert Severity (for details, see alerts severity table below)
* Extension: Custom Extension Name (for details, see the Custom Extension table below)
```

Tabla de gravedad de alertas:
| Gravedad | Nivel |
|----------|-------|
|0|Undefined|
|10|Crítico|
|5|Advertencia|

Tabla de extensión personalizada para las alertas creadas en Azure Stack:
| Nombre de la extensión personalizada | Ejemplo | 
|-----------------------|---------|
|MasEventDescription|DESCRIPCIÓN: una cuenta de usuario \<TestUser\> se creó para \<TestDomain\>. Es un riesgo potencial de seguridad. -- CORRECCIÓN: Póngase en contacto con el servicio de soporte técnico. Se requiere el servicio de asistencia al cliente se para resolver este problema. No intente resolver este problema sin su ayuda. Antes de abrir una solicitud de soporte técnico, inicie el proceso de recopilación de archivos de registro siguiendo las instrucciones de https://aka.ms/azurestacklogfiles |

### <a name="cef-mapping-for-alerts-closed"></a>Asignación de CEF para las alertas cerradas

```
* Signature ID: Microsoft Azure Stack Alert Creation : FaultTypeId
* Name: FaultTypeId : AlertId
* Severity: Information
```

El ejemplo siguiente muestra un mensaje de syslog con carga de CEF:
```
2018:05:17:-23:59:28 -07:00 TestHost CEF:0.0|Microsoft|Microsoft Azure Stack|1.0|3|TITLE: User Account Created -- DESCRIPTION: A user account \<TestUser\> was created for \<TestDomain\>. It's a potential security risk. -- REMEDIATION: Please contact Support. Customer Assistance is required to resolve this issue. Do not try to resolve this issue without their assistance. Before you open a support request, start the log file collection process using the guidance from https://aka.ms/azurestacklogfiles|10
```

## <a name="next-steps"></a>Pasos siguientes

[Directiva de mantenimiento](azure-stack-servicing-policy.md)