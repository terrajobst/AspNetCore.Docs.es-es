---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Agregar un controlador | Microsoft Docs
author: shanselman
description: Una versión actualizada, si este tutorial está disponible aquí con Visual Studio 2013. El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a t...
ms.author: riande
ms.date: 08/14/2010
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: 856b7d98ac8bd30982d81b0609bb9c1288e07e49
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829808"
---
<a name="adding-a-controller"></a>Agregar un controlador
====================
por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Una versión actualizada, si está disponible en este tutorial [aquí](../../getting-started/introduction/getting-started.md) mediante [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a este tutorial.
> 
> 
> Este es un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Creará una aplicación web simple que lee y escribe desde una base de datos. Visite el [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


MVC es el acrónimo de modelo, vista, el controlador. MVC es un patrón para desarrollar aplicaciones de forma que cada parte tiene una responsabilidad que es diferente de otro.

- Modelo: Los datos de la aplicación
- Vistas: Los archivos de plantilla de la aplicación usará para generar respuestas HTML de forma dinámica.
- Controladores: Las clases que controlan las solicitudes entrantes de dirección URL a la aplicación, recuperar datos del modelo y, a continuación, especifique las plantillas de vista que representan una respuesta al cliente

Se va a cubrir todos estos conceptos en este tutorial y le enseñaremos a usarlos para crear una aplicación.

Vamos a crear un nuevo controlador de clic derecho en la carpeta controladores en el Explorador de soluciones y seleccionando Agregar controlador.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

Asigne un nombre al nuevo controlador "HelloWorldController" y haga clic en Agregar.

[![Agregar cuadro de diálogo de controlador](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Observe que en el Explorador de soluciones a la derecha que se ha creado un nuevo archivo para llamó a HelloWorldController.cs y ahora se abre ese archivo en el **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Cree dos nuevos métodos que tienen un aspecto similar al siguiente dentro de la nueva clase pública HelloWorldController. Usaremos una cadena HTML directamente desde nuestro controlador como un ejemplo.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

El controlador se denomina HelloWorldController y al nuevo método se denomina índice. Ejecute la aplicación de nuevo, igual que antes (haga clic en el botón Reproducir o presione F5 para hacer esto). Una vez que haya iniciado el explorador, cambie la ruta de acceso en la barra de direcciones para `http://localhost:xx/HelloWorld` donde xx es cualquier número de su equipo ha elegido. Ahora el explorador debería parecerse a la captura de pantalla siguiente. En el método anterior, se devuelve una cadena que se pasa a un método llamado "Contenido". Le dijimos que el sistema sólo devuelve algo de HTML, y así ha sido!

ASP.NET MVC invoca diferentes clases de controlador (y diferentes métodos de acción dentro de ellos), dependiendo de la dirección URL entrante. La lógica de asignación predeterminada utilizada por ASP.NET MVC usa un formato similar al siguiente para controlar qué código se ejecuta:

/ [Controller] / [ActionName] / [parámetros]

La primera parte de la dirección URL determina la clase de controlador para ejecutar. Por lo que /HelloWorld se asigna a la clase HelloWorldController. La segunda parte de la dirección URL determina el método de acción en la clase que se ejecutará. /HelloWorld/Index causara el método Index() de la clase HelloWorldcontroller para ejecutar. Tenga en cuenta que sólo se tenía visite /HelloWorld anterior y el método que index estaba implícito. Esto es porque un método denominado "Index" es el método predeterminado que se llamará en un controlador si no se especifica explícitamente.

[![Ésta es mi acción predeterminada](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Ahora, vamos a visitar `http://localhost:xx/HelloWorld/Welcome.` ahora nuestro método principal se ejecuta y devuelve la cadena HTML.

Nuevamente, / [Controller] / [ActionName] / [parámetros], por lo que el controlador es HelloWorld y bienvenida en este caso es el método. Aún no hemos hecho los parámetros.

[![Este es el método de acción bienvenida](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Vamos a modificar nuestro ejemplo ligeramente para que nos podemos pasar cierta información en de la dirección URL a nuestro controlador, por ejemplo así: / HelloWorld/Welcome? nombre = Scott&amp;numtimes = 4. Cambiar el método principal para que incluya dos parámetros y actualización como el siguiente. Tenga en cuenta que hemos usado para indicar que el parámetro numTimes debería valor predeterminado es 1 si no se pasa en la característica de parámetro opcional de C#.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Ejecute la aplicación y visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` cambiar el valor de nombre y numtimes como sea necesario. El sistema asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones para los parámetros del método.

En estos ejemplos de ambos el controlador ha sido hace todo el trabajo y ha estado devolviendo HTML directamente. Normalmente no queremos que nuestra controladores devuelve HTML directamente - desde que termina siendo muy complicado de código. En su lugar, vamos a usar normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Veamos cómo podemos hacer esto. Cierre el explorador y volver al IDE.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part1.md)
> [Siguiente](getting-started-with-mvc-part3.md)
