---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
title: Realizar el si implementación | Microsoft Docs
author: jrjlee
description: En este tema se describe cómo realizar "¿Qué ocurre si" (o simulados) las implementaciones que usan la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) y V...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: c711b453-01ac-4e65-a48c-93d99bf22e58
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/performing-a-what-if-deployment
msc.type: authoredcontent
ms.openlocfilehash: 054b15e1e58164ec7e85c77c39fb47bcb47879b9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827746"
---
<a name="performing-a-what-if-deployment"></a>Realización de una implementación "What If"
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo realizar "what if" (o simulados) las implementaciones que usan la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) y VSDBCMD. Esto le permite determinar los efectos de la lógica de implementación en un entorno de destino determinado antes de implementar realmente la aplicación.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación e implementación se controla mediante dos archivos de proyecto&#x2014;uno que contiene las instrucciones de compilación que se aplican a todos los entornos de destino y que contiene la configuración específica del entorno de compilación e implementación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="performing-a-what-if-deployment-for-web-packages"></a>Realización de una implementación "What If" de paquetes Web

Web Deploy incluye funcionalidad que le permite realizar implementaciones en "¿Qué ocurre si" (o versión de prueba) modo. Al implementar los artefactos en el modo "what if", Web Deploy genera un archivo de registro como si hubiera realizado la implementación, pero realmente no cambia nada en el servidor de destino. Revisar el archivo de registro puede ayudarle a comprender el impacto que la implementación tendrá en el servidor de destino, en particular:

- ¿Qué se agregarán.
- ¿Qué se actualizará.
- ¿Qué se eliminará.

Dado que una implementación "what if" realmente no cambia nada en el servidor de destino, lo que no se puede hacer siempre es predecir si una implementación se realizará correctamente.

Como se describe en [implementar paquetes de Web](../web-deployment-in-the-enterprise/deploying-web-packages.md), puede implementar paquetes de web mediante Web Deploy de dos maneras&#x2014;mediante la utilidad de línea de comandos MSDeploy.exe directamente o mediante la ejecución de la *. deploy.cmd* archivo que genera el proceso de compilación.

Si usa MSDeploy.exe directamente, puede ejecutar una implementación "what if" agregando el **– whatif** marca al comando. Por ejemplo, para evaluar qué sucedería si implementó el paquete ContactManager.Mvc.zip en un entorno de ensayo, el comando MSDeploy debe ser similar a esto:


[!code-console[Main](performing-a-what-if-deployment/samples/sample1.cmd)]


Cuando esté satisfecho con los resultados de la implementación "what if", puede quitar el **– whatif** marca para ejecutar una implementación en vivo.

> [!NOTE]
> Para obtener más información sobre las opciones de línea de comandos para MSDeploy.exe, consulte [Web implementar la configuración de operación](https://technet.microsoft.com/library/dd569089(WS.10).aspx).


Si usas el *. deploy.cmd* archivo, puede ejecutar una implementación "what if" mediante la inclusión de la **/t** marca marca (modo de prueba) en lugar de la **/y** marca ("Sí", o el modo de actualización) en el comando. Por ejemplo, para lo que sucedería si implementó el paquete ContactManager.Mvc.zip mediante la ejecución de evaluar la *. deploy.cmd* archivo, el comando debe ser similar a esto:


[!code-console[Main](performing-a-what-if-deployment/samples/sample2.cmd)]


Cuando esté satisfecho con los resultados de la implementación de "modo de prueba", puede reemplazar el **/t** marca con un **/y** marca para ejecutar una implementación activa:


[!code-console[Main](performing-a-what-if-deployment/samples/sample3.cmd)]


> [!NOTE]
> Para obtener más información sobre las opciones de línea de comandos para *. deploy.cmd* archivos, consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx). Si ejecuta el *. deploy.cmd* archivo sin especificar ningún indicador, la línea de comandos mostrará una lista de los indicadores disponibles.


## <a name="performing-a-what-if-deployment-for-databases"></a>Realización de una implementación "What If" para las bases de datos

