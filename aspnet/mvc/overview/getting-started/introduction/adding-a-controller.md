---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Agregar un controlador | Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: be8b21b4b8db60b1a67918d2df8779e729243fc8
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374677"
---
<a name="adding-a-controller"></a>Agregar un controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC es el acrónimo *model-view-controller*. MVC es un patrón para desarrollar aplicaciones que están bien diseñada, puede probar y fáciles de mantener. Las aplicaciones basadas en MVC contienen:

- **M** odelos: clases que representan los datos de la aplicación y que utilizan una lógica de validación para aplicar reglas de negocio para esos datos.
- **V** istas: archivos de plantilla que la aplicación usa para generar respuestas HTML de forma dinámica.
- **C** ontroladores: las clases que controlan las solicitudes entrantes de explorador, recuperar datos del modelo y, a continuación, especifique las plantillas de vista que devuelven una respuesta al explorador.

Se va a cubrir todos estos conceptos en esta serie de tutoriales y mostrarle cómo se usan para compilar una aplicación.

> [!NOTE]
> En el paso anterior MVC predeterminada se ha seleccionado la plantilla. Esto crea el hogar, cuenta y administrar controladores de forma predeterminada.

Comencemos por crear una clase de controlador. En **el Explorador de soluciones**, haga clic en el *controladores* carpeta y, a continuación, haga clic en **agregar**, a continuación, **controlador**.


![](adding-a-controller/_static/image1.png)

En el **agregar Scaffold** cuadro de diálogo, haga clic en **controlador MVC 5 - vacío**y, a continuación, haga clic en **agregar**.

![](adding-a-controller/_static/image2.png)  
 

Asigne un nombre al nuevo controlador "HelloWorldController" y haga clic en **agregar**.

![Agregar controlador](adding-a-controller/_static/image3.png)

Tenga en cuenta en **el Explorador de soluciones** que un nuevo archivo se ha creado con nombre *HelloWorldController.cs* y una nueva carpeta *Views\HelloWorld*. El controlador está abierto en el IDE.

![](adding-a-controller/_static/image4.png)

Reemplace el contenido del archivo con el código siguiente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Los métodos de controlador devolverá una cadena HTML como un ejemplo. El controlador se denomina `HelloWorldController` y el primer método se denomina `Index`. Vamos a invocarlo desde un explorador. Ejecute la aplicación (presione F5 o CTRL+F5). En el explorador, anexe &quot;HelloWorld&quot; a la ruta de acceso en la barra de direcciones. (Por ejemplo, en la ilustración siguiente, su `http://localhost:1234/HelloWorld.`) la página en el explorador será similar a la captura de pantalla siguiente. En el método anterior, el código devuelve una cadena directamente. Dijo el sistema solo se devuelva algo de HTML, y así ha sido!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC invoca las clases de controlador diferente (y los métodos de acción diferentes dentro de ellos) según la dirección URL entrante. La lógica de enrutamiento de dirección URL predeterminada usa ASP.NET MVC usa un formato similar al siguiente para determinar qué código debe invocar:

`/[Controller]/[ActionName]/[Parameters]`

Establecer el formato para el enrutamiento en el *aplicación\_Start/RouteConfig.cs* archivo.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Cuando se ejecuta la aplicación y los segmentos de dirección URL no se proporciona, el valor predeterminado es el controlador "Home" y el método de acción "Index" especificados en la sección de valores predeterminados del código anterior.

La primera parte de la dirección URL determina la clase de controlador para ejecutar. Por lo tanto */HelloWorld* se asigna a la `HelloWorldController` clase. La segunda parte de la dirección URL determina el método de acción en la clase que se ejecutará. Por lo tanto */HelloWorld/Index* provocaría el `Index` método de la `HelloWorldController` clase que se ejecutará. Tenga en cuenta que sólo teníamos que vaya a */HelloWorld* y `Index` usó el método de forma predeterminada. Esto es porque un método denominado `Index` es el método predeterminado que se llamará en un controlador si no se especifica explícitamente. La tercera parte del segmento de dirección URL (`Parameters`) es para los datos de ruta. Veremos enrutar los datos más adelante en este tutorial.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El `Welcome` método se ejecuta y devuelve la cadena &quot;este es el método de acción bienvenida... &quot;. La asignación de MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción. Todavía no ha usado el elemento `[Parameters]` de la dirección URL.

![](adding-a-controller/_static/image6.png)

Vamos a modificar el ejemplo ligeramente para que pueda pasar cierta información del parámetro de la dirección URL al controlador (por ejemplo, */HelloWorld/Welcome? nombre = Scott&amp;numtimes = 4*). Cambiar su `Welcome` método para incluir dos parámetros, como se muestra a continuación. Tenga en cuenta que el código usa la característica de parámetro opcional de C# para indicar que el `numTimes` parámetro, de forma predeterminada en 1 si se pasa ningún valor para ese parámetro.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Nota de seguridad: El código anterior usa [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) para proteger la aplicación de entradas malintencionadas (es decir, JavaScript). Para obtener más información, consulte [Cómo: proteger frente a ataques mediante secuencias de una aplicación Web por aplicar codificación HTML a las cadenas](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Ejecute la aplicación y vaya a la dirección URL de ejemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Puede probar valores diferentes para `name` y `numtimes` en la dirección URL. El [sistema de enlace de modelos ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones para los parámetros del método.

![](adding-a-controller/_static/image7.png)

En el ejemplo anterior, el segmento de dirección URL ( `Parameters`) no se utiliza el `name` y `numTimes` parámetros se pasan como [cadenas de consulta](http://en.wikipedia.org/wiki/Query_string). El carácter comodín ? (signo de interrogación) en la dirección URL anterior es un separador y siguen las cadenas de consulta. El carácter &amp; separa las cadenas de consulta.

Reemplace el método Bienvenido con el código siguiente:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Ejecute la aplicación y escriba la dirección URL siguiente: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

En este momento el tercer segmento de dirección URL coincide con el parámetro de ruta `ID.` el `Welcome` método de acción contiene un parámetro (`ID`) que coincida con la especificación de dirección URL de la `RegisterRoutes` método.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

En las aplicaciones de ASP.NET MVC, es más habitual para pasar los parámetros como datos de ruta que pasarlos como cadenas de consulta (como hicimos con el identificador anterior). También podría agregar una ruta para pasar tanto el `name` y `numtimes` en parámetros como datos de ruta en la dirección URL. En el *aplicación\_start\routeconfig* , agregue la ruta de "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Ejecute la aplicación y vaya a `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Para muchas aplicaciones de MVC, la ruta predeterminada funciona bien. Conocerá más adelante en este tutorial para pasar los datos mediante el enlazador de modelos y no tendrá que modificar la ruta predeterminada para eso.

En estos ejemplos el controlador ha realizado la &quot;VC&quot; parte de MVC, es decir, el trabajo de vista y controlador. El controlador devuelve HTML directamente. Normalmente no desea que los controles devuelvan HTML directamente, porque resulta muy complicado de código. En su lugar, vamos a usar normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Echemos un vistazo siguiente en ¿cómo podemos hacer esto.

> [!div class="step-by-step"]
> [Anterior](getting-started.md)
> [Siguiente](adding-a-view.md)
