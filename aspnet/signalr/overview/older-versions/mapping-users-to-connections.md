---
uid: signalr/overview/older-versions/mapping-users-to-connections
title: Asignar usuarios de SignalR a las conexiones en SignalR 1.x | Microsoft Docs
author: pfletcher
description: En este tema se muestra cómo conservar la información acerca de los usuarios y sus conexiones.
ms.author: riande
ms.date: 10/17/2013
ms.assetid: ebbc93a8-e6c4-4122-8e0d-3aa42293c747
msc.legacyurl: /signalr/overview/older-versions/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: ba7bbea25cd35f8ba23901c5f8db08404c91bd52
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287493"
---
<a name="mapping-signalr-users-to-connections-in-signalr-1x"></a>Asignar usuarios de SignalR a las conexiones en SignalR 1.x
====================
por [Patrick Fletcher](https://github.com/pfletcher), [Tom FitzMacken](https://github.com/tfitzmac)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> En este tema se muestra cómo conservar la información acerca de los usuarios y sus conexiones.


## <a name="introduction"></a>Introducción

Cada cliente que se conecta a un concentrador pasa un identificador único de conexión. Puede recuperar este valor en el `Context.ConnectionId` propiedad del contexto del concentrador. Si la aplicación necesita asignar un usuario para el identificador de conexión y almacenar esa asignación, puede usar uno de los siguientes:

- [Almacenamiento en memoria](#inmemory), por ejemplo, un diccionario
- [Grupo de SignalR para cada usuario](#groups)
- [Almacenamiento externo, permanente](#database), como una tabla de base de datos o almacenamiento de tablas de Azure

En este tema se muestra cada una de estas implementaciones. Usa el `OnConnected`, `OnDisconnected`, y `OnReconnected` métodos de la `Hub` clase para realizar un seguimiento del estado de conexión de usuario.

El mejor enfoque para la aplicación depende de:

- El número de servidores web que hospeda la aplicación.
- Si necesita obtener una lista de los usuarios conectados actualmente.
- Si necesita conservar información de usuario y grupo cuando se reinicia la aplicación o el servidor.
- Si la latencia de la llamada a un servidor externo es un problema.

La siguiente tabla muestra qué enfoque funciona para estas consideraciones.

|  | Más de un servidor | Obtiene una lista de los usuarios conectados actualmente | Conservar la información una vez reiniciado | Obtener un rendimiento óptimo |
| --- | --- | --- | --- | --- |
| En memoria |  | ![](mapping-users-to-connections/_static/image1.png) |  | ![](mapping-users-to-connections/_static/image2.png) |
| Grupos de usuario único | ![](mapping-users-to-connections/_static/image3.png) |  |  | ![](mapping-users-to-connections/_static/image4.png) |
| Permanente, externa | ![](mapping-users-to-connections/_static/image5.png) | ![](mapping-users-to-connections/_static/image6.png) | ![](mapping-users-to-connections/_static/image7.png) |  |

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Almacenamiento en memoria

Los ejemplos siguientes muestran cómo conservar la información de conexión y el usuario en un diccionario que se almacena en memoria. Usa el diccionario un `HashSet` para almacenar el identificador de conexión. En cualquier momento, un usuario podría tener más de una conexión a la aplicación de SignalR. Por ejemplo, un usuario que está conectado a través de varios dispositivos o más de una pestaña del explorador tendría más de un identificador de conexión.

Si cierra la aplicación, se pierde toda la información, pero se vuelve a lo rellenará los usuarios restablecer sus conexiones. Almacenamiento en memoria no funciona si su entorno incluye más de un servidor web, ya que cada servidor tendría una colección independiente de conexiones.

El primer ejemplo muestra una clase que administra la asignación de usuarios a las conexiones. La clave para el objeto HashSet será el nombre del usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

En el ejemplo siguiente se muestra cómo usar la clase de asignación de conexión de un concentrador. La instancia de la clase se almacena en un nombre de variable `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Grupos de usuario único

Puede crear un grupo para cada usuario y, a continuación, enviar un mensaje a ese grupo cuando desea llegar a solo ese usuario. El nombre de cada grupo es el nombre del usuario. Si un usuario tiene más de una conexión, se agrega cada identificador de conexión para el grupo del usuario.

Se debería no quitar manualmente el usuario del grupo cuando el usuario se desconecta. Esta acción se realiza automáticamente el marco de trabajo de SignalR.

El ejemplo siguiente muestra cómo implementar grupos de usuario único.

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Almacenamiento externo, permanente

En este tema se muestra cómo usar una base de datos o el almacenamiento de tablas de Azure para almacenar información de conexión. Este enfoque funciona cuando tiene varios servidores web, ya que cada servidor web puede interactuar con el mismo repositorio de datos. Si los servidores web deja de funcionar o se reinicie la aplicación, el `OnDisconnected` no se llama al método. Por lo tanto, es posible que el repositorio de datos tendrá los registros para los identificadores de conexión que ya no son válidos. Para limpiar estos registros huérfanos, puede que desee invalidar cualquier conexión que se creó fuera de un período de tiempo que sea relevante para la aplicación. Los ejemplos en esta sección incluyen un valor para el seguimiento cuando se creó la conexión, pero no se muestra cómo limpiar los registros antiguos, porque es posible que desee hacerlo como proceso en segundo plano.

### <a name="database"></a>Base de datos

Los ejemplos siguientes muestran cómo conservar la información de conexión y el usuario en una base de datos. Puede usar cualquier tecnología de acceso a datos; Sin embargo, en el ejemplo siguiente se muestra cómo definir los modelos que usan Entity Framework. Estos modelos de entidad se corresponden con los campos y tablas de base de datos. La estructura de datos puede variar considerablemente según los requisitos de la aplicación.

El primer ejemplo muestra cómo definir una entidad de usuario que se puede asociar con muchas entidades de la conexión.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

A continuación, desde el concentrador, puede seguir el estado de cada conexión con el código que se muestra a continuación.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

### <a name="azure-table-storage"></a>Almacenamiento de tablas de Azure

En el siguiente ejemplo de almacenamiento de tablas de Azure es similar a la base de datos ejemplo. No incluir toda la información que necesita para empezar a trabajar con el servicio de Azure Table Storage. Para obtener información, consulte [cómo usar Table storage en .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

El ejemplo siguiente muestra una entidad de tabla para almacenar información de conexión. Divide los datos por nombre de usuario y que identifica cada entidad con el identificador de la conexión, por lo que un usuario puede tener varias conexiones en cualquier momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

En el centro, realizar un seguimiento del estado de conexión de cada usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]
