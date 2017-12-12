---
uid: single-page-application/overview/templates/hottowel-template
title: Plantilla de toallas activa | Documentos de Microsoft
author: madskristensen
description: Plantilla de HotTowel
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/09/2013
ms.topic: article
ms.assetid: 75af2e17-6ed3-4d24-8ea1-bc340027c318
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/hottowel-template
msc.type: authoredcontent
ms.openlocfilehash: bfc6e2c884c422f44e8be5f4f29554ae86f7ecb6
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="hot-towel-template"></a>Plantilla de toallas activa
====================
por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla de MVC de toallas activa esté escrita por John Papa
> 
> Elija qué versión desea descargar:
> 
> [Plantilla MVC de toallas activa para Visual Studio 2012](https://visualstudiogallery.msdn.microsoft.com/1f68fbe8-b4e9-4968-9fd3-ddc7cbc52dca)
> 
> [Plantilla MVC de toallas activa para Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/1eb8780d-d522-4dcf-bf56-56f0eab305c2)


> Toallas activa: Dado que no desea ir a la aplicación SPA sin una!


¿Desea crear un SPA pero no se puede decidir dónde empezar? Usar toallas activa y en segundos, tendrá un SPA y todas las herramientas que necesita para crear en ella.

Toallas activa crea un buen punto de partida para la creación de una aplicación de página única (SPA) con ASP.NET. De fábrica le proporciona una estructura modular de código, navegación por la vista, enlace de datos, administración de datos enriquecidos y un estilo sencillo y elegante. Toallas activa proporciona todo lo que necesita para compilar un SPA, de forma que pueda centrarse en la aplicación, no la mecánica.

> Más información acerca de cómo generar un SPA desde [John Papa vídeos, tutoriales y Pluralsight cursos](http://johnpapa.net/spa?vsix).


## <a name="application-structure"></a>Estructura de aplicación

Acceso rápido de toallas de SPA proporciona una carpeta de la aplicación que contiene los archivos JavaScript y HTML que definen la aplicación.

Dentro de la carpeta de la aplicación:

- durandal
- servicios
- ViewModels
- vistas

La carpeta de la aplicación contiene una colección de módulos. Estos módulos encapsulan la funcionalidad y declaran las dependencias en otros módulos. La carpeta views contiene el código HTML de la aplicación y la carpeta viewmodels contiene la lógica de presentación para las vistas (un modelo común de MVVM). La carpeta de servicios es ideal para albergar los servicios comunes que la aplicación puede necesitar, como la recuperación de datos HTTP o la interacción de almacenamiento local. Es habitual que varias viewmodels reutilizar el código de los módulos de servicio.

## <a name="aspnet-mvc-server-side-application-structure"></a>Estructura de aplicación de lado de servidor de ASP.NET MVC

Toallas activa se basa en la estructura de MVC de ASP.NET conocida y eficaces.

- Aplicación\_iniciar
- Contenido
- Controladores
- Modelos
- Scripts
- Vistas

## <a name="featured-libraries"></a>Bibliotecas destacadas

- ASP.NET MVC
- ASP.NET Web API
- Optimización de ASP.NET Web - agrupar y minificar
- [BREEZE.js](http://Breezejs.com) -administración de datos enriquecidos
- [Durandal.js](http://Durandaljs.com) -navegación y composición de la vista
- [Knockout.js](http://Knockoutjs.com) -enlaces de datos
- [Require.js](http://requirejs.org) -modularidad con la optimización y AMD
- [Toastr.js](http://jpapa.me/c7toastr) -mensajes emergentes
- [Arranque de Twitter](http://twitter.github.com/bootstrap/) : aplicar estilo de CSS sólido

## <a name="installing-via-the-visual-studio-2012-hot-towel-spa-template"></a>Instalar a través de la plantilla de Visual Studio 2012 toallas activa SPA

Toallas activa se pueden instalar como una plantilla de Visual Studio 2012. Simplemente haga clic en `File`  |  `New Project` y elija `ASP.NET MVC 4 Web Application`. A continuación, seleccione el ' activa toallas única página de aplicación "plantilla y ejecutar!

## <a name="installing-via-the-nuget-package"></a>Instalar mediante el paquete de NuGet

Toallas activa también es un paquete de NuGet que aumenta un proyecto de MVC de ASP.NET vacío existente. Solo tiene que instalar mediante Nuget y, a continuación, ejecute!

[!code-powershell[Main](hottowel-template/samples/sample1.ps1)]

## <a name="how-do-i-build-on-hot-towel"></a>¿Cómo se genera en toallas activa?

Simplemente empiece a agregar el código.

1. Agregar su propio código del lado servidor, preferiblemente Entity Framework y WebAPI (que realmente se destacan con Breeze.js)
2. Agregar vistas para la `App/views` carpeta
3. Agregar viewmodels a la `App/viewmodels` carpeta
4. Agregar enlaces de datos HTML y Knockout a las vistas nuevas
5. Actualizar las rutas de navegación de`shell.js`

## <a name="walkthrough-of-the-htmljavascript"></a>Tutorial de HTML/JavaScript

### <a name="viewshottowelindexcshtml"></a>Views/HotTowel/index.cshtml

index.cshtml es la ruta de inicio y la vista para la aplicación de MVC. Contiene todos los metaetiquetas estándar, vínculos de css y referencias de JavaScript que se esperaría. El cuerpo contiene un único `<div>` que es donde todo el contenido (las vistas) se colocará cuando las soliciten. El `@Scripts.Render` Require.js se utiliza para ejecutar el punto de entrada para el código de la aplicación, que se encuentra en el `main.js` archivo. Se proporciona una pantalla de presentación para mostrar cómo crear una pantalla de presentación mientras se carga la aplicación.

[!code-cshtml[Main](hottowel-template/samples/sample2.cshtml)]

### <a name="appmainjs"></a>App/Main.js

El `main.js` archivo contiene el código que se ejecutará en cuanto se carga la aplicación. Esto es donde desea definir sus rutas de navegación, establezca la vistas de inicio y realizar cualquier instalación/arranque como desbloquear los datos de aplicación.

El `main.js` archivo define varios módulos del durandal para ayudar a la puesta en marcha aplicación iniciar. La instrucción de definir ayuda a resolver las dependencias de módulos para que estén disponibles para la función. Los mensajes de depuración se habilitan por primera vez, que enviar mensajes sobre eventos que la aplicación está realizando en la ventana de consola. El código de app.start indica durandal framework para iniciar la aplicación. Las convenciones se establecen para que durandal sabe todas las vistas y viewmodels están incluidos en las mismas carpetas con nombre, respectivamente. Por último, el `app.setRoot` inicia, carga el `shell` mediante una plantilla predeterminada `entrance` animación.

[!code-javascript[Main](hottowel-template/samples/sample3.js)]

## <a name="views"></a>Vistas

Las vistas se encuentran en el `App/views` carpeta.

### <a name="shellhtml"></a>Shell.HTML

El `shell.html` contiene el diseño del patrón para el código HTML. Todas las demás vistas se compondrán en algún lugar en el lado de su `shell` vista. Toallas activa proporciona un `shell` con tres de estas regiones: un encabezado, un área de contenido y un pie de página. Cada una de estas regiones se ha cargado con contenido forma otras vistas cuando se solicita.

El `compose` enlaces para el encabezado y pie de página son difíciles de codificar de toallas activa para que apunte a la `nav` y `footer` vistas, respectivamente. El enlace de redacción de la sección `#content` está enlazado a la `router` elemento activo del módulo. En otras palabras, al hacer clic en un vínculo de navegación es vista correspondiente se carga en esta área.

[!code-html[Main](hottowel-template/samples/sample4.html)]

### <a name="navhtml"></a>NAV.HTML

El `nav.html` contiene los vínculos de navegación de la aplicación SPA. Esto es que la estructura de menú se puede colocar, por ejemplo. Con frecuencia, es datos enlazados (mediante Knockout) a la `router` módulo para mostrar el panel de navegación definida en el `shell.js`. Knockout busca el enlace de datos, atributos y enlaza los utilizados para la `shell` viewmodel para mostrar las rutas de navegación y para mostrar una barra de progreso (mediante arranque de Twitter) si el `router` módulo está en medio de navegar desde una vista a otra (consulte `router.isNavigating`).

[!code-html[Main](hottowel-template/samples/sample5.html)]

### <a name="homehtml-and-detailshtml"></a>Home.HTML y details.html

Estas vistas contienen HTML de vistas personalizadas. Cuando el `home` vincular en el `nav` se hace clic en el menú de la vista, la `home` vista se colocará en el área de contenido de la `shell` vista. Estas vistas se pueden aumentar o reemplazar con sus propias vistas personalizadas.

### <a name="footerhtml"></a>Footer.HTML

El `footer.html` contiene código HTML que aparece en el pie de página, en la parte inferior de la `shell` vista.

## <a name="viewmodels"></a>ViewModels

ViewModels se encuentran en el `App/viewmodels` carpeta.

### <a name="shelljs"></a>Shell.js

El `shell` viewmodel contiene propiedades y funciones que están enlazadas a la `shell` vista. Con frecuencia, es donde se encuentran los enlaces de navegación de menú (consulte la `router.mapNav` lógica).

[!code-javascript[Main](hottowel-template/samples/sample6.js)]

### <a name="homejs-and-detailsjs"></a>Home.js y details.js

Estos viewmodels contienen las propiedades y las funciones que están enlazadas a la `home` vista. También contiene la lógica de presentación de la vista y es el elemento aglutinante entre los datos y la vista.

[!code-javascript[Main](hottowel-template/samples/sample7.js)]

## <a name="services"></a>Servicios

Los servicios se encuentran en la carpeta/Servicios de aplicaciones. Lo ideal es que los servicios futuros como un módulo dataservice, que es responsable de obtener y enviar datos remotos, pudieron colocarse.

### <a name="loggerjs"></a>Logger.js

Toallas activa proporciona un `logger` módulo en la carpeta de servicios. El `logger` módulo es ideal para registrar los mensajes en la consola y para el usuario en emergente notificaciones del sistema.
