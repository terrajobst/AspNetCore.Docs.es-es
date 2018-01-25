---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
title: "Determinar qué archivos deben tener implementado (C#) | Documentos de Microsoft"
author: rick-anderson
description: "¿Qué archivos deben implementarse desde el entorno de desarrollo al entorno de producción depende en parte de si la aplicación ASP.NET se ha generado us..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: f8d78a88-cc91-40d8-bce3-3d7954f6033b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-cs
msc.type: authoredcontent
ms.openlocfilehash: d58956323275a46b44b36d4f19db4d2f607e3916
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="determining-what-files-need-to-be-deployed-c"></a>Determinar qué archivos deben tener implementado (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_CS.zip) o [descarga de PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_cs.pdf)

> ¿Qué archivos deben implementarse desde el entorno de desarrollo al entorno de producción depende en parte de si la aplicación ASP.NET se ha generado utilizando el modelo de sitio Web o aplicación Web. Más información acerca de estos dos modelos de proyecto y cómo afecta el modelo de proyecto a la implementación.


## <a name="introduction"></a>Introducción

Implementar una aplicación web ASP.NET implica copiar los archivos relacionados con ASP.NET desde el entorno de desarrollo al entorno de producción. Los archivos relacionados con ASP.NET incluyen marcado de la página web ASP.NET y archivos de compatibilidad de código y el cliente y servidor. Archivos de compatibilidad del cliente son aquellos archivos hace referencia a las páginas web y envía directamente al explorador - imágenes, archivos CSS y JavaScript archivos, por ejemplo. Archivos de compatibilidad de servidor incluyen las que se utilizan para procesar una solicitud en el servidor. Esto incluye archivos de configuración, los servicios web, archivos de clase, los conjuntos de datos con tipo y LINQ a los archivos SQL, entre otros.

En general, todos los archivos de compatibilidad del cliente se deben copiar desde el entorno de desarrollo al entorno de producción, pero se copien los archivos de compatibilidad de servidor depende de si está compilando explícitamente el código del lado servidor en un ensamblado (un `.dll` archivo) o si tiene estos ensamblados generados automáticamente. Este tutorial resalta qué archivos se deben implementarse al compilar explícitamente el código en un ensamblado en lugar de tener que este paso de compilación se producen automáticamente.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Compilación explícita frente a una compilación automática

Las páginas web ASP.NET se dividen en declarativo marcado y código fuente. La parte del marcado declarativo incluye código HTML, Web controles y la sintaxis de enlace de datos; la parte de código contiene los controladores de eventos escritos en código de Visual Basic o C#. Normalmente se separan las partes del código y marcado en archivos diferentes: `WebPage.aspx` contiene el marcado declarativo mientras `WebPage.aspx.cs` aloja el código.

Considere la posibilidad de una página ASP.NET denominada Clock.aspx que contiene un control de etiqueta cuya propiedad de texto se establece en la fecha y hora actual cuando se carga la página. La parte del marcado declarativo (en `Clock.aspx`) contendría el marcado para un control Web Label -`<asp:Label runat="server" id="TimeLabel" />` : durante la parte de código (en `Clock.aspx.cs`) tendría un `Page_Load` controlador de eventos con el código siguiente:

[!code-csharp[Main](determining-what-files-need-to-be-deployed-cs/samples/sample1.cs)]

En orden para el motor ASP.NET para atender una solicitud de esta página, la parte correspondiente al código de la página (el `WebPage.aspx.cs` archivo) en primer lugar debe compilarse. Esta compilación puede suceder de forma explícita o automáticamente.

