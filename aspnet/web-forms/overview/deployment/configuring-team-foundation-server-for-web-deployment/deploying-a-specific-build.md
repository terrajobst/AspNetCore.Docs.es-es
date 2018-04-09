---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
title: Implementar una compilación concreta | Documentos de Microsoft
author: jrjlee
description: En este tema se describe cómo implementar paquetes de web y scripts de base de datos desde una compilación anterior concreta a un nuevo destino, como un enviro de ensayo o de producción...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c979535f-48a3-4ec4-a633-a77889b86ddb
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/deploying-a-specific-build
msc.type: authoredcontent
ms.openlocfilehash: 271d084b3c69016df5be28ada032973bf7fd5a49
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="deploying-a-specific-build"></a>Implementar una compilación concreta
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo implementar paquetes de web y scripts de base de datos desde una compilación anterior concreta a un nuevo destino, como un entorno de ensayo o producción.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación e implementación se controla mediante dos archivos de proyecto&#x2014;uno que contiene las instrucciones de compilación que se aplican a cada entorno de destino y la otra contiene configuración específica del entorno de compilación e implementación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

Hasta ahora, los temas de este tutorial tiene se centra en cómo compilar, empaquetar e implementar aplicaciones web y bases de datos como parte de un solo paso o proceso automatizado. Sin embargo, en algunos escenarios comunes, conviene seleccionar los recursos que se implementación en una lista de las compilaciones en una carpeta de entrega. En otras palabras, la última compilación no puede ser la compilación que desea implementar.

Tenga en cuenta el escenario de integración continua (CI) descrito en el tema anterior, [crear una compilación que admite la implementación de la definición](creating-a-build-definition-that-supports-deployment.md). Ha creado una definición de compilación de Team Foundation Server (TFS) 2010. Cada vez que un desarrollador protege código en TFS, Team Build se compile el código, crear paquetes de web y las secuencias de comandos de base de datos como parte del proceso de compilación, ejecutar las pruebas unitarias e implementar los recursos en un entorno de prueba. Según la directiva de retención que se configuró al crear la definición de compilación, TFS conservará un cierto número de compilaciones anteriores.

![](deploying-a-specific-build/_static/image1.png)

Ahora, suponga que ha realizado la comprobación y validación de prueba en uno de ellos se basa en el entorno de prueba y está listo para implementar la aplicación en un entorno de ensayo. Mientras tanto, los programadores podrían haber protegido en el nuevo código. No desea volver a generar la solución e implementarla en el entorno de ensayo, y no desea implementar la compilación más reciente en el entorno de ensayo. En su lugar, va a implementar la compilación concreta que se ha verificado y validado en los servidores de prueba.

Para ello, debe saber dónde encontrar los paquetes de web y los scripts de base de datos generadas por una compilación concreta de Microsoft Build Engine (MSBuild).

## <a name="overriding-the-outputroot-property"></a>Invalidar la propiedad OutputRoot

En el [solución de ejemplo](../web-deployment-in-the-enterprise/the-contact-manager-solution.md), *Publish.proj* archivo declara una propiedad denominada **OutputRoot**. Como sugiere su nombre, esta es la carpeta raíz que todo lo que genera el proceso de compilación contiene. En el *Publish.proj* de archivos, puede ver que el **OutputRoot** propiedad hace referencia a la ubicación raíz para todos los recursos de implementación.

> [!NOTE]
> **OutputRoot** es un nombre de propiedad de uso frecuente. Los archivos de proyecto de Visual C# y Visual Basic también declaran esta propiedad para almacenar la ubicación raíz de todos los resultados de la compilación.


[!code-xml[Main](deploying-a-specific-build/samples/sample1.xml)]


Si desea que el archivo de proyecto para implementar paquetes de web y las secuencias de comandos desde una ubicación diferente de la base de datos&#x2014;como los resultados de una compilación TFS anterior&#x2014;simplemente necesita invalidar el **OutputRoot** propiedad. Debe establecer el valor de propiedad a la carpeta de compilación relevante en el servidor de Team Build. Si va a ejecutar MSBuild desde la línea de comandos, puede especificar un valor para **OutputRoot** como un argumento de línea de comandos:


[!code-console[Main](deploying-a-specific-build/samples/sample2.cmd)]


En la práctica, sin embargo, ¿también desea omitir la **generar** destino&#x2014;hay ningún punto de compilar la solución si no planea usar los resultados de compilación. Puede hacerlo mediante la especificación de los destinos que desea ejecutar desde la línea de comandos:


[!code-console[Main](deploying-a-specific-build/samples/sample3.cmd)]


Sin embargo, en la mayoría de los casos, deseará crear su lógica de implementación en una definición de compilación TFS. Esto permite a los usuarios con el **compilaciones en cola** permiso para desencadenar la implementación de cualquier instalación de Visual Studio con una conexión al servidor TFS.

## <a name="creating-a-build-definition-to-deploy-specific-builds"></a>Creación específica de una definición de compilación para implementar compilaciones

El procedimiento siguiente describe cómo crear una definición de compilación que permite a los usuarios a las implementaciones de desencadenador en un entorno de ensayo con un solo comando.

