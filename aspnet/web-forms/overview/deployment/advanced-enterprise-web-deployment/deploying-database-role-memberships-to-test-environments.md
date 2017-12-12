---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: "Implementación de las pertenencias a roles de base de datos en entornos de prueba | Documentos de Microsoft"
author: jrjlee
description: "En este tema se describe cómo agregar cuentas de usuario a los roles de base de datos como parte de una implementación de la solución en un entorno de prueba. Al implementar una solución que contenga..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: ac780c6cd522f9216cafe3b5f23772ef6ebf5d11
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Implementación de las pertenencias a roles de base de datos en entornos de prueba
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo agregar cuentas de usuario a los roles de base de datos como parte de una implementación de la solución en un entorno de prueba.
> 
> Al implementar una solución que contenga un proyecto de base de datos en un entorno de ensayo o de producción, normalmente no desea que el programador para automatizar la adición de cuentas de usuario a los roles de base de datos. En la mayoría de los casos, el desarrollador no sabrá qué cuentas de usuario deben agregarse a los roles de base de datos y estos requisitos podrían cambiar en cualquier momento. Sin embargo, al implementar una solución que contenga un proyecto de base de datos para un desarrollo o entorno de prueba, la situación suele ser muy diferente:
> 
> - El programador suele volver a implementa la solución de forma regular, a menudo varias veces al día.
> - La base de datos normalmente se vuelve a crear en cada implementación, lo que significa que los usuarios de base de datos deben pueden crearse y agregarse a los roles después de cada implementación.
> - Normalmente, el desarrollador tiene control total sobre el entorno de desarrollo o pruebas de destino.
> 
> En este escenario, a menudo resulta útil crear usuarios de base de datos automáticamente y asignar a miembros de la función de base de datos como parte del proceso de implementación.
> 
> El factor clave es que esta operación debe ser condicional según el entorno de destino. Si va a implementar un almacenamiento provisional o en un entorno de producción, que desea omitir la operación. Si va a implementar a un desarrollador o entorno de prueba, va a implementar las pertenencias a roles sin ninguna intervención adicional. Este tema describe un método que puede utilizar para abordar este reto.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales que utiliza una solución de ejemplo & #x 2014; la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar una aplicación web con un nivel de complejidad, incluso una aplicación de ASP.NET MVC 3, Windows realista Servicio de Communication Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos proyectos, archivos de & #x 2014; uno que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

En este tema se supone:

- Usar el enfoque de archivo de proyecto de división para la implementación de la solución, como se describe en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Llamar VSDBCMD desde el archivo de proyecto para implementar el proyecto de base de datos, como se describe en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para crear usuarios de base de datos y asignar a las pertenencias a roles al implementar un proyecto de base de datos en un entorno de prueba, necesitará:

- Crear un script de Transact lenguaje de consulta estructurado (Transact-SQL) que realiza los cambios necesarios de la base de datos.
- Crear un destino de Microsoft Build Engine (MSBuild) que usa la utilidad sqlcmd.exe para ejecutar el script SQL.
- Configurar los archivos de proyecto para invocar el destino cuando va a implementar la solución en un entorno de prueba.

En este tema le mostrará cómo realizar cada uno de estos procedimientos.

## <a name="scripting-the-database-role-memberships"></a>Las pertenencias a roles de base de datos de secuencias de comandos

Puede crear un script de Transact-SQL en un lote de maneras diferentes, y en cualquier ubicación elija. El enfoque más sencillo consiste en crear la secuencia de comandos dentro de la solución en Visual Studio 2010.

**Para crear un script SQL**

1. En el **el Explorador de soluciones** ventana, expanda el nodo del proyecto de base de datos.
2. Haga clic en el **Scripts** carpeta, seleccione **agregar**y, a continuación, haga clic en **nueva carpeta**.
3. Tipo de **prueba** como nombre de la carpeta y, a continuación, presione ENTRAR.
4. Haga clic en el **prueba** carpeta, seleccione **agregar**y, a continuación, haga clic en **Script**.
5. En el **Agregar nuevo elemento** diálogo cuadro, dar un nombre descriptivo a la secuencia de comandos (por ejemplo, **AddRoleMemberships.sql**) y, a continuación, haga clic en **agregar**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. En el *AddRoleMemberships.sql* , agregue las instrucciones de Transact-SQL que:

    1. Cree un usuario de base de datos para el inicio de sesión de SQL Server que tenga acceso a la base de datos.
    2. Agregue el usuario de base de datos a ningún rol de base de datos requeridas.
7. El archivo debe ser similar a este:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Guarde el archivo.

## <a name="executing-the-script-on-the-target-database"></a>Ejecutar el Script en la base de datos de destino

