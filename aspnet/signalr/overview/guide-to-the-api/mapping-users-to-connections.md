---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Asignar usuarios de SignalR a las conexiones | Microsoft Docs
author: tfitzmac
description: En este tema se muestra cómo conservar la información acerca de los usuarios y sus conexiones. Patrick Fletcher ayudó a escribir en este tema. Versiones de software que se usa en este tema...
ms.author: riande
ms.date: 12/30/2014
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: a3598eee30b29a95673cbc313adfb8f5b068ea24
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912584"
---
<a name="mapping-signalr-users-to-connections"></a>Asignar usuarios de SignalR a las conexiones
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tema se muestra cómo conservar la información acerca de los usuarios y sus conexiones.
>
> Patrick Fletcher ayudó a escribir en este tema.
>
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software que se usa en este tema
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - Versión 2 de SignalR
>
>
>
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema.
>
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
>
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
>
> Deje comentarios sobre cómo le gustó de este tutorial y que podíamos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicarlos en el [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="introduction"></a>Introducción

Cada cliente que se conecta a un concentrador pasa un identificador único de conexión. Puede recuperar este valor en el `Context.ConnectionId` propiedad del contexto del concentrador. Si la aplicación necesita asignar un usuario para el identificador de conexión y almacenar esa asignación, puede usar uno de los siguientes:

- [El proveedor de Id. de usuario (SignalR 2)](#IUserIdProvider)
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
| Proveedor de identificador de usuario | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| En memoria |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Grupos de usuario único | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanente, externa | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Proveedor IUserID

Esta característica permite a los usuarios especificar lo que es el identificador de usuario en función de un objeto IRequest a través de una nueva interfaz IUserIdProvider.

**El IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

De forma predeterminada, será una implementación que usa el usuario `IPrincipal.Identity.Name` como el nombre de usuario. Para cambiar esto, registrar su implementación de `IUserIdProvider` con el host global cuando se inicia la aplicación:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Desde dentro de un concentrador, podrá enviar mensajes a estos usuarios a través de la siguiente API:

**Enviar un mensaje a un usuario específico**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Almacenamiento en memoria

Los ejemplos siguientes muestran cómo conservar la información de conexión y el usuario en un diccionario que se almacena en memoria. Usa el diccionario un `HashSet` para almacenar el identificador de conexión. En cualquier momento, un usuario podría tener más de una conexión a la aplicación de SignalR. Por ejemplo, un usuario que está conectado a través de varios dispositivos o más de una pestaña del explorador tendría más de un identificador de conexión.

Si cierra la aplicación, se pierde toda la información, pero se vuelve a lo rellenará los usuarios restablecer sus conexiones. Almacenamiento en memoria no funciona si su entorno incluye más de un servidor web, ya que cada servidor tendría una colección independiente de conexiones.

El primer ejemplo muestra una clase que administra la asignación de usuarios a las conexiones. La clave para el objeto HashSet será el nombre del usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

En el ejemplo siguiente se muestra cómo usar la clase de asignación de conexión de un concentrador. La instancia de la clase se almacena en un nombre de variable `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Grupos de usuario único

Puede crear un grupo para cada usuario y, a continuación, enviar un mensaje a ese grupo cuando desea llegar a solo ese usuario. El nombre de cada grupo es el nombre del usuario. Si un usuario tiene más de una conexión, se agrega cada identificador de conexión para el grupo del usuario.

Se debería no quitar manualmente el usuario del grupo cuando el usuario se desconecta. Esta acción se realiza automáticamente el marco de trabajo de SignalR.

El ejemplo siguiente muestra cómo implementar grupos de usuario único.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Almacenamiento externo, permanente

En este tema se muestra cómo usar una base de datos o el almacenamiento de tablas de Azure para almacenar información de conexión. Este enfoque funciona cuando tiene varios servidores web, ya que cada servidor web puede interactuar con el mismo repositorio de datos. Si los servidores web deja de funcionar o se reinicie la aplicación, el `OnDisconnected` no se llama al método. Por lo tanto, es posible que el repositorio de datos tendrá los registros para los identificadores de conexión que ya no son válidos. Para limpiar estos registros huérfanos, puede que desee invalidar cualquier conexión que se creó fuera de un período de tiempo que sea relevante para la aplicación. Los ejemplos en esta sección incluyen un valor para el seguimiento cuando se creó la conexión, pero no se muestra cómo limpiar los registros antiguos, porque es posible que desee hacerlo como proceso en segundo plano.

### <a name="database"></a>Base de datos

Los ejemplos siguientes muestran cómo conservar la información de conexión y el usuario en una base de datos. Puede usar cualquier tecnología de acceso a datos; Sin embargo, en el ejemplo siguiente se muestra cómo definir los modelos que usan Entity Framework. Estos modelos de entidad se corresponden con los campos y tablas de base de datos. La estructura de datos puede variar considerablemente según los requisitos de la aplicación.

El primer ejemplo muestra cómo definir una entidad de usuario que se puede asociar con muchas entidades de la conexión.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

A continuación, desde el concentrador, puede seguir el estado de cada conexión con el código que se muestra a continuación.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Almacenamiento de tablas de Azure

En el siguiente ejemplo de almacenamiento de tablas de Azure es similar a la base de datos ejemplo. No incluir toda la información que necesita para empezar a trabajar con el servicio de Azure Table Storage. Para obtener información, consulte [cómo usar Table storage en .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

El ejemplo siguiente muestra una entidad de tabla para almacenar información de conexión. Divide los datos por nombre de usuario y que identifica cada entidad con el identificador de la conexión, por lo que un usuario puede tener varias conexiones en cualquier momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

En el centro, realizar un seguimiento del estado de conexión de cada usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
