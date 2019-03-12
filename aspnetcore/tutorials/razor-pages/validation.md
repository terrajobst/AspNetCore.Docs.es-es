---
title: Agregar la validación a una página de Razor de ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo agregar validación a una página de Razor de ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 961b41a63a5dd066a51468239bd855b7f723a3ae
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346052"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a>Agregar la validación a una página de Razor de ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

En esta sección se agrega lógica de validación al modelo `Movie`. Las reglas de validación se aplican cada vez que un usuario crea o edita una película.

## <a name="validation"></a>Validación

Un principio clave de desarrollo de software se denomina [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself), por "**D**on't **R**epeat **Y**ourself" (Una vez y solo una). Las páginas de Razor fomentan un tipo de desarrollo en el que la funcionalidad se especifica una vez y se refleja en la aplicación. DRY puede ayudar a:

* Reducir la cantidad de código en una aplicación.
* Hacer que el código sea menos propenso a errores y resulte más fácil de probar y mantener.

La compatibilidad de validación proporcionada por las páginas de Razor y Entity Framework es un buen ejemplo del principio DRY. Las reglas de validación se especifican mediante declaración en un solo lugar (en la clase del modelo) y se aplican en toda la aplicación.

### <a name="adding-validation-rules-to-the-movie-model"></a>Adición de reglas de validación al modelo de película

Abra el archivo *Models/Movie.cs*. [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) proporciona un conjunto integrado de atributos de validación que se aplican mediante declaración a una clase o propiedad. DataAnnotations también contiene atributos de formato como `DataType` que ayudan a aplicar formato y no proporcionan validación.

Actualice la clase `Movie` para aprovechar los atributos de validación `Required`, `StringLength`, `RegularExpression` y `Range`.

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRatingDA.cs?name=snippet1)]

Los atributos de validación especifican el comportamiento que se aplica a las propiedades del modelo:

* Los atributos `Required` y `MinimumLength` indican que una propiedad debe tener un valor. Aun así, nada impide que el usuario escriba un espacio en blanco para cumplir la restricción de validación de un tipo que acepta valores NULL. Los [tipos de valor](/dotnet/csharp/language-reference/keywords/value-types) que no aceptan valores NULL (como `decimal`, `int`, `float` y `DateTime`) son intrínsecamente necesarios y no necesitan el atributo `Required`.
* El atributo `RegularExpression` limita los caracteres que el usuario puede escribir. En el código anterior, `Genre` debe comenzar por una o varias letras en mayúscula y continuar con cero o más letras, comillas simples o dobles, caracteres de espacio en blanco o guiones. `Rating` debe comenzar por una o varias letras en mayúscula y continuar con cero o más letras, números, comillas simples o dobles, caracteres de espacio en blanco o guiones.
* El atributo `Range` restringe un valor a un intervalo determinado.
* El atributo `StringLength` establece la longitud máxima de una cadena y, de forma opcional, la longitud mínima. 

El que ASP.NET Core aplique automáticamente las reglas de validación ayuda a que una aplicación sea más sólida. La validación automática en modelos ayuda a proteger la aplicación, ya que no hay que acordarse de su aplicación cuando se agrega nuevo código.

### <a name="validation-error-ui-in-razor-pages"></a>Interfaz de usuario de error de validación en páginas de Razor

Ejecute la aplicación y vaya a Pages/Movies.

Seleccione el vínculo **Crear nuevo**. Rellene el formulario con algunos valores no válidos. Cuando la validación de cliente de jQuery detecta el error, muestra un mensaje de error.

![Formulario de vista de película con varios errores de validación de cliente de jQuery](validation/_static/val.png)

[!INCLUDE[](~/includes/currency.md)]

Observe cómo el formulario presenta automáticamente un mensaje de error de validación en cada campo que contiene un valor no válido. Los errores se aplican al cliente (con JavaScript y jQuery) y al servidor (cuando un usuario tiene JavaScript deshabilitado).

Una ventaja importante es que **no** se han necesitado cambios de código en las páginas de creación o edición. Una vez que DataAnnotations se ha aplicado al modelo, la interfaz de usuario de validación se ha habilitado. Las páginas de Razor creadas en este tutorial han obtenido automáticamente las reglas de validación (mediante atributos de validación en las propiedades de la clase del modelo `Movie`). Al probar la validación en la página de edición, se aplica la misma validación.

Los datos del formulario no se publicarán en el servidor hasta que dejen de producirse errores de validación de cliente. Compruebe que los datos del formulario no se publican mediante uno o varios de los métodos siguientes:

