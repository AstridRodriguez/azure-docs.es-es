---
title: Requisitos del sistema de Microsoft Azure Data Box Edge | Microsoft Docs
description: Obtenga más información sobre los requisitos de software y red de Azure Data Box Edge.
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 02/04/2019
ms.author: alkohli
ms.openlocfilehash: 52d2061262fd04e68ed13aac8932c23b7074f83e
ms.sourcegitcommit: fec0e51a3af74b428d5cc23b6d0835ed0ac1e4d8
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2019
ms.locfileid: "56113777"
---
# <a name="azure-data-box-edge-system-requirements-preview"></a>Requisitos del sistema de Azure Data Box Edge (versión preliminar)

En este artículo se describen los requisitos del sistema importantes de la solución Microsoft Azure Data Box Edge y de los clientes que se conectan a Azure Data Box Edge. Se recomienda que revise detenidamente la información antes de implementar Data Box Edge. Puede consultar esta información según considere necesario durante la implementación y las operaciones posteriores.

Los requisitos del sistema de Data Box Edge incluyen:

- **Requisitos de software para hosts**: describe las plataformas compatibles, los exploradores de la interfaz de usuario de configuración local, los clientes SMB y los requisitos adicionales de los clientes que acceden al dispositivo.
- **Requisitos de red para el dispositivo**: proporciona información acerca de los requisitos de red para el funcionamiento del dispositivo físico.

> [!IMPORTANT]
> Data Box Edge se encuentra en versión preliminar. Revise los [términos de uso para la versión preliminar](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) antes de implementar esta solución.

## <a name="supported-os-for-clients-connected-to-device"></a>Sistema operativo compatible con los clientes conectados al dispositivo

[!INCLUDE [Supported OS for clients connected to device](../../includes/data-box-edge-gateway-supported-client-os.md)]

## <a name="supported-protocols-for-clients-accessing-device"></a>Protocolos compatibles con los clientes que acceden al dispositivo

[!INCLUDE [Supported protocols for clients accessing device](../../includes/data-box-edge-gateway-supported-client-protocols.md)]

## <a name="supported-storage-accounts"></a>Cuentas de almacenamiento admitidas

[!INCLUDE [Supported storage accounts](../../includes/data-box-edge-gateway-supported-storage-accounts.md)]

## <a name="supported-storage-types"></a>Tipos de almacenamiento admitidos

[!INCLUDE [Supported storage types](../../includes/data-box-edge-gateway-supported-storage-types.md)]

## <a name="supported-browsers-for-local-web-ui"></a>Exploradores compatibles con la interfaz de usuario web local

[!INCLUDE [Supported browsers for local web UI](../../includes/data-box-edge-gateway-supported-browsers.md)]

## <a name="networking-port-requirements"></a>Requisitos de los puertos de redes

### <a name="port-requirements-for-data-box-edge"></a>Requisitos de puertos para Data Box Edge

La siguiente tabla enumera los puertos que deben abrirse en el firewall para permitir el tráfico de administración, de la nube o de SMB. En esta tabla, *dentro* o *entrante* hace referencia a la dirección desde la que el cliente entrante solicita acceso al dispositivo. *Fuera* o *saliente* hace referencia a la dirección en la que el dispositivo de Data Box Edge envía datos externamente después de la implementación: por ejemplo, saliente a Internet.

[!INCLUDE [Port configuration for device](../../includes/data-box-edge-gateway-port-config.md)]

### <a name="port-requirements-for-iot-edge"></a>Requisitos de puertos para IoT Edge

Azure IoT Edge permite la comunicación saliente desde un dispositivo Edge local a la nube de Azure mediante protocolos de IoT Hub compatibles. La comunicación entrante solo es necesaria para escenarios específicos donde Azure IoT Hub necesita insertar mensajes en el dispositivo de Azure IoT Edge (por ejemplo, mensajes de la nube al dispositivo).

Utilice la siguiente tabla para la configuración de los puertos de los servidores que hospedan Azure IoT Edge en tiempo de ejecución:

