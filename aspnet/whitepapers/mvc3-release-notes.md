---
uid: whitepapers/mvc3-release-notes
title: ASP.NET MVC 3 | Documentos de Microsoft
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/06/2010
ms.topic: article
ms.assetid: f44c166e-7e91-48a0-a6f8-d9285f3594e5
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc3-release-notes
msc.type: content
ms.openlocfilehash: a86fae5698c54a71cb598f508aa91e7d96d1b409
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
- [Información general](#overview)
- [Notas sobre la instalación](#installation-notes)
- [Requisitos de software](#software-requirements)
- [Documentación](#documentation)
- [Soporte técnico](#support)
- [Actualizar un proyecto de ASP.NET MVC 2 a ASP.NET MVC 3 Tools Update](#upgrading)
- [ASP.NET MVC 3 Tools Update (12 de abril de 2011)](#tu-changes)

    - [Cuadro de diálogo "Agregar controlador" ahora puede scaffolding con código de acceso a datos y vistas](#tu-AddControllerDialog)
    - [Mejoras en la "ASP.NET MVC 3 nuevo proyecto" cuadro de diálogo](#tu-ImprovementsNewDialogBox)
    - [Plantillas de proyecto ahora incluyen Modernizr 1.7](#tu-Modernizr)
    - [Plantillas de proyecto incluyen versiones actualizadas de jQuery, jQuery UI y jQuery validación](#tu-UpdatedJQuery)
    - [Plantillas de proyecto ahora incluyen ADO.NET Entity Framework 4.1 como un paquete de NuGet preinstalado](#tu-EF)
    - [Plantillas de proyecto incluyen bibliotecas de JavaScript como paquetes NuGet preinstalados](#tu-JavaScriptLibsNuget)
    - [Problemas conocidos](#tu-KI)
- [ASP.NET MVC 3 RTM (13 de enero de 2011)](#MVC3RTM)

    - [Cambio: La versión actualizada de jQuery UI para 1.8.7](#RTM-1)
    - [Cambio: Cambiar el valor predeterminado ModelMetadataProvider nuevo a DataAnnotationsModelMetadataProvider](#RTM-2)
    - [Problema corregido: Pegar parte de una expresión de Razor que contiene los resultados de espacio en blanco en lo que se va a invertir](#RTM-3)
    - [Problema corregido: Cambiar el nombre de un archivo de Razor que se abre en el editor de deshabilita colores de sintaxis e IntelliSense](#RTM-4)
    - [Problemas conocidos](#RTM-KI)
    - [Cambios importantes](#RTM-BC)
- [ASP.NET MVC 3 Release Candidate 2 (10 de diciembre de 2010)](#_Toc2)

    - [Cambien de plantillas de proyecto para incluir 1.4.4 jQuery, jQuery validación 1.7 y jQuery UI 1.8.6y 1.8.6 de la interfaz de usuario](#_Toc2_1)
    - [Clase de agregado "AdditionalMetadataAttribute"](#_Toc2_2)
    - [Scaffolding de vista mejorada](#_Toc2_3)
    - [Método Html.Raw agregado](#_Toc2_3)
    - [Propiedad ha cambiado el nombre "Controller.ViewModel" y la propiedad de "Vista" en "ViewBag"](#_Toc2_4)
    - [Clase ha cambiado el nombre "ControllerSessionStateAttribute" a "SessionStateAttribute"](#_Toc2_5)
    - [RemoteAttribute ha cambiado el nombre de propiedad de "Campos" a "AdditionalFields"](#_Toc2_6)
    - [Cambiar el nombre "SkipRequestValidationAttribute" a "AllowHtmlAttribute"](#_Toc2_7)
    - [Método modificada "Html.ValidationMessage" para mostrar el primer mensaje de Error útiles](#_Toc2_8)
    - [Fijo @model declaración no agregar espacio en blanco al documento](#_Toc2_9)
    - [Propiedad agregada "FileExtensions" para motores de vista para admitir los nombres de archivo específicas del motor](#_Toc2_10)
    - [Aplicación auxiliar de "LabelFor" fijo para emitir el valor correcto para el atributo "For"](#_Toc2_11)
    - [Método fijo "RenderAction" dar prioridad de valores explícitos durante el enlace del modelo](#_Toc2_12)
    - [Cambios importantes](#_Toc2_BC)
    - [Problemas conocidos](#_Toc2_KI)
- [ASP.NET MVC 3 Release Candidate (9 de noviembre de 2010)](#TOC_ASP_NET_3_RC)

    - [Nuevas características de ASP.NET MVC 3 RC](#_Toc276711785)
    - [Administrador de paquetes de NuGet](#_Toc276711786)
    - [Mejorado "Nuevo proyecto" cuadro de diálogo](#_Toc276711787)
    - [Controladores sin sesión](#_Toc276711788)
    - [Nuevos atributos de validación](#_Toc276711789)
    - [Nuevas sobrecargas para los métodos de "LabelForModel" y "LabelFor"](#_Toc276711790)
    - [Acción secundaria caché de resultados](#_Toc276711791)
    - [Mejoras de cuadro de diálogo "Agregar la vista"](#_Toc276711792)
    - [Validación de solicitud específico](#_Toc276711793)
    - [Cambios importantes](#_Toc276711794)
    - [Problemas conocidos](#_Toc276711795)
- [ASP. Notas de versión Beta MVC 3 (6 de octubre de 2010)](#TOC_ASP_NET_3_Beta)

    - [Nuevas características en la versión Beta de ASP.NET MVC 3](#0.1__Toc274034215)
    - [Administrador de paquetes NuPack](#0.1__Toc274034216)
    - [Mejora el cuadro de diálogo nuevo proyecto](#0.1__Toc274034217)
    - [Forma simplificada para especificar fuertemente tipados modelos en las vistas de Razor](#0.1__Toc274034218)
    - [Compatibilidad con nuevos métodos de aplicación auxiliar de páginas Web ASP.NET](#0.1__Toc274034219)
    - [Soporte técnico de inyección de dependencia adicional](#0.1__Toc274034220)
    - [Nueva compatibilidad con Ajax de jQuery-Based discreto](#0.1__Toc274034221)
    - [Nueva compatibilidad con la validación de jQuery discreta](#0.1__Toc274034222)
    - [Nuevos indicadores de toda la aplicación para la validación de cliente y JavaScript discreto](#0.1__Toc274034223)
    - [Nueva compatibilidad con el código que se ejecuta antes de la ejecución de vistas](#0.1__Toc274034224)
    - [Nueva compatibilidad con la sintaxis de Razor VBHTML](#0.1__Toc274034225)
    - [Más Control Granular sobre ValidateInputAttribute](#0.1__Toc274034226)
    - [Aplicaciones auxiliares de convertir los caracteres de subrayado en guiones para nombres de atributo HTML especificados mediante objetos anónimos](#0.1__Toc274034227)
    - [Correcciones de errores](#0.1__Toc274034228)
    - [Cambios importantes](#0.1__Toc274034229)
    - [Problemas conocidos](#0.1__Toc274034230)
- [Declinación de responsabilidades](#0.1__Toc274034231)

<a id="overview"></a>
## <a name="overview"></a>Información general

Este documento describe la versión de ASP.NET MVC 3 RTM para Visual Studio 2010. ASP.NET MVC es un marco para desarrollar aplicaciones Web que utiliza el modelo Model-View-Controller (MVC). El instalador de ASP.NET MVC 3 incluye los siguientes componentes:

- Componentes de tiempo de ejecución de ASP.NET MVC 3
- Herramientas de ASP.NET MVC 3 para Visual Studio 2010
- Componentes de tiempo de ejecución de páginas Web ASP.NET
- Herramientas de Visual Studio 2010 de páginas Web ASP.NET
- Administrador de paquetes de Microsoft para .NET (NuGet)
- Una actualización de Visual Studio 2010 que habilita la compatibilidad con sintaxis Razor. (Para obtener más información, vea el artículo de Knowledge Base 2483190).

El conjunto completo de notas de la versión para cada versión preliminar de ASP.NET MVC 3 puede encontrarse en el sitio Web ASP.NET en la siguiente URL:

https://www.ASP.NET/Learn/whitepapers/mvc3-Release-Notes

<a id="installation-notes"></a>
## <a name="installation-notes"></a>Notas sobre la instalación

Para instalar ASP.NET MVC 3 RTM mediante el instalador de plataforma Web (Web PI), visite la página siguiente:

[https://www.Microsoft.com/Web/Gallery/Install.aspx?AppID=MVC3](https://www.microsoft.com/web/gallery/install.aspx?appid=MVC3)

Como alternativa, puede descargar al instalador de ASP.NET MVC 3 RTM para Visual Studio 2010 desde la página siguiente:

https://go.Microsoft.com/fwlink/?LinkId=208140

ASP.NET MVC 3 puede instalarse y se puede ejecutar en paralelo con ASP.NET MVC 2.

<a id="software-requirements"></a>
## <a name="software-requirements"></a>Requisitos de software

Los componentes de tiempo de ejecución de ASP.NET MVC 3 necesitan el software siguiente:

- .NET framework versión 4. 

    Herramientas de ASP.NET MVC 3 para Visual Studio 2010 necesitan el software siguiente:
- Visual Studio 2010 o Visual Web Developer 2010 Express.

<a id="documentation"></a>
## <a name="documentation"></a>Documentación

Documentación para ASP.NET MVC está disponible en el sitio Web de MSDN en la dirección URL siguiente:

[https://go.Microsoft.com/fwlink/?LinkId=205717](https://go.microsoft.com/fwlink/?LinkId=205717)

Tutoriales y otra información sobre ASP.NET MVC están disponibles en la página MVC del sitio Web de ASP.NET en la siguiente URL:

[https://www.ASP.NET/MVC/](../mvc/index.md)

<a id="support"></a>
## <a name="support"></a>Compatibilidad

Esta es una versión totalmente compatible. Puede encontrar información sobre cómo obtener soporte técnico en el [sitio Web de Microsoft Support](https://support.microsoft.com/).

También puede publicar preguntas sobre esta versión en el foro de ASP.NET MVC, donde los miembros de la Comunidad de ASP.NET son normalmente pueden ofrecer soporte técnico informal:

[https://forums.ASP.NET/1146.aspx](https://forums.asp.net/1146.aspx)

<a id="upgrading"></a>
## <a name="upgrading-an-aspnet-mvc-2-project-to-aspnet-mvc-3-tools-update"></a>Actualizar un proyecto de ASP.NET MVC 2 a ASP.NET MVC 3 Tools Update

ASP.NET MVC 3 puede instalarse paralelo con ASP.NET MVC 2 en el mismo equipo, lo cual ofrece flexibilidad de elegir cuándo se debe actualizar una aplicación de ASP.NET MVC 2 a ASP.NET MVC 3.

Para actualizar manualmente una aplicación existente de ASP.NET MVC 2 a la versión 3, haga lo siguiente:

1. Cree un nuevo proyecto de ASP.NET MVC 3 vacío en el equipo. Este proyecto contendrá algunos archivos que son necesarios para la actualización.
2. Copie los siguientes archivos desde el proyecto de ASP.NET MVC 3 en la ubicación correspondiente de su proyecto de ASP.NET MVC 2. Debe actualizar todas las referencias a la biblioteca de jQuery para reflejar el nuevo nombre de archivo (jQuery-1.5.1. js): 

    - /Views/Web.config
    - /Packages.config
    - / scripts /\*.js
    - / Temas de contenido / /\*.\*
3. Copia la *paquetes* carpeta en la raíz de la solución de proyecto de ASP.NET MVC 3 vacía en la raíz de la solución, que se encuentra en el directorio donde está ubicado archivo .sln de la solución.
4. Si el proyecto de ASP.NET MVC 2 contiene algún área, copie el archivo /Views/Web.config a la *vistas* carpeta de cada área.
5. En ambos archivos Web.config en el proyecto de ASP.NET MVC 2, globalmente buscar y reemplazar la versión de ASP.NET MVC. Busque lo siguiente: 

    [!code-console[Main](mvc3-release-notes/samples/sample1.cmd)]

    Reemplácelo con lo siguiente:

    [!code-console[Main](mvc3-release-notes/samples/sample2.cmd)]
6. En el Explorador de soluciones, elimine la referencia a *System.Web.Mvc* (que indica la DLL de la versión 2), a continuación, agregue una referencia a *System.Web.Mvc* (v3.0.0.0).
7. Agregue una referencia a System.Web.WebPages.dll y a System.Web.Helpers.dll. Estos ensamblados se encuentran en las siguientes carpetas: 

    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET MVC 3\Assemblies
    - %ProgramFiles%\ Microsoft ASP.NET\ASP.NET Web Pages\v1.0\Assemblies
8. En el Explorador de soluciones, haga clic en el nombre del proyecto y seleccione Descargar proyecto. A continuación, haga clic en el nombre del proyecto nuevo y seleccione Editar *ProjectName*.csproj.
9. Busque la *ProjectTypeGuids* elemento y reemplace {F85E285D-A4E0-4152-9332-AB1D724D3325} con {E53F8FEA-EAE0-44A6-8774-FFD645390401}.
10. Guardar los cambios, haga clic en el proyecto y, a continuación, seleccione recargar el proyecto.
11. En el archivo Web.config de la aplicación raíz, agregue las siguientes opciones para la *ensamblados* sección. 

    [!code-xml[Main](mvc3-release-notes/samples/sample3.xml)]
12. Si el proyecto hace referencia a las bibliotecas de terceros que están compiladas con ASP.NET MVC 2, agregue las siguientes resaltadas *bindingRedirect* elemento en el archivo Web.config en la raíz de la aplicación en el  *configuración* sección: 

    [!code-xml[Main](mvc3-release-notes/samples/sample4.xml)]

<a id="tu-changes"></a>
## <a name="changes-in-aspnet-mvc-3-tools-update"></a>Cambios en ASP.NET MVC 3 Tools Update

Esta sección describen los cambios realizados en la versión de ASP.NET MVC 3 Tools Update desde la versión RTM de ASP.NET MVC 3.

<a id="tu-AddControllerDialog"></a>
### <a name="add-controller-dialog-box-can-now-scaffold-controllers-with-views-and-data-access-code"></a>Cuadro de diálogo "Agregar controlador" ahora puede scaffolding con código de acceso a datos y vistas

La técnica scaffolding es una manera de generar rápidamente un controlador y vistas de la aplicación. Después de generar el código, puede editarlo para cumplir los requisitos del proyecto.

Para iniciar el *Agregar controlador* cuadro de diálogo en ASP.NET MVC 3, haga clic en el *controladores* carpeta *el Explorador de soluciones*, haga clic en *agregar*y, a continuación, haga clic en *controlador*. El cuadro de diálogo se ha mejorado para ofrecer opciones de scaffolding adicionales.

![](mvc3-release-notes/_static/image1.png)

Hay tres plantillas para scaffold disponibles de forma predeterminada.

#### <a name="empty-controller"></a>Controlador en blanco

Esta plantilla genera un archivo de controlador en blanco. Esta plantilla es equivalente a no activar *agregar métodos de acción para crear, editar detalles, eliminar los escenarios* en versiones anteriores de ASP.NET MVC. Si selecciona esta opción, no hay más opciones están disponibles.

#### <a name="controller-with-empty-readwrite-actions"></a>Controlador con acciones de lectura/escritura en blanco

Esta plantilla genera un archivo de controlador que tiene todos los métodos de acción necesarios pero ningún código de implementación en los métodos. Esta plantilla es equivalente a seleccionar *agregar métodos de acción para crear, editar detalles, eliminar los escenarios* en versiones anteriores de ASP.NET MVC. Si selecciona esta opción, no hay más opciones están disponibles.

#### <a name="controller-with-readwrite-actions-and-views-using-entity-framework"></a>Controlador con acciones de lectura/escritura y vistas, mediante Entity Framework

Esta plantilla le permite crear rápidamente una interfaz de usuario de la entrada de datos de trabajo. Genera código que controla una variedad de comunes requisitos y escenarios, como los siguientes:

- *Acceso a datos*. El código generado lee y escribe entidades en una base de datos. Funciona con el enfoque de Entity Framework Code First si elige una clase de contexto de datos existente o si permite que la plantilla de generar un nuevo *DbContext* clase. También funciona con el enfoque de la base de datos de Entity Framework First o Model First si elige otra *ObjectContext* clase.
- *Validación*. El código generado utiliza el enlace del modelo de ASP.NET MVC y funciones de metadatos para que los envíos de formularios se validan según las reglas declaradas en la clase de modelo. Esto incluye reglas de validación integradas, como el *requiere* y *StringLength* atributos y reglas de validación personalizadas.
- *Relaciones uno a varios*. Si define relaciones de clave externa de uno a varios entre las clases de modelo, el código generado producirá listas desplegables para seleccionar las entidades relacionadas. Por ejemplo, puede definir las siguientes clases de modelo según las directrices de Entity Framework Code First: 

    [!code-csharp[Main](mvc3-release-notes/samples/sample5.cs)]

    Cuando a continuación, aplique la técnica scaffolding un controlador para el *producto* (clase), sus vistas le permitirá a los usuarios elegir un *categoría* para cada objeto *producto* instancia.

    Esta plantilla permite opciones adicionales en el *Agregar controlador* cuadro de diálogo. Para *clase modelo*, puede elegir cualquier clase de modelo de la solución, que determina el tipo de datos que los usuarios podrán crear o editar:
- Si desea utilizar Entity Framework Code First, puede elegir cualquier clase de modelo.
- Si usas First de base de datos de Entity Framework o Entity Framework Model First, asegúrese de elegir una clase de entidad definida en el modelo conceptual.

Para *clase de contexto de datos*, podrá seleccionar estas opciones:

- Si desea usar Code First y no tienen ningún contexto de datos existente de clases, elija  *&lt;nuevo contexto de datos... &gt;*". Una clase de contexto de datos, a continuación, se generará automáticamente.
- Si desea usar Code First y tienen una clase de contexto de datos existente, elija aquí. Se actualizará para conservar la clase del modelo que seleccionó.
- Si usas Database First o Model First, elija la clase de contexto de objeto.

Para las vistas, elija el motor de vista que desea usar o elija Ninguno si no desea aplicar la técnica scaffolding todas las vistas.

Puede seleccionar avanzadas opciones para especificar opciones adicionales para las vistas generadas. Por ejemplo, puede elegir el diseño o la página maestra que se usará.

<a id="tu-ImprovementsNewDialogBox"></a>
### <a name="improvements-to-the-aspnet-mvc-3-new-project-dialog-box"></a>Mejoras en la "ASP.NET MVC 3 nuevo proyecto" cuadro de diálogo

El cuadro de diálogo que se usa para crear nuevos proyectos de ASP.NET MVC 3 incluye varias mejoras, como se muestra a continuación.

![](mvc3-release-notes/_static/image2.png)

#### <a name="new-intranet-project-template"></a>Nueva plantilla de "Proyecto de Intranet"

La lista de plantillas de proyecto incluye una nueva plantilla de aplicación de Intranet. Esta plantilla contiene la configuración para la creación de una aplicación web mediante la autenticación de Windows en lugar de autenticación de formularios. Dado que una aplicación de intranet requiere algunas configuraciones de IIS que no se encapsulan en una plantilla de proyecto, la plantilla incluye un archivo Léame con instrucciones sobre cómo hacer que la plantilla de proyecto de trabajo en IIS. Documentación para la nueva plantilla de aplicación de Intranet está disponible en el sitio Web MSDN en la dirección URL siguiente:

[https://msdn.Microsoft.com/en-us/library/gg703322 (VS.98).aspx](https://msdn.microsoft.com/en-us/library/gg703322(VS.98).aspx)

#### <a name="project-templates-are-now-html5-enabled"></a>Plantillas de proyecto ahora están habilitados de HTML5

El cuadro de diálogo nuevo proyecto contiene ahora una opción para agregar características específicas de HTML5 para las plantillas de proyecto. Al seleccionar la opción hace vistas generarse que contienen la nueva HTML5  *&lt;encabezado&gt;*,  *&lt;pie de página&gt;*, y  *&lt;navegación&gt;*  elementos.

Tenga en cuenta que las versiones anteriores de exploradores no admiten etiquetas específicas de HTML5. Para solucionar esta limitación, las plantillas de proyecto de HTML5 incluyen una referencia a la biblioteca de Modernizr. (Vea la sección siguiente).

<a id="tu-Modernizr"></a>
### <a name="project-templates-now-include-modernizr-17"></a>Plantillas de proyecto ahora incluyen Modernizr 1.7

Modernizr es una biblioteca de JavaScript que habilita la compatibilidad con CSS 3 y HTML5 en exploradores que aún no admite estas características. Esta biblioteca se incluye como un paquete de NuGet preinstalado en plantillas para proyectos de ASP.NET MVC 3. Para obtener más información acerca de Modernizr, consulte [http://www.modernizr.com/](http://www.modernizr.com/).

<a id="tu-UpdatedJQuery"></a>
### <a name="project-templates-include-updated-versions-of-jquery-jquery-ui-and-jquery-validation"></a>Plantillas de proyecto incluyen versiones actualizadas de jQuery, jQuery UI y jQuery validación

Las plantillas de proyecto ahora incluyen las siguientes versiones de los scripts de jQuery:

- jQuery 1.5.1
- jQuery 1.8 de validación
- jQuery UI 1.8.11

Estas bibliotecas se incluyen como paquetes de NuGet preinstalados.

<a id="tu-EF"></a>
### <a name="project-templates-now-include-adonet-entity-framework-41-as-a-pre-installed-nuget-package"></a>Plantillas de proyecto ahora incluyen ADO.NET Entity Framework 4.1 como un paquete de NuGet preinstalado

ADO.NET Entity Framework 4.1 incluye la característica de Code First. Code First es un nuevo patrón de desarrollo de ADO.NET Entity Framework que proporciona una alternativa a los patrones Database First y Model First existentes.

Código en primer lugar se centra en definir el modelo utilizando clases POCO ("sin formato antiguos objetos CLR") escritas en Visual Basic o C#. Estas clases, a continuación, se pueden asignar a una base de datos existente o se pueden utilizar para generar un esquema de base de datos. Se puede proporcionar una configuración adicional utilizando *DataAnnotations* atributos o mediante la API fluidas.

Documentación para el uso de código Firstwith ASP.NET MVC está disponible en el sitio Web ASP.NET en las direcciones URL siguientes:

[https://www.ASP.NET/MVC/tutorials/Getting-Started-with-mvc3-part1-CS](../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) [https://www.asp.net/entity-framework/tutorials/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application](../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)

<a id="tu-JavaScriptLibsNuget"></a>
### <a name="project-templates-include-javascript-libraries-as-pre-installed-nuget-packages"></a>Plantillas de proyecto incluyen bibliotecas de JavaScript como paquetes NuGet preinstalados

Cuando crea un nuevo proyecto de ASP.NET MVC 3, el proyecto incluye los archivos JavaScript se ha mencionado anteriormente (por ejemplo, la biblioteca de Modernizr), instálelos mediante NuGet en lugar de agregar directamente las secuencias de comandos a la carpeta de Scripts en la plantilla de proyecto contenido. Esto le permite usar NuGet para actualizar los scripts a la versión más reciente cuando se publiquen nuevas versiones de las secuencias de comandos.

Por ejemplo, dada la frecuencia de las nuevas versiones de jQuery, la versión de jQuery incluido en la plantilla de proyecto en algún momento será no está actualizada. Sin embargo, puesto que jQuery se incluye como un paquete de NuGet instalado, se le notificará en el cuadro de diálogo de NuGet cuando hay disponibles versiones más recientes de jQuery.

Dado que jQuery incluye el número de versión en el nombre de archivo, actualización jQuery a la versión más reciente también es necesario actualizar el  *&lt;script&gt;*  etiqueta que hace referencia al archivo de jQuery para usar el nuevo nombre de archivo. Otras bibliotecas de secuencia de comandos incluida no incluyen el número de versión en el nombre de la secuencia de comandos, por lo que se pueden actualizar más fácilmente a sus versiones más recientes.

<a id="tu-KI"></a>
## <a name="known-issues"></a>Problemas conocidos

- En algunos casos, puede producir un error de instalación con el error mensaje "Error en la instalación con el código de error (0 x 80070643)". Para obtener información acerca de cómo solucionar este problema, consulte [artículo de Knowledge Base 2531566](https://support.microsoft.com/kb/2531566).
- No el scaffolding para agregar un controlador de aplicar la técnica scaffolding entidades que aprovechan las ventajas de compatibilidad de herencia de entidad en Entity Framework. Por ejemplo, dada una base de *persona* clase que hereda un *Student* clase scaffolding el *estudiante* clase dará como resultado en el código generado que no se compila.
- Crear un nuevo proyecto de ASP.NET MVC 3 dentro de una carpeta de solución hace un *NullReferenceException* error. La solución consiste en crear el proyecto de ASP.NET MVC 3 en la raíz de la solución y, a continuación, muévalo a la carpeta de soluciones.
- IntelliSense para consultar la sintaxis Razor no funciona cuando se instala ReSharper. Si tiene ReSharper instalado y desea aprovechar las ventajas de la compatibilidad de IntelliSense de Razor en ASP.NET MVC 3, vea la entrada [Razor Intellisense y ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) en el blog de Hadi Hariri, que describe maneras de utilizarlos juntos hoy en día.
- Durante la instalación, el cuadro de diálogo de aceptación de términos de licencia muestra los términos de licencia en una ventana que es inferior a lo esperado.
- Cuando edita una vista Razor (.cshtml o. *vbhtml* archivo), las vistas. ASP.NET MVC 3 no incluye los fragmentos de código para las vistas de Razor... aspxselecting un fragmento de código para ASP.NET MVC mostrará fragmentos de código para
- Si instala ASP.NET MVC 3 para Visual Web Developer Express en un equipo donde no está instalado Visual Studio y después instala Visual Studio, debe volver a instalar ASP.NET MVC 3. Visual Studio y Visual Web Developer Express comparten componentes que se actualizarán con el instalador de ASP.NET MVC 3. El mismo problema se aplica si se instala ASP.NET MVC 3 para Visual Studio en un equipo que no tenga Visual Web Developer Express y, a continuación, instalar más adelante Visual Web Developer Express.

<a id="MVC3RTM"></a>
## <a name="changes-in-aspnet-mvc-3-rtm"></a>Cambios en ASP.NET MVC 3 RTM

Esta sección describe los cambios y correcciones realizadas en la versión RTM de ASP.NET MVC 3 desde la versión de RC2.

<a id="RTM-1"></a>
### <a name="change-updated-the-version-of-jquery-ui-to-187"></a>Cambio: La versión actualizada de jQuery UI para 1.8.7

Las plantillas de proyecto de MVC de ASP.NET para Visual Studio se han actualizado para incluir la versión más reciente de la biblioteca de interfaz de usuario de jQuery. Las plantillas también incluyen el conjunto mínimo de archivos de recursos requeridos por jQuery UI, como las CSS asociados y los archivos de imagen.

<a id="RTM-2"></a>
### <a name="change-changed-the-default-modelmetadataprovider-back-to-dataannotationsmodelmetadataprovider"></a>Cambio: Cambiar el valor predeterminado ModelMetadataProvider nuevo a DataAnnotationsModelMetadataProvider

La versión de RC2 de ASP.NET MVC 3 introdujo un *CachedDataAnnotationsMetadataProvider* clase que proporciona almacenamiento en caché en la parte superior existente *DataAnnotationsModelMetadataProvider* de clase como un mejora del rendimiento. Sin embargo, algunos errores se notifican con esta implementación, por lo que el cambio se ha revertido y se ha movido el proyecto de MVC futuros, que está disponible en [WebStack ASP.NET](https://github.com/aspnet/AspNetWebStack).

<a id="RTM-3"></a>
### <a name="fixed-pasting-part-of-a-razor-expression-that-contains-whitespace-results-in-it-being-reversed"></a>Problema corregido: Pegar parte de una expresión de Razor que contiene los resultados de espacio en blanco en lo que se va a invertir

En las versiones preliminares de ASP.NET MVC 3, al pegar una parte de una expresión de Razor que contiene espacios en blanco en un archivo Razor, se invierte la expresión resultante. Por ejemplo, considere el siguiente bloque de código Razor:

[!code-cshtml[Main](mvc3-release-notes/samples/sample6.cshtml)]

Si selecciona el texto "primera param" en el primer método y pegarlo como un argumento en el segundo método, el resultado es como sigue:

[!code-cshtml[Main](mvc3-release-notes/samples/sample7.cshtml)]

El comportamiento correcto es que debe dar como resultado de la operación de pegado en lo siguiente:

[!code-cshtml[Main](mvc3-release-notes/samples/sample8.cshtml)]

Este problema se corrigió en el lanzamiento de RTM, para que la expresión correctamente se conserva durante la operación de pegado.

<a id="RTM-4"></a>
### <a name="fixed-renaming-a-razor-file-that-is-opened-in-the-editor-disables-syntax-colorization-and-intellisense"></a>Problema corregido: Cambiar el nombre de un archivo de Razor que se abre en el editor de deshabilita colores de sintaxis e IntelliSense

Cambia el nombre de un archivo de Razor mediante el Explorador de soluciones, mientras que el archivo se abre en la ventana del editor, resaltado de sintaxis e IntelliSense para dejar de funcionar durante ese archivo. Esto se ha corregido para que el resaltado y IntelliSense se mantienen después de un cambio de nombre.

<a id="RTM-KI"></a>
## <a name="known-issues"></a>Problemas conocidos

- Si cierra Visual Studio 2010 SP1 Beta mientras está abierta la consola de administrador de paquetes de NuGet, Visual Studio se bloquea y los intentos de reinicio. Este problema se solucionará en la versión RTM de Visual Studio 2010 SP1.
- El instalador de ASP.NET MVC 3 sólo es capaz de instalar la versión inicial del Administrador de paquetes de NuGet. Después de haber instalado la versión inicial, NuGet se puede instalar y actualiza mediante el Administrador de extensiones de Visual Studio. Si ya tiene instalado NuGet, vaya a la Galería de extensión de Visual Studio para actualizar a la versión más reciente de NuGet.
- Crear un nuevo proyecto de ASP.NET MVC 3 dentro de una carpeta de solución hace un *NullReferenceException* error. La solución consiste en crear el proyecto de ASP.NET MVC 3 en la raíz de la solución y, a continuación, muévalo a la carpeta de soluciones.
- El programa de instalación puede tardar mucho más que en versiones anteriores de ASP.NET MVC que se complete. Esto es que sólo actualiza los componentes de Visual Studio 2010.
- IntelliSense para consultar la sintaxis Razor no funciona cuando se instala ReSharper. Si tiene ReSharper instalado y desea aprovechar las ventajas de la compatibilidad de IntelliSense de Razor en ASP.NET MVC 3, vea la entrada [Razor Intellisense y ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) en el blog de Hadi Hariri, que describe maneras de utilizarlos juntos hoy en día.
- CCSHTML y VBHTML vistas creadas con la versión Beta de ASP.NET MVC 3 no dispone de su acción de compilación establecida correctamente, con lo que permite ver estos tipos se omiten cuando se publica el proyecto. El valor de acción de compilación de estos archivos debe establecerse en "Contenido". ASP.NET MVC 3 RTM corrige este problema para los nuevos archivos, pero no la configuración correcta de los archivos existentes para un proyecto creado con las versiones preliminares.
- ![](mvc3-release-notes/_static/image3.png)
- Durante la instalación, el cuadro de diálogo de aceptación de términos de licencia muestra los términos de licencia en una ventana que sea menor que planteado. / li&gt;
- Al editar una vista Razor (archivo .cshtml), el elemento de menú de ir al controlador en Visual Studio no estará disponible y no hay ningún fragmentos de código.
- Si instala ASP.NET MVC 3 para Visual Web Developer Express en un equipo donde no está instalado Visual Studio y después instala Visual Studio, debe volver a instalar ASP.NET MVC 3. Visual Studio y Visual Web Developer Express comparten componentes que se actualizarán con el instalador de ASP.NET MVC 3. El mismo problema se aplica si se instala ASP.NET MVC 3 para Visual Studio en un equipo que no tenga Visual Web Developer Express y, a continuación, instalar más adelante Visual Web Developer Express.

<a id="RTM-BC"></a>
## <a name="breaking-changes"></a>Cambios importantes

- En versiones anteriores de ASP.NET MVC, acción filtros son crear por solicitud excepto en algunos casos. Este comportamiento era nunca un comportamiento garantizado pero simplemente un detalle de implementación y el contrato para filtros era tenerlos en cuenta sin estado. En ASP.NET MVC 3, los filtros se almacenan en caché una forma más agresiva. Por lo tanto, los filtros de acción personalizada que se almacenen de forma incorrecta el estado de instancia podrían dejar de funcionar.
- Ha cambiado el orden de ejecución para los filtros de excepción para los filtros de excepciones que tienen el mismo *orden* valor. En ASP.NET MVC 2 y versiones anteriores, los filtros de excepciones en el controlador que tienen el mismo *orden* valor tal como los de un método de acción se ejecutan antes que los filtros de excepción en el método de acción. Esto normalmente sería el caso cuando se aplican los filtros de excepción sin una especificado *orden* valor. En ASP.NET MVC 3, este pedido se ha invertido para que el controlador de excepciones más específico se ejecuta primero. Al igual que en versiones anteriores, si la *orden* propiedad se especifica explícitamente, los filtros se ejecutan en el orden especificado.
- Una nueva propiedad denominada *FileExtensions* se agregó a la *VirtualPathProviderViewEngine* clase base. Cuando ASP.NET busca una vista por ruta de acceso (no por nombre), se consideran solo vistas con una extensión de archivo contenidos en la lista especificada por esta nueva propiedad. Se trata de un cambio importante en las aplicaciones que se registra un proveedor de compilación personalizada con el fin de habilitar una extensión de archivo personalizadas para las vistas de formulario Web Forms y donde el proveedor hace referencia a esas vistas mediante el uso de una ruta de acceso completa en lugar de un nombre. La solución consiste en modificar el valor de la *FileExtensions* propiedad debe incluir la extensión de archivo personalizado.
- Implementaciones de generador de dispositivo personalizado que se implementan directamente la *IControllerFactory* interfaz debe proporcionar una implementación de la nueva *GetControllerSessionBehavior* método agrega a la interfaz en esta versión. En general, se recomienda no implementar esta interfaz directamente y en su lugar, derive su clase de *DefaultControllerFactory*.

<a id="_Toc2"></a>
## <a name="changes-in-aspnet-mvc-3-rc2"></a>Cambios en ASP.NET MVC 3 RC2

Esta sección describe los cambios realizados en la versión de ASP.NET MVC 3 RC2 desde la versión RC (nuevas características y correcciones de errores).

<a id="_Toc2_1"></a>
### <a name="project-templates-changed-to-include-jquery-144-jquery-validation-17-and-jquery-ui-186"></a>Cambien de plantillas de proyecto para incluir 1.4.4 jQuery, jQuery validación 1.7 y jQuery UI 1.8.6

Las plantillas de proyecto de ASP.NET MVC 3 ahora incluyen las versiones más recientes de jQuery y validación de jQuery, jQuery UI. jQuery UI es una novedad en las plantillas de proyecto y proporciona los widgets de interfaz de usuario útil. Para obtener más información acerca de jQuery UI, visite su página de inicio: [http://jqueryui.com/](http://jqueryui.com/).

<a id="_Toc2_2"></a>
### <a name="added-additionalmetadataattribute-class"></a>Clase de agregado "AdditionalMetadataAttribute"

Puede usar el *AdditionalMetadataAttribute* clase para rellenar el *ModelMetadata.AdditionalValues* diccionario para una propiedad de modelo.

Por ejemplo, suponga que un modelo de vista tiene propiedades que se deben mostrar sólo a un administrador. Ese modelo se pueden anotar con el nuevo atributo utilizando AdminOnly como la clave y true como el valor, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample9.cs)]

Estos metadatos debe ponerse a disposición a cualquier plantilla de pantalla o editor cuando se procesa un modelo de vista de producto. Depende de usted como desarrollador de aplicaciones para interpretar la información de metadatos.

<a id="_Toc2_3"></a>
### <a name="improved-view-scaffolding"></a>Scaffolding de vista mejorada

Las plantillas T4 utilizadas para las vistas de scaffolding ahora generan llamadas a métodos de aplicación auxiliar de plantilla como *EditorFor* en lugar de las aplicaciones auxiliares como *TextBoxFor*. Este cambio mejora la compatibilidad con metadatos en el modelo con el formato de los atributos de anotación de datos cuando el cuadro de diálogo Agregar vista genera una vista.

La técnica scaffolding agregar vista también incluye detección mejorada y el uso de información de clave principal en el modelo, en función de la convención. Por ejemplo, el cuadro de diálogo Agregar vista usa esta información para asegurarse de que el valor de clave principal no es scaffolding como un campo de formulario editable.

Las predeterminadas son editar y crear plantillas incluyen referencias a los scripts de jQuery necesarios para la validación del cliente.

<a id="_Toc2_4"></a>
### <a name="added-htmlraw-method"></a>Método Html.Raw agregado

De forma predeterminada, el código Razor ver motor codifica en HTML todos los valores. Por ejemplo, el siguiente fragmento de código codifica el código HTML dentro de la variable de saludo para que se muestre en la página como &amp;lt; seguro&amp;gt; ¡Hola mundo! &amp;lt; / strong&amp;gt;.

[!code-cshtml[Main](mvc3-release-notes/samples/sample10.cshtml)]

El nuevo *Html.Raw* método proporciona una manera sencilla de mostrar HTML sin codificar cuando el contenido se sabe que son seguros. En el ejemplo siguiente se muestra la misma cadena, pero la cadena se representa como marcado:

[!code-cshtml[Main](mvc3-release-notes/samples/sample11.cshtml)]

<a id="_Toc2_5"></a>
### <a name="renamed-controllerviewmodel-property-and-the-view-property-to-viewbag"></a>Propiedad ha cambiado el nombre "Controller.ViewModel" y la propiedad de "Vista" en "ViewBag"

Anteriormente, el *ViewModel* propiedad de *controlador* correspondía a la *vista* propiedad de una vista. Ambas propiedades proporcionan una manera en los valores de acceso de la *ViewDataDictionary* objeto mediante la sintaxis de descriptor de acceso de propiedad dinámica. Ambas propiedades se cambió para que sea el mismo para evitar confusiones y para que sean más coherentes.

<a id="_Toc2_6"></a>
### <a name="renamed-controllersessionstateattribute-class-to-sessionstateattribute"></a>Clase ha cambiado el nombre "ControllerSessionStateAttribute" a "SessionStateAttribute"

El *ControllerSessionStateAttribute* clase se introdujo en la versión RC de ASP.NET MVC 3. Se cambió el nombre de la propiedad para que sea más concisa.

<a id="_Toc2_7"></a>
### <a name="renamed-remoteattribute-fields-property-to-additionalfields"></a>RemoteAttribute ha cambiado el nombre de propiedad de "Campos" a "AdditionalFields"

El *RemoteAttribute* la clase *campos* propiedad causó confusión entre los usuarios. Cambiar el nombre de esta propiedad en *AdditionalFields* aclara su intención.

<a id="_Toc2_8"></a>
### <a name="renamed-skiprequestvalidationattribute-to-allowhtmlattribute"></a>Cambiar el nombre "SkipRequestValidationAttribute" a "AllowHtmlAttribute"

El *SkipRequestValidationAttribute* atributo ha cambiado a *AllowHtmlAttribute* para representar mejor su uso previsto.

<a id="_Toc2_9"></a>
### <a name="changed-htmlvalidationmessage-method-to-display-the-first-useful-error-message"></a>Método modificada "Html.ValidationMessage" para mostrar el primer mensaje de Error útiles

El *Html.ValidationMessage* método se corrigió para mostrar el primer mensaje de error útiles en lugar de mostrar simplemente el primer error.

Durante el enlace del modelo, el *ModelState* diccionario se puede rellenar desde varios orígenes con mensajes de error acerca de la propiedad, incluido el propio modelo (si implementa *IValidatableObject* ), de los atributos de validación que se aplican a la propiedad y de las excepciones producidas mientras se tiene acceso la propiedad.

Cuando el *Html.ValidationMessage* método muestra un mensaje de validación, omite las entradas de estado del modelo que incluyen una excepción, porque estos no están diseñados generalmente para el usuario final. En su lugar, el método busca el primer mensaje de validación que no está asociado con una excepción y muestra ese mensaje. Si este mensaje no se encuentra, el valor predeterminado es un mensaje de error genérico que está asociado a la primera excepción.

<a id="_Toc2_10"></a>
### <a name="fixed-model-declaration-to-not-add-whitespace-to-the-document"></a>Fijo @model declaración no agregar espacio en blanco al documento

En versiones anteriores, el  *@model*  declaración en la parte superior de una vista, agrega una línea en blanco a la salida HTML representada. Esto se ha solucionado para que la declaración no incluye un espacio en blanco.

<a id="_Toc2_11"></a>
### <a name="added-fileextensions-property-to-view-engines-to-support-engine-specific-file-names"></a>Propiedad agregada "FileExtensions" para motores de vista para admitir los nombres de archivo específicas del motor

Un motor de vista puede devolver una vista con una ruta de acceso de vista explícita como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample12.cs)]

El primer motor vista siempre intenta representar la vista. De forma predeterminada, el motor de vistas de formularios Web Forms es el primer motor de vista; Dado que el motor de formularios Web Forms no puede representar una vista Razor, se produce un error. Motores de vista ahora tienen un *FileExtensions* la propiedad que se utiliza para especificar las extensiones de archivo admiten. Esta propiedad se comprueba cuando ASP.NET determina si un motor de vista puede representar un archivo. Se trata de un cambio importante y se incluye más información en el [Breaking Changes](#_Toc2_BC) sección de este documento.

<a id="_Toc2_12"></a>
### <a name="fixed-labelfor-helper-to-emit-the-correct-value-for-the-for-attribute"></a>Aplicación auxiliar de "LabelFor" fijo para emitir el valor correcto para el atributo "For"

Se corrigió un error donde la *LabelFor* método representa un *para* atributo que coincida con el *entrada* del elemento *nombre* atributo en su lugar de su identificador. Según el W3C, la *para* atributo debe coincidir con el *entrada* Id. del elemento

<a id="_Toc2_13"></a>
### <a name="fixed-renderaction-method-to-give-explicit-values-precedence-during-model-binding"></a>Método fijo "RenderAction" dar prioridad de valores explícitos durante el enlace del modelo

En versiones anteriores, los valores explícitos que se pasaron a la *RenderAction* método se omitirá en favor de los valores del formulario actual durante el enlace del modelo dentro de una acción secundaria. La revisión garantiza que los valores explícitos tienen prioridad durante el enlace del modelo.

<a id="_Toc2_BC"></a>
## <a name="breaking-changes"></a>Cambios importantes

- En versiones anteriores de ASP.NET MVC, los filtros de acción se crearon por solicitud excepto en algunos casos. Este comportamiento era nunca un comportamiento garantizado pero simplemente un detalle de implementación y el contrato para filtros era tenerlos en cuenta sin estado. En ASP.NET MVC 3, los filtros se almacenan en caché una forma más agresiva. Por lo tanto, los filtros de acción personalizada que se almacenen de forma incorrecta el estado de instancia podrían dejar de funcionar.
- Ha cambiado el orden de ejecución para los filtros de excepción para los filtros de excepciones que tienen el mismo *orden* valor. En ASP.NET MVC 2 y versiones anteriores, los filtros de excepciones en el controlador que tenían el mismo *orden* valor tal como los de un método de acción se ejecutaron antes que los filtros de excepción en el método de acción. Esto normalmente sería el caso cuando se han aplicado filtros de excepción sin una especificado *orden* valor. En ASP.NET MVC 3, este pedido se ha invertido para que el controlador de excepciones más específico se ejecuta primero. Al igual que en versiones anteriores, si la *orden* propiedad se especifica explícitamente, los filtros se ejecutan en el orden especificado.
- Una nueva propiedad denominada *FileExtensions* se agregó a la *VirtualPathProviderViewEngine* clase base. Cuando ASP.NET busca una vista por ruta de acceso (no por nombre), se consideran solo vistas con una extensión de archivo contenidos en la lista especificada por esta nueva propiedad. Se trata de un cambio importante en las aplicaciones que se registra un proveedor de compilación personalizada con el fin de habilitar una extensión de archivo personalizadas para las vistas de formulario Web Forms y donde el proveedor hace referencia a esas vistas mediante el uso de una ruta de acceso completa en lugar de un nombre. La solución consiste en modificar el valor de la *FileExtensions* propiedad debe incluir la extensión de archivo personalizado.
- Implementaciones de generador de dispositivo personalizado que se implementan directamente la *IControllerFactory* interfaz debe proporcionar una implementación de la nueva *GetControllerSessionBehavior*  *método que se agregó a la interfaz en esta versión*. En general, se recomienda no implementar esta interfaz directamente y en su lugar, derive su clase de *DefaultControllerFactory*.

<a id="_Toc2_KI"></a>
## <a name="known-issues"></a>Problemas conocidos

- El instalador de ASP.NET MVC 3 sólo es capaz de instalar la versión inicial del Administrador de paquetes de NuGet. Después de haber instalado la versión inicial, NuGet se puede instalar y actualiza mediante el Administrador de extensiones de Visual Studio. Si ya tiene instalado NuGet, vaya a la Galería de extensión de Visual Studio para actualizar a la versión más reciente de NuGet.
- Crear un nuevo proyecto de ASP.NET MVC 3 dentro de una carpeta de solución hace un *NullReferenceException* error. La solución consiste en crear el proyecto de ASP.NET MVC 3 en la raíz de la solución y, a continuación, muévalo a la carpeta de soluciones.
- El programa de instalación puede tardar mucho más que en versiones anteriores de ASP.NET MVC que se complete. Esto es que sólo actualiza los componentes de Visual Studio 2010.
- IntelliSense para consultar la sintaxis Razor no funciona cuando se instala ReSharper. Si tiene ReSharper instalado y desea aprovechar las ventajas de la compatibilidad de IntelliSense de Razor en ASP.NET MVC 3 RC2, vea la entrada [Razor Intellisense y ReSharper](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) en el blog de Hadi Hariri, que describe maneras de utilizarlos juntos hoy en día.
- CSHTML y VBHTML vistas creadas con la versión Beta de ASP.NET MVC 3 no dispone de su acción de compilación establecida correctamente, con lo que permite ver estos tipos se omiten cuando se publica el proyecto. El *acción de compilación* el valor de estos archivos se deben establecer en contenido ". ASP.NET MVC 3 RC2 corrige este problema para los nuevos archivos, pero no la configuración correcta de los archivos existentes para un proyecto creado con la versión Beta.![](mvc3-release-notes/_static/image4.png)
- Durante la instalación, el cuadro de diálogo de aceptación de términos de licencia muestra los términos de licencia en una ventana que es inferior a lo esperado.
- Al editar una vista Razor (archivo .cshtml), el elemento de menú de ir al controlador en Visual Studio no estará disponible y no hay ningún fragmentos de código.
- Si instala ASP.NET MVC 3 para Visual Web Developer Express en un equipo donde no está instalado Visual Studio y después instala Visual Studio, debe volver a instalar ASP.NET MVC 3. Visual Studio y Visual Web Developer Express comparten componentes que se actualizarán con el instalador de ASP.NET MVC 3. El mismo problema se aplica si se instala ASP.NET MVC 3 para Visual Studio en un equipo que no tenga Visual Web Developer Express y, a continuación, instalar más adelante Visual Web Developer Express.
- Instalar ASP.NET MVC 3 RC 2 no actualiza NuGet si ya tiene instalado. Para actualizar NuGet, vaya al administrador de extensión de Visual Studio y se debería aparecer como una actualización disponible. Puede actualizar NuGet a la versión más reciente desde allí.

<a id="TOC_ASP_NET_3_RC"></a>
## <a name="aspnet-mvc-3-release-candidate"></a>Candidato de versión comercial de ASP.NET MVC 3

Candidato de versión comercial de ASP.NET MVC se publicó en 9 de noviembre de 2010.

<a id="_Toc276711785"></a>
## <a name="new-features-in-aspnet-mvc-3-rc"></a>Nuevas características de ASP.NET MVC 3 RC

Esta sección describen las características que se han introducido en la versión de ASP.NET MVC 3 RC desde la versión Beta.

<a id="_Toc276711786"></a>
### <a name="nuget-package-manager"></a>Administrador de paquetes de NuGet

ASP.NET MVC 3 incluye el Administrador de paquetes de NuGet (anteriormente conocido como NuPack), que es una herramienta de administración del paquete integrado para agregar bibliotecas y herramientas para proyectos de Visual Studio. Esta herramienta automatiza los pasos que los desarrolladores realizar hoy en día para obtener una biblioteca en el árbol de origen.

También puede trabajar con NuGet como una herramienta de línea de comandos, como una ventana de consola integrada dentro de Visual Studio 2010, en el menú contextual de Visual Studio y como un conjunto de cmdlets de PowerShell.

Para obtener más información sobre NuGet, lea la [documentación de Nuget](https://docs.microsoft.com/nuget/).

<a id="_Toc276711787"></a>
### <a name="improved-new-project-dialog-box"></a>Mejorado "Nuevo proyecto" cuadro de diálogo

Cuando crea un nuevo proyecto, el cuadro de diálogo nuevo proyecto ahora permite especificar el motor de vista, así como un tipo de proyecto de MVC de ASP.NET.

![](mvc3-release-notes/_static/image5.png)

Soporte técnico para modificar la lista de plantillas y motores enumeran en el cuadro de diálogo de vista se incluye en esta versión.

Las plantillas predeterminadas son los siguientes:

Vacía. Contiene un conjunto mínimo de archivos para un proyecto de MVC de ASP.NET, incluida la estructura de directorios predeterminada para los proyectos de MVC de ASP.NET, un archivo Site.css que contiene el valor predeterminado que estilos de ASP.NET MVC y un directorio de Scripts que contiene los archivos de JavaScript de forma predeterminada.

Aplicación de Internet. Contiene la funcionalidad de ejemplo que muestra cómo utilizar el proveedor de pertenencia a ASP.NET MVC.

La lista de plantillas de proyecto que se muestra en el cuadro de diálogo se especifica en el registro de Windows.

<a id="_Toc276711788"></a>
### <a name="sessionless-controllers"></a>Controladores sin sesión

El nuevo *ControllerSessionStateAttribute* ofrece un mayor control sobre el comportamiento de estado de sesión para los controladores mediante la especificación de un [System.Web.SessionState.SessionStateBehavior](https://msdn.microsoft.com/en-us/library/system.web.sessionstate.sessionstatebehavior.aspx) valor de enumeración.

En el ejemplo siguiente se muestra cómo desactivar el estado de sesión para todas las solicitudes a un controlador.

[!code-csharp[Main](mvc3-release-notes/samples/sample13.cs)]

En el ejemplo siguiente se muestra cómo establecer el estado de sesión de solo lectura para todas las solicitudes a un controlador.

[!code-csharp[Main](mvc3-release-notes/samples/sample14.cs)]

<a id="_Toc276711789"></a>
### <a name="new-validation-attributes"></a>Nuevos atributos de validación

#### <a name="compareattribute"></a>CompareAttribute

El nuevo *CompareAttribute* atributo de validación le permite comparar los valores de dos propiedades diferentes de un modelo. En el ejemplo siguiente, la *ComparePassword* propiedad debe coincidir con el *contraseña* campo para que sean válidos.

[!code-csharp[Main](mvc3-release-notes/samples/sample15.cs)]

#### <a name="remoteattribute"></a>RemoteAttribute

El nuevo *RemoteAttribute* atributo de validación aprovecha las ventajas de validador remoto de jQuery validación del complemento, que habilita la validación del lado cliente llamar a un método en el servidor que ejecuta la lógica de validación real.

En el ejemplo siguiente, la *nombre de usuario* propiedad tiene el *RemoteAttribute* aplicado. Al editar esta propiedad en una vista de edición, validación del cliente llamará a una acción denominada *UserNameAvailable* en el *UsersController* clase para validar este campo.

[!code-csharp[Main](mvc3-release-notes/samples/sample16.cs)]

En el ejemplo siguiente se muestra el controlador correspondiente.

[!code-csharp[Main](mvc3-release-notes/samples/sample17.cs)]

De forma predeterminada, se envía el nombre de propiedad que se aplica el atributo al método de acción como un parámetro de cadena de consulta.

<a id="_Toc276711790"></a>
### <a name="new-overloads-for-labelfor-and-labelformodel-methods"></a>Nuevas sobrecargas para los métodos de "LabelForModel" y "LabelFor"

Se han agregado nuevas sobrecargas para el *LabelFor* y *LabelForModel* métodos que le permiten especifican el texto del rótulo. En el ejemplo siguiente se muestra cómo usar estas sobrecargas.

[!code-cshtml[Main](mvc3-release-notes/samples/sample18.cshtml)]

<a id="_Toc276711791"></a>
### <a name="child-action-output-caching"></a>Acción secundaria caché de resultados

El *OutputCacheAttribute* admite la caché de resultados de las acciones secundarias que se les llama usando la *Html.RenderAction* o *Html.Action* métodos auxiliares. En el ejemplo siguiente se muestra una vista que llama a otra acción.

[!code-cshtml[Main](mvc3-release-notes/samples/sample19.cshtml)]

El *GetDate* acción se anota con el *OutputCacheAttribute*:

[!code-csharp[Main](mvc3-release-notes/samples/sample20.cs)]

Cuando se ejecuta este código, se almacena en caché el resultado de la llamada a Html.Action("GetDate") durante 100 segundos.

<a id="_Toc276711792"></a>
### <a name="add-view-dialog-box-improvements"></a>Mejoras de cuadro de diálogo "Agregar la vista"

Cuando se agrega una vista fuertemente tipada, el cuadro de diálogo Agregar vista filtra ahora más tipos no sea aplicable que en versiones anteriores, por ejemplo, muchos tipos de .NET Framework core. Además, la lista está ordenada ahora por el nombre de clase y no por el nombre de tipo completo, que resulta más fácil encontrar tipos. Por ejemplo, el nombre del tipo aparece ahora como en el ejemplo siguiente:

ClassName (espacio de nombres)

En versiones anteriores, esto habría se muestra como el siguiente:

Namespace.ClassName

<a id="_Toc276711793"></a>
### <a name="granular-request-validation"></a>Validación de solicitud específico

El *excluir* propiedad de *ValidateInputAttribute* ya no existe. En su lugar, para que la validación de solicitudes omitida de determinadas propiedades de un modelo durante el enlace de modelos, use la nueva *SkipRequestValidationAttribute*.

Por ejemplo, suponga que un método de acción se usa para editar una entrada de blog:

[!code-csharp[Main](mvc3-release-notes/samples/sample21.cs)]

En el ejemplo siguiente se muestra el modelo de vista para una entrada de blog.

[!code-csharp[Main](mvc3-release-notes/samples/sample22.cs)]

Cuando un usuario envía algún marcado para la propiedad de descripción, se producirá un error de enlace del modelo debido a la validación de solicitudes. Para deshabilitar la validación de solicitudes durante el enlace del modelo para la descripción de la entrada de blog, aplicar el *SkipRequpestValidationAttribute* a la propiedad, como se muestra en este ejemplo:.

[!code-csharp[Main](mvc3-release-notes/samples/sample23.cs)]

Como alternativa, para desactivar la validación de solicitudes para cada propiedad del modelo, aplicar *ValidateInputAttribute* con un valor de *false* al método de acción:

[!code-csharp[Main](mvc3-release-notes/samples/sample24.cs)]

<a id="_Toc276711794"></a>
## <a name="breaking-changes"></a>Cambios importantes

- Ha cambiado el orden de ejecución para los filtros de excepción para los filtros de excepciones que tienen el mismo *orden* valor. En ASP.NET MVC 2 y versiones anteriores, los filtros de excepciones en el controlador que tenían el mismo *orden* a medida que los de un método de acción se ejecutan antes que los filtros de excepción en el método de acción. Esto normalmente sería el caso cuando se han aplicado filtros de excepción sin una especificado *orden* valor. En ASP.NET MVC 3, este pedido se ha invertido para que el controlador de excepciones más específico se ejecuta primero. Al igual que en versiones anteriores, si la *orden* propiedad se especifica explícitamente, los filtros se ejecutan en el orden especificado.
- Agrega una nueva propiedad denominada *FileExtensions* a la *VirtualPathProviderViewEngine* clase base. Al buscar una vista por ruta de acceso (y no por nombre), sólo las vistas con una extensión de archivo contenidos en la lista especificada por esta nueva propiedad se considera. Esto es una novedad para aquellos que se registren un personalizado crear proveedor para habilitar una extensión de archivo personalizado para vistas de formulario web y y hacen referencia a esas vistas mediante el uso de una ruta de acceso completa en lugar de un nombre. La solución consiste en modificar el valor de la *FileExtensions* propiedad debe incluir la extensión de archivo personalizado.

<a id="_Toc276711795"></a>
## <a name="known-issues"></a>Problemas conocidos

- El programa de instalación puede tardar mucho más que en versiones anteriores de ASP.NET MVC en completarse que sólo actualiza los componentes de Visual Studio 2010.
- La técnica scaffolding agregar vista al seleccionar astrongly escrito scaffolds ver las propiedades de solo escritura. Éstos siempre se deberían omitir mediante scaffolding. El cuadro de diálogo Agregar vista también scaffolds propiedades de solo lectura al generar una vista "Edit" o "Crear". Solo se deberían scaffolding propiedades de solo lectura para las vistas de lista y mostrar.
- La depuración no funciona cuando se instala ASP.NET MVC 3 junto con la versión de CTP Async. ASP.NET MVC 3 no puede ser instalado en paralelo con la versión de CTP Async. Desinstale la versión de Async CTP para reparar la depuración. Para obtener más información, lea [esta entrada de blog](http://drew-prog.blogspot.com/2010/11/how-to-uninstall-microsoft-aspnet-mvc-3.html) acerca de cómo desinstalar todas las piezas de ASP.NET MVC 3 RC.
- Razor Intellisense no funciona cuando se instala Resharper. Si tiene ReSharper instalado y desea aprovechar las ventajas de la compatibilidad de intellisense de Razor en ASP.NET MVC 3 RC, lea [esta entrada de blog](http://blogs.jetbrains.com/dotnet/2010/11/razor-intellisense-and-resharper/) de JetBrains que aborda formas de utilizarlos juntos hoy en día.
- CSHTML y VBHTML vistas creadas con la versión Beta de ASP.NET MVC 3 no tiene su acción de compilación correctamente que ellos omite de la publicación. El *acción de compilación* para estos archivos se deben establecer en "Contenido". ASP.NET MVC 3 RC corrige este problema para los nuevos archivos, pero no la configuración correcta de los archivos existentes para un proyecto creado con la versión Beta.
- El programa de instalación puede tardar mucho más que en versiones anteriores de ASP.NET MVC en completarse que sólo actualiza los componentes de Visual Studio 2010.
- El scaffolding de agregar vista cuando seleccionando un "Edit" fuertemente tipados vista scaffolds propiedades de sólo lectura. Del mismo modo, las propiedades de solo escritura se scaffolding para las vistas de "Presentación".
- Durante la instalación, el cuadro de diálogo de aceptación de términos de licencia muestra los términos de licencia en una ventana que es inferior a lo esperado.
- Instalar el [CTP de Visual Studio Async](https://www.microsoft.com/downloads/en/details.aspx?FamilyID=18712f38-fcd2-4e9f-9028-8373dc5732b2&amp;displaylang=en) provoca un conflicto con la versión de Razor que se incluye como parte de la instalación de herramientas de ASP.NET MVC 3. Asegúrese de que no intente instalar la versión de CTP de Visual Studio Async y la versión de Razor en el mismo equipo.
- Al editar una vista Razor (archivo .cshtml), el elemento de menú de ir al controlador en Visual Studio no estará disponible y no hay ningún fragmentos de código.

<a id="TOC_ASP_NET_3_Beta"></a>
## <a name="aspnet-mvc-3-beta"></a>Versión Beta de ASP.NET MVC 3

Versión Beta de ASP.NET MVC 3 se publicó en 6 de octubre de 2010. Las notas siguientes son específicas de la versión Beta y están sujetos a las actualizaciones o los cambios que se hace referencia en la sección de ASP.NET MVC 3 Release Candidate anterior.

## <a id="0.1__Toc274034215"></a>Nueva versión Beta de ASP.NET MVC 3 de funciones

<a id="0.1__Default_validation_system"></a>Esta sección describen las características que se han introducido en la versión Beta de ASP.NET MVC 3.

### <a id="0.1__Toc274034216"></a>Administrador de paquetes de NuGet

ASP.NET MVC 3 incluye Administrador de paquetes de NuGet, que es una herramienta de administración del paquete integrado para agregar las bibliotecas y herramientas para proyectos de Visual Studio. En su mayor parte, automatiza los pasos que los desarrolladores realizar hoy en día para obtener una biblioteca en el árbol de origen.

También puede trabajar con NuGet como una herramienta de línea de comandos, como una ventana de consola integrada dentro de Visual Studio 2010, en el menú contextual de Visual Studio y como conjunto de cmdlets de PowerShell.

Para obtener más información sobre NuGet, lea la [documentación de NuGet](https://docs.microsoft.com/nuget/).

### <a id="0.1__Toc274034217"></a>Mejora el cuadro de diálogo nuevo proyecto

Cuando crea un nuevo proyecto, el cuadro de diálogo nuevo proyecto ahora permite especificar el motor de vista, así como un tipo de proyecto de MVC de ASP.NET.

![](mvc3-release-notes/_static/image6.png)

Soporte técnico para modificar la lista de plantillas y motores enumeran en el cuadro de diálogo de vista no se incluye en esta versión.

Las plantillas predeterminadas son los siguientes:

Vacía. Contiene un conjunto mínimo de archivos para un proyecto de MVC de ASP.NET, incluida la estructura de directorios predeterminada para los proyectos de MVC de ASP.NET, un pequeño archivo Site.css que contiene el valor predeterminado que estilos de ASP.NET MVC y un directorio de Scripts que contiene los archivos de JavaScript de forma predeterminada.

Aplicación de Internet. Contiene la funcionalidad de ejemplo que muestra cómo utilizar el proveedor de pertenencia de ASP.NET MVC.

### <a id="0.1__Toc274034218"></a>Forma simplificada para especificar fuertemente tipados modelos en las vistas de Razor

Se ha simplificado la forma de especificar el tipo de modelo para las vistas de Razor fuertemente tipados mediante el uso de la nueva @model directivas para las vistas CSHTML y @ModelType la directiva para las vistas VBHTML. En versiones anteriores de ASP.NET MVC, debe especificar que un modelo fuertemente tipado para Razor ve así:

[!code-cshtml[Main](mvc3-release-notes/samples/sample25.cshtml)]

En esta versión, puede usar la sintaxis siguiente:

[!code-cshtml[Main](mvc3-release-notes/samples/sample26.cshtml)]

### <a id="0.1__Toc274034219"></a>Compatibilidad con nuevos métodos de aplicación auxiliar de páginas Web ASP.NET

La nueva tecnología de ASP.NET Web Pages incluye un conjunto de métodos auxiliares que resultan útiles para agregar funcionalidad de uso frecuente a controladores y vistas. ASP.NET MVC 3 admite el uso de estos métodos auxiliares en controladores y vistas (cuando proceda). Estos métodos se encuentran en el ensamblado System.Web.Helpers. En la tabla siguiente se enumera algunos de los métodos de aplicación auxiliar de ASP.NET Web Pages.

| **Aplicación auxiliar** | **Descripción** |
| --- | --- |
| Chart | Representa un gráfico dentro de una vista. Contiene métodos como Chart.ToWebImage, Chart.Save y Chart.Write. |
| Clave criptográfica | Utiliza algoritmos para crear correctamente hash salt y el valor hash de contraseñas. |
| WebGrid | Representa una colección de objetos (normalmente, los datos de una base de datos) como una cuadrícula. Admite la paginación y la ordenación. |
| WebImage | Representa una imagen. |
| Mensaje de correo | Envía un mensaje de correo electrónico. |

Un tema de referencia rápida que enumera las aplicaciones auxiliares y la sintaxis básica está disponible como parte de la documentación de la sintaxis de ASP.NET Razor en la siguiente URL:

[https://www.ASP.NET/WebMatrix/tutorials/ASP-NET-Web-Pages-API-Reference](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)

### <a id="0.1__Toc274034220"></a>Soporte técnico de inyección de dependencia adicional

Basándose en la versión de ASP.NET MVC 3 Preview 1, la versión actual incluye agregó compatibilidad para dos nuevos servicios y cuatro servicios existentes y compatibilidad mejorada para la resolución de dependencia y el localizador de servicios común.

#### <a name="new-icontrolleractivator-interface-for-fine-grained-controller-instantiation"></a>Nueva interfaz IControllerActivator para la creación de instancias de controlador específica

La nueva interfaz de IControllerActivator proporciona un control más minucioso sobre cómo se crean instancias de controladores a través de la inserción de dependencias. En el ejemplo siguiente se muestra la interfaz:

[!code-csharp[Main](mvc3-release-notes/samples/sample27.cs)]

Esto contrasta con el rol del generador del controlador. Un generador del controlador es una implementación de la interfaz de IControllerFactory, que es responsable para buscar un tipo de controlador y para crear instancias de una instancia de ese tipo de controlador.

Los activadores de controlador están responsables únicamente creando instancias de un tipo de controlador. No tienen que realizar la búsqueda del tipo de controlador. Después de encontrar un tipo de controlador adecuado, generadores de controladores deben delegar a una instancia de IControllerActivator para controlar la creación de instancias real del controlador.

La clase DefaultControllerFactory tiene un nuevo constructor que acepta una instancia de IControllerFactory. Esto le permite aplicar la inserción de dependencias para administrar este aspecto de creación del controlador sin tener que invalidar el comportamiento de búsqueda del tipo de controlador predeterminado.

#### <a name="iservicelocator-interface-replaced-with-idependencyresolver"></a>Interfaz IServiceLocator reemplazado por IDependencyResolver

En función de los comentarios de la Comunidad, la versión Beta de ASP.NET MVC 3 ha reemplazado el uso de la interfaz IServiceLocator con una interfaz de IDependencyResolver simplificada específica para las necesidades de ASP.NET MVC. En el ejemplo siguiente se muestra la nueva interfaz:

[!code-csharp[Main](mvc3-release-notes/samples/sample28.cs)]

Como parte de este cambio, la clase ServiceLocator también se reemplazó por la clase DependencyResolver. Registro de una resolución de dependencia es similar a las versiones anteriores de ASP.NET MVC:

[!code-csharp[Main](mvc3-release-notes/samples/sample29.cs)]

Las implementaciones de esta interfaz simplemente deben delegar en el contenedor de inyección de dependencia subyacente para proporcionar el servicio registrado para el tipo solicitado.

Cuando no hay servicios registrados del tipo solicitado, ASP.NET MVC espera que las implementaciones de esta interfaz para devolver null desde GetService y devolver una colección vacía de GetServices.

La nueva clase DependencyResolver permite registrar las clases que implementan la interfaz de IDependencyResolver nueva o la interfaz de localizador de servicios común (IServiceLocator). Para obtener más información acerca de localizador de servicios común, consulte [CommonServiceLocator en GitHub](https://github.com/unitycontainer/commonservicelocator).

<a id="0.1__Breaking_Changes"></a>

#### <a name="new-iviewactivator-interface-for-fine-grained-view-page-instantiation"></a>Nueva interfaz IViewActivator para la creación de instancias de página de vista específica

La nueva interfaz de IViewPageActivator proporciona un control más minucioso sobre cómo se crean instancias de páginas de vista a través de la inserción de dependencias. Esto se aplica a las instancias de WebFormView y RazorView instancias. En el ejemplo siguiente se muestra la nueva interfaz:

[!code-csharp[Main](mvc3-release-notes/samples/sample30.cs)]

Estas clases ahora aceptan un argumento de constructor IViewPageActivator, que permite usar inserción de dependencias para controlar cómo se crean instancias de los tipos ViewPage, ViewUserControl y WebViewPage.

#### <a name="new-dependency-resolver-support-for-existing-services"></a>Nueva compatibilidad de resolución de dependencia para los servicios existentes

La nueva versión incluye compatibilidad con la resolución de dependencia para los siguientes servicios:

- Proveedores de validación del modelo. Las clases que implementan ModelValidatorProvider se pueden registrar en la resolución de dependencia y el sistema usa para admitir la validación del lado cliente y servidor.
- Proveedor de metadatos del modelo. Una sola clase que implementa ModelMetadataProvider puede estar registrada en la resolución de dependencia y el sistema usará para proporcionar metadatos para los sistemas de validación y definición de plantillas.
- Proveedores de valores. Las clases que implementan ValueProviderFactory se pueden registrar en la resolución de dependencia y el sistema usa para crear proveedores de valores que se usan por el controlador y durante el enlace del modelo.
- Enlazadores de modelos. Las clases que implementan IModelBinderProvider se pueden registrar en la resolución de dependencia y el sistema usa para crear enlazadores de modelos que se usan por el sistema de enlace de modelo.

### <a id="0.1__Toc274034221"></a>Nueva compatibilidad con Ajax de jQuery-Based discreto

ASP.NET MVC incluye métodos de aplicación auxiliar de Ajax como los siguientes:

- Ajax.ActionLink
- Ajax.RouteLink
- Ajax.BeginForm
- Ajax.BeginRouteForm

Estos métodos usan JavaScript para invocar un método de acción en el servidor, en lugar de usar un postback completo. Esta funcionalidad se ha actualizado para aprovechar las ventajas de jQuery de forma discreta. En lugar de emisión de manera intrusiva las secuencias de comandos de cliente, estos métodos auxiliares separan el comportamiento en el marcado mediante la emisión de atributos de HTML5 utilizando la *datos ajax* prefijo. Comportamiento, a continuación, se aplica al código haciendo referencia a los archivos adecuados de JavaScript. Asegúrese de que se hace referencia a los siguientes archivos JavaScript:

- jQuery 1.4.1.js
- jQuery.unobtrusive.AJAX.js

Esta característica está habilitada de forma predeterminada en el archivo Web.config en las plantillas de proyecto nuevo de ASP.NET MVC 3, pero está deshabilitada de forma predeterminada para los proyectos existentes. Para obtener más información, consulte [agrega marcas de toda la aplicación para la validación de cliente y JavaScript discreto](#0.1_AddedApplicationWideFlagsForClientValida) más adelante en este documento.

### <a id="0.1__Toc274034222"></a>Nueva compatibilidad con la validación de jQuery discreta

De forma predeterminada, versión Beta de ASP.NET MVC 3 usa validación de jQuery de forma discreta para realizar la validación del lado cliente. Para habilitar la validación del cliente discreta, realizar una llamada similar al siguiente desde dentro de una vista:

[!code-csharp[Main](mvc3-release-notes/samples/sample31.cs)]

Esto requiere que la propiedad de ViewContext.UnobtrusiveJavaScriptEnabled está establecida en true, lo que puede hacer mediante la realización de la siguiente llamada:

[!code-csharp[Main](mvc3-release-notes/samples/sample32.cs)]

Además, asegúrese de que se hace referencia a los siguientes archivos JavaScript.

- jQuery 1.4.1.js
- jQuery.Validate.js
- jQuery.Validate.unobtrusive.js

Esta característica está habilitada de forma predeterminada en el archivo Web.config en las plantillas de proyecto nuevo de ASP.NET MVC 3, pero está deshabilitada de forma predeterminada para los proyectos existentes. Para obtener más información, consulte [nuevos indicadores de toda la aplicación para la validación de cliente y JavaScript discreto](#0.1_AddedApplicationWideFlagsForClientValida) más adelante en este documento.

<a id="0.1__Toc274034223"></a>

### <a id="0.1_AddedApplicationWideFlagsForClientValida"></a>Nuevos indicadores de toda la aplicación para la validación de cliente y JavaScript discreto

Puede habilitar o deshabilitar la validación del cliente y JavaScript discreto globalmente con miembros estáticos de la clase HtmlHelper, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample33.cs)]

Las plantillas de proyecto predeterminadas habilitan JavaScript discreto de forma predeterminada. También puede habilitar o deshabilitar estas características en el archivo raíz Web.config de su aplicación mediante las siguientes opciones:

[!code-xml[Main](mvc3-release-notes/samples/sample34.xml)]

Dado que puede habilitar estas características de forma predeterminada, se introdujeron nuevas sobrecargas a la clase HtmlHelper que permiten invalidar la configuración predeterminada, tal como se muestra en los ejemplos siguientes:

[!code-csharp[Main](mvc3-release-notes/samples/sample35.cs)]

Por compatibilidad con versiones anteriores, ambas características están deshabilitadas de forma predeterminada.

### <a id="0.1__Toc274034224"></a>Nueva compatibilidad con el código que se ejecuta antes de la ejecución de vistas

Ahora puede colocar un archivo denominado \_viewstart.cshtml (o \_viewstart.vbhtml) en el directorio de vistas y agregue el código que se compartirá entre varias vistas en ese directorio y sus subdirectorios. Por ejemplo, se puede colocar el código siguiente en el \_viewstart.cshtml página en la carpeta ~/Views:

[!code-cshtml[Main](mvc3-release-notes/samples/sample36.cshtml)]

Esto establece la página de diseño para todas las vistas dentro de la carpeta Views y todas su subcarpetas de forma recursiva. Cuando el caso de que se está representando a una vista, el código en el \_viewstart.cshtml se ejecuta antes de que se ejecuta el código de la vista. El \_viewstart.cshtml código se aplica a todas las vistas de esa carpeta.

De forma predeterminada, el código en el \_viewstart.cshtml archivo también se aplica a las vistas en las subcarpetas. Sin embargo, las subcarpetas individuales pueden tener su propia versión de la \_viewstart.cshtml archivo; en que los casos, la versión local tiene prioridad. Por ejemplo, para ejecutar código que es común a todas las vistas para HomeController, coloque un \_viewstart.cshtml archivo en la carpeta ~/Views/Home.

### <a id="0.1__Toc274034225"></a>Nueva compatibilidad con la sintaxis de Razor VBHTML

La versión preliminar de ASP.NET MVC anterior incluye compatibilidad para las vistas con sintaxis Razor basada en C#. Estas vistas utilizan la extensión de archivo .cshtml. Como parte del trabajo en curso para admitir Razor, la versión Beta de ASP.NET MVC 3 incluye compatibilidad con la sintaxis de Razor en Visual Basic, que usa la extensión de archivo .vbhtml.

Para obtener una introducción al uso de sintaxis de Visual Basic en páginas VBHTML, vea el tutorial en la siguiente URL:

[https://www.ASP.NET/WebMatrix/tutorials/ASP-NET-Web-Pages-Visual-Basic](../web-pages/overview/getting-started/introducing-razor-syntax-vb.md)

### <a id="0.1__Toc274034226"></a>Más Control Granular sobre ValidateInputAttribute

ASP.NET MVC siempre se incluye la clase ValidateInputAttribute, que invoca la infraestructura básica de la validación de solicitud ASP.NET para asegurarse de que la solicitud entrante no contiene datos potencialmente dañinos. De forma predeterminada, está habilitada la validación de entrada. Es posible deshabilitar la validación de solicitudes con el atributo ValidateInputAttribute, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample37.cs)]

Sin embargo, muchas aplicaciones web tienen campos de formulario individuales que deben permitir HTML, mientras que los campos restantes no deberían. La clase ValidateInputAttribute ahora le permite especificar una lista de campos que no se debe incluir en la validación de solicitudes.

Por ejemplo, si está desarrollando un motor de blogs, puede permitir marcado en los campos de resumen y cuerpo. Estos campos pueden estar representados por dos elemento input, cada uno con un atributo de nombre correspondiente al nombre de propiedad ("Cuerpo" y "Resumen"). Para deshabilitar la solicitud de validación para estos campos solo, especifique los nombres (separados por comas) en la propiedad de la exclusión de la clase ValidateInput, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample38.cs)]

### <a id="0.1__Toc274034227"></a>Aplicaciones auxiliares de convertir los caracteres de subrayado en guiones para nombres de atributo HTML especificados mediante objetos anónimos

Métodos auxiliares permiten especificar pares de nombre/valor de atributo mediante un objeto anónimo, como en el ejemplo siguiente:

[!code-csharp[Main](mvc3-release-notes/samples/sample39.cs)]

Este enfoque no permite utilizar guiones en el nombre del atributo, porque no se puede usar un guión para un nombre de propiedad en ASP.NET. Sin embargo, son importantes para los atributos personalizados de HTML5; los guiones Por ejemplo, HTML5 utiliza el prefijo "datos-".

Al mismo tiempo, caracteres de subrayado no se puede usar para los nombres de atributo en HTML, pero son válidos en nombres de propiedad. Por lo tanto, si se especifican los atributos con un objeto anónimo, y si los nombres de atributo incluyen un carácter de subrayado, métodos auxiliares convertirá los caracteres de subrayado en guiones. Por ejemplo, la siguiente sintaxis de la aplicación auxiliar usa un carácter de subrayado:

[!code-csharp[Main](mvc3-release-notes/samples/sample40.cs)]

En el ejemplo anterior se representa el marcado siguiente cuando se ejecuta la aplicación auxiliar:

[!code-html[Main](mvc3-release-notes/samples/sample41.html)]

## <a id="0.1__Toc274034228"></a>Correcciones de errores

La plantilla de objeto predeterminado para las aplicaciones auxiliares de plantilla EditorFor y DisplayFor ahora es compatible con el orden especificado en la propiedad DisplayAttribute.Order. (En versiones anteriores, la configuración del orden no se utilizó.)

La validación del cliente ahora admite la validación de propiedades que tienen atributos de validación que se aplican.

JsonValueProviderFactory ya está registrado de forma predeterminada.

## <a id="0.1__Toc274034229"></a>Cambios importantes

Ha cambiado el orden de ejecución para los filtros de excepción para los filtros de excepciones que tienen el mismo valor de orden. En ASP.NET MVC 2 y versiones anteriores, los filtros de excepciones en el controlador con el mismo orden a medida que los de un método de acción se ejecutan antes que los filtros de excepción en el método de acción. Normalmente sería el caso cuando se aplicaron los filtros de excepción sin especificar un valor de orden. En ASP.NET MVC 3, este pedido se ha invertido para que el controlador de excepciones más específico se ejecuta primero. Al igual que en versiones anteriores, si la propiedad orden se especifica explícitamente, los filtros se ejecutan en el orden especificado.

## <a id="0.1__Toc274034230"></a>Problemas conocidos

Durante la instalación, el cuadro de diálogo de aceptación de términos de licencia muestra los términos de licencia en una ventana que es inferior a lo esperado.

Vistas de Razor no tiene compatibilidad con IntelliSense ni el resaltado de sintaxis. Se prevé que compatibilidad con la sintaxis de Razor en Visual Studio se incluirá como parte de una versión posterior.

Al editar una vista de Razor (CSHTML archivo), el <a id="0.1__Toc224729061"> </a> <a id="0.1__Toc238051347"> </a> elemento de menú de ir al controlador en Visual Studio no estará disponible y no hay ningún fragmentos de código.

Cuando se usa el @model ver la sintaxis para especificar un CSHTML fuertemente tipado, no se reconocen los métodos abreviados específicos del idioma para los tipos. Por ejemplo, @model int no funcionará, pero @model funcionará Int32. La solución correspondiente a este error consiste en utilizar el nombre de tipo real cuando se especifica el tipo de modelo.

Cuando se usa el @model sintaxis para especificar una vista fuertemente tipada de CSHTML (o @ModelType para especificar una vista fuertemente tipada de VBHTML), no se admiten los tipos que aceptan valores NULL y las declaraciones de matriz. Por ejemplo, @model int? no se admite. En su lugar, use @model Nullable&lt;Int32&gt;. La sintaxis @model string [] tampoco se admite; en su lugar, use @model IList&lt;cadena&gt;.

Cuando actualiza un proyecto de ASP.NET MVC 2 a ASP.NET MVC 3, asegúrese de agregar lo siguiente a la sección appSettings del archivo Web.config:

[!code-xml[Main](mvc3-release-notes/samples/sample42.xml)]

Hay un problema conocido que hace que la autenticación de formularios redirigir los usuarios no autenticados a ~/Account/inicio de sesión, se omitirá la configuración de autenticación de formularios usada en el archivo Web.config. La solución es agregar la siguiente configuración de aplicación.

[!code-xml[Main](mvc3-release-notes/samples/sample43.xml)]

## <a id="0.1__Toc274034231"></a>Declinación de responsabilidades

© 2011 Microsoft Corporation. Todos los derechos reservados. Este documento se proporciona "como-es." Información y opiniones expresadas en este documento, incluidas direcciones URL y otras referencias a sitios Web de Internet, pueden cambiar sin previo aviso. Usted asume el riesgo de utilizarla.

Este documento no le proporciona ningún derecho legal sobre ninguna propiedad intelectual de ningún producto de Microsoft. Puede copiar y usar este documento para su uso interno de referencia.
