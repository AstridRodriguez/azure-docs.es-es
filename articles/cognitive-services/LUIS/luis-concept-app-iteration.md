---
title: Diseño de aplicaciones iterativo
titleSuffix: Language Understanding - Azure Cognitive Services
description: LUIS aprende mejor en un ciclo iterativo de cambios en el modelo, ejemplos de expresiones, publicación y recopilación de datos de las consultas de punto de conexión.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: diberry
ms.openlocfilehash: 6f8ec10222ae58af9e39d2d88f13f355f9c9baa6
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2019
ms.locfileid: "55861143"
---
# <a name="authoring-cycle-for-your-luis-app"></a>Ciclo de creación de una aplicación de LUIS
LUIS aprende mejor en un ciclo iterativo de cambios en el modelo, ejemplos de expresiones, publicación y recopilación de datos de las consultas de punto de conexión. 

![Ciclo de creación](./media/luis-concept-app-iteration/iteration.png)

## <a name="building-a-luis-model"></a>Generar un modelo de LUIS
El propósito del modelo es averiguar qué pide el usuario (la intención) y qué partes de la pregunta aportan datos (entidades) que permitan determinar la respuesta. 

El modelo debe ser específico del dominio de aplicación para poder determinar palabras y frases que sean relevantes, así como el orden habitual de las palabras. 

El modelo incluye intenciones y entidades. 

## <a name="add-training-examples"></a>Agregar ejemplos de entrenamiento
LUIS necesita expresiones de ejemplo en las intenciones. Los ejemplos necesitan una variación amplia de palabras y del orden de estas para poder determinar qué intención tiene la expresión. Cada expresión de ejemplo debe tener los datos necesarios etiquetados como entidades. 

Debe indicar a LUIS que ignore las expresiones que no sean pertinentes para el dominio de la aplicación. Para ello, debe asignar la expresión a la intención **None** (Ninguno). No es necesario etiquetar todas las palabras o frases que no necesite extraer de una expresión. No hay ninguna etiqueta para las palabras o frases que se vayan a ignorar. 

## <a name="train-and-publish-the-app"></a>Entrenamiento y publicación de la aplicación
Una vez que tenga entre 10 y 15 expresiones diferentes en cada intención, con las entidades necesarias etiquetadas, debe entrenar y efectuar la publicación. Desde la notificación de éxito de publicación, use el vínculo para obtener los puntos de conexión. Asegúrese de crear y publicar la aplicación de manera que esté disponible en las [regiones de punto de conexión](luis-reference-regions.md) que necesite. 

## <a name="https-endpoint-testing"></a>Pruebas de puntos de conexión HTTPS
Puede probar la aplicación de LUIS desde el punto de conexión HTTPS. La realización de pruebas desde el punto de conexión permite a LUIS elegir cualquier expresión con una confianza baja para la revisión.  

## <a name="recycle"></a>Reciclar
Cuando haya terminado un ciclo de creación, puede empezar de nuevo. Empiece revisando las expresiones de punto de conexión que LUIS ha marcado con una confianza baja. Compruebe la intención y la entidad de estas expresiones. Una vez revisadas las expresiones, la lista de revisión debería estar vacía.  

## <a name="batch-testing"></a>Pruebas por lotes
Las pruebas por lotes son una manera de ver cuántas expresiones de ejemplo puntúa LUIS. Los ejemplos deben ser nuevos para LUIS y deben estar correctamente etiquetados con la intención y las entidades que quiere que busque LUIS. Los resultados de la prueba indican el grado de rendimiento que tendría LUIS en ese conjunto de expresiones. 

## <a name="next-steps"></a>Pasos siguientes

Obtenga información sobre conceptos relativos a la [colaboración](luis-concept-collaborator.md).