Si la compilación se produce de forma explícita, a continuación, se compila código fuente de la aplicación todo en uno o más ensamblados (`.dll` archivos) ubicado en la aplicación `Bin` directory. Si la compilación se realiza automáticamente, a continuación, resultante generada automáticamente es el ensamblado, de forma predeterminada, se coloca en el `Temporary ASP.NET` carpeta, que se encuentra en `%WINDOWS%\Microsoft.NET\Framework\`  *&lt;versión&gt;*, Aunque esta ubicación es configurable a través de la [ `<compilation>` elemento](https://msdn.microsoft.com/library/s10awwz0.aspx) en `Web.config`. Con la compilación explícita debe realizar alguna acción para compilar el código de la aplicación ASP.NET en un ensamblado, y este paso se realiza antes de la implementación. Compilación automática con el proceso de compilación se produce en el servidor web cuando se acceda al recurso por primera vez.

Independientemente de qué modelo de compilación que utilice, la parte de marcado de todas las páginas ASP.NET (la `WebPage.aspx` archivos) deben copiarse en el entorno de producción. Con la compilación explícita debe copiar los ensamblados en el `Bin` carpeta, pero no es necesario copiar una de las partes del código de las páginas ASP.NET (la `WebPage.aspx.cs` archivos). Con la compilación automática debe copiar los archivos de la parte de código para que el código está presente y se puede compilar automáticamente cuando se visita la página. La parte de marcado de cada página web ASP.NET incluye una `@Page` directiva con los atributos que indican si el código asociado de la página haya compilado ya explícitamente o si es necesario que se compila de forma automática. Como resultado, el entorno de producción puede funcionar perfectamente con cualquier modelo de compilación y no es necesario aplicar ninguna configuración especial para indicar que se utilizó la compilación explícita o automática.

Tabla 1 se resumen los diferentes archivos para implementar cuando se usa la compilación explícita frente a una compilación automática. Tenga en cuenta que independientemente de la compilación de modelo de usa siempre debe implementar los ensamblados en el `Bin` carpeta, si dicha carpeta existe. El `Bin` carpeta contiene los ensamblados específicos de la aplicación web, que incluye el código fuente compilado cuando se usa el modelo de compilación explícita. El `Bin` directorio también contiene ensamblados de otros proyectos de código abierto o de otros fabricantes y los ensamblados y puede que esté usando, y estos deben estar en el servidor de producción. Por lo tanto, como regla general, copie el `Bin` carpeta hasta la producción cuando se implementa. (Si está utilizando el modelo de compilación automática y no se está usando los ensamblados externos, no tendrá un `Bin` directorio - que es correcto!)

| **Modelo de compilación** | **¿Implementar el archivo de marcado de parte?** | **¿Implementar el archivo de código fuente?** | **¿Implementar ensamblados en `Bin` directorio?** |
| --- | --- | --- | --- |
| Compilación explícita | Sí | No | Sí |
| Compilación automática | Sí | Sí | Sí (si lo hay). |

**Tabla 1:** depende de qué archivos se implementa en el modelo de compilación utilizado.

## <a name="taking-a-trip-down-memory-lane"></a>Realizar un recorrido hacia abajo de la calle de memoria

Se utiliza el enfoque de compilación depende, en parte, cómo se administra la aplicación ASP.NET en Visual Studio. Desde. Principio del NET en el año 2000 ha habido cuatro versiones diferentes de Visual Studio - Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 y Visual Studio 2008. Visual Studio .NET 2002 y 2003 administrar aplicaciones de ASP.NET mediante el *modelo de proyecto de aplicación Web*. Las características clave del modelo de proyecto de aplicación Web son:

- Los archivos que la composición del proyecto se definen en un único archivo de proyecto. Los archivos no definidos en el archivo de proyecto no se consideran parte de la aplicación web de Visual Studio.
- Se utiliza la compilación explícita. Compilar el proyecto compila los archivos de código dentro del proyecto en un único ensamblado que se coloca en el `Bin` carpeta.

Cuando Microsoft publicó Visual Studio 2005 se quita el soporte para el modelo de proyecto de aplicación Web y sustituirla por otra con el modelo de proyecto de sitio Web. El modelo de proyecto de sitio Web se diferencia del modelo de proyecto de aplicación Web de las maneras siguientes:

- En lugar de tener un único archivo de proyecto que pone de relieve los archivos del proyecto, el sistema de archivos se usa en su lugar. En resumen, los archivos dentro de la carpeta de aplicación web (o sus subcarpetas) se consideran parte del proyecto.
- Generar un proyecto en Visual Studio no crea un ensamblado en el `Bin` directory. En su lugar, al compilar un proyecto de sitio Web notifica los errores de tiempo de compilación.
- Compatibilidad con la compilación automática. Proyectos de sitios Web se implementan normalmente copiando el marcado y código fuente en el entorno de producción, aunque el código puede ser precompilada (compilación explícito).

Microsoft a revivir el modelo de proyecto de aplicación Web cuando lance Visual Studio 2005 Service Pack 1. Sin embargo, Visual Web Developer continuado solo admite el modelo de proyecto de sitio Web. Las buenas noticias son que esta limitación se quitó con Visual Web Developer 2008 Service Pack 1. Hoy en día puede crear aplicaciones de ASP.NET en Visual Studio (y Visual Web Developer) mediante el modelo de proyecto de aplicación Web o el modelo de proyecto de sitio Web. Ambos modelos tienen sus ventajas e inconvenientes. Hacer referencia a [Introducción a los proyectos de aplicación Web: comparación de proyectos de sitios Web y proyectos de aplicación Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) para ver una comparación de los dos modelos y para ayudarle a decidir qué modelo de proyecto adapte mejor a su situación.

## <a name="exploring-the-sample-web-application"></a>Explorar la aplicación Web

La descarga de este tutorial incluye una aplicación de ASP.NET denominada reseñas de libros. El sitio Web imita un sitio Web de pasatiempo alguien podría crear compartir su libro revisa con la Comunidad en línea. Esta aplicación web ASP.NET es muy sencilla y consta de los siguientes recursos:

- `Web.config`, archivo de configuración de la aplicación.
- Una página maestra (`Site.master`).
- Siete páginas ASP.NET diferentes: 

    - ~`/Default.aspx`-página principal del sitio.
    - ~`/About.aspx`-una página "Sobre el sitio".
    - ~`/Fiction/Default.aspx`-una página con una lista de los libros de ficción revisados. 

        - ~`/Fiction/Blaze.aspx`-una revisión de la novel Richard Bachman *Blaze*.
    - ~/`Tech/Default.aspx`-una página con una lista de los libros de tecnología que se han revisado. 

        - ~/`Tech/CYOW.aspx`-una revisión de *crear su propio sitio Web de*.
        - ~/`Tech/TYASP35.aspx`-una revisión de *enseñar a usted mismo ASP.NET 3.5 en 24 horas*.
- Tres archivos CSS diferentes en la carpeta Styles.
- Cuatro archivos de imagen: una con tecnología de logotipo ASP.NET y las imágenes de los bastidores de los tres libros revisados - todos los localizados en el `Images` carpeta.
- A `Web.sitemap` archivo, que define el mapa del sitio y se usa para mostrar los menús en la `Default.aspx` páginas en el directorio raíz y `Fiction` y `Tech` carpetas.
- Un archivo de clase denominado `BasePage.cs` que define una base de `Page` clase. Esta clase extiende la funcionalidad de la `Page` clase mediante el establecimiento automático del `Title` propiedad según la posición de la página del mapa del sitio. En pocas palabras, cualquier clase de código subyacente ASP.NET que abarca `BasePage` (en lugar de `System.Web.UI.Page`) tendrá su título establecido en un valor según su posición en el mapa del sitio. Por ejemplo, cuando se visualizan los ~ /`Tech/CYOW.aspx` el título de página, se establece en "Home: tecnología: crear su propio sitio Web".

La figura 1 muestra una captura de pantalla del sitio Web de reseñas de libros cuando se ven a través de un explorador. Aquí verá la página ~ /`Tech/TYASP35.aspx`, que revisa la libreta de *enseñar a usted mismo ASP.NET 3.5 en 24 horas*. La ruta de navegación que abarca la parte superior de la página y el menú de la columna izquierda se basan en la estructura de mapa del sitio definida en `Web.sitemap`. La imagen en la esquina superior derecha es uno de cubierta de libro en las imágenes que se encuentren en el `Images` carpeta. Apariencia del sitio Web se definen a través de en cascada reglas de hojas de estilos deletreadas por los archivos CSS en la carpeta Styles, mientras que el diseño de página determinantes se define en la página maestra, `Site.master`.


[![El sitio Web del libro se revisan ofrece revisiones en una gran variedad de títulos](determining-what-files-need-to-be-deployed-cs/_static/image2.png)](determining-what-files-need-to-be-deployed-cs/_static/image1.png)

**Figura 1:** el sitio Web del libro se revisan ofrece revisiones en una gran variedad de títulos ([haga clic aquí para ver la imagen a tamaño completo](determining-what-files-need-to-be-deployed-cs/_static/image3.png))


Esta aplicación no usa una base de datos; cada revisión se implementa como una página web independiente en la aplicación. Este tutorial (y la siguientes varios tutoriales) guían por la implementación de una aplicación web que no tiene una base de datos. Sin embargo, en un tutorial posterior se mejorará esta aplicación para almacenar las revisiones, comentarios de un lector y otra información en una base de datos y analizará qué pasos deben realizarse para implementar correctamente una aplicación web controlada por datos.

> [!NOTE]
> Estos tutoriales se centran en las aplicaciones de ASP.NET con un proveedor de hospedaje web que hospeda y no explorar temas auxiliares como ASP. Sistema de asignación de sitio de red o con una base de `Page` clase. Para obtener más información sobre estas tecnologías y para obtener más información sobre otros temas que tratan a lo largo del tutorial, consulte la sección más información al final de cada tutorial.


Descarga de este tutorial tiene dos copias de la aplicación web, cada una se implementa como un tipo de proyecto de Visual Studio diferente: BookReviewsWAP, un proyecto de aplicación Web y BookReviewsWSP, un proyecto de sitio Web. Ambos proyectos creados con Visual Web Developer 2008 SP1 y usar ASP.NET 3.5 SP1. Para trabajar con estos proyectos iniciar descomprimir el contenido en el escritorio. Para abrir el proyecto de aplicación Web (BookReviewsWAP), navegue hasta la carpeta BookReviewsWAP y haga doble clic en el archivo de solución `BookReviewsWAP.sln`. Para abrir el proyecto de sitio Web (BookReviewsWSP), inicie Visual Studio y, a continuación, en el menú archivo, elija la opción de abrir sitio Web, vaya a la `BookReviewsWSP` carpeta en el escritorio y haga clic en Aceptar.


Las dos secciones restantes de este tutorial vistazo qué archivos debe copiar en el entorno de producción al implementar la aplicación. Los siguientes dos tutoriales -  *[implementar su sitio mediante FTP](deploying-your-site-using-an-ftp-client-cs.md)*  y  *[implementar su sitio con Visual Studio](deploying-your-site-using-visual-studio-cs.md)*  -muestran distintas formas Copie estos archivos en un proveedor de hospedaje web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Determinar los archivos de implementación para el proyecto de aplicación Web

El modelo de proyecto de aplicación Web usa la compilación explícita: código fuente del proyecto se compila en un único ensamblado cada vez que compile la aplicación. Esta compilación incluye archivos de código subyacente de las páginas ASP.NET (~ /`Default.aspx.cs`, ~ /`About.aspx.cs`, etc.), así como el `BasePage.cs` clase. El ensamblado resultante se denomina BookReviewsWAP.dll y se encuentra en la aplicación `Bin` directory.

La figura 2 muestra los archivos que componen el proyecto de aplicación Web de libreta de revisiones.


[![El Explorador de soluciones se incluyen los archivos que componen el proyecto de aplicación Web](determining-what-files-need-to-be-deployed-cs/_static/image5.png)](determining-what-files-need-to-be-deployed-cs/_static/image4.png)

**Figura 2**: el Explorador de soluciones se incluyen los archivos que componen el proyecto de aplicación Web


Para implementar una aplicación de ASP.NET desarrollada con el inicio del modelo de proyecto de aplicación Web mediante la creación de la aplicación con el fin de compilar el código fuente más reciente explícitamente en un ensamblado. A continuación, copie los siguientes archivos en el entorno de producción:

- Página de los archivos que contienen el marcado declarativo para cada ASP.NET, tal como ~ /`Default.aspx`, ~ /`About.aspx`, y así sucesivamente. Además, copia de seguridad el marcado declarativo para las páginas maestras y controles de usuario.
- Los ensamblados (`.dll` archivos) en el `Bin` carpeta. No es necesario copiar los archivos de base de datos de programa (`.pdb`) o los archivos XML, puede encontrar en el `Bin` directory.

No es necesario copiar los archivos de código fuente de las páginas ASP.NET en el entorno de producción ni es necesario copiar la `BasePage.cs` archivo de clase.

> [!NOTE]
> Como se muestra en la figura 2, el `BasePage` clase se implementa como un archivo de clase en el proyecto, que se coloca en la carpeta denominada `HelperClasses`. Cuando se compila el proyecto el código en el `BasePage.cs` archivo se compila junto con las clases de código subyacente de las páginas ASP.NET en el ensamblado único, `BookReviewsWAP.dll.` ASP.NET dispone de una carpeta especial denominada `App_Code` que está diseñado para almacenar archivos de clase de Web Proyectos de sitios. El código en el `App_Code` carpeta automáticamente se compila y, por tanto, no debe usarse con proyectos de aplicación Web. En su lugar, debe colocar los archivos de clase de la aplicación en una carpeta normal denominada `HelperClasses`, o `Classes`, o algo similar. Como alternativa, puede colocar archivos de clase en un proyecto de biblioteca de clases independiente.


Además de copiar los archivos de marcado relacionado con ASP.NET y el ensamblado en el `Bin` carpeta, también debe copiar los archivos de compatibilidad del cliente: las imágenes y archivos CSS -, así como los demás archivos de compatibilidad de servidor, `Web.config` y `Web.sitemap`. Estos lado cliente y servidor de archivos de compatibilidad que se copiará en el entorno de producción independientemente de si utiliza la compilación explícita o automática.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Determinación de los archivos para implementar para los archivos de proyecto de sitio Web

El modelo de proyecto de sitio Web admite la compilación automática, una característica no está disponible cuando se usa el modelo de proyecto de aplicación Web. Con la compilación explícita debe compilar el código de origen del proyecto en un ensamblado y copiar ese ensamblado en el entorno de producción. Por otro lado, con la compilación automática simplemente copie el código fuente en el entorno de producción y se compila en tiempo de ejecución a petición, según sea necesario.

La opción de menú de compilación en Visual Studio está presente en los proyectos de aplicación Web y proyectos de sitios Web. Crear un proyecto de aplicación Web compila código fuente del proyecto en un único ensamblado que se encuentra en la `Bin` directorio; crear un proyecto de sitio Web comprueba los errores de tiempo de compilación, pero no crea los ensamblados. Para implementar una aplicación de ASP.NET desarrollada con el modelo de proyecto de sitio Web todos los que debe hacer es copiar los archivos adecuados en el entorno de producción, pero le recomiendo que debe generar primero el proyecto para asegurarse de que no hay ningún error de tiempo de compilación.

La figura 3 muestra los archivos que componen el proyecto de sitio Web de libreta de revisiones.


 [![El Explorador de soluciones se incluyen los archivos que componen el proyecto de sitio Web](determining-what-files-need-to-be-deployed-cs/_static/image7.png)](determining-what-files-need-to-be-deployed-cs/_static/image6.png) 

**Figura 3**: el Explorador de soluciones se incluyen los archivos que componen el proyecto de sitio Web


Implementación de un proyecto de sitio Web implica copiar todos los archivos relacionados con ASP.NET en el entorno de producción - que incluye las páginas de marcado para páginas ASP.NET, páginas maestras y controles de usuario, junto con sus archivos de código. También debe copiar la seguridad de los archivos de clase, como BasePage.cs. Tenga en cuenta que la `BasePage.cs` archivo se encuentra en la `App_Code` carpeta, que es una carpeta especial del ASP.NET usa en proyectos de sitios Web para los archivos de clase. Debe crearse en producción, también, como los archivos de clase en la carpeta especial del `App_Code` carpeta en el entorno de desarrollo debe copiarse en el `App_Code` carpeta en producción.

Además de copiar los archivos de código de marcado y código fuente ASP.NET, también debe copiar los archivos de compatibilidad del cliente: las imágenes y archivos CSS -, así como los demás archivos de compatibilidad de servidor, `Web.config` y `Web.sitemap`.

> [!NOTE]
> Proyectos de sitios Web también puede utilizar la compilación explícita. Un tutorial futuras examinará cómo explícitamente compilar un proyecto de sitio Web.


## <a name="summary"></a>Resumen

Implementar una aplicación de ASP.NET implica copiar los archivos necesarios del entorno de desarrollo al entorno de producción. El conjunto de archivos que deben sincronizarse depende de si la aplicación ASP.NET se forma explícita o automáticamente compila el código. La estrategia de compilación empleada se ve influida por si Visual Studio está configurado para administrar la aplicación de ASP.NET mediante el modelo de proyecto de aplicación Web o el modelo de proyecto de sitio Web.

El modelo de proyecto de aplicación Web se utiliza la compilación explícita y compila el código del proyecto en un único ensamblado en el `Bin` carpeta. Al implementar la aplicación, la parte de marcado de las páginas ASP.NET y el contenido de la `Bin` carpeta se debe insertar en el entorno de producción; no es necesario el código fuente en la aplicación - los archivos de código y las clases de código subyacente, por ejemplo: que se copiará en el entorno de producción.

El modelo de proyecto de sitio Web usa una compilación automática de forma predeterminada, aunque es posible compilar explícitamente un proyecto de sitio Web, tal y como se verá en el futuro tutoriales. Implementar una aplicación de ASP.NET que usa una compilación automática requiere que la parte de marcado *y* código fuente debe copiarse en el entorno de producción. El código se compila automáticamente en el entorno de producción cuando se solicita por primera vez.

Ahora que hemos examinado los archivos que se sincronizan entre los entornos de desarrollo y producción se está listos para implementar la aplicación de reseñas de libros en un proveedor de hospedaje web.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Información general sobre la compilación de ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Controles de usuario ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Examen de ASP. Navegación por el sitio de red](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Introducción a los proyectos de aplicación Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Tutoriales de página maestra](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Compartir código entre páginas](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Usar una clase Base personalizada para las clases de código subyacente de las páginas ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Sistema de proyectos del Visual Studio 2005 sitio Web: ¿qué son y por qué hacemos?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Tutorial: Convertir un proyecto de sitio Web a un proyecto de aplicación Web en Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

>[!div class="step-by-step"]
[Anterior](asp-net-hosting-options-cs.md)
[Siguiente](deploying-your-site-using-an-ftp-client-cs.md)
