---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Implementación de proyectos de base de datos | Microsoft Docs
author: jrjlee
description: 'Nota: En muchos escenarios de implementación de enterprise, necesita la capacidad de publicar actualizaciones incrementales en una base de datos implementada. La alternativa es volver a...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 43fa197a1d5a3cf521f4d2202754ff0d121cebe3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838448"
---
<a name="deploying-database-projects"></a>Implementación de proyectos de base de datos
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> En muchos escenarios de implementación de enterprise, necesita la capacidad de publicar actualizaciones incrementales en una base de datos implementada. La alternativa consiste en volver a crear la base de datos en cada implementación, lo que significa que se pierde ningún dato en la base de datos existente. Cuando se trabaja con Visual Studio 2010, usando VSDBCMD es el enfoque recomendado para la publicación de base de datos incrementales. Sin embargo, la próxima versión de Visual Studio y la canalización de publicación de Web (WPP) incluirá las herramientas, que admite la publicación incremental directamente.


Si abre la solución de ejemplo de Contact Manager en Visual Studio 2010, verá que el proyecto de base de datos incluye una carpeta de las propiedades que contiene cuatro archivos.

![](deploying-database-projects/_static/image1.png)

Junto con el archivo de proyecto (*ContactManager.Database.dbproj* en este caso), estos archivos controlan diversos aspectos del proceso de compilación e implementación:

- El *Database.sqlcmdvars* archivo proporciona valores para las variables SQLCMD que utiliza al implementar el proyecto. Cada configuración de soluciones (por ejemplo, debug y release) puede especificar un archivo .sqlcmdvars diferente.
- El *Database.sqldeployment* archivo proporciona valores específicos de la implementación, como si se debe usar la intercalación definida en el proyecto o la intercalación del servidor de destino, si se debe volver a crear la base de datos de destino cada tiempo o simplemente rectificar la base de datos existente para ponerla al día y así sucesivamente. Cada configuración de soluciones puede especificar un archivo .sqldeployment diferentes.
- El *Database.sqlpermissions* archivo es un documento XML que puede usar para definir los permisos que desea agregar a la base de datos de destino. Todas las configuraciones de solución comparten el mismo archivo .sqlpermissions.
- El *Database.sqlsettings* archivo especifica las propiedades de nivel de base de datos que se usará al crear la base de datos, al igual que la intercalación para usar el comportamiento de los operadores de comparación y así sucesivamente. Todas las configuraciones de solución comparten el mismo archivo .sqlsettings.

Merece la pena detenerse un momento para abrir estos archivos en Visual Studio y familiarizarse con el contenido.

Cuando compila un proyecto de base de datos, el proceso de compilación crea dos archivos:

- Un *esquema de base de datos* (archivo .dbschema). Esto describe el esquema de la base de datos que desea crear en formato XML.
- Un *manifiesto de implementación* (archivo .deploymanifest). Contiene toda la información necesaria para crear e implementar la base de datos. Hace referencia al archivo de .dbschema junto con otros recursos, como las instrucciones de implementación (el archivo .sqldeployment) y los scripts SQL anteriores o posteriores a la implementación.

Esto muestra la relación entre estos recursos:

![](deploying-database-projects/_static/image2.png)

Como puede ver, el archivo .sqlsettings y el archivo .sqlpermissions son entradas para el proceso de compilación. Junto con el archivo de proyecto de base de datos, estos archivos se usan para crear el archivo de esquema de base de datos. El archivo .sqldeployment y el archivo .sqlcmdvars pasar a través del proceso de compilación sin cambios. El manifiesto de implementación indica la ubicación de los scripts SQL anteriores o posteriores a la implementación, el archivo .sqldeployment, el archivo .sqlcmdvars y el esquema de base de datos.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>¿Por qué usar VSDBCMD para implementar un proyecto de base de datos?

Hay varios enfoques diferentes para la implementación de proyectos de base de datos. Sin embargo, no todos ellos son adecuadas para implementar un proyecto de base de datos en servidores remotos en un entorno empresarial. Tenga en cuenta qué desea una implementación de proyectos de base de datos. En escenarios de implementación empresarial, es probable que desee:

- La capacidad de implementar el proyecto de base de datos desde una ubicación remota.
- La capacidad de realizar actualizaciones incrementales en una base de datos existente.
- La posibilidad de incluir secuencias de comandos de implementación previa o posterior a la implementación.
- La capacidad de personalizar la implementación en varios entornos de destino.
- La capacidad de implementar el proyecto de base de datos como parte de una mayor, normalmente crea el script, implementación de la solución paso a paso.

Existen tres enfoques principales que puede usar para implementar un proyecto de base de datos:

