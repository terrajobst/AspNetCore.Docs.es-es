---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
title: 'Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: transformaciones del archivo Web.Config - 3 de 12 | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluya una base de datos de SQL Server Compact mediante Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 2b0df3d9-450b-4ea6-b315-4c9650722cad
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12
msc.type: authoredcontent
ms.openlocfilehash: a5afcf4d05d6fa51ad70f7e5bdfafd20002f188a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841763"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-webconfig-file-transformations---3-of-12"></a>Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio o Visual Web Developer: transformaciones del archivo Web.Config - 3 de 12
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> Esta serie de tutoriales muestra cómo implementar un ASP.NET (publicar) proyecto de aplicación web que incluye una base de datos de SQL Server Compact mediante Visual Studio 2012 RC o Visual Studio Express 2012 RC para Web. También puede usar Visual Studio 2010 si instala la actualización de publicación en Web. Para obtener una introducción a la serie, consulte [el primer tutorial de la serie](deployment-to-a-hosting-provider-introduction-1-of-12.md).
> 
> Para ver un tutorial que muestra las características de implementación introducidas después de la versión de Visual Studio 2012 RC, se muestra cómo implementar las ediciones de SQL Server que no sea de SQL Server Compact y se muestra cómo se implementa en Azure App Service Web Apps, consulte [implementación Web de ASP.NET con Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo automatizar el proceso de cambiar el *Web.config* archivo cuando se implementa en distintos entornos de destino. La mayoría de las aplicaciones tienen configuraciones en el *Web.config* archivo que debe ser diferente cuando se implementa la aplicación. Automatizar el proceso de realizar estos cambios no tiene que hacer manualmente cada vez que implementa, lo que resultaría tedioso y propenso a errores.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Parámetros de implementación de las transformaciones de Web.config frente a la Web

Hay dos maneras de automatizar el proceso de cambiar *Web.config* configuración de archivo: [transformaciones de Web.config](https://msdn.microsoft.com/library/dd465326.aspx) y [parámetros Web Deploy](https://msdn.microsoft.com/library/ff398068.aspx). Un *Web.config* archivo de transformación contiene marcado XML que especifica cómo cambiar el *Web.config* archivo cuando se implementa. Puede especificar distintos cambios específica para las configuraciones de compilación y específica para perfiles de publicación. Las configuraciones de compilación predeterminadas son Debug y Release, y puede crear configuraciones de compilación personalizada. Normalmente, un perfil de publicación corresponde a un entorno de destino. (Aprenderá más sobre cómo publicación perfiles en el [implementar en IIS como un entorno de prueba](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md) tutorial.)

Parámetros de implementación Web pueden usarse para especificar distintos tipos de configuración que se debe configurar durante la implementación, incluida la configuración que se encuentra en *Web.config* archivos. Cuando se usa para especificar *Web.config* cambios del archivo, son más complejos para configurar los parámetros de Web Deploy, pero son útiles cuando no conoce el valor debe establecerse hasta que se implemente. Por ejemplo, en un entorno empresarial, puede crear un *paquete de implementación* y darle a una persona del departamento de TI para instalar en producción, y esa persona debe ser capaz de escribir cadenas de conexión o las contraseñas que no lo hacen saber.

Para el escenario que se trata en este tutorial, todo lo que debe realizarse para saber el *Web.config* de archivos, por lo que no es necesario utilizar parámetros de Web Deploy. Configurará algunas transformaciones que varían según la configuración de compilación usa, y otras que varían según el perfil de publicación que utiliza.

## <a name="creating-transformation-files-for-publish-profiles"></a>Creación de archivos de transformación para perfiles de publicación

En **el Explorador de soluciones**, expanda *Web.config* para ver el *Web.Debug.config* y *Web.Release.config* los archivos de transformación se crean de forma predeterminada para las configuraciones de compilación de dos valores predeterminados.

![Web.config_transform_files](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image1.png)

Puede crear archivos de transformación para las configuraciones de compilación personalizada haciendo clic en el archivo Web.config y elegir **agregar transformaciones de configuración** en el menú contextual, pero para este tutorial no es necesario hacerlo.

Necesita dos archivos de transformación más, para configurar los cambios que están relacionados con el destino de implementación en lugar de la configuración de compilación. Un ejemplo típico de este tipo de configuración es un extremo WCF que es diferente para prueba frente a producción. En los tutoriales posteriores, podrá crear, publicar perfiles con el nombre de prueba y producción, por lo que necesita un *Web.Test.config* archivo y un *Web.Production.config* archivo.

Los archivos de transformación que están vinculados para perfiles de publicación deben crearse manualmente. En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y seleccione **Abrir carpeta en el Explorador de Windows**.

![Open_folder_in_Windows_Explorer](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image2.png)

En **Windows Explorer**, seleccione el *Web.Release.config* archivo, copie el archivo y, a continuación, pegar dos copias del mismo. Cambiar el nombre de estas copias *Web.Production.config* y *Web.Test.config*, a continuación, cierre **Windows Explorer**.

En **el Explorador de soluciones**, haga clic en **actualizar** para ver los nuevos archivos.

Seleccione los nuevos archivos, con el botón secundario y, a continuación, haga clic en **incluir en el proyecto** en el menú contextual.

![Inclusión de archivos de configuración de prueba y producción en el proyecto](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image3.png)

Para evitar que se implementen estos archivos, selecciónelos en **el Explorador de soluciones**y, a continuación, en el **propiedades** ventana cambian el **acción de compilación** propiedad desde **Contenido** a **ninguno**. (Los archivos de transformación que se basan en las configuraciones de compilación se les automáticamente de la implementación.)

Ahora está preparado para entrar en *Web.config* las transformaciones en el *Web.config* archivos de transformación.

## <a name="limiting-error-log-access-to-administrators"></a>Limitar el acceso al registro de errores a los administradores

Si hay un error mientras se ejecuta la aplicación, la aplicación muestra una página de error genérico en lugar de la página de error generados por el sistema y lo utiliza [paquete Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) de registro e informes de errores. El `customErrors` elemento en el *Web.config* archivo especifica la página de error:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample1.xml)]

