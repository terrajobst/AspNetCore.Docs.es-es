---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: "Configurar parámetros para la implementación de paquete de Web | Documentos de Microsoft"
author: jrjlee
description: "En este tema se describe cómo establecer valores de parámetro, como nombres de aplicaciones web de Internet Information Services (IIS), las cadenas de conexión y los puntos de conexión de servicio..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: 12a4ba8ad30df43e7192500ad4514dfa9679f899
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="configuring-parameters-for-web-package-deployment"></a>Configurar parámetros para la implementación de paquete de Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo establecer valores de parámetro, como nombres de aplicaciones web de Internet Information Services (IIS), las cadenas de conexión y los puntos de conexión de servicio, cuando se implementa un paquete web en un servidor web IIS remoto.


Cuando se crea un proyecto de aplicación web, la compilación y el proceso de empaquetado genera tres archivos de clave:

- A *[nombre del proyecto] .zip* archivo. Se trata de un paquete de implementación web para el proyecto de aplicación web. Este paquete contiene todos los ensamblados, archivos, scripts de base de datos y recursos necesarios para volver a crear la aplicación web en un servidor web IIS remoto.
- A *.deploy.cmd [nombre del proyecto]* archivo. Contiene un conjunto de comandos de Web Deploy (MSDeploy.exe) con parámetros que publicar el paquete de implementación web en un servidor web IIS remoto.
- Un *[nombre del proyecto]. SetParameters.xml* archivo. Esto proporciona un conjunto de valores de parámetro para el comando MSDeploy.exe. Puede actualizar los valores de este archivo y pasarla a Web Deploy como un parámetro de línea de comandos al implementar el paquete de la web.

> [!NOTE]
> Para obtener más información sobre la compilación y el proceso de empaquetado, vea [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md).


El *SetParameters.xml* archivo se genera dinámicamente desde el archivo de proyecto de aplicación web y los archivos de configuración dentro de su proyecto. Cuando se compila y empaqueta el proyecto, la canalización de publicación de Web (WPP) detecta automáticamente una gran cantidad de las variables que están probables que cambien entre entornos de implementación, como el destino de la aplicación web de IIS y las cadenas de conexión de base de datos. Estos valores se con parámetros en el paquete de implementación web automáticamente y se agregan a la *SetParameters.xml* archivo. Por ejemplo, si agrega una cadena de conexión para el *web.config* archivo en el proyecto de aplicación web, el proceso de compilación detectará este cambio y agregará una entrada a la *SetParameters.xml* archivo en consecuencia.

En muchos casos, esta parametrización automática será suficiente. Sin embargo, si los usuarios necesitan para modificar otros valores entre entornos de implementación, como la configuración de la aplicación o direcciones URL de extremo de servicio, debe indicar el WPP parametrizar estos valores en el paquete de implementación y agregar las entradas correspondientes a la *SetParameters.xml* archivo. Las secciones siguientes explican cómo hacerlo.

### <a name="automatic-parameterization"></a>Parametrización automática

Al compilar y empaquetar una aplicación web, el WPP parametrizan automáticamente estas cosas:

- El destino IIS web nombre y ruta de la aplicación.
- Cadenas de cualquier conexión en la *web.config* archivo.
- Cadenas de conexión para las bases de datos se agrega a la **Empaquetar/publicar SQL** ficha en las páginas de propiedades de proyecto.

Por ejemplo, si fuera a compilar y empaquetar el [póngase en contacto con el Administrador de](the-contact-manager-solution.md) solución de ejemplo sin tocar el proceso de parametrización de cualquier manera, el WPP esto generaría *ContactManager.Mvc.SetParameters.xml* archivo:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


En este caso:

- El **nombre de aplicación Web de IIS** parámetro es la ruta de acceso IIS en el que desea implementar la aplicación web. El valor predeterminado se toma de la **Empaquetar/Publicar Web** página en las páginas de propiedades de proyecto.
- El **cadena de conexión de ApplicationServices Web.config** parámetro se generó a partir un **connectionStrings/agregar** elemento en el *web.config* archivo. Representa la cadena de conexión que la aplicación debe usar para ponerse en contacto con la base de datos de pertenencia. El valor que proporcione aquí se pueden sustituir en implementado *web.config* archivo. El valor predeterminado se toma de la implementación previa *web.config* archivo.