* Coloque un punto de interrupción en el método `OnPostAsync`. Envíe el formulario (seleccione **Crear** o **Guardar**). El punto de interrupción nunca se alcanza.
* Use la [herramienta Fiddler](http://www.telerik.com/fiddler).
* Use las herramientas de desarrollo del explorador para supervisar el tráfico de red.

### <a name="server-side-validation"></a>Validación de servidor

Cuando JavaScript está deshabilitado en el explorador, si se envía el formulario con errores, se publica en el servidor.

Validación de servidor de prueba opcional:

* Deshabilite JavaScript en el explorador. Puede hacerlo con las herramientas para desarrolladores del explorador. Si no se puede deshabilitar JavaScript en el explorador, pruebe con otro explorador.
* Establezca un punto de interrupción en el método `OnPostAsync` de la página de creación o edición.
* Envíe un formulario con errores de validación.
* Compruebe que el estado del modelo no es válido:

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```

El código siguiente muestra una parte de la página *Create.cshtml* a la que se ha aplicado scaffolding anteriormente en el tutorial. Las páginas de creación y edición la usan para mostrar el formulario inicial y para volver a mostrar el formulario en caso de error.

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

El [asistente de etiquetas de entrada](xref:mvc/views/working-with-forms) usa los atributos [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) y genera los atributos HTML necesarios para la validación de jQuery en el cliente. El [asistente de etiquetas de validación](xref:mvc/views/working-with-forms#the-validation-tag-helpers) muestra errores de validación. Para más información, vea [Validación](xref:mvc/models/validation).

Las páginas de creación y edición no tienen ninguna regla de validación. Las reglas de validación y las cadenas de error solo se especifican en la clase `Movie`. Estas reglas de validación se aplican automáticamente a las páginas de Razor que editan el modelo `Movie`.

Cuando es necesario modificar la lógica de validación, se hace únicamente en el modelo. La validación se aplica de forma coherente en toda la aplicación (la lógica de validación se define en un solo lugar). La validación en un solo lugar ayuda a mantener limpio el código y facilita su mantenimiento y actualización.

## <a name="using-datatype-attributes"></a>Uso de atributos DataType

Examine la clase `Movie`. El espacio de nombres `System.ComponentModel.DataAnnotations` proporciona atributos de formato además del conjunto integrado de atributos de validación. El atributo `DataType` se aplica a las propiedades `ReleaseDate` y `Price`.

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

Los atributos `DataType` solo proporcionan sugerencias para que el motor de vista aplique formato a los datos (y ofrece atributos como `<a>` para las direcciones URL y `<a href="mailto:EmailAddress.com">` para el correo electrónico). Use el atributo `RegularExpression` para validar el formato de los datos. El atributo `DataType` se usa para especificar un tipo de datos más específico que el tipo intrínseco de base de datos. Los atributos `DataType` no son atributos de validación. En la aplicación de ejemplo solo se muestra la fecha, sin hora.

La enumeración `DataType` proporciona muchos tipos de datos, como Date, Time, PhoneNumber, Currency, EmailAddress, etc. El atributo `DataType` también puede permitir que la aplicación proporcione automáticamente características específicas del tipo. Por ejemplo, se puede crear un vínculo `mailto:` para `DataType.EmailAddress`. Se puede proporcionar un selector de fecha para `DataType.Date` en exploradores compatibles con HTML5. Los atributos `DataType` emiten atributos HTML 5 `data-` (se pronuncia "datos dash") para su uso por parte de los exploradores HTML 5. Los atributos `DataType` **no** proporcionan ninguna validación.

`DataType.Date` no especifica el formato de la fecha que se muestra. De manera predeterminada, el campo de datos se muestra según los formatos predeterminados basados en el elemento `CultureInfo` del servidor.


La anotación de datos `[Column(TypeName = "decimal(18, 2)")]` es necesaria para que Entity Framework Core asigne correctamente `Price` a la moneda en la base de datos. Para más información, vea [Tipos de datos](/ef/core/modeling/relational/data-types).

El atributo `DisplayFormat` se usa para especificar el formato de fecha de forma explícita:

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

El valor `ApplyFormatInEditMode` especifica que el formato se debe aplicar cuando el valor se muestra para su edición. Es posible que no quiera ese comportamiento para algunos campos. Por ejemplo, en los valores de moneda, probablemente no quiera el símbolo de moneda en la interfaz de usuario de edición.

El atributo `DisplayFormat` puede usarse por sí mismo, pero normalmente es buena idea usar el atributo `DataType`. El atributo `DataType` transmite la semántica de los datos en contraposición a cómo se representan en una pantalla y ofrece las siguientes ventajas que no proporciona DisplayFormat:

* El explorador puede habilitar características de HTML5 (por ejemplo, mostrar un control de calendario, el símbolo de moneda adecuado según la configuración regional, vínculos de correo electrónico, etc.).
* De forma predeterminada, el explorador presenta los datos con el formato correcto en función de la configuración regional.
* El atributo `DataType` puede habilitar el marco de trabajo de ASP.NET Core para elegir la plantilla de campo correcta a fin de presentar los datos. `DisplayFormat`, si se emplea por sí mismo, usa la plantilla de cadena.

Nota: La validación de jQuery no funciona con el atributo `Range` ni `DateTime`. Por ejemplo, el código siguiente siempre muestra un error de validación de cliente, incluso cuando la fecha está en el intervalo especificado:

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

Por lo general no se recomienda compilar fechas fijas en los modelos, así que se desaconseja el empleo del atributo `Range` y `DateTime`.

El código siguiente muestra la combinación de atributos en una línea:

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRatingDAmult.cs?name=snippet1)]

En [Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) (Introducción a Razor Pages y EF Core) se muestran operaciones avanzadas de EF Core con Razor Pages.

### <a name="publish-to-azure"></a>Publicar en Azure

Para obtener información sobre la implementación en Azure, consulte [Tutorial: Compilación de una aplicación ASP.NET en Azure con SQL Database](/azure/app-service/app-service-web-tutorial-dotnet-sqldatabase). Estas instrucciones son para una aplicación ASP.NET, no para una aplicación ASP.NET Core, pero los pasos son los mismos.

Gracias por seguir esta introducción a las páginas de Razor. [Introducción a MVC con Razor Pages y EF Core](xref:data/ef-rp/intro) es un excelente artículo de seguimiento de este tutorial.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [Versión en YouTube de este tutorial](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> [Anterior: ](xref:tutorials/razor-pages/new-field)Adición de un nuevo campo
