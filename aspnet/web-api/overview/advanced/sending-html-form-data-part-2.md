---
uid: web-api/overview/advanced/sending-html-form-data-part-2
title: 'Enviar datos de formulario HTML en ASP.NET Web API: carga de archivos y MIME de varias partes | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: a7f3c1b5-69d9-4261-b082-19ffafa5f16a
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/advanced/sending-html-form-data-part-2
msc.type: authoredcontent
ms.openlocfilehash: 7c1c85f462141daf747e23aa4215d47f2d263140
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386713"
---
<a name="sending-html-form-data-in-aspnet-web-api-file-upload-and-multipart-mime"></a>Enviar datos de formulario HTML en ASP.NET Web API: carga de archivos y MIME de varias partes
====================
por [Mike Wasson](https://github.com/MikeWasson)

## <a name="part-2-file-upload-and-multipart-mime"></a>Parte 2: Carga de archivos y MIME de varias partes

Este tutorial muestra cómo cargar archivos a una API web. También se describe cómo procesar los datos de varias partes MIME.

> [!NOTE]
> [Descargue el proyecto completado](https://code.msdn.microsoft.com/ASPNET-Web-API-File-Upload-a8c0fb0d).


Este es un ejemplo de un formulario HTML para cargar un archivo:

[!code-html[Main](sending-html-form-data-part-2/samples/sample1.html)]

![](sending-html-form-data-part-2/_static/image1.png)

Este formulario contiene un control de entrada de texto y un control de entrada de archivo. Cuando un formulario contiene un control de entrada de archivo, el **enctype** atributo debe ser siempre &quot;multipart/datos de formulario&quot;, que especifica que el formulario se enviará como un mensaje MIME de varias partes.

El formato de un mensaje MIME de varias partes es más fácil comprender echando un vistazo a una solicitud de ejemplo:

[!code-console[Main](sending-html-form-data-part-2/samples/sample2.cmd)]

Este mensaje se divide en dos *partes*, uno para cada control de formulario. Los límites de parte se indican mediante las líneas que comienzan con guiones.

> [!NOTE]
> El límite de la parte incluye un componente aleatorio (&quot;41184676334&quot;) para asegurarse de que la cadena delimitadora no accidentalmente aparecen dentro de una parte de mensaje.


Cada parte del mensaje contiene uno o varios encabezados, seguidos por el contenido del elemento.

- El encabezado Content-Disposition incluye el nombre del control. Para los archivos, también contiene el nombre de archivo.
- El encabezado Content-Type describe los datos en la parte. Si se omite este encabezado, el valor predeterminado es texto/sin formato.

En el ejemplo anterior, el usuario ha cargado un archivo llamado GrandCanyon.jpg, con el tipo de contenido de imagen/jpeg; y el valor de la entrada de texto era &quot;vacaciones de verano&quot;.

## <a name="file-upload"></a>Carga de archivos

Ahora Echemos un vistazo a un controlador de Web API que lee los archivos de un mensaje MIME de varias partes. El controlador lee los archivos de forma asincrónica. Web API es compatible con las acciones asincrónicas mediante el [modelo de programación basado en tareas](https://msdn.microsoft.com/library/dd460693.aspx). En primer lugar, este es el código si tiene como destino .NET Framework 4.5, que es compatible con la **async** y **await** palabras clave.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample3.cs)]

Tenga en cuenta que la acción de controlador no toma ningún parámetro. Esto es debido a que se procesa dentro de la acción, el cuerpo de solicitud sin invocar a un formateador de tipo de medio.

El **IsMultipartContent** método comprueba si la solicitud contiene un mensaje MIME de varias partes. Si no es así, el controlador devuelve el código de estado HTTP 415 (tipo de medio no compatible).

El **MultipartFormDataStreamProvider** clase es un objeto auxiliar que asigna las secuencias de archivo para archivos cargados. Para leer el mensaje MIME de varias partes, llame a la **ReadAsMultipartAsync** método. Este método extrae todas las partes del mensaje y los escribe en las secuencias proporcionadas por el **MultipartFormDataStreamProvider**.

Cuando se completa el método, puede obtener información acerca de los archivos desde el **FileData** propiedad, que es una colección de **MultipartFileData** objetos.

- **MultipartFileData.FileName** es el nombre de archivo local en el servidor, donde se guardó el archivo.
- **MultipartFileData.Headers** contiene el encabezado de elemento (*no* el encabezado de solicitud). Se puede usar para tener acceso al contenido\_encabezados Disposition y Content-Type.

Como sugiere su nombre, **ReadAsMultipartAsync** es un método asincrónico. Para realizar el trabajo después de completarse el método, use un [tarea de continuación](https://msdn.microsoft.com/library/ee372288.aspx) (.NET 4.0) o el **await** palabra clave (.NET 4.5).

Esta es la versión de .NET Framework 4.0 del código anterior:

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample4.cs)]

## <a name="reading-form-control-data"></a>Leer datos de Control de formulario

El formulario HTML que mostré anteriormente tenía un control de entrada de texto.

[!code-html[Main](sending-html-form-data-part-2/samples/sample5.html)]

Puede obtener el valor del control desde el **FormData** propiedad de la **MultipartFormDataStreamProvider**.

[!code-csharp[Main](sending-html-form-data-part-2/samples/sample6.cs?highlight=15)]

**FormData** es un **NameValueCollection** que contiene pares nombre/valor para los controles del formulario. La colección puede contener claves duplicadas. Considere este formulario:

[!code-html[Main](sending-html-form-data-part-2/samples/sample7.html)]

![](sending-html-form-data-part-2/_static/image2.png)

El cuerpo de solicitud podría parecerse a esto:

[!code-console[Main](sending-html-form-data-part-2/samples/sample8.cmd)]

En ese caso, el **FormData** colección contendría los siguientes pares de clave/valor:

- ida y vuelta: ida y vuelta
- opciones: la protección sin interrupciones
- opciones: las fechas
- asiento: ventana
