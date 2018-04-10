---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Crear una definición de compilación que admite la implementación | Documentos de Microsoft
author: jrjlee
description: Si desea realizar cualquier tipo de compilación de Team Foundation Server (TFS) 2010, debe crear una definición de compilación dentro de su proyecto de equipo. Des de este tema...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: c5ea0bd9f01bb57b96abd349741f304c0093d887
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Crear una definición de compilación que admite la implementación
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Si desea realizar cualquier tipo de compilación de Team Foundation Server (TFS) 2010, debe crear una definición de compilación dentro de su proyecto de equipo. En este tema se describe cómo crear una nueva definición de compilación en TFS y cómo controlar la implementación de web como parte del proceso de compilación de Team Build.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación e implementación se controla mediante dos archivos de proyecto&#x2014;uno que contiene las instrucciones de compilación que se aplican a cada entorno de destino y la otra contiene configuración específica del entorno de compilación e implementación. En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general sobre tareas

Una definición de compilación es el mecanismo que controla cómo y cuándo se producen compilaciones de proyectos de equipo en TFS. Cada definición de compilación especifica:

- Las acciones que desee crear, como archivos de solución de Visual Studio o archivos de proyecto personalizados de Microsoft Build Engine (MSBuild).
- Los criterios que determinan cuándo se debe realizar una compilación coloca, como desencadenadores manuales, la integración continua (CI), o entradas validadas.
- La ubicación a la que Team Build debería enviar resultados de la compilación, incluidos los artefactos de implementación como paquetes de web y las secuencias de comandos de base de datos.
- La cantidad de tiempo que debe conservarse cada compilación.
- Varios otros parámetros del proceso de compilación.

> [!NOTE]
> Para obtener más información sobre las definiciones de compilación, consulte [definir el proceso de compilación](https://msdn.microsoft.com/library/ms181715.aspx).


En este tema le mostrará cómo crear una definición de compilación que utiliza el elemento de configuración, por lo que una compilación se desencadena cuando un desarrollador protege en contenido nuevo. Si la compilación se realiza correctamente, el servicio de compilación ejecuta un archivo de proyecto personalizadas para implementar la solución en un entorno de prueba.

Cuando se activa una compilación, estas acciones deben ocurrir:

- En primer lugar, Team Build debe compilar la solución. Como parte de este proceso, Team Build invocará la canalización de publicación de Web (WPP) para generar paquetes de implementación web para cada uno de los proyectos de aplicación web en la solución. Compilación de equipo también ejecutará las pruebas de unidad asociadas a la solución.
- Si se produce un error en la compilación de soluciones, Team Build no realizará ninguna otra acción. Errores de las pruebas unitarias deben tratarse como un error de compilación.
- Si se realiza correctamente la compilación de soluciones, Team Build debe ejecutar el archivo de proyecto personalizadas que controla la implementación de la solución. Como parte de este proceso, Team Build invocará la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) para instalar las aplicaciones web empaquetada en los servidores web de destino y va a invocar la utilidad VSDBCMD.exe para ejecutar la creación de la base de datos secuencias de comandos en los servidores de base de datos de destino.

Esto ilustra el proceso:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

El [póngase en contacto con el Administrador de](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución de ejemplo incluye un archivo de proyecto de MSBuild personalizado, *Publish.proj*, que puede ejecutar desde MSBuild o Team Build. Como se describe en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), este archivo de proyecto define la lógica que implementa los paquetes de web y las bases de datos en un entorno de destino. El archivo incluye lógica que omite el edificio y el proceso de empaquetado, si se está ejecutando en Team Build, dejando las tareas de implementación para que se ejecute. Esto se debe al automatizar la implementación de esta manera, normalmente querrá asegurarse de que la solución se compila correctamente y pasa cualquier prueba unitaria antes de comenzar el proceso de implementación.

La siguiente sección explica cómo implementar este proceso mediante la creación de una nueva definición de compilación.

> [!NOTE]
> Este procedimiento&#x2014;en que una sola automatizada proceso compilaciones, pruebas e implementa una solución&#x2014;es probable que sea más adecuado para la implementación en entornos de prueba. Para entornos de ensayo y producción está mucho más probable que desee distribuir el contenido desde una compilación anterior que ya haya verificado y validado en un entorno de prueba. Este enfoque se describe en el tema siguiente, [implementar una compilación concreta](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>¿Que lleva a cabo este procedimiento?

Normalmente, un administrador de TFS lleva a cabo este procedimiento. En algunos casos, un coordinador del equipo de desarrolladores puede asumir la responsabilidad de la colección de proyectos de equipo en TFS. Para crear una nueva definición de compilación, debe ser un miembro de la **Project Collection Administrators compilar** grupo de la colección de proyectos de equipo que contiene la solución.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Crear una definición de compilación para los elementos de configuración e implementación

El procedimiento siguiente describe cómo crear una definición de compilación que desencadena el elemento de configuración. Si la compilación se realiza correctamente, la solución se implementa con la lógica en un archivo de proyecto de MSBuild personalizado.

**Para crear una definición de compilación para los elementos de configuración e implementación**

