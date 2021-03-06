---
title: Uso de plantillas de Azure Resource Manager en Azure Stack | Microsoft Docs
description: Obtenga información sobre cómo usar plantillas de Azure Resource Manager en Azure Stack para aprovisionar recursos.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2018
ms.author: sethm
ms.reviewer: justini
ms.lastreviewed: 11/14/2018
ms.openlocfilehash: 408be7ad9aa371e749a225ef85be42d002293c0d
ms.sourcegitcommit: 70471c4febc7835e643207420e515b6436235d29
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/15/2019
ms.locfileid: "54302602"
---
# <a name="use-azure-resource-manager-templates-in-azure-stack"></a>Uso de plantillas de Administrador de recursos de Azure en Azure Stack

*Se aplica a: Sistemas integrados de Azure Stack y Kit de desarrollo de Azure Stack*

Puede utilizar las plantillas de Azure Resource Manager para implementar y aprovisionar todos los recursos para su aplicación en una única operación coordinada. También se pueden volver a implementar plantillas para realizar cambios en los recursos de un grupo de recursos.

Estas plantillas se pueden implementar con el portal de Microsoft Azure Stack, PowerShell, con la línea de comandos y Visual Studio.

Las plantillas de inicio rápido siguientes están disponibles en [GitHub](https://aka.ms/azurestackgithub):

## <a name="deploy-sharepoint-server-non-high-availability-deployment"></a>Implementación de SharePoint Server(sin implementación de alta disponibilidad)

Use la extensión DSC de PowerShell para [crear una granja de SharePoint Server 2013](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha) que incluya los recursos siguientes:

* Una red virtual
* Tres cuentas de almacenamiento
* Dos equilibradores de carga externos
* Una máquina virtual (VM) configurada como controlador de dominio para un nuevo bosque con un único dominio
* Una máquina virtual configurada como un servidor independiente de SQL Server 2014
* Una máquina virtual configurada como granja de SharePoint Server 2013 de una máquina

## <a name="deploy-ad-non-high-availability-deployment"></a>Implementación de AD (sin alta disponibilidad)

Use la extensión DSC de PowerShell para [crear un servidor de controlador de dominio de AD](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha) que incluya los recursos siguientes:

* Una red virtual
* Una cuenta de almacenamiento
* Un equilibrador de carga externo
* Una máquina virtual (VM) configurada como controlador de dominio para un nuevo bosque con un único dominio

## <a name="deploy-adsql-non-high-availability-deployment"></a>Implementación de AD/SQL (sin alta disponibilidad)

Use la extensión DSC de PowerShell para [crear un servidor independiente de SQL Server 2014](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha) que incluya los recursos siguientes:

* Una red virtual
* Dos cuentas de almacenamiento
* Un equilibrador de carga externo
* Una máquina virtual (VM) configurada como controlador de dominio para un nuevo bosque con un único dominio
* Una máquina virtual configurada como un servidor independiente de SQL Server 2014

## <a name="vm-dsc-extension-azure-automation-pull-server"></a>Extensión DSC de máquina virtual para un servidor de extracción de Azure Automation

Utilice la extensión de DSC de PowerShell para configurar Administrador de configuración Local (LCM) de una máquina virtual existente y registrarlo en un servidor de extracción de DSC de la cuenta de Azure Automation.

## <a name="create-a-virtual-machine-from-a-user-image"></a>Creación de una máquina virtual desde una imagen de usuario

[Cree una máquina virtual desde una imagen de usuario personalizada](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-vm-from-user-image). Esta plantilla también implementa una red virtual (con DNS), una dirección IP pública y una interfaz de red.

## <a name="basic-virtual-machine"></a>Máquina virtual básica

[Implemente una máquina virtual Windows](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm) que incluya una red virtual (con DNS), una dirección IP pública y una interfaz de red.

## <a name="cancel-a-running-template-deployment"></a>Cancelación de una implementación de la plantilla en ejecución

Para cancelar la implementación de una plantilla en ejecución, use el cmdlet [Stop-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/stop-azurermresourcegroupdeployment) de PowerShell.

## <a name="next-steps"></a>Pasos siguientes

* [Implementación de plantillas con el portal](azure-stack-deploy-template-portal.md)
* [Información general sobre Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md)
