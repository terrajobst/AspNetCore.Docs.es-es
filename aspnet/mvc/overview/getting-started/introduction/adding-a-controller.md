---
uid: mvc/overview/getting-started/introduction/adding-a-controller
title: Cómo agregar un controlador | Documentos de Microsoft
author: Rick-Anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: cc764f3b-6921-486a-8f44-c6ccd1249acd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-a-controller
msc.type: authoredcontent
ms.openlocfilehash: 3864bab284661b0c44f9e4cb363c2d60eccc7c66
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller"></a>Agregar un controlador
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE [Tutorial Note](sample/code-location.md)]

MVC es el acrónimo *model-view-controller*. MVC es un modelo para desarrollar aplicaciones que están bien diseñada, comprobable y fáciles de mantener. Las aplicaciones basadas en MVC contienen:

- **M** odels: las clases que representan los datos de la aplicación y que usan una lógica de validación para aplicar las reglas de negocios para esos datos.
- **V** iews: archivos de plantilla que la aplicación usa para generar dinámicamente las respuestas HTML.
- **C** ontrollers: las clases que controlan las solicitudes entrantes de explorador, recuperar datos del modelo y, a continuación, especificar plantillas de vista que devuelven una respuesta al explorador.

Comenzaremos ser que cubren todos estos conceptos en esta serie de tutoriales y muestran cómo utilizarlas para crear una aplicación.

> [!NOTE]
> En el paso anterior MVC predeterminada se ha seleccionado la plantilla. Esto crea el hogar, cuenta y administrar controladores de forma predeterminada.

Vamos a empezar a crear una clase de controlador. En **el Explorador de soluciones**, haga clic en el *controladores* carpeta y, a continuación, haga clic en **agregar**, a continuación, **controlador**.


![](adding-a-controller/_static/image1.png)

En el **agregar scaffolding** cuadro de diálogo, haga clic en **controlador de MVC 5 - vacío**y, a continuación, haga clic en **agregar**.

![](adding-a-controller/_static/image2.png)  
 

El nuevo controlador el nombre "HelloWorldController" y haga clic en **agregar**.

![Agregar controlador](adding-a-controller/_static/image3.png)

Observe que en **el Explorador de soluciones** que un nuevo archivo se ha creado con nombre *HelloWorldController.cs* y una nueva carpeta *Views\HelloWorld*. El controlador está abierto en el IDE.

![](adding-a-controller/_static/image4.png)

Reemplace el contenido del archivo con el código siguiente.

[!code-csharp[Main](adding-a-controller/samples/sample1.cs)]

Los métodos de controlador devolverá una cadena HTML como un ejemplo. El controlador se denomina `HelloWorldController` y el primer método se denomina `Index`. Vamos a invocarlo desde un explorador. Ejecute la aplicación (presione F5 o CTRL+F5). En el explorador, anexar &quot;HelloWorld&quot; a la ruta de acceso en la barra de direcciones. (Por ejemplo, en la ilustración siguiente, su `http://localhost:1234/HelloWorld.`) la página en el explorador será similar a la captura de pantalla siguiente. En el método anterior, el código devuelve una cadena directamente. Indicar el sistema para devolver algo de HTML y así ha sido!

![](adding-a-controller/_static/image5.png)

ASP.NET MVC invoca las clases de controlador diferente (y métodos de acción diferentes dentro de ellas) dependiendo de la dirección URL entrante. La lógica de enrutamiento de dirección URL predeterminada utilizada por ASP.NET MVC utiliza un formato similar al siguiente para determinar qué código debe invocar:

`/[Controller]/[ActionName]/[Parameters]`

Establecer el formato para el enrutamiento en el *aplicación\_Start/RouteConfig.cs* archivo.

[!code-csharp[Main](adding-a-controller/samples/sample2.cs?highlight=7-8)]

Cuando ejecute la aplicación y no tiene que proporcionar los segmentos de dirección URL, el valor predeterminado es el controlador "Home" y el método de acción "Índice" especificado en la sección valores predeterminados del código anterior.

La primera parte de la dirección URL determina la clase de controlador que se ejecutará. Por lo que */HelloWorld* se asigna a la `HelloWorldController` clase. La segunda parte de la dirección URL determina el método de acción en la clase que se ejecutará. Por lo que */HelloWorld/índice* provocaría que el `Index` método de la `HelloWorldController` clase que se ejecutará. Tenga en cuenta que sólo tuvimos que vaya a */HelloWorld* y `Index` usó el método de forma predeterminada. Esto es porque un método denominado `Index` es el método predeterminado que se llamará en un controlador si no se especifica explícitamente. La tercera parte del segmento de dirección URL (`Parameters`) es para los datos de ruta. Veremos enrutar los datos más adelante en este tutorial.

