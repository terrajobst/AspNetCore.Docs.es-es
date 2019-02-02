---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
title: Crear un punto de conexión de OData v4 con ASP.NET Web API 2.2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) es un protocolo de acceso a datos para la web. OData proporciona una manera uniforme para consultar y manipular conjuntos de datos mediante operaciones CRUD...
ms.author: riande
ms.date: 01/23/2019
ms.assetid: 1e1927c0-ded1-4752-80fd-a146628d2f09
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint
msc.type: authoredcontent
ms.openlocfilehash: c6a4aa4eb563fd77d5afd9248175d5f5b7984d19
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667575"
---
# <a name="create-an-odata-v4-endpoint-using-aspnet-web-api"></a>Crear un punto de conexión de OData v4 con ASP.NET Web API 

> Open Data Protocol (OData) es un protocolo de acceso a datos para la web. OData proporciona una manera uniforme para consultar y manipular conjuntos de datos a través de las operaciones CRUD (crear, leer, actualizar y eliminar).
>
> Es compatible con ASP.NET Web API v3 y v4 del protocolo. Incluso puede tener un punto de conexión de v4 que se ejecuta en paralelo con un punto de conexión v3.
>
> Este tutorial muestra cómo crear un punto de conexión de OData v4 que admite operaciones CRUD.
>
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
>
> - Web API 5.2
> - OData v4
> - Visual Studio 2017 (Descargar Visual Studio 2017 [aquí](https://visualstudio.microsoft.com/downloads/))
> - Entity Framework 6
> - .NET 4.7.2
>
> ## <a name="tutorial-versions"></a>Versiones de tutoriales
>
> Para la versión 3 de OData, consulte [creación de un extremo de OData v3](../odata-v3/creating-an-odata-endpoint.md).

## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

En Visual Studio, desde el **archivo** menú, seleccione **New** &gt; **proyecto**.

Expanda **instalado** &gt; **Visual C#**  &gt; **Web**y seleccione el **aplicación Web ASP.NET (.NET Framework)**  plantilla. Denomine el proyecto &quot;ProductService&quot;.

[![](create-an-odata-v4-endpoint/_static/image7.png)](create-an-odata-v4-endpoint/_static/image7.png)

Seleccione **Aceptar**.



[![](create-an-odata-v4-endpoint/_static/image8.png)](create-an-odata-v4-endpoint/_static/image8.png)

Seleccione el **vacía** plantilla. En **agregar carpetas y referencias centrales para:**, seleccione **API Web**. Seleccione **Aceptar**.

## <a name="install-the-odata-packages"></a>Instalar los paquetes de OData

En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** &gt; **Consola del Administrador de paquetes**. En la ventana de consola de administrador de paquetes, escriba:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample1.cmd)]

Este comando instala los paquetes de OData NuGet más recientes.

## <a name="add-a-model-class"></a>Incorporación de una clase de modelo

Un *modelo* es un objeto que representa una entidad de datos en la aplicación.

En el Explorador de soluciones, haga clic en la carpeta Models. En el menú contextual, seleccione **agregar** &gt; **clase**.

[![](create-an-odata-v4-endpoint/_static/image6.png)](create-an-odata-v4-endpoint/_static/image5.png)

> [!NOTE]
> Por convención, las clases de modelo se colocan en la carpeta Models, pero no tiene que seguir esta convención en sus propios proyectos.


Asigne a la clase el nombre `Product`. En el archivo Product.cs, reemplace el código reutilizable, con lo siguiente:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample2.cs)]

El `Id` propiedad es la clave de entidad. Los clientes pueden consultar las entidades por clave. Por ejemplo, para obtener el producto con el Id. de 5, el URI es `/Products(5)`. El `Id` propiedad también será la clave principal de la base de datos back-end.

## <a name="enable-entity-framework"></a>Habilitar Entity Framework

Para este tutorial, vamos a usar Entity Framework (EF) Code First para crear la base de datos back-end.

> [!NOTE]
> Web API OData no requiere EF. Utilice cualquier capa de acceso a datos que se traduce las entidades de base de datos en modelos.


En primer lugar, instale el paquete de NuGet para EF. En el menú **Herramientas**, seleccione **Administrador de paquetes NuGet** &gt; **Consola del Administrador de paquetes**. En la ventana de consola de administrador de paquetes, escriba:

[!code-console[Main](create-an-odata-v4-endpoint/samples/sample3.cmd)]

Abra el archivo Web.config y agregue la siguiente sección dentro la **configuración** elemento, después el **configSections** elemento.

