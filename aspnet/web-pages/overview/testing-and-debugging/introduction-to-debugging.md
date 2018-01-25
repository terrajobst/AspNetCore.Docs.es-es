---
uid: web-pages/overview/testing-and-debugging/introduction-to-debugging
title: "Introducción a la depuración ASP.NET Web Pages (Razor) sitios | Documentos de Microsoft"
author: tfitzmac
description: "La depuración es el proceso de encontrar y corregir errores en las páginas de código. Este capítulo muestra algunas herramientas y técnicas que puede usar para depurar y analyz..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 68de4326-7611-4b9b-b5f6-79b7adc3069f
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/testing-and-debugging/introduction-to-debugging
msc.type: authoredcontent
ms.openlocfilehash: 0b6b5a886efe515b434948dade1ae840ddaecd42
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="introduction-to-debugging-aspnet-web-pages-razor-sites"></a>Introducción a la depuración ASP.NET Web Pages (Razor) sitios
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En este artículo se explica varias maneras para depurar las páginas en un sitio Web de ASP.NET Web Pages (Razor). La depuración es el proceso de encontrar y corregir errores en las páginas de código.
> 
> **Lo que aprenderá:** 
> 
> - Cómo mostrar información que le ayuda a analizar y depurar páginas.
> - Cómo usar la depuración de las herramientas en Visual Studio.
>   
> 
> Estas son las características ASP.NET presentadas en el artículo:
> 
> - El `ServerInfo` auxiliar.
> - `ObjectInfo`aplicación auxiliar.
>   
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2. Puede usar WebMatrix 3 pero no se admite el depurador integrado.


Un aspecto importante de la solución de problemas de errores y problemas en el código es evitarlos en primer lugar. Puede hacerlo poniendo secciones del código que pueden provocar errores en `try/catch` bloques. Para obtener más información, vea la sección sobre cómo controlar errores en [Introducción a ASP.NET Web programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=202890).

El `ServerInfo` auxiliar es una herramienta de diagnóstico que le ofrece una visión general de la información sobre el entorno de servidor web que hospeda la página. También muestra información de solicitud HTTP que se envía cuando un explorador solicita la página. El `ServerInfo` auxiliar muestra la identidad del usuario actual, el tipo de explorador que realizó la solicitud, y así sucesivamente. Este tipo de información puede ayudarle a solucionar problemas comunes.

1. Crear una nueva página web denominada *ServerInfo.cshtml*.
2. Al final de la página, justo antes del cierre `</body>` etiqueta, agregue `@ServerInfo.GetHtml()`:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample1.cshtml)]

    Puede agregar la `ServerInfo` código en cualquier parte de la página. Pero éste se agrega al final se mantendrá su salida independiente de su contenido de la página, lo que facilita la lectura.

    > [!NOTE] 
    > 
    > **Importante** debe quitar cualquier código de diagnóstico de las páginas web antes de mover las páginas web en un servidor de producción. Esto se aplica a la `ServerInfo` auxiliar, así como las otras técnicas de diagnóstico en este artículo que se debe agregar código a una página. No desea que los visitantes del sitio Web para ver información sobre el nombre del servidor, nombres de usuario, rutas de acceso en el servidor y detalles similares, porque este tipo de información que podría ser útil para personas con malas intenciones.
