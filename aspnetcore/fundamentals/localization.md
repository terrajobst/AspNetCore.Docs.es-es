---
title: "Globalización y localización en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo ASP.NET Core proporciona servicios y middleware para localizar el contenido en diferentes idiomas y referencias culturales."
keywords: "ASP.NET Core, localización, referencia cultural, idioma, archivo de recursos, globalización, internacionalización, configuración regional"
ms.author: riande
manager: wpickett
ms.date: 01/14/2017
ms.topic: article
ms.assetid: 7f275a09-f118-41c9-88d1-8de52d6a5aa1
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/localization
ms.openlocfilehash: a3fdbf8a1ab4ca397824a46da445fa34ddd35204
ms.sourcegitcommit: 4be61844141d3cfb6f263636a36aebd26e90fb28
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 12/22/2017
---
# <a name="globalization-and-localization-in-aspnet-core"></a>Globalización y localización en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Damien Bowden](https://twitter.com/damien_bod), [Bart Calixto](https://twitter.com/bartmax), [Nadeem Afana](https://twitter.com/NadeemAfana), y [Ateya Hisham Bin](https://twitter.com/hishambinateya)

Crear un sitio Web multilingüe con ASP.NET Core permitirá que su sitio llegar a una audiencia mayor. ASP.NET Core proporciona servicios y software intermedio para la localización en diferentes idiomas y referencias culturales.

