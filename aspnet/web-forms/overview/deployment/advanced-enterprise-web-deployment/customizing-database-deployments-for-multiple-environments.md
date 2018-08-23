---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
title: Personalización de las implementaciones de base de datos para varios entornos | Microsoft Docs
author: jrjlee
description: 'Este tema describe cómo personalizar las propiedades de una base de datos en entornos de destino específico como parte del proceso de implementación. Nota: En el tema se supone th...'
ms.author: riande
ms.date: 05/04/2012
ms.assetid: a172979a-1318-4318-a9c6-4f9560d26267
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/customizing-database-deployments-for-multiple-environments
msc.type: authoredcontent
ms.openlocfilehash: 52fb2539ef388d129f88aa8aa87088e2d4a41ccf
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41838573"
---
<a name="customizing-database-deployments-for-multiple-environments"></a>Personalización de las implementaciones de base de datos para varios entornos
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo personalizar las propiedades de una base de datos en entornos de destino específico como parte del proceso de implementación.
> 
> > [!NOTE]
> > El tema se supone que va a implementar un proyecto de base de datos de Visual Studio 2010 con MSBuild.exe y VSDBCMD.exe. Para obtener más información sobre por qué podría elegir este enfoque, consulte [implementación Web en la empresa](../web-deployment-in-the-enterprise/web-deployment-in-the-enterprise.md) y [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md).
> 
> 
> Al implementar un proyecto de base de datos en varios destinos, a menudo desea personalizar las propiedades de implementación de la base de datos para cada entorno de destino. Por ejemplo, en entornos de prueba normalmente vuelva a crear la base de datos en cada implementación, mientras que en los entornos de ensayo o producción sería mucho más probable que realice actualizaciones incrementales para conservar los datos.
> 
> En un proyecto de base de datos de Visual Studio 2010, la configuración de implementación está contenida dentro de un archivo de configuración (.sqldeployment) de la implementación. En este tema le mostrará cómo crear archivos de configuración de implementación específicos del entorno y especificar lo que desee utilizar como un parámetro VSDBCMD.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general de tarea

En este tema se supone que:

- Usar el enfoque de archivo de proyecto de división para la implementación de la solución, como se describe en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Llamar a VSDBCMD desde el archivo de proyecto para implementar el proyecto de base de datos, como se describe en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para crear un sistema de implementación que admite diferentes de las propiedades de implementación de la base de datos entre entornos de destino, deberá:

- Cree un archivo de configuración (.sqldeployment) de implementación para cada entorno de destino.
- Crear un comando VSDBCMD que especifica el archivo de configuración de implementación como un conmutador de línea de comandos.
- Parametrizar el comando VSDBCMD en un archivo de proyecto de Microsoft Build Engine (MSBuild), de modo que las opciones de VSDBCMD son apropiadas para el entorno de destino.

En este tema le mostrará cómo realizar cada uno de estos procedimientos.

## <a name="creating-environment-specific-deployment-configuration-files"></a>Crear archivos de configuración de implementación específicos del entorno

De forma predeterminada, un proyecto de base de datos contiene un archivo de configuración de implementación único denominado *Database.sqldeployment*. Si abre este archivo en Visual Studio 2010, puede ver las opciones de implementación diferentes que están disponibles para usted:

