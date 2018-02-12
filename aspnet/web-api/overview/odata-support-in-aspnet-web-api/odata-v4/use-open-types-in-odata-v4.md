---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Abrir tipos de OData v4 con ASP.NET Web API | Documentos de Microsoft
author: microsoft
description: "En OData v4, un tipo abierto es un tipo de stuctured que contiene las propiedades dinámicas, además de todas las propiedades que se declaran en la definición de tipo. Abrir..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2014
ms.topic: article
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: fe67b9a11a82b55d5f3e0e5f1b0cee10a58833d2
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/12/2018
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a>Abrir tipos de OData v4 con ASP.NET Web API
====================
por [Microsoft](https://github.com/microsoft)

> En OData v4, un *abrir tipo* es un tipo de stuctured que contiene las propiedades dinámicas, además de todas las propiedades que se declaran en la definición de tipo. Tipos abiertos, podrá agregar flexibilidad a los modelos de datos. Este tutorial muestra cómo utilizar tipos abiertos en ASP.NET Web API OData.
> 
> Este tutorial se da por supuesto que ya sabe cómo crear un extremo de OData en ASP.NET Web API. Si no es así, empiece por leer [crear un punto de conexión de OData v4](create-an-odata-v4-endpoint.md) primero.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Web API OData 5.3
> - OData v4


En primer lugar, la terminología OData:

- Tipo de entidad: un tipo estructurado con una clave.
- Tipo complejo: un tipo estructurado sin una clave.
- Tipo Open: un tipo con propiedades dinámicas. Tipos de entidad y tipos complejos pueden estar abiertos.

El valor de una propiedad dinámica que puede ser un tipo primitivo, un tipo complejo o un tipo de enumeración; o una colección de cualquiera de esos tipos. Para obtener más información acerca de tipos abiertos, vea el [especificación de OData v4](http://www.odata.org/documentation/odata-version-4-0/).

## <a name="install-the-web-odata-libraries"></a>Instalar las bibliotecas de OData de Web

Use el Administrador de paquetes de NuGet para instalar las bibliotecas de OData de Web API más recientes. Desde la ventana de la consola de administrador de paquetes:

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a>Definir los tipos de CLR

Empiece por definir los modelos EDM como tipos CLR.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

Cuando se crea el Entity Data Model (EDM),

- `Category`es un tipo de enumeración.
- `Address`es un tipo complejo. (No es necesario una clave, por lo que no es un tipo de entidad.)
- `Customer`es un tipo de entidad. (Tiene una clave).
- `Press`es un tipo complejo abierto.
- `Book`es un tipo de entidad abierto.

Para crear un tipo abierto, el tipo CLR debe tener una propiedad de tipo `IDictionary<string, object>`, que contiene las propiedades dinámicas.

## <a name="build-the-edm-model"></a>Generar el modelo EDM

Si usa **ODataConventionModelBuilder** para crear el EDM, `Press` y `Book` se agregan automáticamente como tipos abiertos, en función de la presencia de un `IDictionary<string, object>` propiedad.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

También puede compilar EDM explícitamente, mediante **ODataModelBuilder**.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a>Agregar un controlador de OData

A continuación, agregue un controlador de OData. Para este tutorial, vamos a usar un controlador de simplificada que solo admite GET y POST solicita y usa una lista en memoria para almacenar las entidades.

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

Tenga en cuenta que la primera `Book` instancia no tiene ninguna propiedad dinámica. El segundo `Book` instancia tiene las siguientes propiedades dinámicas:

- "Publicar": tipo primitivo
- "Los autores": colección de tipos primitivos
- "OtherCategories": colección de tipos de enumeración.

Además, el `Press` propiedad de ese `Book` instancia tiene las siguientes propiedades dinámicas:

- "Blog": tipo primitivo
- "Address": tipo complejo

## <a name="query-the-metadata"></a>Consultar los metadatos

Para obtener el documento de metadatos de OData, envía una solicitud GET a `~/$metadata`. El cuerpo de respuesta debe ser similar al siguiente:

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

En el documento de metadatos, puede ver:

- Para el `Book` y `Press` tipos, el valor de la `OpenType` atributo es true. El `Customer` y `Address` tipos no tienen este atributo.
- El `Book` tipo de entidad tiene tres propiedades declaradas: ISBN, título y presione. Los metadatos de OData no incluyen el `Book.Properties` propiedad de la clase CLR.
- De forma similar, la `Press` tipo complejo tiene solo dos propiedades declaradas: nombre y la categoría. Los metadatos no incluyen el `Press.DynamicProperties` propiedad de la clase CLR.

## <a name="query-an-entity"></a>Consultar una entidad

Para obtener la libreta con ISBN igual a "978-0-7356-7942-9", envía una solicitud GET a `~/Books('978-0-7356-7942-9')`. El cuerpo de respuesta debe ser similar al siguiente. (Con sangría para que sea más legible.)

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

Tenga en cuenta que las propiedades dinámicas están incluidas alineadas con las propiedades declaradas.

## <a name="post-an-entity"></a>REGISTRAR una entidad

Para agregar una entidad del libro, envía una solicitud POST para `~/Books`. El cliente puede establecer propiedades dinámicas en la carga de solicitud.

Esta es una solicitud de ejemplo. Tenga en cuenta las propiedades "Price" y "Publicada".

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

Si establece un punto de interrupción en el método de controlador, puede ver que la API Web agregado estas propiedades para el `Properties` diccionario.

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a>Recursos adicionales

[Ejemplo de tipo abierto de OData](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
