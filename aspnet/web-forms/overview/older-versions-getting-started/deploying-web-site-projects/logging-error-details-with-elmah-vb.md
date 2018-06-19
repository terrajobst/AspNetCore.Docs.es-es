---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: Detalles del Error de registro con ELMAH (VB) | Documentos de Microsoft
author: rick-anderson
description: Error de registro de módulos y controladores (ELMAH) ofrece otra alternativa a registro de errores de tiempo de ejecución en un entorno de producción. ELMAH es un error de código abierto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: 584791a944c9e8eb0113da68719292f448573980
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
ms.locfileid: "30891313"
---
<a name="logging-error-details-with-elmah-vb"></a>Detalles de Error de registro con ELMAH (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip) o [descarga de PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> Error de registro de módulos y controladores (ELMAH) ofrece otra alternativa a registro de errores de tiempo de ejecución en un entorno de producción. ELMAH es una biblioteca de registro de errores de código abierto que incluye características como el filtrado de error y la capacidad para ver el registro de errores de una página web, como una fuente RSS o descargar como un archivo delimitado por comas. Este tutorial le guía a través de descargar y configurar ELMAH.


## <a name="introduction"></a>Introducción

El [tutorial anterior](logging-error-details-with-asp-net-health-monitoring-vb.md) examinar ASP. Estado de NET del sistema, lo que ofrece un fuera de la biblioteca de cuadro para grabar una amplia gama de eventos Web de supervisión. Muchos desarrolladores utilizan para iniciar sesión y enviar por correo electrónico los detalles de las excepciones no controladas de supervisión de estado. Sin embargo, hay algunos puntos débiles con este sistema. Primero y más importante es la falta de cualquier tipo de interfaz de usuario para ver información acerca de los eventos registrados. Si desea ver un resumen de los 10 últimos errores o ver los detalles de un error que produjo la última semana, debe escribir en memoria ya sea a través de la base de datos, busque en el Bandeja de entrada de correo electrónico o compilar una página web que muestra información de la `aspnet_WebEvent_Events` tabla.

Otro punto débil se centra en la complejidad del seguimiento de estado. Debido a la supervisión del estado puede utilizarse para registrar una gran cantidad de eventos diferentes y porque no hay una variedad de opciones para indicar cómo y cuándo se registran eventos, al configurar correctamente el sistema de supervisión de estado puede ser una tarea tan laboriosa. Por último, hay problemas de compatibilidad. Dado que la supervisión de estado en primer lugar se agregó a la versión 2.0 de .NET Framework, no está disponible para las aplicaciones web anteriores compiladas con la versión ASP.NET 1.x. Además, el `SqlWebEventProvider` (clase), que se usa en el tutorial anterior detalles del error de registros a una base de datos, solo funciona con bases de datos de Microsoft SQL Server. Debe crear una clase de proveedor de registro personalizado deba registrar los errores en un almacén de datos alternativo, como un archivo XML o una base de datos de Oracle.

Una alternativa para el sistema de supervisión de estado es Error de registro de módulos y controladores (ELMAH), un sistema de registro de errores gratuita y de código abierto creado por [Atif Aziz](http://www.raboof.com/). La diferencia más notable entre los dos sistemas es la capacidad de ELAMH para mostrar una lista de errores y los detalles de un error específico de una página web y como una fuente RSS. ELMAH es más fácil de configurar que la supervisión de estado porque solamente registra errores. Además, ELMAH incluye compatibilidad con ASP.NET 1.x, las aplicaciones de ASP.NET 2.0 y 3.5 de ASP.NET y se suministra con una variedad de proveedores de registro de origen.

Este tutorial le guía a través de los pasos necesarios para agregar ELMAH a una aplicación de ASP.NET. Comencemos.

> [!NOTE]
> El estado de supervisión del sistema y ELMAH tienen sus propios conjuntos de ventajas y desventajas. Recomendamos que intente ambos sistemas y decidir qué uno más adecuada a sus necesidades.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Agregar ELMAH a una aplicación Web ASP.NET

Integrar ELMAH en una aplicación de ASP.NET nueva o existente es un proceso sencillo y fácil que tarda menos de cinco minutos. En pocas palabras, implica cuatro sencillos pasos:

1. Descargue ELMAH y agregue la `Elmah.dll` ensamblado a la aplicación web,
2. Registrar módulos HTTP y el controlador del ELMAH `Web.config`,
3. Especificar opciones de configuración del ELMAH, y
4. Crear la infraestructura de origen de registro de errores, si es necesario.

Analicemos cada uno de estos cuatro pasos, uno en uno.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Paso 1: Descarga de los archivos de proyecto ELMAH y agregar`Elmah.dll`a la aplicación Web

ELMAH 1.0 BETA 3 (compilación 10617), la versión más reciente en el momento de escribir este artículo, se incluye en la descarga disponible con este tutorial. Como alternativa, puede visitar el [sitio Web ELMAH](https://code.google.com/p/elmah/) para obtener la versión más reciente o para descargar el código fuente. Extraer la descarga ELMAH a una carpeta en el escritorio y busque el archivo de ensamblado ELMAH (`Elmah.dll`).

> [!NOTE]
> El `Elmah.dll` archivo se encuentra en la descarga `Bin` carpeta, que tiene subcarpetas para distintas versiones de .NET Framework y para las compilaciones de lanzamiento y depuración. Utilice la versión de lanzamiento de la versión de framework adecuado. Por ejemplo, si está creando una aplicación web de ASP.NET 3.5, copie el `Elmah.dll` de archivos desde el `Bin\net-3.5\Release` carpeta.


A continuación, abra Visual Studio y agregue el ensamblado al proyecto con el botón secundario en el nombre del sitio Web en el Explorador de soluciones y eligiendo la opción Agregar referencia en el menú contextual. Se abrirá el cuadro de diálogo Agregar referencia. Navegue a la ficha Examinar y elija la `Elmah.dll` archivo. Esta acción agrega el `Elmah.dll` archivo a la aplicación web `Bin` carpeta.

> [!NOTE]
> El tipo de proyecto de aplicación Web (WAP) no se muestra la `Bin` carpeta en el Explorador de soluciones. En su lugar, se muestran estos elementos en la carpeta de referencias.


La `Elmah.dll` ensamblado incluye las clases utilizadas por el sistema ELMAH. Estas clases se dividen en tres categorías:

- **Módulos HTTP** -un módulo HTTP es una clase que defina los controladores de eventos para `HttpApplication` eventos, como el `Error` eventos. ELMAH incluye varios módulos HTTP, tres las más relevante que se va a: 

    - `ErrorLogModule` : registra las excepciones no controladas en un origen de registro.
    - `ErrorMailModule` -envía los detalles de una excepción no controlada en un mensaje de correo electrónico.
    - `ErrorFilterModule` -se aplica filtros especificado por el desarrollador para determinar qué excepciones se registran y lo que los que se pasan por alto.
- **Controladores HTTP** -un controlador HTTP es una clase que es responsable de generar el marcado para un tipo concreto de solicitud. ELMAH incluye controladores HTTP que representen los detalles del error como una página web, como una fuente RSS o como un archivo delimitado por comas (CSV).
- **Orígenes de registro de error** : de fábrica ELMAH puede registrar errores en la memoria, una base de datos de Microsoft SQL Server, una base de datos de Microsoft Access, una base de datos de Oracle, en un archivo XML, una base de datos de SQLite, o a una base de datos de la base de datos de Vista. Al igual que el sistema de supervisión de estado, arquitectura del ELMAH se generó usando el modelo de proveedor, lo que significa que puede crear e integrar fácilmente sus propios proveedores de orígenes de registro personalizado, si es necesario.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Paso 2: Registrar del ELMAH módulo HTTP y controlador

Mientras el `Elmah.dll` archivo contiene los módulos HTTP y controlador necesario para iniciar automáticamente las excepciones no controladas y para mostrar detalles del error de una página web, estos se deben registrar explícitamente en la configuración de la aplicación web. El `ErrorLogModule` módulo HTTP, una vez registrado, se suscribe a la `HttpApplication`de `Error` eventos. Cada vez que este evento se desencadena la `ErrorLogModule` registra los detalles de la excepción a un origen de registro especificado. Veremos cómo definir el proveedor de origen de registro en la siguiente sección, "Configurar ELMAH." El `ErrorLogPageFactory` fábrica de controladores HTTP es responsable de generar el marcado al ver el registro de errores desde una página web.

La sintaxis específica para registrar los módulos HTTP y controladores depende del servidor web que se está activando el sitio. Para que el servidor de desarrollo de ASP.NET y de Microsoft IIS versión 6.0 y anteriores, los módulos HTTP y controladores se registran en el `<httpModules>` y `<httpHandlers>` secciones, que aparecen en la `<system.web>` elemento. Si está utilizando IIS 7.0 y que tienen que ser registrados en el `<system.webServer>` del elemento `<modules>` y `<handlers>` secciones. Afortunadamente, puede definir los módulos HTTP y controladores en *ambos* coloca, independientemente del servidor web que se va a usar. Esta opción es la más portátil ya que permite la misma configuración que se usará en los entornos de producción y desarrollo, independientemente del servidor web que se va a usar.

Iniciar registrando el `ErrorLogModule` módulo HTTP y el `ErrorLogPageFactory` controlador HTTP en el `<httpModules>` y `<httpHandlers>` sección `<system.web>`. Si la configuración ya define estos dos elementos, a continuación, basta con incluir el `<add>` (elemento) para el módulo HTTP del ELMAH y el controlador.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

A continuación, registrar del ELMAH módulo HTTP y el controlador en el `<system.webServer>` elemento. Como antes, si este elemento ya no está presente en la configuración, a continuación, agregarlo.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

De forma predeterminada, IIS 7 se queja si módulos HTTP y controladores se registran en la `<system.web>` sección. El `validateIntegratedModeConfiguration` de atributo en el `<validation>` elemento indica a IIS 7 para suprimir estos mensajes de error.

Tenga en cuenta que la sintaxis para registrar el `ErrorLogPageFactory` controlador HTTP incluye un `path` atributo, que se establece en `elmah.axd`. Este atributo informa a la aplicación web que if llega una solicitud de una página denominada `elmah.axd` , a continuación, la solicitud debe ser procesada por el `ErrorLogPageFactory` controlador HTTP. Vamos a ver el `ErrorLogPageFactory` controlador HTTP en la acción más adelante en este tutorial.

### <a name="step-3-configuring-elmah"></a>Paso 3: Configurar ELMAH

ELMAH busca sus opciones de configuración en el sitio de Web `Web.config` archivo en una sección de configuración personalizada denominada `<elmah>`. Para poder usar una sección personalizada en `Web.config` en primer lugar debe definirse en el `<configSections>` elemento. Abra la `Web.config` de archivos y agregue el siguiente marcado para la `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> Si está configurando ELMAH para una aplicación ASP.NET 1.x, a continuación, quite el `requirePermission="false"` de atributo de la `<section>` elementos anteriores.


La sintaxis anterior registra personalizado `<elmah>` sección y sus subsecciones: `<security>`, `<errorLog>`, `<errorMail>`, y `<errorFilter>`.

A continuación, agregue el `<elmah>` sección a `Web.config`. En esta sección debe aparecer en el mismo nivel que el `<system.web>` elemento. Dentro de la `<elmah>` sección agregue el `<security>` y `<errorLog>` secciones de este modo:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

El `<security>` la sección `allowRemoteAccess` atributo indica si se permite el acceso remoto. Si este valor se establece en 0, a continuación, la página web de registro de error sólo puede verse localmente. Si este atributo se establece en 1, a continuación, la página web de registro de errores está habilitado para los visitantes remotos y locales. Por ahora, vamos a deshabilitar la página web de registro de errores para los visitantes remotos. Se permitirá el acceso remoto más adelante después de que tengamos una oportunidad de debatir las preocupaciones de seguridad de hacerlo.

El `<errorLog>` sección define el origen del registro de error que indica dónde se registran los detalles del error; es similar a la `<providers>` sección en el sistema de supervisión de estado. La sintaxis anterior especifica la `SqlErrorLog` clase como el origen de registro de error, que registra los errores en una base de datos de Microsoft SQL Server especificado por el `connectionStringName` valor de atributo.

> [!NOTE]
> ELMAH se suministra con proveedores de registro de error adicional que pueden usarse para registrar errores en un archivo XML, una base de datos de Microsoft Access, una base de datos de Oracle y otros almacenes de datos. Consulte el ejemplo `Web.config` archivo que se incluye con la descarga ELMAH para obtener información sobre cómo usar estos proveedores de registro de error alternativa.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Paso 4: Creación de la infraestructura de origen del registro de errores

Del ELMAH `SqlErrorLog` proveedor registra los detalles del error en una base de datos de Microsoft SQL Server especificada. El `SqlErrorLog` proveedor espera esta base de datos tiene una tabla denominada `ELMAH_Error` y tres procedimientos almacenan: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, y `ELMAH_LogError`. La descarga ELMAH incluye un archivo denominado `SQLServer.sql` en el `db` carpeta que contiene el código T-SQL para crear esta tabla y estos procedimientos almacenados. Deberá ejecutar estas instrucciones en la base de datos para usar el `SqlErrorLog` proveedor.

**Las figuras 1** y **2** mostrar el Explorador de base de datos en Visual Studio después de los objetos de base de datos que necesitan la `SqlErrorLog` proveedor se han agregado.

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**Figura 1**: el `SqlErrorLog` proveedor registra los errores a la `ELMAH_Error` tabla

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**Figura 2**: el `SqlErrorLog` proveedor usa tres procedimientos almacenados

## <a name="elmah-in-action"></a>ELMAH en acción

En este momento agregamos ELMAH a la aplicación web, registra el `ErrorLogModule` módulo HTTP y el `ErrorLogPageFactory` controlador HTTP, especificar opciones de configuración del ELMAH en `Web.config`y agrega los objetos de base de datos necesaria para la `SqlErrorLog` proveedor de registro de error. Ahora estamos preparados para ver ELMAH en acción. Visite el sitio Web de reseñas de libros y visite una página que genera un error en tiempo de ejecución, como `Genre.aspx?ID=foo`, o una página inexistente, como `NoSuchPage.aspx`. Lo que se ve al visitar estas páginas depende de la `<customErrors>` configuración y si está visitando local o remotamente. (Hacen referencia a la [ *mostrar una página de errores personalizada* tutorial](displaying-a-custom-error-page-vb.md) para repasar en este tema.)

ELMAH no afecta a qué contenido se muestra al usuario si se produce una excepción no controlada. solo registra los detalles. Este registro de errores es accesible desde la página web `elmah.axd` desde la raíz del sitio Web, como `http://localhost/BookReviews/elmah.axd`. (Este archivo no existe físicamente en el proyecto, pero cuando llega una solicitud para `elmah.axd` el tiempo de ejecución lo envía a la `ErrorLogPageFactory` controlador HTTP, ya que genera el marcado que se envía al explorador.)

> [!NOTE]
> También puede usar el `elmah.axd` página para indicar a ELMAH para generar un error de prueba. Visitar `elmah.axd/test` (como en `http://localhost/BookReviews/elmah.axd/test`) hace ELMAH producir una excepción de tipo `Elmah.TestException`, que tiene el mensaje de error: "Ésta es una excepción de prueba que puede pasar por alto."


**Figura 3** muestra el registro de errores al visitar `elmah.axd` desde el entorno de desarrollo.

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**Figura 3**: `Elmah.axd` muestra el registro de errores desde una página Web  
([Haga clic aquí para ver la imagen a tamaño completo](logging-error-details-with-elmah-vb/_static/image7.png))

El error de registro **figura 3** contiene seis entradas de error. Cada entrada incluye el código de estado HTTP (404 ó 500, estos errores), el tipo, la descripción, el nombre de usuario con sesión iniciada cuando se produjo el error y la fecha y hora. Haga clic en el vínculo de detalles muestra una página que incluye el mismo mensaje de error se muestra en el Error detalles amarillo pantalla de la muerte (vea **figura 4**) junto con los valores de las variables de servidor en el momento del error (vea  **Figura 5**). También puede ver el XML sin formato en el que se guardan los detalles del error, que incluye información adicional, como los valores en el encabezado HTTP POST.

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**Figura 4**: ver los detalles del Error YSOD  
([Haga clic aquí para ver la imagen a tamaño completo](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**Figura 5**: explorar los valores de la colección de Variables de servidor en el momento del Error  
([Haga clic aquí para ver la imagen a tamaño completo](logging-error-details-with-elmah-vb/_static/image13.png))

Implementar ELMAH en el sitio Web de producción implica:

- Copiar la `Elmah.dll` del archivo a la `Bin` carpeta en producción,
- Copia los valores de configuración ELMAH específicos para la `Web.config` archivo utilizado en producción, y
- Agregar la infraestructura de origen del registro de errores a la base de datos de producción.

Hemos analizado técnicas para copiar archivos del desarrollo al entorno de producción de tutoriales anteriores. Quizás la manera más fácil de obtener la infraestructura de origen del registro de errores en la base de datos de producción es usar SQL Server Management Studio para conectarse a la base de datos de producción y, a continuación, ejecute el `SqlServer.sql` archivo de script, que se creará la tabla necesaria y almacenados procedimientos.

### <a name="viewing-the-error-details-page-on-production"></a>Ver la página de detalles del Error en producción

Después de implementar el sitio de producción, visite el sitio Web de producción y generar una excepción no controlada. Como en el entorno de desarrollo, ELMAH no tiene ningún efecto en la página de error que se muestra cuando se produce una excepción no controlada; en su lugar, simplemente registra el error. Si intentan volver a visitar la página de registro de errores (`elmah.axd`) desde el entorno de producción, aparecerá con la página de prohibido mostrada en **figura 6**.

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**Figura 6**: de forma predeterminada, los visitantes remotos no pueden ver la página Web de registro de errores  
([Haga clic aquí para ver la imagen a tamaño completo](logging-error-details-with-elmah-vb/_static/image16.png))

Recuerde que en la configuración de ELMAH `<security>` sección establecemos la `allowRemoteAccess` atributo en 0, lo que impide que los usuarios remotos de ver el registro de errores. Es importante prohibir que los visitantes anónimos de ver el registro de errores, como los detalles del error pueden revelar vulnerabilidades de seguridad u otra información confidencial. Si decide establecer este atributo en 1 y habilitar el acceso remoto en el registro de errores, es importante para bloquear el `elmah.axd` ruta de acceso de modo que solo los visitantes autorizados puede tener acceso a él. Esto puede lograrse mediante la adición de un `<location>` elemento a la `Web.config` archivo.

La siguiente configuración permite que solo los usuarios en el rol de administrador para tener acceso a la página web de registro de error:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> Se han agregado el rol de administrador y los usuarios en el sistema - Scott, Jisun y Alice - tres en el [ *configurar un sitio Web que utiliza servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-vb.md). Los usuarios Scott y Jisun son miembros del rol de administrador. Para obtener más información sobre la autenticación y autorización, consulte mi [tutoriales de seguridad del sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


El registro de errores en el entorno de producción puede verse ahora los usuarios remotos; hacen referencia a **las figuras 3**, **4**, y **5** de capturas de pantalla de la página web de registro de error. Sin embargo, si un usuario anónimo o de administrador no intenta ver la página de registro de errores le redirigirá automáticamente a la página de inicio de sesión (`Login.aspx`), como **figura 7** muestra.

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**Figura 7**: los usuarios no autorizados son redirigirá automáticamente a la página de inicio de sesión  
([Haga clic aquí para ver la imagen a tamaño completo](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Registro de errores mediante programación

Del ELMAH `ErrorLogModule` módulo HTTP registra automáticamente las excepciones no controladas en el origen de registro especificado. Como alternativa, puede registrar un error sin necesidad de generar una excepción no controlada mediante la `ErrorSignal` clase y su `Raise` método. El `Raise` se pasa al método un `Exception` objeto y lo registra como si esa excepción se ha iniciado y había alcanzado el tiempo de ejecución ASP.NET sin que se va a controlar. Sin embargo, la diferencia es que la solicitud continúa normalmente después de ejecutar el `Raise` método se ha llamado, mientras que una excepción no controlada producida interrumpe la ejecución normal de la solicitud y hace que el tiempo de ejecución ASP.NET mostrar el configurado página de error.

La `ErrorSignal` clase es útil en situaciones donde hay alguna acción que puede producir un error, pero su error no es grave para la operación global que se va a realizar. Por ejemplo, un sitio Web puede contener un formulario que toma la entrada del usuario, lo almacena en una base de datos y, a continuación, envía al usuario un correo electrónico que les informa que no se procesó la información. ¿Qué debe ocurrir si la información se guarda en la base de datos correctamente, pero hay un error al enviar el mensaje de correo electrónico? Una opción sería producir una excepción y enviar al usuario a la página de error. Sin embargo, esto puede confundir al usuario para hacerle creer que no se ha guardado la información que ha especificado. Otro enfoque sería registrar los errores relacionados con el correo electrónico, pero no modificar la experiencia del usuario en modo alguno. Aquí es donde el `ErrorSignal` clase es útil.

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>Error de notificación por correo electrónico

Junto con el registro de errores para una base de datos, ELMAH también pueden configurarse para enviar por correo electrónico los detalles del error a un destinatario especificado. Esta funcionalidad se proporciona mediante la `ErrorMailModule` módulo HTTP; por lo tanto, debe registrar este módulo HTTP en `Web.config` para enviar los detalles del error a través de correo electrónico.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

A continuación, especifique la información sobre el mensaje de error en la `<elmah>` del elemento `<errorMail>` sección, que indica de remitente y el destinatario, el asunto del correo electrónico y si el correo electrónico se envía de forma asincrónica.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

Con la configuración anterior en su lugar, siempre que un error en tiempo de ejecución se produce ELMAH envía un correo electrónico a support@example.com con los detalles del error. Correo electrónico de error del ELMAH incluye la misma información que se muestra en la página web detalles de error, es decir, el mensaje de error, el seguimiento de pila y las variables de servidor (hacen referencia a **4 cifras** y **5**). El mensaje de error también incluye el contenido de excepción detalles pantalla amarillo de muerte como datos adjuntos (`YSOD.html`).

**Figura 8** muestra el correo electrónico de error del ELMAH generado visitando `Genre.aspx?ID=foo`. Mientras **figura 8** muestra sólo el mensaje y la pila de seguimiento de errores, las variables de servidor se incluyen más abajo en el cuerpo del correo electrónico.

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**Figura 8**: puede configurar ELMAH para enviar los detalles del Error a través de correo electrónico  
([Haga clic aquí para ver la imagen a tamaño completo](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Sólo registrar errores de interés

De forma predeterminada, ELMAH registra los detalles de todas las excepciones no controladas, incluidas 404 y otros errores HTTP. Puede indicar ELMAH para pasar por alto estos u otros tipos de errores mediante el filtrado de error. La lógica de filtrado se realiza del ELMAH `ErrorFilterModule` módulo HTTP, que necesitará para registrar en `Web.config` para poder usar la lógica de filtrado. Las reglas de filtrado se especifican en la `<errorFilter>` sección.

El siguiente marcado indica ELMAH para no registrar 404 errores.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> No olvide, con el fin de usar el filtrado de error debe registrar el `ErrorFilterModule` módulo HTTP.


El `<equal>` elemento dentro de la `<test>` sección se conoce como una aserción. Si la aserción se evalúa como true, a continuación, se filtra el error de registro del ELMAH. Hay otras aserciones disponibles, incluido: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`, y así sucesivamente. También puede combinar las aserciones mediante el `<and>` y `<or>` operadores booleanos. Es más, incluso puede incluir una expresión simple de JavaScript como una aserción o escribir sus propias aserciones en C# o Visual Basic.

Para obtener más información sobre el error del ELMAH capacidades de filtrado, consulte el [sección filtrar errores](https://code.google.com/p/elmah/wiki/ErrorFiltering) en el [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Resumen

ELMAH proporciona un mecanismo sencillo y eficaz para el registro de errores en una aplicación web ASP.NET. Al igual que el sistema de supervisión de estado de Microsoft, ELMAH puede registrar errores en una base de datos y puede enviar los detalles del error a un desarrollador a través de correo electrónico. A diferencia del sistema de supervisión de estado, ELMAH incluye soporte inmediato para una amplia variedad de almacenes de datos de registro de error, incluidas: Microsoft SQL Server, Microsoft Access, Oracle, archivos XML y muchas otras personas. Además, ELMAH ofrece un mecanismo integrado para ver el registro de errores y los detalles sobre un error específico de una página web, `elmah.axd`. La `elmah.axd` página también puede representar la información de error como una fuente RSS o como un archivo de valores separados por comas (CSV), los cuales puede leer con Microsoft Excel. También puede indicar ELMAH para filtrar errores en el registro mediante las aserciones declarativas o mediante programación. Y ELMAH puede usarse con aplicaciones de ASP.NET 1.x.

Todas las aplicaciones implementadas deben disponer de un mecanismo para registrar automáticamente las excepciones no controladas y enviar notificaciones al equipo de desarrollo. Si se realiza mediante el seguimiento de estado o ELMAH es secundario. En otras palabras, no importa realmente mucho si se usa la supervisión del estado o ELMAH; evaluar ambos sistemas y, a continuación, seleccione la que mejor se adapte a sus necesidades. ¿Qué es fundamentalmente importante es que algún mecanismo colocarse en su lugar para registrar excepciones no controladas en el entorno de producción.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [ELMAH - controladores y módulos de registro de Error](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Página de proyecto ELMAH](https://code.google.com/p/elmah/) (origen de código, ejemplos, wiki)
- [Es decir, sería ELMAH en una aplicación Web para detectar las excepciones no controladas](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (vídeo)
- [Páginas de registro de errores de seguridad](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Usar módulos HTTP y controladores para crear componentes ASP.NET conectables](https://msdn.microsoft.com/library/aa479332.aspx)
- [Tutoriales de seguridad de sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Anterior](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [Siguiente](precompiling-your-website-vb.md)