- **Intercalación de comparación de implementación**. Esto le permite elegir si desea usar la intercalación de base de datos del proyecto (el *origen* intercalación) o la intercalación de base de datos de su servidor de destino (el *destino* intercalación). En la mayoría de los casos, desea utilizar la intercalación de origen cuando se implementa para un desarrollo o entorno de prueba. Cuando se implementa en un entorno de ensayo o producción, normalmente deseará dejar la intercalación de destino sin cambios para evitar cualquier problema de interoperabilidad.
- **Implementar propiedades de base de datos**. Esto le permite elegir si desea aplicar las propiedades de la base de datos, como se define en el *Database.sqlsettings* archivo. Al implementar una base de datos por primera vez, debe implementar las propiedades de la base de datos. Si va a actualizar una base de datos, las propiedades ya deben estar en su lugar, y no es necesario implementar de nuevo.
- **Siempre volver a crear la base de datos**. Este permite elegir si se debe volver a crear la base de datos de destino cada vez que implemente o realiza los cambios incrementales en la base de datos de destino al día con el esquema. Si vuelve a crear la base de datos, se perderá ningún dato en la base de datos existente. Por lo tanto, normalmente debería establecerlo en **false** para implementaciones en entornos de ensayo o producción.
- **Bloquear implementación incremental si puede producirse pérdida de datos**. Esto le permite elegir si la implementación debe detenerse si un cambio en el esquema de base de datos provocará la pérdida de datos. Normalmente se establece en **true** para una implementación en un entorno de producción, para ofrecerle la oportunidad de intervenir y proteger los datos importantes. Si ha establecido **volver a crear base de datos siempre** a **false**, esta configuración no tiene ningún efecto.
- **Ejecutar la implementación en modo de usuario único**. Esto no es normalmente un problema en entornos de desarrollo o prueba. Sin embargo, normalmente debería establecerlo en **true** para implementaciones en entornos de ensayo o producción. Esto impide que los usuarios realicen cambios en la base de datos mientras la implementación está en curso.
- **Copia de seguridad de base de datos antes de la implementación**. Normalmente se establece en **true** cuando se implementa en un entorno de producción, como precaución ante la pérdida de datos. También puede establecerlo en **true** al implementar en un entorno de ensayo, si la base de datos de ensayo contiene una gran cantidad de datos.
- **Generar instrucciones DROP para objetos que están en la base de datos de destino pero que no están en el proyecto de base de datos**. En la mayoría de los casos, esto es una parte integral y esencial de realizar los cambios incrementales en una base de datos. Si ha establecido **volver a crear base de datos siempre** a **false**, esta configuración no tiene ningún efecto.
- **No usar instrucciones ALTER ASSEMBLY para actualizar tipos CLR**. Esta configuración determina cómo SQL Server debe actualizar los tipos common language runtime (CLR) para las versiones más recientes de ensamblado. Esto se debe establecer en **false** en la mayoría de los escenarios.

Esta tabla muestra los valores típicos de implementación para distintos entornos de destino. Sin embargo, la configuración puede ser diferente según sus necesidades exactas.

|  | Desarrollo y pruebas | Integración y el almacenamiento provisional | Producción |
| --- | --- | --- | --- |
| **Intercalación de comparación de implementación** | Origen | Destino | Destino |
| **Implementar propiedades de base de datos** | True | Sólo la primera vez | Sólo la primera vez |
| **Siempre volver a crear la base de datos** | True | False | False |
| **Bloquear implementación incremental si puede producirse pérdida de datos** | False | Quizás | True |
| **Ejecutar el script de implementación en modo de usuario único** | False | True | True |
| **Realizar copias de seguridad de base de datos antes de la implementación** | False | Quizás | True |
| **Generar instrucciones DROP para objetos que están en la base de datos de destino pero que no están en el proyecto de base de datos** | False | True | True |
| **No usar instrucciones ALTER ASSEMBLY para actualizar tipos CLR** | False | False | False |
  