| N.º de puerto | Dentro o fuera | Ámbito de puerto | Obligatorio | Guía |
|----------|-----------|------------|----------|----------|
| TCP 5671 (AMQP)| Fuera       | WAN        | Sí      | Protocolo de comunicación predeterminado de IoT Edge. Debe estar abierto si Azure IoT Edge no está configurado para otros protocolos compatibles o AMQP es el protocolo de comunicación que se desea. <br>5672 para AMQP no es compatible con IoT Edge. <br>Bloquee este puerto cuando Azure IoT Edge use un protocolo compatible de IoT Hub diferente. |
| TCP 443 (HTTPS)| Fuera       | WAN        | Sí      | Abierto para comunicación de salida en el aprovisionamiento de IoT Edge. Si tiene una puerta de enlace transparente con dispositivos de hoja que puedan enviar solicitudes de método. En este caso, no es necesario que el puerto 443 esté abierto a las redes externas para conectarse a IoT Hub ni proporcionar servicios de IoT Hub mediante Azure IoT Edge. Por lo tanto, la regla de entrada podría limitarse solo a los puertos entrantes abiertos desde la red interna. |
| TCP 5671 (AMQP) | En        |            | Sin        | Se deben bloquear las conexiones entrantes.|
| TCP 443 (HTTPS) | En        |            | En algunos casos; consulte los comentarios. | La conexión entrante solo debe ser abierta en escenarios concretos. Si no se pueden configurar los protocolos que no sean HTTP (por ejemplo, AMQP o MQTT), los mensajes pueden enviarse a través de WebSockets mediante el puerto 443. |

Para obtener información completa, vaya a [Reglas de configuración de puertos y firewall para la implementación de IoT Edge](https://docs.microsoft.com/azure/iot-edge/troubleshoot).

## <a name="url-patterns-for-firewall-rules"></a>Patrones de URL para reglas de firewall

Con frecuencia, los administradores de red pueden configurar reglas avanzadas de firewall de acuerdo con los patrones de URL para filtrar el tráfico saliente y entrante. El dispositivo de Data Box Edge y el servicio dependen de otras aplicaciones de Microsoft, como Azure Service Bus, Azure Active Directory Access Control, las cuentas de almacenamiento y los servidores de Microsoft Update. Es posible usar los patrones de URL asociados a estas aplicaciones para configurar las reglas de firewall. Es importante entender que los patrones de URL asociados a estas aplicaciones pueden cambiar. Estos cambios requieren que el administrador de red supervise y actualice las reglas de firewall de Data Box Edge según sea necesario.

Se recomienda que establezca las reglas de firewall para el tráfico saliente, basándose en las direcciones IP fijas de Data Box Edge, de forma generosa en la mayoría de los casos. Sin embargo, puede utilizar la información siguiente con el objetivo de establecer las reglas avanzadas de firewall que se necesitan para crear entornos seguros.

> [!NOTE]
> - Las direcciones IP del dispositivo (origen) siempre se deben establecer en todas las interfaces de red habilitadas para la nube.
> - Las IP de destino, por su parte, se deben establecer en los [intervalos de direcciones IP del centro de datos de Azure](https://www.microsoft.com/download/confirmation.aspx?id=41653).

### <a name="url-patterns-for-gateway-feature"></a>Patrones de dirección URL para la característica de puerta de enlace

[!INCLUDE [URL patterns for firewall](../../includes/data-box-edge-gateway-url-patterns-firewall.md)]

### <a name="url-patterns-for-compute-feature"></a>Patrones de dirección URL para la característica de proceso

| Patrón de URL                      | Componente o funcionalidad                     |   |
|----------------------------------|---------------------------------------------|---|
| `https://mcr.microsoft.com`<br></br>https://\*.cdn.mscr.io | Registro de contenedor de Microsoft (obligatorio)               |   |
| https://\*.azurecr.io                     | Registros de contenedores personales y de terceros (opcional) |   |
| https://\*.azure-devices.net              | Acceso de IoT Hub (obligatorio)                             |   |

## <a name="internet-bandwidth"></a>Ancho de banda de Internet

[!INCLUDE [Internet bandwidth](../../includes/data-box-edge-gateway-internet-bandwidth.md)]

## <a name="next-step"></a>Paso siguiente

- [Implementación de Azure Data Box Edge](data-box-Edge-deploy-prep.md)
