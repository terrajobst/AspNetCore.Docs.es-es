---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: 'Introducción a ASP.NET Web Pages: publicar un sitio mediante el uso de WebMatrix | Documentos de Microsoft'
author: tfitzmac
description: Este tutorial es la última entrega en el conjunto de tutorial que presenta ASP.NET Web Pages y WebMatrix de Microsoft. Describe cómo publicar su sitio t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: 7b9bffac5cc72e1bea3f1b211cc03be2ccb8e499
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Introducción a ASP.NET Web Pages: publicar un sitio mediante el uso de WebMatrix
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial es la última entrega en el conjunto de tutorial que presenta ASP.NET Web Pages y WebMatrix de Microsoft. Describe cómo publicar su sitio en Internet para que otros usuarios puedan trabajar con él. Supone que ha completado la serie a través de [crear un aspecto coherente para los sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Obtendrá información sobre cómo publicar su sitio usando:
> 
> - Microsoft Azure
> - Compañía de hospedaje Web


## <a name="about-publishing-your-site"></a>Acerca de cómo publicar su sitio

Hecho hasta ahora, todo el trabajo en un equipo local, incluidas las pruebas de las páginas. Para ejecutar el<em>.cshtml</em> páginas, ha utilizado el servidor web que está integrado en WebMatrix, es decir, IIS Express. Pero, por supuesto nadie puede ver el sitio que salvo que se ha creado. Para permitir que otras personas a trabajar con su sitio, tiene que publicar en Internet.

A menos que tenga acceso a un servidor web público ya, publicación significa que debe tener una cuenta con un *plataforma de nube* o un *proveedor de hospedaje*. Una plataforma de nube, como Microsoft Azure proporciona la infraestructura de petición para sus aplicaciones. Un proveedor de hospedaje es una compañía que posee servidores web accesible públicamente y que alquilan, espacio para su sitio. Hospedaje de planes de ejecución de unos pocos dólares al mes (o incluso libres) para sitios pequeños a muchos cientos de dólares al mes para los sitios Web comerciales de gran volumen.

> [!NOTE]
> Puede tener acceso a un servidor web pública mediante el proveedor de servicios de internet (ISP) que utiliza para obtener el servicio de internet en casa. Sin embargo, el proveedor de hospedaje debe admitir ASP.NET Web Pages. Muchos ISP no, pero siempre es conveniente comprobar.


En este tutorial, le ofrecemos información general sobre cómo publicar. No es práctico proporcionar detalles exactos para todo, porque el proceso es un poco diferente para cada proveedor de hospedaje. Pero, obtendrá una idea clara de cómo funciona el proceso.

Este tutorial contiene cuatro secciones:

1. [Configuración de la página predeterminada](#defaultpage)
2. Publicar (elegir uno de los siguientes)  
 a. [Publicar su sitio en Microsoft Azure](#azure)  
 b. [Publicar su sitio en una empresa de hospedaje Web](#host)
3. [Actualizar el sitio en vivo: volver a publicar](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Configuración de la página predeterminada

Cuando un usuario navega a la dirección base para el sitio web, la página predeterminada para el sitio se muestra al usuario. Por ejemplo, cuando Default.htm se establece como la página predeterminada para el sitio en www.contoso.com, a continuación, vaya a <strong>www.contoso.com</strong> es el mismo que navegar a <strong>www.contoso.com/Default.htm</strong>.

Actualmente, el sitio utiliza **Default.cshtml** como la página predeterminada. Esta página es un problema para la página predeterminada, pero en este tutorial no ha agregado ningún contenido a la página por lo que mostraría una página en blanco. Abra Default.cshtml y reemplace el contenido con el código siguiente.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Ahora su sitio está listo para su publicación. En primer lugar, verá cómo implementar el sitio en Azure y, a continuación, cómo implementarlo en una empresa de hospedaje web. Cualquier opción funciona para el sitio web y solo deberá seguir una de las opciones de implementación.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Publicar su sitio en Microsoft Azure

Este tutorial primero le mostrará cómo implementar el sitio de Microsoft Azure. Al iniciar sesión con una cuenta de Microsoft, puede crear hasta 10 sitios gratuitos en Azure. Estos sitios gratuitos proporcionan una manera cómoda para probar los sitios. Siempre se puede eliminar este sitio de ejemplo más adelante para evitar el uso de todos los sitios libres. Puede crear una cuenta de prueba gratuita en tan solo unos minutos. Para obtener más información, consulte [evaluación gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

En la cinta de opciones de WebMatrix, haga clic en el **publicar** botón.

![Botón 'Publish' en la cinta de opciones de WebMatrix](publishing/_static/image1.png)

El **publicar su sitio Web** se muestra el cuadro de diálogo. Si no ha iniciado sesión en su cuenta de Microsoft, el cuadro de diálogo contendrá una **empezar a trabajar con Azure** vínculo. Haga clic en este vínculo.

![Publica un sitio](publishing/_static/image2.png)

Si no ha iniciado sesión en una cuenta de Microsoft, nuevo se le ofrecerá la oportunidad de iniciar sesión. Debe iniciar sesión en una cuenta de Microsoft para publicar su sitio en Azure.

![Iniciar sesión](publishing/_static/image3.png)

Tras iniciar sesión en tu cuenta de Microsoft, el cuadro de diálogo contiene vínculos para crear un nuevo sitio en Azure o conectarse a uno de los sitios existentes en Azure.

![Crear un sitio nuevo](publishing/_static/image4.png)

Seleccione **crear un nuevo sitio**.

Si con el nombre de su proyecto **WebPagesMovies**, el nombre predeterminado para el sitio será **webpagesmovies.azurewebsites.net**. Este nombre predeterminado es más probable que no está disponible, como se indica mediante la marca de exclamación roja.

![nombre del sitio Web predeterminado](publishing/_static/image5.png)

Cambie el nombre del sitio a algo que está disponible y seleccione una ubicación que esté cerca de su ubicación.

![nombre del sitio modificada](publishing/_static/image6.png)

Haga clic en **Aceptar**.

Performss una prueba para determinar si el servidor es compatible con el sitio de WebMatrix.

![prueba de compatibilidad](publishing/_static/image7.png)

Seleccione **continuar**.

Se muestran los resultados de la prueba de compatibilidad.

![resultado de la compatibilidad](publishing/_static/image8.png)

Seleccione **continuar**.

WebMatrix muestra los archivos y bases de datos que se va a publicar el sitio. Puesto que es la primera vez que se va a publicar el sitio, se muestran todos los archivos. Puede desactivar un archivo que no está listo para publicarse. En las publicaciones posteriores, se mostrará únicamente los archivos que han cambiado. Vea [actualizar el sitio en vivo: volver a publicar](#update).

![vista previa de publicación](publishing/_static/image9.png)

Seleccione **continuar**.

Después de que el sitio se ha implementado en Azure, se muestra un mensaje que indica que ha completado la implementación.

![publicar completa](publishing/_static/image10.png)

El sitio y la base de datos se han publicado en Azure y están ahora disponibles para el público. Haga clic en el vínculo en el mensaje que indica la publicación se ha completado y ahora verá el sitio implementado. Cualquier persona con acceso a Internet puede agregar o modificar registros en la base de datos.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Publicar su sitio en una empresa de hospedaje Web

Si decide no publicar en Azure, en su lugar, puede publicar su sitio en una empresa de hospedaje web.

Haga clic en el **busca hospedaje web** vínculo.

![Botón 'Busca hospedaje web' en el cuadro de diálogo de configuración de publicación](publishing/_static/image12.png)

Ir a una página en el sitio de Microsoft que se enumera los proveedores de hospedaje compatibles con ASP.NET.

![Página en el sitio de Microsoft que se enumera los proveedores de hospedaje](publishing/_static/image13.png)

Obviamente, puede ser difícil ahora saber exactamente qué características de hospedaje es posible que necesite a largo plazo. Aquí se muestran un par de cosas a tener en cuenta:

- Para fines del sitio WebPagesMovies, no tienes que tener un complemento independiente para SQL Server, que a menudo tiene un costo adicional. En el sitio que está usando SQL Server Compact Edition, que son independiente entre sí. Sin embargo, podría necesitar acceso a SQL Server para algún trabajo futuro sitio Web que lo hace. Si cree que es posible que, asegúrese de que posteriormente puede agregar capacidad de SQL Server.
- Compruebe si el proveedor de hospedaje admite el protocolo de publicación de Web Deploy. Puede publicar utilizando el protocolo FTP, pero resulta más conveniente utilizar Web Deploy.

Algunos sitios ofrecen un período de evaluación gratuita. Una evaluación gratuita es una buena manera de intentar realizar la publicación y hospedaje mientras el equipo todavía está experimentar con WebMatrix y ASP.NET Web Pages.

Elija una que le guste. Para este tutorial, hemos seleccionado DiscountASP.NET, porque mientras se estábamos creando el tutorial, dicha compañía tenía una promoción que permite a los usuarios hospedar un sitio gratuito para unos meses.

> [!NOTE]
> La elección de un proveedor de hospedaje para este tutorial no debe interpretarse como una aprobación de dicha compañía sobre cualquier otra. Pero tuvimos que elegir un modo de ilustración y DiscountASP.NET es una de las empresas que es compatible con ASP.NET Web Pages y el protocolo Web Deploy para la publicación.


Normalmente, una vez se haya registrado con el proveedor de hospedaje, la compañía envía un correo electrónico que contiene un nombre de usuario y una contraseña, la dirección URL del servidor de web y así sucesivamente. Si la empresa de hospedaje es compatible con Web Deploy protocolo, puede enviar es un archivo que contiene la configuración de publicación o permiten descargar uno. Un archivo de configuración de publicación, simplifica el proceso para usted.

Cuando haya registrado y esté listo para publicar, haga clic en el **publicar** botón en la cinta de opciones de WebMatrix. El **configuración de publicación** se muestra el cuadro de diálogo.

Si el proveedor de hospedaje envía un archivo de configuración de publicación, haga clic en el **importar la configuración de publicación** vincular e importar el archivo. Si no tiene un archivo de configuración de publicación, rellene los campos con los valores que la empresa de hospedaje le ha enviado por correo electrónico. Esto es lo que la **configuración de publicación** cuadro de diálogo podría ser similar a cuando haya terminado:

![Configuración de publicación introducido en el cuadro de diálogo Configuración de publicación](publishing/_static/image14.png)

Haga clic en **validar conexión**. Si todo es correcto, el cuadro de diálogo notifica **conectado correctamente**, lo que significa que puede comunicarse con el servidor del proveedor de hospedaje.

![Mensaje de correcto si publicar configuración es correcta](publishing/_static/image15.png)

Si hay un problema, WebMatrix hará todo lo posible para saber cuál puede ser el problema:

![Mensaje de error si hay un problema con la configuración de publicación](publishing/_static/image16.png)

Haga clic en **guardar** para guardar la configuración. WebMatrix ofrece para realizar una prueba para asegurarse de que se puede comunicar correctamente con el sitio de hospedaje:

![Mensaje de la oferta para realizar una prueba del proceso de publicación](publishing/_static/image17.png)

Haga clic en **Sí**. WebMatrix carga algunos archivos de ejemplo en el proveedor de hospedaje. Cuando se realiza la prueba de compatibilidad, WebMatrix informa de los resultados:

![Resultados de la prueba de publicación](publishing/_static/image18.png)

Si está listo, continúe y haga clic en **continuar** para iniciar el proceso de publicación reales. WebMatrix averigua qué archivos están en su sitio y ya están en el servidor host (ahora mismo, ninguno) y le ofrece una vista previa del proceso de publicación:

![Vista previa de qué los archivos que se van a cargar el proceso de publicación](publishing/_static/image19.png)

La lista de archivos que se va a publicar incluye las páginas web que ha creado como *Movies.cshtml*. La lista también incluye los archivos para las aplicaciones auxiliares que se han instalado, los archivos para ejecutar SQL Server Compact Edition de la base de datos y así sucesivamente. Como resultado, la inicial publicar proceso puede ser elevado.

Haga clic en **Continuar**. WebMatrix copia los archivos al servidor del proveedor de hospedaje. Cuando termine, los resultados se notifican en la barra de estado:

![Mensaje de la barra de estado cuando el proceso de publicación ha finalizado correctamente](publishing/_static/image20.png)

Para ver el sitio en directo, haga clic en el vínculo en la barra de estado. Agregar *películas* a la dirección URL, y verá la *Movies.cshtml* archivo que ha creado:

![El sitio activo que muestra la página de películas](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Actualizar el sitio en vivo: volver a publicar

Una vez que ha publicado su sitio (para Azure o en una empresa de hospedaje web), hay dos copias del mismo &mdash; la versión en el equipo y la versión en el proveedor de servicios. Probablemente deseará continuar desarrollando el sitio (si nada más, como parte del siguiente conjunto de tutorial). Al hacerlo, tendrá que volver a publicar el sitio para copiar los cambios del equipo para el proveedor de servicios. El proceso de publicación en WebMatrix puede determinar qué archivos han cambiado en el sitio y publicar solo esos archivos.

Para ver cómo funciona la opción volver a publicar, abra el *Movies.cshtml* de sitio, realizar algún cambio pequeño y, a continuación, guarde el archivo. Por ejemplo, cambie el título a `Movies - Updated`.

Haga clic en el **publicar** botón en la cinta de opciones. WebMatrix determina lo que se cambia y muestra una vista previa de los archivos que publicará.

![El cuadro de diálogo 'Publish' mostrando modificada archivos listos para volver a publicar](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> De forma predeterminada, WebMatrix publica la base de datos (*.sdf* archivo) solo la primera vez que publique el sitio. Una vez que el sitio está publicado y personas interactúa con el sitio Web, la base de datos en el sitio activo normalmente tiene datos reales del sitio. Tendrá que tener cuidado de no sobrescribir la base de datos en directo con el *.sdf* archivo que se encuentra en el equipo, que normalmente contiene únicamente datos de prueba. Por eso verá la advertencia **publicación, sobrescribirá cualquier base de datos remoto**, y por qué la casilla de verificación *WebPagesMovies.sdf* está desactivada de forma predeterminada.


Haga clic en **Continuar**. WebMatrix publica los archivos modificados y muestra un mensaje de confirmación, como lo hizo la primera vez que se ha publicado.

Vaya al sitio en vivo (puede hacer clic en el vínculo en el mensaje de confirmación si todavía se muestra) y compruebe que el cambio se ha publicado.

> [!TIP] 
> 
> **Editar archivos de forma remota**
> 
> Como alternativa a cambiar su sitio y, a continuación, volver a publicar, puede editar los archivos remotos directamente en WebMatrix. En este escenario, se abre un archivo que se encuentra en el proveedor de servicios y WebMatrix descarga una copia del mismo para que pueda modificarlas. Cada vez que guarde el archivo, WebMatrix envía los cambios en el sitio.
> 
> Edición remota es una forma sencilla de realizar cambios en su sitio en vivo. Sin embargo, los cambios que realice de esta manera no están sincronizados con los archivos en el sitio local. Para sincronizar los archivos locales con el sitio remoto, puede descargar los archivos remotos. Este proceso funciona como la publicación, excepto en orden inverso.
> 
> No describen más acerca de la edición de remoto remoto descarga instalaciones y de WebMatrix aquí. Son muy útiles si deban de varias personas para que funcione en el mismo sitio en equipos diferentes. Para obtener más información, consulte [publicar y editar un sitio remoto con la versión Beta 2 de WebMatrix](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Recursos adicionales

- [Foro ASP.NET WebMatrix ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), un buen lugar para exponer preguntas y obtener respuestas.

> [!div class="step-by-step"]
> [Anterior](layouts.md)
