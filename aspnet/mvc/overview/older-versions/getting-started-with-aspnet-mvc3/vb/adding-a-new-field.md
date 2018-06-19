---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
title: Agregar un nuevo campo en el modelo de película como una tabla de base de datos (VB) | Documentos de Microsoft
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 28970e1b-1845-4015-86ef-121e52a6c397
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 5927b7d977e375881fe618b4b844cbd708023ba1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30877325"
---
<a name="adding-a-new-field-to-the-movie-model-and-database-table-vb"></a>Agregar un nuevo campo en el modelo de película como una tabla de base de datos (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)
> 
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con el código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [versión de C#](../cs/adding-a-new-field.md) de este tutorial.


En esta sección podrá realizar algunos cambios en las clases del modelo y obtenga información acerca de cómo puede actualizar el esquema de base de datos para que coincida con los cambios del modelo.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Empiece agregando un nuevo `Rating` propiedad existente `Movie` clase. Abra la *Movie.cs* de archivos y agregar el `Rating` propiedad como esta:

[!code-vb[Main](adding-a-new-field/samples/sample1.vb)]

La sección completa `Movie` clase ahora parece similar al código siguiente:

[!code-vb[Main](adding-a-new-field/samples/sample2.vb)]

Volver a compilar la aplicación mediante el **depurar** &gt; **crear películas** comando de menú.

Ahora que ha actualizado el `Model` (clase), también debe actualizar el *\Views\Movies\Index.vbhtml* y *\Views\Movies\Create.vbhtml* ver plantillas para admitir la nueva `Rating`propiedad.

Abra la<em>\Views\Movies\Index.vbhtml</em> de archivos y agregar un `<th>Rating</th>` justo después de encabezado de columna la <strong>precio</strong> columna. A continuación, agregue un `<td>` columna cerca del final de la plantilla para representar la `@item.Rating` valor. A continuación se muestra qué actualizado <em>Index.vbhtml</em> plantilla de vista es similar:

[!code-vbhtml[Main](adding-a-new-field/samples/sample3.vbhtml)]

A continuación, abra el *\Views\Movies\Create.vbhtml* de archivos y agregue el siguiente marcado cerca del final del formulario. Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una película nuevo.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Administración de modelo y las diferencias de esquema de base de datos

Ahora ha actualizado el código de aplicación para admitir los nuevos `Rating` propiedad.

Ahora ejecute la aplicación y navegue hasta la */Movies* dirección URL. Sin embargo, al hacerlo, verá el siguiente error:

![](adding-a-new-field/_static/image1.png)

Está viendo este error porque la actualización `Movie` clase de modelo en la aplicación ahora es diferente que el esquema de la `Movie` tabla de la base de datos existente. (No hay ninguna columna `Rating` en la tabla de la base de datos).

De forma predeterminada, cuando se usa Entity Framework Code First para crear automáticamente una base de datos, como lo hizo anteriormente en este tutorial, Code First agrega una tabla para ayudar a realizar el seguimiento de si el esquema de la base de datos está sincronizado con las clases del modelo se generó a partir de la base de datos. Si no están sincronizados, Entity Framework produce un error. Esto hace más fácil realizar un seguimiento de los problemas en tiempo de desarrollo que en caso contrario, podría encontrar solamente (por errores poco claros) en tiempo de ejecución. La característica de comprobación de sincronización es lo que hace que el mensaje de error que se mostrará que acabamos de ver.

Hay dos métodos para resolver el error:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo. Este enfoque resulta muy cómoda para realizar el desarrollo activo en una base de datos de prueba, porque permite evolucione rápidamente el esquema de modelo y la base de datos entre sí. La desventaja, sin embargo, es que se pierden los datos existentes en la base de datos, por lo que *no* va a utilizar este enfoque en una base de datos de producción.
2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.

Para este tutorial, usaremos el primer enfoque, tendrá Entity Framework Code First automáticamente volver a crear la base de datos cada vez que cambia el modelo.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Volver a crear automáticamente la base de datos en los cambios de modelo

Vamos a actualizar la aplicación para que Code First automáticamente quita y vuelve a crear la base de datos cada vez que cambie el modelo de la aplicación.

> [!NOTE] 
> 
> **Advertencia** debe habilitar este enfoque de automáticamente quitar y volver a crear la base de datos solo cuando se usa una base de datos de desarrollo o de prueba, y *nunca* en una base de datos de producción que contiene los datos reales. Uso en un servidor de producción puede provocar la pérdida de datos.


En **el Explorador de soluciones**, haga clic con el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.

![](adding-a-new-field/_static/image2.png)

Nombre de la clase &quot;MovieInitializer&quot;. Actualización de la `MovieInitializer` clase para que contenga el código siguiente:

[!code-vb[Main](adding-a-new-field/samples/sample5.vb)]

La `MovieInitializer` clase especifica que la base de datos utilizada por el modelo debe quitarse y vuelve a crear automáticamente si alguna vez cambia las clases del modelo. El código incluye un `Seed` método para especificar datos predeterminados que agregue automáticamente a la base de datos un vez que se ha creado (o vuelve a crear). Esto proporciona una manera útil para rellenar la base de datos con datos de ejemplo, sin necesidad de rellenarla manualmente cada vez que realice un modelo cambie.

Ahora que ha definido el `MovieInitializer` (clase), debe tener conectarlo, por lo que cada vez que se ejecuta la aplicación, comprueba si las clases del modelo son diferentes del esquema en la base de datos. Si es así, puede ejecutar el inicializador para volver a crear la base de datos para que coincida con el modelo y, a continuación, rellene la base de datos con los datos de ejemplo.

Abra la *Global.asax* archivo que está en la raíz de la `MvcMovies` proyecto:

El *Global.asax* archivo contiene la clase que define toda la aplicación para el proyecto y contiene un `Application_Start` controlador de eventos que se ejecuta cuando la aplicación se inicia por primera vez.

Buscar el `Application_Start` método y agregue una llamada a `Database.SetInitializer` al principio del método, tal y como se muestra a continuación:

[!code-vb[Main](adding-a-new-field/samples/sample6.vb)]

El `Database.SetInitializer` instrucción que acaba de agregar indica que la base de datos utilizado por el `MovieDBContext` instancia debe eliminarse automáticamente y se vuelve a crear si el esquema y la base de datos no coinciden. Y como ha podido comprobar, también rellenará la base de datos con los datos de ejemplo que se especifica en la `MovieInitializer` clase.

Cerrar la *Global.asax* archivo.

Vuelva a ejecutar la aplicación y navegue hasta la */Movies* dirección URL. Cuando se inicia la aplicación, éste detecta que la estructura del modelo ya no coincide con el esquema de base de datos. Automáticamente se vuelve a crear la base de datos para que coincida con la nueva estructura de modelo y se rellena la base de datos con las películas de ejemplo:

![7_MyMovieList_SM](adding-a-new-field/_static/image3.png)

Haga clic en el **crear nuevo** el vínculo para agregar una película nuevo. Tenga en cuenta que puede agregar una clasificación.

[![7_CreateRioII](adding-a-new-field/_static/image5.png)](adding-a-new-field/_static/image4.png)

Haga clic en **Crear**. La película nuevo, incluida la clasificación, se muestra ahora en la lista de películas:

![7_ourNewMovie_SM](adding-a-new-field/_static/image6.png)

En esta sección se ha visto cómo puede modificar objetos de modelo y mantenerlos sincronizados con los cambios de la base de datos. También aprendió una forma de rellenar una base de datos recién creada con datos de ejemplo para probar escenarios. A continuación, echemos un vistazo a cómo puede agregar una lógica de validación a las clases del modelo y habilitar algunas reglas de negocios que se aplicará.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-validation-to-the-model.md)
