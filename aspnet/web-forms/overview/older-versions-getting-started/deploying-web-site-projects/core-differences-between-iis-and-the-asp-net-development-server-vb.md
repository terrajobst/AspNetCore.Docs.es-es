---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Principales diferencias entre IIS y el servidor de desarrollo de ASP.NET (VB) | Microsoft Docs
author: rick-anderson
description: Al probar una aplicación ASP.NET localmente, lo más probable es que usa el servidor Web de desarrollo de ASP.NET. Sin embargo, el sitio Web de producción es más probable pow...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 586bc45a44e773c3097de0959411a2d27098459a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821095"
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Diferencias principales entre IIS y el servidor de desarrollo de ASP.NET (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) o [descargar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> Al probar una aplicación ASP.NET localmente, lo más probable es que usa el servidor Web de desarrollo de ASP.NET. Sin embargo, el sitio Web de producción es más probable es que IIS con tecnología. Hay algunas diferencias entre cómo estos servidores web controlan las solicitudes, y estas diferencias pueden tener consecuencias importantes. Este tutorial exploran algunas de las diferencias más relevante.


## <a name="introduction"></a>Introducción

Cada vez que un usuario visita una aplicación ASP.NET su explorador envía una solicitud al sitio Web. El software de servidor web, que se coordina con el tiempo de ejecución ASP.NET para generar y devolver el contenido para el recurso solicitado recoge dicha solicitud. El[**me** punto **me** para obtener más información **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) son un conjunto de servicios que proporcionan funcionalidad común basado en Internet para Servidores de Windows. IIS es el servidor web que se usan con más frecuencia para las aplicaciones de ASP.NET en entornos de producción; es más probable es que el software de servidor web que utiliza el proveedor de hospedaje web para dar servicio a su aplicación de ASP.NET. IIS también puede usarse como el software de servidor web en el entorno de desarrollo, aunque esto implica la instalación de IIS y configurarlo correctamente.


El servidor de desarrollo de ASP.NET es una opción de servidor web alternativos para el entorno de desarrollo. se incluye con y se integra en Visual Studio. A menos que la aplicación web se configuró para usar IIS, el servidor de desarrollo de ASP.NET se inicia automáticamente y se usa como el servidor web de la primera vez que visite una página web desde dentro de Visual Studio. Las aplicaciones web de demostración se creó en el [ *determinar qué archivos se deben implementarse* ](determining-what-files-need-to-be-deployed-vb.md) tutorial fueron ambas aplicaciones web basado en el sistema de archivos que no se han configurado para usar IIS. Por lo tanto, cuando se visita cualquiera de estos sitios Web desde dentro de Visual Studio se usa el servidor de desarrollo de ASP.NET.

En un mundo perfecto sería idénticos los entornos de desarrollo y producción. Sin embargo, como se explicó en el tutorial anterior no es raro que los entornos de tener diferentes valores de configuración. Uso de software de servidor web diferente en los entornos, agrega otra variable que se debe tener en cuenta al implementar una aplicación. Este tutorial explica las diferencias clave entre IIS y el servidor de desarrollo de ASP.NET. Debido a estas diferencias, hay escenarios donde el código que se ejecuta perfectamente en el entorno de desarrollo produce una excepción o se comporta de manera diferente cuando se ejecuta en producción.

## <a name="security-context-differences"></a>Diferencias de contexto de seguridad

Cada vez que el software de servidor web controla una solicitud entrante asocia esa solicitud con un contexto de seguridad determinado. Esta información de contexto de seguridad se usa el sistema operativo para determinar qué acciones están permitidas por la solicitud. Por ejemplo, una página ASP.NET podría incluir código que registra algún mensaje en un archivo en disco. En orden para esta página ASP.NET se ejecute sin errores, el contexto de seguridad debe tener adecuado de permisos de nivel de sistema de archivos, es decir, permisos de escritura en ese archivo.

El servidor de desarrollo de ASP.NET asocia las solicitudes entrantes con el contexto de seguridad del usuario que ha iniciado sesión actualmente. Si ha iniciado sesión en su escritorio como administrador, las páginas ASP.NET atendidas por el servidor de desarrollo de ASP.NET tendrán los mismos derechos de acceso como administrador. Sin embargo, IIS administra las solicitudes de ASP.NET están asociadas con una cuenta de equipo específico. De forma predeterminada, la cuenta de equipo del servicio de red está usando IIS 6 y 7, aunque el proveedor de hospedaje web ha configurado una cuenta única para cada cliente. Además, el proveedor de hospedaje web probablemente ha dado permisos limitados a esta cuenta de equipo. El resultado neto es que puede que tenga las páginas web que se ejecute sin errores en el entorno de desarrollo, pero generar excepciones relacionadas con la autorización cuando se hospeda en el entorno de producción.

Para mostrar este tipo de error en la acción que he creado una página en el sitio Web de reseñas de libros que crea un archivo en disco que almacena la fecha y hora más recientes que alguien ve los *enseñar a usted mismo ASP.NET 3.5 en 24 horas* revisar. Para poder continuar, abra el `~/Tech/TYASP35.aspx` página y agregue el código siguiente a la `Page_Load` controlador de eventos:

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> El [ `File.WriteAllText` método](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) crea un nuevo archivo si no existe y, a continuación, escribe el contenido especificado en ella. Si el archivo ya existe, se sobrescribe su contenido existente.


A continuación, visite la *enseñar a usted mismo ASP.NET 3.5 en 24 horas* página de revisión del libro en el entorno de desarrollo con el servidor de desarrollo de ASP.NET. Suponiendo que ha iniciado sesión en el equipo con una cuenta que tenga los permisos adecuados para crear y modificar un archivo de texto en la web, directorio raíz de la aplicación la reseña de libro aparece igual que antes, pero cada vez que la página visitó la fecha y hora del usuario  Dirección IP se almacena en el `LastTYASP35Access.txt` archivo. Dirija el explorador para este archivo; debería ver un mensaje similar al que se muestra en la figura 1.


[![El archivo de texto contiene la última fecha y hora que se ha visitado la reseña de libro&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Figura 1**: el archivo de texto contiene la última fecha y hora que se ha visitado la reseña de libro ([haga clic aquí para ver imagen en tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))


Implementar la aplicación web en producción y, a continuación, visite hospedado *enseñar a usted mismo ASP.NET 3.5 en 24 horas* página de revisión del libro. En este momento ya debería ver la página de revisión del libro como normal o el mensaje de error que se muestra en la figura 2. Algunos proveedores de host web concesión permisos de escritura a la cuenta de equipo ASP.NET anónimo en el que caso de la página funcionará sin errores. Si, sin embargo, el proveedor de hospedaje web prohíbe el acceso de escritura para la cuenta anónima una [ `UnauthorizedAccessException` excepción](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) se genera cuando el `TYASP35.aspx` página intenta escribir la fecha y hora actuales a la `LastTYASP35Access.txt` archivo.


[![La cuenta de equipo predeterminado utilizada por IIS no tiene permisos para escribir en el sistema de archivos](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Figura 2**: The Default máquina cuenta usada por IIS Does no tiene permisos para escribir en el sistema de archivos ([haga clic aquí para ver imagen en tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))


La buena noticia es que la mayoría de los proveedores de host de web tiene algún tipo de herramienta de permisos que le permite especificar permisos de sistema de archivos en su sitio Web. Conceda acceso de escritura de cuenta ASP.NET anónimo al directorio raíz y, a continuación, volver a visitar la página de revisión del libro. (Si es necesario, póngase en contacto con su proveedor de hospedaje web para obtener ayuda sobre cómo conceder permisos de escritura a la cuenta ASP.NET predeterminada.) Esta vez la página debería cargarse sin errores y la `LastTYASP35Access.txt` archivo debe crearse correctamente.

La distancia aquí es que, dado que el servidor de desarrollo de ASP.NET funciona bajo un contexto de seguridad diferente de IIS, es posible que las páginas ASP.NET que leer o escriben en el sistema de archivos, leer o escribir en el registro de eventos de Windows, o leer o escribir en el Windows  registro se funcione según lo previsto en desarrollo, pero se generan excepciones cuando se encuentra en producción. Al crear una aplicación web que se implementará en un entorno de hospedaje compartido, no leer ni escribir en el registro de eventos o el registro de Windows. También tome nota de las páginas ASP.NET que leer o escribir en el sistema de archivos, ya que es posible que deba conceder privilegios y escritura en las carpetas correspondientes en el entorno de producción.

## <a name="differences-on-serving-static-content"></a>Diferencias en atender contenido estático

Otra diferencia principal entre IIS y el servidor de desarrollo de ASP.NET es cómo tratar las solicitudes de contenido estático. Todas las solicitudes que lleguen en el servidor de desarrollo de ASP.NET, ya sea para una página ASP.NET, una imagen o un archivo JavaScript, se procesan el tiempo de ejecución de ASP.NET. De forma predeterminada, IIS sólo invoca el tiempo de ejecución ASP.NET cuando llega una solicitud para un recurso ASP.NET, como una página web ASP.NET, un servicio Web y así sucesivamente. Se recuperan las solicitudes de contenido estático: imágenes, archivos CSS, archivos JavaScript, archivos PDF, archivos ZIP y similares: por IIS sin la participación de la ejecución de ASP.NET. (Es posible indicar a IIS para que funcione con el tiempo de ejecución ASP.NET al usar contenido estático; consulte la sección "Realizar la autenticación basada en formularios y autenticación de direcciones URL en archivos estáticos con IIS 7" en este tutorial para obtener más información.)

El tiempo de ejecución ASP.NET realiza una serie de pasos para generar el contenido solicitado, incluido (que identifica el solicitante) de autenticación y autorización (determinar si el solicitante tiene permiso para ver el contenido solicitado). Es una forma de autenticación popular *autenticación basada en formularios*, en que un usuario se identifica con sus credenciales (normalmente un nombre de usuario y contraseña) en cuadros de texto en una página web. Tras validar sus credenciales, el sitio Web almacena un *vale de autenticación* cookie en el explorador del usuario, que se envía con cada solicitud posterior para el sitio Web y es lo que se usa para autenticar al usuario. Además, es posible especificar *autorización de URL* reglas que determinen qué usuarios pueden o no se puede obtener acceso a una carpeta concreta. Muchos sitios Web ASP.NET usan autenticación basada en formularios y la autorización de dirección URL para admitir cuentas de usuario y para definir las partes del sitio que solo se puede acceder a los usuarios autenticados o usuarios que pertenecen a una función determinada.

> [!NOTE]
> Para realizar un examen exhaustivo de ASP. Autenticación basada en formularios de NET, la autorización de URL y otras características relacionadas con la cuenta de usuario, asegúrese de consultar mi [tutoriales de seguridad del sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Considere la posibilidad de un sitio Web que es compatible con cuentas de usuario mediante autorización basada en formularios y tiene una carpeta que, con la autorización de URL, está configurada para permitir que solo los usuarios autenticados. Imagine que esta carpeta contiene las páginas ASP.NET y los archivos PDF y que la intención es que los usuarios autenticados solo pueden ver estos archivos PDF.

¿Qué ocurre si un visitante intenta ver uno de estos archivos PDF escribiendo la dirección URL directamente en la barra de direcciones de su explorador? Para averiguar, vamos a crear una nueva carpeta en el sitio de reseñas de libros, agregar algunos archivos PDF y configurar el sitio para usar autorización de URL para impedir que los usuarios anónimos visitar esta carpeta. Si descarga la aplicación de demostración verá que he creado una carpeta denominada `PrivateDocs` y se agrega un archivo PDF desde mi [tutoriales de seguridad del sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (cómo algoritmos). El `PrivateDocs` carpeta también contiene un `Web.config` archivo que especifica las reglas de autorización de dirección URL para denegar a los usuarios anónimos:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Por último, he configurado la aplicación web para usar la autenticación basada en formularios al actualizar el archivo Web.config en el directorio raíz, reemplace:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

Por:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

Con el servidor de desarrollo de ASP.NET, visite el sitio y escriba la dirección URL directa a uno de los archivos PDF en la barra de direcciones del explorador. Si descargó el sitio Web asociado a este tutorial de que la dirección URL debe ser similar: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Escribir esta dirección URL en la barra de direcciones, hace que el explorador enviar una solicitud al servidor de desarrollo de ASP.NET para el archivo. El servidor de desarrollo de ASP.NET cederá la solicitud para el tiempo de ejecución ASP.NET para su procesamiento. Dado que aún no hemos iniciado sesión así como la `Web.config` en el `PrivateDocs` carpeta está configurada para denegar el acceso anónimo, el tiempo de ejecución ASP.NET nos redirige automáticamente a la página de inicio de sesión, `Login.aspx` (consulte la figura 3). Al redirigir el usuario en el registro en la página, ASP.NET incluye un `ReturnUrl` parámetro de cadena de consulta que indica la página el usuario estaba intentando ver. Después de iniciar sesión correctamente en el usuario puede devolverse a esta página.


[![Los usuarios no autorizados son redirigirá automáticamente a la página de inicio de sesión](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Figura 3**: los usuarios no autorizados son redirigirá automáticamente a la página de inicio de sesión ([haga clic aquí para ver imagen en tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))


Ahora vamos a ver cómo esto se comporta en producción. Implemente la aplicación y escriba la dirección URL directa a uno de los archivos PDF de la `PrivateDocs` carpeta en producción. Esto pide el explorador para enviar una solicitud IIS para el archivo. Dado que se solicita un archivo estático, IIS recupera y devuelve el archivo sin invocar el tiempo de ejecución ASP.NET. Como resultado, no hubo ninguna comprobación de autorización de dirección URL puede realizada; el contenido del PDF supuestamente privado es accesible para cualquier persona que conozca la dirección URL directa al archivo.


[![Los usuarios anónimos pueden descargar los archivos de PDF de Private escribiendo la dirección URL directa al archivo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Figura 4**: los usuarios anónimos pueden descargar la privada PDF archivos al escribir la dirección URL directa al archivo ([haga clic aquí para ver imagen en tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Realizar la autenticación basada en formularios y la autenticación de la dirección URL en archivos estáticos con IIS 7

Hay un par de técnicas que puede usar para proteger contenido estático de los usuarios no autorizados. IIS 7 introdujo el *canalización integrada*, que combina el flujo de trabajo de IIS con el flujo de trabajo de la ejecución de ASP.NET. En pocas palabras, puede indicar a IIS para invocar los módulos de autorización y autenticación de la ejecución de ASP.NET todas las solicitudes entrantes (incluido el contenido estático, como archivos PDF). Póngase en contacto con su proveedor de hospedaje web para obtener información sobre cómo configurar su sitio Web para usar la canalización integrada.

Una vez que IIS se ha configurado para usar la canalización integrada agregue el marcado siguiente a la `Web.config` archivo en el directorio raíz:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Esta marca indica al 7 de IIS para usar los módulos de autenticación y autorización basadas en ASP.NET. Volver a implementar la aplicación y, a continuación, volver a visitar el archivo PDF. Esta vez cuando IIS controla la solicitud proporciona lógica de autenticación y autorización de la ejecución de ASP.NET una oportunidad para inspeccionar la solicitud. Dado que solo los usuarios autenticados están autorizados para ver el contenido en el `PrivateDocs` carpeta, el visitante anónimo se redirige automáticamente a la página de inicio de sesión (consulte la vuelta a la figura 3).

> [!NOTE]
> Si su proveedor de hospedaje web sigue usando IIS 6, a continuación, no puede usar la característica de canalización integrada. Una solución alternativa consiste en colocar sus documentos privados en una carpeta que prohíbe el acceso HTTP (como `App_Data`) y, a continuación, cree una página para atender a estos documentos. Esta página podría denominarse `GetPDF.aspx`y se pasa el nombre del documento PDF a través de un parámetro de cadena de consulta. El `GetPDF.aspx` página comprobaría primero que el usuario tiene permiso para ver el archivo y, si es así, se utilizaría el [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) método para devolver el contenido del archivo PDF solicitado al cliente solicitante. Esta técnica también funcionaría para IIS 7 si no desea habilitar la canalización integrada.


## <a name="summary"></a>Resumen

Aplicaciones Web en un entorno de producción se hospedan mediante software de servidor web de Microsoft IIS. En el entorno de desarrollo, sin embargo, la aplicación puede hospedarse con IIS o el servidor de desarrollo de ASP.NET. Idealmente, debe usarse el mismo software de servidor web en ambos entornos porque utiliza un software diferente, agrega otra variable en la combinación. Sin embargo, la facilidad de uso del servidor de desarrollo de ASP.NET facilita una opción atractiva en el entorno de desarrollo. La buena noticia es que hay sólo unas pocas diferencias fundamentales entre IIS y el servidor de desarrollo de ASP.NET, y si es consciente de estas diferencias puede tomar medidas para ayudar a garantizar que la aplicación funciona y funciona de la misma forma independientemente de la entorno.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Integración de ASP.NET con IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Mediante la autenticación de los foros ASP.NET con todos los tipos de contenido en IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (vídeo)
- [Servidores Web en Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Anterior](common-configuration-differences-between-development-and-production-vb.md)
> [Siguiente](deploying-a-database-vb.md)