Vaya a `http://localhost:xxxx/HelloWorld/Welcome`. El `Welcome` método se ejecuta y devuelve la cadena &quot;éste es el método de acción bienvenida... &quot;. La asignación de MVC predeterminada es `/[Controller]/[ActionName]/[Parameters]`. Para esta dirección URL, el controlador es `HelloWorld` y `Welcome` es el método de acción. Todavía no ha usado el elemento `[Parameters]` de la dirección URL.

![](adding-a-controller/_static/image6.png)

Vamos a modificar el ejemplo ligeramente para que pueda pasar cierta información de parámetro de la dirección URL para el controlador (por ejemplo, */HelloWorld/bienvenida? name = Scott&amp;numtimes = 4*). Cambiar el `Welcome` método debe incluir dos parámetros, tal y como se muestra a continuación. Tenga en cuenta que el código usa la característica de parámetro opcional de C# para indicar que el `numTimes` parámetro, de forma predeterminada en 1 si se pasa ningún valor para ese parámetro.

[!code-csharp[Main](adding-a-controller/samples/sample3.cs)]

> [!NOTE]
> Nota de seguridad: El código anterior se usa [HttpUtility.HtmlEncode](https://msdn.microsoft.com/library/ee360286(v=vs.110).aspx) para proteger la aplicación de entrada malintencionada (es decir, JavaScript). Para obtener más información, consulte [Cómo: proteger frente a ataques mediante secuencias de una aplicación Web mediante la aplicación de la codificación de HTML a cadenas](https://msdn.microsoft.com/library/a2a4yykt(v=vs.100).aspx).


 Ejecute la aplicación y busque la dirección URL de ejemplo (`http://localhost:xxxx/HelloWorld/Welcome?name=Scott&numtimes=4`). Puede probar valores diferentes para `name` y `numtimes` en la dirección URL. El [sistema de enlace de modelo de ASP.NET MVC](http://odetocode.com/Blogs/scott/archive/2009/04/27/6-tips-for-asp-net-mvc-model-binding.aspx) asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones a los parámetros del método.

![](adding-a-controller/_static/image7.png)

En el ejemplo anterior, el segmento de dirección URL ( `Parameters`) no se utiliza, el `name` y `numTimes` parámetros se pasan como [las cadenas de consulta](http://en.wikipedia.org/wiki/Query_string). El carácter comodín ? (signo de interrogación) en la dirección URL anterior es un separador y siguen las cadenas de consulta. El carácter &amp; separa las cadenas de consulta.

Reemplace el método Bienvenido con el código siguiente:

[!code-csharp[Main](adding-a-controller/samples/sample4.cs)]

Ejecute la aplicación y escriba la dirección URL siguiente: `http://localhost:xxx/HelloWorld/Welcome/1?name=Scott`

![](adding-a-controller/_static/image8.png)

Esta vez el tercer segmento de dirección URL coincide con el parámetro de ruta `ID.` el `Welcome` método de acción contiene un parámetro (`ID`) que coinciden con la especificación de dirección URL de la `RegisterRoutes` método.

[!code-csharp[Main](adding-a-controller/samples/sample5.cs?highlight=7)]

En las aplicaciones de ASP.NET MVC, es más habitual para pasar parámetros como datos de ruta (al igual que hicimos con el identificador anterior) que se pasan como cadenas de consulta. También puede agregar una ruta para pasar tanto el `name` y `numtimes` en parámetros como datos de ruta en la dirección URL. En el *aplicación\_Start\RouteConfig.cs* , agregue la ruta de "Hello":

[!code-csharp[Main](adding-a-controller/samples/sample6.cs?highlight=13-16)]

Ejecute la aplicación y vaya a `/localhost:XXX/HelloWorld/Welcome/Scott/3`.

![](adding-a-controller/_static/image9.png)

Para muchas aplicaciones de MVC, la ruta predeterminada funciona bien. Aprenderá más adelante en este tutorial para pasar datos mediante el enlazador de modelos y no tendrá que modificar la ruta predeterminada para.

En estos ejemplos, el controlador ha realizado la &quot;VC&quot; parte de MVC, es decir, el trabajo de vista y controlador. El controlador devuelve HTML directamente. Normalmente no desea devolver HTML directamente, desde que se vuelve muy complicada al código de controladores. En su lugar, vamos a usar normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Echemos un vistazo siguiente en cómo podemos hacer esto.

> [!div class="step-by-step"]
> [Anterior](getting-started.md)
> [Siguiente](adding-a-view.md)