[!code-xml[Main](create-an-odata-v4-endpoint/samples/sample4.xml?highlight=6)]

Esta configuración agrega una cadena de conexión para una base de datos LocalDB. Esta base de datos se usará cuando se ejecuta la aplicación localmente.

A continuación, agregue una clase denominada `ProductsContext` a la carpeta modelos:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample5.cs)]

En el constructor, `"name=ProductsContext"` proporciona el nombre de la cadena de conexión.

## <a name="configure-the-odata-endpoint"></a>Configurar el extremo de OData

Abra el archivo de aplicación\_Start/WebApiConfig.cs. Agregue el siguiente **mediante** instrucciones:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample6.cs)]

A continuación, agregue el código siguiente a la **registrar** método:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample7.cs)]

Este código hace dos cosas:

- Crea un Entity Data Model (EDM).
- Agrega una ruta.

Un EDM es un modelo abstracto de los datos. El EDM se usa para crear el documento de metadatos de servicio. El **ODataConventionModelBuilder** clase crea un modelo EDM utilizando las convenciones de nomenclatura predeterminado. Este enfoque requiere menos código. Si desea más control sobre el modelo EDM, puede usar el **ODataModelBuilder** clase para crear el EDM agregando explícitamente las propiedades, claves y las propiedades de navegación.

Un *ruta* indica cómo enrutar las solicitudes HTTP al punto de conexión de API Web. Para crear una ruta de OData v4, llame a la **MapODataServiceRoute** método de extensión.

Si la aplicación tiene varios puntos de conexión de OData, cree una ruta independiente para cada uno. Asigne a cada ruta de un nombre de ruta único y un prefijo.

## <a name="add-the-odata-controller"></a>Agregar el controlador de OData

Un *controlador* es una clase que controla las solicitudes HTTP. Cree un controlador independiente para cada conjunto de entidades en el servicio de OData. En este tutorial, creará un controlador para el `Product` entidad.

En el Explorador de soluciones, haga clic en la carpeta Controllers y seleccione **agregar** &gt; **clase**. Asigne a la clase el nombre `ProductsController`.

> [!NOTE]
> La versión de este tutorial para OData v3 utiliza el **Agregar controlador** scaffolding. Actualmente, no hay ningún scaffolding para OData v4.


Reemplace el código reutilizable en ProductsController.cs por lo siguiente.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample8.cs)]

Utiliza el controlador del `ProductsContext` clase para tener acceso a la base de datos con EF. Tenga en cuenta que el controlador invalida la **Dispose** método para desechar el **ProductsContext**.

Es el punto de partida para el controlador. A continuación, agregaremos métodos para todas las operaciones CRUD.

## <a name="query-the-entity-set"></a>Consultar el conjunto de entidades

Agregue los siguientes métodos para `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample9.cs)]

La versión sin parámetros de la `Get` método devuelve el conjunto completo de productos. El `Get` método con un *clave* parámetro busca un producto por su clave (en este caso, el `Id` propiedad).

El **[EnableQuery]** atributo permite a los clientes modificar la consulta, mediante las opciones de consulta como $filter, $sort y $page. Para obtener más información, consulte [que admiten opciones de consulta de OData](../supporting-odata-query-options.md).

## <a name="add-an-entity-to-the-entity-set"></a>Agregar una entidad para el conjunto de entidades

Para permitir que los clientes agregar un nuevo producto a la base de datos, agregue el método siguiente a `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample10.cs)]

## <a name="update-an-entity"></a>Actualizar una entidad

OData admite dos una semántica diferente para actualizar una entidad, PATCH y PUT.

- REVISIÓN realiza una actualización parcial. El cliente especifica las propiedades de actualización.
- PUT reemplaza toda la entidad.

La desventaja de PUT es que el cliente debe enviar los valores de todas las propiedades de la entidad, incluidos los valores que no va a modificar. El [especificación OData](http://docs.oasis-open.org/odata/odata/v4.0/os/part1-protocol/odata-v4.0-os-part1-protocol.html#_Toc372793719) indica que se prefiere la revisión.

En cualquier caso, este es el código para métodos PATCH y PUT:

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample11.cs)]

En el caso de revisiones, el controlador utiliza la **Delta&lt;T&gt;**  tipo para realizar el seguimiento de los cambios.

## <a name="delete-an-entity"></a>Eliminar una entidad

Para permitir que los clientes eliminar un producto de la base de datos, agregue el método siguiente a `ProductsController`.

[!code-csharp[Main](create-an-odata-v4-endpoint/samples/sample12.cs)]
