---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: "Agregar validación para el modelo | Documentos de Microsoft"
author: Rick-Anderson
description: "Nota: Una versión actualizada de este tutorial está disponible aquí que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y demostraciones..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/28/2012
ms.topic: article
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 73332d168e2f22621cb234a6591f3ce0eeed802f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-to-the-model"></a>Agregar validación para el modelo
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> > [!NOTE]
> > Hay disponible una versión actualizada de este tutorial [aquí](../../getting-started/introduction/getting-started.md) que usa ASP.NET MVC 5 y Visual Studio 2013. Es más seguro y mucho más fácil de seguir y se muestra más características.


En esto en esta sección, se agregará una lógica de validación para el `Movie` modelo y deberá asegurarse de que las reglas de validación se aplican siempre que un usuario intenta crear o editar una película con la aplicación.

## <a name="keeping-things-dry"></a>Mantener las cosas SECA

Uno de los principios de diseño principales de ASP.NET MVC es SECA (&quot;no repitas&quot;). ASP.NET MVC le anima a especificar funcionalidad o comportamiento de una sola vez y para que se reflejen en todas partes en una aplicación. Esto reduce la cantidad de código que necesario para escribir y hace que el código que escribe menos errores propensa a y fáciles de mantener.

La compatibilidad de validación proporcionada por ASP.NET MVC y Entity Framework Code First es un buen ejemplo del principio seco en acción. Puede especificar mediante declaración las reglas de validación en un solo lugar (en la clase del modelo) y las reglas se aplican en todas partes en la aplicación.

Echemos un vistazo a cómo puede aprovechar las ventajas de esta compatibilidad de validación en la aplicación de película.

## <a name="adding-validation-rules-to-the-movie-model"></a>Agregar reglas de validación para el modelo de película

Comenzará agregando alguna lógica de validación para el `Movie` clase.

Abra el archivo *Movie.cs*. Agregar un `using` instrucción en la parte superior del archivo que se hace referencia a la [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) espacio de nombres:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

Tenga en cuenta el espacio de nombres no contiene `System.Web`. DataAnnotations proporciona un conjunto integrado de atributos de validación que se pueden aplicar mediante declaración a cualquier clase o propiedad.

