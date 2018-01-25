---
uid: signalr/overview/guide-to-the-api/mapping-users-to-connections
title: Asignar usuarios de SignalR a conexiones | Documentos de Microsoft
author: tfitzmac
description: "En este tema se muestra cómo conservar la información sobre los usuarios y sus conexiones. Patrick Fletcher ayudó a escribir en este tema. Versiones de software que se usa en este tema..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/30/2014
ms.topic: article
ms.assetid: f80c08b1-3f1f-432c-980c-c7b6edeb31b1
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/mapping-users-to-connections
msc.type: authoredcontent
ms.openlocfilehash: c4f95a3b65c57dd7cb7c5c7f1ee09daa17fa9616
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="mapping-signalr-users-to-connections"></a>Asignar usuarios a las conexiones de SignalR
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este tema se muestra cómo conservar la información sobre los usuarios y sus conexiones.
> 
> Patrick Fletcher ayudó a escribir en este tema.
> 
> ## <a name="software-versions-used-in-this-topic"></a>Versiones de software que se usa en este tema
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR versión 2
>   
> 
> 
> ## <a name="previous-versions-of-this-topic"></a>Versiones anteriores de este tema
> 
> Para obtener información acerca de las versiones anteriores de SignalR, consulte [versiones anteriores de SignalR](../older-versions/index.md).
> 
> ## <a name="questions-and-comments"></a>Preguntas y comentarios
> 
> Vota sobre cómo le gustó este tutorial y lo que podemos mejorar en los comentarios en la parte inferior de la página. Si tiene preguntas que no están directamente relacionados con el tutorial, puede publicar para la [foro de ASP.NET SignalR](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR) o [StackOverflow.com](http://stackoverflow.com/).


## <a name="introduction"></a>Introducción

Cada cliente que se conecta a un concentrador pasa un identificador único de conexión. Puede recuperar este valor en el `Context.ConnectionId` propiedad del contexto de base de datos central. Si la aplicación necesita asignar un usuario para el identificador de conexión y conservar esa asignación, puede usar uno de los siguientes:

- [El proveedor de Id. de usuario (SignalR 2)](#IUserIdProvider)
- [Almacenamiento en memoria](#inmemory), por ejemplo, un diccionario
- [Grupo de SignalR para cada usuario](#groups)
- [Almacenamiento externo, permanente](#database), como una tabla de base de datos o almacenamiento de tabla de Azure

En este tema se muestra cada una de estas implementaciones. Usa el `OnConnected`, `OnDisconnected`, y `OnReconnected` métodos de la `Hub` clase para realizar un seguimiento del estado de conexión de usuario.

El mejor método para la aplicación depende de:

- El número de servidores web que hospeda la aplicación.
- Si tiene que obtener una lista de los usuarios conectados actualmente.
- Si necesita conservar información de usuario y de grupo cuando se reinicia la aplicación o el servidor.
- Si la latencia de la llamada a un servidor externo es un problema.

La siguiente tabla muestra qué enfoque funciona en estas consideraciones.

|  | Más de un servidor | Obtener lista de usuarios conectados actualmente | Conservar la información una vez reiniciado | Obtener un rendimiento óptimo |
| --- | --- | --- | --- | --- |
| Proveedor de identificador de usuario | ![](mapping-users-to-connections/_static/image1.png) |  |  | ![](mapping-users-to-connections/_static/image2.png) |
| En memoria |  | ![](mapping-users-to-connections/_static/image3.png) |  | ![](mapping-users-to-connections/_static/image4.png) |
| Grupos de usuario único | ![](mapping-users-to-connections/_static/image5.png) |  |  | ![](mapping-users-to-connections/_static/image6.png) |
| Permanente, externo | ![](mapping-users-to-connections/_static/image7.png) | ![](mapping-users-to-connections/_static/image8.png) | ![](mapping-users-to-connections/_static/image9.png) |  |

<a id="IUserIdProvider"></a>

## <a name="iuserid-provider"></a>Proveedor de IUserID

Esta característica permite a los usuarios especificar ¿qué es el identificador de usuario en función de un objeto IRequest a través de una nueva interfaz IUserIdProvider.

**El IUserIdProvider**

[!code-csharp[Main](mapping-users-to-connections/samples/sample1.cs)]

De forma predeterminada, habrá una implementación que usa el usuario `IPrincipal.Identity.Name` como el nombre de usuario. Para cambiar esta configuración, registrar su implementación de `IUserIdProvider` con el host global cuando se inicia la aplicación:

[!code-csharp[Main](mapping-users-to-connections/samples/sample2.cs)]

Desde dentro de un concentrador, podrá enviar mensajes a estos usuarios a través de la siguiente API:

**Enviar un mensaje a un usuario específico**

[!code-csharp[Main](mapping-users-to-connections/samples/sample3.cs?highlight=5)]

<a id="inmemory"></a>

## <a name="in-memory-storage"></a>Almacenamiento en memoria

Los ejemplos siguientes muestran cómo conservar la información de conexión y el usuario en un diccionario que se almacena en memoria. El diccionario usa un `HashSet` para almacenar el identificador de conexión. En cualquier momento, un usuario podría tener más de una conexión a la aplicación de SignalR. Por ejemplo, un usuario que está conectado a través de varios dispositivos o más de una pestaña de explorador tendría más de un identificador de conexión.

Si la aplicación se cierra, se pierde toda la información, pero se vuelve a rellenará como los usuarios restablecer sus conexiones. Almacenamiento en memoria no funciona si su entorno incluye más de un servidor web, ya que cada servidor tendría un conjunto diferente de las conexiones.

El primer ejemplo muestra una clase que administra la asignación de usuarios a las conexiones. La clave para el objeto HashSet será el nombre del usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample4.cs)]

En el ejemplo siguiente se muestra cómo utilizar la clase de asignación de conexión de un concentrador. La instancia de la clase se almacena en un nombre de variable `_connections`.

[!code-csharp[Main](mapping-users-to-connections/samples/sample5.cs)]

<a id="groups"></a>

## <a name="single-user-groups"></a>Grupos de usuario único

Puede crear un grupo para cada usuario y, a continuación, enviar un mensaje a ese grupo cuando desee alcanzar sólo ese usuario. El nombre de cada grupo es el nombre del usuario. Si un usuario tiene más de una conexión, cada identificador de conexión se agrega al grupo del usuario.

Debes no quitar manualmente el usuario del grupo cuando el usuario se desconecta. Esta acción se realiza automáticamente el marco de trabajo de SignalR.

En el ejemplo siguiente se muestra cómo implementar grupos de usuario único.

[!code-csharp[Main](mapping-users-to-connections/samples/sample6.cs)]

<a id="database"></a>

## <a name="permanent-external-storage"></a>Almacenamiento externo, permanente

Este tema muestra cómo utilizar una base de datos o el almacenamiento de tabla de Azure para almacenar información de conexión. Este enfoque funciona cuando tiene varios servidores web, puesto que todos los servidores web pueden interactuar con el mismo repositorio de datos. Si los servidores web de detener el trabajo o se reinicie la aplicación, la `OnDisconnected` no se llama el método. Por lo tanto, es posible que el repositorio de datos tengan registros para los identificadores de conexión que ya no son válidos. Para limpiar estos registros huérfanos, puede que desee invalidar cualquier conexión que se creó fuera de un período de tiempo que es relevante para la aplicación. Los ejemplos de esta sección incluyen un valor para el seguimiento cuando se creó la conexión, pero no se muestra cómo limpiar los registros antiguos, porque puede que desee hacerlo como proceso en segundo plano.

### <a name="database"></a>Base de datos

Los ejemplos siguientes muestran cómo conservar la información de conexión y el usuario en una base de datos. Puede usar cualquier tecnología de acceso a datos; Sin embargo, en el ejemplo siguiente muestra cómo definir los modelos que usan Entity Framework. Estos modelos de entidad corresponden a los campos y tablas de base de datos. La estructura de datos puede variar considerablemente según los requisitos de la aplicación.

El primer ejemplo muestra cómo definir una entidad de usuario que se puede asociar varias entidades de conexión.

[!code-csharp[Main](mapping-users-to-connections/samples/sample7.cs)]

A continuación, desde el concentrador, puede controlar el estado de cada conexión con el código que se muestra a continuación.

[!code-csharp[Main](mapping-users-to-connections/samples/sample8.cs)]

<a id="azure"></a>
### <a name="azure-table-storage"></a>Almacenamiento de tabla de Azure

En el siguiente ejemplo de almacenamiento de tabla de Azure es similar al ejemplo de la base de datos. No incluir toda la información que necesita para empezar a trabajar con el servicio de almacenamiento de tabla de Azure. Para obtener información, consulte [Introducción al almacenamiento de tabla de .NET](https://azure.microsoft.com/documentation/articles/storage-dotnet-how-to-use-tables/).

En el ejemplo siguiente se muestra una entidad de tabla para almacenar información de conexión. Particiones de los datos por nombre de usuario y que identifica cada entidad por el identificador de conexión, por lo que un usuario puede tener varias conexiones en cualquier momento.

[!code-csharp[Main](mapping-users-to-connections/samples/sample9.cs)]

En el concentrador, realizar un seguimiento del estado de conexión de cada usuario.

[!code-csharp[Main](mapping-users-to-connections/samples/sample10.cs)]
