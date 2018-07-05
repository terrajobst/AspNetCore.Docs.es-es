---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
title: Configurar parámetros para la implementación de paquete de Web | Microsoft Docs
author: jrjlee
description: Este tema describe cómo establecer los valores de parámetro, como nombres de aplicaciones web de Internet Information Services (IIS), las cadenas de conexión y los puntos de conexión de servicio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 37947d79-ab1e-4ba9-9017-52e7a2757414
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/configuring-parameters-for-web-package-deployment
msc.type: authoredcontent
ms.openlocfilehash: e6db7a8351e01bbbc14eb2b993248ee7d5a84f7e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386554"
---
<a name="configuring-parameters-for-web-package-deployment"></a>Configurar parámetros para la implementación de paquete de Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo establecer los valores de parámetro, como nombres de aplicaciones web de Internet Information Services (IIS), las cadenas de conexión y puntos de conexión de servicio, al implementar un paquete web en un servidor web IIS remoto.


Cuando crea un proyecto de aplicación web, la compilación y el proceso de empaquetado genera tres archivos de clave:

- Un *.zip [nombre de proyecto]* archivo. Se trata de un paquete de implementación web para el proyecto de aplicación web. Este paquete contiene todos los ensamblados, archivos, scripts de base de datos y los recursos necesarios para volver a crear la aplicación web en un servidor web IIS remoto.
- Un *.deploy.cmd [nombre de proyecto]* archivo. Contiene un conjunto de comandos de Web Deploy (MSDeploy.exe) con parámetros que publicar el paquete de implementación web en un servidor web IIS remoto.
- Un *[nombre de proyecto]. SetParameters.xml* archivo. Esto proporciona un conjunto de valores de parámetro para el comando MSDeploy.exe. Puede actualizar los valores de este archivo y pasarlo a Web Deploy como un parámetro de línea de comandos al implementar el paquete de web.

> [!NOTE]
> Para obtener más información sobre el proceso de empaquetado y la compilación, consulte [compilar y empaquetar proyectos de aplicación Web](building-and-packaging-web-application-projects.md).


El *SetParameters.xml* archivo se genera dinámicamente desde el archivo de proyecto de aplicación web y los archivos de configuración dentro de su proyecto. Cuando se compila y empaqueta el proyecto, la canalización de publicación de Web (WPP) detectará automáticamente muchas de las variables que están probables que cambien entre entornos de implementación, como el destino de la aplicación web de IIS y las cadenas de conexión de base de datos. Estos valores se parametrizada en el paquete de implementación web automáticamente y se agregan a la *SetParameters.xml* archivo. Por ejemplo, si agrega una cadena de conexión para el *web.config* archivo en el proyecto de aplicación web, el proceso de compilación lo detectará este cambio y agregará una entrada para el *SetParameters.xml* archivo en consecuencia.

En muchos casos, esta parametrización automática será suficiente. Sin embargo, si necesitan modificar otras configuraciones entre entornos de implementación, como la configuración de la aplicación o direcciones URL del punto de conexión de servicio, los usuarios necesitan saber el WPP parametrizar estos valores en el paquete de implementación y agregar las entradas correspondientes a la *SetParameters.xml* archivo. Las secciones siguientes explican cómo hacerlo.

### <a name="automatic-parameterization"></a>Parametrización automática

Al compilar y empaquetar una aplicación web, el WPP parametrizar automáticamente estas cosas:

- El destino de IIS web nombre y ruta de la aplicación.
- Las cadenas de cualquier conexión en su *web.config* archivo.
- Cadenas de conexión para las bases de datos se agrega a la **Empaquetar/publicar SQL** ficha en las páginas de propiedades del proyecto.

Por ejemplo, si desea compilar y empaquetar la [Contact Manager](the-contact-manager-solution.md) solución de ejemplo sin tocar el proceso de parametrización de cualquier manera, el WPP esto generaría *ContactManager.Mvc.SetParameters.xml* archivo:


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample1.xml)]


En este caso:

- El **nombre de aplicación Web de IIS** parámetro es la ruta de acceso IIS en el que desea implementar la aplicación web. El valor predeterminado se toma de la **Empaquetar/Publicar Web** página en las páginas de propiedades del proyecto.
- El **ApplicationServices-Web.config Connection String** parámetro se generó a partir un **connectionStrings/agregar** elemento en el *web.config* archivo. Representa la cadena de conexión que la aplicación debe usar para ponerse en contacto con la base de datos de pertenencia. El valor que proporcione aquí será sustituido en implementado *web.config* archivo. El valor predeterminado se toma de la implementación previa *web.config* archivo.

El WPP parametriza también estas propiedades en el paquete de implementación que genera. Puede proporcionar valores para estas propiedades cuando se instala el paquete de implementación. Si instala el paquete manualmente mediante el Administrador de IIS, como se describe en [instalar manualmente los paquetes Web](manually-installing-web-packages.md), el Asistente para instalación le solicita que proporcione valores para los parámetros. Si instala el paquete de forma remota mediante la *. deploy.cmd* de archivos, como se describe en [implementar paquetes de Web](deploying-web-packages.md), Web Deploy verá esto *SetParameters.xml* del archivo a Proporcione los valores de parámetro. Puede editar los valores de la *SetParameters.xml* archivos manualmente, o puede personalizar el archivo como parte de un proceso automatizado de compilación e implementación. Este proceso se describe con más detalle más adelante en este tema.

### <a name="custom-parameterization"></a>Parametrización personalizada

En escenarios de implementación más complejos, con frecuencia deseará parametrizar propiedades adicionales antes de implementar el proyecto. Por lo general, se deben parametrizar las propiedades y los valores que puede variar entre los entornos de destino. Estos pueden incluir:

- En los puntos de conexión de servicio la *web.config* archivo.
- Configuración de la aplicación en el *web.config* archivo.
- Otras propiedades declarativas que se desean pedir a los usuarios especificar.

Estas propiedades se parametrizan de la manera más fácil consiste en Agregar un *parameters.xml* archivo a la carpeta raíz del proyecto de aplicación web. Por ejemplo, en la solución Contact Manager, el proyecto ContactManager.Mvc incluye un *parameters.xml* archivo en la carpeta raíz.

![](configuring-parameters-for-web-package-deployment/_static/image1.png)

Si abre este archivo, verá que contiene un único **parámetro** entrada. La entrada usa una consulta XML Path Language (XPath) para localizar y parametrizar la URL del extremo del servicio ContactService Windows Communication Foundation (WCF) en el *web.config* archivo.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample2.xml)]


Además de parametrizar la URL del extremo en el paquete de implementación, el WPP también agrega una entrada correspondiente a la *SetParameters.xml* archivo que se genera junto con el paquete de implementación.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample3.xml)]


Si instala manualmente el paquete de implementación, el Administrador de IIS le solicitará la dirección del punto de conexión de servicio junto con las propiedades que se parametriza automáticamente. Si instala el paquete de implementación mediante la ejecución de la *. deploy.cmd* archivo, puede editar el *SetParameters.xml* archivo para proporcionar un valor para la dirección del punto de conexión de servicio junto con los valores de la propiedades que se parametriza automáticamente.

Para obtener detalles sobre cómo crear un *parameters.xml* de archivos, consulte [Cómo: usar parámetros para configurar la configuración cuando un paquete de implementación está instalado](https://msdn.microsoft.com/library/ff398068.aspx). El procedimiento llamado **para usar los parámetros de implementación para la configuración del archivo Web.config** proporciona instrucciones paso a paso.

## <a name="modifying-the-setparametersxml-file"></a>Modificación del archivo SetParameters.xml

Si tiene previsto implementar el paquete de aplicación web manualmente&#x2014;ya sea ejecutando el *. deploy.cmd* archivo o ejecutando MSDeploy.exe desde la línea de comandos&#x2014;no hay nada que evite que editar manualmente el  *SetParameters.xml* archivo antes de la implementación. Sin embargo, si está trabajando en una solución empresarial, es posible que deba implementar un paquete de aplicación web como parte del proceso de compilación e implementación automatizado, mayor. En este escenario, se necesita Microsoft Build Engine (MSBuild) para modificar el *SetParameters.xml* archivo automáticamente. Puede hacerlo mediante el uso de la versión de MSBuild **XmlPoke** tarea.

El [solución de ejemplo de Contact Manager](the-contact-manager-solution.md) ilustra este proceso. Los ejemplos de código siguientes se han editado para mostrar solo los detalles que son relevantes para este ejemplo.

> [!NOTE]
> Información general más amplia del modelo de archivo de proyecto en la solución de ejemplo y una introducción a los archivos de proyecto personalizadas en general, consulte [descripción del archivo de proyecto](understanding-the-project-file.md) y [descripción del proceso de compilación](understanding-the-build-process.md).


En primer lugar, los valores de parámetro de interés se definen como propiedades en el archivo de proyecto específicos del entorno (por ejemplo, *Env-Dev.proj*).


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample4.xml)]


