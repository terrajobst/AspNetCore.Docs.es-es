---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
title: Principales diferencias entre IIS y el servidor de desarrollo de ASP.NET (VB) | Documentos de Microsoft
author: rick-anderson
description: Cuando se prueba una aplicación de ASP.NET localmente, lo más probable es que usa el servidor Web de desarrollo de ASP.NET. Sin embargo, el sitio Web de producción es más probable pow...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 090e9205-52f3-4d72-ae31-44775b8b8421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-vb
msc.type: authoredcontent
ms.openlocfilehash: 47b1959f9b92d161da0476b274c8154333ad80dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="core-differences-between-iis-and-the-aspnet-development-server-vb"></a>Diferencias principales entre IIS y el servidor de desarrollo de ASP.NET (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/4/5/F/45F815EC-8B0E-46D3-9FB8-2DC015CCA306/ASPNET_Hosting_Tutorial_06_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial06_WebServerDiff_vb.pdf)

> Cuando se prueba una aplicación de ASP.NET localmente, lo más probable es que usa el servidor Web de desarrollo de ASP.NET. Sin embargo, el sitio Web de producción es más probable es que IIS apagada. Hay algunas diferencias entre la forma en que estos servidores web controlan las solicitudes, y estas diferencias pueden tener consecuencias importantes. Este tutorial explora algunas de las diferencias más relevante.


## <a name="introduction"></a>Introducción