3. Guarde la página y ejecútelo en un explorador.

    ![Depuración de-1](introduction-to-debugging/_static/image1.jpg)

    El `ServerInfo` auxiliar muestra cuatro tablas de información en la página:

    - Configuración del servidor. Esta sección proporciona información sobre el servidor de hospedaje web, incluido el nombre de equipo, la versión de ASP.NET que se está ejecutando, el nombre de dominio y la hora del servidor.
    - Variables de servidor de ASP.NET. En esta sección se proporcionan detalles acerca de los numerosos detalles de protocolo HTTP (llamado HTTP variables) y valores que forman parte de cada solicitud de página web.
    - Información de tiempo de ejecución HTTP. Esta sección se proporcionan detalles sobre el que la versión de Microsoft .NET Framework que se está ejecutando la página web, la ruta de acceso, los detalles acerca de la memoria caché y así sucesivamente. (Según se desprende de [Introducción a ASP.NET Web programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=202890), ASP.NET Web Pages usando el código Razor sintaxis se basan en la tecnología del servidor web de Microsoft ASP.NET que a su vez se basa en una amplia de software biblioteca de desarrollo denomina .NET Framework).
    - Variables de entorno. En esta sección se proporciona una lista de todas las variables de entorno local y sus valores en el servidor web.

    Una descripción completa de toda la información de servidor y la solicitud está fuera del ámbito de este artículo, pero puede ver que el `ServerInfo` auxiliar devuelve una gran cantidad de información de diagnóstico. Para obtener más información acerca de los valores que `ServerInfo` devuelve, vea [reconoce las Variables de entorno](https://technet.microsoft.com/library/dd560744(WS.10).aspx) en el sitio Web de Microsoft TechNet y [Variables de servidor IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) en el sitio Web MSDN.

## <a name="embedding-output-expressions-to-display-page-values"></a>Incrustar expresiones de salida para mostrar los valores de la página

Es otra forma de ver lo que está sucediendo en el código insertar las expresiones de salida en la página. Como sabe, puede generar directamente el valor de una variable mediante la adición de algo parecido a `@myVariable` o `@(subTotal * 12)` a la página. Para la depuración, puede colocar estas expresiones de salida en puntos estratégicos del código. Esto le permite ver el valor de claves variables o el resultado de los cálculos cuando se ejecuta la página. Cuando haya terminado la depuración, puede quitar las expresiones o para convertirlas en comentarios. Este procedimiento muestra una forma habitual de utilizar expresiones incrustadas para facilitar la depuración de una página.

1. Crear una nueva página de WebMatrix que se denomina *OutputExpression.cshtml*.
2. Reemplace el contenido con la siguiente página:

    [!code-html[Main](introduction-to-debugging/samples/sample2.html)]

    El ejemplo se usa un `switch` instrucción para comprobar el valor de la `weekday` variable y, a continuación, mostrar un mensaje de salida diferente dependiendo de qué día de la semana es. En el ejemplo, el `if` bloque dentro del primer bloque de código arbitrariamente cambia el día de la semana mediante la adición de un día en el valor de día de la semana actual. Se trata de un error que se introdujo con fines meramente ilustrativos.
3. Guarde la página y ejecútelo en un explorador.

    La página muestra el mensaje para el día de la semana incorrecto. El día de la semana realmente está, verá el mensaje durante un día más tarde. Aunque en este caso, sabrá por qué el mensaje está desactivada (porque el código deliberadamente establece el valor de día incorrecto), en realidad a menudo es difícil saber dónde va todo incorrecto en el código. Para depurar, es preciso averiguar lo que ocurre con el valor de variables y objetos de clave como `weekday`.
4. Agregue las expresiones de salida insertando `@weekday` tal como se muestra en los dos lugares que indiquen los comentarios en el código. Estas expresiones de resultado mostrará los valores de la variable en ese momento en la ejecución del código.

    [!code-csharp[Main](introduction-to-debugging/samples/sample3.cs?highlight=2-3,15-16)]
5. Guarde y ejecute la página en un explorador.

    La página muestra el día real de la semana en primer lugar, a continuación, el día de la semana actualizado que se origina de adición de un día y, a continuación, el mensaje resultante de la `switch` instrucción. La salida de las dos expresiones de variable (`@weekday`) no tiene espacios entre los días como no se agrega código HTML `<p>` etiquetas a la salida; las expresiones son solo para pruebas.

    ![Depuración de-2](introduction-to-debugging/_static/image2.jpg)

    Ahora puede ver dónde está el error. Al mostrar el `weekday` variable en el código, muestra el día correcto. Cuando se muestren la segunda vez, después de la `if` en el código de bloque, el día está desactivado en uno. Para que sepan que algo ha sucedido entre el primero y segundo el aspecto de la variable de día de la semana. Si se tratara de un error real, este tipo de enfoque contribuye a reducir la ubicación del código que está causando el problema.
6. Corrija el código en la página Quitar las expresiones de dos salida agregó y quitando el código que cambia el día de la semana. Lo restante, completando el bloque de código es similar al ejemplo siguiente:

    [!code-cshtml[Main](introduction-to-debugging/samples/sample4.cshtml)]
7. Ejecute la página en un explorador. Esta ocasión verá el mensaje correcto que se muestra para el día real de la semana.

## <a name="using-the-objectinfo-helper-to-display-object-values"></a>Uso de la aplicación auxiliar ObjectInfo para mostrar los valores de objeto

El `ObjectInfo` auxiliar muestra el tipo y el valor de cada objeto que se pasa a él. Puede utilizarlo para ver el valor de las variables y objetos en el código (como hizo con las expresiones de salida en el ejemplo anterior); además, puede ver datos de información sobre el objeto de tipo.

1. Abra el archivo denominado *OutputExpression.cshtml* que creó anteriormente.
2. Reemplace todo el código en la página con el siguiente bloque de código:

    [!code-html[Main](introduction-to-debugging/samples/sample5.html)]
3. Guarde y ejecute la página en un explorador.

    ![Depuración-4](introduction-to-debugging/_static/image3.jpg)

    En este ejemplo, el `ObjectInfo` auxiliar muestra dos elementos:

    - Tipo. En la primera variable, el tipo es `DayOfWeek`. En la segunda variable, el tipo es `String`.
    - Valor. En este caso, puesto que ya se muestra el valor de la variable de saludo en la página, el valor se muestra otra vez cuando pase la variable a `ObjectInfo`.

    Para los objetos más complejos, la `ObjectInfo` auxiliar puede mostrar más información &#8212; básicamente, puede mostrar los tipos y valores de todas las propiedades de un objeto.

## <a name="using-debugging-tools-in-visual-studio"></a>Con herramientas de depuración en Visual Studio

Para una experiencia de depuración más completa, use Visual Studio 2013 o gratuitamente [Visual Studio Express 2013 para Web](https://www.visualstudio.com/downloads/download-visual-studio-vs#d-2013-express). Con Visual Studio, puede establecer un punto de interrupción en el código en la línea que desea inspeccionar.

![Punto de interrupción establecido](introduction-to-debugging/_static/image1.png)

Cuando se prueba el sitio web, el código en ejecución se detiene en el punto de interrupción.

![alcanzar el punto de interrupción](introduction-to-debugging/_static/image2.png)

Puede examinar los valores actuales de las variables y recorrer el código línea por línea.

![ver los valores](introduction-to-debugging/_static/image3.png)

Para obtener información sobre cómo usar el depurador integrado en Visual Studio para depurar páginas ASP.NET Razor, consulte [programación ASP.NET Web Pages (Razor) en Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854).

## <a name="additional-resources"></a>Recursos adicionales

- [Programar páginas Web ASP.NET (Razor) con Visual Studio](https://go.microsoft.com/fwlink/?LinkId=205854)
- [Variables de servidor IIS](https://msdn.microsoft.com/library/ms524602(VS.90).aspx) (MSDN)
- [Reconoce las Variables de entorno](https://technet.microsoft.com/library/dd560744(WS.10).aspx) (TechNet)
