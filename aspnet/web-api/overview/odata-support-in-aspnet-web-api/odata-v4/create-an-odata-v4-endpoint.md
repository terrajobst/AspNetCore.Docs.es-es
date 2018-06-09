---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Crear un punto de conexión de OData v4 con ASP.NET Web API 2.2 | Documentos de Microsoft
author: MikeWasson
description: Open Data Protocol (OData) es un protocolo de acceso de datos para la web. OData proporciona una manera uniforme para consultar y manipular conjuntos de datos a través de operaciones CRUD...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/24/2014
ms.topic: article
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: a3f94818f9674b0e1e9a45b2a6cc9455edc79726
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508054"
---
<a name="create-an-odata-v4-endpoint-using-aspnet-web-api-22"></a>Crear un punto de conexión de OData v4 con ASP.NET Web API 2.2
====================
por [Mike Wasson](https://github.com/MikeWasson)

> Open Data Protocol (OData) es un protocolo de acceso de datos para la web. OData proporciona una manera uniforme para consultar y manipular conjuntos de datos a través de las operaciones CRUD (crear, leer, actualizar y eliminar).
> 
> Es compatible con ASP.NET Web API v3 y v4 del protocolo. Se puede tener un punto de conexión de v4 que se ejecuta en paralelo con un punto de conexión v3.
> 
> Este tutorial muestra cómo crear un punto de conexión de OData v4 que admite operaciones CRUD.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - 2.2 API Web
> - OData v4
> - [Visual Studio 2013 Update 2](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - Entity Framework 6
> - .NET 4.5
> 
> 
> ## <a name="tutorial-versions"></a>Versiones de tutoriales
> 
> Para la versión 3 de OData, vea [creación de un extremo de OData v3](../odata-v3/creating-an-odata-endpoint.md).


## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

En Visual Studio, desde la **archivo** menú, seleccione **New** &gt; **proyecto**.

Expanda **instalado** &gt; **plantillas** &gt; **Visual C#** &gt; **Web**y seleccione el  **Aplicación Web ASP.NET** plantilla. Denomine el proyecto &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image2.png)](create-an-odata-v4-endpoint/_static/image1.png)

En el **nuevo proyecto** cuadro de diálogo, seleccione la **vacía** plantilla. En &quot;agregar carpetas y principales referencias... &quot;, haga clic en **API Web**. Haga clic en **Aceptar**.

[![](create-an-odata-v4-endpoint/_static/image4.png)](create-an-odata-v4-endpoint/_static/image3.png)

## <a name="install-the-odata-packages"></a>Instalar los paquetes de OData

Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet** &gt; **Package Manager Console**. En la ventana de la consola de administrador de paquetes, escriba:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Este comando instala los paquetes de OData NuGet más recientes.

## <a name="add-a-model-class"></a>Agregar una clase de modelo

A *modelo* es un objeto que representa una entidad de datos en la aplicación.

En el Explorador de soluciones, haga clic en la carpeta Models. En el menú contextual, seleccione **agregar** &gt; **clase**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Por convención, las clases de modelo se colocan en la carpeta Models, pero no tienes que siguen esta convención en sus propios proyectos.


Asigne a la clase el nombre `Product`. En el archivo Product.cs, reemplace el código reutilizable con lo siguiente:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

El `Id` propiedad es la clave de entidad. Los clientes pueden consultar entidades por clave. Por ejemplo, para obtener el producto con el identificador de 5, el URI es `/Products(5)`. El `Id` propiedad también será la clave principal de la base de datos back-end.

## <a name="enable-entity-framework"></a>Habilitar Entity Framework

Para este tutorial, vamos a usar Code First de Entity Framework (EF) para crear la base de datos back-end.

> [!NOTE]
> Web API OData no requiere EF. Utilice cualquier capa de acceso a datos que las entidades de base de datos se puede convertir en modelos.


En primer lugar, instale el paquete de NuGet para EF. Desde el **herramientas** menú, seleccione **Administrador de paquetes de NuGet** &gt; **Package Manager Console**. En la ventana de la consola de administrador de paquetes, escriba:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Abra el archivo Web.config y agregue la siguiente sección dentro de la **configuración** elemento, después la **configSections** elemento.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Esta opción agrega una cadena de conexión para una base de datos de LocalDB. Esta base de datos se usará cuando se ejecuta la aplicación localmente.

A continuación, agregue una clase denominada `ProductsContext` en la carpeta modelos:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

En el constructor, `"name=ProductsContext"` proporciona el nombre de la cadena de conexión.

## <a name="configure-the-odata-endpoint"></a>Configurar el extremo de OData

Abra el archivo de aplicación\_Start/WebApiConfig.cs. Agregue el siguiente **con** instrucciones:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

A continuación, agregue el código siguiente a la **registrar** método:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Este código hace dos cosas:

- Crea un Entity Data Model (EDM).
- Agrega una ruta.

Un EDM es un modelo de datos abstracto. EDM se usa para crear el documento de metadatos del servicio. El **ODataConventionModelBuilder** clase crea un modelo EDM mediante el uso de convenciones de nomenclatura predeterminadas. Este enfoque requiere que el código mínimo. Si desea más control sobre el modelo EDM, puede usar el **ODataModelBuilder** clase para crear el modelo EDM agregando propiedades, claves y propiedades de navegación explícitamente.

A *ruta* indica cómo enrutar las solicitudes HTTP al extremo de API Web. Para crear una ruta de OData v4, llame a la **MapODataServiceRoute** método de extensión.

Si la aplicación tiene varios puntos de conexión de OData, cree una ruta independiente para cada uno. Asigne cada ruta de un nombre de ruta único y un prefijo.

## <a name="add-the-odata-controller"></a>Agregar el controlador de OData

A *controlador* es una clase que controla las solicitudes HTTP. Crear un controlador independiente para cada conjunto de entidades en el servicio de OData. En este tutorial, creará un controlador para el `Product` entidad.

En el Explorador de soluciones, haga clic en la carpeta Controllers y seleccione **agregar** &gt; **clase**. Asigne a la clase el nombre `ProductsController`.

> [!NOTE]
> La versión de este tutorial para OData v3 utiliza el **Agregar controlador** scaffolding. Actualmente, no hay ningún scaffolding para OData v4.


Reemplace el código reutilizable en ProductsController.cs con lo siguiente.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

El controlador utiliza la `ProductsContext` clase para tener acceso a la base de datos usan EF. Tenga en cuenta que el controlador invalida la **Dispose** método para desechar el **ProductsContext**.

Este es el punto inicial para el controlador. A continuación, vamos a agregar métodos para todas las operaciones CRUD.

## <a name="querying-the-entity-set"></a>Consultar el conjunto de entidades

Agregue los métodos siguientes para `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

La versión sin parámetros de la `Get` método devuelve toda la colección de productos. El `Get` método con un *clave* parámetro busca un producto por su clave (en este caso, el `Id` propiedad).

El **[EnableQuery]** atributo permite a los clientes modificar la consulta, utilizando las opciones de consulta como $filter, $sort y $page. Para obtener más información, consulte [admiten opciones de consulta de OData](../supporting-odata-query-options.md).

## <a name="adding-an-entity-to-the-entity-set"></a>Agregar una entidad al conjunto de entidades

Para permitir que los clientes agregar un nuevo producto a la base de datos, agregue el método siguiente a `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="updating-an-entity"></a>Actualizar una entidad

OData admite dos una semántica diferente para actualizar una entidad, PATCH y PUT.

- REVISIÓN lleva a cabo una actualización parcial. El cliente especifica las propiedades de actualización.
- PUT reemplaza toda la entidad.

La desventaja de PUT es que el cliente debe enviar valores para todas las propiedades de la entidad, incluidos los valores que no va a modificar. El [especificación OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indica que se prefiere la revisión.

En cualquier caso, este es el código para los métodos de revisión y PUT:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

En el caso de revisión, el controlador utiliza la **Delta&lt;T&gt;**  tipo para realizar el seguimiento de los cambios.

## <a name="deleting-an-entity"></a>Eliminar una entidad

Para permitir que los clientes eliminar un producto de la base de datos, agregue el método siguiente a `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
