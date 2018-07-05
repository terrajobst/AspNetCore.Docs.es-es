---
uid: web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
title: 'Introducción a ASP.NET Web Pages: publicar un sitio mediante el uso de WebMatrix | Microsoft Docs'
author: tfitzmac
description: Este tutorial es la entrega final en el conjunto del tutorial que presenta ASP.NET Web Pages y WebMatrix de Microsoft. Describe cómo publicar el sitio t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/28/2015
ms.topic: article
ms.assetid: 7e85c70e-1a88-4408-8b3d-29611c7713ed
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-aspnet-web-pages-2/publishing
msc.type: authoredcontent
ms.openlocfilehash: c6fa7692527b7aa65e93cd57ed5bd56f42e54bd6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37373492"
---
<a name="introducing-aspnet-web-pages---publishing-a-site-by-using-webmatrix"></a>Introducción a las páginas Web ASP.NET - publicar un sitio mediante el uso de WebMatrix
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial es la entrega final en el conjunto del tutorial que presenta ASP.NET Web Pages y WebMatrix de Microsoft. Describe cómo publicar su sitio en Internet para que otros usuarios puedan trabajar con él. Supone que ha completado la serie a través de [creación de un aspecto coherente para sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=251585).
> 
> Obtendrá información sobre cómo publicar su sitio mediante:
> 
> - Microsoft Azure
> - Empresa de hospedaje Web


## <a name="about-publishing-your-site"></a>Acerca de cómo publicar su sitio

Hasta ahora, ha hecho todo el trabajo en un equipo local, incluidas las pruebas de las páginas. Para ejecutar su<em>.cshtml</em> páginas, ha usado el servidor web que está integrado en WebMatrix, es decir, IIS Express. Pero por supuesto nadie puede ver el sitio, excepto ha creado. Para permitir que otros usuarios a trabajar con su sitio, debe publicarlo en Internet.

A menos que todavía tiene acceso a un servidor web pública, publicación significa que debe tener una cuenta con un *plataforma en la nube* o un *proveedor de hospedaje*. Una plataforma en la nube, como Microsoft Azure, proporciona la infraestructura y a petición para sus aplicaciones. Un proveedor de hospedaje es una empresa que posee los servidores web accesible públicamente y que alquilan, espacio para el sitio. Hospedaje de los planes de ejecución de unos pocos dólares al mes (o incluso libres) para sitios pequeños a muchos cientos de dólares al mes para los sitios Web comerciales de gran volumen.

> [!NOTE]
> Podría tener acceso a un servidor web pública mediante el proveedor de servicios de internet (ISP) que usar para obtener el servicio de internet en casa. Sin embargo, el proveedor de hospedaje debe admitir ASP.NET Web Pages. Muchos ISP no, aunque siempre resulta conveniente comprobar.


En este tutorial, se le ofrece información general de cómo publicar. No es práctico proporcionar los detalles exactos para todo, ya que el proceso es un poco diferente para cada proveedor de hospedaje. Pero obtendrá una buena idea de cómo funciona el proceso.

Este tutorial contiene cuatro secciones:

