---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Implementación Web de ASP.NET con Visual Studio: transformaciones del archivo Web.config | Microsoft Docs'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 0a52530396efa3612d19817eeaa0498cffdac38f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37842444"
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Implementación Web de ASP.NET con Visual Studio: transformaciones del archivo Web.config
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar el proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) una ASP.NET web application a Azure App Service Web Apps o a un proveedor de hospedaje de terceros, mediante el uso de Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, vea [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo automatizar el proceso de cambiar el *Web.config* archivo cuando se implementa en distintos entornos de destino. La mayoría de las aplicaciones tienen configuraciones en el *Web.config* archivo que debe ser diferente cuando se implementa la aplicación. Automatizar el proceso de realizar estos cambios no tiene que hacer manualmente cada vez que implementa, lo que resultaría tedioso y propenso a errores.

Recordatorio: Si recibe un mensaje de error o algo no funciona conforme avance en el tutorial, no olvide consultar la [página de solución](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformaciones de Web.config en comparación con los parámetros de Web Deploy

Hay dos maneras de automatizar el proceso de cambiar *Web.config* configuración de archivo: [transformaciones de Web.config](https://msdn.microsoft.com/library/dd465326.aspx) y [parámetros Web Deploy](https://msdn.microsoft.com/library/ff398068.aspx). Un *Web.config* archivo de transformación contiene marcado XML que especifica cómo cambiar el *Web.config* archivo cuando se implementa. Puede especificar distintos cambios específica para las configuraciones de compilación y específica para perfiles de publicación. Las configuraciones de compilación predeterminadas son Debug y Release, y puede crear configuraciones de compilación personalizada. Normalmente, un perfil de publicación corresponde a un entorno de destino. (Aprenderá más sobre cómo publicación perfiles en el [implementar en IIS como un entorno de prueba](deploying-to-iis.md) tutorial.)

Parámetros de implementación Web pueden usarse para especificar distintos tipos de configuración que se debe configurar durante la implementación, incluida la configuración que se encuentra en *Web.config* archivos. Cuando se usa para especificar *Web.config* cambios del archivo, son más complejos para configurar los parámetros de Web Deploy, pero son útiles cuando no conoce el valor debe establecerse hasta que se implemente. Por ejemplo, en un entorno empresarial, puede crear un *paquete de implementación* y darle a una persona del departamento de TI para instalar en producción, y esa persona debe ser capaz de escribir cadenas de conexión o las contraseñas que no lo hacen saber.

Para el escenario que abarca esta serie de tutoriales, se sabe con antelación todo lo que debe realizarse en el *Web.config* de archivos, por lo que no es necesario utilizar parámetros de Web Deploy. Configurará algunas transformaciones que varían según la configuración de compilación usa, y otras que varían según el perfil de publicación que utiliza.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Especifica la configuración de Web.config en Azure

Si el *Web.config* se encuentra en configuración del archivo que desea cambiar la `<connectionStrings>` o `<appSettings>` elemento, y si va a implementar en Web Apps en Azure App Service, tiene otra opción para automatizar los cambios durante implementación. Puede especificar la configuración que desea que surta efecto en Azure en el **configurar** ficha de la página del portal de administración de la aplicación web (desplácese hacia abajo hasta la **configuración de la aplicación** y **las cadenas de conexión**  secciones). Al implementar el proyecto, Azure aplica automáticamente los cambios. Para obtener más información, consulte [sitios Web Windows Azure: How Application Strings and Connection Strings Work](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Archivos de transformación de forma predeterminada

En **el Explorador de soluciones**, expanda *Web.config* para ver el *Web.Debug.config* y *Web.Release.config* los archivos de transformación se crean de forma predeterminada para las configuraciones de compilación de dos valores predeterminados.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Puede crear archivos de transformación para las configuraciones de compilación personalizada haciendo clic en el archivo Web.config y elegir **agregar transformaciones de configuración** en el menú contextual. Para este tutorial no es necesario hacer eso, y la opción de menú está deshabilitada porque no ha creado las configuraciones de compilación personalizada.

Más adelante podrá crear tres archivos de transformación más, uno para la prueba, ensayo y producción de perfiles de publicación. Un ejemplo típico de una configuración que se deben controlar en un archivo de transformación de perfil de publicación porque depende del entorno de destino es un extremo WCF que es diferente para prueba frente a producción. A crear archivos de transformación del perfil de publicación en los tutoriales posteriores después de crear los perfiles de publicación que se pasan con.

## <a name="disable-debug-mode"></a>Deshabilitar el modo de depuración

Un ejemplo de una configuración que dependa de configuración de compilación en lugar de entorno de destino es el `debug` atributo. Para una versión de lanzamiento, normalmente desea depuración deshabilitada, independientemente del entorno que está implementando. Por lo tanto, de forma predeterminada, Visual Studio, plantillas de proyecto crean *Web.Release.config* transformar archivos de código que se quita el `debug` atributo desde el `compilation` elemento. Este es el valor predeterminado *Web.Release.config*: además de algún código de transformación de ejemplo que está marcada como comentario, incluye código en el `compilation` elemento que se quita el `debug` atributo:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

El `xdt:Transform="RemoveAttributes(debug)"` atributo especifica que desea que el `debug` atributo va a quitar de la `system.web/compilation` elemento implementado *Web.config* archivo. Esto se hará cada vez que implementa una versión de lanzamiento.

## <a name="limit-error-log-access-to-administrators"></a>Limitar el acceso de registro de errores a los administradores

Si hay un error mientras se ejecuta la aplicación, la aplicación muestra una página de error genérico en lugar de la página de error generados por el sistema y lo utiliza el [paquete Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) de registro e informes de errores. El `customErrors` elemento en la aplicación *Web.config* archivo especifica la página de error:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Para ver la página de error, cambie temporalmente el `mode` atributo de la `customErrors` elemento de "RemoteOnly" en "Activado" y ejecute la aplicación desde Visual Studio. Producirá un error al solicitar una dirección URL no válida, como *Studentsxxx.aspx*. En lugar de un error generado por IIS "no se encuentra el recurso" página, verá el *GenericErrorPage.aspx* página.

![Página de error](web-config-transformations/_static/image2.png)

Para ver el registro de errores, reemplace todo el contenido de la dirección URL después del número de puerto con *elmah.axd* (por ejemplo, `http://localhost:51130/elmah.axd`) y presione ENTRAR:

![Página ELMAH](web-config-transformations/_static/image3.png)

No olvide establecer el `customErrors` elemento volver al modo de "RemoteOnly" cuando haya terminado.

En el equipo de desarrollo es conveniente permitir el acceso gratuito a la página de registro de errores, pero en la producción que sería un riesgo de seguridad. Para el sitio de producción, que desea agregar una regla de autorización que restringe el acceso de registro de errores a los administradores y para asegurarse de que funciona de la restricción desee en las pruebas y ensayo también. Por lo que es otro cambio que desea implementar cada vez que implementa una versión de lanzamiento, por lo que pertenece el *Web.Release.config* archivo.

Abra *Web.Release.config* y agregue un nuevo `location` elemento inmediatamente antes del cierre `configuration` etiqueta, como se muestra aquí.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

El `Transform` Esto hace que el valor de atributo de "Inserción" `location` elemento agregarse como un elemento relacionado a cualquier existente `location` elementos en el *Web.config* archivo. (Ya hay uno `location` reglas de elemento que especifica la autorización para el **actualización créditos** página.)

Ahora puede obtener una vista previa de la transformación para asegurarse de que se codificó correctamente.

En **el Explorador de soluciones**, haga clic en *Web.Release.config* y haga clic en **Preview transformar**.

![Menú de transformación de vista previa](web-config-transformations/_static/image4.png)

Se abrirá una página que muestra la evolución *Web.config* archivo a la izquierda y qué implementado *Web.config* archivo tendrá un aspecto similar a la derecha, con cambios resaltados.

![Vista previa de la transformación de depuración](web-config-transformations/_static/image5.png)

![Vista previa de la transformación de ubicación](web-config-transformations/_static/image6.png)

(En la versión preliminar, es posible que observe algunos cambios adicionales que no es suyo transformaciones para: estos suelen implican la eliminación de espacio en blanco que no afectan a la funcionalidad.)

Cuando se prueba el sitio después de la implementación, que también va a probar para comprobar que la regla de autorización es efectiva.

> [!NOTE] 
> 
> **Nota de seguridad** nunca mostrar detalles del error para el público en una aplicación de producción, o almacenar esa información en una ubicación pública. Los atacantes pueden usar la información de error para detectar vulnerabilidades en un sitio. Si usas ELMAH en su propia aplicación, configure ELMAH para minimizar los riesgos de seguridad. El ejemplo ELMAH en este tutorial no debe considerarse una configuración recomendada. Es un ejemplo que se ha elegido para ilustrar cómo controlar una carpeta que la aplicación debe ser capaz de crear archivos. Para obtener más información, consulte [proteger el punto de conexión ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Un valor que se va a controlar en publicar archivos de transformación de perfil

Un escenario común es tener *Web.config* configuración que debe ser diferente en cada entorno que se implementa en archivos. Por ejemplo, una aplicación que llama a un servicio WCF podría necesitar un punto de conexión diferentes en entornos de prueba y producción. La aplicación Contoso University también incluye un valor de este tipo. Esta configuración controla un indicador visible en las páginas de un sitio que le indica qué entorno está utilizando, como desarrollo, prueba o producción. El valor de configuración determina si la aplicación anexará "(desarrollo)" o "(probar)" en el título del principal el *Site.Master* página maestra:

![Indicador de entorno](web-config-transformations/_static/image7.png)

El indicador de entorno se omite cuando se ejecuta la aplicación en ensayo o producción.

Las páginas web de Contoso University leer un valor que se establece en `appSettings` en el *Web.config* archivo con el fin de determinar en qué entorno se está ejecutando la aplicación en:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

El valor debe ser "Test" en el entorno de prueba y "Prod" de ensayo y producción.

El siguiente código en un archivo de transformación implementará esta transformación:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

El `xdt:Transform` "SetAttributes" indica que es el propósito de esta transformación cambiar los valores de atributo de un elemento existente en el valor del atributo el *Web.config* archivo. El `xdt:Locator` "Match(key)" indica que el elemento que se va a modificar es del valor del atributo cuya `key` atributo coincide con el `key` atributo especificado aquí. El solo otro atributo de la `add` es elemento `value`, y eso es lo que se cambiará en implementado *Web.config* archivo. El código mostrado aquí hace que el `value` atributo de la `Environment` `appSettings` elemento que se va a establecerse en "Test" la *Web.config* archivo que se implementa.

Esta transformación pertenece a los archivos de transformación de perfil de publicación, que todavía no ha creado. Creará y actualizar los archivos de transformación que implementan este cambio al crear los perfiles de publicación para los entornos de prueba, ensayo y producción. Hágalo el [implementar en IIS](deploying-to-iis.md) y [implementar en producción](deploying-to-production.md) tutoriales.

> [!NOTE]
> Dado que esta opción está en el `<appSettings>` elemento, tiene otra alternativa para especificar la transformación cuando se va a implementar en Web Apps en Azure App Service, consulte [Web.config especificando su configuración](#watransforms) anteriormente en en este tema.


## <a name="setting-connection-strings"></a>Configuración de las cadenas de conexión

Aunque el archivo de transformación predeterminado contiene un ejemplo que muestra cómo actualizar una cadena de conexión, en la mayoría de los casos no necesitará configurar transformaciones de cadena de conexión, ya que puede especificar las cadenas de conexión del perfil de publicación. Hágalo el [implementar en IIS](deploying-to-iis.md) y [implementar en producción](deploying-to-production.md) tutoriales.

## <a name="summary"></a>Resumen

Ahora ha hecho todo lo pueda con *Web.config* transformaciones antes de crear los perfiles de publicación, y ha visto una vista previa de lo que estará en el archivo Web.config implementado.

![Vista previa de la transformación de ubicación](web-config-transformations/_static/image8.png)

En el tutorial siguiente, nos ocupamos de las tareas de configuración de implementación que requieren el establecimiento de propiedades del proyecto.

## <a name="more-information"></a>Más información

Para obtener más información sobre los temas tratados en este tutorial, vea [transformaciones de Web.config utilizando para cambiar la configuración en el archivo Web.config de destino o el archivo app.config durante la implementación](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) en el mapa de contenido de implementación Web de Visual Studio y ASP.NET.

> [!div class="step-by-step"]
> [Anterior](preparing-databases.md)
> [Siguiente](project-properties.md)
