---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
title: Implementación de las pertenencias a roles de base de datos en entornos de prueba | Microsoft Docs
author: jrjlee
description: Este tema describe cómo agregar cuentas de usuario a roles de base de datos como parte de una implementación de la solución en un entorno de prueba. Al implementar una solución que contiene...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: 9b2af539-7ad9-47aa-b66e-873bd9906e79
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments
msc.type: authoredcontent
ms.openlocfilehash: 07442b7a016ce2a32b1c9e7f44010517e40d7189
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828869"
---
<a name="deploying-database-role-memberships-to-test-environments"></a>Implementación de las pertenencias a roles de base de datos en entornos de prueba
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo agregar cuentas de usuario a roles de base de datos como parte de una implementación de la solución en un entorno de prueba.
> 
> Al implementar una solución que contiene un proyecto de base de datos en un entorno de ensayo o producción, normalmente no desea que el programador para automatizar la adición de cuentas de usuario a roles de base de datos. En la mayoría de los casos, el desarrollador no sabrá qué cuentas de usuario deben agregarse a los roles de base de datos y estos requisitos podrían cambiar en cualquier momento. Sin embargo, al implementar una solución que contiene un proyecto de base de datos a un desarrollo o entorno de prueba, la situación normalmente es distinta en su lugar:
> 
> - El programador suele volver a implementa la solución de forma periódica, a menudo varias veces al día.
> - La base de datos normalmente se vuelve a crear en cada implementación, lo que significa que los usuarios de base de datos deben crearse y agregar a roles después de cada implementación.
> - Normalmente, el desarrollador tiene control total sobre el entorno de desarrollo o pruebas de destino.
> 
> En este escenario, a menudo resulta útil crear usuarios de base de datos automáticamente y asignar a las pertenencias a roles de base de datos como parte del proceso de implementación.
> 
> El factor clave es que esta operación debe ser condicional basándose en el entorno de destino. Si va a implementar en un almacenamiento provisional o en un entorno de producción, desea omitir la operación. Si va a implementar en un desarrollador o entorno de prueba, va a implementar las pertenencias a roles sin ninguna intervención adicional. Este tema describe un enfoque que puede usar para abordar este desafío.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general de tarea

En este tema se supone que:

- Usar el enfoque de archivo de proyecto de división para la implementación de la solución, como se describe en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md).
- Llamar a VSDBCMD desde el archivo de proyecto para implementar el proyecto de base de datos, como se describe en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

Para crear usuarios de base de datos y asignar a las pertenencias a roles al implementar un proyecto de base de datos en un entorno de prueba, necesitará:

- Crear un script de Transact lenguaje de consulta estructurado (Transact-SQL) que realiza los cambios de la base de datos necesarios.
- Crear un destino de Microsoft Build Engine (MSBuild) que usa la utilidad sqlcmd.exe para ejecutar el script de SQL.
- Configurar los archivos de proyecto para invocar el destino al implementar la solución en un entorno de prueba.

En este tema le mostrará cómo realizar cada uno de estos procedimientos.

## <a name="scripting-the-database-role-memberships"></a>Las pertenencias a roles de base de datos de secuencias de comandos

Puede crear un script Transact-SQL en una gran cantidad de maneras diferentes, y en cualquier ubicación elija. El enfoque más sencillo es crear la secuencia de comandos dentro de la solución en Visual Studio 2010.

**Para crear un script SQL**

1. En el **el Explorador de soluciones** ventana, expanda el nodo del proyecto de base de datos.
2. Haga clic en el **Scripts** carpeta, seleccione **agregar**y, a continuación, haga clic en **nueva carpeta**.
3. Tipo **prueba** como el nombre de la carpeta y, a continuación, presione ENTRAR.
4. Haga clic en el **prueba** carpeta, seleccione **agregar**y, a continuación, haga clic en **Script**.
5. En el **Agregar nuevo elemento** diálogo cuadro, dar un nombre descriptivo a la secuencia de comandos (por ejemplo, **AddRoleMemberships.sql**) y, a continuación, haga clic en **agregar**.

    ![](deploying-database-role-memberships-to-test-environments/_static/image1.png)
6. En el *AddRoleMemberships.sql* , agregue las instrucciones Transact-SQL que:

    1. Cree un usuario de base de datos para el inicio de sesión de SQL Server que tenga acceso a la base de datos.
    2. Agregue el usuario de base de datos a los roles de base de datos necesarios.
7. El archivo debe ser similar a esto:

    [!code-sql[Main](deploying-database-role-memberships-to-test-environments/samples/sample1.sql)]
8. Guarde el archivo.

## <a name="executing-the-script-on-the-target-database"></a>Ejecutar el Script en la base de datos de destino

