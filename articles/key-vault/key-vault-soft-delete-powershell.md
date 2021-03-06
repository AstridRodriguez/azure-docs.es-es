---
title: 'Azure Key Vault: Uso de la eliminación temporal con PowerShell'
description: Ejemplos de casos de uso de eliminación temporal con fragmentos de código de PowerShell
author: bryanla
manager: barbkess
ms.service: key-vault
ms.topic: conceptual
ms.date: 02/01/2018
ms.author: bryanla
ms.openlocfilehash: 70437403d3b78b7f8b9eef921c933a68793450da
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113590"
---
# <a name="how-to-use-key-vault-soft-delete-with-powershell"></a>Uso de la eliminación temporal de Key Vault con PowerShell

La característica de eliminación temporal de Azure Key Vault permite recuperar almacenes y objetos de almacenes eliminados. En concreto, la eliminación temporal trata los siguientes escenarios:

- Compatibilidad con la eliminación recuperable de una instancia de Key Vault
- Compatibilidad con la eliminación recuperable de objetos de Key Vault: claves, secretos y certificados

## <a name="prerequisites"></a>Requisitos previos

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

- Azure PowerShell 1.0.0 o posterior: si aún no lo tiene instalado, instale Azure PowerShell y asócielo con la suscripción de Azure. Para ello, consulte [How to install and configure Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview) (Instalación y configuración de Azure PowerShell). 

>[!NOTE]
> Hay una versión no actualizada del archivo de formato de salida de PowerShell de Key Vault que se **podría** cargar en el entorno en lugar de la versión correcta. Se espera una versión actualizada de PowerShell que contenga la corrección necesaria para el formato de salida y este tema se actualizará en ese momento. La solución actual, si se encuentra con este problema de formato, es:
> - Use la siguiente consulta si no ve la propiedad Habilitar la eliminación temporal descrita en este tema: `$vault = Get-AzKeyVault -VaultName myvault; $vault.EnableSoftDelete`.


Para obtener más información específica de referencia sobre Key Vault para PowerShell, consulte la [referencia de Azure Key Vault para PowerShell](/powershell/module/az.keyvault).

## <a name="required-permissions"></a>Permisos necesarios

Las operaciones de Key Vault se administran por separado mediante los permisos de control de acceso basado en roles (RBAC) como se indica a continuación:

| Operación | DESCRIPCIÓN | Permiso del usuario |
|:--|:--|:--|
|Enumerar|Enumera los almacenes de claves eliminados.|Microsoft.KeyVault/deletedVaults/read|
|Recuperar|Restaura un almacén de claves eliminado.|Microsoft.KeyVault/vaults/write|
|Purgar|Elimina permanentemente un almacén de claves eliminado y todo su contenido.|Microsoft.KeyVault/locations/deletedVaults/purge/action|

Para más información sobre los permisos y el control de acceso, consulte [Protección de un almacén de claves](key-vault-secure-your-key-vault.md).

## <a name="enabling-soft-delete"></a>Habilitar la eliminación temporal

La "eliminación temporal" se habilita para permitir la recuperación de un almacén de claves eliminado o de los objetos almacenados en un almacén de claves.

> [!IMPORTANT]
> La acción de habilitar la "eliminación temporal" en un almacén de claves es irreversible. Una vez que la propiedad de eliminación temporal se ha establecido en "true", no se puede modificar ni eliminar.  

### <a name="existing-key-vault"></a>Almacén de claves existente

Para un almacén de claves existente denominado ContosoVault, habilite la eliminación temporal como se indica a continuación. 

```powershell
($resource = Get-AzResource -ResourceId (Get-AzKeyVault -VaultName "ContosoVault").ResourceId).Properties | Add-Member -MemberType "NoteProperty" -Name "enableSoftDelete" -Value "true"

Set-AzResource -resourceid $resource.ResourceId -Properties $resource.Properties
```

### <a name="new-key-vault"></a>Almacén de claves nuevo

La habilitación de la eliminación temporal para un nuevo almacén de claves se realiza en el momento de la creación agregando el indicador de habilitación de eliminación temporal al comando create.

```powershell
New-AzKeyVault -Name "ContosoVault" -ResourceGroupName "ContosoRG" -Location "westus" -EnableSoftDelete
```

### <a name="verify-soft-delete-enablement"></a>Comprobación de la habilitación de la eliminación temporal

Para comprobar que un almacén de claves tiene habilitada la eliminación temporal, ejecute el comando *show* y busque el atributo "Soft Delete Enabled?" :

```powershell
Get-AzKeyVault -VaultName "ContosoVault"
```

## <a name="deleting-a-soft-delete-protected-key-vault"></a>Eliminación de un almacén de claves protegido por eliminación temporal