En esta sección se da por supuesto que está usando la utilidad VSDBCMD para realizar la implementación incremental, basado en el esquema de base de datos. Este enfoque se describe con más detalle en [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Se recomienda que se familiarice con este tema antes de aplicar los conceptos descritos aquí.

Cuando usas VSDBCMD en **implementar** modo, puede usar el **/dd** (o **/DeployToDatabase**) marca para controlar si VSDBCMD implementa realmente la base de datos o simplemente genera un script de implementación. Si va a implementar un archivo .dbschema, este es el comportamiento:

- Si especifica **/dd+** o **/dd**, VSDBCMD generará un script de implementación y la implementación de la base de datos.
- Si especifica **/dd-** u omite el modificador, VSDBCMD generará un script de implementación solo.

> [!NOTE]
> Si va a implementar un archivo .deploymanifest en lugar de un archivo .dbschema, el comportamiento de la **/dd** conmutador es mucho más complicado. En esencia, VSDBCMD pasará por alto el valor de la **/dd** cambiar si el archivo .deploymanifest incluye un **DeployToDatabase** elemento con un valor de **True**. [Implementación de proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md) describe este comportamiento en su totalidad.


Por ejemplo, para generar un script de implementación para el **ContactManager** base de datos sin implementar realmente la base de datos, el comando VSDBCMD debe ser similar a esto:


[!code-console[Main](performing-a-what-if-deployment/samples/sample4.cmd)]


VSDBCMD es una herramienta de implementación de la base de datos diferencial y, por lo tanto se genera dinámicamente el script de implementación para que contenga todos los comandos SQL necesarios para actualizar la base de datos actual, si existe, al esquema especificado. Revisar el script de implementación es una manera útil para determinar qué afectar a la implementación tendrá en la base de datos actual y los datos que contiene. Por ejemplo, es posible que desee determinar:

- Si se quitarán las tablas existentes y, si esto causará pérdida de datos.
- Si el orden de las operaciones supone un riesgo de pérdida de datos, por ejemplo, si va a dividir o combinar tablas.

Si está satisfecho con el script de implementación, puede repetir el VSDBCMD con un **/dd+** indicador para realizar los cambios. Como alternativa, puede editar el script de implementación para satisfacer sus requisitos y, a continuación, ejecutarlo manualmente en el servidor de base de datos.

## <a name="integrating-what-if-functionality-into-custom-project-files"></a>Integrar la funcionalidad de "What If" en los archivos de proyecto personalizadas

En escenarios de implementación más complejos, desea usar un archivo de proyecto personalizado de Microsoft Build Engine (MSBuild) para encapsular la lógica de compilación e implementación, como se describe en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md). Por ejemplo, en el [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) , solución de ejemplo la *Publish.proj* archivo:

- Compila la solución.
- Utiliza Web Deploy para empaquetar e implementar la aplicación ContactManager.Mvc.
- Utiliza Web Deploy para empaquetar e implementar la aplicación ContactManager.Service.
- Implementa el **ContactManager** base de datos.

Al integrar la implementación de varios paquetes de web o bases de datos en un proceso paso a paso de este modo, puede también la opción de realizar toda la implementación en un modo "what if".

El *Publish.proj* archivo muestra cómo hacerlo. En primer lugar, deberá crear una propiedad para almacenar el valor "what if":


[!code-xml[Main](performing-a-what-if-deployment/samples/sample5.xml)]


En este caso, ha creado una propiedad denominada **WhatIf** con un valor predeterminado de **false**. Los usuarios pueden invalidar este valor estableciendo la propiedad en **true** en un parámetro de línea de comandos, como verá en breve.

La siguiente fase es parametrizar cualquier Web Deploy y VSDBCMD comandos para que reflejen las marcas de la **WhatIf** valor de propiedad. Por ejemplo, el destino siguiente (procedente del *Publish.proj* de archivos y simplificado) se ejecuta el *. deploy.cmd* archivo para implementar un paquete de web. De forma predeterminada, el comando incluye un **/Y** conmutador ("Sí" o modo de actualización). Si **WhatIf** está establecido en **true**, ésta se reemplazará por un **/T** conmutador (versión de prueba o modo "what if").


[!code-xml[Main](performing-a-what-if-deployment/samples/sample6.xml)]


De forma similar, el destino siguiente usa la utilidad VSDBCMD para implementar una base de datos. De forma predeterminada, un **/dd** no se incluye el modificador. Esto significa que VSDBCMD generará un script de implementación, pero no implementará la base de datos&#x2014;en otras palabras, un "what if" escenario. Si el **WhatIf** propiedad no está establecida en **true**, un **/dd** se agrega el conmutador y VSDBCMD implementará la base de datos.


[!code-xml[Main](performing-a-what-if-deployment/samples/sample7.xml)]


Puede usar el mismo enfoque para parametrizar todos los comandos pertinentes en el archivo de proyecto. Cuando desea ejecutar una implementación "what if", a continuación, puede simplemente proporcionar un **WhatIf** valor de propiedad de la línea de comandos:


[!code-console[Main](performing-a-what-if-deployment/samples/sample8.cmd)]


De este modo, puede ejecutar una implementación "what if" para todos los componentes de proyecto en un solo paso.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo ejecutar "what if" implementaciones mediante Web Deploy, VSDBCMD y MSBuild. Una implementación "what if" le permite evaluar el impacto de una implementación propuesto antes de realizar cualquier cambio en el entorno de destino.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre la sintaxis de línea de comandos de Web Deploy, consulte [Web implementar la configuración de operación](https://technet.microsoft.com/library/dd569089(WS.10).aspx). Para obtener instrucciones sobre las opciones de línea de comandos cuando se usa el *. deploy.cmd* de archivos, consulte [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd](https://msdn.microsoft.com/library/ff356104.aspx). Para obtener instrucciones sobre la sintaxis de línea de comandos VSDBCMD, consulte [referencia de línea de comandos de VSDBCMD. EXE (implementación e importación del esquema)](https://msdn.microsoft.com/library/dd193283.aspx).

> [!div class="step-by-step"]
> [Anterior](advanced-enterprise-web-deployment.md)
> [Siguiente](customizing-database-deployments-for-multiple-environments.md)
