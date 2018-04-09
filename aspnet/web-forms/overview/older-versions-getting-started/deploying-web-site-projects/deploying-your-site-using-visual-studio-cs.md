---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
title: Implementación del sitio con Visual Studio (C#) | Documentos de Microsoft
author: rick-anderson
description: Visual Studio incluye herramientas para implementar un sitio Web. Más información acerca de estas herramientas en este tutorial.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: cde4ee53-a5d0-4937-a54b-67877e8266c3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/deploying-your-site-using-visual-studio-cs
msc.type: authoredcontent
ms.openlocfilehash: f06e2fe1fdfb03b106466a1792f6381495f76096
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="deploying-your-site-using-visual-studio-c"></a>Implementación del sitio con Visual Studio (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_04_CS.zip) o [descarga de PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial04_DeployingViaVS_cs.pdf)

> Visual Studio incluye herramientas para implementar un sitio Web. Más información acerca de estas herramientas en este tutorial.


## <a name="introduction"></a>Introducción

El tutorial anterior examinando cómo implementar una aplicación web ASP.NET simple a un proveedor de hospedaje web. En concreto, el tutorial que se ha explicado cómo usar a un cliente FTP como FileZilla para transferir los archivos necesarios del entorno de desarrollo al entorno de producción. Visual Studio también ofrece herramientas integradas que facilitan la implementación de un proveedor de hospedaje web. Este tutorial examina dos de estas herramientas: la herramienta Copiar sitio Web, donde puede mover los archivos a y desde un servidor web remoto mediante FTP o extensiones de servidor de FrontPage; y la herramienta de publicación, que copia todo el sitio Web en una ubicación especificada.


> [!NOTE]
> Otras herramientas relacionadas con la implementación proporcionadas por Visual Studio incluyen [proyectos de instalación Web](https://msdn.microsoft.com/library/wx3b589t.aspx) y [proyectos de implementación Web](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) Add-In. Proyectos de instalación Web empaquetar el contenido de un sitio Web y la información de configuración en un único archivo MSI. Esta opción es muy útil para los sitios Web que se implementa dentro de una intranet o para las empresas que venden una aplicación web preconfiguradas que los clientes instalen en sus propios servidores web. El complemento de proyectos de implementación Web es que un complemento Visual en Studio que facilita la especificación de configuración de diferencias entre las compilaciones para entornos de desarrollo y entornos de producción. Proyectos de instalación Web no se tratan en esta serie de tutoriales; Proyectos de implementación Web se resumen en la [ *comunes diferencias de configuración entre desarrollo y producción* ](common-configuration-differences-between-development-and-production-cs.md) tutorial.


## <a name="deploying-your-site-using-the-copy-web-site-tool"></a>Implementación del sitio mediante la herramienta Copiar sitio Web

Herramienta Copiar sitio Web de Visual Studio es una funcionalidad similar a un cliente FTP independiente. En pocas palabras, la herramienta Copiar sitio Web le permite conectarse a un sitio web remoto a través de FTP o extensiones de servidor de FrontPage. La interfaz de usuario de Copiar sitio Web es similar a la interfaz de usuario de FileZilla, consta de dos paneles: el panel izquierdo enumera los archivos locales mientras el panel derecho muestran los archivos en el servidor de destino.

> [!NOTE]
> La herramienta Copiar sitio Web solo está disponible para proyectos de sitios Web. Visual Studio ofrece esta herramienta cuando se trabaja con un proyecto de aplicación Web.

¡Eche un vistazo al uso de la herramienta Copiar sitio Web para publicar la aplicación de revisión de libro a la producción. Con esta herramienta con el proyecto BookReviewsWSP solo podemos examinar porque la herramienta Copiar sitio Web solo funciona con los proyectos que usan el modelo de proyecto de sitio Web. Abrir ese proyecto.

Iniciar el proyecto de la herramienta Copiar sitio Web haciendo clic en el icono Copiar sitio Web en el Explorador de soluciones (este icono es dentro de un círculo en la figura 1); como alternativa, puede seleccionar la opción de Copiar sitio Web en el menú del sitio Web. Cualquier enfoque inicia la interfaz de usuario de Copiar sitio Web se muestra en la figura 1; solo el panel izquierdo en la figura 1 se rellena como todavía tenemos para conectarse a un servidor remoto.


[![Interfaz de usuario de la herramienta Copiar sitio Web está dividido en dos paneles](deploying-your-site-using-visual-studio-cs/_static/image2.png)](deploying-your-site-using-visual-studio-cs/_static/image1.png)

**Figura 1**: interfaz de usuario de la herramienta de la copia sitio Web está dividido en dos paneles ([haga clic aquí para ver la imagen a tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image3.png))


Para implementar nuestro sitio es necesario conectar primero con el proveedor de hospedaje web. Haga clic en el botón conectar en la parte superior de la interfaz de usuario de Copiar sitio Web. Esto muestra el cuadro de diálogo Abrir sitio Web se muestra en la figura 2.

Puede conectarse al sitio Web de destino, seleccione una de las cuatro opciones de la izquierda:

- **Sistema de archivos** : seleccione esta opción para implementar el sitio en un carpeta de recurso compartido de red accesible desde su equipo.
- **IIS local** -Utilice esta opción para implementar el sitio en el servidor web IIS instalado en el equipo.
- **Sitio FTP** -conectarse a un sitio web remoto mediante FTP.
- **Sitio remoto** -conectarse a un sitio Web remoto usa extensiones de servidor de FrontPage.

FTP admite la mayoría de los proveedores de host de web, pero menos ofrecen compatibilidad con las extensiones de servidor de FrontPage. Por esta razón, he seleccionado la opción de sitio FTP y, a continuación, especificar la información de conexión tal y como se muestra en la figura 2.


[![Especifique el sitio Web de destino](deploying-your-site-using-visual-studio-cs/_static/image5.png)](deploying-your-site-using-visual-studio-cs/_static/image4.png)

**Figura 2**: especifique el sitio Web de destino ([haga clic aquí para ver la imagen a tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image6.png))


Después de conectarse, la herramienta Copiar sitio Web carga los archivos en el sitio remoto en el panel derecho e indica el estado de cada archivo: nuevos, eliminados, modificados o Unchanged. Puede copiar un archivo desde el sitio local para el sitio remoto, o viceversa una.

Vamos a agregar una nueva página al proyecto BookReviewsWSP y, a continuación, implementarlo para que podemos ver la herramienta Copiar sitio Web en acción. Crear una nueva página ASP.NET en Visual Studio en el directorio raíz denominado `Privacy.aspx`. Tiene la página usa la página maestra `Site.master` y agregue la directiva de privacidad de su sitio para esta página. La figura 3 muestra Visual Studio después de esta página se ha creado.


[![Agregar una nueva página denominada &lt;código&gt;Privacy.aspx&lt;/código&gt; a carpeta de raíz del sitio Web](deploying-your-site-using-visual-studio-cs/_static/image8.png)](deploying-your-site-using-visual-studio-cs/_static/image7.png)

**Figura 3**: agregar una nueva página denominada `Privacy.aspx` a carpeta de raíz del sitio Web ([haga clic aquí para ver la imagen a tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image9.png))


A continuación, volver a la interfaz de usuario de Copiar sitio Web. Como se muestra en la figura 4, el panel izquierdo ahora incluye los nuevos archivos - `Policy.aspx` y `Policy.aspx.cs`. Además, estos archivos se marcan con un icono de flecha y un estado de nuevo, que indica que existen en el sitio local, pero no en el sitio remoto.


[![La herramienta Copiar sitio Web incluye el icono nuevo &lt;código&gt;Privacy.aspx&lt;/código&gt; página en el panel de la izquierda](deploying-your-site-using-visual-studio-cs/_static/image11.png)](deploying-your-site-using-visual-studio-cs/_static/image10.png)

**Figura 4**: la herramienta Copiar sitio Web incluye el icono nuevo `Privacy.aspx` página en el panel de la izquierda ([haga clic aquí para ver la imagen a tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image12.png))


Para implementar los nuevos archivos, selecciónelos y, a continuación, haga clic en el icono de flecha para transferirlas al sitio remoto. Una vez completada la transferencia la `Policy.aspx` y `Policy.aspx.cs` archivos existen en los sitios locales y remotos con el estado Unchanged.

Junto con la lista de archivos de nuevo, la herramienta Copiar sitio Web resalta los archivos que difieren entre los sitios locales y remotos. Para ver esto en acción, vuelva a la `Privacy.aspx` página y agregar algunas palabras más a la directiva de privacidad. Guarde la página y, a continuación, volver a la herramienta Copiar sitio Web. Como se muestra en la figura 5, la `Privacy.aspx` página en el panel izquierdo tiene un estado de Changed que indica que está sincronizado con el sitio remoto.


[![La herramienta Copiar sitio Web indica que el &lt;código&gt;Privacy.aspx&lt;/código&gt; página ha cambiado](deploying-your-site-using-visual-studio-cs/_static/image14.png)](deploying-your-site-using-visual-studio-cs/_static/image13.png)

**Figura 5**: la herramienta Copiar sitio Web indica que el `Privacy.aspx` página ha cambiado ([haga clic aquí para ver la imagen a tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image15.png))


La herramienta Copiar sitio Web también indica si se ha eliminado un archivo desde la última operación de copia. Eliminar el `Privacy.aspx` desde el proyecto local y actualizar la herramienta Copiar sitio Web. El `Privacy.aspx` y `Privacy.aspx.cs` archivos permanezca en la lista en el panel izquierdo, pero tienen un estado eliminado que indica que se han quitado desde la última operación de copia.

## <a name="publishing-a-web-application"></a>Publicar una aplicación Web

Otra manera de implementar la aplicación web desde dentro de Visual Studio es usar la opción de publicación, que es accesible mediante el menú Generar. La opción publicar explícitamente compila la aplicación y, a continuación, copia todos los archivos necesarios en el sitio remoto especificado. Como se verá en breve, la opción de publicación es más obvio que la herramienta Copiar sitio Web. Mientras que la herramienta Copiar sitio Web le permite examinar los archivos en los sitios locales y remotos y permite cargar o descargar archivos individuales según sea necesario, la opción Publicar implementa toda la aplicación web.


Además de copiar todos los archivos necesarios para el sitio remoto especificado, la opción publicar explícitamente compila la aplicación. Dado que los proyectos de aplicación Web necesita ser compilado explícitamente debe proceder como no sorprende que la opción Publicar está disponible para los proyectos de aplicación Web. Lo que puede resultar un poco sorprendente es que la opción de publicación también está disponible para los proyectos de sitio Web. Como se indicó en el [ *determinar qué archivos deben implementarse* ](determining-what-files-need-to-be-deployed-cs.md) tutorial, proyectos de sitios Web se puede compilar explícitamente a través de un proceso que se conoce como *precompilación*. Este tutorial se centra en usar la opción Publicar con proyectos de aplicación Web. un tutorial futuras explorará la precompilación, momento en que se tendrá que volver para mirar utilizando la opción de publicación con proyectos de sitios Web.

> [!NOTE]
> Mientras que la opción de publicación está disponible en Visual Studio para proyectos de sitios Web y proyectos de aplicación Web, Visual Web Developer solo ofrece la opción de publicar proyectos de aplicación Web.


Echemos un vistazo a implementar la aplicación de reseñas de libros con la opción de publicación. Comience abriendo BookReviewsWAP (el proyecto de aplicación Web) en Visual Studio. En el menú publicar elija el proyecto de compilación BookReviewsWAP. Se abrirá un cuadro de diálogo que solicita la ubicación de destino, entre otras opciones de configuración (consulte la figura 6). Mucho al igual que con la herramienta Copiar sitio Web puede especificar una ubicación que apunta a una carpeta local, un sitio Web local en IIS, un sitio Web remoto que admite extensiones de servidor de FrontPage, o una dirección del servidor FTP. Puede elegir si desea reemplazar los archivos en el servidor web remoto con los archivos de implementada o eliminar todo el contenido en el sitio remoto antes de la publicación. También puede especificar si desea copiar:

- Solo los archivos de proyecto necesarios para ejecutar la aplicación, que omite el código fuente innecesarios y archivos relacionados con el proyecto.
- Todos los archivos de proyecto, que incluye los archivos de código fuente y archivos de proyecto de Visual Studio, como el archivo de solución.
- Todos los archivos en la carpeta de proyecto de origen, que copia todos los archivos en la carpeta de proyecto de origen, independientemente de si está incluidos en el proyecto.

También hay una opción para cargar el contenido de la `App_Data` carpeta.


[![Especifique el sitio Web de destino](deploying-your-site-using-visual-studio-cs/_static/image17.png)](deploying-your-site-using-visual-studio-cs/_static/image16.png)

**Figura 6**: especifique el sitio Web de destino ([haga clic aquí para ver la imagen a tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image18.png))


Para la aplicación de revisión de la libreta el sitio remoto contiene los archivos que se implementan al copiar el proyecto BookReviewsWSP a través de la herramienta Copiar sitio Web. Por lo tanto, vamos a tener la opción Publicar iniciar mediante la eliminación de todo el contenido existente. Además, vamos a simplemente copie los archivos necesarios, en lugar de saturar el entorno de producción con los archivos de proyecto y de código de origen innecesarios. Después de especificar estas opciones, haga clic en el botón Publicar. A través de los segundos siguientes Visual Studio implementará los archivos necesarios para el sitio de destino, muestra su progreso en la ventana de salida.

La figura 7 muestra los archivos en el sitio FTP, una vez completada la operación de publicación. Tenga en cuenta que solo las páginas de marcado y los archivos de compatibilidad necesario Server - y de cliente se han cargado.


[![Solo los archivos necesarios se publican en el entorno de producción](deploying-your-site-using-visual-studio-cs/_static/image20.png)](deploying-your-site-using-visual-studio-cs/_static/image19.png)

**Figura 7**: solo los necesarios archivos se publican en el entorno de producción ([haga clic aquí para ver la imagen a tamaño completo](deploying-your-site-using-visual-studio-cs/_static/image21.png))


La opción de publicación es una herramienta matizada menor que la herramienta Copiar sitio Web. Mientras que la herramienta Copiar sitio Web le permite inspeccionar los archivos en los sitios locales y remotos, y ver cómo difieren, la opción publicar no proporciona ninguna interfaz de este tipo. Además, la herramienta Copiar sitio Web permite realizar cambios en un solo uso, cargar o eliminar archivos individuales. La opción de publicación no permite tal control específica; en su lugar, publica el *todo* aplicación. Este comportamiento tiene sus ventajas e inconvenientes. En el lado del signo más, sabrá que al usar la opción de publicación que no olvidar para cargar un archivo importante. Tenga en cuenta lo que ocurre que si ha realizado un pequeño cambio en un sitio Web muy grande: con la opción publicar no se puede actualizar esa página o dos que se ha modificado, pero en su lugar, debe esperar mientras Visual Studio implementa todo el sitio.

No es raro que exista determinados archivos cuyo contenido es diferente entre los entornos de desarrollo y producción. Un ejemplo de clave es el archivo de configuración de la aplicación, `Web.config`. Puesto que la opción Publicar seguirá a ciegas copia los archivos de aplicación web sobrescribe los archivos de configuración personalizada del entorno de producción con la versión en el entorno de desarrollo. El tutorial posterior aún más en este tema explora y ofrece sugerencias para la implementación de una aplicación web cuando existen tales diferencias.

## <a name="summary"></a>Resumen

Implementar un sitio Web implica copiar los archivos necesarios del entorno de desarrollo en el entorno de producción. El tutorial anterior se ha explicado cómo transferir archivos mediante un cliente FTP como FileZilla. Este tutorial examina dos herramientas de implementación de Visual Studio: la herramienta Copiar sitio Web y la opción de publicación. La herramienta Copiar sitio Web es similar a un cliente FTP porque tiene una interfaz de dos paneles enumerar los archivos en el equipo local y un equipo remoto especificado que facilita el proceso cargar o descargar archivos entre los dos equipos. La opción de publicación es una herramienta más obvio que explícitamente se compila el proyecto y, a continuación, implementa toda la aplicación en el destino especificado.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Copiar sitio Web con la herramienta Copiar sitio Web](https://msdn.microsoft.com/library/1cc82atw.aspx)
- [¿Cómo implementar I: un sitio Web mediante la herramienta Copiar sitio Web](../../../videos/how-do-i/how-do-i-deploy-a-web-site-using-the-copy-web-site-tool.md) (vídeo)
- [Cómo: Publicar proyectos de aplicación Web](https://msdn.microsoft.com/library/aa983453.aspx)
- [Cómo: Publicar sitios Web](https://msdn.microsoft.com/library/20yh9f1b.aspx)
- [El programa de instalación y proyectos de implementación de Visual Studio](https://msdn.microsoft.com/library/wx3b589t.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-your-site-using-an-ftp-client-cs.md)
> [Siguiente](common-configuration-differences-between-development-and-production-cs.md)
