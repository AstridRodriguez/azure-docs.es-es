---
title: Comprobación de las implementaciones de Kubernetes en Azure para la implementación de procedimientos recomendados
description: Más información sobre cómo comprobar la implementación de procedimientos recomendados en las implementaciones en Azure Kubernetes Service mediante kube-advisor.
services: container-service
author: seanmck
ms.service: container-service
ms.topic: troubleshooting
ms.date: 11/05/2018
ms.author: seanmck
ms.openlocfilehash: 01095ac4ed8e362f1a89a53b10b5da6a547feb57
ms.sourcegitcommit: 1b186301dacfe6ad4aa028cfcd2975f35566d756
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/06/2018
ms.locfileid: "51218648"
---
# <a name="checking-for-kubernetes-best-practices-in-your-cluster"></a>Comprobación de los procedimientos recomendados de Kubernetes en el clúster

Hay varios procedimientos recomendados que debería seguir en las implementaciones de Kubernetes para garantizar el mejor rendimiento y resistencia para las aplicaciones. Puede usar la herramienta kube-advisor para buscar las implementaciones que no siguen dichas sugerencias.

## <a name="about-kube-advisor"></a>Acerca de kube-advisor

La [herramienta kube-advisor][kube-advisor-github] es un contenedor único diseñado para ejecutarse en el clúster. Consulta el servidor de API de Kubernetes para obtener información acerca de las implementaciones y devuelve un conjunto de mejoras sugeridas.

> [!NOTE]
> La herramienta kube-advisor es compatible con Microsoft dentro de lo posible. Los problemas y las sugerencias deben presentarse en GitHub.

## <a name="running-kube-advisor"></a>Ejecución de kube-advisor

Para ejecutar la herramienta en un clúster configurado para el [control de acceso basado en roles de Kubernetes (RBAC)](aad-integration.md), deben usarse los siguientes comandos. El primer comando crea una cuenta de servicio de Kubernetes. El segundo, ejecuta la herramienta en un pod con esa cuenta de servicio y configura el pod para su eliminación después de salir. 

```bash
kubectl apply -f https://raw.githubusercontent.com/Azure/kube-advisor/master/sa.yaml

kubectl run --rm -i -t kubeadvisor --image=mcr.microsoft.com/aks/kubeadvisor --restart=Never --overrides="{ \"apiVersion\": \"v1\", \"spec\": { \"serviceAccountName\": \"kube-advisor\" } }"
```

Si no usa RBAC, puede ejecutar el comando como se indica a continuación:

```bash
kubectl run --rm -i -t kubeadvisor --image=mcr.microsoft.com/aks/kubeadvisor --restart=Never
```

En unos pocos segundos, debería ver una tabla donde se describen posibles mejoras para las implementaciones.

![Salida de kube-advisor](media/kube-advisor-tool/kube-advisor-output.png)

## <a name="checks-performed"></a>Comprobaciones realizadas

La herramienta valida varios procedimientos recomendados de Kubernetes, cada uno con su propia sugerencia de corrección.

### <a name="resource-requests-and-limits"></a>Límites y solicitudes de recursos

Kubernetes admite la definición de [límites y solicitudes de recursos en las especificaciones de pod][kube-cpumem]. La solicitud define el nivel mínimo de CPU y memoria necesario para ejecutar el contenedor. El límite define el nivel máximo de CPU y memoria que se debe permitir.

De forma predeterminada, no se establecen límites ni solicitudes en las especificaciones de pod. Esto puede provocar que los nodos se programen en exceso y que se priven los contenedores. La herramienta kube-advisor resalta los pods sin establecer solicitudes ni límites.

## <a name="cleaning-up"></a>Limpiar

Si el clúster tiene habilitado RBAC, puede limpiar `ClusterRoleBinding` después de ejecutar la herramienta con el comando siguiente:

```bash
kubectl delete -f https://raw.githubusercontent.com/Azure/kube-advisor/master/sa.yaml
```

Si ejecuta la herramienta en un clúster que no tenga RBAC habilitado, no se requiere ninguna limpieza.

## <a name="next-steps"></a>Pasos siguientes

- [Solución de problemas de Azure Kubernetes Service](troubleshooting.md)

<!-- RESOURCES -->

[kube-cpumem]: https://github.com/Azure/azure-quickstart-templates
[kube-advisor-github]: https://github.com/azure/kube-advisor