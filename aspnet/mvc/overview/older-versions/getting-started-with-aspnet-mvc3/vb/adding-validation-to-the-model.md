---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
title: Agregar validación al modelo (VB) | Microsoft Docs
author: Rick-Anderson
description: Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es...
ms.author: riande
ms.date: 01/12/2011
ms.assetid: 878f6c31-972d-45f4-8849-5c633b511409
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/vb/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: d2b4fb9d8f673f223c923ba0705b1ea27d430f43
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827610"
---
<a name="adding-validation-to-the-model-vb"></a>Agregar validación al modelo (VB)
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial le enseñará los aspectos básicos de la creación de una aplicación Web de ASP.NET MVC mediante Microsoft Visual Web Developer 2010 Express Service Pack 1, que es una versión gratuita de Microsoft Visual Studio. Antes de empezar, asegúrese de que ha instalado los requisitos previos descritos a continuación. Puede instalar todos ellos haciendo clic en el siguiente vínculo: [instalador de plataforma Web](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack). Como alternativa, puede instalar individualmente los requisitos previos mediante los vínculos siguientes:
> 
> - [Requisitos previos de Visual Studio Web Developer Express SP1](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [ASP.NET MVC 3 Tools Update](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - [SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(tiempo de ejecución de herramientas de soporte técnico +)
> 
> Si usa Visual Studio 2010, en lugar de Visual Web Developer 2010, instale los requisitos previos, haga clic en el siguiente vínculo: [requisitos previos de Visual Studio 2010](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).
> 
> Un proyecto de Visual Web Developer con código fuente VB.NET está disponible como acompañamiento de este tema. [Descargue la versión VB.NET](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098). Si lo prefiere C#, cambie a la [C# versión](../cs/adding-validation-to-the-model.md) de este tutorial.


En esta sección agregará lógica de validación para el `Movie` modelo y se asegurará de que las reglas de validación se aplican cada vez que un usuario intenta crear o editar una película con la aplicación.

## <a name="keeping-things-dry"></a>Mantener las cosas seco

Uno de los principios de diseño principales de ASP.NET MVC es DRY ("no repitas"). ASP.NET MVC le anima a especificar funcionalidad o comportamiento de una sola vez y, a continuación, hacer que se reflejen en todas partes en una aplicación. Esto reduce la cantidad de código que necesita para escribir y hace que el código que escribir mucho más fácil mantener.

La compatibilidad de validación proporcionada por ASP.NET MVC y Entity Framework Code First es un buen ejemplo del principio DRY en acción. Puede especificar mediante declaración las reglas de validación en un solo lugar (en la clase del modelo) y, a continuación, esas reglas se aplican en toda la aplicación.

Echemos un vistazo a cómo puede aprovechar las ventajas de esta compatibilidad de validación de la aplicación de película.

## <a name="adding-validation-rules-to-the-movie-model"></a>Agregar reglas de validación al modelo de película

Para comenzar, agregar alguna lógica de validación para el `Movie` clase.

Abra el *Movie.vb* archivo. Agregar un `Imports` instrucción en la parte superior del archivo que se hace referencia a la [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espacio de nombres:

[!code-vb[Main](adding-validation-to-the-model/samples/sample1.vb)]

El espacio de nombres forma parte de .NET Framework. Proporciona un conjunto integrado de atributos de validación que se pueden aplicar mediante declaración a cualquier clase o propiedad.

Ahora, actualice el `Movie` clase para aprovechar las ventajas de los integrados [ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), y [ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributos de validación . Use el código siguiente como ejemplo de dónde aplicar los atributos.

[!code-vb[Main](adding-validation-to-the-model/samples/sample2.vb)]

Los atributos de validación especifican el comportamiento que quiere aplicar en las propiedades del modelo al que se aplican. El `Required` atributo indica que una propiedad debe tener un valor; en este ejemplo, una película tiene que tener valores para el `Title`, `ReleaseDate`, `Genre`, y `Price` propiedades para que sean válidos. El atributo `Range` restringe un valor a un intervalo determinado. El atributo `StringLength` permite establecer la longitud máxima de una propiedad de cadena y, opcionalmente, su longitud mínima.

En primer lugar, código garantiza que se aplican las reglas de validación que especifique en una clase de modelo antes de que la aplicación guarda los cambios en la base de datos. Por ejemplo, el código siguiente generará una excepción cuando el `SaveChanges` se denomina método, porque varios necesario `Movie` faltan valores de propiedad y el precio es cero (lo que está fuera del intervalo válido).

[!code-vb[Main](adding-validation-to-the-model/samples/sample3.vb)]

Las reglas de validación que aplique automáticamente .NET Framework ayuda a que la aplicación sea más robusto. También nos permite asegurarnos de que todo se valida y que no nos dejamos ningún dato incorrecto en la base de datos accidentalmente.

Este es el código completo para la actualización *Movie.vb* archivo:

[!code-vb[Main](adding-validation-to-the-model/samples/sample4.vb)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Error de validación de la interfaz de usuario en ASP.NET MVC

Vuelva a ejecutar la aplicación y navegue hasta la */Movies* dirección URL.

Haga clic en el **crear películas** vínculo para agregar una nueva película. Rellene el formulario con algunos valores no válidos y, a continuación, haga clic en el **crear** botón.

[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)

Observe cómo el formulario automáticamente usa un color de fondo para resaltar los cuadros de texto que contienen datos no válidos y se genera un mensaje de error de validación adecuado junto a cada uno de ellos. Los mensajes de error coinciden con las cadenas de error que especificó cuando anota el `Movie` clase. Los errores se aplican del lado cliente (mediante JavaScript) y del lado servidor (en caso de que un usuario tiene JavaScript deshabilitado).

Una ventaja real es que no necesita cambiar una sola línea de código en el `MoviesController` clase o en el *Create.vbhtml* vista con el fin de habilitar esta interfaz de usuario de validación. El controlador y las vistas que creó anteriormente en este tutorial automáticamente seleccionaron las reglas de validación que especificó mediante atributos en el `Movie` clase de modelo.

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Cómo se produce la validación en el crear, ver y crea método de acción

Tal vez se pregunte cómo se generó la validación de la interfaz de usuario sin actualizar el código en el controlador o las vistas. La lista siguiente muestra lo que el `Create` métodos en el `MovieController` se parecen a clase. Son iguales que cómo haya creado anteriormente en este tutorial.

[!code-vb[Main](adding-validation-to-the-model/samples/sample5.vb)]

El primer método de acción muestra el formulario de creación inicial. El segundo administra el envío de formulario. El segundo `Create` llamadas al método `ModelState.IsValid` para comprobar si la película tiene errores de validación. Al llamar a este método se evalúan todos los atributos de validación que se hayan aplicado al objeto. Si el objeto tiene errores de validación, el `Create` método vuelve a mostrar el formulario. Si no hay ningún error, el método guarda la nueva película en la base de datos.

A continuación se presenta la *Create.vbhtml* plantilla de vista que se ha aplicado scaffolding anteriormente en este tutorial. Los métodos de acción que se muestran arriba la usan para mostrar el formulario inicial y para volver a mostrarlo en caso de error.

[!code-vbhtml[Main](adding-validation-to-the-model/samples/sample6.vbhtml)]

Observe cómo el código usa un `Html.EditorFor` auxiliar para generar el `<input>` para cada elemento `Movie` propiedad. Junto a esta aplicación auxiliar es una llamada a la `Html.ValidationMessageFor` método auxiliar. Estos dos métodos auxiliares que funcionan con el objeto de modelo que se pasa por el controlador a la vista (en este caso, un `Movie` objeto). Automáticamente buscar atributos de validación especificados en los mensajes de error de modelo y la presentación según corresponda.

Lo que resulta agradable sobre este enfoque es que el controlador ni la plantilla de vista Create conoce nada acerca de las reglas de validación real que se apliquen o los mensajes de error específico que se muestra. Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`.

Si desea cambiar la lógica de validación más adelante, puede hacerlo en exactamente un solo lugar. No tendrá que preocuparse de que diferentes partes de la aplicación sean incoherentes con el modo en que se aplican las reglas: toda la lógica de validación se definirá en un solo lugar y se usará en todas partes. Esto mantiene el código muy limpio y hace que sea fácil de mantener y evolucionar. También significa que respeta totalmente el principio DRY.

## <a name="adding-formatting-to-the-movie-model"></a>Agregar formato al modelo de película

Abra el *Movie.vb* archivo. El [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) espacio de nombres proporciona atributos de formato además del conjunto integrado de atributos de validación. Que va a aplicar el [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo y un [ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) valor de enumeración para la fecha de lanzamiento y los campos de precio. El siguiente código muestra la `ReleaseDate` y `Price` propiedades con los valores adecuados [ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo.

[!code-vb[Main](adding-validation-to-the-model/samples/sample7.vb)]

Como alternativa, podría establecer explícitamente un [ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx) valor. El código siguiente muestra la propiedad fecha de lanzamiento con una cadena de formato de fecha (es decir, "d"). Usaría esto para especificar que no desea tiempo como parte de la fecha de lanzamiento.

[!code-vb[Main](adding-validation-to-the-model/samples/sample8.vb)]

El siguiente código formatos el `Price` propiedad como moneda.

[!code-vb[Main](adding-validation-to-the-model/samples/sample9.vb)]

La completa `Movie` clase se muestra a continuación.

[!code-vb[Main](adding-validation-to-the-model/samples/sample10.vb)]

Ejecute la aplicación y vaya a la `Movies` controlador.

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

En la siguiente parte de la serie, revisaremos la aplicación y realizaremos algunas mejoras a la que se han generado automáticamente `Details` y `Delete` métodos...

> [!div class="step-by-step"]
> [Anterior](adding-a-new-field.md)
> [Siguiente](improving-the-details-and-delete-methods.md)
