---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: ASP.NET y Web Tools 2013.2 para notas de la versión de Visual Studio 2013 | Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 03/06/2014
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: fb36d9f469265be60a7e40ed7e317d8da6560bf9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824502"
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET y Web Tools 2013.2 para notas de la versión de Visual Studio 2013
====================
por [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Notas de instalación

ASP.NET y Web Tools para Visual Studio 2013.2 están agrupados en el programa de instalación principal y se pueden descargar como parte de [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentación

Tutoriales y otra información acerca de ASP.NET y Web Tools para Visual Studio 2013.2 están disponibles desde el [sitio web de ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Requisitos de software

ASP.NET y Web Tools para Visual Studio 2013.2 requiere Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nuevas características de ASP.NET y Web Tools para Visual Studio 2013.2

Las siguientes secciones describen las características que se han introducido en la versión.

- [Plantillas de un proyecto de ASP.NET](#oneaspnet)
- [Admite SSL al inicio de aplicaciones Web en IIS Express](#ssl)
- [Mejoras del Editor Web de Visual Studio](#vswebeditor)
- [Vínculo con exploradores](#browserlink)
- [Soporte técnico para Azure App Service Web Apps en Visual Studio](#waws)
- [Crear recursos remotos de Azure al crear un nuevo proyecto Web](#AzureResources)
- [Mejoras de publicación Web](#webpublish)
- [Scaffolding de ASP.NET](#scaffolding)
- [NuGet 2.8.1](#nuget)
- [Formularios Web Forms ASP.NET](#webforms)
- [ASP.NET MVC 5.1.2](#mvc)
- [ASP.NET Web API 2.1.2](#webapi)
- [3.1.2 de ASP.NET Web Pages](#webpages)
- [Entity Framework 6.1](#ef)
- [Identidad de ASP.NET 2.0.0](#identity)
- [Componentes de Microsoft OWIN](#owin)
- [ASP.NET SignalR 2.0.2](#signalr)

<a id="oneaspnet"></a>
### <a name="one-aspnet-project-templates"></a>Plantillas de un proyecto de ASP.NET

- Actualizaciones a las plantillas de proyecto de ASP.NET para admitir la confirmación de la cuenta y restablecimiento de contraseña.
- Actualizar plantilla de ASP.NET Web API para admitir la autenticación mediante en localmente las cuentas organizativas.
- La plantilla de SPA de ASP.NET ahora contiene la autenticación basada en vistas MVC y el servidor. La plantilla tiene un controlador WebAPI que solo se puede acceder a los usuarios autenticados.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Admite SSL al inicio de aplicaciones Web en IIS Express

Para eliminar la advertencia de seguridad al explorar y depurar HTTPS en localhost, hemos agregado un cuadro de diálogo para permitir que Internet Explorer y Chrome confiar autofirmado IIS express certificado SSL.

Por ejemplo, se puede establecer una propiedad de proyecto web para usar SSL. Pulse F4 para que aparezca el cuadro de diálogo de propiedades. Cambio **SSL habilitado** en true. Copie la dirección URL de SSL.

![Propiedad Enabled de SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Conjunto de la pestaña web proyecto propiedad página web para utilizar HTTPS en función de dirección URL (la dirección URL de SSL será `https://localhost:44300/` a menos que haya creado previamente los sitios Web SSL.)

![Establecer la dirección URL del proyecto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Presione CTRL+F5 para ejecutar la aplicación. Siga las instrucciones para confiar en el certificado autofirmado que ha generado IIS Express.

![Advertencia de SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Leer el **advertencia de seguridad** cuadro de diálogo y, a continuación, haga clic en **Sí** si desea instalar el certificado que representa el host local.

![Advertencia de seguridad](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

El sitio se mostrará en Internet Explorer o Chrome sin la advertencia de certificado en el explorador.

![Página HTTPS sin advertencias](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox usa su propio almacén de certificados, por lo que mostrará una advertencia.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Mejoras del Editor Web de Visual Studio

- **Nuevo elemento de proyecto JSON y editor**: hemos agregado un elemento de proyecto JSON y el editor de Visual Studio. Las características del editor de JSON actuales incluyen coloración, validación de la sintaxis, finalización de llave, esquematización, configuración de la opción de herramientas y mucho más.

    ![Editor de JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense ahora admite [esquema JSON](http://json-schema.org/) v3 y v4. Hay un cuadro combinado de esquema para elegir los esquemas existentes, edite la ruta de acceso local del esquema, o simplemente arrastrar y colocar un archivo JSON del proyecto a ella para obtener la ruta de acceso relativa.

    ![Intellisense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![Editor de esquemas de JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nuevo editor Sass (SCSS)**: hemos agregado menor en VS2013 RTM, y ahora tenemos un elemento de proyecto Sass y el editor. Editor de SASS características son comparables en el editor LESS e incluyen coloración, variable y Mixins de IntelliSense, comentario o quite, información rápida, formato, validación de la sintaxis, esquematización, ir a definición, el selector de colores, las herramientas de configuración de la opción etcetera.

    ![Agregar nuevo elemento: Hoja de estilos SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor de hojas de estilo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nuevo selector de URL en HTML, Razor, CSS, menos y documentos Sass:** VS 2013 incluyó ningún selector de direcciones URL fuera de las páginas de formularios Web Forms. El nuevo selector de URL para HTML, Razor, CSS, LESS y Sass editores es un selector de escritura fluido, libre de cuadro de diálogo que entiende '..' y archivo de filtros se muestra correctamente para obtener vínculos y etiquetas img.

    ![Selector de direcciones URL para la etiqueta de imagen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Selector de direcciones URL para las vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Selector de direcciones URL para CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Actualizaciones de editor LESS agregando más características**
- **Actualización de Intellisense de knockout**: se ha agregado una sintaxis no estándar de KnockOut para intelliSense de VS, "ko-vs-editor viewModel:" sintaxis. Se puede usar para enlazar a varios modelos de vista en una página con comentarios en el formulario:

    ![Intellisense de knockout](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    También hemos agregado compatibilidad con ViewModel IntelliSense anidados, por lo que puede profundizar en objetos profundamente anidados en el modelo de vista.

    `<div data-bind="text: foo.bar.baz.etc" />`

    La IntelilSense muestra es la funcionalidad IntelliSense completa del objeto JavaScript.

    ![Objeto de JavaScript completo de IntelliSense que muestra](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nuevo selector de URL en HTML, Razor, CSS, menos y documentos Sass**: VS 2013 incluyó ningún selector de direcciones URL fuera de las páginas de formularios Web Forms. El nuevo selector de URL para HTML, Razor, CSS, LESS y Sass editores es un selector de escritura fluido, libre de cuadro de diálogo que entiende '..' y archivo de filtros se muestra correctamente para obtener vínculos y etiquetas img.

    ![Selector de direcciones URL para la etiqueta de imagen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Selector de direcciones URL para las vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Selector de direcciones URL para CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Vínculo de explorador

- Vínculo de explorador ahora admite conexiones HTTPS y mostrará en el panel con otras conexiones siempre que el certificado es de confianza mediante el explorador.
- Asignación del código fuente HTML estático
- Compatibilidad con datos de asignación de SPA
- Datos de asignación de actualización automática

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Soporte técnico para Azure App Service Web Apps en Visual Studio

- **Compatibilidad con Azure inicie sesión.**
- **Vista remota para las aplicaciones web y depuración remota**: ahora se admite [depuración remota para aplicaciones web en Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) y vista remota de archivos de contenido de aplicación web en el Explorador de servidores.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Crear recursos remotos de Azure al crear un nuevo proyecto Web

Se ha agregado un Azure ["Crear recursos remotos"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) casilla de verificación en el cuadro de diálogo de aplicación web nueva. Al seleccionarlo, podrá integrar la experiencia de creación de una nueva aplicación web, configurar el sitio de publicación de Azure para probar y crear el perfil de publicación en unos pocos pasos sencillos.

![Nuevo proyecto con recursos de Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publicación en Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Mejoras de publicación Web

- Mejorar la experiencia del usuario para la publicación.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

- **Compatibilidad de enumeraciones:** si el modelo utiliza las enumeraciones, a continuación, el proveedor de scaffolding de MVC generará la lista desplegable para la enumeración. Esto usa la enumeración de las aplicaciones auxiliares de MVC.
- **Compatibilidad de arranque**: actualiza las plantillas de EditorFor de Scaffolding de MVC para que usen las clases de Bootstrap.
- **Paquete de soporte técnico**: MVC y Web API Scaffolders agregará 5.1 paquetes para MVC y Web API

Las capturas de pantalla siguientes muestran los modelos de scaffolding.

- Modelo de código:

     ![Código de modelo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compilar el código de modelo, contextual y seleccione **agregar**, **nuevo elemento de scaffolding**.

     ![Agregar nuevo elemento con scaffolding](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Elija **MVC5 controlador con vistas que usan Entity Framework**:

     ![Agregar nuevo controlador MVC5 con vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Agregar controlador** con el modelo:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Compruebe el código generado, por ejemplo Views/WeekdayModels/Edit.cshtml contiene `@Html.EnumDropDownListFor`: ![vista que contiene EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Ejecute la página para ver el cuadro combinado de enumeración generado, tenga en cuenta que si un valor puede ser null, se puede elegir una cadena vacía para el cuadro combinado. Por ejemplo, el **crear** página muestra lo siguiente:

    ![Cuadro combinado que permite una cadena vacía](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 que RTM se lanzará en abril de 2014. Estos son los puntos importantes de las notas de la versión, pero compruebe la [notas de la versión completa](http://docs.nuget.org/docs/release-notes/nuget-2.8) para obtener más información acerca de estos cambios.

- **Apuntar a las aplicaciones de Windows Phone 8.1**: NuGet 2.8.1 ahora admite dirigidos a aplicaciones de Windows Phone 8.1 mediante los monikers de la plataforma de destino 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' y 'WPA81'.
- **Revisión de resolución de dependencias**: al resolver las dependencias del paquete, NuGet históricamente ha implementado una estrategia de selección de la versión más antigua del paquete principal y secundaria que satisface las dependencias del paquete. A diferencia de la versión principal y secundaria, sin embargo, la versión de revisión se resuelve siempre a la versión más alta. Aunque el comportamiento era bien intencionado que involuntariamente, creó una falta de determinismo para instalar paquetes con dependencias.
- **El modificador DependencyVersion**: aunque NuGet 2.8 cambia el *predeterminada* comportamiento para resolver las dependencias, también agrega un control más preciso sobre el proceso de resolución de dependencia a través del modificador DependencyVersion - en el consola del Administrador de paquetes. El modificador permite resolver las dependencias a la versión más baja posible (comportamiento predeterminado), la versión más alta posible, o la mayor versión menor o revisión. Este modificador solo funciona para el paquete de instalación en el comando de powershell.
- **Atributo DependencyVersion**: el modificador DependencyVersion - detallado anteriormente, además de NuGet también ha permitido por la capacidad para establecer un nuevo atributo en el archivo nuget.config definir lo que es el valor predeterminado, si el modificador DependencyVersion - no se especifica en una invocación de install-package. Este valor también se respetan el cuadro de diálogo Administrador de paquetes de NuGet para las operaciones del paquete de instalación. Para establecer este valor, agregue el atributo siguiente al archivo nuget.config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Obtener una vista previa de las operaciones de NuGet con - whatif**: paquetes de NuGet algunas pueden tener gráficos de dependencia profunda y por lo tanto, puede resultar útil durante una instalación, desinstalar o para ver primero lo que ocurrirá la operación de actualización. NuGet 2.8 agrega el estándar de PowerShell: ¿qué ocurre si se va a los comandos install-package, paquete desinstalar y paquete de actualización para habilitar la visualización de la clausura completa de los paquetes a la que se aplicará el comando.
- **Cambiar el paquete**: no es raro que instale una versión preliminar de un paquete con el fin de investigar las nuevas características y, a continuación, decidir revertir a la última versión estable. Antes de NuGet 2.8, esto era un proceso de varios pasos de desinstalación del paquete preliminar y sus dependencias y, a continuación, instalar la versión anterior. Con NuGet 2.8, sin embargo, el paquete de actualización ahora revertirá al cierre del paquete completo (por ejemplo, el árbol de dependencias del paquete) a la versión anterior.
- **Las dependencias de desarrollo**: muchos tipos diferentes de las capacidades se pueden entregar como paquetes de NuGet - incluidas las herramientas que se usan para optimizar el proceso de desarrollo. No se deben considerar estos componentes, aunque pueden ser fundamental en el desarrollo de un nuevo paquete, publica una dependencia del paquete de nuevo cuando es una versión posterior. NuGet 2.8 habilita un paquete para identificarse en el archivo .nuspec como un developmentDependency. Cuando se instala, estos metadatos también se agregará al archivo packages.config del proyecto en el que se instaló el paquete. Cuando ese archivo packages.config se analiza más adelante para las dependencias de NuGet durante pack de nuget.exe, excluirá esas dependencias marcados como las dependencias de desarrollo.
- **Archivos packages.config individuales para diferentes plataformas**: al desarrollar aplicaciones para varias plataformas de destino, es habitual tener distintos archivos de proyecto para cada uno de los entornos de compilación respectivos. También es común para consumir paquetes de NuGet diferentes en distintos archivos de proyecto, como los paquetes tienen distintos niveles de compatibilidad para diferentes plataformas. NuGet 2.8 proporciona compatibilidad mejorada para este escenario mediante la creación de archivos packages.config diferentes para los archivos de proyecto específico de plataforma diferente.
- **Reserva de memoria caché Local**: paquetes de NuGet aunque normalmente se consumen desde una galería remota, como el [Galería de NuGet](http://www.nuget.org) mediante una conexión de red, hay muchos escenarios donde el cliente no está conectado. Sin una conexión de red, el cliente de NuGet no pudo instalar correctamente paquetes - incluso cuando esos paquetes ya estaban en el equipo cliente en la caché local de NuGet. NuGet 2.8 agrega almacenamiento en caché automático reserva a la consola del Administrador de paquetes.

    La característica de reserva de caché no requiere ningún argumento de comando específico. Además, la reserva de caché actualmente solo funciona en la consola del Administrador de paquetes: el comportamiento no funciona actualmente en el cuadro de diálogo del Administrador de paquetes.
- **Correcciones de errores**: una de las correcciones de errores principales realizadas fue mejora del rendimiento en el paquete de actualización-volver a instalar el comando.

    Además de estas características y la corrección de rendimiento mencionados anteriormente, esta versión de NuGet también incluye muchas otras correcciones de errores. Se produjeron 181 total de problemas solucionado en la versión. Para obtener una lista completa de los trabajos elementos corregidos en NuGet 2.8, por favor, ver el [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) para esta versión.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Formularios Web Forms de ASP.NET

- Las plantillas de formularios Web Forms ahora muestran cómo realizar la confirmación de la cuenta y restablecimiento de contraseña para la identidad de ASP.NET.
- El control de origen de datos de entidad y el proveedor de datos dinámico para Entity Framework 6. Para obtener más información, consulte el blog siguiente de MSDN: [proveedor de datos dinámicos y control EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Mejoras de enrutamiento de atributo](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Compatibilidad de arranque para plantillas de editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Compatibilidad de enumeraciones en las vistas](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Compatibilidad con Unobstrusive MinLength / MaxLength atributos](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Compatibilidad con el contexto 'this' en Ajax discreto](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Diversos [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Control de errores global](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Mejoras de enrutamientos de atributo](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Mejoras en la página de ayuda](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Compatibilidad con IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formateador de tipo de medio BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Mejor compatibilidad para los filtros de asincronía](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Consulta de análisis para el cliente de biblioteca de formato](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Diversos [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>3.1.2 de ASP.NET Web Pages

- Diversos [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Se actualizó a la versión 6.1 de tiempo de ejecución y las herramientas de Entity Framework. Entity Framework (EF) 6.1 es una actualización secundaria de Entity Framework 6 y se incluye una serie de correcciones de errores y nuevas características. Para obtener información detallada sobre EF6.1, incluidos vínculos a documentación para las nuevas características, consulte [historial de versiones de Entity Framework](https://msdn.microsoft.com/data/jj574253). Las nuevas características en esta versión incluyen:

- **Las herramientas de consolidación** proporciona una manera coherente para crear un nuevo modelo EF. Esta característica amplía el Asistente de Entity Data Model de ADO.NET para admitir la creación de modelos de Code First, como la ingeniería inversa desde una base de datos existente. Estas características estaban disponibles en la calidad de la versión Beta en EF Power Tools.
- **Control de errores de confirmación de transacción** proporciona el nuevo [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) que hace uso de la capacidad recién introducida para interceptar las operaciones de transacción. El **CommitFailureHandler** permite la recuperación automática de errores de conexión al confirmar una transacción.
- **IndexAttribute** permite que los índices que se especifique mediante la colocación de un atributo en una propiedad (o propiedades) en el modelo de Code First. Código en primer lugar, a continuación, creará un índice correspondiente en la base de datos.
- **La API pública de la asignación** proporciona acceso a la información de EF en cómo se asignan los tipos y propiedades a columnas y tablas en la base de datos. En versiones anteriores, esta API era interna.
- **Capacidad de configurar los interceptores mediante el archivo App/Web.config**(lo que permite los interceptores agregarse sin volver a compilar la aplicación).
- **DatabaseLogger** es un nuevo interceptor que facilita la tarea iniciar todas las operaciones de base de datos en un archivo. En combinación con la característica anterior, esto le permite cambiar fácilmente el registro de operaciones de base de datos para una aplicación implementada, sin necesidad de volver a compilar.
- **Detección de cambios de modelo de migraciones** se ha mejorado para que sean más precisas; rendimiento del proceso de detección de cambios también se ha mejorado en gran medida las migraciones con scaffolding.
- **Mejoras de rendimiento** incluidas las operaciones de reducción de la base de datos durante la inicialización, las optimizaciones para la comparación de igualdad null en las consultas LINQ, más rápido ver generación (creación de modelos) en otros escenarios y más eficaz materialización de las entidades registradas con varias asociaciones.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>Identidad de ASP.NET 2.0.0

- **Autenticación en dos fases**: ASP.NET Identity ahora es compatible con la autenticación en dos fases. Autenticación en dos fases proporciona una capa adicional de seguridad para las cuentas de usuario en el caso de que está en peligro la contraseña. También hay protección para los ataques de fuerza bruta contra los códigos de dos fases.
- **Bloqueo de cuenta:** proporciona una manera de bloquear al usuario si el usuario escribe su contraseña o códigos de dos factores de forma incorrecta. El número de intentos no válidos y el intervalo de tiempo para los usuarios están bloqueados se puede configurar. Un desarrollador puede, opcionalmente, desactivar el bloqueo de cuenta para determinadas cuentas de usuario que necesitan para.
- **Confirmación de cuenta:** el sistema ASP.NET Identity ahora es compatible con la confirmación de la cuenta. Se trata de un escenario muy común en la mayoría de sitios Web hoy mismo donde, al registrar una nueva cuenta en el sitio Web, que es necesarias para confirmar su correo electrónico antes de hacer cualquier cosa en el sitio Web. Confirmación por correo electrónico es útil porque evita que las cuentas ficticias que se está creando. Esto es muy útil si utilizas el correo electrónico como un método de comunicación con los usuarios de su sitio Web como sitios de foro, banca, comercio electrónico o sitios web sociales.
- **Restablecimiento de contraseña:** restablecimiento de contraseña es una característica que el usuario puede restablecer sus contraseñas si ha olvidado su contraseña.
- **El sello de seguridad (en todas partes de cierre de sesión):** admite una forma de volver a generar el Token de seguridad para el usuario en los casos, cuando el usuario cambia la contraseña o cualquier otra seguridad relacionados con la información como la eliminación de un inicio de sesión asociado (por ejemplo, Facebook, Google, Cuenta de Microsoft y así sucesivamente). Esto es necesario para asegurarse de que los tokens generados con la contraseña antigua se invalidan. En el proyecto de ejemplo, si cambia la contraseña del usuario, a continuación, se genera un nuevo token para el usuario y se invalida cualquier token anterior. Esta característica proporciona una capa adicional de seguridad a la aplicación desde al cambiar la contraseña, le cerrará en todas partes (todos los demás exploradores) donde haya iniciado sesión en esta aplicación.
- **Convierta el tipo de clave principal sea extensible para los usuarios y Roles**: en ASP.NET Identity 1.0, el tipo de clave principal de la tabla que los usuarios y Roles era cadenas. Esto significa que cuando el sistema ASP.NET Identity se mantuvo en SQL Server mediante el uso de Entity Framework, usábamos nvarchar. Había muchas discusiones en torno a esta implementación predeterminada en Stack Overflow y basándose en las respuestas entrantes. Hemos proporcionado un enlace de extensibilidad donde puede especificar cuál debe ser la clave principal de la tabla de usuarios y Roles. Este enlace de la extensibilidad es especialmente útil que si va a migrar la aplicación y la aplicación estaba almacenar identificadores de usuario son enteros o GUID.
- **Admitir IQueryable en usuarios y Roles**: se agregó compatibilidad para IQueryable en UsersStore y RolesStore, puede obtener fácilmente la lista de usuarios y Roles.
- **Operación de eliminación de soporte técnico a través de UserManager**
- **Indexación en nombre de usuario**: implementación de ASP.NET Identity Entity Framework, hemos agregado un índice único en el nombre de usuario mediante el uso de la nueva IndexAttribute en EF 6.1.0. Esto garantiza que los nombres de usuario siempre son únicos y no había ninguna condición de carrera en el que puede acabar con nombres de usuario duplicados.
- **Validador de contraseña mejorada:** el validador de contraseña que se envió en ASP.NET Identity 1.0 era un validador de contraseña bastante básica que sólo se valida la longitud mínima. Hay un validador de contraseña nueva que le ofrece más control sobre la complejidad de la contraseña. Tenga en cuenta que aunque activar todas las configuraciones de esta contraseña, le animamos a habilitar la autenticación en dos fases para las cuentas de usuario.
- **IdentityFactory Middleware / CreatePerOwinContext**:

    - **El Administrador de usuarios**: implementación de generador puede usar para obtener una instancia de UserManager desde el contexto OWIN. Este patrón es similar a lo que se usan para obtener AuthenticationManager de contexto OWIN para el inicio de sesión y cierre de sesión. Se trata de una manera recomendada de obtención de una instancia de UserManager por solicitud para la aplicación.
    - **DbContextFactory**: ASP.NET Identity utiliza Entity Framework para conservar el sistema de identidad en SQL Server. Para ello, el sistema de identidad tiene una referencia a la ApplicationDbContext. El DbContextFactory Middleware devuelve una instancia de la ApplicationDbContext por solicitud que puede usar en la aplicación.
- **Paquete de NuGet de ejemplos de identidad ASP.NET**: paquete de NuGet de los ejemplos puede hacer más fácil de instalar y ejecutar los ejemplos de ASP.NET Identity y siga las prácticas recomendadas. Esto es un ejemplo de aplicación de ASP.NET MVC. Modifique el código para que se adapte a su aplicación antes de implementar esto en producción. El ejemplo debe instalarse en una aplicación ASP.NET vacía. Para obtener más información sobre el paquete, vaya a la siguiente entrada de blog: [anuncio RTM de ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componentes de Microsoft OWIN

Hubo una gran cantidad de errores corregidos en esta versión. Consulte la [notas de la 2.1.0 versión](https://katanaproject.codeplex.com/releases/view/113281) para obtener más información.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

Hubo una gran cantidad de errores corregidos en esta versión. Consulte la [notas de la 2.0.2 versión](https://github.com/SignalR/SignalR/releases/tag/2.0.2) para obtener más información.