- Puede usar la funcionalidad de implementación con el tipo de proyecto de base de datos en Visual Studio 2010. Al compilar e implementar un proyecto de base de datos en Visual Studio 2010, el proceso de implementación usa el manifiesto de implementación para generar un archivo de implementación basada en SQL específico para la configuración de compilación. Esto creará la base de datos si ya no existe o realizar los cambios necesarios en la base de datos si ya existe. Puede utilizar SQLCMD.exe para ejecutar este archivo en el servidor de destino, o puede configurar Visual Studio para crear y ejecutar el archivo. La desventaja de este enfoque es que solo tiene limitados control sobre la configuración de implementación. A menudo también es posible que deba modificar el archivo de implementación de SQL para proporcionar valores de variables de entorno específico. Solo puede usar este enfoque desde un equipo con Visual Studio 2010 instalado y el desarrollador debería saber y proporcionar las cadenas de conexión y credenciales para todos los entornos de destino.
- Puede usar la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) para [implementar una base de datos como parte de un proyecto de aplicación web](https://msdn.microsoft.com/library/dd465343.aspx). Sin embargo, este enfoque es mucho más complejo si desea implementar un proyecto de base de datos en lugar de simplemente replicar una base de datos local existente en un servidor de destino. Puede configurar Web Deploy para ejecutar el script de implementación de SQL que genera el proyecto de base de datos, pero para ello, deberá crear un archivo de destinos WPP personalizado para el proyecto de aplicación web. Esto agrega una cantidad considerable de complejidad al proceso de implementación. Además, Web Deploy no admite directamente las actualizaciones incrementales para bases de datos existentes. Para obtener más información sobre este enfoque, consulte [ampliar la canalización de publicación Web al proyecto de base de datos del paquete implementado archivo SQL](https://go.microsoft.com/?linkid=9805121).
- Puede usar la utilidad VSDBCMD para implementar la base de datos mediante el esquema de base de datos o el manifiesto de implementación. Puede llamar a VSDBCMD.exe desde un destino de MSBuild, lo que le permite publicar bases de datos como parte de un proceso de implementación más grandes, secuencias de comandos. Puede invalidar las variables en el archivo .sqlcmdvars y muchas otras propiedades de la base de datos de un comando VSDBCMD, que le permite personalizar la implementación para diferentes entornos sin necesidad de crear varias configuraciones de compilación. VSDBCMD proporciona funcionalidad de diferenciación, lo que significa hará que solo los cambios necesarios para alinear una base de datos de destino con el esquema de base de datos. VSDBCMD también ofrece una amplia gama de opciones de línea de comandos, que le ofrecen un mayor control sobre el proceso de implementación.

En esta introducción, puede ver que el uso VSDBCMD con MSBuild es el enfoque más adecuado para un escenario de implementación empresarial típico:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| ¿Admite la implementación remota? | Sí | Sí | Sí |
| ¿Admite actualizaciones incrementales? | Sí | No | Sí |
| ¿Admite scripts anteriores o posteriores a la implementación? | Sí | Sí | Sí |
| ¿Admite la implementación de varios entorno? | Limitado | Limitado | Sí |
| ¿Admite la implementación con script? | Limitado | Sí | Sí |

El resto de este tema describe el uso de VSDBCMD con MSBuild para implementar proyectos de base de datos.

## <a name="understanding-the-deployment-process"></a>Descripción del proceso de implementación

La utilidad VSDBCMD le permite implementar una base de datos con el esquema de base de datos (el archivo .dbschema) o el manifiesto de implementación (el archivo .deploymanifest). En la práctica, casi siempre usará el manifiesto de implementación, como el manifiesto de implementación le permite proporcionar los valores predeterminados para las distintas propiedades de implementación e identificar cualquier script SQL anterior o posterior a la implementación que desea ejecutar. Por ejemplo, este comando VSDBCMD se usa para implementar el **ContactManager** base de datos a un servidor de base de datos en un entorno de prueba:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


En este caso:

- El **/a** (o **/Action**) modificador especifica qué desea VSDBCMD hacer. Puede establecerlo en **importación** o **implementar**. El **importación** opción se usa para generar un archivo .dbschema desde una base de datos existente y la **implementar** opción se usa para implementar un archivo .dbschema en una base de datos de destino.
- El **manifiesto** (o **/MANIFESTFILE**) conmutador identifica el archivo .deploymanifest que desea implementar. Si desea utilizar el archivo .dbschema en su lugar, usaría el **/modelo** (o **/ModelFile**) cambie.
- El **/CS** (o **/ConnectionString**) conmutador proporciona la cadena de conexión para el servidor de base de datos de destino. Tenga en cuenta que esto no incluye el nombre de la base de datos&#x2014;VSDBCMD necesita para conectarse al servidor para crear la base de datos; no es necesario para conectarse a una base de datos individual. Si el archivo .deploymanifest incluye una cadena de conexión, puede omitir este modificador. Si utiliza el modificador de todos modos, el valor del modificador reemplazará el valor de .deploymanifest.
- El <strong>TargetDatabase</strong> propiedad proporciona el nombre que desea asignar a la base de datos de destino durante la creación. Esto invalida el valor de la <strong>TargetDatabase</strong> propiedad en el archivo .deploymanifest. Puede usar el <strong>/p:</strong> <em>[nombre de propiedad]</em>sintaxis para establecer una amplia variedad de propiedades de implementación y para invalidar las variables SQLCMD declarado en el archivo .sqlcmdvars.
- El **/dd+** (o **/DeployToDatabase+**) modificador indica que desea crear una implementación e implementarlo en el entorno de destino. Si especifica **/dd-**, u omite el modificador, VSDBCMD generará un script de implementación, pero no implementará en el entorno de destino. Este modificador es a menudo la fuente de confusión y se explica con más detalle en la sección siguiente.
- El **/script** (o **/DeploymentScriptFile**) conmutador especifica dónde desea generar el script de implementación. Este valor no afecta el proceso de implementación.

Para obtener más información sobre VSDBCMD, consulte [referencia de línea de comandos de VSDBCMD. EXE (implementación e importación del esquema)](https://msdn.microsoft.com/library/dd193283.aspx) y [Cómo: preparar una base de datos para la implementación desde un símbolo del sistema mediante VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Para obtener un ejemplo de cómo puede utilizar VSDBCMD desde un archivo de proyecto de MSBuild, vea [descripción del proceso de compilación](understanding-the-build-process.md). Para obtener ejemplos de cómo configurar opciones de implementación de la base de datos para varios entornos, vea [personalizar implementaciones de base de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Descripción del modificador DeployToDatabase

El comportamiento de la **/dd** o **/DeployToDatabase** conmutador depende de si está usando VSDBCMD con un archivo .dbschema o un archivo .deploymanifest. Si usa un archivo .dbschema, el comportamiento es bastante sencillo:

- Si especifica **/dd+** o **/dd**, VSDBCMD generará un script de implementación y la implementación de la base de datos.
- Si especifica **/dd-** u omite el modificador, VSDBCMD generará un script de implementación solo.

Si usa un archivo .deploymanifest, el comportamiento es mucho más complicado. Esto es porque el archivo .deploymanifest contiene un nombre de propiedad **DeployToDatabase** que también determina si se ha implementado la base de datos.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


El valor de esta propiedad se establece según las propiedades del proyecto de base de datos. Si establece la **implementar acción** a **crear un script de implementación (. SQL)**, el valor será **False**. Si establece la **implementar acción** a **crear un script de implementación (. SQL) e implementar en la base de datos**, el valor será **True**.

> [!NOTE]
> Estos valores están asociados con una plataforma y configuración de compilación específica. Por ejemplo, si establece la configuración para el **depurar** configuración y, a continuación, publicar utilizando la **versión** configuración, no se usará la configuración.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> En este escenario, el **implementar acción** siempre debe establecerse en **crear un script de implementación (. SQL)**, ya que no desea que Visual Studio 2010 para implementar la base de datos. En otras palabras, el **DeployToDatabase** propiedad siempre debe ser **False**.


Cuando un **DeployToDatabase** propiedad se especifica, el **/dd** conmutador solo invalidará la propiedad si el valor de propiedad es **false**:

- Si el **DeployToDatabase** propiedad es **False**, y especificar **/dd+** o **/dd**, VSDBCMD invalidará el  **DeployToDatabase** propiedad e implementar la base de datos.
- Si el **DeployToDatabase** propiedad es **False**, y especificar **/dd-** u omite el modificador, VSDBCMD no implementará la base de datos.
- Si el **DeployToDatabase** propiedad es **True**, VSDBCMD omitirá el modificador y la implementación de la base de datos.
- Se genera un script de implementación en cada caso, independientemente de si está implementando la base de datos.

## <a name="conclusion"></a>Conclusión

En este tema se proporciona información general sobre el proceso de compilación e implementación para los proyectos de base de datos en Visual Studio 2010. También se describe cómo puede usar VSDBCMD.exe con MSBuild para admitir la implementación de la base de datos de escala empresarial.

Para obtener más información sobre cómo funciona esto en la práctica, consulte [personalizar implementaciones de base de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Información adicional

Para obtener información sobre cómo personalizar las implementaciones de la base de datos mediante la creación de un archivo de configuración de implementación independiente para cada entorno, consulte [personalizar implementaciones de base de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Para obtener instrucciones sobre cómo configurar las pertenencias a roles de base de datos ejecutando un script posterior a la implementación, consulte [implementar pertenencias a roles de base de datos en entornos de prueba](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Para obtener instrucciones acerca de cómo administrar algunos de los desafíos únicos de dicha pertenencia imponer bases de datos, vea [implementar bases de datos de pertenencia a entornos empresariales](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Estos temas en MSDN proporcionan orientación más amplio y obtener información general acerca de los proyectos de base de datos de Visual Studio y el proceso de implementación de la base de datos:

- [Proyectos de base de datos de Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx)
- [Administración de cambios de la base de datos](https://msdn.microsoft.com/library/aa833404.aspx)
- [Cómo: preparar una base de datos para la implementación desde un símbolo del sistema mediante VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Información general de implementación y compilación de la base de datos](https://msdn.microsoft.com/library/aa833165.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-web-packages.md)
> [Siguiente](creating-and-running-a-deployment-command-file.md)