Internacionalización implica [globalización](https://docs.microsoft.com/dotnet/api/system.globalization) y [localización](https://docs.microsoft.com/dotnet/standard/globalization-localization/localization). La globalización es el proceso de diseño de las aplicaciones que admiten distintas referencias culturales. Globalización agrega compatibilidad para entrada, visualización y salida de un conjunto definido de secuencias de comandos de lenguaje que se relacionan a áreas geográficas específicas.

Localización es el proceso de adaptar una aplicación globalizada, que ya se ha procesado de localizabilidad, para una determinada referencia cultural o configuración regional.  Para obtener más información, consulte **términos de globalización y localización** cerca del final de este documento.

Localización de la aplicación implica lo siguiente:

1. Hacer que el contenido de la aplicación localizable

2. Proporcionar recursos localizados para los idiomas y referencias culturales que admite

3. Implementar una estrategia para seleccionar el idioma o referencia cultural para cada solicitud

## <a name="make-the-apps-content-localizable"></a>Hacer que el contenido de la aplicación localizable

Introdujo en ASP.NET Core, `IStringLocalizer` y `IStringLocalizer<T>` se han diseñado para mejorar la productividad al desarrollar localizada aplicaciones. `IStringLocalizer`usa el [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager) y [ResourceReader](https://docs.microsoft.com/dotnet/api/system.resources.resourcereader) para proporcionar recursos específicos de la referencia cultural en tiempo de ejecución. La interfaz simple tiene un indizador y un `IEnumerable` para devolver las cadenas localizadas. `IStringLocalizer`no necesita almacenar las cadenas de idioma predeterminado en un archivo de recursos. Puede desarrollar una aplicación de destino para la localización y no se necesita crear archivos de recursos al principio de desarrollo. El código siguiente muestra cómo ajustar la cadena "Sobre el título" para la localización.

[!code-csharp[Main](localization/sample/Localization/Controllers/AboutController.cs)]

En el código anterior, el `IStringLocalizer<T>` implementación procede de [inyección de dependencia](dependency-injection.md). Si no se encuentra el valor localizado de "Sobre el título", se devuelve la clave de indizador, es decir, la cadena "Sobre el título". Puede dejar el valor predeterminado de cadenas literales de idioma en la aplicación y ajustarlas en el localizador, para que pueda centrarse en desarrollar la aplicación. Desarrollar la aplicación con el idioma predeterminado y prepararlo para el paso de localización sin crear primero un archivo de recursos predeterminado. Como alternativa, puede usar el enfoque tradicional y proporcionar una clave para recuperar la cadena de idioma predeterminado. Para muchos desarrolladores el nuevo flujo de trabajo por no disponer de un idioma predeterminado *.resx* archivo y sencillamente se ajustó los literales de cadena pueden reducir la sobrecarga de localizar una aplicación. Otros desarrolladores preferirán el flujo de trabajo tradicional, tal y como se puede hacer más fácil trabajar con literales de cadena más largos y que resulten más fáciles de actualizar las cadenas localizadas.

Use la `IHtmlLocalizer<T>` implementación para los recursos que contienen HTML. `IHtmlLocalizer`Argumentos que se da formato a la cadena de recurso codifican en HTML, pero no HTML no codifica la cadena de recurso propio. En el ejemplo de resaltado a continuación, solo el valor de `name` parámetro está codificado en HTML.

[!code-csharp[Main](../fundamentals/localization/sample/Localization/Controllers/BookController.cs?highlight=3,5,20&start=1&end=24)]

**Nota:** generalmente desea localizar sólo texto y no HTML.

En el nivel más bajo, puede obtener `IStringLocalizerFactory` de [inyección de dependencia](dependency-injection.md):

[!code-csharp[Main](localization/sample/Localization/Controllers/TestController.cs?start=9&end=26&highlight=7-13)]

El código anterior muestra cada uno de los dos generador crear métodos.

Puede dividir las cadenas localizadas mediante el controlador, área, o tiene un solo contenedor. En la aplicación de ejemplo, una clase ficticia denominada `SharedResource` se utiliza para los recursos compartidos.

[!code-csharp[Main](localization/sample/Localization/Resources/SharedResource.cs)]

Algunos programadores utilizan la `Startup` clase para contener cadenas globales o compartidas. En el ejemplo siguiente, la `InfoController` y `SharedResource` se utilizan los localizadores:

[!code-csharp[Main](localization/sample/Localization/Controllers/InfoController.cs?range=9-26)]

## <a name="view-localization"></a>Localización de vista

El `IViewLocalizer` servicio proporciona las cadenas adaptadas para un [vista](https://docs.microsoft.com/aspnet/core). La `ViewLocalizer` clase implementa esta interfaz y busca la ubicación del recurso de la ruta de acceso del archivo de vista. El código siguiente muestra cómo utilizar la implementación predeterminada de `IViewLocalizer`:

[!code-cshtml[Main](localization/sample/Localization/Views/Home/About.cshtml)]

La implementación predeterminada de `IViewLocalizer` busca el archivo de recursos basado en nombre de archivo de la vista. No hay ninguna opción para utilizar un archivo de recurso compartido global. `ViewLocalizer`implementa el localizador mediante `IHtmlLocalizer`, por lo que Razor no HTML codificar la cadena adaptada. Se pueden parametrizar las cadenas de recursos y `IViewLocalizer` HTML codificará los parámetros, pero no la cadena de recurso. Tenga en cuenta el siguiente marcado de Razor:

```cshtml
@Localizer["<i>Hello</i> <b>{0}!</b>", UserManager.GetUserName(User)]
```

Un archivo de recursos en francés podría contener lo siguiente:

| Key | Valor |
| ----- | ------ |
| `<i>Hello</i> <b>{0}!</b>` | `<i>Bonjour</i> <b>{0} !</b> ` |

La vista representada contendría el marcado HTML desde el archivo de recursos.

**Nota:** generalmente desea localizar sólo texto y no HTML.

Para usar un archivo de recurso compartido en una vista, insertar `IHtmlLocalizer<T>`:

[!code-cshtml[Main](../fundamentals/localization/sample/Localization/Views/Test/About.cshtml?highlight=5,12)]

## <a name="dataannotations-localization"></a>Localización de DataAnnotations

Los mensajes de error de DataAnnotations están localizados con `IStringLocalizer<T>`. Mediante la opción `ResourcesPath = "Resources"`, mensajes de error en `RegisterViewModel` pueden almacenarse en cualquiera de las rutas de acceso siguientes:

* Resources/ViewModels.Account.RegisterViewModel.fr.resx
* Resources/ViewModels/Account/RegisterViewModel.fr.resx

[!code-csharp[Main](localization/sample/Localization/ViewModels/Account/RegisterViewModel.cs?start=9&end=26)]

Están localizados en MVC de ASP.NET Core 1.1.0 y atributos alta, no la validación. Núcleo de ASP.NET MVC 1.0 **no** buscar cadenas traducidas para atributos de no validación.

<a name="one-resource-string-multiple-classes"></a>
### <a name="using-one-resource-string-for-multiple-classes"></a>Utilizando una cadena de recurso para varias clases

El código siguiente muestra cómo utilizar una cadena de recursos para los atributos de validación con varias clases:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc()
        .AddDataAnnotationsLocalization(options => {
            options.DataAnnotationLocalizerProvider = (type, factory) =>
                factory.Create(typeof(SharedResource));
        });
}
```

En el código anterior, `SharedResource` es la clase correspondiente a la resx donde se almacenan los mensajes de validación. Con este enfoque, sólo utilizará DataAnnotations `SharedResource`, en lugar de los recursos para cada clase. 

## <a name="provide-localized-resources-for-the-languages-and-cultures-you-support"></a>Proporcionar recursos localizados para los idiomas y referencias culturales que admite  

### <a name="supportedcultures-and-supporteduicultures"></a>SupportedCultures y SupportedUICultures

ASP.NET Core le permite especificar dos valores de referencia cultural, `SupportedCultures` y `SupportedUICultures`. El [CultureInfo](https://docs.microsoft.com/dotnet/api/system.globalization.cultureinfo) objeto `SupportedCultures` determina los resultados de funciones dependientes de la referencia cultural, como fecha, hora, número y el formato de moneda. `SupportedCultures`También determina el criterio de ordenación de texto, convenciones de mayúsculas y minúsculas y las comparaciones de cadenas. Vea [CultureInfo.CurrentCulture](https://docs.microsoft.com/dotnet/api/system.stringcomparer.currentculture#System_StringComparer_CurrentCulture) para obtener más información sobre cómo el servidor obtiene la referencia cultural. El `SupportedUICultures` determina que traduce las cadenas (de *.resx* archivos) se buscan por la [ResourceManager](https://docs.microsoft.com/dotnet/api/system.resources.resourcemanager). El `ResourceManager` simplemente busca cadenas específicas de referencias culturales que viene determinado por `CurrentUICulture`. Tiene todos los subprocesos de .NET `CurrentCulture` y `CurrentUICulture` objetos. ASP.NET Core inspecciona estos valores al representar funciones dependientes de la referencia cultural. Por ejemplo, si la referencia cultural del subproceso actual se establece en "en-US" (inglés, Estados Unidos), `DateTime.Now.ToLongDateString()` muestra "Jueves, 18 de febrero de 2016", pero si `CurrentCulture` se establece en "es-es" (español, España) el resultado será "jueves, 18 de febrero de 2016".

## <a name="resource-files"></a>Archivos de recursos

Un archivo de recursos es un mecanismo útil para separar cadenas localizables desde el código. Las cadenas traducidas para el idioma predeterminado no están aisladas *.resx* archivos de recursos. Por ejemplo, puede crear el archivo de recursos de español denominado *Welcome.es.resx* que contiene traduce cadenas. "es" es el código de idioma para español. Para crear este archivo de recursos en Visual Studio:

1. En **el Explorador de soluciones**, haga clic con el botón secundario en la carpeta que contendrá el archivo de recursos > **agregar** > **nuevo elemento**.

    ![Menú contextual anidado: en el Explorador de soluciones, un menú contextual está abierto para los recursos. Un segundo menú contextual está abierto para el complemento que muestra el comando de nuevo elemento resaltado.](localization/_static/newi.png)

2. En el **buscar plantillas instaladas** cuadro, escriba "recurso" y un nombre al archivo.

    ![Cuadro de diálogo Agregar nuevo elemento](localization/_static/res.png)

3. Escriba el valor de clave (cadena nativa) en el **nombre** columna y la cadena traducida en el **valor** columna.

    ![Welcome.es.resx (el archivo principal de recursos para el idioma español) con la palabra Hola en la columna nombre y la palabra Hola (Hola en español) en la columna valor](localization/_static/hola.png)

    Visual Studio muestra el *Welcome.es.resx* archivo.

    ![Explorador de soluciones con el archivo de recursos de bienvenida español (es)](localization/_static/se.png)

<a name="error"></a>

Si está utilizando la versión de Visual Studio de 2017 Preview 15.3, obtendrá un indicador de error en el editor de recursos. Quitar el *ResXFileCodeGenerator* valor de la *herramienta personalizada* cuadrícula de propiedades para evitar este mensaje de error:

![Resx editor](localization/_static/err.png)

Como alternativa, puede omitir este error. Esperamos solucionar este problema en la próxima versión.

## <a name="resource-file-naming"></a>Nomenclatura de archivos de recursos

Se denominan recursos para el nombre de tipo completo de su clase menos el nombre del ensamblado. Por ejemplo, un recurso francés en un proyecto cuyo ensamblado principal está `LocalizationWebsite.Web.dll` para la clase `LocalizationWebsite.Web.Startup` se denominará *Startup.fr.resx*. Un recurso para la clase `LocalizationWebsite.Web.Controllers.HomeController` se denominará *Controllers.HomeController.fr.resx*. Si el espacio de nombres de la clase de su destino no es el mismo que el nombre del ensamblado necesitará el nombre de tipo completo. Por ejemplo, en el ejemplo de un recurso para el tipo de proyecto `ExtraNamespace.Tools` se denominará *ExtraNamespace.Tools.fr.resx*.

En el proyecto de ejemplo, el `ConfigureServices` método establece la `ResourcesPath` a "Recursos", por lo que es la ruta de acceso relativa de proyecto para el archivo de recursos en francés del controlador home *Resources/Controllers.HomeController.fr.resx*. Como alternativa, puede usar carpetas para organizar los archivos de recursos. Para el controlador home, la ruta de acceso sería *Resources/Controllers/HomeController.fr.resx*. Si no utiliza la `ResourcesPath` opción, el *.resx* archivo saldría en el directorio base del proyecto. El archivo de recursos para `HomeController` se denominará *Controllers.HomeController.fr.resx*. La opción de utilizar la convención de nomenclatura de punto o ruta de acceso depende de cómo desea organizar los archivos de recursos.

| Nombre del recurso | Punto o nombres de ruta de acceso |
| ------------   | ------------- |
| Resources/Controllers.HomeController.fr.resx | Punto  |
| Resources/Controllers/HomeController.fr.resx  | Ruta de acceso |
|    |     |

Archivos de recursos mediante `@inject IViewLocalizer` en las vistas de Razor siguen un patrón similar. El archivo de recursos para obtener una vista puede llamarse mediante nombres de punto o nombres de ruta de acceso. Archivos de recursos de vista Razor imitan la ruta de acceso de su archivo de vista asociada. Suponiendo que se establezca la `ResourcesPath` a "Recursos", el archivo de recursos francés asociados a la *Views/Home/About.cshtml* vista podría ser cualquiera de las siguientes acciones:

* Resources/Views/Home/About.fr.resx

* Resources/Views.Home.About.fr.resx

Si no utiliza la `ResourcesPath` opción, el *.resx* de archivos de una vista se pueden ubicar en la misma carpeta que la vista.

## <a name="culture-fallback-behavior"></a>Comportamiento de reserva de la referencia cultural

Por ejemplo, si quita el designador de referencia cultural ".fr" y tiene la referencia cultural establecida en francés, se lee el archivo de recursos predeterminado y las cadenas están localizadas. El Administrador de recursos designa un valor predeterminado o recurso de reserva para cuando nada que coincida con la referencia cultural solicitada. Si desea devolver la clave cuando falta un recurso para solicitado de la referencia cultural no debe tener un archivo de recursos predeterminado.

### <a name="generate-resource-files-with-visual-studio"></a>Generar archivos de recursos con Visual Studio

Si crea un archivo de recursos en Visual Studio sin una referencia cultural en el nombre de archivo (por ejemplo, *Welcome.resx*), Visual Studio creará una clase de C# con una propiedad para cada cadena. Normalmente no es lo desea con ASP.NET Core; Normalmente no tiene valor predeterminado es *.resx* archivo de recursos (A *.resx* archivo sin el nombre de la referencia cultural). Se recomienda crear la *.resx* archivo con un nombre de referencia cultural (por ejemplo *Welcome.fr.resx*). Cuando se crea un *.resx* archivo con un nombre de referencia cultural, Visual Studio no generará el archivo de clase. Prevemos que muchos desarrolladores será **no** crear un archivo de recursos de idioma predeterminado.

### <a name="add-other-cultures"></a>Agregar otras referencias culturales

Cada combinación de idioma y referencia cultural (que no sea el idioma predeterminado) requiere un archivo de recurso único. Crear archivos de recursos de diferentes referencias culturales y configuraciones regionales mediante la creación de nuevos archivos de recursos en el que los códigos de idioma ISO forman parte del nombre de archivo (por ejemplo, **en-us**, **fr-ca**, y  **en-gb**). Estos códigos ISO se colocan entre el nombre de archivo y la *.resx* extensión de nombre de archivo como en *Welcome.es-MX.resx* (Español/México). Para especificar un idioma culturalmente neutro, quite el código de país (`MX` en el ejemplo anterior). El nombre de archivo de recursos de español de referencia cultural neutra es *Welcome.es.resx*.

## <a name="implement-a-strategy-to-select-the-languageculture-for-each-request"></a>Implementar una estrategia para seleccionar el idioma o referencia cultural para cada solicitud  

### <a name="configure-localization"></a>Configurar la localización

Localización está configurada en el `ConfigureServices` método:

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet1)]

* `AddLocalization`Agrega los servicios de localización para el contenedor de servicios. El código anterior también establece la ruta de acceso de recursos para "Recursos".

* `AddViewLocalization`Agrega compatibilidad para ver localizado archivos. En esta vista de ejemplo localización se basa en el sufijo de archivo de vista. Por ejemplo, "fr" en la *Index.fr.cshtml* archivo.

* `AddDataAnnotationsLocalization`Agrega compatibilidad para localizar `DataAnnotations` a través de los mensajes de validación `IStringLocalizer` abstracciones.

### <a name="localization-middleware"></a>Middleware de localización

Se establece la referencia cultural actual en una solicitud en la localización [Middleware](middleware.md). El middleware de localización está habilitado en el `Configure` método *Program.cs* archivo. Tenga en cuenta que el middleware de localización debe configurarse antes de cualquier middleware que podría comprobar la referencia cultural de la solicitud (por ejemplo, `app.UseMvcWithDefaultRoute()`).

[!code-csharp[Main](localization/sample/Localization/Program.cs?name=snippet2)]

`UseRequestLocalization`Inicializa un `RequestLocalizationOptions` objeto. En cada solicitud de la lista de `RequestCultureProvider` en el `RequestLocalizationOptions` es de tipo enumerado y se utiliza el primer proveedor que correctamente puede determinar la referencia cultural de la solicitud. Los proveedores predeterminados proceden de la `RequestLocalizationOptions` clase:

1. `QueryStringRequestCultureProvider`
2. `CookieRequestCultureProvider`
3. `AcceptLanguageHeaderRequestCultureProvider`

La lista predeterminada que se va desde los más específicos al menos específico. Más adelante en el artículo veremos cómo puede cambiar el orden e incluso agregar un proveedor de la referencia cultural personalizada. Si ninguno de los proveedores pueden determinar la referencia cultural de la solicitud, el `DefaultRequestCulture` se utiliza.

### <a name="querystringrequestcultureprovider"></a>QueryStringRequestCultureProvider

Algunas aplicaciones usarán una cadena de consulta para establecer el [referencia cultural y la referencia cultural de interfaz de usuario](https://msdn.microsoft.com/library/system.globalization.cultureinfo.aspx). Para las aplicaciones que usan la cookie o el enfoque del encabezado Accept-Language, agregar una cadena de consulta a la dirección URL es útil para depurar y probar el código. De forma predeterminada, el `QueryStringRequestCultureProvider` está registrado como el primer proveedor de localización en el `RequestCultureProvider` lista. Pase la consulta de parámetros de cadena `culture` y `ui-culture`. En el ejemplo siguiente se establece la referencia cultural concreta (idioma y región) para Español/México:

   `http://localhost:5000/?culture=es-MX&ui-culture=es-MX`

Si se pasa sólo en uno de los dos (`culture` o `ui-culture`), el proveedor de la cadena de consulta establecerá dos valores utilizando uno pasado en. Por ejemplo, establecer simplemente la referencia cultural para establecer ambos el `Culture` y `UICulture`:

   `http://localhost:5000/?culture=es-MX`

### <a name="cookierequestcultureprovider"></a>CookieRequestCultureProvider

Aplicaciones de producción a menudo proporcionará un mecanismo para establecer la referencia cultural con la cookie de la referencia cultural principal de ASP.NET. Use la `MakeCookieValue` método para crear una cookie.

El `CookieRequestCultureProvider` `DefaultCookieName` información de referencia cultural devuelve el nombre de cookie predeterminado utilizado para realizar el seguimiento del usuario preferido. El nombre de cookie predeterminado es ". AspNetCore.Culture".

El formato de la cookie es `c=%LANGCODE%|uic=%LANGCODE%`, donde `c` es `Culture` y `uic` es `UICulture`, por ejemplo:

    c=en-UK|uic=en-US

Si solo especifica uno de información de referencia cultural y la referencia cultural de interfaz de usuario, la referencia cultural especificada se usará para obtener información de referencia cultural y referencia cultural de interfaz de usuario.

### <a name="the-accept-language-http-header"></a>El encabezado HTTP Accept-Language

El [encabezado Accept-Language](https://www.w3.org/International/questions/qa-accept-lang-locales) se puede establecer en la mayoría de los exploradores y se diseñó originalmente para especificar el idioma del usuario. Este valor indica lo que se ha establecido para enviar el explorador o ha heredado del sistema operativo subyacente. El encabezado HTTP Accept-Language de una solicitud del explorador no es un método infalible para detectar el idioma preferido del usuario (consulte [establecer las preferencias de idioma en un explorador](https://www.w3.org/International/questions/qa-lang-priorities.en.php)). Una aplicación de producción debe incluir una manera para que un usuario personalizar su elección de la referencia cultural.

### <a name="set-the-accept-language-http-header-in-ie"></a>Establezca el encabezado HTTP Accept-Language en Internet Explorer

1. Desde el icono de engranaje, pulse **opciones de Internet**.

2. Pulse **idiomas**.

    ![Opciones de Internet](localization/_static/lang.png)

3. Pulse **establecer las preferencias de idioma**.

4. Pulse **agregar un idioma**.

5. Agregue el idioma.

6. Puntee en el idioma y luego pulse **Subir**.

### <a name="use-a-custom-provider"></a>Usar un proveedor personalizado

Imagine que desea permitir que los clientes almacenan su idioma y referencia cultural en las bases de datos. Puede escribir un proveedor para buscar estos valores para el usuario. El código siguiente muestra cómo agregar un proveedor personalizado:

```csharp
private const string enUSCulture = "en-US";

services.Configure<RequestLocalizationOptions>(options =>
{
    var supportedCultures = new[]
    {
        new CultureInfo(enUSCulture),
        new CultureInfo("fr")
    };

    options.DefaultRequestCulture = new RequestCulture(culture: enUSCulture, uiCulture: enUSCulture);
    options.SupportedCultures = supportedCultures;
    options.SupportedUICultures = supportedCultures;

    options.RequestCultureProviders.Insert(0, new CustomRequestCultureProvider(async context =>
    {
        // My custom request culture logic
        return new ProviderCultureResult("en");
    }));
});
```

Use `RequestLocalizationOptions` para agregar o quitar proveedores de localización.

### <a name="set-the-culture-programmatically"></a>Establecer la referencia cultural mediante programación

Este ejemplo **Localization.StarterWeb** proyecto en [GitHub](https://github.com/aspnet/entropy) contiene la interfaz de usuario para establecer el `Culture`. El *Views/Shared/_SelectLanguagePartial.cshtml* archivo le permite seleccionar la referencia cultural de la lista de referencias culturales admitidas:

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_SelectLanguagePartial.cshtml)]

El *Views/Shared/_SelectLanguagePartial.cshtml* archivo se agrega a la `footer` sección del archivo de diseño para que esté disponible para todas las vistas:

[!code-cshtml[Main](localization/sample/Localization/Views/Shared/_Layout.cshtml?range=43-56&highlight=10)]

El `SetLanguage` método establece la cookie de la referencia cultural.

[!code-csharp[Main](localization/sample/Localization/Controllers/HomeController.cs?range=57-67)]

No se puede conectar el *_SelectLanguagePartial.cshtml* al código de ejemplo para este proyecto. El **Localization.StarterWeb** proyecto en [GitHub](https://github.com/aspnet/entropy) tiene código de flujo de la `RequestLocalizationOptions` a una cuchilla parcial a través de la [inyección de dependencia](dependency-injection.md) contenedor.

## <a name="globalization-and-localization-terms"></a>Términos de globalización y localización

El proceso de localizar la aplicación también requiere un conocimiento básico de juegos de caracteres pertinentes usadas en el desarrollo de software moderno y una descripción de los problemas asociados con ellos. Aunque todos los equipos almacenan texto y números (códigos), los sistemas diferentes almacenan el mismo texto con un número diferente. El proceso de localización hace referencia al traducir la interfaz de usuario (UI) de aplicación para una referencia cultural o configuración regional específica.

[Localizabilidad](https://docs.microsoft.com/dotnet/standard/globalization-localization/localizability-review) es un proceso intermedio para comprobar que una aplicación globalizada está preparada para la localización.

El [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) de formato para el nombre de referencia cultural es "<languagecode2>-< country/regioncode2 >", donde <languagecode2> es el código de idioma y < country/regioncode2 > es el código de referencia cultural secundaria. Por ejemplo, `es-CL` para el idioma español (Chile), `en-US` para inglés (Estados Unidos), y `en-AU` para inglés (Australia). [RFC 4646](https://www.ietf.org/rfc/rfc4646.txt) es una combinación de un código de referencia cultural de dos letras en minúscula asociado con un idioma de ISO 639 y un código de referencia cultural secundaria de dos letras en mayúscula asociado con un país o región de ISO 3166.  Vea [nombre de referencia cultural del idioma](https://msdn.microsoft.com/library/ee825488(v=cs.20).aspx).

Internacionalización es a menudo abreviado como "I18N". La abreviatura toma la primera y última letra y el número de letras entre ellos, por lo que 18 representa el número de letras entre la primera "I" y el último "N". Lo mismo se aplica a (G11N) de globalización y localización (L10N).

Términos de:

* Globalización (G11N): El proceso de crear una aplicación compatible con diferentes idiomas y regiones.
* Localización (L10N): El proceso de personalización de una aplicación para un determinado idioma y región.
* Internacionalización (I18N): Describe la globalización y localización.
* Referencia cultural: Es un lenguaje y, opcionalmente, una región.
* Referencia cultural neutra: una referencia cultural que tiene un idioma especificado, pero no una región. (por ejemplo "es-es", "es")
* Referencia cultural específica: una referencia cultural que tiene un idioma especificado y la región. (por ejemplo "en-US", "en-GB", "es-CL")
* Primario de la referencia cultural: referencia cultural neutra que contiene una referencia cultural concreta. (por ejemplo, "es-es" es la referencia cultural principal de "en-US" y "en-GB")
* Configuración regional: Una configuración regional es el mismo que una referencia cultural.

## <a name="additional-resources"></a>Recursos adicionales

* [Proyecto Localization.StarterWeb](https://github.com/aspnet/entropy) utilizada en el artículo.
* [Archivos de recursos en Visual Studio](https://docs.microsoft.com/cpp/windows/resource-files-visual-studio)
* [Recursos en archivos .resx](https://docs.microsoft.com/dotnet/framework/resources/working-with-resx-files-programmatically)
