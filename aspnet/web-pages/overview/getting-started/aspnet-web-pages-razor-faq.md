---
uid: web-pages/overview/getting-started/aspnet-web-pages-razor-faq
title: ASP.NET Web Pages (Razor) preguntas más frecuentes | Microsoft Docs
author: tfitzmac
description: En este artículo se enumera algunas preguntas frecuentes sobre ASP.NET Web Pages (Razor) y WebMatrix. Versiones de software que se usa en el tutorial ASP.NET Web Pages (r...
ms.author: riande
ms.date: 02/07/2014
ms.assetid: b137bd04-25e1-47cb-9d96-ef2e179ecf1f
msc.legacyurl: /web-pages/overview/getting-started/aspnet-web-pages-razor-faq
msc.type: authoredcontent
ms.openlocfilehash: 03e89adc2415808ea49107616f529fa895d6e52a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836071"
---
<a name="aspnet-web-pages-razor-faq"></a>ASP.NET Web Pages (Razor) preguntas más frecuentes
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> > [!NOTE] 
> > WebMatrix no se recomienda como un entorno de desarrollo integrado para ASP.NET Web Pages. Use [Visual Studio](xref:aspnet/web-pages/overview/getting-started/program-asp-net-web-pages-in-visual-studio) o [código de Visual Studio](https://code.visualstudio.com/).
>
> En este artículo se enumera algunas preguntas frecuentes sobre ASP.NET Web Pages (Razor) y WebMatrix.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
> - Visual Studio 2013
> - WebMatrix 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2, 2 de WebMatrix y Visual Studio 2012.


- [¿Qué es la diferencia entre las páginas Web ASP.NET, formularios Web Forms ASP.NET y ASP.NET MVC?](#Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC)
- [¿Es necesario WebMatrix para trabajar con páginas Web?](#Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages)
- [¿Puedo usar controles de formularios Web Forms de ASP.NET en una página de páginas Web?](#Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page)
- [¿Puedo implementar un sitio de ASP.NET Web Pages sin usar WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix)
- [¿Es necesario usar la aplicación auxiliar WebSecurity para admitir inicios de sesión?](#Do_I_have_to_use_the_WebSecurity_helper_to_support_logins)
- [¿ASP.NET Web Pages admite HTML5?](#Does_ASP.NET_Web_Pages_support_HTML5)
- [¿Puedo usar JavaScript y jQuery con páginas Web?](#Can_I_use_JavaScript_and_jQuery_with_Web_Pages)
- [Recursos adicionales](#AdditionalResources)

Si tiene preguntas sobre los errores y otros problemas, consulte el [Guía de solución de problemas de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001).

<a id="Whats_the_difference_between_ASP.NET_Web_Pages,_ASP.NET_Web_Forms,_and_ASP.NET_MVC"></a>
## <a name="whats-the-difference-between-aspnet-web-pages-aspnet-web-forms-and-aspnet-mvc"></a>¿Qué es la diferencia entre las páginas Web ASP.NET, formularios Web Forms ASP.NET y ASP.NET MVC?

Las tres son tecnologías ASP.NET para crear aplicaciones web dinámicas:

- ASP.NET Web Pages se centra en la adición dinámica de código (servidor) y el acceso de la base de datos a páginas HTML y la sintaxis simple y ligera de características.
- ASP.NET Web Forms se basa en un modelo de objetos de página y controles de tipo de ventana tradicional (botones, listas, etcetera.). Formularios Web Forms utiliza un modelo basado en eventos que resultan familiar a los que ha trabajado con el desarrollo con (Windows forms) basada en cliente.
- ASP.NET MVC implementa el patrón model-view-controller para ASP.NET. El énfasis está puesto en "separación de preocupaciones" (procesamiento, datos y capas de interfaz de usuario).

Los tres marcos son totalmente compatibles y continuarán se desarrollado por el equipo ASP.NET. En general, la elección de qué marco que use depende de fondo de la experiencia y con ASP.NET.

ASP.NET Web Pages en particular se diseñó para que resulte sencillo para las personas que ya conocen HTML para agregar el procesamiento del servidor a sus páginas. Es una buena opción para estudiantes, aficionados, las personas en general que no están familiarizados con la programación. También puede ser una buena elección para los desarrolladores que tienen experiencia con las tecnologías web que no sean ASP.NET.

<a id="Do_I_need_WebMatrix_in_order_to_work_with_Web_Pages"></a>
## <a name="do-i-need-webmatrix-in-order-to-work-with-web-pages"></a>¿Es necesario WebMatrix para trabajar con páginas Web?

No. WebMatrix no se recomienda como un entorno de desarrollo integrado para ASP.NET Web Pages. Use [Visual Studio](program-asp-net-web-pages-in-visual-studio.md) o [código de Visual Studio](https://code.visualstudio.com/).

Si no desea usar Visual Studio o Visual Studio Code, puede instalar los productos de componente individualmente mediante [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx). Necesita los siguientes productos:

- Microsoft .NET Framework 4.5
- ASP.NET MVC 5 (que se instala el marco de ASP.NET Web Pages)
- IIS Express (el servidor web)
- Microsoft SQL Server Compact 4.0 (la base de datos)

Puede usar un editor de texto para editar *.cshtml* (o *.vbhtml*) páginas.

Administración de bases de datos de SQL Server Compact (*.sdf* archivos) sin una herramienta es un poco más difícil. Visual Studio tools de containds para administrar *.sdf* bases de datos. También puede ejecutar comandos SQL en el código para realizar muchas tareas de administración de SQL Server.

Para probar *.cshtml* páginas sin usar un entorno de desarrollo integrado (IDE), puede implementarlos en un servidor activo. (Consulte [puedo implementar un sitio de ASP.NET Web Pages sin usar WebMatrix?](#Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix))

### <a name="running-iis-express-without-using-an-ide"></a>Está ejecutando IIS Express sin usar un IDE

Si instala IIS Express en el equipo como un servidor web, puede usar para probar las páginas. Puede ejecutar IIS Express desde la línea de comandos y asociarlo con un número de puerto específico. A continuación, especifique ese puerto al solicitar *.cshtml* archivos en el explorador.

En Windows, abra un símbolo del sistema con privilegios de administrador y cambie a *C:\Program Files\IIS Express.* (Para sistemas de 64 bits, utilice la carpeta *C:\Program Files (x86) \IIS Express.)* A continuación, escriba el comando siguiente, con la ruta de acceso real a su sitio:

`iisexpress.exe /port:35896 /path:C:\BasicWebSite`

Puede usar cualquier número de puerto que ya no está reservada por algún otro proceso. (Los números de puerto por encima de 1024 son normalmente libres). Para el `path` valor, use la ruta de acceso de la carpeta del sitio Web donde el *.cshtml* son archivos.

Después de ejecutar este comando para configurar IIS Express para atender las páginas, puede abrir un explorador y vaya a un *.cshtml* archivo. Use una dirección URL similar al siguiente:

`http://localhost:35896/default.cshtml`

Para obtener ayuda con las opciones de línea de comandos de IIS Express, escriba `iisexpress.exe /?` en la línea de comandos.

<a id="Can_I_use_ASP.NET_Web_Forms_controls_on_a_Web_Pages_page"></a>
## <a name="can-i-use-aspnet-web-forms-controls-on-a-web-pages-page"></a>¿Puedo usar controles de formularios Web Forms de ASP.NET en una página de páginas Web?

No. Controles de formularios Web, como el [casilla](https://msdn.microsoft.com/library/system.web.ui.webcontrols.checkbox) (control), el [controles de validación](https://msdn.microsoft.com/library/bwd43d0x)y el [GridView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview) controlar sólo funciona en las páginas de formularios Web Forms (*.aspx* archivos). Estos controles requieren que el marco de páginas de formularios Web Forms.

<a id="Can_I_deploy_an_ASP.NET_Web_Pages_site_without_using_WebMatrix"></a>
## <a name="can-i-deploy-an-aspnet-web-pages-site-without-using-webmatrix"></a>¿Puedo implementar un sitio de ASP.NET Web Pages sin usar WebMatrix?

Sí. Puede copiar manualmente los archivos del sitio Web a un servidor (normalmente mediante el uso de FTP). Si realiza una copia manual, también debe copiar los archivos que admiten SQL Server Compact (la base de datos). Para obtener más información, consulte la entrada de blog [las páginas Web de implementación de aplicaciones sin una herramienta](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2317).

<a id="Do_I_have_to_use_the_WebSecurity_helper_to_support_logins"></a>
## <a name="do-i-have-to-use-the-websecurity-helper-to-support-logins"></a>¿Es necesario usar la aplicación auxiliar WebSecurity para admitir inicios de sesión?

No. El `SimpleMembership` proveedor que forma parte de ASP.NET Web Pages es una opción. Los proveedores de seguridad que forman parte de ASP.NET (que se puede utilizar para trabajar con en formularios Web Forms) también están disponibles. Por ejemplo, puede usar la autenticación de formularios en ASP.NET Web Pages como haría en formularios Web Forms. Para un ejemplo de cómo usar la autenticación de formularios, consulte el artículo de Microsoft Support [How To Implement Forms-Based autenticación en su aplicación de ASP.NET mediante el uso de C# .NET](https://support.microsoft.com/kb/301240). Para descargar un ejemplo sencillo, consulte [versión de ASP.NET de "inicio de sesión &amp; contraseña](http://www.codeguru.com/csharp/.net/net_asp/scripting/article.php/c19295/ASPNET-version-of-Login--Password.htm).

Para obtener información acerca de cómo usar la autenticación de Windows, consulte la entrada de blog [en ASP.NET Web Pages de la autenticación de Windows utilizando](http://mikepope.com/blog/DisplayBlog.aspx?permalink=2298).

<a id="Does_ASP.NET_Web_Pages_support_HTML5"></a>
## <a name="does-aspnet-web-pages-support-html5"></a>¿ASP.NET Web Pages admite HTML5?

Sí. Las páginas que crea con ASP.NET Web Pages (*.cshtml* o *.vbhtml* páginas) son básicamente las páginas HTML que también contienen código que se ejecuta en el servidor, antes de presenta la página. Siempre que el explorador del usuario es compatible con HTML5, puede usar elementos de HTML5 en un *.cshtml* o *.vbhtml* página.

<a id="Can_I_use_JavaScript_and_jQuery_with_Web_Pages"></a>
## <a name="can-i-use-javascript-and-jquery-with-web-pages"></a>¿Puedo usar JavaScript y jQuery con páginas Web?

Por supuesto. Las páginas que crea con ASP.NET Web Pages (*.cshtml* o *.vbhtml* páginas) son simplemente páginas HTML con código del servidor en ellos. Por lo tanto, todo lo que puede hacer en una página HTML normal con JavaScript o jQuery también puede hacer en un *.cshtml* o *.vbhtml* página.

El **Starter Site** plantilla en WebMatrix contiene un número de bibliotecas de jQuery. Si crea un sitio mediante el uso de esa plantilla, el *Scripts* carpeta contiene una biblioteca de jQuery core (*1.6.2.js de jquery)* y bibliotecas para la validación de jQuery (*jquery.validate.js*, etcetera.).

Estos son algunos blogs que ilustran cómo usar jQuery con ASP.NET Web Pages:

- [Al agregar jQuery adecuación a ASP.NET Web Pages con WebMatrix](http://rachelappel.com/jquery/adding-jquery-goodness-to-asp-net-web-pages-using-webmatrix/) Rachel Appel
- [5 minutos: WebMatrix jQuery UI json + + jQuery plantillas](http://joeriks.com/2011/01/30/5-min-webmatrix-jquery-ui-json-jquery-templates/) por Jonas Eriksson
- [WebMatrix y formularios de jQuery](http://mikesdotnetting.com/Article/155/WebMatrix-And-jQuery-Forms) por Mike Brind

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a>Recursos adicionales


[Guía de solución de problemas de ASP.NET Web Pages (Razor)](https://go.microsoft.com/fwlink/?LinkId=253001)

[Foro de WebMatrix y ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix) en el sitio Web ASP.NET
