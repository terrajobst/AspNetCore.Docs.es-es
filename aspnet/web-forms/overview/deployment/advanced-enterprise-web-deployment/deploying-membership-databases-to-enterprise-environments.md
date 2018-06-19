---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
title: Implementación de las bases de datos de pertenencia en entornos empresariales | Documentos de Microsoft
author: jrjlee
description: Este tema explican las consideraciones claves y los desafíos que deberá superar al aprovisionar bases de datos de servicios de aplicaciones de ASP.NET (más común...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 3cf765df-d311-4f68-a295-c9685ceea830
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments
msc.type: authoredcontent
ms.openlocfilehash: b783fcf57759f2a431480eec6902105f6d683408
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892483"
---
<a name="deploying-membership-databases-to-enterprise-environments"></a>Implementación de las bases de datos de pertenencia en entornos empresariales
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema explica las consideraciones claves y los desafíos que deberá superar al aprovisionar la aplicación ASP.NET de servicios de bases de datos (más comúnmente como bases de datos de pertenencia) en entornos de prueba, ensayo o producción. También se describen los enfoques que puede usar para superar estos retos.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="what-are-the-issues-when-you-deploy-a-membership-database"></a>¿Cuáles son los problemas al implementar una base de datos de suscripción?

En la mayoría de los casos, al diseñar una estrategia de implementación para una base de datos, lo primero que debe tener en cuenta es los datos que se va a implementar. En un entorno de desarrollo o de prueba, puede implementar datos de la cuenta de usuario para facilitar la prueba de forma rápida y fácil. En un entorno de ensayo o de producción, no es muy probable que se va a implementar datos de la cuenta de usuario.

Desafortunadamente, las bases de datos de pertenencia ASP.NET presentan algunos desafíos específicos que tomar esta decisión mucho más compleja:

- Una implementación de solo esquema dejará la base de datos de pertenencia en un estado no operativo. Esto es porque la base de datos de pertenencia incluye algunos datos de configuración (en el **aspnet\_SchemaVersions** tabla) que requiere la base de datos para poder funcionar. Por lo tanto, si lleva a cabo una implementación solo de esquema de la base de datos de pertenencia con el fin de excluir los datos de la cuenta de usuario, debe ejecutar un script posterior a la implementación para agregar los datos de la configuración básica.
- Según cómo esté configurada la base de datos de pertenencia, el proveedor de pertenencia puede utilizar la clave del equipo para cifrar contraseñas y almacenarlos en la base de datos. En este caso, los datos de la cuenta de usuario que se implementa con la base de datos quedará inservible en el servidor de destino. Por este motivo, la implementación de datos de la cuenta de usuario no es un escenario admitido.

## <a name="choosing-a-membership-database-strategy"></a>Elegir una estrategia de base de datos de pertenencia

Siga estas directrices para elegir cómo aprovisionar una base de datos de pertenencia en un entorno de servidor empresarial:

- Siempre que sea posible, implemente las bases de datos de pertenencia. En su lugar, cree la base de datos de pertenencia manualmente en el servidor de base de datos de destino. Si no ha personalizado el esquema de base de datos de pertenencia, puede simplemente crear uno nuevo in situ en el de destino mediante el [herramienta de registro de servidor de SQL de ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).
- Si no tendrá ninguna opción pero al implementar una base de datos de pertenencia&#x2014;por ejemplo, si ha tomado una amplia modificaciones en el esquema de base de datos&#x2014;debe realizar una implementación solo de esquema de la base de datos de pertenencia, para excluir los datos de la cuenta de usuario y, a continuación, ejecutar un script posterior a la implementación para agregar los datos de configuración necesarias. Puede encontrar una amplia orientación sobre estos enfoques en [Cómo: implementar las ASP.NET pertenencia base de datos sin incluidas cuentas de usuario](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

Es importante recordar que *el esquema de la base de datos de pertenencia es probable que sea relativamente estáticos*. Incluso si ha personalizado la base de datos de pertenencia, no es probable que, necesitará actualizar el esquema de forma regular&#x2014;no se va a cambiar con la misma frecuencia que el código en una aplicación web o un proyecto de base de datos. Por lo tanto, no es necesario incluir la base de datos de pertenencia en los procesos de implementación automatizada o paso a paso.

## <a name="using-vsdbcmd-to-update-a-membership-database-schema"></a>Usando VSDBCMD para actualizar un esquema de base de datos de pertenencia

Si modifica la estructura de la base de datos de pertenencia tras la primera implementación, no puede usar la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) para volver a implementar la base de datos. La funcionalidad de implementación de base de datos de Web Deploy no incluye la capacidad de realizar actualizaciones diferenciales en una base de datos de destino&#x2014;en su lugar, la implementación Web debe quitar y volver a crear la base de datos. Esto significa que perder los datos de cuenta de usuario existentes, que es normalmente no deseados en los entornos de ensayo o de producción.

La alternativa es usar la utilidad VSDBCMD para actualizar el esquema de la base de datos de destino. VSDBCMD incluye dos funciones importantes. En primer lugar, puede importar el esquema de una base de datos existente en un archivo .dbschema. En segundo lugar, puede implementar un archivo .dbschema en una base de datos como una actualización diferencial, lo que significa que sólo realiza los cambios necesarios para la base de datos de destino actualizado y no perderá ningún dato.

Puede usar estos pasos de alto nivel para actualizar un esquema de base de datos de pertenencia:

1. Use la herramienta VSDBCMD **importación** acción que se va a generar un archivo .dbschema para la base de datos de pertenencia de origen. Este procedimiento se describe en [Cómo: importar un esquema desde un símbolo del sistema](https://msdn.microsoft.com/library/dd172135.aspx).
2. Use la herramienta VSDBCMD **implementar** acción que se va a implementar el archivo .dbschema en la base de datos de suscripción de destino. Este procedimiento se describe en [referencia de línea de comandos de VSDBCMD. EXE (implementación e importación del esquema)](https://msdn.microsoft.com/library/dd193283.aspx).

## <a name="conclusion"></a>Conclusión

En este tema se describe algunos de los desafíos que pueden darse cuando necesite aprovisionar bases de datos de pertenencia ASP.NET en varios entornos de destino. En concreto, explica por qué las implementaciones que solo esquema dejará la base de datos de pertenencia en un estado no operativo y por qué no se admite implementar datos de la cuenta de usuario. El tema presentaba también instrucciones sobre cómo aprovisionar, implementar y actualizar las bases de datos de pertenencia en escenarios diferentes.

## <a name="further-reading"></a>Información adicional

Para obtener más instrucciones y ejemplos de cómo usar VSDBCMD, consulte [referencia de línea de comandos de VSDBCMD. EXE (implementación e importación del esquema)](https://msdn.microsoft.com/library/dd193283.aspx) y [Cómo: importar un esquema desde un símbolo del sistema](https://msdn.microsoft.com/library/dd172135.aspx). Para obtener más información sobre el uso de aspnet\_regsql.exe crear bases de datos de pertenencia, vea [herramienta de registro de servidor de SQL de ASP.NET (aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx). Para obtener instrucciones más generales sobre la implementación de bases de datos de pertenencia, vea [Cómo: implementar las ASP.NET pertenencia base de datos sin incluidas cuentas de usuario](https://msdn.microsoft.com/library/ff361972(v=vs.100).aspx).

> [!div class="step-by-step"]
> [Anterior](deploying-database-role-memberships-to-test-environments.md)
> [Siguiente](excluding-files-and-folders-from-deployment.md)
