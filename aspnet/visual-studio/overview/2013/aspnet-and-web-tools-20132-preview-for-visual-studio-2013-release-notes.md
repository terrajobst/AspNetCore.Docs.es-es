---
uid: visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
title: 2013.2 para Visual Studio 2013 consultar las notas de las herramientas de ASP.NET y Web | Documentos de Microsoft
author: microsoft
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/06/2014
ms.topic: article
ms.assetid: 7ef5f73c-ca60-43c1-bdb2-702800347e7e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 0e7ad52662f7ceaa1f087d007d0b14b610f90bee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-and-web-tools-20132--for-visual-studio-2013-release-notes"></a>ASP.NET y herramientas Web 2013.2 para notas de versión de Visual Studio 2013
====================
por [Microsoft](https://github.com/microsoft)

## <a name="installation-notes"></a>Notas sobre la instalación

ASP.NET y herramientas Web de Visual Studio 2013.2 están agrupados en el programa de instalación principal y se puede descargar como parte de [Visual Studio 2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521).

## <a name="documentation"></a>Documentación

Tutoriales y otra información acerca de ASP.NET y herramientas Web de Visual Studio 2013.2 están disponibles en la [sitio web de ASP.NET](https://www.asp.net/).

## <a name="software-requirements"></a>Requisitos de software

ASP.NET y herramientas Web de Visual Studio 2013.2 requiere Visual Studio 2013.

## <a name="new-features-in-aspnet-and-web-tools-for-visual-studio-20132"></a>Nuevas características de ASP.NET y herramientas Web de Visual Studio 2013.2

En las siguientes secciones se describen las características que se han introducido en la versión.

- [Plantillas de un proyecto de ASP.NET](#oneaspnet)
- [Admite SSL al iniciar aplicaciones Web en IIS Express](#ssl)
- [Mejoras del Editor de Visual Studio Web](#vswebeditor)
- [Vínculo con exploradores](#browserlink)
- [Compatibilidad con aplicaciones de Azure de aplicación de servicio Web en Visual Studio](#waws)
- [Crear recursos de Azure remotos al crear un nuevo proyecto Web](#AzureResources)
- [Mejoras de publicación de Web](#webpublish)
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
- Actualizar plantilla de ASP.NET Web API para admitir la autenticación mediante en local las cuentas organizativas.
- La plantilla de ASP.NET SPA ahora contiene autenticación que se basa en las vistas de lado MVC y el servidor. La plantilla tiene un controlador de WebAPI que solo se puede tener acceso a los usuarios autenticados.

<a id="ssl"></a>
### <a name="support-ssl-when-launching-web-applications-on-iis-express"></a>Admite SSL al iniciar aplicaciones Web en IIS Express

Para eliminar la advertencia de seguridad al explorar y depurar HTTPS en localhost, hemos agregado un cuadro de diálogo para permitir que Internet Explorer y Chrome confiar autofirmado IIS express certificado SSL.

Por ejemplo, se puede establecer una propiedad de proyecto web para usar SSL. Haga clic en F4 para que aparezca el cuadro de diálogo de propiedades. Cambio **SSL habilitado** en true. Copie la dirección URL de SSL.

![Propiedad Enabled de SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image1.png)

Conjunto la pestaña web proyecto propiedad página web para usar HTTPS según la dirección URL (será la dirección URL de SSL `https://localhost:44300/` a menos que haya creado previamente los sitios Web SSL.)

![Establecer la dirección URL del proyecto (HTTPS)](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image2.png)

Presione CTRL+F5 para ejecutar la aplicación. Siga las instrucciones para confiar en el certificado autofirmado que generó el IIS Express.

![Advertencia de SSL](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image3.png)

Leer la **advertencia de seguridad** cuadro de diálogo y, a continuación, haga clic en **Sí** si desea instalar el certificado que representa el host local.

![Advertencia de seguridad](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image4.png)

El sitio se mostrará en Internet Explorer o Chrome sin la advertencia de certificado en el explorador.

![Página HTTPS sin advertencias](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image5.png)

Firefox usa su propio almacén de certificados, por lo que mostrará una advertencia.

<a id="vswebeditor"></a>
### <a name="visual-studio-web-editor-enhancements"></a>Mejoras del Editor de Visual Studio Web

- **Nuevo elemento de proyecto JSON y editor**: hemos agregado un elemento de proyecto JSON y el editor de Visual Studio. Características del editor de JSON actuales incluyen coloración, validación de la sintaxis, finalización de llave, esquematización, opción de herramientas y mucho más.

    ![Editor de JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image6.png)

    IntelliSense ahora admite [esquema JSON](http://json-schema.org/) v3 y v4. Hay un cuadro combinado de esquema para elegir esquemas existentes, edite la ruta de acceso local del esquema, o simplemente arrastrar un archivo de proyecto JSON en él para obtener la ruta de acceso relativa.

    ![Intellisense JSON](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image7.png)    ![JSON Schema editor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image8.png)
- **Nuevo editor de Sass (SCSS)**: hemos agregado menor en VS2013 RTM y ahora tenemos un elemento de proyecto Sass y el editor. Editor de SASS características son comparables en el editor de LESS e incluyen coloración, variable y Mixins IntelliSense, comentario o quite, información rápida, formato, validación de la sintaxis, la esquematización, ir a definición, selector de color, las herramientas de configuración de la opción etcetera.

    ![Agregar nuevo elemento: Hoja de estilos SCSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image9.png)    ![Editor de la hoja de estilos](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image10.png)
- **Nuevo selector de direcciones URL en HTML, Razor, CSS, menos y documentos de Sass:** VS 2013 incluidos con ningún selector de direcciones URL fuera de las páginas de formularios Web Forms. El nuevo selector de direcciones URL para HTML, Razor, CSS, menos y Sass editores es un selector de tipo fluido, libre de cuadro de diálogo que entienda '..' y archivo de filtros se enumera adecuadamente para vínculos y etiquetas img.

    ![Selector de direcciones URL para la etiqueta de imagen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image11.png)    ![Selector de direcciones URL para las vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image12.png)    ![Selector de direcciones URL para CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image13.png)
- **Actualizaciones al menos editor agregando más características**
- **Knockout Intellisense actualización**: hemos agregado una sintaxis de cobertura no estándar para intelliSense de VS, "ko-vs-editor viewModel:" sintaxis. Se puede utilizar para enlazar a varios modelos de vista en una página de la utilización de comentarios en el formulario:

    ![Intellisense de cobertura](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image14.png)

    También hemos agregado compatibilidad con ViewModel IntelliSense anidados, por lo que puede profundizar en los objetos profundamente anidados en el modelo de vista.

    `<div data-bind="text: foo.bar.baz.etc" />`

    La IntelilSense muestra es el IntelliSense completo del objeto de JavaScript.

    ![Objeto de JavaScript completo de IntelliSense que muestra](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image15.png)
- **Nuevo selector de direcciones URL en HTML, Razor, CSS, menos y documentos de Sass**: VS 2013 incluidos con ningún selector de direcciones URL fuera de las páginas de formularios Web Forms. El nuevo selector de direcciones URL para HTML, Razor, CSS, menos y Sass editores es un selector de tipo fluido, libre de cuadro de diálogo que entienda '..' y archivo de filtros se enumera adecuadamente para vínculos y etiquetas img.

    ![Selector de direcciones URL para la etiqueta de imagen](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image16.png)    ![Selector de direcciones URL para las vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image17.png)    ![Selector de direcciones URL para CSS](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image18.png)

<a id="browserlink"></a>
### <a name="browser-link"></a>Vínculo de explorador

- Vínculo de explorador ahora es compatible con las conexiones HTTPS y enumerará en el panel junto con otras conexiones siempre y cuando el certificado es de confianza para el explorador.
- Asignación del código fuente HTML estático
- SPA admite para los datos de asignación
- Datos de asignación de actualización automática

<a id="waws"></a>
### <a name="support-for-azure-app-service-web-apps-in-visual-studio"></a>Compatibilidad con aplicaciones de Azure de aplicación de servicio Web en Visual Studio

- **Compatibilidad con Azure inicia sesión en.**
- **Depuración remota y vista remota para las aplicaciones web**: ahora se admite [depuración remota para las aplicaciones web en el servicio de aplicación de Azure](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) y vista remota de archivos de contenido de aplicación web en el Explorador de servidores.

<a id="AzureResources"></a>
### <a name="create-remote-azure-resources-when-creating-a-new-web-project"></a>Crear recursos de Azure remotos al crear un nuevo proyecto Web

Hemos agregado un Azure ["Crear recursos remotos"](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) casilla de verificación en el cuadro de diálogo de aplicación web nueva. Al seleccionarlo, podrá integrar la experiencia de creación de una nueva aplicación web, configurar el sitio de publicación de Azure para las pruebas y crear el perfil de publicación en unos pocos pasos sencillos.

![Nuevo proyecto con recursos de Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image19.png)![Publicación en Azure](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image20.png)

<a id="webpublish"></a>
### <a name="web-publish-enhancements"></a>Mejoras de publicación de Web

- Mejorar la experiencia del usuario para la publicación.

<a id="scaffolding"></a>
### <a name="aspnet-scaffolding"></a>Scaffolding de ASP.NET

- **Compatibilidad de enumeraciones:** si el modelo utiliza las enumeraciones, entonces el Scaffolder MVC generará la lista desplegable de enumeración. Las aplicaciones auxiliares de enumeración se utiliza en MVC.
- **Arrancar soporte**: actualizar las plantillas de EditorFor en MVC Scaffolding para que usen las clases de arranque.
- **Paquete de soporte técnico**: MVC y procesos scaffolding de Web API agregará 5.1 paquetes para MVC y la API de Web

Las capturas de pantalla siguientes muestran los modelos de scaffolding.

- Código de modelo:

     ![Código de modelo](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image21.png)
- Compilar el código de modelo, menú contextual y seleccione **agregar**, **nuevo elemento de scaffolding**.

     ![Agregar nuevo elemento con scaffolding](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image22.png)
- Elija **MVC5 controlador con vistas que usan Entity Framework**:

     ![Agregar nuevo controlador MVC5 con vistas](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image23.png)
- **Agregar controlador** utilizando el modelo:

    ![](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image24.png)
- Compruebe el código generado, por ejemplo contiene Views/WeekdayModels/Edit.cshtml `@Html.EnumDropDownListFor`: ![ver que contiene EnumDropDownListFor](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image25.png)
- Ejecute la página para ver el combobox enum generado, tenga en cuenta que si un valor puede ser null, se puede elegir una cadena vacía para el cuadro combinado. Por ejemplo, el **crear** página muestra lo siguiente:

    ![Cuadro combinado que permite una cadena vacía](aspnet-and-web-tools-20132-preview-for-visual-studio-2013-release-notes/_static/image26.png)

<a id="nuget"></a>
### <a name="nuget-281"></a>NuGet 2.8.1

NuGet 2.8.1 que RTM se publicarán en abril de 2014. Estos son los puntos importantes de las notas de la versión, pero compruebe el [notas de la versión completa](http://docs.nuget.org/docs/release-notes/nuget-2.8) para obtener más información sobre estos cambios.

- **Apuntar a las aplicaciones de Windows Phone 8.1**: NuGet 2.8.1 ahora es compatible con aplicaciones de Windows Phone 8.1 mediante los monikers de framework de destino 'WindowsPhoneApp', 'WPA', 'WindowsPhoneApp81' y 'WPA81' de destino.
- **Resolución de revisión para las dependencias**: al resolver las dependencias de paquetes, NuGet históricamente ha implementado una estrategia de la selección de la versión más antigua de paquete principal y secundaria que satisface las dependencias del paquete. A diferencia de la versión principal y secundaria, sin embargo, la versión de revisión se resuelve siempre la versión más alta. Aunque el comportamiento era dañino, crea una falta de determinismo para instalar paquetes con dependencias.
- **Conmutador de DependencyVersion**: aunque NuGet 2.8 cambia el *predeterminado* comportamiento para resolver las dependencias, también agrega un control más preciso sobre el proceso de resolución de dependencia mediante el modificador - DependencyVersion en el consola de administrador de paquetes. El conmutador permite resolver dependencias con respecto a la versión más antigua de posibles (comportamiento predeterminado), la versión más alta posible, o el más alto versión menor o revisión. Este parámetro solo funciona para el paquete de instalación en el comando de powershell.
- **Atributo de DependencyVersion**: además el conmutador - DependencyVersion detallado anteriormente, NuGet también ha permitido la capacidad de establecer un nuevo atributo en el archivo de nuget.config define lo que es el valor predeterminado, si cambia la DependencyVersion - no se especifica en una invocación del paquete de instalación. Este valor también se respetan en el cuadro de diálogo Administrador de paquetes de NuGet para las operaciones del paquete de instalación. Para establecer este valor, agregue el atributo siguiente al archivo nuget.config:

    `<config> <add key="dependencyversion" value="Highest" /> </config>`
- **Obtener una vista previa de las operaciones de NuGet con - whatif**: paquetes de NuGet algunas pueden tener gráficos de dependencia profunda y por lo tanto, puede resultar útil durante una instalación, desinstalar o para ver primero lo que ocurrirá la operación de actualización. NuGet 2.8 agrega el PowerShell estándar: ¿qué ocurre si se cambia a los comandos install-package, paquete de desinstalación y paquete de actualización para habilitar la visualización el cierre completo de los paquetes a la que se aplicará el comando.
- **Cambiar el paquete**: no es raro que instale una versión preliminar de un paquete para investigar nuevas características y, a continuación, decida revertir a la última versión estable. Antes de NuGet 2.8, esto era un proceso en varias fases de desinstalar el paquete de versión preliminar y sus dependencias y, a continuación, instalar la versión anterior. Con NuGet 2.8, sin embargo, el paquete de actualización ahora revertirá al cierre de paquete completo (por ejemplo, el árbol de dependencias del paquete) a la versión anterior.
- **Dependencias de desarrollo**: muchos tipos diferentes de funciones se pueden entregar como paquetes de NuGet - incluidas las herramientas que se usan para optimizar el proceso de desarrollo. Estos componentes, mientras que pueden contribuir en el desarrollo de un nuevo paquete, no deberían considerarse publica una dependencia del nuevo paquete cuando resulta más adelante. NuGet 2.8 habilita un paquete para identificarse en el archivo NuSpec como un developmentDependency. Cuando instala, estos metadatos también se agregará al archivo packages.config del proyecto en el que se instaló el paquete. Cuando ese archivo packages.config se analiza más adelante para las dependencias de NuGet durante nuget.exe pack, excluirá esas dependencias marcadas como dependencias de desarrollo.
- **Archivos de packages.config individuales para diferentes plataformas**: al desarrollar aplicaciones para varias plataformas de destino, es habitual tener los archivos de proyecto diferentes para cada uno de los entornos de compilación correspondientes. También es común para utilizar paquetes de NuGet diferentes en distintos archivos de proyecto, como paquetes tienen distintos niveles de compatibilidad para diferentes plataformas. NuGet 2.8 proporciona compatibilidad mejorada para este escenario mediante la creación de archivos packages.config diferentes para los archivos de proyecto específicos de la plataforma diferente.
- **Retroceder a la memoria caché Local**: paquetes de NuGet aunque normalmente se usan desde una galería de remota como el [Galería de NuGet](http://www.nuget.org) mediante una conexión de red, hay muchos escenarios donde el cliente no está conectado. Sin una conexión de red, el cliente de NuGet no pudo instalar correctamente los paquetes - incluso cuando los paquetes que ya estaban en el equipo cliente en la caché local de NuGet. NuGet 2.8 agrega almacenamiento en caché automático reserva en la consola de administrador de paquetes.

    La característica de reserva de memoria caché no requiere ningún argumento de comando concreto. Además, caché reserva actualmente solo funciona en la consola de administrador de paquete; el comportamiento no funciona actualmente en el cuadro de diálogo del Administrador de paquetes.
- **Correcciones de errores**: una de las correcciones de errores principales realizadas fue mejora del rendimiento en el paquete de actualización-volver a instalar el comando.

    Además de estas características y la corrección de rendimiento mencionado anteriormente, esta versión de NuGet también incluye muchas otras correcciones de errores. Había 181 problemas total cubiertos en la versión. Para obtener una lista completa de los trabajos elementos corregidos en NuGet 2.8, por favor, vista la [NuGet Issue Tracker](https://nuget.codeplex.com/workitem/list/advanced?release=NuGet%202.8&status=all) para esta versión.

<a id="webforms"></a>
### <a name="aspnet-web-forms"></a>Formularios Web Forms de ASP.NET

- Las plantillas de formularios Web Forms ahora muestran cómo realizar la confirmación de la cuenta y restablecimiento de contraseñas para identidades de ASP.NET.
- El control de origen de datos de entidad y el proveedor de datos dinámicos para Entity Framework 6. Para obtener más información, consulte el blog MSDN siguiente: [proveedor de datos dinámicos y control EntityDataSource para Entity Framework 6](https://blogs.msdn.com/b/webdev/archive/2014/01/30/announcing-preview-of-dynamic-data-provider-and-entitydatasource-control-for-entity-framework-6.aspx).

<a id="mvc"></a>
### <a name="aspnet-mvc-512"></a>ASP.NET MVC 5.1.2

- [Atributo mejoras de enrutamiento](../../../mvc/overview/releases/mvc51-release-notes.md#AttributeRouting)
- [Compatibilidad de arranque para plantillas de editor](../../../mvc/overview/releases/mvc51-release-notes.md#Bootstrap)
- [Compatibilidad de enumeraciones en las vistas](../../../mvc/overview/releases/mvc51-release-notes.md#Enum)
- [Compatibilidad con Unobstrusive MinLength y MaxLength atributos](../../../mvc/overview/releases/mvc51-release-notes.md#Unobtrusive)
- [Compatibilidad con el contexto de 'this' en Ajax discreto](../../../mvc/overview/releases/mvc51-release-notes.md#thisContext)
- Varios [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=MVC&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webapi"></a>
### <a name="aspnet-web-api-212"></a>ASP.NET Web API 2.1.2

- [Control de errores global](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#global-error)
- [Atributo mejoras de enrutamientos](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#attribute-routing)
- [Mejoras de la página de ayuda](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#help-page)
- [Compatibilidad con IgnoreRoute](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#ignoreroute)
- [Formateador de tipo de medio BSON](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#bson)
- [Una mejor compatibilidad con filtros de async](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#async-filters)
- [Análisis para el cliente de biblioteca de formato de la consulta](../../../web-api/overview/releases/whats-new-in-aspnet-web-api-21.md#query-parsing)
- Varios [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20API%7cWeb%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="webpages"></a>
### <a name="aspnet-web-pages-312"></a>3.1.2 de ASP.NET Web Pages

- Varios [correcciones de errores](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=Closed&type=All&priority=All&release=v5.1%20Preview%7cv5.1%20RTM&assignedTo=All&component=Web%20Pages/Razor&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="ef"></a>
### <a name="entity-framework-61"></a>Entity Framework 6.1

Entity Framework se actualizó a la versión 6.1 en tiempo de ejecución y herramientas. Entity Framework (EF) 6.1 es una actualización secundaria a Entity Framework 6 y se incluye una serie de correcciones y nuevas características. Para obtener información detallada sobre EF6.1, incluidos los vínculos a documentación para las nuevas características, consulte [historial de versiones de Entity Framework](https://msdn.microsoft.com/data/jj574253). Las nuevas características en esta versión incluyen:

- **Herramientas de consolidación** proporciona una manera coherente para crear un nuevo modelo EF. Esta característica amplía el Asistente de Entity Data Model de ADO.NET para admitir la creación de modelos de Code First, incluidas las técnicas de ingeniería inversa de una base de datos existente. Estas características no estaban disponibles anteriormente en la calidad de la versión Beta en EF Power Tools.
- **Control de errores de confirmación de transacción** proporciona las nuevas [System.Data.Entity.Infrastructure.CommitFailureHandler](https://msdn.microsoft.com/library/system.data.entity.infrastructure.commitfailurehandler(v=vs.113).aspx) que hace uso de la capacidad recién introducida para interceptar las operaciones de transacción. El **CommitFailureHandler** permite la recuperación automática de errores de conexión al confirmar una transacción.
- **IndexAttribute** permite índices especificarse mediante la colocación de un atributo en una propiedad (o propiedades) en el modelo de Code First. Código en primer lugar, a continuación, creará un índice correspondiente en la base de datos.
- **La API pública de la asignación** proporciona acceso a la información de EF tiene sobre cómo las propiedades y los tipos se asignan a columnas y tablas en la base de datos. En las versiones anteriores, esta API era interna.
- **Capacidad de configurar interceptores mediante el archivo App/Web.config**(lo que permite interceptores agregarse sin volver a compilar la aplicación).
- **DatabaseLogger** es un interceptor nueva que facilita el proceso Registrar todas las operaciones de base de datos en un archivo. En combinación con la característica anterior, esto permite cambiar fácilmente en el registro de operaciones de base de datos para una aplicación implementada, sin necesidad de volver a compilar.
- **Detección de cambios de modelo de migraciones** se ha mejorado para que las migraciones con scaffolding son más precisas; rendimiento del proceso de detección de cambios también se ha mejorado considerablemente.
- **Mejoras de rendimiento** incluidas las operaciones de reducción de la base de datos durante la inicialización, las optimizaciones para la comparación de igualdad null en las consultas LINQ, más rápido ver generación (creación de modelos) en más escenarios y más eficaz materialización de entidades sometidas a seguimiento con varias asociaciones.

<a id="identity"></a>
### <a name="aspnet-identity-200"></a>Identidad de ASP.NET 2.0.0

- **Autenticación en dos fases**: ASP.NET Identity ahora es compatible con la autenticación en dos fases. Autenticación en dos fases proporciona un nivel adicional de seguridad a sus cuentas de usuario en el caso donde en peligro la contraseña. También hay protección para los ataques de fuerza bruta contra los códigos de dos fases.
- **Bloqueo de cuenta:** proporciona una manera de bloquear al usuario si el usuario escribe su contraseña o códigos de dos factores de forma incorrecta. El número de intentos no válidos y el intervalo de tiempo para los usuarios quedan bloqueados se puede configurar. Un desarrollador puede, opcionalmente, desactivar el bloqueo de cuenta para determinadas cuentas de usuario que necesitan para.
- **Confirmación de cuenta:** el sistema de identidades de ASP.NET ahora es compatible con la confirmación de la cuenta. Se trata de una situación bastante común en la mayoría de sitios Web hoy en día que, al registrar una nueva cuenta en el sitio Web, debe confirmar el correo electrónico para poder realizar cualquier acción en el sitio Web. Confirmación por correo electrónico es útil, ya que impide que se va a crear cuentas ficticias. Esto es muy útil si va a utilizar el correo electrónico como un método de comunicación con los usuarios de su sitio Web, como sitios de foro, banca, comercio electrónico o sitios web sociales.
- **Restablecimiento de contraseña:** restablecer contraseñas es una característica que el usuario puede restablecer sus contraseñas si ha olvidado su contraseña.
- **El sello de seguridad (everywhere de cierre de sesión):** admite una forma de volver a generar el Token de seguridad para el usuario en los casos, cuando el usuario cambie su contraseña o cualquier otro título relacionados con la información como la eliminación de un inicio de sesión asociado (por ejemplo, Facebook, Google, Cuenta de Microsoft y así sucesivamente). Esto es necesario para asegurarse de que se invalidan los tokens que se genera con la contraseña antigua. En el proyecto de ejemplo, si cambia la contraseña del usuario, a continuación, se genera un nuevo token para el usuario y se invalidan los tokens anteriores. Esta característica proporciona un nivel adicional de seguridad a la aplicación desde cuando cambia la contraseña, cerrará la sesión de cualquier lugar (todos los otros exploradores) donde ha iniciado sesión en esta aplicación.
- **Hacer que el tipo de clave principal sea extensible para usuarios y Roles**: en ASP.NET Identity 1.0, el tipo de clave principal de la tabla a los usuarios y Roles era cadenas. Esto significa que si el sistema de identidades de ASP.NET se mantuvo en SQL Server mediante el uso de Entity Framework, que estuviéramos usando nvarchar. Había muchas discusiones en torno a esta implementación predeterminada acerca del desbordamiento de pila y basándose en las respuestas entrantes. Proporcionamos un enlace de la extensibilidad donde puede especificar cuál debe ser la clave principal de la tabla de usuarios y Roles. Este enlace de la extensibilidad es especialmente útil que si va a migrar la aplicación y la aplicación se almacenar identificadores de usuario son GUID o ints.
- **Admitir IQueryable en usuarios y Roles**: ha agregado compatibilidad con IQueryable en UsersStore y RolesStore, puede obtener fácilmente la lista de usuarios y Roles.
- **Operación de eliminación de soporte técnico a través de UserManager**
- **La indización en nombre de usuario**: implementación de ASP.NET Identity Entity Framework, hemos agregado un índice único en el nombre de usuario usando el nuevo IndexAttribute de EF 6.1.0. Esto garantiza que siempre son únicos nombres de usuario y no había ninguna condición de carrera en el que puede acabar con nombres de usuario duplicados.
- **Validador de contraseña mejorado:** el validador de contraseña que se envió en ASP.NET Identity 1.0 era un validador de contraseña bastante básica que sólo se valida la longitud mínima. Hay un validador de contraseña nueva que ofrece un mayor control sobre la complejidad de la contraseña. Tenga en cuenta que aunque activar todas las configuraciones de esta contraseña, le recomendamos para habilitar la autenticación en dos fases para las cuentas de usuario.
- **IdentityFactory Middleware / CreatePerOwinContext**:

    - **El Administrador de usuarios**: puede usar la implementación del generador para obtener una instancia de UserManager desde el contexto OWIN. Este patrón es similar a lo que se usan para recibir AuthenticationManager de contexto OWIN de inicio de sesión y cierre de sesión. Se trata de una manera recomendada de obtener una instancia de UserManager por solicitud para la aplicación.
    - **DbContextFactory**: ASP.NET Identity utiliza Entity Framework para conservar el sistema de identidades en SQL Server. Para ello, el sistema de identidades tiene una referencia a la ApplicationDbContext. El DbContextFactory Middleware devuelve una instancia de la ApplicationDbContext por solicitud que puede usar en la aplicación.
- **Paquete de NuGet de ejemplos de identidad ASP.NET**: el ejemplos paquete NuGet puede ser de ayuda instalar y ejecutar los ejemplos de ASP.NET Identity y siga las prácticas recomendadas. Se trata de un ejemplo de aplicación de ASP.NET MVC. Modifique el código para satisfacer la aplicación antes de implementar esto en producción. El ejemplo debe instalarse en una aplicación de ASP.NET vacía. Para obtener más información sobre el paquete, vaya a la siguiente entrada de blog: [anuncio de RTM de ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx)

<a id="owin"></a>
### <a name="microsoft-owin-components"></a>Componentes de Microsoft OWIN

No hay una gran cantidad de errores que se corrigieron en esta versión. Vea la [notas para el 2.1.0 versión](https://katanaproject.codeplex.com/releases/view/113281) para obtener más información.

<a id="signalr"></a>
### <a name="aspnet-signalr-202"></a>ASP.NET SignalR 2.0.2

No hay una gran cantidad de errores que se corrigieron en esta versión. Vea la [notas para el 2.0.2 versión](https://github.com/SignalR/SignalR/releases/tag/2.0.2) para obtener más información.
