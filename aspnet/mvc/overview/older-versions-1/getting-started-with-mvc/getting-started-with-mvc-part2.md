---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
title: Cómo agregar un controlador | Documentos de Microsoft
author: shanselman
description: Una versión actualizada, si este tutorial está disponible aquí con Visual Studio 2013. El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: ff03dcc0-da97-458d-838f-0823e7482642
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part2
msc.type: authoredcontent
ms.openlocfilehash: c6ecd1ffdd53a629d0079d57b85c7f6db2f316ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="adding-a-controller"></a>Agregar un controlador
====================
por [Scott Hanselman](https://github.com/shanselman)

> > [!NOTE]
> > Una versión actualizada, si está disponible en este tutorial [aquí](../../getting-started/introduction/getting-started.md) con [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads). El nuevo tutorial usa ASP.NET MVC 5, que proporciona muchas mejoras con respecto a este tutorial.
> 
> 
> Se trata de un tutorial para principiantes que presenta los conceptos básicos de ASP.NET MVC. Se creará una aplicación web simple que lee y escribe desde una base de datos. Visite la [centro de aprendizaje de ASP.NET MVC](../../../index.md) para buscar otros ASP.NET MVC, tutoriales y ejemplos.


MVC hace referencia para el modelo, vista y controlador. MVC es un modelo para desarrollar aplicaciones de forma que cada parte tiene una responsabilidad que es diferente de otro.

- : Los datos del modelo De la aplicación
- Vistas: Los archivos de plantilla de la aplicación utilizará para generar dinámicamente las respuestas HTML.
- Controladores: Las clases que controlan las solicitudes entrantes de dirección URL a la aplicación, recuperar datos del modelo y, a continuación, especificar plantillas de vista que representan una respuesta al cliente

Comenzaremos ser que cubren todos estos conceptos en este tutorial y muestran cómo utilizarlas para crear una aplicación.

Vamos a crear un nuevo controlador haciendo clic en la carpeta de controladores en el Explorador de soluciones y seleccionando Agregar controlador.

[![AddControllerRightClick](getting-started-with-mvc-part2/_static/image2.png)](getting-started-with-mvc-part2/_static/image1.png)

El nuevo controlador de "HelloWorldController" el nombre y haga clic en Agregar.

[![Agregar cuadro de diálogo de controlador](getting-started-with-mvc-part2/_static/image4.png)](getting-started-with-mvc-part2/_static/image3.png)

Observe que en el Explorador de soluciones de la derecha que se ha creado un nuevo archivo para el que llama HelloWorldController.cs y ahora se abre ese archivo en el **IDE**.

[![HelloWorldControllerCode](getting-started-with-mvc-part2/_static/image6.png)](getting-started-with-mvc-part2/_static/image5.png)

Cree dos nuevos métodos que tengan un aspecto similar al siguiente dentro de la clase pública HelloWorldController. Se tendrá que volver una cadena HTML directamente desde nuestro controlador como un ejemplo.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample1.cs)]

El controlador se denomina HelloWorldController y se llama al método nuevo índice. Ejecute la aplicación de nuevo, igual que antes (haga clic en el botón Reproducir o presione F5 para hacer esto). Una vez que el explorador se ha iniciado, cambiar la ruta de acceso en la barra de direcciones para `http://localhost:xx/HelloWorld` donde xx es el número de su equipo ha elegido. El explorador debe parecerse a la captura de pantalla siguiente. En el método anterior se devuelve una cadena que se pasa a un método llamado "Contenido". Le dijimos el sistema sólo devuelve el código HTML y así ha sido!

ASP.NET MVC invoca distintas clases de controlador (y diferentes métodos de acción dentro de ellas) dependiendo de la dirección URL entrante. La lógica de asignación predeterminada usa ASP.NET MVC utiliza un formato similar al siguiente para controlar qué código se ejecuta:

/[Controller]/[ActionName]/[Parameters]

La primera parte de la dirección URL determina la clase de controlador que se ejecutará. Por lo que /HelloWorld se asigna a la clase HelloWorldController. La segunda parte de la dirección URL determina el método de acción en la clase que se ejecutará. Por lo que /HelloWorld/Index provocaría que el método Index() de la clase HelloWorldcontroller que se ejecutará. Tenga en cuenta que solo se tenía visite /HelloWorld anterior y el método que índice estaba implícito. Esto es porque un método denominado "Índice" es el método predeterminado que se llamará en un controlador si no se especifica explícitamente.

[![Ésta es la acción predeterminada](getting-started-with-mvc-part2/_static/image8.png)](getting-started-with-mvc-part2/_static/image7.png)

Ahora, vamos a visitar `http://localhost:xx/HelloWorld/Welcome.` ahora nuestro método principal se ejecuta y devuelve la cadena HTML.

Una vez más, / [Controller] / [ActionName] / [parámetros] por lo que es HelloWorld y bienvenida es el método en este caso. Aún no hemos hecho parámetros.

[![Este es el método de acción bienvenida](getting-started-with-mvc-part2/_static/image10.png)](getting-started-with-mvc-part2/_static/image9.png)

Vamos a modificar nuestro ejemplo ligeramente para que para pasar información en desde la dirección URL a nuestro controlador, por ejemplo como esta: / HelloWorld/bienvenida? name = Scott&amp;numtimes = 4. Cambiar el método principal para que incluya dos parámetros y actualización como las siguientes. Tenga en cuenta que hemos usado la característica de parámetro opcional de C# para indicar que el parámetro numTimes, de forma predeterminada en 1 si no se pasa en.

[!code-csharp[Main](getting-started-with-mvc-part2/samples/sample2.cs)]

Ejecute la aplicación y visite `http://localhost:xx/HelloWorld/Welcome?name=Scott&numtimes=4` cambiar el valor de nombre y numtimes como sea necesario. El sistema asigna automáticamente los parámetros con nombre de la cadena de consulta en la barra de direcciones a los parámetros del método.

En estos ejemplos de ambos el controlador ha sido realizar todo el trabajo y ha sido devolver HTML directamente. Normalmente no es aconsejable nuestro controladores devolver directamente - HTML desde que acabe siendo muy complicadas de código. En su lugar, vamos a usar normalmente un archivo de plantilla de vista independiente para ayudar a generar la respuesta HTML. Echemos un vistazo a cómo podemos hacer esto. Cierre el explorador y volver al IDE.

> [!div class="step-by-step"]
> [Anterior](getting-started-with-mvc-part1.md)
> [Siguiente](getting-started-with-mvc-part3.md)