Idealmente, ejecutaría cualquier script de Transact-SQL necesario como parte de un script posterior a la implementación al implementar el proyecto de base de datos. Sin embargo, las secuencias de comandos posteriores a la implementación no permiten ejecutar lógica de manera condicional basándose en las configuraciones de soluciones o propiedades de compilación. La alternativa consiste en ejecutar los scripts de SQL directamente desde el archivo de proyecto de MSBuild, mediante la creación de un **destino** elemento que ejecuta un comando de sqlcmd.exe. Puede utilizar este comando para ejecutar el script en la base de datos de destino:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Para obtener más información sobre las opciones de línea de comandos de sqlcmd, vea [utilidad sqlcmd](https://msdn.microsoft.com/en-us/library/ms162773.aspx).


Antes de insertar este comando en un destino de MSBuild, debe tener en cuenta en las condiciones que desea que el script se ejecute:

- La base de datos de destino debe existir antes de cambiar su pertenencia a roles. Por lo tanto, debe ejecutar este script *después* la implementación de la base de datos.
- Debe incluir una condición para que solo se ejecuta la secuencia de comandos para entornos de prueba.
- Si está ejecutando una implementación de "¿Qué ocurre si" & #x 2014; en otras palabras, si está generando secuencias de comandos de implementación pero no ejecutándose en ellos & #x 2014; no debe ejecutar el script SQL.

Si está usando el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), como se muestra en la solución de ejemplo póngase en contacto con el administrador, puede dividir las instrucciones de compilación para la secuencia de comandos SQL similar al siguiente:

- Las necesarias propiedades específicas del entorno, junto con la propiedad que determina si se debe implementar permisos, deberían ir en el archivo de proyecto específicas del entorno (por ejemplo, *Dev.proj Env*).
- El destino de MSBuild, junto con todas las propiedades que no se cambiará entre entornos de destino, debe ir en el archivo de proyecto universal (por ejemplo, *Publish.proj*).

En el archivo de proyecto específicos del entorno, debe definir el nombre del servidor de base de datos, el nombre de base de datos de destino y una propiedad booleana que permite al usuario especificar si va a implementar las pertenencias a roles.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


En el archivo de proyecto universal, debe proporcionar la ubicación del ejecutable de SQLCMD y la ubicación de la secuencia de comandos SQL que va a ejecutar. Estas propiedades seguirá siendo el mismo independientemente del entorno de destino. También debe crear un destino de MSBuild para ejecutar el comando sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Tenga en cuenta que agregar la ubicación del ejecutable de SQLCMD como una propiedad estática, como esto puede resultar útil a otros destinos. En cambio, defina la ubicación de la secuencia de comandos SQL y la sintaxis del comando sqlcmd como propiedades dinámicas dentro del destino, ya que no son necesarios antes de que se ejecuta el destino. En este caso, el **DeployTestDBPermissions** destino solo se ejecutará si se cumplen estas condiciones:

- El **DeployTestDBRoleMemberships** propiedad está establecida en **true**.
- El usuario no ha especificado un **WhatIf = true** marca.

Por último, no olvide invocar el destino. En el *Publish.proj* archivo, puede hacerlo agregando el destino a la lista de dependencias para el valor predeterminado **FullPublish** destino. Necesita asegurarse de que el **DeployTestDBPermissions** destino no se ejecuta hasta que la **PublishDbPackages** se ha ejecutado el destino.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Conclusión

En este tema se describe una manera en que se pueden agregar usuarios de base de datos y las pertenencias a roles como una acción posterior a la implementación al implementar un proyecto de base de datos. Esto es suele ser útil cuando periódicamente volver a crear una base de datos en un entorno de prueba, pero normalmente debería evitarse al implementar las bases de datos en entornos de ensayo o de producción. Por lo tanto, debe asegurarse de que usa la lógica condicional necesaria para que los usuarios de base de datos y las pertenencias a roles solo se crean cuándo es adecuado hacerlo.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre cómo utilizar VSDBCMD para implementar proyectos de base de datos, vea [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obtener instrucciones acerca de cómo personalizar las implementaciones de la base de datos para diferentes entornos de destino, vea [personalizar implementaciones de base de datos para varios entornos](customizing-database-deployments-for-multiple-environments.md). Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, consulte [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obtener más información sobre las opciones de línea de comandos de sqlcmd, vea [utilidad sqlcmd](https://msdn.microsoft.com/en-us/library/ms162773.aspx).

>[!div class="step-by-step"]
[Anterior](customizing-database-deployments-for-multiple-environments.md)
[Siguiente](deploying-membership-databases-to-enterprise-environments.md)