En este caso, no desea que la definición de compilación para compilar realmente nada&#x2014;su intención es que se ejecute la lógica de implementación en el archivo de proyecto personalizado. El *Publish.proj* archivo incluye una lógica condicional que omite el **generar** si está ejecutando el archivo en Team Build como destino. Esto consigue mediante la evaluación de los integrados **BuildingInTeamBuild** propiedad, que se establece automáticamente en **true** si ejecuta el archivo de proyecto en Team Build. Como resultado, puede omitir el proceso de compilación y simplemente ejecute el archivo de proyecto para implementar una compilación existente.

**Para crear una definición de compilación para desencadenar manualmente la implementación**

1. En Visual Studio 2010, en la **Team Explorer** ventana, expanda el nodo del proyecto de equipo, haga clic en **compilaciones**y, a continuación, haga clic en **nueva definición de compilación**.

    ![](deploying-a-specific-build/_static/image2.png)
2. En el **General** ficha, asigne un nombre a la definición de compilación (por ejemplo, **DeployToStaging**) y una descripción opcional.
3. En el **desencadenador** ficha, seleccione **Manual: protecciones no desencadenan una nueva compilación**.
4. En el **valores predeterminados de compilación** ficha la **resultados en la siguiente carpeta de destino de compilación copia** , escriba la ruta de acceso de convención de nomenclatura universal (UNC, Universal Naming Convention) de la carpeta de entrega (por ejemplo,  **\\TFSBUILD\Drops**).

    ![](deploying-a-specific-build/_static/image3.png)
5. En el **proceso** ficha la **archivo del proceso de compilación** lista desplegable, deje **DefaultTemplate.xaml** seleccionado. Esta es una de las plantillas de proceso de compilación predeterminadas que se agregan a todos los nuevos proyectos de equipo.
6. En el **parámetros del proceso de compilación** , haga clic en el **elementos para compilar** fila y, a continuación, haga clic en el **puntos suspensivos** botón.

    ![](deploying-a-specific-build/_static/image4.png)
7. En el **elementos para compilar** cuadro de diálogo, haga clic en **agregar**.
8. En el **elementos de tipo** lista desplegable, seleccione **archivos de proyecto de MSBuild**.
9. Vaya a la ubicación del archivo de proyecto personalizado con el que controlan el proceso de implementación, seleccione el archivo y, a continuación, haga clic en **Aceptar**.

    ![](deploying-a-specific-build/_static/image5.png)
10. En el **elementos para compilar** cuadro de diálogo, haga clic en **Aceptar**.
11. En el **parámetros del proceso de compilación** tabla, expanda la **avanzadas** sección.
12. En el **argumentos de MSBuild** de filas, especifique la ubicación del archivo de proyecto específicas del entorno y agregar un marcador de posición para la ubicación de la carpeta de compilación:

    [!code-console[Main](deploying-a-specific-build/samples/sample4.cmd)]

    ![](deploying-a-specific-build/_static/image6.png)

    > [!NOTE]
    > Debe invalidar el **OutputRoot** valor cada vez que se ponga en cola una compilación. Este tema se trata en el procedimiento siguiente.
13. Haga clic en **Guardar**.

Cuando se activa una compilación, debe actualizar el **OutputRoot** propiedad para que apunte a la compilación que desea implementar.

**Para implementar una compilación concreta de una definición de compilación**

1. En el **Team Explorer** ventana, haga clic en la definición de compilación y, a continuación, haga clic en **poner nueva compilación en cola**.

    ![](deploying-a-specific-build/_static/image7.png)
2. En el **poner en cola compilación** cuadro de diálogo la **parámetros** , expanda la **avanzadas** sección.
3. En el **argumentos de MSBuild** la fila, sustituya el valor de la **OutputRoot** propiedad con la ubicación de la carpeta de compilación. Por ejemplo:

    [!code-console[Main](deploying-a-specific-build/samples/sample5.cmd)]

    ![](deploying-a-specific-build/_static/image8.png)

    > [!NOTE]
    > No olvide incluir una barra diagonal al final de la ruta de acceso a la carpeta de compilación.
4. Haga clic en **cola**.

Cuando pone en cola la compilación, el archivo de proyecto implementará las secuencias de comandos de base de datos y paquetes de web de la carpeta de entrega de compilación que especificó en el **OutputRoot** propiedad.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo publicar los recursos de implementación, como paquetes de web y los scripts de base de datos, de un determinado anterior generar mediante el modelo de implementación del archivo de proyecto de división. Explica cómo reemplazar la **OutputRoot** definición de compilación de la propiedad y cómo incorporar la lógica de implementación en un TFS.

## <a name="further-reading"></a>Información adicional

Para obtener más información acerca de cómo crear definiciones de compilación, consulte [crear una definición de compilación básica](https://msdn.microsoft.com/library/ms181716.aspx) y [definir el proceso de compilación](https://msdn.microsoft.com/library/ms181715.aspx). Para obtener más instrucciones en las compilaciones de puesta en cola, vea [poner en cola una compilación](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Anterior](creating-a-build-definition-that-supports-deployment.md)
> [Siguiente](configuring-permissions-for-team-build-deployment.md)
