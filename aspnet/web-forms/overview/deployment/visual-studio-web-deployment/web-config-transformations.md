---
uid: web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
title: 'Implementación de Web ASP.NET con Visual Studio: transformaciones del archivo Web.config | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 5a2a927b-14cb-40bc-867a-f0680f9febd7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations
msc.type: authoredcontent
ms.openlocfilehash: 77ed0d8b2fe85adb009a3f4759030b7fba8fb9d7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-webconfig-file-transformations"></a>Implementación de Web ASP.NET con Visual Studio: transformaciones del archivo Web.config
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Este tutorial muestra cómo automatizar el proceso de cambiar el *Web.config* archivo cuando se implementa en distintos entornos de destino. Mayoría de las aplicaciones almacenan su configuración en el *Web.config* archivo que debe ser diferente cuando se implementa la aplicación. Automatizar el proceso de realizar estos cambios evita tener que realizar manualmente cada vez que implementa, lo que podría ser tedioso y propenso a errores.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](troubleshooting.md).

## <a name="webconfig-transformations-versus-web-deploy-parameters"></a>Transformaciones de Web.config frente a parámetros de Web Deploy

Hay dos maneras de automatizar el proceso de cambio *Web.config* archivo de configuración: [transformaciones de Web.config](https://msdn.microsoft.com/library/dd465326.aspx) y [Web Deploy parámetros](https://msdn.microsoft.com/library/ff398068.aspx). A *Web.config* archivo de transformación contiene marcado XML que especifica cómo cambiar el *Web.config* archivo cuando se implementa. Puede especificar distintos cambios específica para configuraciones de compilación y específica para perfiles de publicación. Las configuraciones de compilación predeterminadas son Debug y Release, y puede crear configuraciones de compilación personalizada. Normalmente, un perfil de publicación corresponde a un entorno de destino. (Aprenderá más acerca de cómo publicación perfiles en el [implementación en IIS como un entorno de prueba](deploying-to-iis.md) tutorial.)

Parámetros de implementación Web pueden utilizarse para especificar varios tipos diferentes de configuración que se debe configurar durante la implementación, incluida la configuración que se encuentra en *Web.config* archivos. Cuando se usa para especificar *Web.config* cambios del archivo, son más complejas para configurar los parámetros de implementación Web, pero son útiles cuando no conoce el valor que se establecerá hasta que implemente. Por ejemplo, en un entorno empresarial, puede crear un *paquete de implementación* y darle a una persona del departamento de TI para instalar en producción, y esa persona debe ser capaz de escribir las cadenas de conexión o las contraseñas que no conocer.

Para el escenario que abarca esta serie de tutoriales, se sabe de antemano todo lo que tiene que hacer para la *Web.config* de archivos, por lo que no necesita usar parámetros de Web Deploy. Para configurar algunas transformaciones que varían según la configuración de compilación que se usan y otras que varían según el perfil de publicación.

<a id="watransforms"></a>

## <a name="specifying-webconfig-settings-in-azure"></a>Especifica la configuración de Web.config en Azure

Si el *Web.config* configuración de archivo que desea cambiar está en la `<connectionStrings>` o `<appSettings>` elemento, y si va a implementar para las aplicaciones Web en el servicio de aplicación de Azure, tiene otra opción para automatizar los cambios durante implementación. Puede especificar la configuración que desea que se aplique en Azure en la **configurar** ficha de la página del portal de administración de la aplicación web (desplácese hacia abajo hasta la **configuración de la aplicación** y **las cadenas de conexión**  secciones). Al implementar el proyecto, Azure aplica automáticamente los cambios. Para obtener más información, consulte [sitios Web Windows Azure: cómo las cadenas de la aplicación y el trabajo de las cadenas de conexión](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx).

## <a name="default-transformation-files"></a>Archivos de transformación predeterminados

En **el Explorador de soluciones**, expanda *Web.config* para ver el *Web.Debug.config* y *Web.Release.config* archivos de transformación se crean de forma predeterminada para las configuraciones de compilación de dos valores predeterminados.

![Web.config_transform_files](web-config-transformations/_static/image1.png)

Puede crear archivos de transformación para configuraciones de compilación personalizada haciendo clic en el archivo Web.config y elegir **agregar transformaciones de configuración** en el menú contextual. Para este tutorial no es necesario hacerlo y está deshabilitada la opción de menú, porque no ha creado las configuraciones de compilación personalizada.

Más adelante creará tres archivos de transformación más, uno para la prueba, ensayo y producción de perfiles de publicación. Un ejemplo típico de una configuración que se deben controlar en un archivo de transformación de perfil de publicación porque depende del entorno de destino es un extremo WCF que es diferente de prueba y producción. Creará publicar archivos de perfil de transformación en tutoriales posteriores después de crear los perfiles de publicación que salen con.

## <a name="disable-debug-mode"></a>Deshabilitar el modo de depuración

Un ejemplo de una configuración que se base en la configuración de compilación en lugar de entorno de destino es el `debug` atributo. Para una versión de lanzamiento, probablemente le interese deshabilitada la depuración sin tener en cuenta qué referencia del entorno que está implementando. Por lo tanto, de forma predeterminada, Visual Studio crean plantillas de proyecto *Web.Release.config* transformar los archivos con el código que se quita el `debug` de atributo de la `compilation` elemento. Este es el valor predeterminado *Web.Release.config*: además de algún código de transformación de ejemplo que se hace referencia a, incluye código en el `compilation` elemento que se quita el `debug` atributo:

[!code-xml[Main](web-config-transformations/samples/sample1.xml?highlight=18)]

El `xdt:Transform="RemoveAttributes(debug)"` atributo especifica que desea que el `debug` atributo va a quitar de la `system.web/compilation` elemento implementado *Web.config* archivo. Esto se realizará cada vez que implementa una versión de lanzamiento.

## <a name="limit-error-log-access-to-administrators"></a>Limitar el acceso de registro de errores a los administradores

Si hay un error mientras se ejecuta la aplicación, la aplicación muestra una página de error genérico en lugar de la página de error generados por el sistema y usa el [paquete Elmah NuGet](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx) de registro e informes de errores. El `customErrors` elemento de la aplicación *Web.config* archivo especifica la página de error:

[!code-xml[Main](web-config-transformations/samples/sample2.xml)]

Para ver la página de error, cambie temporalmente la `mode` atributo de la `customErrors` elemento de "RemoteOnly" en "On" y ejecute la aplicación desde Visual Studio. Producirá un error solicitando una dirección URL no válida, como *Studentsxxx.aspx*. En lugar de un error generado por IIS "no se puede encontrar el recurso" página, verá el *GenericErrorPage.aspx* página.

![Página de error](web-config-transformations/_static/image2.png)

Para ver el registro de errores, reemplace todo el contenido de la dirección URL después del número de puerto con *elmah.axd* (por ejemplo, `http://localhost:51130/elmah.axd`) y presione ENTRAR:

![Página ELMAH](web-config-transformations/_static/image3.png)

No olvide establecer el `customErrors` elemento volver al modo de "RemoteOnly" cuando haya terminado.

En el equipo de desarrollo es adecuada permitir el acceso gratuito a la página de registro de errores, pero en producción que sería un riesgo de seguridad. Para el sitio de producción, que desee para agregar una regla de autorización que restringe el acceso de registro de errores a los administradores y para asegurarse de que funcionan las restricciones de la desea que estén en la prueba y ensayo también. Por lo que es otro cambio que se va a implementar cada vez que implementa una versión de lanzamiento, por lo que pertenece a la *Web.Release.config* archivo.

Abra *Web.Release.config* y agregue un nuevo `location` elemento inmediatamente antes del cierre `configuration` etiqueta, como se muestra aquí.

[!code-xml[Main](web-config-transformations/samples/sample3.xml?highlight=27-34)]

El `Transform` hace que el valor de atributo de "Inserción" esta `location` elemento agregarse como un elemento relacionado a cualquier existente `location` elementos en la *Web.config* archivo. (Ya hay uno `location` reglas de elemento que especifica la autorización para la **actualización créditos** página.)

Ahora puede obtener una vista previa de la transformación para asegurarse de que se codificó correctamente.

En **el Explorador de soluciones**, haga clic en *Web.Release.config* y haga clic en **transformar de vista previa**.

![Menú de transformación de vista previa](web-config-transformations/_static/image4.png)

Abre una página que muestra la evolución *Web.config* archivo a la izquierda y qué implementado *Web.config* archivo será similar a la derecha, con cambios resaltados.

![Vista previa de transformación de depuración](web-config-transformations/_static/image5.png)

![Vista previa de transformación de ubicación](web-config-transformations/_static/image6.png)

(En la vista previa, podría observar algunos cambios adicionales que no es suyo transforma para: estos suelen implican la eliminación de espacio en blanco que no afectan a la funcionalidad.)

Cuando se prueba el sitio después de la implementación, probará para comprobar que la regla de autorización es efectiva.

> [!NOTE] 
> 
> **Nota de seguridad** nunca mostrar detalles del error para el público en una aplicación de producción, o almacenar la información en una ubicación pública. Los atacantes pueden utilizar la información de error para detectar vulnerabilidades en un sitio. Si usas ELMAH en su propia aplicación, configure ELMAH para minimizar los riesgos de seguridad. El ejemplo ELMAH en este tutorial no debe considerarse una configuración recomendada. Es un ejemplo que se ha elegido para ilustrar cómo controlar una carpeta que la aplicación debe ser capaz de crear archivos en. Para obtener más información, consulte [proteger el extremo ELMAH](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages).


## <a name="a-setting-that-youll-handle-in-publish-profile-transformation-files"></a>Un valor que se va a controlar en publicar archivos de transformación de perfil

Un escenario común es que *Web.config* archivo de configuración que debe ser diferente en cada entorno que se implementa en. Por ejemplo, una aplicación que llama a un servicio WCF podría necesitar un punto de conexión diferentes en entornos de prueba y producción. La aplicación de la Universidad de Contoso también incluye un valor de este tipo. Esta configuración controla un indicador visible en las páginas de un sitio que indica si el entorno en el que está utilizando, como desarrollo, prueba o producción. El valor de configuración determina si la aplicación anexará "(desarrollo)" o "(Test)" para el encabezado principal en la *Site.Master* página maestra:

![Indicador de entorno](web-config-transformations/_static/image7.png)

El indicador de entorno se omite cuando se ejecuta la aplicación de ensayo o de producción.

Las páginas web de Contoso Universidad leer un valor que se establece en `appSettings` en el *Web.config* archivo con el fin de determinar qué entorno de la aplicación se ejecuta en:

[!code-xml[Main](web-config-transformations/samples/sample4.xml)]

El valor debe ser "Test" en el entorno de prueba y "Prod" para ensayo y producción.

El siguiente código en un archivo de transformación implementará esta transformación:

[!code-xml[Main](web-config-transformations/samples/sample5.xml)]

El `xdt:Transform` "SetAttributes" indica que el propósito de esta transformación es cambiar los valores de atributo de un elemento en el valor del atributo el *Web.config* archivo. El `xdt:Locator` "Match(key)" indica que el elemento que se va a modificar es del valor del atributo cuyo `key` atributo coincide con la `key` atributo especificado aquí. El solo otro atributo de la `add` elemento es `value`, y eso es lo que se cambiará en implementado *Web.config* archivo. El código mostrado aquí causas el `value` atributo de la `Environment` `appSettings` elemento que se va a establecer en "Test" en la *Web.config* archivo que se implementa.

Esta transformación pertenece a los archivos de transformación de perfil de publicación, que no ha creado todavía. Podrá crear y actualizar los archivos de transformación que implementan este cambio al crear los perfiles de publicación para los entornos de prueba, ensayo y producción. Llevará a cabo el [implementar en IIS](deploying-to-iis.md) y [implementarse en producción](deploying-to-production.md) tutoriales.

> [!NOTE]
> Dado que esta opción está en el `<appSettings>` elemento, tiene otra alternativa para especificar la transformación cuando se va a implementar para las aplicaciones Web en el servicio de aplicación de Azure vea [configuración de Web.config especificando en Azure](#watransforms) anteriormente en en este tema.


## <a name="setting-connection-strings"></a>Configuración de las cadenas de conexión

Aunque el archivo de transformación predeterminado contiene un ejemplo que muestra cómo actualizar una cadena de conexión, en la mayoría de los casos no necesitará configurar transformaciones de cadena de conexión, ya que puede especificar las cadenas de conexión en el perfil de publicación. Llevará a cabo el [implementar en IIS](deploying-to-iis.md) y [implementarse en producción](deploying-to-production.md) tutoriales.

## <a name="summary"></a>Resumen

Ahora ha hecho tan tanto como sea posible con *Web.config* transformaciones antes de crear los perfiles de publicación y ha visto una vista previa de lo que estará en el archivo Web.config implementado.

![Vista previa de transformación de ubicación](web-config-transformations/_static/image8.png)

En el siguiente tutorial, también deberá tener cuidado de las tareas de configuración de implementación que requieren el establecimiento de propiedades del proyecto.

## <a name="more-information"></a>Más información

Para obtener más información acerca de los temas tratados en este tutorial, vea [Web.config utilizando transformaciones para cambiar la configuración en el archivo Web.config de destino o el archivo app.config durante la implementación](https://go.microsoft.com/fwlink/p/?LinkId=282413#transforms) en el mapa de contenido de implementación Web de Visual Studio y ASP.NET.

> [!div class="step-by-step"]
> [Anterior](preparing-databases.md)
> [Siguiente](project-properties.md)
