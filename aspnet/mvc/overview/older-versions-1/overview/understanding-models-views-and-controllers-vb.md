---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
title: Descripción de los modelos, vistas y controladores (VB) | Documentos de Microsoft
author: StephenWalther
description: ¿Está confundido por modelos, vistas y controladores? En este tutorial, Stephen Walther presenta las distintas partes de una aplicación ASP.NET MVC.
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: a106374a-5e74-4fd0-9ac0-1a32280e5d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
msc.type: authoredcontent
ms.openlocfilehash: 36f70503f7e96210e5fb8a2d361da6759c18c85b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500994"
---
<a name="understanding-models-views-and-controllers-vb"></a>Descripción de los modelos, vistas y controladores (VB)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> ¿Está confundido por modelos, vistas y controladores? En este tutorial, Stephen Walther presenta las distintas partes de una aplicación ASP.NET MVC.


Este tutorial proporciona una descripción general de ASP.NET MVC modelos, vistas y controladores. En otras palabras, se explica la M', V' y C' en ASP.NET MVC.

Después de leer este tutorial, debe comprender cómo funcionan conjuntamente las distintas partes de una aplicación ASP.NET MVC. También debe entender cómo la arquitectura de una aplicación ASP.NET MVC difiere de una aplicación de formularios Web Forms de ASP.NET o una aplicación de páginas Active Server.

## <a name="the-sample-aspnet-mvc-application"></a>La aplicación de MVC de ASP.NET de ejemplo

La plantilla de Visual Studio de forma predeterminada para crear aplicaciones Web de ASP.NET MVC incluye una aplicación de ejemplo muy simple que puede usarse para entender las diferentes partes de una aplicación ASP.NET MVC. Se sacar partido de esta aplicación sencilla en este tutorial.