Cada vez que un usuario visita una aplicación ASP.NET su explorador envía una solicitud al sitio Web. Dicha solicitud se recoge el software de servidor web, que se coordina con el tiempo de ejecución ASP.NET para generar y devolver el contenido para el recurso solicitado. El[**I** nternet **I** nformación **S** ervices (IIS)](http://en.wikipedia.org/wiki/Internet_Information_Services) son un conjunto de servicios que proporcionan funcionalidad común basado en Internet para Servidores de Windows. IIS es el servidor web que se usan normalmente para las aplicaciones ASP.NET en entornos de producción; es muy probable que el software de servidor web que utiliza el proveedor de hospedaje web para dar servicio a la aplicación de ASP.NET. IIS también puede usarse como el software de servidor web en el entorno de desarrollo, aunque esto conlleva la instalación de IIS y configurarlo correctamente.


El servidor de desarrollo de ASP.NET es una opción de servidor web alternativo para el entorno de desarrollo; se suministra con y se integra en Visual Studio. A menos que la aplicación web se ha configurado para usar IIS, el servidor de desarrollo de ASP.NET se inicia automáticamente y se usa como el servidor web la primera vez que visite una página web desde dentro de Visual Studio. Las aplicaciones web de demostración hemos creado en el [ *determinar qué archivos deben implementarse* ](determining-what-files-need-to-be-deployed-vb.md) tutorial fueron ambas aplicaciones web basados en el sistema de archivos que no se han configurado para usar IIS. Por lo tanto, al visitar cualquiera de estos sitios Web desde dentro de Visual Studio se utiliza el servidor de desarrollo de ASP.NET.

En un mundo perfecto sería idénticos los entornos de desarrollo y producción. Sin embargo, como se explicó en el tutorial anterior no es común para los entornos que tienen una configuración que no son iguales. Uso de software de servidor web diferente en los entornos, agrega otra variable que se debe tener en cuenta al implementar una aplicación. Este tutorial trata las diferencias clave entre IIS y el servidor de desarrollo de ASP.NET. Debido a estas diferencias existen escenarios donde el código que se ejecuta completamente en el entorno de desarrollo produce una excepción o se comporta de manera diferente cuando se ejecuta en producción.

## <a name="security-context-differences"></a>Diferencias de contexto de seguridad

Cada vez que el software del servidor web controla una solicitud entrante asocia dicha solicitud con un contexto de seguridad determinada. Esta información de contexto de seguridad se utiliza el sistema operativo para determinar qué acciones están permitidas por la solicitud. Por ejemplo, una página ASP.NET puede incluir código que registra algunos mensajes en un archivo en disco. De esta página de ASP.NET se ejecute sin errores, el contexto de seguridad debe tener la correspondiente permisos de nivel de sistema de archivos, es decir, permisos de escritura en ese archivo.

El servidor de desarrollo de ASP.NET asocia las solicitudes entrantes con el contexto de seguridad del usuario que ha iniciado sesión actualmente. Si ha iniciado sesión el escritorio como administrador, las páginas ASP.NET atendidas por el servidor de desarrollo de ASP.NET tendrá los mismos derechos de acceso como administrador. Sin embargo, las solicitudes ASP.NET administradas por IIS están asociadas a una cuenta de equipo específico. De forma predeterminada, la cuenta de equipo de servicio de red se usa por IIS versiones 6 y 7, aunque el proveedor de hospedaje web ha configurado una cuenta única para cada cliente. Es más, el proveedor de hospedaje web probablemente ha dado permisos limitados para esta cuenta de equipo. El resultado neto es que pueden tener páginas web que se ejecute sin errores en el entorno de desarrollo, pero generar excepciones relacionadas con la autorización cuando se hospeda en el entorno de producción.

Para mostrar este tipo de error en la acción he creado una página en el sitio Web de reseñas de libros que crea un archivo en disco que almacena la fecha y hora más recientes que alguien ve los *enseñar a usted mismo ASP.NET 3.5 en 24 horas* revisar. Para poder continuar, abra el `~/Tech/TYASP35.aspx` página y agregue el código siguiente a la `Page_Load` controlador de eventos:

[!code-vb[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample1.vb)]

> [!NOTE]
> El [ `File.WriteAllText` método](https://msdn.microsoft.com/library/system.io.file.writealltext.aspx) crea un nuevo archivo si no existe y, a continuación, escribe el contenido especificado. Si el archivo ya existe, se sobrescribe su contenido existente.


A continuación, visite la *enseñar a usted mismo ASP.NET 3.5 en 24 horas* página de revisión de libro en el entorno de desarrollo con el servidor de desarrollo de ASP.NET. Si damos por hecho que ha iniciado sesión en el equipo con una cuenta que tenga los permisos adecuados para crear y modificar un archivo de texto en la web directorio raíz de la aplicación la revisión del libro parece igual que antes, pero cada vez que la página visita la fecha y hora del usuario  Dirección IP se almacena en el `LastTYASP35Access.txt` archivo. Diríjase a este archivo; debería ver un mensaje similar al que se muestra en la figura 1.


[![El archivo de texto contiene la última fecha y hora se ha visitado la revisión del libro&lt;](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image2.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image1.png)

**Figura 1**: el archivo de texto contiene la última fecha y hora se ha visitado la revisión del libro ([haga clic aquí para ver la imagen a tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image3.png))


Implementar la aplicación web en producción y, a continuación, visite hospedado *enseñar a usted mismo ASP.NET 3.5 en 24 horas* página de revisión del libro. En este momento ya sea debería ver la página de revisión del libro como normal o el mensaje de error que se muestra en la figura 2. Algunos proveedores de host web conceda permisos de escritura a la cuenta anónima de máquina ASP.NET, en el que caso la página funcionará sin errores. Si, sin embargo, el proveedor de hospedaje web prohíbe el acceso de escritura para la cuenta anónima un [ `UnauthorizedAccessException` excepción](https://msdn.microsoft.com/library/system.unauthorizedaccessexception.aspx) se inicia cuando el `TYASP35.aspx` página intenta escribir la fecha y hora actuales a la `LastTYASP35Access.txt` archivo.


[![La cuenta de equipo predeterminado utilizada por IIS no tiene permisos para escribir en el sistema de archivos](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image5.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image4.png)

**Figura 2**: el valor predeterminado máquina cuenta utilizada por IIS Does no tiene permisos para escribir en el sistema de archivos ([haga clic aquí para ver la imagen a tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image6.png))


Las buenas noticias son que la mayoría de los proveedores de host de web tiene algún tipo de herramienta de permisos que le permite especificar permisos de sistema de archivos en su sitio Web. Conceder el acceso anónimo de escritura de cuenta ASP.NET en el directorio raíz y, a continuación, volver a visitar la página de revisión del libro. (Si es necesario, póngase en contacto con su proveedor de hospedaje web para obtener ayuda acerca de cómo conceder permisos de escritura para la cuenta ASP.NET predeterminada.) Este tiempo debe cargar la página sin errores y la `LastTYASP35Access.txt` archivo debe crearse correctamente.

La distancia aquí es que, dado que el servidor de desarrollo de ASP.NET funciona en un contexto de seguridad diferente que IIS, es posible que las páginas ASP.NET que leen o escriben en el sistema de archivos, leer o escribir en el registro de eventos de Windows, o leer o escribir en las ventanas  Registro funcionan según lo previsto en el desarrollo, pero generan excepciones cuando se encuentra en producción. Cuando cree una aplicación web que se implementará en un entorno de hospedaje compartido, no leen o escriben en el registro de eventos o el registro de Windows. Además, tome nota de las páginas ASP.NET que leer o escribir en el sistema de archivos ya que puede ser necesario conceder privilegios y escritura en las carpetas correspondientes en el entorno de producción.

## <a name="differences-on-serving-static-content"></a>Diferencias en servir contenido estático

Otra diferencia principal entre IIS y el servidor de desarrollo de ASP.NET es cómo controlan las solicitudes de contenido estático. Todas las solicitudes que lleguen en el servidor de desarrollo de ASP.NET, ya sea para una página ASP.NET, una imagen o un archivo de JavaScript, se procesan en tiempo de ejecución de ASP.NET. De forma predeterminada, IIS sólo, invoca el tiempo de ejecución ASP.NET cuando llega una solicitud para un recurso ASP.NET, como una página web ASP.NET, un servicio Web y así sucesivamente. Las solicitudes de contenido estático: imágenes, archivos CSS, archivos JavaScript, archivos PDF, archivos ZIP y similares - recuperadas por IIS sin la participación de la ejecución de ASP.NET. (Es posible indicar a IIS para que funcione con el tiempo de ejecución ASP.NET cuando servir contenido estático, consulte la sección "Realizar la autenticación mediante formularios y autenticación de direcciones URL en archivos estáticos con IIS 7" en este tutorial para obtener más información.)

El tiempo de ejecución ASP.NET realiza una serie de pasos para generar el contenido solicitado, incluido (identificar al solicitante) de autenticación y autorización (determinar si el solicitante tiene permiso para ver el contenido solicitado). Es una forma de autenticación popular *autenticación basada en formularios*, en que se identifica un usuario, escriba sus credenciales - normalmente un nombre de usuario y contraseña - en cuadros de texto en una página web. Tras validar sus credenciales, se almacena el sitio Web de un *vale de autenticación* cookie en el explorador del usuario, que se envía con cada solicitud posterior para el sitio Web y lo que se usa para autenticar al usuario. Además, es posible especificar *autorizaciones de direcciones URL* reglas que dictan lo que los usuarios pueden o no se puede obtener acceso a una carpeta concreta. Muchos sitios Web ASP.NET usa autenticación basada en formularios y la autorización de dirección URL para admitir las cuentas de usuario y para definir las partes del sitio que solo están disponibles para los usuarios autenticados o usuarios que pertenecen a una función determinada.

> [!NOTE]
> Para realizar un examen exhaustivo de ASP. Autenticación basada en formularios de NET, autorización URL y otras características relacionadas con la cuenta de usuario, asegúrese de desproteger mi [tutoriales de seguridad del sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


Considere la posibilidad de un sitio Web que es compatible con cuentas de usuario mediante autorización basada en formularios y dispone de una carpeta que, con autorización de URL, está configurada para permitir que solo los usuarios autenticados. Imagine que esta carpeta contiene las páginas ASP.NET y archivos PDF y que la intención es que los usuarios autenticados solo pueden ver estos archivos PDF.

¿Qué ocurre si un visitante intenta ver uno de estos archivos PDF, escriba la dirección URL directamente en la barra de direcciones de su explorador? Para obtener información, vamos a crear una nueva carpeta en el sitio de reseñas de libros, agregar algunos archivos PDF y configurar el sitio para usar autorización de dirección URL para impedir que los usuarios anónimos visitar esta carpeta. Si descarga la aplicación de demostración verá que he creado una carpeta denominada `PrivateDocs` y se agrega un archivo PDF desde mi [tutoriales de seguridad del sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md) (cómo adecuado). El `PrivateDocs` carpeta también contiene un `Web.config` archivo que especifica las reglas de autorización de dirección URL para denegar a los usuarios anónimos:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample2.xml)]

Por último, configura la aplicación web para utilizar la autenticación basada en formularios al actualizar el archivo Web.config en el directorio raíz, reemplazando:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample3.xml)]

Por:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample4.xml)]