El WPP parametriza también estas propiedades en el paquete de implementación genera. Puede proporcionar valores para estas propiedades cuando se instala el paquete de implementación. Si instala el paquete manualmente mediante el Administrador de IIS, como se describe en [instalación manual de paquetes de Web](manually-installing-web-packages.md), el Asistente para instalación le solicita que proporcione valores para los parámetros. Si instala el paquete de forma remota mediante la *. deploy.cmd* de archivos, como se describe en [implementar paquetes de Web](deploying-web-packages.md), Web Deploy verá esto *SetParameters.xml* del archivo a Proporcione los valores de parámetro. Puede editar los valores en el *SetParameters.xml* archivos manualmente, o puede personalizar el archivo como parte de un proceso automatizado de compilación e implementación. Este proceso se describe con más detalle más adelante en este tema.

### <a name="custom-parameterization"></a>Parametrización personalizada

En escenarios de implementación más complejos, es a menudo conveniente parametrizar propiedades adicionales antes de implementar el proyecto. Por lo general, se deben parametrizar las propiedades y los valores que varían entre los entornos de destino. Estos pueden incluir:

- Servicio de extremos en el *web.config* archivo.
- Configuración de la aplicación en el *web.config* archivo.
- Otras propiedades declarativas que se desea pedir a los usuarios especificar.

La manera más fácil de estas propiedades se parametrizan consiste en Agregar un *parameters.xml* archivo a la carpeta raíz del proyecto de aplicación web. Por ejemplo, en la solución póngase en contacto con el administrador, el proyecto ContactManager.Mvc incluye un *parameters.xml* archivo en la carpeta raíz.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Si abre este archivo, verá que contiene un único **parámetro** entrada. La entrada usa una consulta de XML Path Language (XPath) para buscar y parametrizar la dirección URL del extremo del servicio ContactService Windows Communication Foundation (WCF) en el *web.config* archivo.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Además de parametrizar la dirección URL del extremo en el paquete de implementación, el WPP también agrega una entrada correspondiente a la *SetParameters.xml* archivo que se genera junto con el paquete de implementación.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Si instala el paquete de implementación manualmente, el Administrador de IIS le solicitará la dirección del extremo de servicio junto con las propiedades que se parametriza automáticamente. Si instala el paquete de implementación mediante la ejecución de la *. deploy.cmd* archivo, puede editar la *SetParameters.xml* archivo para proporcionar un valor para la dirección del extremo de servicio junto con valores para el propiedades que se parametriza automáticamente.

