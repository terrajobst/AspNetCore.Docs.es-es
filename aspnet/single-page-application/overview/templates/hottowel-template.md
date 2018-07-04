---
uid: single-page-application/overview/templates/hottowel-template
title: Plantilla de hot toalla | Microsoft Docs
author: madskristensen
description: Plantilla HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: de81f12f57d7f2fb7c6478bfa1f3a278ae905a39
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388556"
---
<a name="hot-towel-template"></a>Plantilla de hot Towel
====================
por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla de MVC toalla activo está escrita por John Papa
> 
> Elija qué versión desea descargar:
> 
> [Plantilla MVC toalla activo para Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Plantilla MVC toalla activo para Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)
> 
> 
> Hot toalla: Ya no quiere ir a la SPA sin uno!


¿Si desea crear una SPA, pero no se puede decidir dónde empezar? Utilizar una toalla activo y en segundos, tendrá una SPA y todas las herramientas que necesita para crear en él!

Hot toalla crea un excelente punto de partida para la creación de una aplicación de página única (SPA) con ASP.NET. De fábrica se proporciona una estructura modular para su código, navegación de la vista, el enlace de datos, administración de datos enriquecidos y estilo sencilla pero elegante. Hot toalla proporciona todo lo que necesita para crear una SPA, para que pueda centrarse en la aplicación, no las conexiones subyacentes.

