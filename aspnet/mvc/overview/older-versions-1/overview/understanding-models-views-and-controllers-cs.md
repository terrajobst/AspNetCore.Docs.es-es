---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: Descripción de los modelos, vistas y controladores (C#) | Microsoft Docs
author: StephenWalther
description: ¿Tiene dudas acerca de los modelos, vistas y controladores? En este tutorial, Stephen Walther se presentan las distintas partes de una aplicación ASP.NET MVC.
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e9186a6c261266de8f1a1509a49b84b359bd920
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829025"
---
<a name="understanding-models-views-and-controllers-c"></a>Descripción de los modelos, vistas y controladores (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> ¿Tiene dudas acerca de los modelos, vistas y controladores? En este tutorial, Stephen Walther se presentan las distintas partes de una aplicación ASP.NET MVC.


Este tutorial proporciona una descripción general de ASP.NET MVC modelos, vistas y controladores. En otras palabras, explica la M', V' y C' en ASP.NET MVC.

Después de leer este tutorial, debe comprender cómo funcionan conjuntamente las distintas partes de una aplicación ASP.NET MVC. También debe entender cómo la arquitectura de una aplicación ASP.NET MVC difiere de una aplicación de páginas Active Server o la aplicación de formularios Web Forms de ASP.NET.

## <a name="the-sample-aspnet-mvc-application"></a>La aplicación de ejemplo ASP.NET MVC

La plantilla de Visual Studio de forma predeterminada para crear aplicaciones Web de ASP.NET MVC incluye una aplicación de ejemplo muy sencillo que puede usarse para comprender las diferentes partes de una aplicación ASP.NET MVC. Aprovechamos esta aplicación simple en este tutorial.

Crea una nueva aplicación MVC de ASP.NET con la plantilla MVC, inicie Visual Studio 2008 y seleccionar la opción de menú archivo, nuevo proyecto (consulte la figura 1). En el cuadro de diálogo nuevo proyecto, seleccione su lenguaje de programación favorito en tipos de proyecto (Visual Basic o C#) y seleccione **aplicación Web ASP.NET MVC** en plantillas. Haga clic en el botón Aceptar.


[![Cuadro de diálogo nuevo proyecto](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**Figura 01**: cuadro de diálogo nuevo proyecto ([haga clic aquí para ver imagen en tamaño completo](understanding-models-views-and-controllers-cs/_static/image2.png))


Cuando se crea una nueva aplicación de ASP.NET MVC, la **crear proyecto de prueba unitaria** cuadro de diálogo aparece (consulte la figura 2). Este cuadro de diálogo le permite crear un proyecto independiente de la solución para probar la aplicación ASP.NET MVC. Seleccione la opción **No, no cree un proyecto de prueba unitaria** y haga clic en el **Aceptar** botón.


[![Crear el cuadro de diálogo de pruebas unitarias](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**Figura 02**: crear unidad Probar cuadro de diálogo ([haga clic aquí para ver imagen en tamaño completo](understanding-models-views-and-controllers-cs/_static/image4.png))


Después de la nueva de ASP.NET MVC se crea la aplicación. Verá varias carpetas y archivos en la ventana Explorador de soluciones. En concreto, verá tres carpetas denominadas modelos, vistas y controladores. Como puede imaginar desde los nombres de carpeta, estas carpetas contienen los archivos para la implementación de modelos, vistas y controladores.

Si expande la carpeta Controllers, debería ver un archivo denominado AccountController.cs y un archivo denominado HomeController.cs. Si expande la carpeta Views, debería ver tres subcarpetas con el nombre de cuenta de inicio y compartido. Si expande la carpeta Inicio, verá dos archivos adicionales denominados About.aspx y Index.aspx (consulte la figura 3). Estos archivos constituyen la aplicación de ejemplo incluida con la plantilla predeterminada ASP.NET MVC.


[![La ventana del explorador de soluciones](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**Figura 03**: la ventana del explorador de soluciones ([haga clic aquí para ver imagen en tamaño completo](understanding-models-views-and-controllers-cs/_static/image6.png))


Puede ejecutar la aplicación de ejemplo seleccionando la opción de menú **depurar, Iniciar depuración**. Como alternativa, puede presionar la tecla F5.

Al ejecutar una aplicación ASP.NET en primer lugar, aparece el cuadro de diálogo en la figura 4 que se recomienda habilitar el modo de depuración. Haga clic en el botón Aceptar y la aplicación se ejecutará.


[![Cuadro de diálogo depuración no habilitada](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**Figura 04**: cuadro de diálogo de depuración no habilitada ([haga clic aquí para ver imagen en tamaño completo](understanding-models-views-and-controllers-cs/_static/image8.png))


Al ejecutar una aplicación ASP.NET MVC, Visual Studio inicia la aplicación en el explorador web. La aplicación de ejemplo consta de dos páginas: la página de índice y la página About. Cuando la aplicación se inicia por primera vez, aparece la página de índice (consulte la figura 5). Puede navegar a la página About haciendo clic en el vínculo del menú en la parte superior derecha de la aplicación.


[![La página de índice](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**Figura 05**: la página de índice ([haga clic aquí para ver imagen en tamaño completo](understanding-models-views-and-controllers-cs/_static/image11.png))


Tenga en cuenta las direcciones URL en la barra de direcciones del explorador. Por ejemplo, al hacer clic en el vínculo del menú About, la dirección URL en la barra de direcciones del explorador cambia a **/Home/About**.

Si cierra la ventana del explorador y vuelva a Visual Studio, no podrá encontrar un archivo con la página principal de la ruta de acceso/aproximadamente. Los archivos no existen. ¿Cómo es esto posible?

## <a name="a-url-does-not-equal-a-page"></a>Una dirección URL no es igual a una página

Al compilar una aplicación de formularios Web Forms de ASP.NET tradicional o una aplicación de páginas Active Server, hay una correspondencia uno a uno entre una dirección URL y una página. Si se solicita una página denominada SomePage.aspx desde el servidor, a continuación, sea una página en disco llamado SomePage.aspx. Si no existe el archivo SomePage.aspx, obtendrá un ugly **404 - página no encontrada** error.

Al compilar una aplicación ASP.NET MVC, en cambio, no hay ninguna correspondencia entre la dirección URL que se escribe en la barra de direcciones del explorador y los archivos que encontrará en la aplicación. En una aplicación ASP.NET MVC, una dirección URL corresponde a una acción de controlador en lugar de una página en disco.

En una aplicación ASP o ASP.NET tradicional, las solicitudes del explorador se asignan a las páginas. En cambio, en una aplicación ASP.NET MVC, las solicitudes del explorador se asignan a acciones del controlador. Una aplicación de formularios Web Forms de ASP.NET está centrado en el contenido. Una aplicación ASP.NET MVC, en cambio, está centrado en la lógica de la aplicación.

## <a name="understanding-aspnet-routing"></a>Descripción del enrutamiento de ASP.NET

Una solicitud de explorador se asigna a una acción de controlador a través de una característica de ASP.NET framework llama *enrutamiento de ASP.NET*. El enrutamiento de ASP.NET se usa el marco de ASP.NET MVC para *ruta* las solicitudes entrantes a acciones del controlador.

El enrutamiento de ASP.NET utiliza una tabla de rutas para controlar las solicitudes entrantes. Esta tabla de rutas se crea cuando se inicia por primera vez la aplicación web. La tabla de enrutamiento está configurado en el archivo Global.asax. El archivo Global.asax de MVC predeterminada se encuentra en el listado 1.

**Listado 1 - Global.asax**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

Cuando se inicia por primera vez una aplicación ASP.NET, la aplicación\_se llama al método Start(). En el listado 1, este método llama al método RegisterRoutes() y el método RegisterRoutes() crea la tabla de enrutamiento predeterminada.

La tabla de enrutamiento predeterminada consta de una ruta. Esta ruta predeterminada interrumpe todas las solicitudes entrantes en tres segmentos (un segmento de dirección URL es cualquier cosa entre barras diagonales). El primer segmento se asigna a un nombre de controlador, el segundo segmento se asigna a un nombre de acción y el segmento final se asigna a un parámetro pasado a la acción denominada Id.

Pongamos como ejemplo esta URL:

/ Product/detalles/3

Esta dirección URL se analiza en tres parámetros similar al siguiente:

Controlador = producto

Acción = detalles

Id. = 3

La ruta predeterminada definida en el archivo Global.asax incluye valores predeterminados para los tres parámetros. El valor predeterminado es de controlador de inicio, la acción predeterminada es el índice y el Id. predeterminado es una cadena vacía. Con estos valores predeterminados en mente, tenga en cuenta cómo se analiza la dirección URL siguiente:

Por empleado

Esta dirección URL se analiza en tres parámetros similar al siguiente:

Controlador = empleado

acción = índice

Id. =

Por último, si abre una aplicación de MVC de ASP.NET sin proporcionar cualquier dirección URL (por ejemplo, `http://localhost`), a continuación, se analiza la dirección URL similar al siguiente:

controlador = Home

acción = índice

Id. =

La solicitud se enruta a la acción de Index() en la clase HomeController.

## <a name="understanding-controllers"></a>Descripción de los controladores

Un controlador es responsable de controlar la manera en que un usuario interactúa con una aplicación MVC. Un controlador contiene la lógica de control de flujo para una aplicación ASP.NET MVC. Un controlador determina qué respuesta para enviar a un usuario cuando un usuario realiza una solicitud del explorador.

Un controlador es simplemente una clase (por ejemplo, una clase de Visual Basic o C#). El ejemplo de una aplicación ASP.NET MVC incluye un controlador denominado HomeController.cs ubicado en la carpeta Controllers. Se reproduce el contenido del archivo HomeController.cs en el listado 2.

**Listado 2 - HomeController.cs**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

Tenga en cuenta que el HomeController tiene dos métodos denominados Index() y About(). Estos dos métodos se corresponden con las dos acciones expuestas por el controlador. El índice de/Home URL invoca el método de HomeController.Index() y/Home de dirección URL/About invoca el método HomeController.About().

Cualquier método público en un controlador se expone como una acción de controlador. Deberá tener cuidado con esto. Esto significa que se puede invocar cualquier método público contenido en un controlador de cualquier persona con acceso a Internet escribiendo la dirección URL correcta en un explorador.

## <a name="understanding-views"></a>Descripción de vistas

Las acciones de dos controlador expuestas la clase HomeController, Index() y About(), ambos devuelven una vista. Una vista contiene el marcado HTML y el contenido que se envía al explorador. Una vista es el equivalente de una página cuando se trabaja con una aplicación ASP.NET MVC.

Debe crear las vistas en la ubicación correcta. La acción HomeController.Index() devuelve una vista ubicada en la ruta de acceso siguiente:

\Views\Home\Index.aspx

La acción HomeController.About() devuelve una vista ubicada en la ruta de acceso siguiente:

\Views\Home\About.aspx

En general, si desea devolver una vista para una acción de controlador, a continuación, deberá crear una subcarpeta en la carpeta vistas con el mismo nombre que el controlador. Dentro de la subcarpeta, debe crear un archivo .aspx con el mismo nombre que la acción del controlador.

El archivo en el listado 3 contiene la vista About.aspx.

**Listado 3 - About.aspx**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

Si omite la primera línea en el listado 3, la mayoría del resto de la vista consta de HTML estándar. Puede modificar el contenido de la vista escribiendo código HTML que desee aquí.

Una vista es muy similar a una página de formularios Web Forms de ASP.NET o de páginas Active Server. Una vista puede contener las secuencias de comandos y el contenido HTML. Puede escribir los scripts en sus favoritos en .NET (por ejemplo, C# o Visual Basic. NET) de lenguaje de programación. Usar secuencias de comandos para mostrar el contenido dinámico, como la base de datos.

## <a name="understanding-models"></a>Descripción de los modelos

Hemos analizado los controladores y analizamos las vistas. El último tema que necesitamos analizar es los modelos. ¿Qué es un modelo de MVC?

Un modelo MVC contiene toda la lógica de aplicación que no está contenida en una vista o un controlador. El modelo debe contener toda la lógica de negocios de la aplicación, la lógica de validación y la lógica de acceso de la base de datos. Por ejemplo, si usas Microsoft Entity Framework para tener acceso a la base de datos, podría crear las clases de Entity Framework (el archivo .edmx) en la carpeta Models.

Una vista debe contener sólo la lógica relacionada con la generación de la interfaz de usuario. Un controlador solo debe contener la configuración mínima de la lógica necesaria para devolver la vista derecha o redirigir al usuario a otra acción (control de flujo). Todo lo demás debe incluirse en el modelo.

En general, debe procurar para modelos fat y controladores delgados. Los métodos del controlador deben contener sólo unas pocas líneas de código. Si obtiene demasiado gorda una acción de controlador, debe considerar salir la lógica de una nueva clase en la carpeta Models.

## <a name="summary"></a>Resumen

Este tutorial proporciona una visión general de alto nivel de las distintas partes de ASP.NET MVC la aplicación web. Ha aprendido cómo el enrutamiento de ASP.NET asigna las solicitudes entrantes a acciones del controlador determinado. Ha aprendido cómo los controladores de organizar cómo las vistas se devuelven al explorador. Por último, ha aprendido cómo los modelos contienen empresariales de la aplicación, la validación y lógica de acceso de la base de datos.