Para obtener detalles completos sobre cómo crear un *parameters.xml* de archivos, consulte [Cómo: usar parámetros para configurar la configuración cuando un paquete de implementación está instalado](https://msdn.microsoft.com/library/ff398068.aspx). El procedimiento denominado **para usar los parámetros de implementación para la configuración del archivo Web.config** proporciona instrucciones paso a paso.

## <a name="modifying-the-setparametersxml-file"></a>Modificar el archivo SetParameters.xml

Si planea implementar manualmente el paquete de aplicación web & #x 2014; ya sea ejecutando el *. deploy.cmd* de archivo o ejecute MSDeploy.exe desde la línea de comandos & #x 2014; que no hay nada que evite que editar manualmente el *SetParameters.xml* archivo antes de la implementación. Sin embargo, si está trabajando en una solución empresarial, debe implementar un paquete de aplicación web como parte de un proceso de compilación e implementación automatizado, mayor. En este escenario, necesita Microsoft Build Engine (MSBuild) para modificar el *SetParameters.xml* archivo automáticamente. Puede hacerlo mediante el uso de MSBuild **XmlPoke** tarea.

El [solución de ejemplo de Contact Manager](the-contact-manager-solution.md) ilustra este proceso. Los ejemplos de código siguientes se han editado para mostrar sólo los detalles que son relevantes para este ejemplo.

> [!NOTE]
> Para obtener una descripción más amplia del modelo de archivo de proyecto en la solución de ejemplo y una introducción a los archivos de proyecto personalizadas en general, vea [comprender el archivo de proyecto](understanding-the-project-file.md) y [descripción del proceso de compilación](understanding-the-build-process.md).


En primer lugar, los valores de parámetro de interés se definen como propiedades en el archivo de proyecto específicas del entorno (por ejemplo, *Dev.proj Env*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicas del entorno para sus propios entornos de servidor, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


Después, el *Publish.proj* archivo importa estas propiedades. Porque cada *SetParameters.xml* archivo está asociado con un *. deploy.cmd* archivo y se desea que en última instancia el archivo de proyecto para invocar cada *. deploy.cmd* de archivos, el proyecto archivo crea un MSBuild *elemento* para cada *. deploy.cmd* de archivos y define las propiedades de interés como *metadatos del elemento*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


En este caso:

- El **ParametersXml** valor de metadatos indica la ubicación de la *SetParameters.xml* archivo.
- El **IisWebAppName** valor es la ruta de acceso IIS al que desea implementar la aplicación web.
- El **MembershipDBConnectionString** valor es la cadena de conexión para la base de datos de pertenencia y el **MembershipDBConnectionName** valor es el **nombre** atributo del parámetro correspondiente en el *SetParameters.xml* archivo.
- El **ServiceEndpointValue** valor es la dirección del extremo para el servicio WCF en el servidor de destino y el **ServiceEndpointParamName** valor es el atributo de nombre del parámetro correspondiente en el *SetParameters.xml* archivo.

Por último, en la *Publish.proj* archivo, el **PublishWebPackages** destino utiliza la **XmlPoke** tareas para modificar estos valores en el *SetParameters.xml* archivo.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Observará que cada **XmlPoke** tarea especifica cuatro valores de atributo:

- El **XmlInputPath** atributo indica la tarea de dónde se encuentra el archivo que desea modificar.
- El **consulta** atributo es una consulta de XPath que identifica el nodo XML que desea cambiar.
- El **valor** atributo es el nuevo valor que desea insertar en el nodo XML seleccionado.
- El **condición** atributo es los criterios en el que debe ejecutarse o no ejecutar la tarea. En estos casos, la condición se asegura de que no intenta insertar un valor nulo o está vacío en la *SetParameters.xml* archivo.

## <a name="conclusion"></a>Conclusión

En este tema se describe el rol de la *SetParameters.xml* de archivos y la explicación de cómo se genera cuando se compila un proyecto de aplicación web. Explica cómo se pueden parametrizar los valores de configuración adicionales mediante la adición de un *parameters.xml* archivo al proyecto. También se describe cómo puede modificar el *SetParameters.xml* archivo como parte de un proceso de compilación, más grandes y automatizadas, mediante el uso de la **XmlPoke** tareas en los archivos de proyecto.

El siguiente tema, [implementar paquetes de Web](deploying-web-packages.md), se describe cómo puede implementar un paquete de web ya sea ejecutando el *. deploy.cmd* de archivo o mediante el uso de MSDeploy.exe comandos directamente. En ambos casos, puede especificar su *SetParameters.xml* archivo como un parámetro de implementación.

## <a name="further-reading"></a>Información adicional

Para obtener información sobre cómo crear paquetes de web, consulte [edificio y proyectos de aplicación Web de empaquetado](building-and-packaging-web-application-projects.md). Para obtener instrucciones sobre cómo implementar realmente un paquete web, consulte [implementar paquetes de Web](deploying-web-packages.md). Para ver un tutorial paso a paso sobre cómo crear un *parameters.xml* de archivos, consulte [Cómo: usar parámetros para configurar la configuración cuando un paquete de implementación está instalado](https://msdn.microsoft.com/library/ff398068.aspx).

Para obtener información general sobre la parametrización de Web Deploy, vea [Web implementar parametrización en acción](https://go.microsoft.com/?linkid=9805119) (entrada de blog).

>[!div class="step-by-step"]
[Anterior](building-and-packaging-web-application-projects.md)
[Siguiente](deploying-web-packages.md)
