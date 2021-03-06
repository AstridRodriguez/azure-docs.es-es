---
title: Supervisión en Azure Database for MariaDB
description: En este artículo se describen las métricas de supervisión y alertas de Azure Database for MariaDB, incluidas las estadísticas de CPU, almacenamiento y conexión.
author: rachel-msft
ms.author: raagyema
ms.service: mariadb
ms.topic: conceptual
ms.date: 11/10/2018
ms.openlocfilehash: 2ad641ae054f9542ec1ef42f5ebbe724ba4ecf87
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54354032"
---
# <a name="monitoring-in-azure-database-for-mariadb"></a>Supervisión en Azure Database for MariaDB
La supervisión de los datos sobre los servidores le permite solucionar problemas y optimizar la carga de trabajo. Azure Database for MariaDB proporciona diversas métricas que proporcionan información sobre el comportamiento del servidor.

## <a name="metrics"></a>Métricas
Todas las métricas de Azure tienen una frecuencia de un minuto y cada métrica proporciona 30 días de historial. Puede configurar alertas en las métricas. Otras tareas incluyen la configuración de acciones automatizadas, la realización de análisis avanzados y el archivo del historial. Para más información, consulte la [Introducción a las métricas de Azure] (../monitoring-and-diagnostics/monitoring-overview-metrics.md).

Para obtener instrucciones paso a paso, consulte [How to set up alerts](howto-alert-metric.md) (Configuración de alertas).

### <a name="list-of-metrics"></a>Lista de métricas
Estas métricas están disponibles para Azure Database for MariaDB:

|Métrica|Nombre de métrica para mostrar|Unidad|DESCRIPCIÓN|
|---|---|---|---|---|
|cpu_percent|Porcentaje de CPU|Percent|Porcentaje de CPU en uso.|
|memory_percent|Porcentaje de memoria|Percent|Porcentaje de memoria en uso.|
|io_consumption_percent|Porcentaje de E/S|Percent|Porcentaje de E/S en uso.|
|storage_percent|Porcentaje de almacenamiento|Percent|Porcentaje de almacenamiento que se usa más allá del límite máximo del servidor.|
|storage_used|Almacenamiento utilizado|Bytes|Cantidad de almacenamiento en uso. El almacenamiento que usa el servicio puede incluir los archivos de base de datos, los registros de transacciones y los registros de servidor.|
|serverlog_storage_percent|Porcentaje de almacenamiento del registro del servidor|Percent|El porcentaje usado del almacenamiento máximo de registro del servidor.|
|serverlog_storage_usage|Almacenamiento del registro del servidor usado|Bytes|La cantidad de almacenamiento de registro del servidor en uso.|
|serverlog_storage_limit|Límite de almacenamiento del registro del servidor|Bytes|El almacenamiento máximo de registro de este servidor.|
|storage_limit|Límite de almacenamiento|Bytes|Almacenamiento máximo de este servidor.|
|active_connections|Conexiones activas|Recuento|Número de conexiones activas al servidor.|
|connections_failed|Conexiones con errores|Recuento|Número de conexiones con errores al servidor.|
|network_bytes_egress|Red interna|Bytes|Red externa a través de conexiones activas.|
|network_bytes_ingress|Red interna|Bytes|Red interna a través de conexiones activas.|

## <a name="server-logs"></a>Registros del servidor
Puede habilitar el registro de consultas lentas en el servidor. Para más información sobre el registro, visite la página  [Registros de servidor](concepts-server-logs.md).

## <a name="next-steps"></a>Pasos siguientes
- Para obtener más información sobre cómo acceder a las métricas y exportarlas con Azure Portal, la API de REST o la CLI, consulte [Información general sobre las métricas en Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).
 - Consulte [How to set up alerts](howto-alert-metric.md) (Configuración de alertas) para obtener instrucciones sobre cómo crear una alerta en una métrica.