1. [Configuración de la página predeterminada](#defaultpage)
2. Publicar (elija uno de los siguientes)  
 a. [Publicar su sitio en Microsoft Azure](#azure)  
 b. [Publicar su sitio en una empresa de hospedaje Web](#host)
3. [Actualizar el sitio activo: volver a publicar](#update)

<a id="defaultpage"></a>
## <a name="setting-up-the-default-page"></a>Configuración de la página predeterminada

Cuando un usuario navega a la dirección base para el sitio web, la página predeterminada para el sitio se muestra al usuario. Por ejemplo, cuando Default.htm se establece como la página predeterminada para el sitio en www.contoso.com, a continuación, vaya a <strong>www.contoso.com</strong> es el mismo que navegar a <strong>www.contoso.com/Default.htm</strong>.

Actualmente, el sitio usa **Default.cshtml** como la página predeterminada. Esta página es correcto para la página predeterminada, pero en este tutorial no ha agregado ningún contenido a esa página para que desea mostrar una página en blanco. Abra Default.cshtml y reemplace el contenido con el código siguiente.

[!code-cshtml[Main](publishing/samples/sample1.cshtml)]

Ahora su sitio está listo para su publicación. En primer lugar, verá cómo implementar el sitio en Azure y, a continuación, cómo implementarla en una empresa de hospedaje web. Cualquier opción funciona para su sitio web y solo debe seguir una de las opciones de implementación.

<a id="azure"></a>
## <a name="publishing-your-site-to-microsoft-azure"></a>Publicar su sitio en Microsoft Azure

Este tutorial primero le mostrará cómo implementar su sitio en Microsoft Azure. Iniciando sesión con una cuenta de Microsoft, puede crear hasta 10 sitios gratuitos en Azure. Estos sitios gratuitos proporcionan una manera cómoda para probar los sitios. Siempre se puede eliminar este sitio de ejemplo más adelante para evitar el uso de todos los sitios gratuitos. Puede crear una cuenta de evaluación gratuita en tan solo unos minutos. Para obtener más información, consulte [evaluación gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

En la cinta de opciones de WebMatrix, haga clic en el **publicar** botón.

![Botón 'Publish' en la cinta de opciones de WebMatrix](publishing/_static/image1.png)

El **publicar su sitio** se muestra el cuadro de diálogo. Si no han iniciado sesión su cuenta de Microsoft, el cuadro de diálogo contendrá una **comience a usar Azure** vínculo. Haga clic en este vínculo.

![Publicar el sitio](publishing/_static/image2.png)

Si no han iniciado sesión una cuenta de Microsoft, nuevo tiene la oportunidad de iniciar sesión. Debe iniciar sesión en una cuenta de Microsoft para publicar el sitio en Azure.

![Iniciar sesión](publishing/_static/image3.png)

Después de iniciar sesión su cuenta de Microsoft, el cuadro de diálogo contiene vínculos para crear un nuevo sitio en Azure o conectarse a uno de los sitios existentes en Azure.

![Crear nuevo sitio](publishing/_static/image4.png)

Seleccione **crear un nuevo sitio**.

Si el nombre de su proyecto **WebPagesMovies**, será el nombre predeterminado para el sitio **webpagesmovies.azurewebsites.net**. Este nombre predeterminado es más probable es que no están disponibles, tal y como indica el signo de exclamación rojo.

![nombre del sitio Web predeterminado](publishing/_static/image5.png)

Cambie el nombre de sitio a algo que está disponible y seleccione una ubicación cercana a su ubicación.

![nombre del sitio modificada](publishing/_static/image6.png)

Haga clic en **Aceptar**.

Performss una prueba para determinar si el servidor es compatible con el sitio de WebMatrix.

![prueba de compatibilidad](publishing/_static/image7.png)

Seleccione **continuar**.

Se muestran los resultados de la prueba de compatibilidad.

![resultado de la compatibilidad](publishing/_static/image8.png)

Seleccione **continuar**.

WebMatrix muestra los archivos y bases de datos que se va a publicar el sitio. Puesto que es la primera vez que se va a publicar el sitio, se muestran todos los archivos. Puede desactivar un archivo que no está listo para publicarse. En las publicaciones posteriores, se mostrará solo los archivos que han cambiado. Consulte [actualizar el sitio activo: volver a publicar](#update).

![vista previa de publicación](publishing/_static/image9.png)

Seleccione **continuar**.

Después de que el sitio se ha implementado en Azure, se muestra un mensaje que indica que la implementación haya finalizado.

![publicación completa](publishing/_static/image10.png)

El sitio y la base de datos se han publicado en Azure y ahora están disponibles al público. Haga clic en el vínculo en el mensaje que indica la publicación se ha completado, y ahora verá su sitio implementado. Cualquier persona con acceso a Internet puede agregar o modificar registros en la base de datos.

![](publishing/_static/image11.png)

<a id="host"></a>
## <a name="publishing-your-site-to-a-web-hosting-company"></a>Publicar su sitio en una empresa de hospedaje Web

Si decide no publicar en Azure, en su lugar, puede publicar el sitio en una empresa de hospedaje web.

Haga clic en el **busca hospedaje web** vínculo.

![Botón 'Encontró el hospedaje web' en el cuadro de diálogo Configuración de publicación](publishing/_static/image12.png)

Ir a una página en el sitio de Microsoft que se enumera los proveedores de hospedaje compatibles con ASP.NET.

![Página en el sitio de Microsoft que se enumera los proveedores de hospedaje](publishing/_static/image13.png)

Obviamente, puede resultar difícil saber exactamente qué características de hospedaje es posible que necesite a largo plazo de ahora. Existen un par de cosas a tener en cuenta:

- Para fines del sitio WebPagesMovies, no tienes que tener un complemento independiente para SQL Server, que a menudo tiene un costo adicional. En el sitio, que está usando SQL Server Compact Edition, que son independiente entre sí. Sin embargo, podría necesitar acceso a SQL Server para algún trabajo futuro del sitio Web que realice. Si piensa que podría, asegúrese de que puede agregar funcionalidad de SQL Server más adelante.
- Compruebe si el proveedor de hospedaje admite el protocolo de publicación de Web Deploy. Puede publicar mediante el protocolo FTP, pero resulta más conveniente usar Web Deploy.

Algunos sitios ofrecen un período de evaluación gratuita. Una evaluación gratuita es una buena manera para intentar realizar la publicación y hospedaje al tiempo que se está experimentando todavía con WebMatrix y ASP.NET Web Pages.

Elija una que le guste. Para este tutorial, hemos seleccionado DiscountASP.NET, porque mientras se estaba creando el tutorial, esa compañía tenía una promoción que permite a los usuarios hospede un sitio gratuito durante unos meses.

> [!NOTE]
> Nuestra opción de un proveedor de hospedaje para este tutorial no debe interpretarse como un patrocinio de dicha empresa a través de cualquier otro. Pero tuvimos que elegir una fines ilustrativos y DiscountASP.NET es una de las empresas que es compatible con ASP.NET Web Pages y el protocolo Web Deploy para la publicación.


Normalmente, una vez que se ha registrado con el proveedor de hospedaje, la compañía envía un correo electrónico que contiene un nombre de usuario y una contraseña, la dirección URL del servidor de web y así sucesivamente. Si la empresa de hospedaje admite protocolos Web Deploy, podría enviar es un archivo que contiene la configuración de publicación o le permiten descargar uno. Un archivo de configuración de publicación, simplifica el proceso para usted.

Cuando se ha registrado y esté preparado para publicar, haga clic en el **publicar** botón en la cinta de opciones de WebMatrix. El **configuración de publicación** se muestra el cuadro de diálogo.

Si el proveedor de hospedaje envía un archivo de configuración de publicación, haga clic en el **importar configuración de publicación** vincular e importe el archivo. Si no tiene un archivo de configuración de publicación, rellene los campos con los valores que la empresa de hospedaje le ha enviado por correo electrónico. Esto es lo que el **configuración de publicación** cuadro de diálogo podría parecerse a cuando haya terminado:

![Configuración de publicación se rellena en el cuadro de diálogo Configuración de publicación](publishing/_static/image14.png)

Haga clic en **validar conexión**. Si todo está correcto, el cuadro de diálogo notifica **se conectó correctamente**, lo que significa que puede comunicarse con el servidor del proveedor de hospedaje.

![Mensaje de éxito si publicar configuración es correcta](publishing/_static/image15.png)

Si hay un problema, WebMatrix lo hace todo lo posible para saber cuál es el problema:

![Mensaje de error si hay un problema con la configuración de publicación](publishing/_static/image16.png)

Haga clic en **guardar** para guardar la configuración. WebMatrix ofrece para realizar una prueba para asegurarse de que se puede comunicar correctamente con el sitio de hospedaje:

![Mensaje de oferta para realizar una prueba del proceso de publicación](publishing/_static/image17.png)

Haga clic en **Sí**. WebMatrix carga algunos archivos de ejemplo en el proveedor de hospedaje. Cuando se realiza la prueba de compatibilidad, WebMatrix informa de los resultados:

![Resultados de la prueba de publicación](publishing/_static/image18.png)

Si está listo, continúe y haga clic en **continuar** para iniciar el proceso de publicación con datos reales. WebMatrix averigua qué archivos están en su sitio y ya están en el servidor host (ahora mismo, ninguno) y le ofrece una vista previa del proceso de publicación:

![Vista previa de lo que los archivos que se cargará el proceso de publicación](publishing/_static/image19.png)

La lista de archivos para publicar incluye las páginas web que ha creado como *Movies.cshtml*. La lista también incluye los archivos para las aplicaciones auxiliares que haya instalado, los archivos para ejecutar SQL Server Compact Edition para la base de datos y así sucesivamente. Como resultado, el proceso de publicación inicial puede ser considerable.

Haga clic en **Continuar**. WebMatrix copia los archivos al servidor del proveedor de hospedaje. Cuando haya terminado, los resultados se notifican en la barra de estado:

![Mensaje de la barra de estado cuando el proceso de publicación ha finalizado correctamente](publishing/_static/image20.png)

Para ver el sitio activo, haga clic en el vínculo en la barra de estado. Agregar *películas* a la dirección URL, y verá el *Movies.cshtml* archivo que ha creado:

![El sitio activo que muestra la página de películas](publishing/_static/image21.png)

<a id="update"></a>
## <a name="updating-the-live-site-republishing"></a>Actualizar el sitio activo: volver a publicar

Una vez que haya publicado su sitio (a Azure o una empresa de hospedaje web), hay dos copias del mismo &mdash; la versión en el equipo y la versión en el proveedor de servicios. Probablemente deseará continuar desarrollando el sitio (si nada más, como parte del siguiente conjunto de tutoriales). Al hacerlo, tendrá que volver a publicar su sitio con el fin de copiar los cambios desde el equipo en el proveedor de servicios. El proceso de publicación en WebMatrix puede determinar qué archivos han cambiado en el sitio y publicar esos archivos.

Para ver cómo funciona la republicación, abra el *Movies.cshtml* de sitio, realice algún pequeño cambio y, a continuación, guarde el archivo. Por ejemplo, cambie el título a `Movies - Updated`.

Haga clic en el **publicar** botón en la cinta de opciones. WebMatrix determina lo que se cambia y muestra una vista previa de los archivos que va a publicar.

![El cuadro de diálogo "Publicar" que muestra cambiado archivos listos para volver a publicar](publishing/_static/image22.png)

> [!IMPORTANT] 
> 
> De forma predeterminada, WebMatrix publica la base de datos (*.sdf* archivo) solo la primera vez que publique el sitio. Una vez que se publica el sitio y las personas interactúan con el sitio Web, la base de datos en el sitio activo tiene normalmente los datos reales de la carpeta del sitio. Hay que tener cuidado de no sobrescribir la base de datos en directo con el *.sdf* archivo en su equipo, que normalmente contiene solo los datos de prueba. Por esta razón aparece la advertencia **publicación sobrescribirá las bases de datos remotos**, y por qué la casilla de verificación *WebPagesMovies.sdf* está desactivada de forma predeterminada.


Haga clic en **Continuar**. WebMatrix publica los archivos modificados y muestra un mensaje de confirmación, como lo hizo la primera vez que publicó.

Vaya al sitio activo (puede hacer clic en el vínculo en el mensaje de éxito si se sigue mostrando) y compruebe que el cambio se ha publicado.

> [!TIP] 
> 
> **Edición de archivos de forma remota**
> 
> Como alternativa a cambiar su sitio y, a continuación, volver a publicar, puede editar los archivos remotos directamente en WebMatrix. En este escenario, abra un archivo que se encuentra en el proveedor de servicios y WebMatrix descarga una copia del mismo para que pueda editar. Cada vez que guarde el archivo, WebMatrix envía los cambios en el sitio.
> 
> Edición remota es una manera fácil de realizar cambios en el sitio activo. Sin embargo, los cambios que realice esta forma no están sincronizados con los archivos en el sitio local. Para sincronizar los archivos locales con el sitio remoto, puede descargar los archivos remotos. Este proceso funciona de forma similar, la publicación, excepto en orden inverso.
> 
> No se describirá más acerca de la edición remota y remoto descarga instalaciones de WebMatrix aquí. Son muy útiles si tienen varias personas trabajando en el mismo sitio en equipos diferentes. Para obtener más información, consulte [publicar y modificar un sitio remoto con la versión Beta 2 de WebMatrix](https://go.microsoft.com/fwlink/?LinkId=251591).


## <a name="additional-resources"></a>Recursos adicionales

- [Foro de ASP.NET WebMatrix ASP.NET Web Pages](https://forums.asp.net/1224.aspx/1?WebMatrix+and+ASP+NET+Web+Pages), un lugar excelente para publicar preguntas y obtenga respuestas.

> [!div class="step-by-step"]
> [Anterior](layouts.md)