Crea una nueva aplicación MVC de ASP.NET con la plantilla MVC al ejecutar Visual Studio 2008 y seleccionar la opción de menú archivo, nuevo proyecto (consulte la figura 1). En el cuadro de diálogo nuevo proyecto, seleccione el lenguaje de programación favoritos en tipos de proyecto (Visual Basic o C#) y seleccione **aplicación Web ASP.NET MVC** en plantillas. Haga clic en el botón Aceptar.


[![Cuadro de diálogo nuevo proyecto](understanding-models-views-and-controllers-vb/_static/image1.jpg)](understanding-models-views-and-controllers-vb/_static/image1.png)

**Figura 01**: cuadro de diálogo nuevo proyecto ([haga clic aquí para ver la imagen a tamaño completo](understanding-models-views-and-controllers-vb/_static/image2.png))


Cuando se crea una nueva aplicación de ASP.NET MVC, la **crear proyecto de prueba unitaria** aparece de cuadro de diálogo (consulte la figura 2). Este cuadro de diálogo le permite crear un proyecto independiente de la solución para probar la aplicación de ASP.NET MVC. Seleccione la opción **No, deseo crear un proyecto de prueba unitaria** y haga clic en el **Aceptar** botón.


[![Crear cuadro de diálogo de pruebas unitarias](understanding-models-views-and-controllers-vb/_static/image2.jpg)](understanding-models-views-and-controllers-vb/_static/image3.png)

**Figura 02**: crear unidad Probar cuadro de diálogo ([haga clic aquí para ver la imagen a tamaño completo](understanding-models-views-and-controllers-vb/_static/image4.png))


Después de la nueva ASP.NET MVC se crea la aplicación. Verá varias carpetas y archivos en la ventana Explorador de soluciones. En concreto, verá tres carpetas denominadas modelos, vistas y controladores. Como habrá adivinado de los nombres de carpeta, estas carpetas contienen los archivos de implementación de modelos, vistas y controladores.

Si expande la carpeta Controllers, debería ver un archivo denominado AccountController.vb y un archivo denominado HomeController.vb. Si expande la carpeta Views, verá tres subcarpetas con el nombre de cuenta de inicio y compartido. Si expande la carpeta Inicio, verá dos archivos adicionales denominados About.aspx y Index.aspx (consulte la figura 3). Estos archivos constituyen la aplicación de ejemplo incluido con el valor predeterminado de plantilla de ASP.NET MVC.


[![La ventana del explorador de soluciones](understanding-models-views-and-controllers-vb/_static/image3.jpg)](understanding-models-views-and-controllers-vb/_static/image5.png)

**Figura 03**: la ventana del explorador de soluciones ([haga clic aquí para ver la imagen a tamaño completo](understanding-models-views-and-controllers-vb/_static/image6.png))


Puede ejecutar la aplicación de ejemplo seleccionando la opción de menú **depurar, Iniciar depuración**. Como alternativa, puede presionar la tecla F5.

Cuando ejecuta una aplicación ASP.NET por primera vez, aparece el cuadro de diálogo en la figura 4 que se recomienda que habilite el modo de depuración. Haga clic en el botón Aceptar y la aplicación se ejecutará.


[![Cuadro de diálogo depuración no habilitada](understanding-models-views-and-controllers-vb/_static/image4.jpg)](understanding-models-views-and-controllers-vb/_static/image7.png)

**Figura 04**: cuadro de diálogo de depuración no habilitada ([haga clic aquí para ver la imagen a tamaño completo](understanding-models-views-and-controllers-vb/_static/image8.png))


Cuando se ejecuta una aplicación de ASP.NET MVC, Visual Studio inicia la aplicación en el explorador web. La aplicación de ejemplo consta de dos páginas: la página de índice y la página About. Cuando la aplicación se inicia por primera vez, aparece la página de índice (consulte la figura 5). Puede navegar hasta la página About haciendo clic en el vínculo del menú en la parte superior derecha de la aplicación.


[![La página de índice](understanding-models-views-and-controllers-vb/_static/image5.jpg)](understanding-models-views-and-controllers-vb/_static/image9.png)

**Figura 05**: la página de índice ([haga clic aquí para ver la imagen a tamaño completo](understanding-models-views-and-controllers-vb/_static/image10.png))


Tenga en cuenta las direcciones URL en la barra de direcciones del explorador. Por ejemplo, al hacer clic en el vínculo del menú acerca, la dirección URL en la barra de direcciones del explorador cambia a **/principal/sobre**.

Si cierra la ventana del explorador y vuelva a Visual Studio, no podrá buscar un archivo con la ruta de acceso principal/aproximadamente. Los archivos no existen. ¿Cómo es posible?

## <a name="a-url-does-not-equal-a-page"></a>Una dirección URL no es igual a una página

Al compilar una aplicación de formularios Web Forms de ASP.NET tradicional o una aplicación de páginas Active Server, hay una correspondencia uno a uno entre una dirección URL y una página. Si se solicita una página denominada SomePage.aspx en el servidor, a continuación, tenía mejor haber una página en disco llamado SomePage.aspx. Si no existe el archivo SomePage.aspx, obtendrá un feo **404 - página no encontrada** error.

Al compilar una aplicación ASP.NET MVC, en cambio, no hay ninguna correspondencia entre la dirección URL que se escribe en la barra de direcciones del explorador y los archivos que encuentra en la aplicación. En una aplicación ASP.NET MVC, una dirección URL corresponde a una acción de controlador en lugar de una página en el disco.

En una aplicación tradicional de ASP o ASP.NET, las solicitudes del explorador se asignan a las páginas. En una aplicación ASP.NET MVC, en cambio, las solicitudes del explorador se asignan a las acciones de controlador. Una aplicación de formularios Web Forms de ASP.NET está centrado en el contenido. Una aplicación de ASP.NET MVC, en cambio, está centrado en la lógica de la aplicación.

## <a name="understanding-aspnet-routing"></a>Descripción del enrutamiento de ASP.NET

Se asigna una solicitud de explorador a una acción de controlador a través de una característica del marco de ASP.NET denominada *enrutamiento de ASP.NET*. Enrutamiento ASP.NET se utiliza el marco de MVC de ASP.NET para *ruta* las solicitudes entrantes a las acciones del controlador.

Enrutamiento ASP.NET utiliza una tabla de rutas para controlar las solicitudes entrantes. Esta tabla de ruta se crea cuando se inicia por primera vez la aplicación web. La tabla de enrutamiento está configurado en el archivo Global.asax. El archivo de Global.asax MVC predeterminada se encuentra en la lista 1.

**Lista 1 - Global.asax**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample1.vb)]

Cuando se inicia primero una aplicación ASP.NET, la aplicación\_se llama al método Start(). En la lista 1, este método llama al método RegisterRoutes() y el método RegisterRoutes() crea la tabla de rutas predeterminadas.

La tabla de rutas predeterminadas se compone de una ruta. Esta ruta predeterminada interrumpe todas las solicitudes entrantes en tres segmentos (un segmento de dirección URL es nada entre barras diagonales). El primer segmento se asigna a un nombre de controlador, el segundo segmento se asigna a un nombre de acción y el segmento final se asigna a un parámetro pasado a la acción con el nombre de identificador.

Por ejemplo, considere la siguiente dirección URL:

/ Productos/detalles/3

Esta dirección URL se analiza en tres parámetros similar al siguiente:

Controlador = producto

Acción = detalles

Id. = 3

La ruta predeterminada definida en el archivo Global.asax incluye valores predeterminados para los tres parámetros. El valor predeterminado es de controlador de inicio, la acción predeterminada es el índice y el Id. predeterminado es una cadena vacía. Con estos valores predeterminados en mente, tenga en cuenta cómo se analiza la dirección URL siguiente:

/ Employee

Esta dirección URL se analiza en tres parámetros similar al siguiente:

