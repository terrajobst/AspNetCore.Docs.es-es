---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
title: Implementar proyectos de base de datos | Documentos de Microsoft
author: jrjlee
description: "Nota: En una gran cantidad de escenarios de implementación empresarial, necesita la capacidad para publicar actualizaciones incrementales en una base de datos implementada. La alternativa es volver a..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 832f226a-1aa3-4093-8c29-ce4196793259
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/deploying-database-projects
msc.type: authoredcontent
ms.openlocfilehash: 9b1f9a19c76e33b5d996cb4d562cf0c1a3e2f83b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="deploying-database-projects"></a>Implementar proyectos de base de datos
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> [!NOTE]
> Una gran cantidad de escenarios de implementación empresarial, necesita la capacidad para publicar actualizaciones incrementales en una base de datos implementada. La alternativa consiste en volver a crear la base de datos en cada implementación, lo que significa que se perderá ningún dato en la base de datos existente. Cuando se trabaja con Visual Studio 2010, usando VSDBCMD es el enfoque recomendado para la publicación de bases de datos incrementales. Sin embargo, la próxima versión de Visual Studio y la canalización de publicación de Web (WPP) incluye herramientas que admite directamente la publicación incremental.


Si la solución de ejemplo de administrador de contacto se abre en Visual Studio 2010, verá que el proyecto de base de datos incluye una carpeta de propiedades que contiene cuatro archivos.

![](deploying-database-projects/_static/image1.png)

Junto con el archivo de proyecto (*ContactManager.Database.dbproj* en este caso), estos archivos controlan diversos aspectos del proceso de compilación e implementación:

- El *Database.sqlcmdvars* archivo proporciona valores para las variables SQLCMD que utiliza al implementar el proyecto. Cada configuración de soluciones (por ejemplo, debug y release) puede especificar un archivo .sqlcmdvars diferente.
- El *Database.sqldeployment* archivo proporciona la configuración específica de la implementación, como si se utiliza la intercalación definida en el proyecto o la intercalación del servidor de destino, si se debe volver a crear la base de datos de destino cada hora o basta con modificar la base de datos existente para poner al día y así sucesivamente. Cada configuración de soluciones puede especificar un archivo .sqldeployment diferentes.
- El *Database.sqlpermissions* archivo es un documento XML que puede utilizar para definir los permisos que se va a agregar a la base de datos de destino. Todas las configuraciones de solución comparten el mismo archivo .sqlpermissions.
- El *Database.sqlsettings* archivo especifica las propiedades de nivel de base de datos que se usará al crear la base de datos, como la intercalación que utiliza, el comportamiento de los operadores de comparación y así sucesivamente. Todas las configuraciones de solución comparten el mismo archivo .sqlsettings.

Merece la pena dedicar unos instantes a abrir estos archivos en Visual Studio y familiarizarse con el contenido.

Cuando compila un proyecto de base de datos, el proceso de compilación crea dos archivos:

- A *esquema de base de datos* (archivo .dbschema). Describe el esquema de la base de datos que desea crear en formato XML.
- A *manifiesto de implementación* (archivo .deploymanifest). Contiene toda la información necesaria para crear e implementar la base de datos. Hace referencia al archivo .dbschema junto con otros recursos, como las instrucciones de implementación (el archivo .sqldeployment) y los scripts SQL anteriores o posteriores a la implementación.

Esto muestra la relación entre estos recursos:

![](deploying-database-projects/_static/image2.png)

Como puede ver, el archivo .sqlsettings y el archivo .sqlpermissions son entradas para el proceso de compilación. Junto con el archivo de proyecto de base de datos, estos archivos se usan para crear el archivo de esquema de base de datos. El archivo .sqldeployment y el archivo .sqlcmdvars pasar a través del proceso de compilación sin cambios. El manifiesto de implementación indica la ubicación del esquema de base de datos, el archivo .sqldeployment, el archivo .sqlcmdvars y los scripts SQL anteriores o posteriores a la implementación.