Idealmente, ejecutaría los scripts de Transact-SQL necesarios como parte de un script posterior a la implementación al implementar el proyecto de base de datos. Sin embargo, los scripts posteriores a la implementación no permiten ejecutar lógica condicional en función de las configuraciones de soluciones o las propiedades de compilación. La alternativa consiste en ejecutar las secuencias de comandos SQL directamente desde el archivo de proyecto de MSBuild, mediante la creación de un **destino** elemento que se ejecuta un comando de sqlcmd.exe. Puede usar este comando para ejecutar el script en la base de datos de destino:


[!code-console[Main](deploying-database-role-memberships-to-test-environments/samples/sample2.cmd)]


> [!NOTE]
> Para obtener más información sobre las opciones de línea de comandos de sqlcmd, consulte [utilidad sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).


Antes de insertar este comando en un destino de MSBuild, debe considerar en qué condiciones se desea ejecutar el script:

- La base de datos de destino debe existir antes de cambiar su pertenencia a roles. Por lo tanto, deberá ejecutar este script *después* la implementación de la base de datos.
- Debe incluir una condición para que solo se ejecuta el script para entornos de prueba.
- Si está ejecutando una implementación "what if"&#x2014;en otras palabras, si está generando scripts de implementación pero realmente no ejecutarlos&#x2014;no debe ejecutar el script SQL.

Si está usando el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), como se muestra en la solución de ejemplo de Contact Manager, puede dividir las instrucciones de compilación del script SQL similar al siguiente:

- Las necesarias propiedades específicas del entorno, junto con la propiedad que determina si se debe implementar permisos, deben ir en el archivo de proyecto específicos del entorno (por ejemplo, *Env-Dev.proj*).
- El destino de MSBuild, junto con las propiedades que no se cambiará entre los entornos de destino, debe ir en el archivo de proyecto universal (por ejemplo, *Publish.proj*).

En el archivo de proyecto específicos del entorno, deberá definir el nombre del servidor de base de datos, el nombre de la base de datos de destino y una propiedad booleana que permite al usuario especificar si va a implementar las pertenencias a roles.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample3.xml)]


En el archivo de proyecto universal, deberá proporcionar la ubicación que el ejecutable de sqlcmd y la ubicación de la secuencia de comandos SQL que desea ejecutar. Estas propiedades seguirá siendo el mismo independientemente del entorno de destino. También deberá crear un destino de MSBuild para ejecutar el comando sqlcmd.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample4.xml)]


Tenga en cuenta que agregar la ubicación que el ejecutable de sqlcmd como una propiedad estática, como esto puede resultar útil a otros destinos. En cambio, defina la ubicación de la secuencia de comandos SQL y la sintaxis del comando sqlcmd como propiedades dinámicas dentro del destino, ya que no se requiere antes de ejecutar el destino. En este caso, el **DeployTestDBPermissions** destino solo se ejecutará si se cumplen estas condiciones:

- El **DeployTestDBRoleMemberships** propiedad está establecida en **true**.
- El usuario no ha especificado un **WhatIf = true** marca.

Por último, no olvide invocar el destino. En el *Publish.proj* archivo, puede hacerlo agregando el destino a la lista de dependencias para el valor predeterminado **FullPublish** destino. Debe asegurarse de que el **DeployTestDBPermissions** destino no se ejecuta hasta el **PublishDbPackages** se ha ejecutado el destino.


[!code-xml[Main](deploying-database-role-memberships-to-test-environments/samples/sample5.xml)]


## <a name="conclusion"></a>Conclusión

En este tema se describe una manera en que se pueden agregar usuarios de base de datos y las pertenencias a roles como una acción posterior a la implementación al implementar un proyecto de base de datos. Esto es útil normalmente cuando periódicamente volver a crear una base de datos en un entorno de prueba, pero por lo general debe evitarse al implementar las bases de datos en entornos de ensayo o producción. Por lo tanto, debe asegurarse de que usar la lógica condicional necesaria para que los usuarios de base de datos y las pertenencias a roles se crean solo cuando resulta adecuado hacerlo.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre el uso de VSDBCMD para implementar proyectos de base de datos, vea [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obtener instrucciones sobre cómo personalizar las implementaciones de base de datos para diferentes entornos de destino, vea [personalizar implementaciones de base de datos para varios entornos](customizing-database-deployments-for-multiple-environments.md). Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, consulte [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md). Para obtener más información sobre las opciones de línea de comandos de sqlcmd, consulte [utilidad sqlcmd](https://msdn.microsoft.com/library/ms162773.aspx).

> [!div class="step-by-step"]
> [Anterior](customizing-database-deployments-for-multiple-environments.md)
> [Siguiente](deploying-membership-databases-to-enterprise-environments.md)