El comando para eliminar un almacén de claves cambia de comportamiento, según si está habilitada la eliminación temporal.

> [!IMPORTANT]
>Si ejecuta el comando anterior para un almacén de claves que no tiene habilitada la eliminación temporal, este almacén de claves y todo su contenido se eliminarán definitivamente sin ninguna oportunidad de recuperación.

```powershell
Remove-AzKeyVault -VaultName 'ContosoVault'
```

### <a name="how-soft-delete-protects-your-key-vaults"></a>Uso de la eliminación temporal para proteger los almacenes de claves

Con la eliminación temporal habilitada:

- El almacén de claves eliminado se quita de su grupo de recursos y se coloca en un espacio de nombres reservado, asociado con la ubicación donde se creó. 
- Los objetos eliminados, como las claves, los secretos y los certificados, son inaccesibles mientras el almacén de claves que los contenga esté en estado eliminado. 
- El nombre DNS de un almacén de claves eliminado es reservado, así se impide que se cree un almacén de claves con el mismo nombre.  

Puede ver los almacenes de claves en estado eliminado asociados a su suscripción con el comando siguiente:

```powershell
PS C:\> Get-AzKeyVault -InRemovedState 
```

- El campo *Id* (Id.) se puede usar para identificar el recurso durante la recuperación o purga. 
- *Resource ID* (Identificador de recurso) es el identificador de recurso original de este almacén. Puesto que este almacén de claves está ahora en estado eliminado, no existe ningún recurso con ese identificador de recurso. 
- *Scheduled Purge Date* (Fecha de purga programada) es la fecha en la que el almacén se eliminará definitivamente si no se realiza ninguna acción. El período de retención predeterminado que se utiliza para calcular la *fecha de purga programada* es de 90 días.

## <a name="recovering-a-key-vault"></a>Recuperación de un almacén de claves

Para recuperar un almacén de claves, debe especificar el nombre del almacén de claves, el grupo de recursos y la ubicación. Anote la ubicación y el grupo de recursos del almacén de claves eliminado ya que los necesitará para un proceso de recuperación.

```powershell
Undo-AzKeyVaultRemoval -VaultName ContosoVault -ResourceGroupName ContosoRG -Location westus
```

Cuando se recupera un almacén de claves, se crea un recurso con el identificador de recurso original del almacén de claves. Si se quita el grupo de recursos original, se debe crear uno con el mismo nombre antes de intentar la recuperación.

## <a name="deleting-and-purging-key-vault-objects"></a>Eliminación y purga de objetos del almacén de claves

El siguiente comando elimina la clave "ContosoFirstKey", en un almacén de claves denominado "ContosoVault", que tiene habilitada la eliminación temporal:

```powershell
Remove-AzKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey
```

Con el almacén de claves habilitado para la eliminación temporal, una clave eliminada todavía se muestra para eliminar, a no ser que enumere explícitamente las claves eliminadas. La mayoría de las operaciones que se realicen en una clave en estado eliminado producirán error, excepto las operaciones de enumeración, recuperación o purga. 

Por ejemplo, el siguiente comando enumera las claves eliminadas en el almacén de claves "ContosoVault":

```powershell
Get-AzKeyVaultKey -VaultName ContosoVault -InRemovedState
```

### <a name="transition-state"></a>Estado de transición 

Cuando se elimina una clave de un almacén de claves con eliminación temporal habilitada, puede tardar unos segundos en completarse la transición. Durante esta transición, puede parecer que la clave no está ni en estado activo ni en estado eliminado. 

### <a name="using-soft-delete-with-key-vault-objects"></a>Uso de la eliminación temporal con objetos de almacén de claves

Al igual que con los almacenes de claves, una clave, secreto o certificado eliminados permanecerán en ese estado durante 90 días a menos que los recupere o los purgue. 

#### <a name="keys"></a>Claves

Para recuperar una clave eliminada temporalmente:

```powershell
Undo-AzKeyVaultKeyRemoval -VaultName ContosoVault -Name ContosoFirstKey
```

Para eliminar definitivamente (lo que se conoce también como purga) una clave eliminada temporalmente:

> [!IMPORTANT]
> Al purgar una clave se elimina definitivamente, lo que significa que no se podrá recuperar de nuevo. 

```powershell
Remove-AzKeyVaultKey -VaultName ContosoVault -Name ContosoFirstKey -InRemovedState
```

Las acciones de **recuperación** y **purga** tienen sus propios permisos asociados en una directiva de acceso del almacén de claves. Para que un usuario o una entidad de servicio puedan ejecutar una acción de **recuperación** o **purga**, deben tener el permiso correspondiente para ese almacén o secreto. De forma predeterminada, el permiso de **purga** no se agrega a la directiva de acceso del almacén de claves cuando se usa el acceso directo "todos" para conceder todos los permisos. Debe conceder específicamente el permiso de **purga**. 