## <a name="why-use-vsdbcmd-to-deploy-a-database-project"></a>¿Por qué usar VSDBCMD para implementar un proyecto de base de datos?

Hay varios enfoques diferentes para implementar proyectos de base de datos. Sin embargo, no todas ellas son adecuados para implementar un proyecto de base de datos en servidores remotos en un entorno empresarial. Tenga en cuenta que desee de una implementación de proyectos de base de datos. En escenarios de implementación empresarial, es probable que se desee:

- La capacidad de implementar el proyecto de base de datos desde una ubicación remota.
- La capacidad de realizar actualizaciones incrementales en una base de datos existente.
- La capacidad de incluir secuencias de comandos de implementación previa o posterior a la implementación.
- La capacidad para personalizar la implementación en varios entornos de destino.
- La capacidad de implementar el proyecto de base de datos como parte de una mayor normalmente genera un script, implementación de la solución de paso a paso.

Hay tres maneras principales para implementar un proyecto de base de datos:

- Puede usar la funcionalidad de implementación con el tipo de proyecto de base de datos en Visual Studio 2010. Al compilar e implementar un proyecto de base de datos en Visual Studio 2010, el proceso de implementación utiliza el manifiesto de implementación para generar un archivo de implementación basado en SQL específico de la configuración de compilación. Si ya no existe o realizar los cambios necesarios en la base de datos si ya existe, se creará la base de datos. Puede utilizar SQLCMD.exe para ejecutar este archivo en el servidor de destino, o puede configurar Visual Studio para crear y ejecutar el archivo. La desventaja de este enfoque es que solo tiene limitados control sobre la configuración de implementación. A menudo también tendrá que modificar el archivo de implementación de SQL para proporcionar los valores de las variables específicas del entorno. Solo se puede utilizar este enfoque desde un equipo con Visual Studio 2010 instalado, y el desarrollador tendría que conocer y proporcionar las cadenas de conexión y credenciales para todos los entornos de destino.
- Puede usar la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) a [implementar una base de datos como parte de un proyecto de aplicación web](https://msdn.microsoft.com/library/dd465343.aspx). Sin embargo, este enfoque es mucho más complejo si desea implementar un proyecto de base de datos en lugar de simplemente replicar una base de datos local existente en un servidor de destino. También puede configurar Web Deploy para ejecutar el script de implementación de SQL que genera el proyecto de base de datos, pero para ello, debe crear un archivo de destinos WPP personalizado para el proyecto de aplicación web. Esto agrega una cantidad considerable de complejidad al proceso de implementación. Además, Web Deploy no admite directamente las actualizaciones incrementales para bases de datos existentes. Para obtener más información sobre este enfoque, consulte [extender la canalización de publicación de Web al proyecto de base de datos del paquete implementado archivo SQL](https://go.microsoft.com/?linkid=9805121).
- Puede usar la utilidad VSDBCMD para implementar la base de datos mediante el esquema de base de datos o el manifiesto de implementación. Puede llamar a VSDBCMD.exe desde un destino de MSBuild, que permite la publicación de bases de datos como parte de un proceso de implementación más grandes, con scripts. Puede invalidar las variables en el archivo .sqlcmdvars y una gran cantidad de otras propiedades de la base de datos de un comando VSDBCMD, que le permite personalizar la implementación para diferentes entornos sin necesidad de crear varias configuraciones de compilación. VSDBCMD proporciona funcionalidad de diferenciación, lo que significa que pueda realizar únicamente los cambios necesarios para alinear una base de datos de destino con el esquema de base de datos. VSDBCMD también ofrece una amplia gama de opciones de línea de comandos, que le ofrecen un control más preciso sobre el proceso de implementación.

En esta información general, puede ver que el uso VSDBCMD con MSBuild es el enfoque que mejor se adapte a un escenario de implementación típicos de empresas:

|  | Visual Studio 2010 | Web Deploy 2.0 | VSDBCMD.exe |
| --- | --- | --- | --- |
| ¿Admite la implementación remota? | Sí | Sí | Sí |
| ¿Admite actualizaciones incrementales? | Sí | No | Sí |
| ¿Admite scripts anteriores o posteriores a la implementación? | Sí | Sí | Sí |
| ¿Admite la implementación de varios entorno? | Limitado | Limitado | Sí |
| ¿Admite la implementación con script? | Limitado | Sí | Sí |

El resto de este tema describe el uso de VSDBCMD con MSBuild para implementar proyectos de base de datos.

## <a name="understanding-the-deployment-process"></a>Descripción del proceso de implementación

La utilidad VSDBCMD le permite implementar una base de datos con el esquema de base de datos (el archivo .dbschema) o el manifiesto de implementación (el archivo .deploymanifest). En la práctica, casi siempre usará el manifiesto de implementación, como el manifiesto de implementación le permite proporcionar los valores predeterminados para diversas propiedades de implementación e identificar cualquier script SQL anteriores o posteriores a la implementación que desea ejecutar. Por ejemplo, se usa este comando VSDBCMD para implementar la **ContactManager** base de datos a un servidor de base de datos en un entorno de prueba:


[!code-console[Main](deploying-database-projects/samples/sample1.cmd)]


En este caso:

- El **/a** (o **/Action**) modificador especifica la acción que realizará VSDBCMD hacer. También puede establecerlo en **importación** o **implementar**. El **importación** opción se usa para generar un archivo .dbschema desde una base de datos existente y la **implementar** opción se utiliza para implementar un archivo .dbschema en una base de datos de destino.
- El **/manifest** (o **/MANIFESTFILE**) conmutador identifica el archivo .deploymanifest que desea implementar. Si desea utilizar el archivo .dbschema en su lugar, utilice la **/modelo** (o **/ModelFile**) cambiar.
- El **/cs** (o **bien**) conmutador proporciona la cadena de conexión para el servidor de base de datos de destino. Tenga en cuenta que esto no incluye el nombre de la base de datos & #x 2014; VSDBCMD necesita para conectarse al servidor para crear la base de datos; no es necesario conectarse a una base de datos individual. Si el archivo .deploymanifest incluye una cadena de conexión, puede omitir este modificador. Si utiliza el modificador de todos modos, el valor del modificador invalidará el valor .deploymanifest.
- El **/p: TargetDatabase** propiedad proporciona el nombre que desea asignar a la base de datos de destino durante la creación. Esto reemplaza el valor de la **TargetDatabase** propiedad en el archivo .deploymanifest. Puede usar el **/p:** *[nombre de la propiedad]*sintaxis para establecer una gran variedad de propiedades de implementación y para invalidar las variables SQLCMD declarados en el archivo .sqlcmdvars.
- El **/dd+** (o **/DeployToDatabase+**) conmutador indica que desea crear una implementación e implementarlo en el entorno de destino. Si especifica **/dd-**, u omite el modificador, VSDBCMD generará un script de implementación, pero no implementará en el entorno de destino. Este modificador es a menudo el origen de confusión y se explica con más detalle en la sección siguiente.
- El **/script** (o **/DeploymentScriptFile**) modificador especifica dónde desea generar el script de implementación. Este valor no afecta el proceso de implementación.

Para obtener más información sobre VSDBCMD, consulte [referencia de línea de comandos de VSDBCMD. EXE (implementación e importación del esquema)](https://msdn.microsoft.com/library/dd193283.aspx) y [Cómo: preparar una base de datos para la implementación desde un símbolo del sistema mediante VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx).

Para obtener un ejemplo de cómo puede utilizar VSDBCMD desde un archivo de proyecto de MSBuild, vea [descripción del proceso de compilación](understanding-the-build-process.md). Para obtener ejemplos de cómo configurar opciones de implementación de base de datos para varios entornos, vea [personalizar implementaciones de base de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="understanding-the-deploytodatabase-switch"></a>Descripción del conmutador DeployToDatabase

El comportamiento de la **/dd** o **/DeployToDatabase** conmutador depende de si está usando VSDBCMD con un archivo .dbschema o un archivo .deploymanifest. Si está usando un archivo .dbschema, el comportamiento es bastante sencillo:

- Si especifica **/dd+** o **/dd**, VSDBCMD se genera un script de implementación e implementar la base de datos.
- Si especifica **/dd-** u omite el modificador, VSDBCMD generará solo un script de implementación.

Si está usando un archivo .deploymanifest, el comportamiento es mucho más complicado. Esto es porque el archivo .deploymanifest contiene un nombre de propiedad **DeployToDatabase** que también determina si se implementa la base de datos.


[!code-xml[Main](deploying-database-projects/samples/sample2.xml)]


El valor de esta propiedad se establece según las propiedades del proyecto de base de datos. Si establece la **implementar acción** a **crear un script de implementación (. SQL)**, el valor será **False**. Si establece la **implementar acción** a **crear un script de implementación (. SQL) e implementar en la base de datos**, el valor será **True**.

> [!NOTE]
> Estos valores están asociados con una plataforma y la configuración de compilación concreta. Por ejemplo, si establece la configuración para la **depurar** configuración y, a continuación, publicar utilizando la **versión** configuración, no se utilizará la configuración.


![](deploying-database-projects/_static/image3.png)

> [!NOTE]
> En este escenario, el **implementar acción** debe establecerse siempre en **crear un script de implementación (. SQL)**, ya que no desea que Visual Studio 2010 para implementar la base de datos. En otras palabras, el **DeployToDatabase** propiedad siempre debe ser **False**.


Cuando un **DeployToDatabase** propiedad se especifica, el **/dd** conmutador solo invalidará la propiedad si el valor de propiedad es **false**:

- Si el **DeployToDatabase** propiedad es **False**, y especificar **/dd+** o **/dd**, VSDBCMD invalidará el  **DeployToDatabase** propiedad e implementar la base de datos.
- Si el **DeployToDatabase** propiedad es **False**, y especificar **/dd-** u omite el modificador, VSDBCMD no implementará la base de datos.
- Si el **DeployToDatabase** propiedad es **True**, VSDBCMD omitirá el conmutador e implementar la base de datos.
- Se genera un script de implementación en cada caso, independientemente de si va a implementar la base de datos.

## <a name="conclusion"></a>Conclusión

En este tema se proporciona información general sobre el proceso de compilación e implementación para los proyectos de base de datos en Visual Studio 2010. También se describe cómo puede usar VSDBCMD.exe con MSBuild para admitir la implementación de la base de datos de escala empresarial.

Para obtener más información sobre su funcionamiento en la práctica, consulte [personalizar implementaciones de base de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md).

## <a name="further-reading"></a>Información adicional

Para obtener información sobre cómo personalizar las implementaciones de la base de datos mediante la creación de un archivo de configuración de implementación independiente para cada entorno, consulte [personalizar implementaciones de base de datos para varios entornos](../advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments.md). Para obtener instrucciones sobre cómo configurar las pertenencias a roles de base de datos ejecutando un script posterior a la implementación, consulte [implementar pertenencias a roles de base de datos a los entornos de prueba](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Para obtener instrucciones sobre la administración de algunos de los desafíos únicos dicha pertenencia imponer bases de datos, vea [implementar bases de datos de pertenencia a los entornos empresariales](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

Estos temas en MSDN proporcionan instrucciones más amplio y obtener información general sobre los proyectos de base de datos de Visual Studio y el proceso de implementación de base de datos:

- [Proyectos de base de datos de Visual Studio 2010 SQL Server](https://msdn.microsoft.com/library/ff678491.aspx)
- [Administración de cambios de base de datos](https://msdn.microsoft.com/library/aa833404.aspx)
- [Cómo: preparar una base de datos para la implementación desde un símbolo del sistema mediante VSDBCMD. EXE](https://msdn.microsoft.com/library/dd193258.aspx)
- [Información general de implementación y compilación de la base de datos](https://msdn.microsoft.com/library/aa833165.aspx)

>[!div class="step-by-step"]
[Anterior](deploying-web-packages.md)
[Siguiente](creating-and-running-a-deployment-command-file.md)
