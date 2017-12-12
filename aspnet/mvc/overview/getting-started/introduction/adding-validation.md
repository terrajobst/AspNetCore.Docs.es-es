---
uid: mvc/overview/getting-started/introduction/adding-validation
title: "Agregar validación | Documentos de Microsoft"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 9f35ca15-e216-4db6-9ebf-24380b0f31b4
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-validation
msc.type: authoredcontent
ms.openlocfilehash: db09b6c947b219ce21e8f4248dcc6629c6add76c
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/19/2017
---
<a name="adding-validation"></a>Agregar una validación
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

En esto en esta sección, se agregará una lógica de validación para el `Movie` modelo y deberá asegurarse de que las reglas de validación se aplican siempre que un usuario intenta crear o editar una película con la aplicación.

## <a name="keeping-things-dry"></a>Mantener las cosas SECA

Uno de los principios de diseño principales de ASP.NET MVC es [SECA](http://en.wikipedia.org/wiki/Don't_repeat_yourself) (&quot;no repitas&quot;). ASP.NET MVC le anima a especificar funcionalidad o comportamiento de una sola vez y para que se reflejen en todas partes en una aplicación. Esto reduce la cantidad de código que necesario para escribir y hace que el código que escribe menos errores propensa a y fáciles de mantener.

La compatibilidad de validación proporcionada por ASP.NET MVC y Entity Framework Code First es un buen ejemplo del principio seco en acción. Puede especificar mediante declaración las reglas de validación en un solo lugar (en la clase del modelo) y las reglas se aplican en todas partes en la aplicación.

Echemos un vistazo a cómo puede aprovechar las ventajas de esta compatibilidad de validación en la aplicación de película.

## <a name="adding-validation-rules-to-the-movie-model"></a>Agregar reglas de validación para el modelo de película

Comenzará agregando alguna lógica de validación para el `Movie` clase.

Abra el archivo *Movie.cs*. Observe el [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) espacio de nombres no contiene `System.Web`. DataAnnotations proporciona un conjunto integrado de atributos de validación que se pueden aplicar mediante declaración a cualquier clase o propiedad. (También contiene atributos de formato como [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) que ayuda con el formato y no proporciona ninguna validación.)

Ahora, actualice la `Movie` clase para aprovechar las ventajas de los integrados [ `Required` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx), [ `StringLength` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), [RegularExpression](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx), y [ `Range` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributos de validación. Reemplace el `Movie` clase por lo siguiente:

[!code-csharp[Main](adding-validation/samples/sample1.cs?highlight=8,22-24,30-31,37-38)]

El [ `StringLength` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx) atributo establece la longitud máxima de la cadena y establece esta limitación en la base de datos, por lo tanto, se cambiará el esquema de base de datos. Haga clic con el botón secundario en el **películas** tabla **Explorador de servidores** y haga clic en **Abrir definición de tabla**:

![](adding-validation/_static/image1.png)

En la imagen anterior, puede ver todos los campos de cadena se establecen en [NVARCHAR (MAX)](https://technet.microsoft.com/en-us/library/ms186939.aspx). Las migraciones se usará para actualizar el esquema. Compile la solución y, a continuación, abra el **Package Manager Console** ventana y escriba los siguientes comandos:

[!code-console[Main](adding-validation/samples/sample2.cmd)]

Cuando finaliza este comando, Visual Studio abre el archivo de clase que define la nueva `DbMIgration` clase derivada con el nombre especificado (`DataAnnotations`) y en el `Up` método también puede ver el código que actualiza las restricciones de esquema:

[!code-csharp[Main](adding-validation/samples/sample3.cs)]

El `Genre` es el campo ya no se admiten valores null (es decir, debe especificar un valor). El `Rating` campo tiene una longitud máxima de 5 y `Title` tiene una longitud máxima de 60. La longitud mínima de 3 en `Title` y el intervalo de `Price` no se ha creado los cambios de esquema.

Examine el esquema de la película:

![](adding-validation/_static/image2.png)

Los campos de cadena muestran los nuevos límites de longitud y `Genre` ya no se comprueba como que acepta valores NULL.

Los atributos de validación especifican el comportamiento que quiere aplicar en las propiedades del modelo al que se aplican. Los atributos `Required` y `MinimumLength` indican que una propiedad debe tener un valor, pero nada evita que un usuario escriba espacios en blanco para satisfacer esta validación. El [RegularExpression](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atributo se utiliza para limitar los caracteres que pueden ser de entrada. En el código anterior, `Genre` y `Rating` solamente pueden usar letras (no se permiten espacios en blanco, números ni caracteres especiales). El [ `Range` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributo restringe un valor para dentro de un intervalo especificado. El atributo `StringLength` permite establecer la longitud máxima de una propiedad de cadena y, opcionalmente, su longitud mínima. Los tipos de valor (como `decimal, int, float, DateTime`) son intrínsecamente necesarios y no es necesario el `Required` atributo.

En primer lugar, código garantiza que se aplican las reglas de validación que se especifique en una clase de modelo antes de que la aplicación guarda los cambios en la base de datos. Por ejemplo, el código siguiente producirá un [DbEntityValidationException](https://msdn.microsoft.com/en-us/library/system.data.entity.validation.dbentityvalidationexception(v=vs.103).aspx) excepción cuando la `SaveChanges` se denomina método, porque requiere varios `Movie` faltan valores de propiedad:

[!code-csharp[Main](adding-validation/samples/sample4.cs)]

El código anterior produce la excepción siguiente:

*Error de validación de una o más entidades. Vea la propiedad 'EntityValidationErrors' para obtener más detalles.*

Tener reglas de validación se aplica automáticamente con .NET Framework ayuda a que la aplicación más sólida. También nos permite asegurarnos de que todo se valida y que no nos dejamos ningún dato incorrecto en la base de datos accidentalmente.

## <a name="validation-error-ui-in-aspnet-mvc"></a>Error de validación de interfaz de usuario en ASP.NET MVC

Ejecute la aplicación y navegue hasta la */Movies* dirección URL.

Haga clic en el **crear nuevo** el vínculo para agregar una película nuevo. Rellene el formulario con algunos valores no válidos. En cuanto la validación del lado cliente de jQuery detecta el problema, muestra un mensaje de error.

![8_validationErrors](adding-validation/_static/image3.png)

> [!NOTE]
> para admitir la validación de jQuery para configuraciones regionales no inglesas que usan una coma (",") para que un separador decimal, debe incluir NuGet globalizar como se describió anteriormente en este tutorial.


Observe cómo el formulario automáticamente utiliza un color de borde de color rojo para resaltar los cuadros de texto que contienen datos no válidos y se genera un mensaje de error de validación adecuada junto a cada uno de ellos. Los errores se aplican en el lado cliente (con JavaScript y jQuery) y en el lado servidor (cuando un usuario tiene JavaScript deshabilitado).

Una ventaja real es que no necesita cambiar una sola línea de código en el `MoviesController` clase o en la *Create.cshtml* vista para permitir que esta interfaz de usuario de validación. El controlador y las vistas que creó en pasos anteriores de este tutorial seleccionaron automáticamente las reglas de validación que especificó mediante atributos de validación en las propiedades de la clase del modelo `Movie`. Pruebe la aplicación mediante el método de acción `Edit` y se aplicará la misma validación.

Los datos del formulario no se envían al servidor hasta que no hay ningún error de validación del lado cliente. Puede comprobarlo colocando un punto de interrupción en el método HTTP Post, mediante el uso de la [herramienta fiddler](http://fiddler2.com/fiddler2/), o IE [herramientas de desarrollo F12](https://msdn.microsoft.com/en-us/ie/aa740478).

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a>Cómo se produce la validación en el crear, ver y crea método de acción

Tal vez se pregunte cómo se generó la validación de la interfaz de usuario sin actualizar el código en el controlador o las vistas. La lista siguiente muestra lo que hace el `Create` métodos en la `MovieController` clase aspecto. Se trata de no difieren en cómo haya creado anteriormente en este tutorial.

[!code-csharp[Main](adding-validation/samples/sample5.cs)]

El primer método de acción `Create` (HTTP GET) muestra el formulario de creación inicial. La segunda versión (`[HttpPost]`) controla el envío de formulario. El segundo método `Create` (la versión `HttpPost`) llama a `ModelState.IsValid` para comprobar si la película tiene errores de validación. Al llamar a este método se evalúan todos los atributos de validación que se hayan aplicado al objeto. Si el objeto tiene errores de validación, el método `Create` vuelve a mostrar el formulario. Si no hay ningún error, el método guarda la nueva película en la base de datos. En nuestro ejemplo de película, **el formulario no se envía al servidor cuando hay errores de validación que se ha detectado en el lado del cliente; el segundo** `Create` **nunca se llama al método**. Si deshabilita JavaScript en el explorador, se deshabilita la validación del cliente y HTTP POST `Create` llamadas al método `ModelState.IsValid` para comprobar si la película tiene errores de validación.

Puede establecer un punto de interrupción en el método `HttpPost Create` y comprobar si nunca se llama al método. La validación del lado cliente no enviará los datos del formulario cuando se detecten errores de validación. Si deshabilita JavaScript en el explorador y después envía el formulario con errores, se alcanzará el punto de interrupción. Puede seguir obteniendo validación completa sin JavaScript. La siguiente imagen muestra cómo deshabilitar JavaScript en Internet Explorer.

![](adding-validation/_static/image5.png)

![](adding-validation/_static/image6.png)

En la siguiente imagen se muestra cómo deshabilitar JavaScript en el explorador Firefox.

![](adding-validation/_static/image7.png)

En la siguiente imagen se muestra cómo deshabilitar JavaScript en el explorador Chrome.

![](adding-validation/_static/image8.png)

A continuación se muestra la *Create.cshtml* plantilla de vista de scaffolding anteriormente en el tutorial. Los métodos de acción que se muestran arriba la usan para mostrar el formulario inicial y para volver a mostrarlo en caso de error.

[!code-cshtml[Main](adding-validation/samples/sample6.cshtml?highlight=16-17)]

Observe cómo el código utiliza un `Html.EditorFor` auxiliar para generar el `<input>` para cada elemento `Movie` propiedad. Junto a esta aplicación auxiliar es una llamada a la `Html.ValidationMessageFor` método auxiliar. Estos dos métodos auxiliares trabajar con el objeto de modelo que se pasa por el controlador a la vista (en este caso, un `Movie` objeto). Se muestran automáticamente para los atributos de validación especificados en los mensajes de error de modelo y la presentación según corresponda.

Lo realmente bueno de este enfoque es que ni el controlador ni la plantilla de vista `Create` saben que las reglas de validación actuales se están aplicando ni conocen los mensajes de error específicos que se muestran. Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`. Estas mismas reglas de validación se aplican automáticamente a la vista `Edit` y a cualquier otra vista de plantillas creada que edite el modelo.

Si desea cambiar la lógica de validación más adelante, puede hacerlo en exactamente un solo lugar mediante la adición de atributos de validación para el modelo (en este ejemplo, el `movie` clase). No tendrá que preocuparse de que diferentes partes de la aplicación sean incoherentes con el modo en que se aplican las reglas: toda la lógica de validación se definirá en un solo lugar y se usará en todas partes. Esto mantiene el código muy limpio y hace que sea fácil de mantener y evolucionar. Y significa que deberá ser totalmente respetando la *SECA* principio.

## <a name="using-datatype-attributes"></a>Uso de atributos DataType

Abra el archivo *Movie.cs* y examine la clase `Movie`. El [ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) espacio de nombres proporciona los atributos de formato además del conjunto de atributos de validación integrado. Ya hemos aplicado un [ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) valor de enumeración para la fecha de lanzamiento y los campos de precio. El código siguiente muestra el `ReleaseDate` y `Price` propiedades con el adecuado [ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) atributo.

[!code-csharp[Main](adding-validation/samples/sample7.cs)]

El [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributos sólo proporcionan sugerencias para el motor de vista para dar formato a los datos (y proporcionar atributos como `<a>` para la dirección URL y `<a href="mailto:EmailAddress.com">` para correo electrónico. Puede usar el [RegularExpression](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) atributo para validar el formato de los datos. El [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo se utiliza para especificar un tipo de datos que es más específico que el tipo intrínseco de base de datos, son ***no*** atributos de validación. En este caso sólo desea realizar un seguimiento de la fecha, no la fecha y hora. El [enumeración DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) proporciona para muchos tipos de datos, como *fecha, hora, número de teléfono, moneda, EmailAddress* y mucho más. El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo. Por ejemplo, un `mailto:` vínculo se puede crear para [DataType.EmailAddress](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx), y no se puede proporcionar un selector de fecha para [DataType.Date](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) en exploradores que admiten [HTML5](http://html5.org/). El [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributos emite HTML 5 [datos -](http://ejohn.org/blog/html-5-data-attributes/) (pronunciado *guión datos*) atributos que pueden entender exploradores HTML 5. El [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributos no proporcionan ninguna validación.

`DataType.Date` no especifica el formato de la fecha que se muestra. De forma predeterminada, se muestra el campo de datos según los formatos predeterminados en función del servidor[CultureInfo](https://msdn.microsoft.com/en-us/library/vstudio/system.globalization.cultureinfo(v=vs.110).aspx).

El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:


[!code-csharp[Main](adding-validation/samples/sample8.cs)]


El `ApplyFormatInEditMode` configuración especifica que el formato especificado debe también se aplica cuando el valor se muestra en un cuadro de texto para su edición. (Puede no ser conveniente que para algunos campos, por ejemplo, para los valores de moneda, podría no desea que el símbolo de moneda en el cuadro de texto para su edición.)

Puede usar el [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) atributo por sí mismo, pero suele ser una buena idea usar el [DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo también. El `DataType` atributo transmite la *semántica* de los datos como contraposición a cómo representar en una pantalla y ofrece las siguientes ventajas que no obtendrá con `DisplayFormat`:

- El explorador puede habilitar características de HTML5 (por ejemplo mostrar un control de calendario, el símbolo de moneda adecuado para la configuración regional, vínculos de correo electrónico, etcetera.).
- De forma predeterminada, el explorador representará los datos con el formato correcto en función de su[configuración regional](https://msdn.microsoft.com/en-us/library/vstudio/wyzd2bce.aspx).
- El[DataType](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatypeattribute.aspx) atributo puede habilitar MVC elegir la plantilla de campo a la derecha para representar los datos (la [DisplayFormat](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) si usa por sí mismo utiliza la plantilla de cadena). Para obtener más información, consulte de Brad Wilson[ASP.NET MVC 2 Templates](http://bradwilson.typepad.com/blog/2009/10/aspnet-mvc-2-templates-part-1-introduction.html). (Aunque redactado para MVC 2, este artículo todavía se aplica a la versión actual de ASP.NET MVC.)

Si usas el `DataType` atributo con un campo de fecha, tendrá que especificar el `DisplayFormat` atributo también con el fin de asegurarse de que el campo se representa correctamente en los exploradores de Chrome. Para obtener más información, consulte [este subproceso StackOverflow](http://stackoverflow.com/questions/12633471/mvc4-datatype-date-editorfor-wont-display-date-value-in-chrome-fine-in-ie).

> [!NOTE]
> validación de jQuery no funciona con la[intervalo](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributo y[DateTime](https://msdn.microsoft.com/en-us/library/system.datetime.aspx). Por ejemplo, el código siguiente siempre muestra un error de validación del lado cliente, incluso cuando la fecha está en el intervalo especificado:
> 
> [!code-csharp[Main](adding-validation/samples/sample9.cs)]
> 
> Debe deshabilitar la validación de la fecha de jQuery para usar el [intervalo](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) atribuir a[DateTime](https://msdn.microsoft.com/en-us/library/system.datetime.aspx). Por lo general no es una buena práctica para compilar las fechas de disco duras en los modelos, por lo que usar el[intervalo](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) atributo y[DateTime](https://msdn.microsoft.com/en-us/library/system.datetime.aspx) se desaconseja.


El código siguiente muestra la combinación de atributos en una línea:

[!code-csharp[Main](adding-validation/samples/sample10.cs?highlight=6,10)]

En la siguiente parte de la serie de tutoriales, revisaremos la aplicación y realizaremos algunas mejoras a los métodos `Details` y `Delete` generados automáticamente.

>[!div class="step-by-step"]
[Anterior](adding-a-new-field.md)
[Siguiente](examining-the-details-and-delete-methods.md)
