---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Implementación de bases de datos de pertenencia en entornos empresariales | Microsoft Docs
author: jrjlee
description: En este tema se explica las consideraciones clave y los desafíos que se debe superar al aprovisionar bases de datos de servicios de aplicaciones de ASP.NET (más habitual...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: 432951b54fc7cc6b0384dfb4dbd255b16a546e76
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370340"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Implementación de bases de datos de pertenencia en entornos empresariales
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se explica las consideraciones clave y los desafíos que se debe superar al aprovisionar la aplicación ASP.NET de servicios de bases de datos (más comúnmente como bases de datos de pertenencia) en entornos de prueba, ensayo o producción. También se describen los enfoques que puede usar para superar estos retos.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>¿Cuáles son los problemas al implementar una base de datos de pertenencia?

En la mayoría de los casos, al diseñar una estrategia de implementación para una base de datos, lo primero que debe tener en cuenta es qué datos desea implementar. En un entorno de desarrollo o prueba, puede implementar datos de cuentas de usuario para facilitar la prueba de manera rápida y sencilla. En un entorno de ensayo o producción, es muy poco probable que desearía implementar datos de la cuenta de usuario.

Lamentablemente, las bases de datos de pertenencia ASP.NET presentan algunos desafíos específicos que tomar esta decisión mucho más complejo:

- Implementación de una solo esquema dejará la base de datos de pertenencia en un estado no operativo. Esto es porque la base de datos de pertenencia incluye algunos datos de configuración (en el **aspnet\_SchemaVersions** tabla) que requiere la base de datos para poder funcionar. Por lo tanto, si lleva a cabo una implementación solo de esquema de la base de datos de pertenencia con el fin de excluir los datos de la cuenta de usuario, deberá ejecutar un script posterior a la implementación para agregar los datos de la configuración básica.
- Según cómo esté configurada la base de datos de pertenencia, el proveedor de pertenencia puede usar la clave del equipo para cifrar las contraseñas y almacenarlos en la base de datos. En este caso, los datos de la cuenta de usuario que se implementa con la base de datos quedará inservible en el servidor de destino. Por este motivo, la implementación de datos de la cuenta de usuario no es un escenario admitido.

## <a name="choosing-a-membership-database-strategy"></a>Elegir una estrategia de base de datos de pertenencia

Siga estas directrices para elegir cómo aprovisionar una base de datos de pertenencia en un entorno de servidor empresarial:

- Siempre que sea posible, implemente las bases de datos de pertenencia. En el servidor de base de datos de destino en su lugar, cree manualmente la base de datos de pertenencia. Si no ha personalizado el esquema de base de datos de pertenencia, puede simplemente crear una nueva in situ en el destino mediante el [herramienta de registro de SQL Server de ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Si no tiene ninguna opción, pero para implementar una base de datos de pertenencia&#x2014;por ejemplo, si ha realizado modificaciones extensas el esquema de base de datos&#x2014;debe realizar una implementación solo de esquema de la base de datos de pertenencia, para excluir los datos de la cuenta de usuario y, a continuación, ejecutar un script posterior a la implementación para agregar los datos de configuración necesarias. Puede encontrar amplia información sobre estos enfoques en [Cómo: implementar el ASP.NET pertenencia base de datos sin incluidas cuentas de usuario](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Es importante recordar que *el esquema de la base de datos de pertenencia es probable que sea bastante estático*. Incluso si ha personalizado la base de datos de pertenencia, es improbable que necesitará actualizar el esquema de forma periódica&#x2014;no va a cambiar con la misma frecuencia que el código en una aplicación web o un proyecto de base de datos. Por lo tanto, no es necesario incluir la base de datos de pertenencia en los procesos de implementación automatizada o paso a paso.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Uso de VSDBCMD para actualizar un esquema de base de datos de pertenencia

Si modifica la estructura de la base de datos de pertenencia tras la primera implementación, es posible que no desea usar la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) para volver a implementar la base de datos. La funcionalidad de implementación de la base de datos de Web Deploy no incluye la capacidad de realizar actualizaciones diferenciales de una base de datos de destino&#x2014;en su lugar, la Web Deploy debe quitar y volver a crear la base de datos. Esto significa que perder los datos de cuenta de usuario existentes, que no es deseable normalmente en entornos de ensayo o producción.

La alternativa es usar la utilidad VSDBCMD para actualizar el esquema de la base de datos de destino. VSDBCMD incluye dos funciones importantes. En primer lugar, puede importar el esquema de base de datos existente en un archivo .dbschema. En segundo lugar, puede implementar un archivo .dbschema a una base de datos existente como una actualización diferencial, lo que significa que solo realiza los cambios necesarios para la base de datos de destino al día y no perderá ningún dato.

Puede usar estos pasos generales para actualizar un esquema de base de datos de pertenencia:

1. Utilice la herramienta VSDBCMD **importación** acción para generar un archivo .dbschema para la base de datos de pertenencia de origen. Este procedimiento se describe en [Cómo: importar un esquema desde un símbolo del sistema](https://msdn.microsoft.com/library/dd172135.aspx).
2. Utilice la herramienta VSDBCMD **implementar** acción para implementar el archivo .dbschema en la base de datos de pertenencia de destino. Este procedimiento se describe en [referencia de línea de comandos de VSDBCMD. EXE (implementación e importación del esquema)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusión

En este tema se describe algunos de los desafíos que pueden enfrentar cuando necesite aprovisionar bases de datos de pertenencia ASP.NET en diversos entornos de destino. En concreto, se explica por qué las implementaciones de solo esquema dejará la base de datos de pertenencia en un estado no operativo y por qué no se admite implementar datos de la cuenta de usuario. El tema también presenta instrucciones sobre cómo aprovisionar, implementar y actualizar bases de datos de pertenencia en escenarios diferentes.

## <a name="further-reading"></a>Información adicional

Para obtener más instrucciones y ejemplos de cómo usar VSDBCMD, consulte [referencia de línea de comandos de VSDBCMD. EXE (implementación e importación del esquema)](https://msdn.microsoft.com/library/dd193283.aspx) y [Cómo: importar un esquema desde un símbolo del sistema](https://msdn.microsoft.com/library/dd172135.aspx). Para obtener más información sobre el uso de aspnet\_regsql.exe crear bases de datos de pertenencia, vea [herramienta de registro de SQL Server de ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Para obtener instrucciones más generales sobre la implementación de bases de datos de pertenencia, vea [Cómo: implementar el ASP.NET pertenencia base de datos sin incluidas cuentas de usuario](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-role-memberships-to-test-environments.md)
> [Siguiente](excluding-files-and-folders-from-deployment.md)
