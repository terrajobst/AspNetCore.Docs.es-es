---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: "Personalización de las implementaciones de la base de datos para varios entornos | Documentos de Microsoft"
author: jrjlee
description: "En este tema se describe cómo personalizar las propiedades de una base de datos a los entornos de destino específico como parte del proceso de implementación. Nota: El tema se da por supuesto th..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 449c448d1be237f3f95a437bb2c0415bd8ed0d99
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Personalización de las implementaciones de la base de datos para varios entornos
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo personalizar las propiedades de una base de datos a los entornos de destino específico como parte del proceso de implementación.
> 
> > [!NOTE]
> > El tema se supone que va a implementar un proyecto de base de datos de Visual Studio 2010 mediante MSBuild.exe y VSDBCMD.exe. Para obtener más información sobre por qué se puede elegir este método, consulte [implementación Web de la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) y [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Al implementar un proyecto de base de datos a varios destinos, a menudo desea personalizar las propiedades de implementación de base de datos para cada entorno de destino. Por ejemplo, en entornos de prueba, se suele volver a la base de datos en cada implementación, mientras que en los entornos de ensayo o de producción sería mucho más probable que las actualizaciones incrementales para conservar los datos.
> 
> En un proyecto de base de datos de Visual Studio 2010, configuración de implementación está dentro de un archivo de configuración (.sqldeployment) de la implementación. En este tema le mostrará cómo crear archivos de configuración de implementación específicas del entorno y especificar las restricciones que desea usar como un parámetro VSDBCMD.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales que utiliza una solución de ejemplo & #x 2014; la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar una aplicación web con un nivel de complejidad, incluso una aplicación de ASP.NET MVC 3, Windows realista Servicio de Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos proyectos, archivos de & #x 2014; uno que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

En este tema se supone:

- Usar el enfoque de archivo de proyecto de división para la implementación de la solución, como se describe en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Llamar VSDBCMD desde el archivo de proyecto para implementar el proyecto de base de datos, como se describe en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para crear un sistema de implementación que admite diferentes de las propiedades de implementación de base de datos entre los entornos de destino, necesitará:

- Crear un archivo de configuración (.sqldeployment) de implementación para cada entorno de destino.
- Crear un comando VSDBCMD que especifica el archivo de configuración de implementación como un conmutador de línea de comandos.
- Parametrizar el comando VSDBCMD en un archivo de proyecto de Microsoft Build Engine (MSBuild), para que las opciones de VSDBCMD son adecuadas para el entorno de destino.

En este tema le mostrará cómo realizar cada uno de estos procedimientos.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Crear archivos de configuración de implementación específicas del entorno

De forma predeterminada, un proyecto de base de datos contiene un archivo de configuración de implementación único denominado *Database.sqldeployment*. Si abre este archivo en Visual Studio 2010, puede ver las distintas opciones de implementación que están disponibles:

- **Intercalación de comparación de implementación**. Esto le permite elegir si desea usar la intercalación de base de datos del proyecto (la *origen* intercalación) o la intercalación de base de datos de su servidor de destino (el *destino* intercalación). En la mayoría de los casos, deseará usar la intercalación de origen cuando se implementa para un desarrollo o entorno de prueba. Cuando se implementa en un entorno de ensayo o de producción, normalmente deseará dejar la intercalación de destino sin cambios para evitar los problemas de interoperabilidad.
- **Implementar propiedades de base de datos**. Esto le permite elegir si desea aplicar las propiedades de la base de datos, como se define en el *Database.sqlsettings* archivo. Cuando se implementa una base de datos por primera vez, debe implementar las propiedades de la base de datos. Si va a actualizar una base de datos existente, las propiedades deben estar en su lugar, y no es necesario implementar de nuevo.
- **Siempre vuelva a crear la base de datos**. Este le permite seleccionar si se debe volver a crear la base de datos de destino cada vez que implemente o realiza los cambios incrementales en la base de datos de destino actualizado con el esquema. Si vuelve a crear la base de datos, se perderá cualquier dato en la base de datos existente. Por lo tanto, normalmente debería configurarlo **false** para las implementaciones en entornos de ensayo o de producción.
- **Bloquear implementación incremental si puede producirse una pérdida de datos**. Esto le permite elegir si la implementación debe detenerse si un cambio en el esquema de base de datos provocará la pérdida de datos. Normalmente se establece en **true** para una implementación en un entorno de producción, para ofrecerle la oportunidad de intervenir y proteger los datos importantes. Si ha configurado **base de datos volver a crear siempre** a **false**, esta configuración no tendrá ningún efecto.
- **Implementación se ejecuta en modo de usuario único**. Esto no es normalmente un problema en entornos de prueba o desarrollo. Sin embargo, normalmente debe establecer esto **true** para las implementaciones en entornos de ensayo o de producción. Esto impide que los usuarios realizar cambios en la base de datos mientras se está llevando a cabo la implementación.
- **Copia de seguridad base de datos antes de la implementación**. Normalmente se establece en **true** cuando se implementa en un entorno de producción, como medida de precaución contra la pérdida de datos. También puede establecerlo en **true** cuando se implementan en un entorno de ensayo, si la base de datos de ensayo contiene una gran cantidad de datos.
- **Generar instrucciones DROP para objetos que están en la base de datos de destino pero que no están en el proyecto de base de datos**. En la mayoría de los casos, esto es una parte integral y esencial de realizar los cambios incrementales en una base de datos. Si ha configurado **base de datos volver a crear siempre** a **false**, esta configuración no tendrá ningún efecto.
- **No usar instrucciones ALTER ASSEMBLY para actualizar tipos CLR**. Esta configuración determina cómo SQL Server debe actualizar tipos common language runtime (CLR) para las versiones más recientes de ensamblado. Esto debe establecerse en **false** en la mayoría de los escenarios.

Esta tabla muestra los valores de una implementación típica para distintos entornos de destino. Sin embargo, la configuración puede ser diferente dependiendo de los requisitos exactos.

|  | Desarrollador y pruebas | E integración de almacenamiento provisional | Producción |
| --- | --- | --- | --- |
| **Intercalación de comparación de implementación** | Origen | Destino | Destino |
| **Implementar propiedades de base de datos** | True | Sólo la primera vez | Sólo la primera vez |
| **Siempre vuelva a crear la base de datos** | True | False | False |
| **Bloquear implementación incremental si puede producirse una pérdida de datos** | False | Quizás | True |
| **Ejecutar script de implementación en modo de usuario único** | False | True | True |
| **Hacer copia de seguridad de base de datos antes de la implementación** | False | Quizás | True |
| **Generar instrucciones DROP para objetos que están en la base de datos de destino pero que no están en el proyecto de base de datos** | False | True | True |
| **No usar instrucciones ALTER ASSEMBLY para actualizar tipos CLR** | False | False | False |
  

> [!NOTE]
> Para obtener más información sobre propiedades de implementación de base de datos y consideraciones sobre el entorno, consulte [una información general de configuración de proyecto de base de datos](https://msdn.microsoft.com/en-us/library/aa833291(v=VS.100).aspx), [Cómo: configurar propiedades para obtener detalles de implementación](https://msdn.microsoft.com/en-us/library/dd172125.aspx), [ Compilar e implementar la base de datos en un entorno de desarrollo aislado](https://msdn.microsoft.com/en-us/library/dd193409.aspx), y [compilar e implementar bases de datos en un entorno de producción o ensayo](https://msdn.microsoft.com/en-us/library/dd193413.aspx).


Para admitir la implementación de un proyecto de base de datos a varios destinos, debe crear un archivo de configuración de implementación para cada entorno de destino.

**Para crear un archivo de configuración específicos del entorno**

1. En Visual Studio 2010, en la **el Explorador de soluciones** ventana, haga clic en el proyecto de base de datos y, a continuación, haga clic en **propiedades**.
2. En la página de propiedades del proyecto de base de datos, en la **implementar** ficha la **archivo de configuración de implementación** la fila, haga clic en **nuevo**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. En el **nuevo archivo de configuración de implementación** diálogo cuadro, asigne al archivo un nombre descriptivo (por ejemplo, **TestEnvironment.sqldeployment**) y, a continuación, haga clic en **guardar**.
4. En el *[nombreDeArchivo]***.sqldeployment** página, establezca las propiedades de implementación para que coincida con los requisitos de su entorno de destino y, a continuación, guarde el archivo.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Observe que el nuevo archivo se agrega a la carpeta de propiedades del proyecto de base de datos.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Especificar el archivo de configuración de implementación en VSDBCMD

Al usar configuraciones de solución (por ejemplo, Debug y Release) dentro de Visual Studio 2010, puede asociar un archivo de configuración de implementación con cada configuración. Cuando se compila una configuración determinada, el proceso de compilación genera un archivo de manifiesto de implementación específico de la configuración que apunta al archivo de configuración de implementación específico de la configuración. Sin embargo, uno de los principales objetivos de enfoque de implementación que se describe en estos tutoriales es proporcionar a las personas la capacidad de controlar el proceso de implementación sin usar Visual Studio 2010 y configuraciones de soluciones. En este enfoque, la configuración de soluciones es el mismo independientemente del entorno de implementación de destino. Para personalizar la implementación de la base de datos en un entorno de destino específico, puede usar las opciones de línea de comandos de VSDBCMD para especificar el archivo de configuración de implementación.

Para especificar un archivo de configuración de implementación en su VSDBCMD, use la **DeploymentConfigurationFile/p:** cambie y proporcionar la ruta de acceso completa al archivo. Esto anulará el archivo de configuración de implementación que identifica el manifiesto de implementación. Por ejemplo, podría utilizar este comando VSDBCMD para implementar la **ContactManager** base de datos en un entorno de prueba:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Tenga en cuenta que el proceso de compilación puede cambiar el nombre de su archivo .sqldeployment cuando copia el archivo en el directorio de salida.


Si utiliza variables de comandos SQL en los scripts anteriores o posteriores a la implementación de SQL, puede utilizar un enfoque similar para asociar un archivo .sqlcmdvars específicos del entorno con la implementación. En este caso, utilice la **p:/SqlCommandVariablesFile** conmutador para identificar el archivo .sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Ejecute el comando VSDBCMD desde un archivo de proyecto de MSBuild

Puede invocar un comando VSDBCMD desde un archivo de proyecto de MSBuild mediante un **Exec** tarea dentro de un destino de MSBuild. En su forma más simple, tendría este aspecto:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- En la práctica, para facilitar la lectura y volver a utilizar, los archivos de proyecto debe tener crear propiedades para almacenar los distintos parámetros de línea de comandos. Esto facilita a los usuarios para proporcionar valores de propiedad en un archivo de proyecto específicas del entorno o para reemplazar los valores predeterminados de la línea de comandos de MSBuild. Si usas el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), debe dividir la instrucciones de compilación y las propiedades entre los dos archivos según corresponda:
- Configuración específica del entorno, como el nombre de archivo de configuración de implementación, la cadena de conexión de base de datos y el nombre de base de datos de destino, debería ir en el archivo de proyecto específicos del entorno.
- El destino de MSBuild que ejecuta el comando VSDBCMD, junto con las propiedades universales como la ubicación del ejecutable VSDBCMD, debería ir en el archivo de proyecto universal.

También debe asegurarse de que se compilará el proyecto de base de datos antes de invocar VSDBCMD para que el archivo .deploymanifest se haya creado y listo para usarse. Puede ver un ejemplo completo de este enfoque en el tema [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), que le guía a través de los archivos de proyecto en el [solución de ejemplo de Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo puede personalizar propiedades de base de datos para distintos entornos de destino al implementar proyectos de base de datos mediante MSBuild y VSDBCMD. Este enfoque es útil cuando necesite implementar proyectos de base de datos como parte de mayores, soluciones de escala empresarial. Estas soluciones se implementan a menudo a varios destinos, como entornos de desarrollo o pruebas en un espacio aislado, plataformas de ensayo o de integración y producción o entornos activos. Normalmente, cada uno de estos entornos de destino requiere un único conjunto de propiedades de implementación de base de datos.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre la implementación de proyectos de base de datos mediante VSDBCMD.exe, consulte [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, consulte [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Estos artículos en MSDN proporcionan más información general sobre la implementación de la base de datos:

- [Información general sobre la configuración del proyecto de base de datos](https://msdn.microsoft.com/en-us/library/aa833291(v=VS.100).aspx)
- [Cómo: configurar las propiedades para obtener detalles de implementación](https://msdn.microsoft.com/en-us/library/dd172125.aspx)
- [Compilar e implementar bases de datos en un entorno de desarrollo aislado](https://msdn.microsoft.com/en-us/library/dd193409.aspx)
- [Compilar e implementar bases de datos en un entorno de producción o ensayo](https://msdn.microsoft.com/en-us/library/dd193413.aspx)

>[!div class="step-by-step"]
[Anterior](performing-a-what-if-deployment.md)
[Siguiente](deploying-database-role-memberships-to-test-environments.md)