#### <a name="set-a-key-vault-access-policy"></a>Establecimiento de una directiva de acceso del almacén de claves

El siguiente comando concede a user@contoso.com el permiso para realizar varias operaciones en las claves de *ContosoVault*, incluida la **purga**.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName ContosoVault -UserPrincipalName user@contoso.com -PermissionsToKeys get,create,delete,list,update,import,backup,restore,recover,purge
```

>[!NOTE] 
> Si tiene un almacén de claves existente para el que se acaba de habilitar la eliminación temporal, puede que no tenga los permisos de **recuperación** y **purga**.

#### <a name="secrets"></a>Secretos

Al igual que las claves, los secretos se administran con sus propios comandos:

- Elimine un secreto llamado SQLPassword: 
```powershell
Remove-AzKeyVaultSecret -VaultName ContosoVault -name SQLPassword
```

- Enumere todos los secretos eliminados de un almacén de claves: 
```powershell
Get-AzKeyVaultSecret -VaultName ContosoVault -InRemovedState
```

- Recupere un secreto en el estado eliminado: 
```powershell
Undo-AzKeyVaultSecretRemoval -VaultName ContosoVault -Name SQLPAssword
```

- Purgue un secreto en el estado eliminado: 

  > [!IMPORTANT]
  > Si purga un secreto se eliminará definitivamente y no se podrá volver a recuperar.

  ```powershell
  Remove-AzKeyVaultSecret -VaultName ContosoVault -InRemovedState -name SQLPassword
  ```

## <a name="purging-a-soft-delete-protected-key-vault"></a>Purga de un almacén de claves protegido por eliminación temporal

> [!IMPORTANT]
> Si purga un almacén de claves o uno de los objetos que contiene, se eliminará definitivamente, lo que significa que no se podrá recuperar.

La función de purga se usa para eliminar permanentemente un objeto del almacén de claves o un almacén de claves completo que anteriormente se habían eliminado temporalmente. Como se muestra en la sección anterior, los objetos almacenados en un almacén de claves con la característica de eliminación temporal habilitada pueden pasar por varios estados:
- **Activo**: antes de la eliminación.
- **Eliminado temporalmente**: después de la eliminación, puede mostrarse y recuperarse para volver al estado activo.
- **Eliminado permanentemente**: después de la purga, no se puede recuperar.


Lo mismo ocurre con el almacén de claves. Con el fin de eliminar permanentemente un almacén de claves eliminado temporalmente y su contenido, debe purgar el propio almacén de claves.

### <a name="purging-a-key-vault"></a>Purga de un almacén de claves

Cuando se purga un almacén de claves, todo su contenido, incluidas las claves, los secretos y los certificados, se eliminan definitivamente. Para purgar un almacén de claves eliminado de forma temporal, use el comando `Remove-AzKeyVault` con la opción `-InRemovedState` y especifique la ubicación del almacén de claves eliminado con el argumento `-Location location`. Puede encontrar la ubicación de un almacén eliminado mediante el comando `Get-AzKeyVault -InRemovedState`.

```powershell
Remove-AzKeyVault -VaultName ContosoVault -InRemovedState -Location westus
```

### <a name="purge-permissions-required"></a>Se requieren permisos de purga
- Para purgar un almacén de claves eliminado, el usuario debe tener el permiso de RBAC para la operación *Microsoft.KeyVault/locations/deletedVaults/purge/action*. 
- Para mostrar un almacén de claves eliminado, el usuario necesita el permiso de RBAC para la operación *Microsoft.KeyVault/deletedVaults/read*. 
- Solo un administrador de suscripción tiene estos permisos. 

### <a name="scheduled-purge"></a>Purga programada

La lista de objetos del almacén de claves eliminado también muestra cuándo se han programado para su purga mediante Key Vault. El campo *Scheduled Purge Date* (Fecha de purga programada) indica cuándo se eliminará definitivamente un objeto de almacén de claves si no se realiza ninguna acción. De forma predeterminada, el período de retención para un objeto de almacén de claves eliminado es de 90 días.

>[!IMPORTANT]
>Un objeto de almacén purgado, desencadenado por el valor de su campo de *fecha de purga programada*, se eliminará permanentemente. No se podrá volver a recuperar.

## <a name="other-resources"></a>Otros recursos:

- Para obtener una información general sobre la nueva característica de eliminación temporal, consulte [la información general sobre la eliminación temporal de Azure Key Vault](key-vault-ovw-soft-delete.md).
- Para obtener información general del uso de Azure Key Vault, consulte [¿Qué es Azure Key Vault?](key-vault-overview.md).