Para ver la página de error, cambie temporalmente el `mode` atributo de la `customErrors` elemento de "RemoteOnly" en "Activado" y ejecute la aplicación desde Visual Studio. Producirá un error al solicitar una dirección URL no válida, como *Studentsxxx.aspx*. En lugar de una página de error generado por IIS "página de no encontrado", verá el *GenericErrorPage.aspx* página.

[![Error_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image5.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image4.png)

Para ver el registro de errores, reemplace todo el contenido de la dirección URL después del número de puerto con *elmah.axd* (por ejemplo, en la captura de pantalla `http://localhost:51130/elmah.axd`) y presione ENTRAR:

[![Elmah_log_page](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image7.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image6.png)

No olvide establecer el `customErrors` elemento volver al modo de "RemoteOnly" cuando haya terminado.

En el equipo de desarrollo es conveniente permitir el acceso gratuito a la página de registro de errores, pero en la producción que sería un riesgo de seguridad. Para el sitio de producción, puede agregar una regla de autorización que restringe el acceso de registro de error solo a los administradores configurar una transformación en el *Web.Production.config* archivo.

Abra *Web.Production.config* y agregue un nuevo `location` elemento inmediatamente después de la apertura `configuration` etiqueta, como se muestra aquí. (Asegúrese de que solo agrega el `location` elemento y no el marcado circundante que se muestra aquí únicamente para ofrecer cierto contexto.)

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample2.xml?highlight=2-9)]

El `Transform` Esto hace que el valor de atributo de "Inserción" `location` elemento agregarse como un elemento relacionado a cualquier existente `location` elementos en el *Web.config* archivo. (Ya hay uno `location` reglas de elemento que especifica la autorización para el **actualización créditos** página.) Cuando se prueba el sitio de producción después de la implementación, que va a probar para comprobar que esta regla de autorización es efectiva.

No es necesario restringir el acceso de registro de error en el entorno de prueba, por lo que no debe agregar este código a la *Web.Test.config* archivo.

> [!NOTE] 
> 
> **Nota de seguridad** nunca mostrar detalles del error para el público en una aplicación de producción, o almacenar esa información en una ubicación pública. Los atacantes pueden usar la información de error para detectar vulnerabilidades en un sitio. Si usas ELMAH en su propia aplicación, asegúrese de investigar formas en que se puede configurar ELMAH para minimizar los riesgos de seguridad. El ejemplo ELMAH en este tutorial no debe considerarse una configuración recomendada. Es un ejemplo que se ha elegido para ilustrar cómo controlar una carpeta que la aplicación debe ser capaz de crear archivos.


## <a name="setting-an-environment-indicator"></a>Establecer un indicador de entorno

