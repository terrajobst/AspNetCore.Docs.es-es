---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: "Realizar el si implementación | Documentos de Microsoft"
author: jrjlee
description: "En este tema se describe cómo realizar '¿qué ocurre si' (o de simulacro sino) implementaciones con la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) y V..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 62be7c9636fb74c40bec812e9ac76b360995da50
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="performing-a-what-if-deployment"></a>Realizar una implementación de "¿Qué ocurre si"
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo realizar "¿Qué ocurre si" (o de simulacro sino) implementaciones con la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) y VSDBCMD. Esto le permite determinar los efectos de la lógica de implementación en un entorno de destino concreto antes de implementar realmente la aplicación.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales que utiliza una solución de ejemplo & #x 2014; la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar una aplicación web con un nivel de complejidad, incluso una aplicación de ASP.NET MVC 3, Windows realista Servicio de Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación e implementación se controla mediante dos archivos de proyecto & #x 2014; o ne que contiene las instrucciones de compilación que se aplican a cada entorno de destino y la otra contiene configuración específica del entorno de compilación e implementación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Llevar a cabo una implementación de "¿Qué ocurre si" para paquetes de Web

Web Deploy incluye funcionalidad que le permite realizar implementaciones en "¿Qué ocurre si" (o versión de prueba) modo. Al implementar artefactos en modo de "¿Qué ocurre si", Web Deploy genera un archivo de registro como si hubiera realizado la implementación, pero realmente no cambia nada en el servidor de destino. Revisar el archivo de registro puede ayudarle a entender el impacto que la implementación tendrá en el servidor de destino, en concreto:

- ¿Qué se agregarán.
- ¿Qué se actualizará.
- Lo que se eliminen.

Dado que una implementación de "¿Qué ocurre si" realmente no cambia nada en el servidor de destino, lo que no se puede hacer siempre es predecir si una implementación se realizará correctamente.

Como se describe en [implementar paquetes de Web](../web-deployment-in-the-enterprise/deploying-web-packages.md), puede implementar paquetes de web con Web Deploy en dos formas & #x 2014; mediante la utilidad de línea de comandos MSDeploy.exe directamente o mediante la ejecución de la *. deploy.cmd* archivo que genera el proceso de compilación.

Si usa MSDeploy.exe directamente, puede ejecutar una implementación de "¿Qué ocurre si" agregando el **– whatif** marca al comando. Por ejemplo, para evaluar lo que sucedería si implementó el paquete de ContactManager.Mvc.zip en un entorno de ensayo, el comando MSDeploy debe ser similar a este:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Cuando esté satisfecho con los resultados de la implementación de "¿Qué ocurre si", puede quitar el **– whatif** marca para ejecutar una implementación en vivo.

