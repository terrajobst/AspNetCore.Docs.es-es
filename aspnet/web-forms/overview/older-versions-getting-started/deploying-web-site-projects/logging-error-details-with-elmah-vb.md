---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: Registrar detalles del Error con ELMAH (VB) | Microsoft Docs
author: rick-anderson
description: Error de registro de módulos y controladores (ELMAH) ofrece otro enfoque para registrar errores en tiempo de ejecución en un entorno de producción. ELMAH es un error de código abierto...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: ed59c6099925a2046d201e0eab0a9afdd620de28
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389207"
---
<a name="logging-error-details-with-elmah-vb"></a>Registrar detalles del Error con ELMAH (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip) o [descargar PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> Error de registro de módulos y controladores (ELMAH) ofrece otro enfoque para registrar errores en tiempo de ejecución en un entorno de producción. ELMAH es una biblioteca de registro de errores de código abierto gratuito que incluye características como filtrado del error y la capacidad para ver el registro de errores en una página web, como una fuente RSS, o para descargarla como un archivo delimitado por comas. Este tutorial le guía a través de la descarga y la configuración ELMAH.


## <a name="introduction"></a>Introducción

El [tutorial anterior](logging-error-details-with-asp-net-health-monitoring-vb.md) examinan ASP. Sistema, que ofrece un fuera de la biblioteca de casilla para registrar una amplia gama de eventos Web de supervisión de estado de la red. Muchos desarrolladores usan para iniciar sesión y enviar por correo electrónico los detalles de las excepciones no controladas de supervisión de estado. Sin embargo, hay algunos puntos débiles con este sistema. En primer lugar y ante todo es la falta de algún tipo de interfaz de usuario para ver información acerca de los eventos registrados. Si desea ver un resumen de los 10 últimos errores, o ver los detalles de un error que se produjo la última semana, debe escribir en memoria ya sea a través de la base de datos, busque en su Bandeja de entrada de correo electrónico o crear una página web que muestra información de la `aspnet_WebEvent_Events` tabla.

Otra dificultad se centra en la complejidad del seguimiento de estado. Dado que la supervisión de estado se puede usar para registrar una gran cantidad de eventos diferentes, y porque hay una variedad de opciones para indicar cómo y cuándo se registran los eventos, al configurar correctamente el sistema de supervisión de estado puede ser una tarea tan laboriosa. Por último, hay problemas de compatibilidad. Dado que la supervisión de estado en primer lugar se agregó a la versión 2.0 de .NET Framework, no está disponible para aplicaciones web anteriores creadas con la versión de ASP.NET 1.x. Además, el `SqlWebEventProvider` (clase), que hemos usado en el tutorial anterior para los detalles del error de los registros a una base de datos, solo funciona con bases de datos de Microsoft SQL Server. Deberá crear una clase de proveedor de registro personalizado necesita registrar errores en un almacén de datos alternativo, como un archivo XML o base de datos de Oracle.

Una alternativa al sistema de supervisión de estado es Error de registro de módulos y controladores (ELMAH), un sistema de registro de errores gratuito, código abierto creado por [Atif Aziz](http://www.raboof.com/). La diferencia más notable entre los dos sistemas es la capacidad del ELAMH para mostrar una lista de errores y los detalles de un error específico desde una página web y como una fuente RSS. ELMAH es más fácil de configurar que la supervisión de estado porque solo registra errores. Además, ELMAH incluye compatibilidad para ASP.NET 1.x, las aplicaciones de ASP.NET 2.0 y ASP.NET 3.5 y se distribuye con una variedad de proveedores de registro de origen.

Este tutorial le guía a través de los pasos necesarios para agregar ELMAH a una aplicación de ASP.NET. Comencemos.

> [!NOTE]
> El estado de supervisión del sistema y ELMAH ambos tienen sus propios conjuntos de ventajas y desventajas. Le animo a probar ambos sistemas y decidir qué se ajusta mejor a sus necesidades.


## <a name="adding-elmah-to-an-aspnet-web-application"></a>Agregar ELMAH a una aplicación Web ASP.NET

Integración ELMAH en una aplicación de ASP.NET nueva o existente es un proceso muy sencillo que tarda menos de cinco minutos. En pocas palabras, implica cuatro pasos sencillos:

1. Descargar ELMAH y agregar el `Elmah.dll` ensamblado a la aplicación web,
2. Registrar módulos HTTP y el controlador en ELMAH `Web.config`,
3. Especifique las opciones de configuración del ELMAH, y
4. Crear la infraestructura de origen de registro de errores, si es necesario.

Analicemos cada uno de estos cuatro pasos, una vez.

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>Paso 1: Descargar los archivos de proyecto ELMAH y agregar`Elmah.dll`a la aplicación Web

ELMAH 1.0 BETA 3 (compilación 10617), la versión más reciente en el momento de escribir este artículo, se incluye en la descarga disponible con este tutorial. Como alternativa, puede visitar el [sitio Web ELMAH](https://code.google.com/p/elmah/) para obtener la versión más reciente o para descargar el código fuente. Extraer la descarga ELMAH a una carpeta en el escritorio y busque el archivo de ensamblado ELMAH (`Elmah.dll`).

> [!NOTE]
> El `Elmah.dll` archivo se encuentra en la descarga `Bin` carpeta, que tiene subcarpetas para diferentes versiones de .NET Framework y para las compilaciones de lanzamiento y depuración. Use la versión de lanzamiento para la versión de framework adecuado. Por ejemplo, si está creando una aplicación web de ASP.NET 3.5, copie el `Elmah.dll` de archivos desde el `Bin\net-3.5\Release` carpeta.


A continuación, abra Visual Studio y agregue el ensamblado al proyecto con el botón secundario en el nombre de sitio Web en el Explorador de soluciones y elegir la opción Agregar referencia en el menú contextual. Se abrirá el cuadro de diálogo Agregar referencia. Vaya a la pestaña Examinar y elija el `Elmah.dll` archivo. Esta acción agrega el `Elmah.dll` a la aplicación web `Bin` carpeta.

> [!NOTE]
> El tipo de proyecto de aplicación Web (WAP) no se muestra el `Bin` carpeta en el Explorador de soluciones. En su lugar, muestra estos elementos en la carpeta de referencias.


El `Elmah.dll` ensamblado incluye las clases utilizadas por el sistema ELMAH. Estas clases se dividen en tres categorías:

- **Los módulos HTTP** -un módulo HTTP es una clase que define los controladores de eventos para `HttpApplication` eventos, como el `Error` eventos. ELMAH incluye varios módulos HTTP, tres las más relevante que se va a: 

    - `ErrorLogModule` : registra las excepciones no controladas en un origen de registro.
    - `ErrorMailModule` -envía los detalles de una excepción no controlada en un mensaje de correo electrónico.
    - `ErrorFilterModule` -se aplica filtros especificado por el desarrollador para determinar qué excepciones se registran y lo que se omiten las.
- **Controladores HTTP** -un controlador HTTP es una clase que es responsable de generar el marcado para un determinado tipo de solicitud. ELMAH incluye controladores HTTP que representen los detalles del error como una página web, como una fuente RSS o como un archivo delimitado por comas (CSV).
- **Orígenes de registro de errores** : fábrica ELMAH puede registrar errores en la memoria, una base de datos de Microsoft SQL Server, una base de datos de Microsoft Access, una base de datos de Oracle en un archivo XML, una base de datos de SQLite, o a una base de datos de la base de datos de Vista. Al igual que el sistema de supervisión de estado, arquitectura de ELMAH se generó con el modelo de proveedor, lo que significa que puede crear e integrar sin problemas sus propios proveedores de orígenes de registro personalizado, si es necesario.

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>Paso 2: Registrar del ELMAH módulo HTTP y controlador

Mientras el `Elmah.dll` archivo contiene los módulos HTTP y controlador necesaria para iniciar sesión automáticamente las excepciones no controladas y para mostrar los detalles del error de una página web, estos se deben registrar explícitamente en la configuración de la aplicación web. El `ErrorLogModule` módulo HTTP, una vez registrado, se suscribe a la `HttpApplication`del `Error` eventos. Cada vez que este evento se desencadena la `ErrorLogModule` registra los detalles de la excepción a un origen de registro especificado. Veremos cómo se define el proveedor de origen de registro en la siguiente sección, "Configurar ELMAH." El `ErrorLogPageFactory` fábrica de controladores HTTP es responsable de generar el marcado al ver el registro de errores de una página web.

El servidor web que se está activando el sitio depende de la sintaxis específica para el registro de módulos y controladores HTTP. Para el servidor de desarrollo de ASP.NET y Microsoft IIS versión 6.0 y versiones anterior, los módulos y controladores HTTP se registran en el `<httpModules>` y `<httpHandlers>` secciones, que aparecen en la `<system.web>` elemento. Si está utilizando IIS 7.0, que deben estar registrados en el `<system.webServer>` del elemento `<modules>` y `<handlers>` secciones. Afortunadamente, puede definir los módulos y controladores HTTP en *ambos* coloca, independientemente del servidor web que se va a usar. Esta opción es lo más portable, ya que permite la misma configuración que se usará en los entornos de desarrollo y producción, independientemente del servidor web que se va a usar.

Comience por registrar los `ErrorLogModule` módulo HTTP y el `ErrorLogPageFactory` controlador HTTP en el `<httpModules>` y `<httpHandlers>` sección `<system.web>`. Si la configuración ya define estos dos elementos, a continuación, basta con incluir el `<add>` (elemento) para el módulo HTTP del ELMAH y el controlador.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

A continuación, registre del ELMAH módulo HTTP y el controlador en el `<system.webServer>` elemento. Como antes, si este elemento ya no está presente en la configuración, a continuación, agregarlo.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

De forma predeterminada, IIS 7 se queja si los módulos y controladores HTTP se registran en el `<system.web>` sección. El `validateIntegratedModeConfiguration` atributo el `<validation>` elemento indica a IIS 7 para suprimir los mensajes de error.

Tenga en cuenta que la sintaxis para registrar el `ErrorLogPageFactory` controlador HTTP incluye un `path` atributo, que se establece en `elmah.axd`. Este atributo informa a la aplicación web que if llega una solicitud de una página denominada `elmah.axd` , a continuación, la solicitud debe ser procesada por el `ErrorLogPageFactory` controlador HTTP. Veremos el `ErrorLogPageFactory` controlador HTTP en acción, más adelante en este tutorial.

### <a name="step-3-configuring-elmah"></a>Paso 3: Configurar ELMAH

ELMAH busca sus opciones de configuración en el sitio de Web `Web.config` archivo en una sección de configuración personalizada denominada `<elmah>`. Para poder usar una sección personalizada en `Web.config` debe definirse primero en el `<configSections>` elemento. Abra el `Web.config` archivo y agregue el siguiente marcado para el `<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> Si está configurando ELMAH para una aplicación ASP.NET 1.x, a continuación, quite el `requirePermission="false"` atributo desde el `<section>` elementos anteriores.


La sintaxis anterior registra personalizado `<elmah>` sección y sus subsecciones: `<security>`, `<errorLog>`, `<errorMail>`, y `<errorFilter>`.

A continuación, agregue el `<elmah>` sección a `Web.config`. En esta sección debe aparecer en el mismo nivel que el `<system.web>` elemento. Dentro de la `<elmah>` Agregar sección la `<security>` y `<errorLog>` secciones de este modo:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

El `<security>` la sección `allowRemoteAccess` atributo indica si se permite el acceso remoto. Si este valor se establece en 0, a continuación, la página web de registro de error solo puede verse localmente. Si este atributo se establece en 1, a continuación, la página web de registro de errores está habilitada para los visitantes remotos y locales. Por ahora, vamos a deshabilitar la página web de registro de errores para los visitantes remotos. Se permitirá el acceso remoto más adelante después de que tengamos una oportunidad de debatir las preocupaciones de seguridad de hacerlo.

El `<errorLog>` sección define el origen de registro de error que indica dónde se registran los detalles del error; es similar a la `<providers>` sección en el sistema de supervisión de estado. La sintaxis anterior especifica el `SqlErrorLog` clase como el origen de registro de error, que registra los errores en una base de datos de Microsoft SQL Server especificado por el `connectionStringName` valor del atributo.

> [!NOTE]
> ELMAH se suministra con los proveedores de registro de error adicional que pueden usarse para registrar los errores a un archivo XML, una base de datos de Microsoft Access, una base de datos de Oracle y otros almacenes de datos. Consulte el ejemplo `Web.config` archivo que se incluye con la descarga ELMAH para obtener información sobre cómo usar estos proveedores de registro de error alternativa.


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>Paso 4: Creación de la infraestructura de origen del registro de errores

Del ELMAH `SqlErrorLog` proveedor registra los detalles del error en una base de datos de Microsoft SQL Server especificada. El `SqlErrorLog` proveedor espera que esta base de datos tiene una tabla denominada `ELMAH_Error` y tres procedimientos almacenan: `ELMAH_GetErrorsXml`, `ELMAH_GetErrorXml`, y `ELMAH_LogError`. La descarga ELMAH incluye un archivo denominado `SQLServer.sql` en el `db` carpeta que contiene el T-SQL para crear esta tabla y estos procedimientos almacenados. Deberá ejecutar estas instrucciones en la base de datos para usar el `SqlErrorLog` proveedor.

**Las figuras 1** y **2** mostrar el Explorador de base de datos en Visual Studio después de los objetos de base de datos necesarios para la `SqlErrorLog` proveedor se han agregado.

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**Figura 1**: el `SqlErrorLog` proveedor registra los errores para el `ELMAH_Error` tabla

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**Figura 2**: el `SqlErrorLog` proveedor utiliza tres procedimientos almacenados

## <a name="elmah-in-action"></a>ELMAH en acción

En este punto hemos agregado ELMAH a la aplicación web, que se registró el `ErrorLogModule` módulo HTTP y el `ErrorLogPageFactory` controlador HTTP, especificar las opciones de configuración del ELMAH en `Web.config`y agrega los objetos de base de datos necesarios para la `SqlErrorLog` proveedor de registro de error. ¡Ahora estamos preparados para ver ELMAH en acción! Visite el sitio Web de reseñas de libros y visita una página que genera un error en tiempo de ejecución, como `Genre.aspx?ID=foo`, o una página inexistente, tales como `NoSuchPage.aspx`. Lo que ve al visitar estas páginas depende el `<customErrors>` configuración y si está visitando local o remotamente. (Vuelva a consultar el [ *mostrar una página de Error personalizada* tutorial](displaying-a-custom-error-page-vb.md) para hacer un repaso sobre este tema.)

ELMAH no afecta a qué contenido se muestra al usuario cuando se produce una excepción no controlada; sólo registra sus detalles. Este registro de errores es accesible desde la página web `elmah.axd` desde la raíz del sitio Web, como `http://localhost/BookReviews/elmah.axd`. (Este archivo no existe físicamente en el proyecto, pero cuando llega una solicitud para `elmah.axd` el tiempo de ejecución lo envía a la `ErrorLogPageFactory` controlador HTTP que genera el marcado que se envía al explorador.)

> [!NOTE]
> También puede usar el `elmah.axd` página para indicar a ELMAH para generar un error de prueba. Visitar `elmah.axd/test` (como en `http://localhost/BookReviews/elmah.axd/test`) hace que ELMAH producir una excepción de tipo `Elmah.TestException`, que tiene el mensaje de error: "Es una excepción de prueba puede omitirse."


**Figura 3** muestra el registro de errores cuando se visita `elmah.axd` desde el entorno de desarrollo.

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**Figura 3**: `Elmah.axd` muestra el registro de errores de una página Web  
([Haga clic aquí para ver imagen en tamaño completo](logging-error-details-with-elmah-vb/_static/image7.png))

El registro de errores **figura 3** contiene seis entradas de error. Cada entrada incluye el código de estado HTTP (404 o 500, estos errores), el tipo, la descripción, el nombre del usuario con sesión iniciada cuando se produjo el error y la fecha y hora. Al hacer clic en el vínculo de detalles muestra una página que incluye el mismo mensaje de error que se muestra en el Error detalles amarillo pantalla de la muerte (consulte **figura 4**) junto con los valores de las variables de servidor en el momento del error (consulte  **Figura 5**). También puede ver el XML sin formato en el que se guardan los detalles del error, que incluye información adicional, como los valores en el encabezado de solicitud HTTP POST.

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**Figura 4**: ver los detalles del Error YSOD  
([Haga clic aquí para ver imagen en tamaño completo](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**Figura 5**: Explore los valores de la colección de Variables de servidor en el momento del Error  
([Haga clic aquí para ver imagen en tamaño completo](logging-error-details-with-elmah-vb/_static/image13.png))

Implementar ELMAH en el sitio Web de producción implica:

- Copiar el `Elmah.dll` del archivo a la `Bin` carpeta en producción,
- Copiar los valores de configuración ELMAH específicos para la `Web.config` archivo utilizado en producción, y
- Adición de la infraestructura de origen del registro de errores para la base de datos de producción.

Hemos explorado las técnicas para copiar archivos desde el desarrollo a producción en los tutoriales anteriores. Quizás es la manera más fácil de obtener la infraestructura de origen del registro de errores en la base de datos de producción usar SQL Server Management Studio para conectarse a la base de datos de producción y, a continuación, ejecute el `SqlServer.sql` archivo de script, que creará la tabla necesaria y se almacena procedimientos.

### <a name="viewing-the-error-details-page-on-production"></a>Ver la página de detalles del Error en producción

Después de implementar el sitio de producción, visite el sitio Web de producción y generar una excepción no controlada. Como se muestra en el entorno de desarrollo ELMAH no tiene ningún efecto en la página de error aparece cuando se produce una excepción no controlada; en su lugar, simplemente registra el error. Si intentas visitar la página de registro de errores (`elmah.axd`) desde el entorno de producción, aparecerá la página prohibido se muestra en **figura 6**.

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**Figura 6**: de forma predeterminada, los visitantes remotos no pueden ver la página Web de registro de errores  
([Haga clic aquí para ver imagen en tamaño completo](logging-error-details-with-elmah-vb/_static/image16.png))

Recuerde que en la configuración de ELMAH `<security>` sección establecemos el `allowRemoteAccess` en 0, lo que prohíbe a los usuarios remotos puedan ver el registro de errores de atributo. Es importante prohibir los visitantes anónimos puedan ver el registro de errores, como los detalles del error pueden revelar las vulnerabilidades de seguridad u otra información confidencial. Si decide establecer este atributo en 1 y habilitar el acceso remoto al registro de errores, es importante para bloquear el `elmah.axd` ruta de acceso para que sólo los visitantes autorizados tener acceso a él. Esto puede lograrse mediante la adición de un `<location>` elemento a la `Web.config` archivo.

La siguiente configuración permite que solo los usuarios en el rol de administrador para tener acceso a la página web de registro de error:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> El rol de administrador y los tres usuarios en el sistema - Scott, Alicia y Jisun - se agregaron en el [ *configurar un sitio Web que usa servicios de aplicación* tutorial](configuring-a-website-that-uses-application-services-vb.md). Los usuarios Scott y Jisun son miembros del rol de administrador. Para obtener más información sobre la autenticación y autorización, consulte mi [tutoriales de seguridad del sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md).


El registro de errores en el entorno de producción ahora se puede ver los usuarios remotos; Vuelva a consultar **figuras 3**, **4**, y **5** para capturas de pantalla de la página web de registro de error. Sin embargo, si un usuario anónimo o que no es administrador intenta ver la página de registro de errores se redirijan automáticamente a la página de inicio de sesión (`Login.aspx`), como **figura 7** muestra.

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**Figura 7**: los usuarios no autorizados son redirigirá automáticamente a la página de inicio de sesión  
([Haga clic aquí para ver imagen en tamaño completo](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>Registro de errores mediante programación

Del ELMAH `ErrorLogModule` módulo HTTP registra automáticamente las excepciones no controladas en el origen de registro especificado. Como alternativa, puede registrar un error sin tener que provocan una excepción no controlada mediante la `ErrorSignal` clase y su `Raise` método. El `Raise` se pasa al método un `Exception` objeto y lo registra como si esa excepción se ha iniciado y había alcanzado el tiempo de ejecución ASP.NET sin que se está controlando. Sin embargo, la diferencia es que la solicitud continúa ejecutando con normalidad después el `Raise` se ha llamado al método, mientras que una excepción no controlada producida interrumpe la ejecución normal de la solicitud y hace que el tiempo de ejecución ASP.NET mostrar la configurada página de error.

La `ErrorSignal` clase es útil en situaciones donde hay alguna acción que puede producir un error, pero el error no es grave para realizar la operación global. Por ejemplo, un sitio Web puede contener un formulario que toma la entrada del usuario, lo almacena en una base de datos y, a continuación, envía al usuario un correo electrónico que les informa de que se procesó la información. ¿Qué debe ocurrir si la información se guarda en la base de datos correctamente, pero hay un error al enviar el mensaje de correo electrónico? Una opción sería iniciar una excepción y enviar al usuario a la página de error. Sin embargo, esto podría confundir al usuario haciéndole creer que no se ha guardado la información que especificó. Otro enfoque sería el error relacionado con el correo electrónico de registro, pero no modificar la experiencia del usuario en modo alguno. Aquí es donde el `ErrorSignal` clase es útil.

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-email"></a>Error de notificación por correo electrónico

Junto con el registro de errores para una base de datos, también puede configurarse ELMAH detalles del error de correo electrónico a un destinatario especificado. Esta funcionalidad se proporciona mediante el `ErrorMailModule` módulo HTTP; por lo tanto, debe registrar este módulo HTTP en `Web.config` con el fin de enviar por correo electrónico los detalles del error.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

A continuación, especifique información sobre el correo electrónico de error en la `<elmah>` del elemento `<errorMail>` sección, que indica el correo de electrónico remitente y destinatario, el asunto, y si el correo electrónico se envía de forma asincrónica.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

Con la configuración anterior en su lugar, siempre que un error en tiempo de ejecución se produce ELMAH envía un correo electrónico a support@example.com con los detalles del error. Correo electrónico de error de ELMAH incluye la misma información que se muestra en la página web detalles de error, es decir, el mensaje de error, el seguimiento de pila y las variables de servidor (vuelva a consultar **figuras 4** y **5**). El correo electrónico de error también incluye el contenido de la excepción detalles de la pantalla amarilla de muerte como datos adjuntos (`YSOD.html`).

**Figura 8** muestra el correo electrónico de error de ELMAH generado visitando `Genre.aspx?ID=foo`. Mientras **figura 8** muestra sólo el mensaje y la pila de seguimiento de errores, las variables de servidor se incluyen más abajo en el cuerpo del correo electrónico.

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**Figura 8**: puede configurar ELMAH para enviar por correo electrónico los detalles del Error  
([Haga clic aquí para ver imagen en tamaño completo](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>Solo registro de errores de interés

De forma predeterminada, ELMAH registra los detalles de todas las excepciones no controladas, incluidas 404 y otros errores HTTP. Puede indicar a ELMAH para pasar por alto estos u otros tipos de errores mediante el filtrado de error. La lógica de filtrado se realiza mediante de ELMAH `ErrorFilterModule` módulo HTTP, que necesitará para registrar en `Web.config` para poder usar la lógica de filtrado. Las reglas de filtrado se especifican en la `<errorFilter>` sección.

El marcado siguiente indica a ELMAH para no registrar 404 errores.

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> No se olvide, con el fin de usar el filtrado de error debe registrar el `ErrorFilterModule` módulo HTTP.


El `<equal>` elemento dentro de la `<test>` sección se conoce como una aserción. Si la aserción se evalúa como true, a continuación, se filtra el error de registro del ELMAH. Hay disponibles, incluidas otras aserciones: `<greater>`, `<greater-or-equal>`, `<not-equal>`, `<lesser>`, `<lesser-or-equal>`, y así sucesivamente. También puede combinar las aserciones mediante el `<and>` y `<or>` operadores booleanos. Además, incluso puede incluir una expresión de JavaScript simple como una aserción o escribir sus propias aserciones en C# o Visual Basic.

Para obtener más información sobre el error de ELMAH las capacidades de filtrado, consulte el [sección filtrar errores](https://code.google.com/p/elmah/wiki/ErrorFiltering) en el [ELMAH wiki](https://code.google.com/p/elmah/w/list).

## <a name="summary"></a>Resumen

ELMAH proporciona un mecanismo sencillo y eficaz para el registro de errores en una aplicación web ASP.NET. Al igual que el sistema de supervisión de estado de Microsoft, ELMAH puede registrar errores en una base de datos y puede enviar los detalles del error a un desarrollador a través de correo electrónico. A diferencia de la supervisión de estado de sistema, ELMAH incluye soporte para una amplia variedad de almacenes de datos de registro de errores, incluidas inmediato: Microsoft SQL Server, Microsoft Access, Oracle, archivos XML entre otras. Además, ELMAH ofrece un mecanismo integrado para ver el registro de errores y detalles sobre un error específico de una página web, `elmah.axd`. El `elmah.axd` página también puede procesar la información de error como una fuente RSS o como un archivo de valores separados por comas (CSV), que puede leer con Microsoft Excel. También puede indicar a ELMAH para filtrar errores del registro mediante las aserciones declarativas o mediante programación. Y ELMAH puede usarse con aplicaciones de ASP.NET 1.x.

Todas las aplicaciones implementadas deben tener algún mecanismo para registrar automáticamente las excepciones no controladas y enviar notificación al equipo de desarrollo. Si esto se logra mediante la supervisión de estado o ELMAH es secundaria. En otras palabras, no importa realmente mucho si se utiliza la supervisión de estado o ELMAH; Evalúe ambos sistemas y, a continuación, elija la que mejor se adapte a sus necesidades. ¿Qué es fundamentalmente importante es que algún mecanismo de colocarse en su lugar para registrar excepciones no controladas en el entorno de producción.

Feliz programación.

### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [ELMAH - módulos de registro de Error y controladores](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [Página del proyecto ELMAH](https://code.google.com/p/elmah/) (origen de código, ejemplos, wiki)
- [Es decir, sería ELMAH en una aplicación Web para detectar las excepciones no controladas](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx) (vídeo)
- [Páginas de registro de errores de seguridad](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [Uso de módulos y controladores HTTP para crear componentes ASP.NET conectables](https://msdn.microsoft.com/library/aa479332.aspx)
- [Tutoriales de seguridad del sitio Web](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [Anterior](logging-error-details-with-asp-net-health-monitoring-vb.md)
> [Siguiente](precompiling-your-website-vb.md)
