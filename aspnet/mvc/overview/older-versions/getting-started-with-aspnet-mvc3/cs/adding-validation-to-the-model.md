---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: Agregar validación para el modelo (C#) | Documentos de Microsoft
author: Rick-Anderson
description: Creación de un controlador
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: cdfe76f440e34ca7356af193186f90d2231b9db6
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872551"
---
<a name="adding-validation-to-the-model-c"></a>Agregar validación para el modelo (C#)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestra más características.
> 
> 
> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todas ellas haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar por separado los requisitos previos mediante los siguientes vínculos:
> 
> - [Requisitos previos visuales Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(se admiten en tiempo de ejecución + herramientas)
> 
> Si usa Visual Studio 2010 en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con el código fuente de C# está disponible como acompañamiento de este tema. [Descargue la versión de C#](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si prefiere Visual Basic, cambie a la [versión de Visual Basic](../vb/intro-to-aspnet-mvc-3.md) de este tutorial.


En esta sección agregará una lógica de validación para el `Movie` modelo y deberá asegurarse de que las reglas de validación se aplican siempre que un usuario intenta crear o editar una película con la aplicación.

## <a name="keeping-things-dry"></a>Mantener las cosas SECA

Uno de los principios de diseño principales de ASP.NET MVC es SECA ("no repitas"). ASP.NET MVC le anima a especificar funcionalidad o comportamiento de una sola vez y para que se reflejen en todas partes en una aplicación. Esto reduce la cantidad de código que necesita para escribir y hace que el código que escribe mucho más fácil mantener.

La compatibilidad de validación proporcionada por ASP.NET MVC y Entity Framework Code First es un buen ejemplo del principio seco en acción. Puede especificar mediante declaración las reglas de validación en un solo lugar (en la clase del modelo) y, a continuación, se aplican esas reglas en cualquier lugar en la aplicación.

Echemos un vistazo a cómo puede aprovechar las ventajas de esta compatibilidad de validación en la aplicación de película.

## <a name="adding-validation-rules-to-the-movie-model"></a>Agregar reglas de validación para el modelo de película

Comenzará agregando alguna lógica de validación para el `Movie` clase.

Abra el archivo *Movie.cs*. Agregar un `using` instrucción en la parte superior del archivo que se hace referencia a la [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espacio de nombres:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

El espacio de nombres forma parte de .NET Framework. Proporciona un conjunto integrado de atributos de validación que se pueden aplicar mediante declaración a cualquier clase o propiedad.

Ahora, actualice la `Movie` clase para aprovechar las ventajas de los integrados [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), y [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributos de validación . Utilice el código siguiente como ejemplo de dónde se debe aplicar los atributos.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

Los atributos de validación especifican el comportamiento que quiere aplicar en las propiedades del modelo al que se aplican. El `Required` atributo indica que una propiedad debe tener un valor; en este ejemplo, una película que tienen valores para el `Title`, `ReleaseDate`, `Genre`, y `Price` propiedades para que sean válidos. El atributo `Range` restringe un valor a un intervalo determinado. El atributo `StringLength` permite establecer la longitud máxima de una propiedad de cadena y, opcionalmente, su longitud mínima.

En primer lugar, código garantiza que se aplican las reglas de validación que se especifique en una clase de modelo antes de que la aplicación guarda los cambios en la base de datos. Por ejemplo, el código siguiente iniciará una excepción cuando la `SaveChanges` se denomina método, porque requiere varios `Movie` faltan valores de propiedad y el precio es cero (lo que está fuera del intervalo válido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

Tener reglas de validación se aplica automáticamente con .NET Framework ayuda a que la aplicación más sólida. También nos permite asegurarnos de que todo se valida y que no nos dejamos ningún dato incorrecto en la base de datos accidentalmente.

Esta es una lista de los archivos de código completa *Movie.cs* archivo:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Error de validación de interfaz de usuario en ASP.NET MVC

Vuelva a ejecutar la aplicación y navegue hasta la */Movies* dirección URL.

Haga clic en el **crear películas** el vínculo para agregar una película nuevo. Rellene el formulario con algunos valores no válidos y, a continuación, haga clic en el **crear** botón.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Observe cómo el formulario automáticamente utiliza un color de fondo para resaltar los cuadros de texto que contienen datos no válidos y se genera un mensaje de error de validación adecuada junto a cada uno de ellos. Los mensajes de error coinciden con las cadenas de error que especificó cuando anota el `Movie` clase. Los errores se aplican (uso de JavaScript) en el lado de cliente y servidor (en caso de que un usuario tenga JavaScript deshabilitado).

Una ventaja real es que no necesita cambiar una sola línea de código en el `MoviesController` clase o en la *Create.cshtml* vista para permitir que esta interfaz de usuario de validación. El controlador y vistas que creó anteriormente en este tutorial automáticamente elegido la validación de reglas que especifica el uso de atributos en la `Movie` clase de modelo.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Cómo se produce la validación en el crear, ver y crea método de acción

Tal vez se pregunte cómo se generó la validación de la interfaz de usuario sin actualizar el código en el controlador o las vistas. La lista siguiente muestra lo que hace el `Create` métodos en la `MovieController` clase aspecto. Se trata de no difieren en cómo haya creado anteriormente en este tutorial.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

El primer método de acción muestra el formulario de creación inicial. El segundo controla el envío de formulario. El segundo `Create` llamadas al método `ModelState.IsValid` para comprobar si la película tiene errores de validación. Al llamar a este método se evalúan todos los atributos de validación que se hayan aplicado al objeto. Si el objeto tiene errores de validación, el `Create` método vuelve a mostrar el formulario. Si no hay ningún error, el método guarda la nueva película en la base de datos.

A continuación se muestra la *Create.cshtml* plantilla de vista de scaffolding anteriormente en el tutorial. Los métodos de acción que se muestran arriba la usan para mostrar el formulario inicial y para volver a mostrarlo en caso de error.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Observe cómo el código utiliza un `Html.EditorFor` auxiliar para generar el `<input>` para cada elemento `Movie` propiedad. Junto a esta aplicación auxiliar es una llamada a la `Html.ValidationMessageFor` método auxiliar. Estos dos métodos auxiliares trabajar con el objeto de modelo que se pasa por el controlador a la vista (en este caso, un `Movie` objeto). Se muestran automáticamente para los atributos de validación especificados en los mensajes de error de modelo y la presentación según corresponda.

¿Qué es excelente sobre este enfoque es que el controlador ni la plantilla de vista de crear sabe nada acerca de las reglas de validación real que se apliquen o acerca de los mensajes de error específicos que se muestran. Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`.

Si desea cambiar la lógica de validación más adelante, puede hacerlo en exactamente un solo lugar. No tendrá que preocuparse de que diferentes partes de la aplicación sean incoherentes con el modo en que se aplican las reglas: toda la lógica de validación se definirá en un solo lugar y se usará en todas partes. Esto mantiene el código muy limpio y hace que sea fácil de mantener y evolucionar. También significa que respeta totalmente el principio DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Si agrega un formato para el modelo de película

Abra el archivo *Movie.cs*. El [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espacio de nombres proporciona los atributos de formato además del conjunto de atributos de validación integrado. Que va a aplicar la [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo y un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valor de enumeración para la fecha de lanzamiento y los campos de precio. El código siguiente muestra el `ReleaseDate` y `Price` propiedades con el adecuado [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

Como alternativa, podría establecer explícitamente un [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valor. El código siguiente muestra la propiedad de fecha de lanzamiento con una cadena de formato de fecha (es decir, "d"). Se utilizaría para especificar que no desea tiempo como parte de la fecha de lanzamiento.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

El siguiente código formatos el `Price` propiedad como moneda.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

La sección completa `Movie` clase se muestra a continuación.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Ejecute la aplicación y busque el `Movies` controlador.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

En la siguiente parte de la serie de tutoriales, revisaremos la aplicación y realizaremos algunas mejoras a los métodos `Details` y `Delete` generados automáticamente.

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field.md)
> [Siguiente](improving-the-details-and-delete-methods.md)
