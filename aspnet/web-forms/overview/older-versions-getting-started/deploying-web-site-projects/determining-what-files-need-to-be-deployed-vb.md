---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
title: Determinar qué archivos se deben implementar (VB) | Microsoft Docs
author: rick-anderson
description: Qué archivos se deben implementarse desde el entorno de desarrollo al entorno de producción depende en parte de si se ha generado la aplicación ASP.NET nosotros...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: ea918f62-c9d6-4a7f-9bc6-e054d3764b2c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/determining-what-files-need-to-be-deployed-vb
msc.type: authoredcontent
ms.openlocfilehash: 52ba518ec1c911b2eb659c0b36a2a718fbdd0c36
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825707"
---
<a name="determining-what-files-need-to-be-deployed-vb"></a>Determinar qué archivos se deben implementar (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_02_VB.zip) o [descargar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial02_FilesToDeploy_vb.pdf)

> Qué archivos se deben implementarse desde el entorno de desarrollo al entorno de producción depende en parte de si la aplicación ASP.NET se ha generado mediante el modelo de sitio Web o aplicación Web. Más información sobre estos dos modelos de proyecto y cómo afecta el modelo de proyecto a la implementación.


## <a name="introduction"></a>Introducción

Implementar una aplicación web ASP.NET implica copiar los archivos de ASP.NET desde el entorno de desarrollo al entorno de producción. Los archivos relacionados con ASP.NET incluyen marcado de página web ASP.NET y código y del lado cliente y servidor admiten archivos. Archivos auxiliares del lado cliente son aquellos archivos que se hace referencia a las páginas web y los envía directamente al explorador - imágenes, archivos CSS y archivos de JavaScript, por ejemplo. Los archivos de compatibilidad de servidor incluyen los que se usan para procesar una solicitud en el servidor. Esto incluye los archivos de configuración, servicios web, archivos de clase, los conjuntos de datos con tipo y LINQ para archivos SQL, entre otros.

En general, todos los archivos de compatibilidad del cliente se deben copiar desde el entorno de desarrollo al entorno de producción, pero se copian los archivos de soporte técnico del lado servidor depende de si está compilando explícitamente el código del lado servidor en un ensamblado (un `.dll` archivo) o si tiene estos ensamblados generados automáticamente. En este tutorial se resalta qué archivos se deben implementarse al compilar explícitamente el código en un ensamblado en lugar de que este paso de compilación se producen automáticamente.

## <a name="explicit-compilation-versus-automatic-compilation"></a>Compilación explícita frente a la compilación automática

Páginas web de ASP.NET se dividen en código declarativo de marcado y código fuente. La sección de marcado declarativo incluye el código HTML, Web, controles y la sintaxis de enlace de datos; la parte de código contiene controladores de eventos escritos en código de Visual Basic o C#. Las partes del marcado y código normalmente se dividen en archivos diferentes: `WebPage.aspx` contiene el marcado declarativo mientras `WebPage.aspx.vb` aloja el código.

Considere la posibilidad de una página ASP.NET denominada `Clock.aspx` que contiene un control de etiqueta cuya propiedad de texto se establece en la fecha y hora actuales cuando se cargue la página. La parte del marcado declarativo (en `Clock.aspx`) contendría el marcado para un control Web Label: `<asp:Label runat="server" id="TimeLabel" />` , la parte del código (en `Clock.aspx.vb`) tendría un `Page_Load` controlador de eventos con el código siguiente:

[!code-vb[Main](determining-what-files-need-to-be-deployed-vb/samples/sample1.vb)]

En orden para el motor de ASP.NET atender una solicitud para esta página, parte del código de la página (la *`WebPage`* `.aspx.vb` archivo) debe compilarse primero. Esta compilación puede pasar explícitamente o automáticamente.