Use el servidor de desarrollo de ASP.NET, visite el sitio y escriba la dirección URL directa a uno de los archivos PDF en la barra de direcciones de su explorador. Si ha descargado del sitio Web asociado a este tutorial que la dirección URL debe ser similar: `http://localhost:portNumber/PrivateDocs/aspnet_tutorial01_Basics_vb.pdf`

Escribir esta dirección URL en la barra de direcciones hace que el explorador enviar una solicitud al servidor de desarrollo de ASP.NET para el archivo. El servidor de desarrollo de ASP.NET cederá la solicitud para el tiempo de ejecución ASP.NET para su procesamiento. Porque nos hemos no han iniciado sesión y que la `Web.config` en el `PrivateDocs` carpeta está configurada para denegar el acceso anónimo, el tiempo de ejecución ASP.NET nos redirige automáticamente a la página de inicio de sesión, `Login.aspx` (consulte la figura 3). Cuando se redirige al usuario a la página de inicio de sesión, ASP.NET incluye una `ReturnUrl` parámetro de cadena de consulta que indique la página el usuario estaba intentando ver. Después de iniciar sesión correctamente en el usuario puede devolverse a esta página.


[![Los usuarios no autorizados son redirigirá automáticamente a la página de inicio de sesión](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image8.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image7.png)

**Figura 3**: los usuarios no autorizados son redirigirá automáticamente a la página de inicio de sesión ([haga clic aquí para ver la imagen a tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image9.png))


Ahora veamos cómo esto se comporta en producción. Implementar la aplicación y escriba la dirección URL directa a uno de los archivos PDF de la `PrivateDocs` carpeta en producción. Esto le pide su explorador para que envíe una solicitud de IIS para el archivo. Dado que se solicita un archivo estático, IIS recupera y devuelve el archivo sin invocar el tiempo de ejecución ASP.NET. Como resultado, no había ninguna comprobación de autorización de dirección URL que realizar; el contenido del documento PDF supuestamente privado es accesible para cualquier usuario que conozca la dirección URL directa al archivo.


[![Los usuarios anónimos pueden descargar los archivos de PDF de Private escribiendo la dirección URL directa al archivo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image11.png)](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image10.png)

**Figura 4**: los usuarios anónimos pueden descargar la privada PDF archivos al escribir la dirección URL directa al archivo ([haga clic aquí para ver la imagen a tamaño completo](core-differences-between-iis-and-the-asp-net-development-server-vb/_static/image12.png))


### <a name="performing-forms-based-authentication-and-url-authentication-on-static-files-with-iis-7"></a>Realizar la autenticación basada en formularios y autenticación de direcciones URL en archivos estáticos con IIS 7

Hay un par de técnicas que puede usar para proteger el contenido estático de usuarios no autorizados. IIS 7 introdujo el *canalización integrada*, que combina el flujo de trabajo de IIS con el flujo de trabajo de runtime ASP.NET. En pocas palabras, puede indicar a IIS para invocar los módulos de autorización y autenticación del runtime ASP.NET todas las solicitudes entrantes (incluido el contenido estático, como archivos PDF). Póngase en contacto con su proveedor de hospedaje web para obtener información sobre cómo configurar su sitio Web para utilizar la canalización integrada.

Una vez que IIS se ha configurado para usar la canalización integrada agregue el siguiente marcado para la `Web.config` archivo en el directorio raíz:

[!code-xml[Main](core-differences-between-iis-and-the-asp-net-development-server-vb/samples/sample5.xml)]

Esta marca indica a 7 de IIS para utilizar los módulos de autenticación y autorización basada en ASP.NET. Volver a implementar la aplicación y, a continuación, volver a visitar el archivo PDF. Este tiempo cuando IIS administra la solicitud ofrece lógica de autenticación y autorización del runtime ASP.NET una oportunidad para inspeccionar la solicitud. Dado que solo los usuarios autenticados están autorizados para ver el contenido de la `PrivateDocs` carpeta, el visitante anónimo se redirige automáticamente a la página de inicio de sesión (hacen referencia a la figura 3).

> [!NOTE]
> Si su proveedor de hospedaje web sigue utilizando IIS 6 no puede usar la característica de canalización integrada. Una solución es colocar los documentos privados en una carpeta que prohíbe el acceso HTTP (como `App_Data`) y, a continuación, cree una página para dar servicio a estos documentos. Esta página podría denominarse `GetPDF.aspx`y se pasa el nombre del documento PDF a través de un parámetro de cadena de consulta. El `GetPDF.aspx` página pueda comprobar primero que el usuario tiene permiso para ver el archivo y, en ese caso, utilizaría el [ `Response.WriteFile(filePath)` ](https://msdn.microsoft.com/library/system.web.httpresponse.writefile.aspx) método para devolver el contenido del archivo PDF solicitado al cliente solicitante. Esta técnica también funcionaría para IIS 7 si no desea habilitar la canalización integrada.


## <a name="summary"></a>Resumen

Las aplicaciones Web en un entorno de producción se hospedan mediante software de servidor web de Microsoft IIS. En el entorno de desarrollo, sin embargo, la aplicación puede estar hospedada mediante IIS o el servidor de desarrollo de ASP.NET. Idealmente, debe usarse el mismo software de servidor web en ambos entornos porque utilizando software diferente agrega otra variable en la combinación. Sin embargo, la facilidad de uso del servidor de desarrollo de ASP.NET lo convierte en una opción apropiada en el entorno de desarrollo. Las buenas noticias son que hay solo unas pocas diferencias fundamentales entre IIS y el servidor de desarrollo de ASP.NET, y si está al corriente de estas diferencias puede tomar medidas para ayudar a garantizar que la aplicación funciona y funciona de la misma forma independientemente de la entorno.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Integración de ASP.NET con IIS 7.0](https://www.iis.net/learn/application-frameworks/building-and-running-aspnet-applications/aspnet-integration-with-iis)
- [Mediante la autenticación de foros ASP.NET con todos los tipos de contenido en IIS 7](https://blogs.iis.net/bills/archive/2007/05/19/using-asp-net-forms-authentication-with-all-types-of-content-with-iis7-video.aspx) (vídeo)
- [Servidores Web en Visual Web Developer](https://msdn.microsoft.com/library/58wxa9w5.aspx)

> [!div class="step-by-step"]
> [Anterior](common-configuration-differences-between-development-and-production-vb.md)
> [Siguiente](deploying-a-database-vb.md)