1. En Visual Studio 2010, en la **Team Explorer** ventana, expanda el nodo del proyecto de equipo, haga clic en **compilaciones**y, a continuación, haga clic en **nueva definición de compilación**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. En el **General** ficha, asigne un nombre a la definición de compilación (por ejemplo, **DeployToTest**) y una descripción opcional.
3. En el **desencadenador** ficha, seleccione los criterios en el que desea desencadenar una compilación nueva. Por ejemplo, si desea compilar la solución e implementarla en el entorno de prueba, cada vez que un desarrollador protege en el nuevo código, seleccione **integración continua**.
4. En el **valores predeterminados de compilación** ficha la **resultados en la siguiente carpeta de destino de compilación copia** , escriba la ruta de acceso de convención de nomenclatura universal (UNC, Universal Naming Convention) de la carpeta de entrega (por ejemplo,  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Esta ubicación de destino almacena varias compilaciones, dependiendo de la directiva de retención configurada. Si desea publicar artefactos de implementación desde una compilación concreta en un entorno de ensayo o de producción, esto es donde podrá encontrarlos.
5. En el **proceso** ficha la **archivo del proceso de compilación** lista desplegable, deje **DefaultTemplate.xaml** seleccionado. Esta es una de las plantillas de proceso de compilación predeterminadas que se agregan a todos los nuevos proyectos de equipo.
6. En el **parámetros del proceso de compilación** , haga clic en el **elementos para compilar** fila y, a continuación, haga clic en el **puntos suspensivos** botón.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. En el **elementos para compilar** cuadro de diálogo, haga clic en **agregar**.
8. Vaya a la ubicación del archivo de solución y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. En el **elementos para compilar** cuadro de diálogo, haga clic en **agregar**.
10. En el **elementos de tipo** lista desplegable, seleccione **archivos de proyecto de MSBuild**.
11. Vaya a la ubicación del archivo de proyecto personalizado con el que controlan el proceso de implementación, seleccione el archivo y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. El **elementos para compilar** cuadro de diálogo debe mostrar ahora dos elementos. Haga clic en **Aceptar**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. En el **proceso** ficha la **parámetros del proceso de compilación** tabla, expanda la **avanzadas** sección.
14. En el **argumentos de MSBuild** la fila, agregue los argumentos de línea de comandos de MSBuild que *cualquier* de los elementos para compilar requiere. En el escenario de solución póngase en contacto con el administrador, se requieren estos argumentos:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. En este ejemplo:

    1. El **DeployOnBuild = true** y **DeployTarget = paquete** argumentos son necesarios cuando se compila la solución póngase en contacto con el administrador. Esto indica a MSBuild que cree web paquetes de implementación después de compilar cada proyecto de aplicación web, como se describe en [edificio y proyectos de aplicación Web de empaquetado](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. El **TargetEnvPropsFile** argumento es necesario cuando se compila el *Publish.proj* archivo. Esta propiedad indica la ubicación del archivo de configuración específicos del entorno, tal y como se describe en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. En el **directiva de retención** pestaña, configure el número de compilaciones de cada tipo que desea conservar según sea necesario.
17. Haga clic en **Guardar**.

## <a name="queue-a-build"></a>Poner en cola una compilación

En este momento, ha creado al menos una nueva definición de compilación. Ahora se ejecutará el proceso de compilación que define según los desencadenadores que especificó en la definición de compilación.

Si ha configurado la definición de compilación para usar el elemento de configuración, puede probar la definición de compilación de dos maneras:

- Compruebe en algún contenido al proyecto de equipo para desencadenar una compilación automática.
- Poner en cola una compilación manualmente.

**Para poner en cola una compilación manualmente**

1. En el **Team Explorer** ventana, haga clic en la definición de compilación y, a continuación, haga clic en **poner nueva compilación en cola**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. En el **poner en cola compilación** cuadro de diálogo, revise las propiedades de compilación y, a continuación, haga clic en **cola**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Para revisar el progreso y el resultado de una compilación&#x2014;independientemente de si se activó de forma manual o automática&#x2014;haga doble clic en la definición de compilación en el **Team Explorer** ventana. Se abrirá una **Explorador de compilaciones** ficha.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Desde aquí, puede solucionar los errores en las generaciones. Si hace doble clic en una generación individual, puede ver información de resumen y haga clic en a través de los archivos de registro detallado.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Puede utilizar esta información para solucionar problemas de errores en las generaciones y solucionar los problemas antes de intentar otra compilación.

> [!NOTE]
> Compilaciones que se ejecutan la lógica de implementación son propensos a errores hasta que se hayan concedido al servidor de compilación de los permisos necesarios en el entorno de destino. Para obtener más información, consulte [configurar permisos para el equipo de compilación implementación](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Supervisar el proceso de compilación

Proporciona una amplia gama de funcionalidad para ayudarle a supervisar el proceso de compilación en TFS. Por ejemplo, TFS puede enviar un correo electrónico o mostrar las alertas en el área de notificación de la barra de tareas cuando se complete una compilación. Para obtener más información, consulte [ejecutar y supervisar las compilaciones](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo crear una definición de compilación en TFS. La definición de compilación está configurada para el elemento de configuración, por lo que el proceso de compilación se ejecute siempre que un desarrollador protege en el contenido al proyecto de equipo. La definición de compilación ejecuta un archivo de proyecto de MSBuild personalizado para implementar paquetes de web y las secuencias de comandos de base de datos en un entorno de servidor de destino.

En orden para una implementación automatizada se realice correctamente como parte de un proceso de compilación, deberá conceder los permisos adecuados a la cuenta de servicio de compilación en los servidores web de destino y el servidor de base de datos de destino. En este tutorial, el tema final [configurar permisos para el equipo de compilación implementación](configuring-permissions-for-team-build-deployment.md), se describe cómo identificar y configurar los permisos necesarios para la implementación automatizada de un servidor de Team Build.

## <a name="further-reading"></a>Información adicional

Para obtener más información acerca de cómo crear definiciones de compilación, consulte [crear una definición de compilación básica](https://msdn.microsoft.com/library/ms181716.aspx) y [definir el proceso de compilación](https://msdn.microsoft.com/library/ms181715.aspx). Para obtener más instrucciones en las compilaciones de puesta en cola, vea [poner en cola una compilación](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-a-tfs-build-server-for-web-deployment.md)
> [Siguiente](deploying-a-specific-build.md)
