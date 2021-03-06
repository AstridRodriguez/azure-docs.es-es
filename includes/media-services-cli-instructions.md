---
title: archivo de inclusión
description: archivo de inclusión
services: media-services
author: Juliako
ms.service: media-services
ms.topic: include
ms.date: 01/28/2019
ms.author: juliako
ms.custom: include file
ms.openlocfilehash: 06651b06ae84934c16e9f1ac9f604abda8b65615
ms.sourcegitcommit: ba035bfe9fab85dd1e6134a98af1ad7cf6891033
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/01/2019
ms.locfileid: "55648575"
---
## <a name="open-cli-shell"></a>Apertura de un shell de la CLI

Para ejecutar comandos de la CLI, se recomienda usar [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest). **Cloud Shell** es un shell interactivo gratuito que se puede usar para ejecutar los pasos de este artículo. Cloud Shell incluye herramientas comunes de Azure preinstaladas y configuradas para que las use con su cuenta. Ofrece la flexibilidad de poder elegir la experiencia de shell que mejor se adapte a su forma de trabajar. Los usuarios de Linux pueden elegir una experiencia de Bash, mientras que los usuarios de Windows pueden optar por PowerShell.

Si decide instalar y usar la CLI localmente, para este artículo es preciso que ejecute la versión 2.0 o posterior de la CLI de Azure. Ejecute `az --version` para encontrar la versión. Si necesita instalarla o actualizarla, consulte [Instalación de la CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 

### <a name="login"></a>Inicio de sesión

Para empezar a usar el shell de la CLI (tanto en la nube como localmente), ejecute `az login` para crear una conexión con Azure.

Si la CLI puede abrir el explorador predeterminado, lo hará y cargará una página de inicio de sesión. De lo contrario, deberá abrir una página del explorador y seguir las instrucciones de la línea de comandos para especificar un código de autorización después de ir a https://aka.ms/devicelogin en el explorador.

### <a name="specify-location-of-files"></a>Especificación de la ubicación de los archivos

Muchos de los comandos de la CLI de Media Services permiten usar un parámetro con un nombre de archivo. Si usa **Cloud Shell**, puede cargar el archivo para su unidad en la nube (con Bash o PowerShell). 

![Carga de archivos]

Si usa una CLI local o **Cloud Shell**, deberá especificar la ruta de acceso de archivo según el sistema operativo o Cloud Shell (Bash o PowerShell) que esté usando. A continuación se muestran algunos ejemplos:

Ruta de acceso relativa al archivo de metadatos (todos los sistemas operativos)

* `@"mytestfile.json"`
* `@"../mytestfile.json"`

Ruta de acceso absoluta del archivo en el sistema operativo Windows y Linux o Mac

* `@ "/usr/home/mytestfile.json"`
*   `@"c:\tmp\user\mytestfile.json"`


[Carga de archivos]: ./media/media-services-cli/upload-download-files.png