Controlador = empleado

acción = índice

Id. =

Por último, si abre una aplicación de MVC de ASP.NET sin proporcionar cualquier dirección URL (por ejemplo, `http://localhost`), a continuación, se analiza la dirección URL similar al siguiente:

controlador = inicio

acción = índice

Id. =

La solicitud se enruta a la acción de Index() en la clase HomeController.

## <a name="understanding-controllers"></a>Descripción de controladores

Un controlador es responsable de controlar la manera en que un usuario interactúa con una aplicación de MVC. Un controlador contiene la lógica de control de flujo para una aplicación ASP.NET MVC. Un controlador determina qué respuesta para enviar a un usuario cuando un usuario realiza una solicitud del explorador.

Un controlador es simplemente una clase (por ejemplo, una clase de Visual Basic o C#). El ejemplo de aplicación de ASP.NET MVC incluye un controlador denominado HomeController.vb que se encuentra en la carpeta de controladores. El contenido del archivo HomeController.vb se reproduce en el listado 2.

**La lista 2 - HomeController.cs**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample2.vb)]

Observe que el HomeController tiene dos métodos denominados Index() y About(). Estos dos métodos corresponden a las dos acciones expuestas por el controlador. El índice de dirección URL /Home invoca el método de HomeController.Index() y la dirección URL /Home/sobre invoca el método HomeController.About().

Cualquier método público en un controlador se expone como una acción de controlador. Debe tener cuidado con esto. Esto significa que se puede invocar un método público contenido en un controlador de cualquier persona con acceso a Internet escribiendo la dirección URL correcta en un explorador.

## <a name="understanding-views"></a>Descripción de vistas

Las acciones de dos controlador expuestas por la clase HomeController, Index() y About(), ambos devuelven una vista. Una vista contiene el marcado HTML y el contenido que se envía al explorador. Una vista es el equivalente de una página cuando se trabaja con una aplicación ASP.NET MVC.

Debe crear las vistas en la ubicación correcta. La acción HomeController.Index() devuelve una vista ubicada en la ruta de acceso siguiente:

\Views\Home\Index.aspx

La acción HomeController.About() devuelve una vista ubicada en la ruta de acceso siguiente:

\Views\Home\About.aspx

En general, si desea obtener una vista de una acción de controlador, debe crear una subcarpeta en la carpeta vistas con el mismo nombre que el controlador. Dentro de la subcarpeta, debe crear un archivo .aspx con el mismo nombre que la acción del controlador.

El archivo en la lista 3 contiene la vista About.aspx.

**El listado 3 - About.aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-vb/samples/sample3.aspx)]

Si omite la primera línea en la lista 3, la mayoría del resto de la vista está formada por HTML estándar. Puede modificar el contenido de la vista escribiendo código HTML que desea aquí.

Una vista es muy similar a una página de formularios Web Forms de ASP.NET o de páginas Active Server. Una vista puede contener secuencias de comandos y contenido HTML. Puede escribir las secuencias de comandos en sus favoritos en .NET (por ejemplo, C# o Visual Basic. NET) de lenguaje de programación. Utilizar secuencias de comandos para mostrar contenido dinámico como la base de datos.

## <a name="understanding-models"></a>Descripción de los modelos

Hemos discutido los controladores y hemos discutido vistas. El último tema que necesitamos para explicar es los modelos. ¿Qué es un modelo MVC?

Un modelo MVC contiene toda la lógica de aplicación que no se encuentra en una vista o un controlador. El modelo debe contener toda la lógica de negocios de la aplicación, la lógica de validación y la lógica de acceso de la base de datos. Por ejemplo, si está utilizando Microsoft Entity Framework para tener acceso a la base de datos, podría crear las clases de Entity Framework (el archivo .edmx) en la carpeta de modelos.

Una vista debe contener solo aspectos lógicos relacionados con la generación de la interfaz de usuario. Un controlador solo debe contener la configuración mínima de la lógica necesaria para devolver la vista derecha o redirigir al usuario a otra acción (control de flujo). Todo lo demás debe incluirse en el modelo.

En general, debe procurar para modelos fat y controladores delgados. Los métodos de controlador deben contener solo unas pocas líneas de código. Si una acción de controlador obtiene demasiado fat, debe considerar mover la lógica de a una nueva clase en la carpeta de modelos.

## <a name="summary"></a>Resumen

Este tutorial proporciona una visión general de alto nivel de las diferentes partes de un ASP.NET MVC la aplicación web. Ha aprendido cómo el enrutamiento de ASP.NET asigna las solicitudes entrantes a las acciones del controlador determinado. Ha aprendido cómo controladores orquestar cómo se devuelven las vistas en el explorador. Por último, ha aprendido cómo modelos contienen empresariales de la aplicación, la validación y la lógica de acceso de la base de datos.