Si la compilación sucede explícitamente, a continuación, se compila código fuente de la aplicación todo en uno o varios ensamblados (`.dll` archivos) ubicado en la aplicación `Bin` directory. Si la compilación se realiza automáticamente, a continuación, el resultado generado automáticamente es el ensamblado, de forma predeterminada, se coloca en el `Temporary ASP.NET Files` carpeta, que puede encontrarse en `%WINDOWS%\Microsoft.NET\Framework\<version>`, aunque esta ubicación se puede configurar a través de la [ &lt; compilación&gt; elemento](https://msdn.microsoft.com/library/s10awwz0.aspx) en `Web.config`. Con la compilación explícita debe realizar alguna acción para compilar el código de la aplicación ASP.NET en un ensamblado, y este paso se produce antes de la implementación. Compilación automática con el proceso de compilación se produce en el servidor web cuando primero se tiene acceso al recurso.

Independientemente de qué modelo de compilación que use, la parte del marcado de todas las páginas ASP.NET (el `WebPage.aspx` archivos) deben copiarse en el entorno de producción. Con la compilación explícita deberá copiar los ensamblados en el `Bin` carpeta, pero no es necesario copiar secciones de código de las páginas de ASP.NET (el `WebPage.aspx.vb` archivos). Con la compilación automática, deberá copiar los archivos de la parte de código para que el código está presente y se puede compilar automáticamente cuando se visita la página. La parte del marcado de cada página web ASP.NET incluye un `@Page` la directiva con los atributos que indican si el código asociado de la página haya compilado ya explícitamente o si es necesario que se compila automáticamente. Como resultado, el entorno de producción puede funcionar perfectamente con cualquier modelo de compilación y no tiene que aplicar ninguna configuración especial para indicar que se usa la compilación explícita o automática.

Tabla 1 resume los diferentes archivos para implementar cuando se usa una compilación explícita frente a la compilación automática. Tenga en cuenta que, independientemente de la compilación de modelo usado es siempre debe implementar los ensamblados en el `Bin` carpeta, si existe esa carpeta. El `Bin` carpeta contiene los ensamblados específicos de la aplicación web, que incluyen el código fuente compilado cuando se usa el modelo de compilación explícita. El `Bin` directorio también contiene los ensamblados de otros proyectos y los ensamblados de código abierto o de terceros puede que esté usando, y estos deben estar en el servidor de producción. Por lo tanto, como regla general, copie el `Bin` carpeta hasta la producción cuando se implementa. (Si está utilizando el modelo de compilación automática y no utilizan los ensamblados externos, no tendrá un `Bin` directorio - que es correcto!)

| **Modelo de compilación** | **¿Implementar el archivo de marcado de parte?** | **¿Implementar el archivo de código fuente?** | **¿Implementar ensamblados en `Bin` directorio?** |
| --- | --- | --- | --- |
| Compilación explícita | Sí | No | Sí |
| Compilación automática | Sí | Sí | Sí (si existe) |

**Tabla 1: Qué archivos implementar depende de usa el modelo de compilación.**

## <a name="taking-a-trip-down-memory-lane"></a>Realizar un viaje por el pasaje de memoria

Se usa el enfoque de compilación depende, en parte, cómo se administra la aplicación ASP.NET en Visual Studio. Desde. Comienzo de la red en el año 2000 han sido cuatro versiones diferentes de Visual Studio, Visual Studio .NET 2002, Visual Studio .NET 2003, Visual Studio 2005 y Visual Studio 2008. Visual Studio .NET 2002 y 2003 administra aplicaciones de ASP.NET mediante el *proyecto de aplicación Web* modelo. Las características clave del modelo de proyecto de aplicación Web son:

- Los archivos que la composición del proyecto se definen en un único archivo de proyecto. Los archivos no definidos en el archivo de proyecto no se consideran parte de la aplicación web de Visual Studio.
- Se utiliza la compilación explícita. Compilar el proyecto compila los archivos de código dentro del proyecto en un único ensamblado que se coloca en el `Bin` carpeta.

Cuando Microsoft publicó Visual Studio 2005 se quitó la compatibilidad con el modelo de proyecto de aplicación Web y lo reemplacé con el modelo de proyecto de sitio Web. El modelo de proyecto de sitio Web propio de diferencia la *proyecto de aplicación Web* modelos de las maneras siguientes:

- En lugar de tener un único archivo de proyecto que se detalla los archivos del proyecto, se usa el sistema de archivos. En resumen, los archivos dentro de la carpeta de aplicación web (o subcarpetas) se consideran parte del proyecto.
- Crear un proyecto en Visual Studio no crea un ensamblado en el `Bin` directory. En su lugar, compilar un proyecto de sitio Web informa de los errores de tiempo de compilación.
- Compatibilidad con la compilación automática. Proyectos de sitios Web se implementan normalmente copiando el marcado y código fuente en el entorno de producción, aunque el código puede ser precompilado (compilación explícita).

Microsoft reactiva el modelo de proyecto de aplicación Web cuando lance Visual Studio 2005 Service Pack 1. Sin embargo, Visual Web Developer continuado solo admiten el modelo de proyecto de sitio Web. La buena noticia es que esta limitación se quitó con Visual Web Developer 2008 Service Pack 1. Hoy en día pueden crear aplicaciones ASP.NET en Visual Studio (y Visual Web Developer) mediante el modelo de proyecto de aplicación Web o el modelo de proyecto de sitio Web. Ambos modelos tienen sus ventajas y desventajas. Consulte [Introducción a los proyectos de aplicación Web: comparación de proyectos de sitios Web y proyectos de aplicación Web](https://msdn.microsoft.com/library/aa730880.aspx#wapp_topic5) para ver una comparación de los dos modelos y para ayudarle a decidir qué modelo de proyecto funciona mejor para su situación.

## <a name="exploring-the-sample-web-application"></a>Exploración de la aplicación Web

La descarga de este tutorial incluye una aplicación de ASP.NET denominada reseñas de libros. Imita a un sitio Web de afición alguien puede crear el sitio Web compartir su libro revisiones con la Comunidad en línea. Esta aplicación web ASP.NET es muy sencilla y consta de los siguientes recursos:

- `Web.config`, el archivo de configuración de la aplicación.
- Una página maestra (`Site.master`).
- Siete diferentes páginas ASP.NET:

    - ~/`Default.aspx` -página principal de la carpeta del sitio.
    - ~/`About.aspx` -una página "Sobre el sitio".
    - ~/`Fiction/Default.aspx` -una página de listado de los libros en pantalla de ficción que se han revisado.

        - ~/`Fiction/Blaze.aspx` -una revisión de la novela Richard Bachman *Blaze*.
    - ~/`Tech/Default.aspx` -una página de listado de los libros de tecnología que se han revisado.

        - ~/`Tech/CYOW.aspx` -una revisión de *crear su propio sitio Web de*.
        - ~/`Tech/TYASP35.aspx` -una revisión de *enseñar a usted mismo ASP.NET 3.5 en 24 horas*.
- Tres archivos CSS en el `Styles` carpeta.
- Cuatro archivos de imagen: una con tecnología de logotipo ASP.NET e imágenes de segundo plano de los tres libros revisados - todos los localizados en el `Images` carpeta.
- Un `Web.sitemap` archivo, que define el mapa del sitio y se usa para mostrar los menús en la `Default.aspx` páginas en el directorio raíz y `Fiction` y `Tech` carpetas.
- Un archivo de clase denominado `BasePage.vb` que define una base de `Page` clase. Esta clase extiende la funcionalidad de la `Page` clase mediante el establecimiento automático del `Title` propiedad basándose en la posición de la página del mapa del sitio. En pocas palabras, cualquier clase de código subyacente ASP.NET que se extiende `BasePage` (en lugar de `System.Web.UI.Page`) tendrán su título establecida en un valor según su posición en el mapa del sitio. Por ejemplo, cuando se visualizan los ~ /`Tech/CYOW.aspx` página, el título se establece en "Home: tecnología: crear su propio sitio Web".

Figura 1 muestra una captura de pantalla de la reseñas de libros del sitio Web cuando se ve mediante un explorador. Aquí verá la página ~ / Tech/TYASP35.aspx, que revisa el libro *enseñar a usted mismo ASP.NET 3.5 en 24 horas*. La ruta de navegación que abarca la parte superior de la página y el menú de la columna izquierda se basan en la estructura de mapa del sitio definida en `Web.sitemap`. La imagen en la esquina superior derecha es una de las imágenes ubicadas en la cubierta del libro la `Images` carpeta. Apariencia del sitio Web se definen mediante reglas de hojas de estilos en cascada deletreadas los archivos CSS en el `Styles` carpeta, mientras que el diseño general de la página se define en la página maestra, `Site.master`.


[![El sitio Web de libro de revisiones ofrece las revisiones de una gran variedad de títulos](determining-what-files-need-to-be-deployed-vb/_static/image2.png)](determining-what-files-need-to-be-deployed-vb/_static/image1.png)

**Figura 1**: sitio Web de la libreta de revisiones ofrece las revisiones de una gran variedad de títulos ([haga clic aquí para ver imagen en tamaño completo](determining-what-files-need-to-be-deployed-vb/_static/image3.png))


Esta aplicación no usa una base de datos; cada revisión se implementa como una página web independiente en la aplicación. En este tutorial (y la siguientes varios tutoriales) guían por la implementación de una aplicación web que no tiene una base de datos. Sin embargo, en un futuro tutorial se mejorará esta aplicación para almacenar las revisiones, los comentarios de lector y otra información dentro de una base de datos y explorará qué pasos deben realizarse para implementar correctamente una aplicación web controlada por datos.

> [!NOTE]
> Estos tutoriales se centran en las aplicaciones de ASP.NET con un proveedor de hospedaje web que hospeda y no explorar temas auxiliares, como ASP. Sistema de sitio del mapa de la red o utilizando una clase de página base. Para obtener más información sobre estas tecnologías y para obtener más información sobre otros temas tratados en el tutorial, consulte la sección Lecturas adicionales al final de cada tutorial.


Descarga de este tutorial tiene dos copias de la aplicación web, cada uno se implementa como un tipo de proyecto de Visual Studio diferentes: BookReviewsWAP, BookReviewsWSP, un proyecto de sitio Web y un proyecto de aplicación Web. Ambos proyectos creados con Visual Web Developer 2008 SP1 y usar ASP.NET 3.5 SP1. Para trabajar con estos proyectos de inicio, descomprima el contenido en su escritorio. Para abrir el proyecto de aplicación Web (BookReviewsWAP), vaya a la `BookReviewsWAP` carpeta y haga doble clic en el archivo de solución `BookReviewsWAP.sln`. Para abrir el proyecto de sitio Web (BookReviewsWSP), inicie Visual Studio y, a continuación, en el menú archivo, elija la opción de abrir sitio Web, vaya a la `BookReviewsWSP` carpeta en el escritorio y haga clic en Aceptar.


Las dos secciones restantes en este tutorial vistazo a qué archivos, deberá copiar en el entorno de producción al implementar la aplicación. Los siguientes dos tutoriales - [ *implementar su sitio mediante FTP* ](deploying-your-site-using-an-ftp-client-vb.md) y [ *implementar su sitio con Visual Studio* ](deploying-your-site-using-visual-studio-vb.md) -muestran distintas formas Copie estos archivos en un proveedor de hospedaje web.

## <a name="determining-the-files-to-deploy-for-the-web-application-project"></a>Determinación de los archivos de implementación para el proyecto de aplicación Web

El modelo de proyecto de aplicación Web usa la compilación explícita: código fuente del proyecto se compila en un único ensamblado cada vez que compile la aplicación. Esta compilación incluye archivos de código subyacente de las páginas de ASP.NET (~ /`Default.aspx.vb`, ~ /`About.aspx.vb`, y así sucesivamente), así como el `BasePage.vb` clase. El ensamblado resultante se denomina `BookReviewsWAP.dll` y se encuentra en la aplicación `Bin` directory.

Figura 2 muestra los archivos que componen el proyecto de aplicación Web de libreta de revisiones.


[![El Explorador de soluciones muestra los archivos que componen el proyecto de aplicación Web.](determining-what-files-need-to-be-deployed-vb/_static/image5.png)](determining-what-files-need-to-be-deployed-vb/_static/image4.png)

**Figura 2**: el Explorador de soluciones muestra los archivos que componen el proyecto de aplicación Web


> [!NOTE]
> Como se muestra en la figura 2, los archivos de código subyacente de las páginas de ASP.NET no se muestran en el Explorador de soluciones para un proyecto de aplicación Web de Visual Basic. Para ver la clase de código subyacente para una página, haga doble clic en la página en el Explorador de soluciones y elija Ver código.


Para implementar una aplicación de ASP.NET desarrollada con el inicio del modelo de proyecto de aplicación Web mediante la creación de la aplicación con el fin de compilar explícitamente el código fuente más reciente en un ensamblado. A continuación, copie los siguientes archivos en el entorno de producción:

- Página de los archivos que contienen el marcado declarativo para cada ASP.NET, como ~ /`Default.aspx`, ~ /`About.aspx`, y así sucesivamente. Además, copia de seguridad el marcado declarativo para las páginas maestras y controles de usuario.
- Los ensamblados (`.dll` archivos) en el `Bin` carpeta. No es necesario copiar los archivos de base de datos de programa (`.pdb`) o cualquier archivo XML que puede encontrar en el `Bin` directory.

No es necesario copiar los archivos de código fuente de las páginas de ASP.NET en el entorno de producción ni es necesario copiar el `BasePage.vb` archivo de clase.

> [!NOTE]
> Como se muestra en la figura 2, el `BasePage` clase se implementa como un archivo de clase en el proyecto, se coloca en la carpeta denominada `HelperClasses`. Cuando el proyecto se compila el código en el `BasePage.vb` archivo se compila junto con las clases de código subyacente de las páginas de ASP.NET en el ensamblado único, `BookReviewsWAP.dll`. ASP.NET tiene una carpeta especial denominada `App_Code` que está diseñado para almacenar archivos de clase para los proyectos de sitio Web. El código en el `App_Code` carpeta se compila automáticamente y, por tanto, no debe usarse con proyectos de aplicación Web. En su lugar, debe colocar los archivos de clase de la aplicación en una carpeta normal denominada `HelperClasses`, o `Classes`, o algo similar. Como alternativa, puede colocar archivos de clase en un proyecto de biblioteca de clases independiente.


Además de copiar los archivos de marcado relacionados con ASP.NET y el ensamblado en el `Bin` carpeta, también deberá copiar los archivos de soporte técnico de cliente - imágenes y archivos CSS -, así como los archivos de compatibilidad de servidor, `Web.config` y `Web.sitemap`. Estos del cliente y servidor de archivos de compatibilidad que se copiarán en el entorno de producción, independientemente de si utiliza las compilación explícita o automática.

## <a name="determining-the-files-to-deploy-for-the-web-site-project-files"></a>Determinación de los archivos de implementación para los archivos de proyecto de sitio Web

El modelo de proyecto de sitio Web admite la compilación automática, una característica no está disponible cuando se usa el modelo de proyecto de aplicación Web. Con la compilación explícita debe compilar el código de origen del proyecto en un ensamblado y copiar ese ensamblado en el entorno de producción. Por otro lado, con la compilación automática simplemente copie el código fuente para el entorno de producción y se compila el tiempo de ejecución a petición, según sea necesario.

La opción de menú de la compilación en Visual Studio está presente en los proyectos de aplicación Web y proyectos de sitios Web. Creación de un proyecto de aplicación Web compila código fuente del proyecto en un único ensamblado ubicado en el `Bin` directorio; creación de un proyecto de sitio Web comprueba los errores de tiempo de compilación, pero no crea ningún ensamblado. Para implementar una aplicación ASP.NET desarrollada mediante el modelo de proyecto de sitio Web de todos los que debe hacer es copiar los archivos adecuados para el entorno de producción, pero le recomiendo compilar primero el proyecto para asegurarse de que no hay ningún error de tiempo de compilación.

Figura 3 muestra los archivos que componen el proyecto de sitio Web de las revisiones de libro.


[![El Explorador de soluciones muestra los archivos que componen el proyecto de sitio Web.](determining-what-files-need-to-be-deployed-vb/_static/image7.png)](determining-what-files-need-to-be-deployed-vb/_static/image6.png)

**Figura 3**: el Explorador de soluciones muestra los archivos que componen el proyecto de sitio Web


Implementar un proyecto de sitio Web implica copiar todos los archivos relacionados con ASP.NET en el entorno de producción - que incluye las páginas de marcado de páginas ASP.NET, las páginas maestras y controles de usuario, junto con sus archivos de código. También deberá copiar los archivos de clase, como `BasePage.vb`. Tenga en cuenta que el `BasePage.vb` archivo se encuentra en la `App_Code` carpeta, que es una carpeta especial de ASP.NET usan en proyectos de sitios Web para archivos de clase. Debe crearse en producción, también, como los archivos de clase en la carpeta especial del `App_Code` carpeta en el entorno de desarrollo debe copiarse en el `App_Code` carpeta en producción.

Además de copiar los archivos de código de marcado y código fuente ASP.NET, también deberá copiar los archivos de soporte técnico de cliente - imágenes y archivos CSS -, así como los archivos de compatibilidad de servidor, `Web.config` y `Web.sitemap`.

> [!NOTE]
> Proyectos de sitios Web también puede usar la compilación explícita. Un futuro tutorial examinará cómo compilar explícitamente un proyecto de sitio Web.


## <a name="summary"></a>Resumen

Implementar una aplicación ASP.NET implica copiar los archivos necesarios del entorno de desarrollo al entorno de producción. El conjunto preciso de los archivos que deben sincronizarse depende de si la aplicación ASP.NET se explícita o automáticamente compila el código. La estrategia de compilación que emplea viene determinado por si se ha configurado Visual Studio para administrar la aplicación de ASP.NET mediante el modelo de proyecto de aplicación Web o el modelo de proyecto de sitio Web.

El modelo de proyecto de aplicación Web utiliza la compilación explícita y compila el código del proyecto en un único ensamblado en el `Bin` carpeta. Al implementar la aplicación, la parte del marcado de las páginas ASP.NET y el contenido de la `Bin` carpeta se debe insertar en el entorno de producción; no es necesario el código fuente en la aplicación - archivos de código y las clases de código subyacente, por ejemplo: que se copiarán en el entorno de producción.

El modelo de proyecto de sitio Web usa compilación automática de forma predeterminada, aunque es posible compilar explícitamente un proyecto de sitio Web, como veremos en el futuro tutoriales. Implementar una aplicación ASP.NET que usa la compilación automática requiere que la parte del marcado *y* se debe copiar el código fuente en el entorno de producción. El código se compila automáticamente en el entorno de producción cuando se solicita por primera vez.

Ahora que hemos examinado los archivos que se sincronizan entre los entornos de desarrollo y producción estamos preparados implementar la aplicación de reseñas de libros a un proveedor de hospedaje web.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Información general sobre la compilación de ASP.NET](https://msdn.microsoft.com/library/ms178466.aspx)
- [Controles de usuario ASP.NET](https://msdn.microsoft.com/library/y6wb1a0e.aspx)
- [Examen de ASP. Navegación del sitio de red](http://aspnet.4guysfromrolla.com/articles/111605-1.aspx)
- [Introducción a los proyectos de aplicación Web](https://msdn.microsoft.com/library/aa730880.aspx)
- [Tutoriales de página maestra](../master-pages/creating-a-site-wide-layout-using-master-pages-cs.md)
- [Compartir código entre páginas](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/pages/code.aspx)
- [Uso de una clase Base personalizada para las clases de código subyacente de las páginas de ASP.NET](http://aspnet.4guysfromrolla.com/articles/041305-1.aspx)
- [Sistema de proyectos de sitio Web de Visual Studio 2005: ¿qué es y por qué lo hacemos?](https://weblogs.asp.net/scottgu/archive/2005/08/21/423201.aspx)
- [Tutorial: Convertir un proyecto de sitio Web en un proyecto de aplicación Web en Visual Studio](https://msdn.microsoft.com/library/aa983476.aspx)

> [!div class="step-by-step"]
> [Anterior](asp-net-hosting-options-vb.md)
> [Siguiente](deploying-your-site-using-an-ftp-client-vb.md)
