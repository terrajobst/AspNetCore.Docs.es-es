---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
title: Agregar un nuevo campo a la tabla (C#) y el modelo de película | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: b4e76c1a-f66e-43a0-aa72-f39df79c07c1
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-new-field
msc.type: authoredcontent
ms.openlocfilehash: 62995c1b53dad12f4d9202333520080af9e71e1a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837375"
---
<a name="adding-a-new-field-to-the-movie-model-and-table-c"></a>Agregar un nuevo campo a la tabla (C#) y el modelo de película
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestran las características más.
> 
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todos ellos haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos mediante los vínculos siguientes:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tiempo de ejecución de herramientas de soporte técnico +)
> 
> Si usa Visual Studio 2010, en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente de C# está disponible como acompañamiento de este tema. [Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.


En esta sección podrá realizar algunos cambios en las clases del modelo y obtenga información sobre cómo se puede actualizar el esquema de base de datos para que coincida con los cambios del modelo.

## <a name="adding-a-rating-property-to-the-movie-model"></a>Adición de una propiedad de clasificación al modelo Movie

Empiece agregando un nuevo `Rating` propiedad existente `Movie` clase. Abra el *Movie.cs* y agréguele el `Rating` propiedad como esta:

[!code-csharp[Main](adding-a-new-field/samples/sample1.cs)]

La completa `Movie` clase ahora parece similar al código siguiente:

[!code-csharp[Main](adding-a-new-field/samples/sample2.cs)]

Volver a compilar la aplicación mediante el **depurar** &gt; **crear películas** comando de menú.

Ahora que ha actualizado el `Model` (clase), también deberá actualizar el *\Views\Movies\Index.cshtml* y *\Views\Movies\Create.cshtml* ver plantillas para admitir la nueva `Rating`propiedad.

Abra el *\Views\Movies\Index.cshtml* archivo y agregue un `<th>Rating</th>` justo después de encabezado de columna la **precio** columna. A continuación, agregue un `<td>` columna cerca del final de la plantilla para representar el `@item.Rating` valor. A continuación es lo que la actualización *Index.cshtml* plantilla de vista es similar:

[!code-cshtml[Main](adding-a-new-field/samples/sample3.cshtml)]

A continuación, abra el *\Views\Movies\Create.cshtml* archivo y agregue el siguiente marcado cerca del final del formulario. Esto representa un cuadro de texto para que pueda especificar una clasificación cuando se crea una nueva película.

[!code-cshtml[Main](adding-a-new-field/samples/sample4.cshtml)]

## <a name="managing-model-and-database-schema-differences"></a>Administración de modelo y las diferencias de esquema de base de datos

Se ha actualizado el código de aplicación para admitir los nuevos `Rating` propiedad.

Ahora ejecute la aplicación y vaya a la */Movies* dirección URL. Sin embargo, al hacerlo, verá el siguiente error:

![](adding-a-new-field/_static/image1.png)

Si ve este error porque la actualización `Movie` ahora es diferente del esquema de clase de modelo en la aplicación la `Movie` tabla de la base de datos existente. (No hay ninguna columna `Rating` en la tabla de la base de datos).

De forma predeterminada, al usar Entity Framework Code First para crear automáticamente una base de datos, como hizo anteriormente en este tutorial, Code First agrega una tabla para ayudar a saber si el esquema de la base de datos está sincronizado con las clases del modelo se generó a partir de la base de datos. Si no están sincronizados, Entity Framework produce un error. Esto facilita realizar un seguimiento de los problemas en tiempo de desarrollo en caso contrario, podría encontrar solamente (por errores poco conocidos) en tiempo de ejecución. La característica de comprobación de sincronización es lo que ocasiona que se mostrará el mensaje de error que acabamos de ver.

Hay dos enfoques para resolver el error:

1. Haga que Entity Framework quite de forma automática la base de datos y la vuelva a crear basándose en el nuevo esquema de la clase del modelo. Este enfoque es muy práctico al realizar el desarrollo activo en una base de datos de prueba, ya que permite desarrollar rápidamente el esquema de modelo y la base de datos entre sí. La desventaja, sin embargo, es la que se pierden los datos existentes en la base de datos, por lo que se *no* va a utilizar este enfoque en una base de datos de producción.
2. Modifique explícitamente el esquema de la base de datos existente para que coincida con las clases del modelo. La ventaja de este enfoque es que se conservan los datos. Puede realizar este cambio de forma manual o mediante la creación de un script de cambio de base de datos.

Para este tutorial, usaremos el primer enfoque, tendrá el Entity Framework Code First automáticamente volver a crear la base de datos cada vez que cambia el modelo.

## <a name="automatically-re-creating-the-database-on-model-changes"></a>Volver a crear automáticamente la base de datos los cambios del modelo

Vamos a actualizar la aplicación para que Code First automáticamente se quita y vuelve a crear la base de datos cada vez que cambia el modelo de la aplicación.

> [!NOTE] 
> 
> **Advertencia** debe habilitar este enfoque de automáticamente eliminar y volver a crear la base de datos solo cuando se usa una base de datos de prueba o desarrollo, y *nunca* en una base de datos de producción que contiene datos reales. En uso en un servidor de producción puede provocar la pérdida de datos.


En **el Explorador de soluciones**, haga clic en el *modelos* carpeta, seleccione **agregar**y, a continuación, seleccione **clase**.

![](adding-a-new-field/_static/image2.png)

Nombre de la clase "MovieInitializer". Actualización de la `MovieInitializer` clase que contiene el código siguiente:

[!code-csharp[Main](adding-a-new-field/samples/sample5.cs)]

La `MovieInitializer` clase especifica que la base de datos utilizada por el modelo debe quitarse y volver a crear automáticamente si alguna vez cambian las clases del modelo. El código incluye un `Seed` método para especificar algunos datos de forma predeterminada para agregar automáticamente a la base de datos cualquiera de tiempo que ha creado (o volver a crear). Esto proporciona una manera útil para rellenar la base de datos con datos de ejemplo, sin tener que rellenarlo manualmente cada vez que realice un modelo cambie.

Ahora que ha definido el `MovieInitializer` (clase), querrá conectarla para que cada vez que se ejecuta la aplicación, compruebe si las clases del modelo son diferentes del esquema en la base de datos. Si es así, puede ejecutar el inicializador para volver a crear la base de datos para que coincidan con el modelo y, a continuación, rellenar la base de datos con los datos de ejemplo.

Abra el *Global.asax* archivo que está en la raíz de la `MvcMovies` proyecto:

[![](adding-a-new-field/_static/image4.png)](adding-a-new-field/_static/image3.png)

El *Global.asax* archivo contiene la clase que defina toda la aplicación para el proyecto y contiene un `Application_Start` controlador de eventos que se ejecuta cuando la aplicación se inicia por primera vez.

Vamos a agregar dos instrucciones using a la parte superior del archivo. El espacio de nombres de Entity Framework hace referencia a la primera, y el segundo el espacio de nombres donde nuestro `MovieInitializer` vidas de clase:

[!code-csharp[Main](adding-a-new-field/samples/sample6.cs)]

A continuación, busque el `Application_Start` método y agregue una llamada a `Database.SetInitializer` al principio del método, tal como se muestra a continuación:

[!code-csharp[Main](adding-a-new-field/samples/sample7.cs)]

El `Database.SetInitializer` instrucción que acaba de agregar indica que la base de datos utilizado por el `MovieDBContext` instancia debe eliminarse automáticamente y se vuelve a crear si no coinciden con el esquema y la base de datos. Y como se vio, también rellenará la base de datos con los datos de ejemplo que se especifica en el `MovieInitializer` clase.

Cerrar la *Global.asax* archivo.

Vuelva a ejecutar la aplicación y navegue hasta la */Movies* dirección URL. Cuando se inicia la aplicación, detecta que la estructura del modelo ya no coincide con el esquema de base de datos. Automáticamente se vuelve a crear la base de datos para que coincida con la nueva estructura del modelo y se rellena la base de datos con las películas de ejemplo:

![7_MyMovieList_SM](adding-a-new-field/_static/image5.png)

Haga clic en el **crear nuevo** el vínculo para agregar una nueva película. Tenga en cuenta que puede agregar una clasificación.

[![7_CreateRioII](adding-a-new-field/_static/image7.png)](adding-a-new-field/_static/image6.png)

Haga clic en **Crear**. La nueva película, incluida la clasificación, se muestra ahora en la lista de películas:

[![7_ourNewMovie_SM](adding-a-new-field/_static/image9.png)](adding-a-new-field/_static/image8.png)

En esta sección, ha visto cómo puede modificar los objetos de modelo y mantener sincronizados con los cambios de la base de datos. También ha aprendido cómo rellenar una base de datos recién creada con datos de ejemplo para que pueda probar escenarios. A continuación, echemos un vistazo a cómo puede agregar lógica de validación más completa para las clases del modelo y habilitar algunas reglas de negocios que se aplicará.

> [!div class="step-by-step"]
> [Anterior](examining-the-edit-methods-and-edit-view.md)
> [Siguiente](adding-validation-to-the-model.md)