Un escenario común es tener *Web.config* configuración que debe ser diferente en cada entorno que se implementa en archivos. Por ejemplo, una aplicación que llama a un servicio WCF podría necesitar un punto de conexión diferentes en entornos de prueba y producción. La aplicación Contoso University también incluye un valor de este tipo. Esta configuración controla un indicador visible en las páginas de un sitio que le indica qué entorno está utilizando, como desarrollo, prueba o producción. El valor de configuración determina si la aplicación anexará "(desarrollo)" o "(probar)" en el título del principal el *Site.Master* página maestra:

[![Environment_indicator](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image9.png)](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/_static/image8.png)

El indicador de entorno se omite cuando se ejecuta la aplicación en producción.

Las páginas web de Contoso University leer un valor que se establece en `appSettings` en el *Web.config* archivo con el fin de determinar en qué entorno se está ejecutando la aplicación en:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample3.xml)]

El valor debe ser "Test" en el entorno de prueba y "Prod" en el entorno de producción.

Abra *Web.Production.config* y agregue un `appSettings` elemento inmediatamente antes de la etiqueta de apertura de la `location` elemento agregado anteriormente:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample4.xml)]

El `xdt:Transform` "SetAttributes" indica que es el propósito de esta transformación cambiar los valores de atributo de un elemento existente en el valor del atributo el *Web.config* archivo. El `xdt:Locator` "Match(key)" indica que el elemento que se va a modificar es del valor del atributo cuya `key` atributo coincide con el `key` atributo especificado aquí. El solo otro atributo de la `add` es elemento `value`, y eso es lo que se cambiará en implementado *Web.config* archivo. Este código hace que el `value` atributo de la `Environment` `appSettings` elemento que se va a establecerse en "Prod" el *Web.config* archivo que se implementa en producción.

A continuación, se aplican los mismos cambios *Web.Test.config* archivo, excepto set el `value` a "Test" en lugar de "Prod". Cuando haya terminado, la `appSettings` sección *Web.Test.config* tendrá un aspecto similar al ejemplo siguiente:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample5.xml)]

## <a name="disabling-debug-mode"></a>Deshabilitar el modo de depuración

Para una compilación de versión, no desea habilitada la depuración, independientemente del entorno que está implementando. De forma predeterminada el *Web.Release.config* archivo de transformación se crea automáticamente con el código que se quita el `debug` atributo desde el `compilation` elemento:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample6.xml)]

El `Transform` atributo hace que el `debug` omisión del atributo de implementado *Web.config* archivo cada vez que implemente una versión de lanzamiento.

Esta misma transformación está en prueba y producción archivos de transformación porque se creó mediante la copia del archivo de transformación de la versión. No es necesario que haya duplicados, de modo que abra cada uno de esos archivos, quite el **compilación** elemento y guarde y cierre cada archivo.

## <a name="setting-connection-strings"></a>Configuración de las cadenas de conexión

En la mayoría de los casos no es necesario configurar transformaciones de cadena de conexión, ya que puede especificar las cadenas de conexión del perfil de publicación. Pero hay una excepción al implementar una base de datos de SQL Server Compact y usa migraciones de Entity Framework Code First para actualizar la base de datos del servidor de destino. En este escenario, debe especificar una cadena de conexión adicionales que se usará en el servidor para actualizar el esquema de base de datos. Para configurar esta transformación, agregue un **&lt;connectionStrings&gt;** elemento inmediatamente después de la apertura **&lt;configuración&gt;** etiqueta en ambos el *Web.Test.config* y *Web.Production.config* transformar archivos:

[!code-xml[Main](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12/samples/sample7.xml)]

El `Transform` atributo especifica que esta cadena de conexión se agregará a la *connectionStrings* elemento implementado *Web.config* archivo. (El proceso de publicación crea automáticamente esta cadena de conexión adicionales automáticamente si no existe, pero de forma predeterminada el **providerName** atributo se establece para `System.Data.SqlClient`, que no funciona no para SQL Server Compact. Al agregar manualmente la cadena de conexión, se mantenga el proceso de implementación desde la creación de un elemento de la cadena de conexión con el nombre de un proveedor incorrecto).

Ahora ha especificado todas las *Web.config* transformaciones que necesita para implementar la aplicación Contoso University para probar y producción. En el tutorial siguiente, nos ocupamos de las tareas de configuración de implementación que requieren el establecimiento de propiedades del proyecto.

## <a name="more-information"></a>Más información

Para obtener más información sobre los temas tratados en este tutorial, consulte el escenario de transformación de Web.config en [mapa de contenido de implementación ASP.NET](https://msdn.microsoft.com/library/bb386521.aspx).

> [!div class="step-by-step"]
> [Anterior](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)
> [Siguiente](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