> [!NOTE]
> Para obtener más información sobre las propiedades de implementación de base de datos y consideraciones del entorno, consulte [An Overview of Database Project Settings](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx), [Cómo: configurar las propiedades para obtener detalles de implementación](https://msdn.microsoft.com/library/dd172125.aspx), [ Compilar e implementar la base de datos en un entorno de desarrollo aislado](https://msdn.microsoft.com/library/dd193409.aspx), y [compilar e implementar bases de datos en un entorno de producción o ensayo](https://msdn.microsoft.com/library/dd193413.aspx).


Para admitir la implementación de un proyecto de base de datos a varios destinos, debe crear un archivo de configuración de implementación para cada entorno de destino.

**Para crear un archivo de configuración específicos del entorno**

1. En Visual Studio 2010, en el **el Explorador de soluciones** , haga clic en el proyecto de base de datos y, a continuación, haga clic en **propiedades**.
2. En la página de propiedades del proyecto de base de datos, en el **implementar** ficha la **archivo de configuración de implementación** la fila, haga clic en **New**.

    ![](customizing-database-deployments-for-multiple-environments/_static/image1.png)
3. En el **nuevo archivo de configuración de implementación** diálogo cuadro, asigne al archivo un nombre descriptivo (por ejemplo, **TestEnvironment.sqldeployment**) y, a continuación, haga clic en **guardar**.
4. En el *[nombreDeArchivo]***.sqldeployment** página, establezca las propiedades de implementación para que coincida con los requisitos de su entorno de destino y, a continuación, guarde el archivo.

    ![](customizing-database-deployments-for-multiple-environments/_static/image2.png)
5. Tenga en cuenta que el nuevo archivo se agrega a la carpeta Propiedades del proyecto de base de datos.

    ![](customizing-database-deployments-for-multiple-environments/_static/image3.png)

## <a name="specifying-the-deployment-configuration-file-in-vsdbcmd"></a>Especificar el archivo de configuración de implementación en VSDBCMD

Al usar configuraciones de soluciones (por ejemplo, Debug y Release) dentro de Visual Studio 2010, puede asociar un archivo de configuración de implementación con cada configuración. Cuando se compila una configuración determinada, el proceso de compilación genera un archivo de manifiesto de implementación específicos de la configuración que apunta al archivo de configuración de implementación específicos de la configuración. Sin embargo, uno de los principales objetivos del enfoque a la implementación que se describe en estos tutoriales es ofrecer a las personas la capacidad para controlar el proceso de implementación sin usar Visual Studio 2010 y configuraciones de soluciones. En este enfoque, la configuración de soluciones es el mismo independientemente del entorno de implementación de destino. Para adaptar su implementación de la base de datos en un entorno de destino específico, puede usar las opciones de línea de comandos VSDBCMD para especificar el archivo de configuración de implementación.

Para especificar un archivo de configuración de implementación en su VSDBCMD, use el **DeploymentConfigurationFile/p:** cambie y proporcionar la ruta de acceso completa al archivo. Esto reemplazará el archivo de configuración de implementación que identifica el manifiesto de implementación. Por ejemplo, podría utilizar este comando VSDBCMD para implementar la **ContactManager** base de datos en un entorno de prueba:


[!code-console[Main](customizing-database-deployments-for-multiple-environments/samples/sample1.cmd)]


> [!NOTE]
> Tenga en cuenta que el proceso de compilación puede cambiar el nombre de su archivo .sqldeployment cuando copia el archivo en el directorio de salida.


Si usa las variables de comando SQL en los scripts anteriores o posteriores a la implementación de SQL, puede usar un enfoque similar para asociar un archivo .sqlcmdvars específicos del entorno con la implementación. En este caso, usa el **SqlCommandVariablesFile/p:** conmutador para identificar el archivo .sqlcmdvars.

## <a name="running-the-vsdbcmd-command-from-an-msbuild-project-file"></a>Ejecute el comando VSDBCMD desde un archivo de proyecto de MSBuild

Puede invocar un comando VSDBCMD desde un archivo de proyecto de MSBuild con un **Exec** tarea dentro de un destino de MSBuild. En su forma más sencilla, sería similar al siguiente:


[!code-xml[Main](customizing-database-deployments-for-multiple-environments/samples/sample2.xml)]


- En la práctica, para facilitar los archivos de proyecto leer y volver a usar, deseará crear propiedades para almacenar los distintos parámetros de línea de comandos. Esto facilita a los usuarios para proporcionar valores de propiedad en un archivo de proyecto específicos del entorno o para invalidar los valores predeterminados de la línea de comandos de MSBuild. Si utiliza el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), debe dividir sus propiedades entre los dos archivos y las instrucciones de compilación según corresponda:
- Configuración específica del entorno, como el nombre de archivo de configuración de implementación, la cadena de conexión de base de datos y el nombre de base de datos de destino, debe ir en el archivo de proyecto específicos del entorno.
- El destino de MSBuild que se ejecuta el comando VSDBCMD, junto con las propiedades, como la ubicación del ejecutable VSDBCMD, universales debe ir en el archivo de proyecto universal.

También debe asegurarse de compilar el proyecto de base de datos antes de invocar VSDBCMD para que el archivo .deploymanifest está creado y listo para su uso. Puede ver un ejemplo completo de este enfoque en el tema [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), que le guiará a través de los archivos del proyecto en el [solución de ejemplo de Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md).

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo puede personalizar las propiedades de la base de datos para distintos entornos de destino al implementar proyectos de base de datos utilizando MSBuild y VSDBCMD. Este enfoque es útil cuando se necesita implementar proyectos de base de datos como parte de las soluciones de escala empresarial de mayor tamaño. A menudo, estas soluciones se implementan en varios destinos, como entornos de desarrollo o pruebas en espacio aislado, ensayo o integración de plataformas y producción o entornos en vivo. Normalmente, cada uno de estos entornos de destino requiere un único conjunto de propiedades de implementación de la base de datos.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre la implementación de proyectos de base de datos mediante VSDBCMD.exe, consulte [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, consulte [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Estos artículos en MSDN proporcionan una orientación más general sobre la implementación de la base de datos:

- [Información general de configuración del proyecto de base de datos](https://msdn.microsoft.com/library/aa833291(v=VS.100).aspx)
- [Cómo: configurar las propiedades de los detalles de implementación](https://msdn.microsoft.com/library/dd172125.aspx)
- [Compilar e implementar bases de datos en un entorno de desarrollo aislado](https://msdn.microsoft.com/library/dd193409.aspx)
- [Compilar e implementar bases de datos en un entorno de producción o ensayo](https://msdn.microsoft.com/library/dd193413.aspx)

> [!div class="step-by-step"]
> [Anterior](performing-a-what-if-deployment.md)
> [Siguiente](deploying-database-role-memberships-to-test-environments.md)