Ahora, actualice la `Movie` clase para aprovechar las ventajas de los integrados [ `Required` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), y [ `Range` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributos de validación . Utilice el código siguiente como ejemplo de dónde se debe aplicar los atributos.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

Ejecute la aplicación y vuelva a obtendrá el siguiente error de tiempo de ejecución:

***El modelo de realizar una copia del contexto de 'MovieDBContext' ha cambiado desde que se creó la base de datos. Considere la posibilidad de usar migraciones de Code First para actualizar la base de datos ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***

Las migraciones se usará para actualizar el esquema. Compile la solución y, a continuación, abra el **Package Manager Console** ventana y escriba los siguientes comandos:

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` clase derivada con el nombre especificado (*AddDataAnnotationsMig*) y en el `Up` método puede ver el código que actualice las restricciones de esquema. El `Title` y `Genre` campos ya no se admiten valores null (es decir, debe especificar un valor) y la `Rating` campo tiene una longitud máxima de 5.

Los atributos de validación especifican el comportamiento que quiere aplicar en las propiedades del modelo al que se aplican. El `Required` atributo indica que una propiedad debe tener un valor; en este ejemplo, una película que tienen valores para el `Title`, `ReleaseDate`, `Genre`, y `Price` propiedades para que sean válidos. El atributo `Range` restringe un valor a un intervalo determinado. El atributo `StringLength` permite establecer la longitud máxima de una propiedad de cadena y, opcionalmente, su longitud mínima. Tipos intrínsecos (como `decimal, int, float, DateTime`) se requiere de forma predeterminada y no necesita la `Required` atributo.

En primer lugar, código garantiza que se aplican las reglas de validación que se especifique en una clase de modelo antes de que la aplicación guarda los cambios en la base de datos. Por ejemplo, el código siguiente iniciará una excepción cuando la `SaveChanges` se denomina método, porque requiere varios `Movie` faltan valores de propiedad y el precio es cero (lo que está fuera del intervalo válido).

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

Tener reglas de validación se aplica automáticamente con .NET Framework ayuda a que la aplicación más sólida. También nos permite asegurarnos de que todo se valida y que no nos dejamos ningún dato incorrecto en la base de datos accidentalmente.

Esta es una lista de los archivos de código completa *Movie.cs* archivo:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a>Error de validación de interfaz de usuario en ASP.NET MVC

Vuelva a ejecutar la aplicación y navegue hasta la */Movies* dirección URL.

Haga clic en el **crear nuevo** el vínculo para agregar una película nuevo. Rellene el formulario con algunos valores no válidos y, a continuación, haga clic en el **crear** botón.

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> para admitir la validación de jQuery para configuraciones regionales no inglesas que usan una coma (&quot;,&quot;) para que un separador decimal, debe incluir *globalize.js* y específica de su *cultures/globalize.cultures.js* archivo (de [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) y JavaScript para usar `Globalize.parseFloat`. El código siguiente muestra las modificaciones en el archivo Views\Movies\Edit.cshtml para trabajar con la &quot;fr-FR&quot; referencia cultural:


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

Observe cómo el formulario automáticamente utiliza un color de borde de color rojo para resaltar los cuadros de texto que contienen datos no válidos y se genera un mensaje de error de validación adecuada junto a cada uno de ellos. Los errores se aplican en el lado cliente (con JavaScript y jQuery) y en el lado servidor (cuando un usuario tiene JavaScript deshabilitado).

Una ventaja real es que no necesita cambiar una sola línea de código en el `MoviesController` clase o en la *Create.cshtml* vista para permitir que esta interfaz de usuario de validación. El controlador y las vistas que creó en pasos anteriores de este tutorial seleccionaron automáticamente las reglas de validación que especificó mediante atributos de validación en las propiedades de la clase del modelo `Movie`.

Es posible que haya observado para las propiedades `Title` y `Genre`, el atributo requerido no se aplica hasta que se envía el formulario (alcanza el **crear** botón), o escribir texto en el campo de entrada y lo quitó. Para un campo que se muestra inicialmente vacío (por ejemplo, los campos en la vista de creación) y que tiene solo el atributo requerido y ningún otro atributo de validación, puede hacer lo siguiente para desencadenar la validación:

1. Pestaña en el campo.
2. Escriba algún texto.
3. Pestaña salida.
4. Pestaña en el campo.
5. Quite el texto.
6. Pestaña salida.

El orden anterior activará las validaciones requeridas sin utilizar el botón Enviar. Simplemente pulsando el botón de envío sin especificar cualquiera de los campos se activará la validación del lado cliente. Los datos del formulario no se envían al servidor hasta que no hay ningún error de validación del lado cliente. Para comprobarlo, colocar un punto de interrupción en el método HTTP Post o use la [herramienta fiddler](http://fiddler2.com/fiddler2/) o Internet Explorer 9 [herramientas de desarrollo F12](https://msdn.microsoft.com/en-us/ie/aa740478).

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Cómo se produce la validación en el crear, ver y crea método de acción

Tal vez se pregunte cómo se generó la validación de la interfaz de usuario sin actualizar el código en el controlador o las vistas. La lista siguiente muestra lo que hace el `Create` métodos en la `MovieController` clase aspecto. Se trata de no difieren en cómo haya creado anteriormente en este tutorial.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

El primer método de acción `Create` (HTTP GET) muestra el formulario de creación inicial. La segunda versión (`[HttpPost]`) controla el envío de formulario. El segundo método `Create` (la versión `HttpPost`) llama a `ModelState.IsValid` para comprobar si la película tiene errores de validación. Al llamar a este método se evalúan todos los atributos de validación que se hayan aplicado al objeto. Si el objeto tiene errores de validación, el método `Create` vuelve a mostrar el formulario. Si no hay ningún error, el método guarda la nueva película en la base de datos. En nuestro ejemplo de película que estamos usando, **el formulario no se envía al servidor cuando hay errores de validación que se ha detectado en el lado del cliente; el segundo** `Create` **nunca se llama al método**. Si deshabilita JavaScript en el explorador, se deshabilita la validación del cliente y HTTP POST `Create` llamadas al método `ModelState.IsValid` para comprobar si la película tiene errores de validación.

Puede establecer un punto de interrupción en el método `HttpPost Create` y comprobar si nunca se llama al método. La validación del lado cliente no enviará los datos del formulario cuando se detecten errores de validación. Si deshabilita JavaScript en el explorador y después envía el formulario con errores, se alcanzará el punto de interrupción. Puede seguir obteniendo validación completa sin JavaScript. La siguiente imagen muestra cómo deshabilitar JavaScript en Internet Explorer.

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

En la siguiente imagen se muestra cómo deshabilitar JavaScript en el explorador Firefox.

![](adding-validation-to-the-model/_static/image5.png)

La siguiente imagen muestra cómo deshabilitar JavaScript con el explorador Chrome.

![](adding-validation-to-the-model/_static/image6.png)

A continuación se muestra la *Create.cshtml* plantilla de vista de scaffolding anteriormente en el tutorial. Los métodos de acción que se muestran arriba la usan para mostrar el formulario inicial y para volver a mostrarlo en caso de error.

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

Observe cómo el código utiliza un `Html.EditorFor` auxiliar para generar el `<input>` para cada elemento `Movie` propiedad. Junto a esta aplicación auxiliar es una llamada a la `Html.ValidationMessageFor` método auxiliar. Estos dos métodos auxiliares trabajar con el objeto de modelo que se pasa por el controlador a la vista (en este caso, un `Movie` objeto). Se muestran automáticamente para los atributos de validación especificados en los mensajes de error de modelo y la presentación según corresponda.

¿Qué es excelente sobre este enfoque es que el controlador ni la plantilla de vista de crear sabe nada acerca de las reglas de validación real que se apliquen o acerca de los mensajes de error específicos que se muestran. Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`. Estas mismas reglas de validación se aplican automáticamente a la vista de edición y las otras vistas plantillas podría crear que editar el modelo.

Si desea cambiar la lógica de validación más adelante, puede hacerlo en exactamente un solo lugar mediante la adición de atributos de validación para el modelo (en este ejemplo, el `movie` clase). No tendrá que preocuparse de que diferentes partes de la aplicación sean incoherentes con el modo en que se aplican las reglas: toda la lógica de validación se definirá en un solo lugar y se usará en todas partes. Esto mantiene el código muy limpio y hace que sea fácil de mantener y evolucionar. Y significa que estará totalmente respetando el principio seco.

## <a name="adding-formatting-to-the-movie-model"></a>Si agrega un formato para el modelo de película

Abra el archivo *Movie.cs* y examine la clase `Movie`. El [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) espacio de nombres proporciona los atributos de formato además del conjunto de atributos de validación integrado. Ya hemos aplicado un [ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) valor de enumeración para la fecha de lanzamiento y los campos de precio. El código siguiente muestra el `ReleaseDate` y `Price` propiedades con el adecuado [ `DisplayFormat` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

El [ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) los atributos no son atributos de validación, se usan para indicar el motor de vista cómo representar el código HTML. En el ejemplo anterior, el `DataType.Date` atributo muestra las fechas de película como fechas, sin hora. Por ejemplo, la siguiente [ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atributos no validar el formato de los datos:

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

Los atributos mencionados anteriormente sólo proporcionan sugerencias para el motor de vista para dar formato a los datos (y proporcionar atributos como &lt;una&gt; para la dirección URL y &lt;un href =&quot;mailto:EmailAddress.com&quot; &gt; para correo electrónico. Puede usar el [RegularExpression](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atributo para validar el formato de los datos.

Un enfoque alternativo a usar el `DataType` atributos, puede establecer explícitamente un [ `DataFormatString` ](https://msdn.microsoft.com/en-us/library/system.string.format.aspx) valor. El código siguiente muestra la propiedad de fecha de lanzamiento con una cadena de formato de fecha (es decir, &quot;d.&quot;). Se utilizaría para especificar que no desea tiempo como parte de la fecha de lanzamiento.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

La sección completa `Movie` clase se muestra a continuación.

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

Ejecute la aplicación y busque el `Movies` controlador. Bien se da formato a la fecha de lanzamiento y el precio. La imagen siguiente muestra la fecha de lanzamiento y de precios mediante &quot;fr-FR&quot; como la referencia cultural.

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

La imagen siguiente muestra los mismos datos que se muestran con la referencia cultural predeterminada (inglés Estados Unidos).

![](adding-validation-to-the-model/_static/image8.png)

En la siguiente parte de la serie de tutoriales, revisaremos la aplicación y realizaremos algunas mejoras a los métodos `Details` y `Delete` generados automáticamente.

>[!div class="step-by-step"]
[Anterior](adding-a-new-field-to-the-movie-model-and-table.md)
[Siguiente](examining-the-details-and-delete-methods.md)