> [!NOTE]
> Para obtener más información sobre las opciones de línea de comandos para MSDeploy.exe, consulte [Web Deploy operación Settings](https://technet.microsoft.com/en-us/library/dd569089(WS.10).aspx).


Si usas el *. deploy.cmd* archivo, puede ejecutar una implementación de "¿Qué ocurre si" mediante la inclusión de la **/t** flag bandera (modo de prueba) en lugar de la **/y** marca ("Sí", o el modo de actualización) en el comando. Por ejemplo, para evaluar lo que sucedería si implementó el paquete de ContactManager.Mvc.zip mediante la ejecución de la *. deploy.cmd* archivo, el comando debe ser similar a este:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Cuando esté satisfecho con los resultados de la implementación de "modo de prueba.", puede reemplazar el **/t** marca con un **/y** marca para ejecutar una implementación en vivo:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Para obtener más información sobre las opciones de línea de comandos para *. deploy.cmd* archivos, consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd](https://msdn.microsoft.com/en-us/library/ff356104.aspx). Si ejecuta el *. deploy.cmd* archivo sin especificar todas las marcas, la línea de comandos se mostrará una lista de las marcas disponibles.


## <a name="performing-a-what-if-deployment-for-databases"></a>Llevar a cabo una implementación de "¿Qué ocurre si" para las bases de datos

En esta sección se da por supuesto que está usando la utilidad VSDBCMD para realizar la implementación incremental, basada en el esquema de base de datos. Este enfoque se describe con más detalle en [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Se recomienda que se familiarice con este tema antes de aplicar los conceptos descritos aquí.

Cuando usas VSDBCMD en **implementar** modo, puede usar el **/dd** (o **/DeployToDatabase**) marca para controlar si VSDBCMD implementa realmente la base de datos o simplemente genera un script de implementación. Si va a implementar un archivo .dbschema, éste es el comportamiento:

- Si especifica **/dd+** o **/dd**, VSDBCMD se genera un script de implementación e implementar la base de datos.
- Si especifica **/dd-** u omite el modificador, VSDBCMD generará solo un script de implementación.

> [!NOTE]
> Si va a implementar un archivo .deploymanifest en lugar de un archivo .dbschema, el comportamiento de la **/dd** conmutador es mucho más complicado. En esencia, VSDBCMD pasará por alto el valor de la **/dd** cambiar si el archivo .deploymanifest incluye un **DeployToDatabase** elemento con un valor de **True**. [Implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md) describe este comportamiento en su totalidad.


Por ejemplo, para generar un script de implementación para la **ContactManager** base de datos sin implementación real de la base de datos, el comando VSDBCMD debe ser similar a este:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD es una herramienta de implementación de base de datos diferencial y, por lo tanto se genera dinámicamente el script de implementación para que contenga todos los comandos SQL necesarios para actualizar la base de datos actual, si la hay, al esquema especificado. Revisar el script de implementación es una manera útil para determinar lo que afecta a la implementación tendrá en la base de datos actual y los datos que contiene. Por ejemplo, quizá desee determinar:

- Si se va a quitar las tablas existentes y, si esto causará pérdida de datos.
- Si el orden de las operaciones supone un riesgo de pérdida de datos, por ejemplo, si va a dividir o combinar tablas.

Si está satisfecho con el script de implementación, puede repetir el VSDBCMD con un **/dd+** marca para realizar los cambios. Como alternativa, puede editar el script de implementación para satisfacer sus requisitos y, a continuación, ejecutarlo manualmente en el servidor de base de datos.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrar la funcionalidad de "¿Qué ocurre si" en los archivos de proyecto personalizadas

En escenarios de implementación más complejos, es conveniente usar un archivo de proyecto personalizado de Microsoft Build Engine (MSBuild) para encapsular la lógica de compilación e implementación, como se describe en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Por ejemplo, en la [póngase en contacto con el Administrador de](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución, de ejemplo del *Publish.proj* archivo:

- Compile la solución.
- Utiliza Web Deploy para empaquetar e implementar la aplicación ContactManager.Mvc.
- Utiliza Web Deploy para empaquetar e implementar la aplicación ContactManager.Service.
- Implementa el **ContactManager** base de datos.

Al integrar la implementación de varios paquetes de web o bases de datos en un proceso paso a paso de esta manera, puede también la opción de realizar toda la implementación en modo de "¿Qué ocurre si".

El *Publish.proj* archivo muestra cómo hacerlo. En primer lugar, debe crear una propiedad para almacenar el valor "¿Qué ocurre si":


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


En este caso, ha creado una propiedad denominada **WhatIf** con un valor predeterminado de **false**. Los usuarios pueden invalidar este valor estableciendo la propiedad en **true** en un parámetro de línea de comandos, como verá en breve.

La siguiente fase es parametrizar cualquier Web Deploy y VSDBCMD comandos para que reflejen las marcas de la **WhatIf** valor de propiedad. Por ejemplo, el destino siguiente (procedente del *Publish.proj* de archivos y se ha simplificado) se ejecuta la *. deploy.cmd* archivo para implementar un paquete de web. De forma predeterminada, el comando incluye una **/Y** conmutador ("Sí" o modo de actualización). Si **WhatIf** está establecido en **true**, ésta se reemplazará por un **/T** conmutador (versión de prueba o modo de "¿Qué ocurre si").


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


De forma similar, el destino siguiente utiliza la utilidad VSDBCMD para implementar una base de datos. De forma predeterminada, un **/dd** no se incluye el modificador. Esto significa que VSDBCMD generará un script de implementación, pero no implementará la base de datos & #x 2014; es decir, un "¿Qué ocurre si" escenario. Si el **WhatIf** propiedad no está establecida en **true**, **/dd** se agrega el conmutador y VSDBCMD implementará la base de datos.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


Puede utilizar el mismo enfoque para parametrizar todos los comandos relevantes en el archivo de proyecto. Si desea ejecutar una implementación de "¿Qué ocurre si", a continuación, puede simplemente proporcionar un **WhatIf** valor de la propiedad de la línea de comandos:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


De esta manera, puede ejecutar una implementación de "¿Qué ocurre si" para todos los componentes de proyecto en un solo paso.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo ejecutar "¿Qué ocurre si" implementaciones mediante Web Deploy, VSDBCMD y MSBuild. Una implementación de "¿Qué ocurre si" le permite evaluar el impacto de una implementación propuesto antes de implementar los cambios en el entorno de destino.

## <a name="further-reading"></a>Información adicional

Para obtener más información acerca de la sintaxis de línea de comandos de Web Deploy, vea [Web Deploy operación Settings](https://technet.microsoft.com/en-us/library/dd569089(WS.10).aspx). Para obtener información sobre las opciones de línea de comandos cuando se usa el *. deploy.cmd* de archivos, consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd](https://msdn.microsoft.com/en-us/library/ff356104.aspx). Para obtener instrucciones acerca de la sintaxis de línea de comandos de VSDBCMD, consulte [referencia de línea de comandos de VSDBCMD. EXE (implementación e importación del esquema)](https://msdn.microsoft.com/en-us/library/dd193283.aspx).

>[!div class="step-by-step"]
[Anterior](advanced-enterprise-web-deployment.md)
[Siguiente](customizing-database-deployments-for-multiple-environments.md)