> Más información acerca de cómo crear una SPA de [vídeos, tutoriales y cursos de Pluralsight de John Papa](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Estructura de la aplicación

Hot SPA toalla proporciona una carpeta de la aplicación que contiene los archivos JavaScript y HTML que definen la aplicación.

Dentro de la carpeta de aplicación:

- Durandal
- servicios
- modelos de vista
- vistas

La carpeta de aplicación contiene una colección de módulos. Estos módulos encapsulan la funcionalidad y declaran las dependencias en otros módulos. La carpeta views contiene el código HTML de la aplicación y la carpeta viewmodels contiene la lógica de presentación para las vistas (un patrón común en MVVM). La carpeta de servicios es ideal para los servicios comunes que la aplicación puede necesitar, como la recuperación de datos HTTP o la interacción de almacenamiento local de la caja. Es común para varios ViewModel reutilizar el código de los módulos del servicio.

## <a name="aspnet-mvc-server-side-application-structure"></a>Estructura de aplicación de lado servidor de ASP.NET MVC

Hot toalla se basa en la estructura de MVC de ASP.NET eficaces y familiar.

- Aplicación\_iniciar
- Contenido
- Controladores
- Modelos
- Scripts
- Vistas

## <a name="featured-libraries"></a>Bibliotecas de destacados

- ASP.NET MVC
- ASP.NET Web API
- Optimización de ASP.NET Web - unión y minificación
- [BREEZE.js](http://Breezejs.com) -administración de datos enriquecidos
- [Durandal.js](http://Durandaljs.com) -navegación y composición de la vista
- [Knockout.js](http://Knockoutjs.com) -enlaces de datos
- [Require.js](http://requirejs.org) -modularidad con AMD y optimización
- [Toastr.js](http://jpapa.me/c7toastr) -mensajes emergentes
- [Twitter Bootstrap](http://twitter.github.com/bootstrap/) : estilos de CSS sólido

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalar a través de la plantilla SPA toalla activo de Visual Studio 2012

Hot toalla puede instalarse como una plantilla de Visual Studio 2012. Simplemente haga clic en `File`  |  `New Project` y elija `ASP.NET MVC 4 Web Application`. A continuación, seleccione el "Hot toalla de aplicación de página única" plantilla y ejecute!

## <a name="installing-via-the-nuget-package"></a>Instalar mediante el paquete NuGet

Hot toalla también es un paquete de NuGet que aumenta en un proyecto vacío de ASP.NET MVC existente. Sólo tiene que instalar mediante Nuget y, a continuación, ejecute.

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>¿Cómo se compila en caliente toalla?

Simplemente empiece a agregar código.

1. Agregar su propio código del lado servidor, preferiblemente de Entity Framework y API Web (que realmente se lucen con Breeze.js)
2. Agregar vistas a la `App/views` carpeta
3. Agregar modelos de vista a la `App/viewmodels` carpeta
4. Agregar enlaces de datos HTML y Knockout a las nuevas vistas
5. Actualizar las rutas de navegación `shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Tutorial sobre la HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml es la ruta de inicio y la vista para la aplicación de MVC. Contiene todos los estándar etiquetas meta, vínculos de css y las referencias de JavaScript que cabría esperar. El cuerpo contiene un único `<div>` que es donde todo el contenido (las vistas) se colocarán cuando se solicitan. El `@Scripts.Render` Require.js se utiliza para ejecutar el punto de entrada para el código de la aplicación, que se encuentra en el `main.js` archivo. Se proporciona una pantalla de presentación para mostrar cómo crear una pantalla de presentación mientras se carga la aplicación.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main.js

El `main.js` archivo contiene el código que se ejecutará en cuanto se carga la aplicación. Esto es donde desea definir las rutas de navegación, establezca el inicio de las vistas y realizar cualquier programa de instalación o de arranque como desbloquear los datos de la aplicación.

El `main.js` archivo define varios de los módulos de durandal para ayudar a la puesta en marcha aplicaciones de inicio. La instrucción definir ayuda a resolver las dependencias de los módulos para que estén disponibles para la función. En primer lugar se habilitan los mensajes de depuración, que envían mensajes acerca de qué eventos de que la aplicación se está ejecutando en la ventana de consola. El código app.start indica el marco de trabajo de durandal para iniciar la aplicación. Las convenciones se establecen para que durandal sabe todas las vistas y viewmodels están incluidos en las mismas carpetas con nombre, respectivamente. Por último, el `app.setRoot` cargas se inicia el `shell` mediante una plantilla predeterminada `entrance` animación.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Vistas

Las vistas se encuentran en el `App/views` carpeta.

### <a name="shellhtml"></a>Shell.HTML

El `shell.html` contiene el diseño maestra para el código HTML. Todas las vistas se compone en algún lugar en el lado de su `shell` vista. Hot toalla proporciona un `shell` con tres de estas regiones: un encabezado, un área de contenido y un pie de página. Cada una de estas regiones se carga con contenido forma a otras vistas cuando se solicita.

El `compose` enlaces para el encabezado y pie de página están codificados en toalla activo para que apunte a la `nav` y `footer` vistas, respectivamente. El enlace de compose de la sección `#content` está enlazado a la `router` elemento activo del módulo. En otras palabras, al hacer clic en un vínculo de navegación es la vista correspondiente se carga en esta área.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV.HTML

El `nav.html` contiene los vínculos de navegación de la SPA. Esto es donde la estructura de menú se puede colocar, por ejemplo. Suele ser datos enlazados (con Knockout) a la `router` módulo para mostrar el panel de navegación que definió en el `shell.js`. Knockout busca el enlace de datos de atributos y enlaza los de la `shell` viewmodel para mostrar las rutas de navegación y para mostrar una barra de progreso (con Twitter Bootstrap) si el `router` módulo está en medio de navegar desde una vista a otra (consulte `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home.HTML y details.html

Estas vistas contienen HTML para las vistas personalizadas. Cuando el `home` vincular en el `nav` se hace clic en el menú de la vista, el `home` vista se colocará en el área de contenido de la `shell` vista. Estas vistas se pueden ampliar o reemplazadas con sus propias vistas personalizadas.

### <a name="footerhtml"></a>Footer.HTML

El `footer.html` contiene código HTML que aparece en el pie de página, en la parte inferior de la `shell` vista.

## <a name="viewmodels"></a>ViewModels

ViewModel se encuentra en el `App/viewmodels` carpeta.

### <a name="shelljs"></a>Shell.js

El `shell` viewmodel contiene propiedades y funciones que están enlazadas a la `shell` vista. A menudo esto es donde se encuentran los enlaces de navegación del menú (consulte la `router.mapNav` lógica).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js y details.js

Estos modelos de vista contienen las propiedades y funciones que están enlazadas a la `home` vista. También contiene la lógica de presentación para la vista y es el pegamento entre los datos y la vista.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Servicios

Los servicios se encuentran en la carpeta de aplicaciones y servicios. Lo ideal es que los servicios futuros como un módulo de servicio de datos, que es responsable de obtener y enviar los datos remotos, podrían colocarse.

### <a name="loggerjs"></a>Logger.js

Hot toalla proporciona un `logger` módulo en la carpeta de servicios. El `logger` módulo es ideal para registrar mensajes en la consola y al usuario en emergente notificaciones del sistema.
