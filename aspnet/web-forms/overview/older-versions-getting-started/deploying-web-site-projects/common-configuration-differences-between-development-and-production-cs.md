---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: Diferencias de configuración comunes entre el desarrollo y producción (C#) | Microsoft Docs
author: rick-anderson
description: En los tutoriales anteriores, hemos implementado nuestro sitio Web copiando todos los archivos pertinentes desde el entorno de desarrollo en el entorno de producción. Sin embargo,...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3476cdb25a0fc87ce233d7e26de094a0b7b17c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808913"
---
<a name="common-configuration-differences-between-development-and-production-c"></a>Diferencias de configuración comunes entre el desarrollo y producción (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> En los tutoriales anteriores, hemos implementado nuestro sitio Web copiando todos los archivos pertinentes desde el entorno de desarrollo en el entorno de producción. Sin embargo, no es raro que haya diferencias de configuración entre entornos, lo que necesita que cada entorno tiene un único archivo Web.config. Este tutorial examina las diferencias de configuración típicos y se examina las estrategias para mantener la información de configuración independiente.


## <a name="introduction"></a>Introducción


Los dos últimos tutoriales ha avanzado a través de la implementación de una aplicación web sencilla. El [ *implementar el sitio mediante un cliente FTP* ](deploying-your-site-using-an-ftp-client-cs.md) tutorial ha mostrado cómo usar un cliente FTP independiente para copiar los archivos necesarios del entorno de desarrollo hasta la producción. El tutorial anterior, [ *implementar su sitio con Visual Studio*](deploying-your-site-using-visual-studio-cs.md), buscar en la implementación mediante la herramienta Copiar sitio Web y la opción de publicación de Visual Studio. En ambos tutoriales todos los archivos en el entorno de producción era una copia de un archivo en el entorno de desarrollo. Sin embargo, no es infrecuente que los archivos de configuración en el entorno de producción diferentes a los del entorno de desarrollo. Configuración de la aplicación web se almacena en la `Web.config` de archivos y suele incluir información acerca de los recursos externos, como la base de datos, web y servidores de correo electrónico. También detalla el comportamiento de la aplicación en determinadas situaciones, como el curso de acción que se realizará cuando se produce una excepción no controlada.

Al implementar una aplicación web es importante que la información de configuración correcto que terminen en el entorno de producción. En la mayoría de los casos el `Web.config` no se puede copiar el archivo en el entorno de desarrollo al entorno de producción como-es. En su lugar, una versión personalizada de `Web.config` debe cargarse en producción. En este tutorial se revisa brevemente algunas de las diferencias de configuración más comunes; También resume algunas técnicas para mantener la información de configuración diferente entre los entornos.

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>Configuración típica de diferencias entre los entornos de producción y desarrollo

El `Web.config` archivo incluye una gran variedad de información de configuración para una aplicación ASP.NET. Parte de esta información de configuración es el mismo independientemente del entorno. Por ejemplo, la configuración de autenticación y las reglas de autorización de dirección URL especificada en el `Web.config` del archivo `<authentication>` y `<authorization>` elementos suelen ser el mismo independientemente del entorno. Pero otra información de configuración - como información sobre los recursos externos - normalmente difiere según el entorno.

Cadenas de conexiones de base de datos son un buen ejemplo de información de configuración que difiere en función del entorno. Cuando una aplicación web se comunica con un servidor de base de datos primero debe establecer una conexión, y esto se consigue a través de un [cadena de conexión](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string). Aunque es posible codificar la cadena de conexión de base de datos directamente en las páginas web o el código que se conecta a la base de datos, es preferible colocarlo `Web.config`del [ `<connectionStrings>` elemento](https://msdn.microsoft.com/library/bf7sd233.aspx) para que la cadena de conexión información está en una única ubicación centralizada. A menudo se usa otra base de datos durante el desarrollo que se usa en producción; por lo tanto, la información de la cadena de conexión debe ser única para cada entorno.

> [!NOTE]
> Tutoriales futuros exploran la implementación de aplicaciones controladas por datos, momento en que analizaremos en profundidad los detalles de cómo se almacenan cadenas de conexión de base de datos en el archivo de configuración.


El comportamiento previsto de los entornos de desarrollo y producción difiere considerablemente. Una aplicación web en el entorno de desarrollo se está creando, probado y depurado por un pequeño grupo de desarrolladores. En el entorno de producción que se está visitando la misma aplicación por distintos usuarios simultáneos. ASP.NET incluye una serie de características que ayudan a los desarrolladores probar y depurar una aplicación, pero se deben deshabilitar estas características por motivos de seguridad en el entorno de producción y rendimiento. Echemos un vistazo a algunas opciones de configuración de dicha configuración.

### <a name="configuration-settings-that-impact-performance"></a>Opciones de configuración que afectan al rendimiento

Cuando se visita una página ASP.NET para la primera vez (o la primera vez después de haya cambiado), se debe convertir su marcado declarativo en una clase y esta clase se debe compilar. Si la aplicación web usa una compilación automática, a continuación, la clase de código subyacente de la página debe compilarse, demasiado. Puede configurar una gran variedad de opciones de compilación a través de la `Web.config` del archivo [ `<compilation>` elemento](https://msdn.microsoft.com/library/s10awwz0.aspx).

El atributo de depuración es uno de los atributos más importantes en el `<compilation>` elemento. Si el `debug` atributo está establecido en "true", a continuación, los ensamblados compilados incluyen símbolos de depuración, que son necesarias al depurar una aplicación en Visual Studio. Pero aumentan el tamaño del conjunto de símbolos de depuración e imponen requisitos adicionales de memoria cuando se ejecuta el código. Además, cuando el `debug` atributo está establecido en "true", cualquier contenido devuelto por `WebResource.axd` no está en caché, lo que significa que cada vez que un usuario visita una página tendrán que volver a descargar el contenido estático que devuelve `WebResource.axd`.

> [!NOTE]
> `WebResource.axd` es un controlador HTTP integrado presentado en ASP.NET 2.0 que utilizan los controles de servidor para recuperar los recursos incrustados, como archivos de script, imágenes, archivos CSS y otro contenido. Para obtener más información acerca de cómo `WebResource.axd` funciona y cómo puede utilizarlo para tener acceso a los recursos incrustados desde los controles de servidor personalizado, vea [acceso incrustado recursos a través de una dirección URL usando `WebResource.axd` ](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx).


El `<compilation>` del elemento `debug` atributo normalmente se establece en "true" en el entorno de desarrollo. De hecho, este atributo debe establecerse en "true" para depurar una aplicación web; Si se intenta depurar una aplicación ASP.NET desde Visual Studio y el `debug` está establecido en "false", Visual Studio mostrará un mensaje que explica que no se puede depurar la aplicación hasta que el `debug` atributo está establecido en "true" y le oferta para realizar este cambio automáticamente.

Debería **nunca** tienen la `debug` atributo establecido en "true" en un entorno de producción debido a su impacto en el rendimiento. Para obtener una explicación más completa sobre este tema, consulte [Scott Guthrie](https://weblogs.asp.net/scottgu/)de entrada de blog, [no ejecutar producción ASP.NET aplicaciones con `debug="true"` habilitado](https://weblogs.asp.net/scottgu/442448).

### <a name="custom-errors-and-tracing"></a>Seguimiento y los errores personalizados

Cuando se produce una excepción no controlada en una aplicación ASP.NET propaga hasta el tiempo de ejecución en ese momento se produce una de estas tres cosas:

- Se muestra un mensaje de error genérico en tiempo de ejecución. Esta página informa al usuario que se ha producido un error en tiempo de ejecución, pero no proporciona los detalles sobre el error.
- Se muestra un mensaje de detalles de excepción, que incluye información sobre la excepción que se produce solo.
- Se muestra una página de error personalizada, que es una página ASP.NET que cree que muestra cualquier mensaje que desee.

¿Qué ocurre en el caso de una excepción no controlada depende de la `Web.config` del archivo [ `<customErrors>` sección](https://msdn.microsoft.com/library/h0hfz6fc.aspx).

Al desarrollar y probar una aplicación que ayuda a ver los detalles de cualquier excepción en el explorador. Sin embargo, que muestra los detalles de la excepción en una aplicación en producción es un riesgo de seguridad. Además, es unflattering y hace que su sitio Web parezca poco profesional. Lo ideal es que, si se produce una excepción no controlada una aplicación web en el entorno de desarrollo mostrará los detalles de la excepción mientras la misma aplicación en producción mostrará una página de error personalizada.

> [!NOTE]
> El valor predeterminado `<customErrors>` valor de la sección muestra los detalles de la excepción del mensaje sólo cuando la página se visita a través de localhost y, en caso contrario, muestra la página de error genérico en tiempo de ejecución. Esto no es lo ideal, pero se garantiza para saber que el comportamiento predeterminado no mostrar los detalles de excepción para los visitantes no locales. Un futuro tutorial examina el `<customErrors>` sección con más detalle y se muestra cómo hacer que una página de error personalizado que se muestra cuando se produce un error en producción.


Otra característica ASP.NET que es útil durante el desarrollo de seguimiento. Seguimiento, si está habilitada, registra información acerca de cada solicitud entrante y proporciona una página web especial, `Trace.axd`, para ver los detalles de solicitud reciente. Puede activar y configurar el seguimiento a través de la [ `<trace>` elemento](https://msdn.microsoft.com/library/6915t83k.aspx) en `Web.config`.

Si habilita la traza Asegúrese de que TI está deshabilitada en producción. Dado que la información de seguimiento incluye las cookies, datos de la sesión y otra información potencialmente confidencial, es importante deshabilitar el seguimiento en producción. La buena noticia es que, de forma predeterminada, el seguimiento está deshabilitado y el `Trace.axd` archivo solo es accesible a través de localhost. Si cambia estos valores predeterminados en el desarrollo, asegúrese de que están desactivados volver en producción.

## <a name="techniques-for-maintaining-separate-configuration-information"></a>Técnicas para mantener la información de configuración independiente

Tener distintos valores de configuración en los entornos de desarrollo y producción complica el proceso de implementación. En los dos tutoriales anteriores el proceso de implementación implica copiar todos los archivos necesarios desde el desarrollo en producción, pero ese enfoque solo funciona si la información de configuración es la misma en ambos entornos. Hay una variedad de técnicas para implementar una aplicación con información de configuración diferentes. Vamos a catálogo algunas de estas opciones para aplicaciones web hospedadas.

### <a name="manually-deploying-the-production-environment-configuration-file"></a>Implementar manualmente el archivo de configuración del entorno de producción

El enfoque más sencillo consiste en mantener dos versiones de la `Web.config` archivos: uno para el entorno de desarrollo y otra para el entorno de producción. Implementar un sitio de producción implica copiar todos los archivos en el servidor de producción en el entorno de desarrollo *excepto* para el `Web.config` archivo. En su lugar, el entorno de producción específicos `Web.config` archivos se copian a producción.

Este enfoque no es muy sofisticado, pero es fácil de implementar porque la información de configuración cambia con poca frecuencia. Funciona mejor para las aplicaciones con un equipo de desarrollo pequeños que se hospedan en un único servidor web y cuya información de configuración se cambia con poca frecuencia. Es más tenable al implementar manualmente los archivos de aplicación mediante un cliente FTP independiente. Cuando se usa la herramienta Copiar sitio Web o la opción de publicación de Visual Studio, deberá primero intercambiar la implementación específica `Web.config` de archivos con la producción específica antes de implementar y, después, intercambie después de que finalice la implementación.

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>Cambiar la configuración durante la compilación o el proceso de implementación

Las discusiones hasta ahora suponen que un proceso de compilación e implementación de ad hoc. Muchos proyectos de software más grandes han formalizado más procesos que hacen uso de código abierto, internamente, o herramientas de terceros. Para estos proyectos es probable que puede personalizar el proceso de compilación o implementación para modificar correctamente la información de configuración antes de que se inserte en producción. Si compila la aplicación web mediante [MSBuild](http://en.wikipedia.org/wiki/MSBuild), [NAnt](http://nant.sourceforge.net/), o alguna otra herramienta de compilación, es probable que se puede agregar un paso de compilación para modificar el `Web.config` archivo para incluir la configuración específica de producción. O el flujo de trabajo de implementación mediante programación puede conectarse al servidor de control de código fuente y recuperar adecuado `Web.config` archivo.

El enfoque para obtener la información de configuración correspondiente a la producción real varía ampliamente en función de sus herramientas y flujo de trabajo. Por lo tanto, no se ahondará en este tema aún más. Si usa una herramienta de compilación populares como MSBuild o NAnt encontrará tutoriales y artículos de implementación específicos de estas herramientas a través de una búsqueda web.

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>Diferencias de configuración de administración a través de la implementación Web de proyecto complemento

En 2006 Microsoft publicó el complemento de Project de desarrollo Web para Visual Studio 2005. Un complemento para Visual Studio 2008 se publicó en 2008. Este complemento permite a los desarrolladores ASP.NET crear un proyecto de implementación Web independiente junto con su proyecto de aplicación web, cuando compila, explícitamente compila la aplicación web y copia los archivos para implementar en un directorio de salida local. El proyecto de aplicación Web usa MSBuild en segundo plano.

De forma de predeterminada, el entorno de desarrollo `Web.config` archivo se copia en el directorio de salida, pero puede configurar el proyecto de implementación Web para personalizar el

información de configuración que se copie en este directorio de las maneras siguientes:

- A través de `Web.config` reemplazo de sección, en el que se especifique la sección para reemplazar y un archivo XML que contiene el texto de reemplazo de archivos.
- Al proporcionar una ruta de acceso a un archivo de origen de configuración externo. Con esta opción seleccionada, el proyecto de implementación Web copia un determinado `Web.config` archivo al directorio de salida (en lugar de `Web.config` archivo usado en el entorno de desarrollo).
- Mediante la adición de reglas personalizadas para el archivo de MSBuild utilizado por el proyecto de implementación Web.

Para implementar la compilación el proyecto de implementación Web de la aplicación web y, a continuación, copie los archivos de la carpeta de salida del proyecto en el entorno de producción.

Para obtener información sobre cómo usar el proyecto de implementación Web, consulte [en este artículo de proyectos de implementación Web](https://msdn.microsoft.com/magazine/cc163448.aspx) de la edición de abril de 2007 de [MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx), o consulte los vínculos en la sección Lecturas adicionales en el final de este tutorial.

> [!NOTE]
> No se puede usar el proyecto de implementación Web con Visual Web Developer porque el proyecto de implementación Web se implementa como un Visual Studio Add-In y Visual Studio Express Editions (incluyendo Visual Web Developer) no admiten complementos.


## <a name="summary"></a>Resumen

Los recursos externos y el comportamiento de una aplicación web en el desarrollo son normalmente diferentes que cuando sea la misma aplicación en producción. Por ejemplo, las cadenas de conexión de base de datos, las opciones de compilación y el comportamiento cuando se produce una excepción no controlada normalmente difieren entre entornos. El proceso de implementación debe dar cabida a estas diferencias. Como se explicó en este tutorial, el enfoque más sencillo es copiar manualmente un archivo de configuración alternativa en el entorno de producción. Soluciones más inteligentes son posibles cuando se usa el complemento de Project de implementación Web o con un proceso de compilación o implementación más formal que puede dar cabida a estas personalizaciones.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Explicados las cadenas de conexión](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [@ ConnectionStrings.com de cadenas de conexión de base de datos](http://www.connectionstrings.com/)
- [No se ejecutan las aplicaciones de ASP.NET de producción con `debug="true"` habilitado](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [Responder correctamente a las excepciones no controladas - mostrar páginas de Error descriptivo](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [Cómo: usar un proyecto de implementación Web de Visual Studio 2008?](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [Opciones de configuración principales al implementar una base de datos](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Descarga de proyectos de Visual Studio 2008 Web implementación](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [descarga de proyectos de Visual Studio 2005 Web Deployment](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [Proyectos de implementación Web de VS 2008](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [VS 2008 implementación compatibilidad con proyectos Web publicado](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Proyectos de implementación web](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-visual-studio-cs.md)
> [Siguiente](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