> [!NOTE]
> Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicos del entorno para sus propios entornos de servidor, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).


A continuación, el *Publish.proj* archivo importa estas propiedades. Porque cada *SetParameters.xml* archivo está asociado con un *. deploy.cmd* archivo y se desea en última instancia, el archivo de proyecto para invocar cada *. deploy.cmd* archivo del proyecto archivo crea un MSBuild *elemento* para cada *. deploy.cmd* de archivo y define las propiedades de interés como *metadatos del elemento*.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample5.xml)]


En este caso:

- El **ParametersXml** metadatos valor indica la ubicación de la *SetParameters.xml* archivo.
- El **IisWebAppName** valor es la ruta de acceso IIS al que desea implementar la aplicación web.
- El **MembershipDBConnectionString** valor es la cadena de conexión para la base de datos de pertenencia y la **MembershipDBConnectionName** valor es el **nombre** atributo del parámetro correspondiente en el *SetParameters.xml* archivo.
- El **ServiceEndpointValue** valor es la dirección del extremo para el servicio WCF en el servidor de destino y el **ServiceEndpointParamName** valor es el atributo de nombre del parámetro correspondiente en el *SetParameters.xml* archivo.

Por último, en el *Publish.proj* archivo, el **PublishWebPackages** usa como destino el **XmlPoke** tareas para modificar estos valores en el *SetParameters.xml* archivo.


[!code-xml[Main](configuring-parameters-for-web-package-deployment/samples/sample6.xml)]


Observará que cada **XmlPoke** tarea especifica cuatro valores de atributo:

- El **XmlInputPath** atributo indica a la tarea dónde encontrar el archivo que desea modificar.
- El **consulta** atributo es una consulta XPath que identifica el nodo XML que desea cambiar.
- El **valor** atributo es el nuevo valor que desea insertar en el nodo XML.
- El **condición** atributo es los criterios en el que debe ejecutar o no ejecutar la tarea. En estos casos, la condición garantiza que no intente insertar un valor nulo o está vacío en el *SetParameters.xml* archivo.

## <a name="conclusion"></a>Conclusión

En este tema se describe el rol de la *SetParameters.xml* de archivos y se explica cómo se genera al compilar un proyecto de aplicación web. Asimismo, explica cómo se puede parametrizar una configuración adicional mediante la adición de un *parameters.xml* archivo al proyecto. También se describe cómo se puede modificar el *SetParameters.xml* archivo como parte de un proceso de compilación automatizada, mayor, mediante el uso de la **XmlPoke** tareas en los archivos de proyecto.

El tema siguiente, [implementar paquetes de Web](deploying-web-packages.md), describe cómo puede implementar un paquete web ya sea ejecutando el *. deploy.cmd* de archivos o utilizando MSDeploy.exe comandos directamente. En ambos casos, puede especificar su *SetParameters.xml* archivo como un parámetro de implementación.

## <a name="further-reading"></a>Información adicional

Para obtener información sobre cómo crear paquetes de web, consulte [compilar y empaquetar proyectos de aplicación Web](building-and-packaging-web-application-projects.md). Para obtener instrucciones sobre cómo implementar un paquete de web, consulte [implementar paquetes de Web](deploying-web-packages.md). Para ver un tutorial paso a paso sobre cómo crear un *parameters.xml* de archivos, consulte [Cómo: usar parámetros para configurar la configuración cuando un paquete de implementación está instalado](https://msdn.microsoft.com/library/ff398068.aspx).

Para obtener información general sobre la parametrización de Web Deploy, consulte [parametrización de implementación Web en acción](https://go.microsoft.com/?linkid=9805119) (entrada de blog).

> [!div class="step-by-step"]
> [Anterior](building-and-packaging-web-application-projects.md)
> [Siguiente](deploying-web-packages.md)
