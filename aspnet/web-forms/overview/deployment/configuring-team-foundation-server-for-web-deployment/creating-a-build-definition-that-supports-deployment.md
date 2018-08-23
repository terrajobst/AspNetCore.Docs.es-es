---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
title: Crear una definición de compilación que admite la implementación | Microsoft Docs
author: jrjlee
description: Si desea realizar cualquier tipo de compilación en Team Foundation Server (TFS) 2010, deberá crear una definición de compilación dentro de su proyecto de equipo. Des de este tema...
ms.author: riande
ms.date: 05/04/2012
ms.assetid: fe47a018-f6d0-4979-80e7-5b1fa75a5865
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-build-definition-that-supports-deployment
msc.type: authoredcontent
ms.openlocfilehash: 33ebde3074603801945c676ace64b26ca5bbf44a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835897"
---
<a name="creating-a-build-definition-that-supports-deployment"></a>Crear una definición de compilación que admite la implementación
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Si desea realizar cualquier tipo de compilación en Team Foundation Server (TFS) 2010, deberá crear una definición de compilación dentro de su proyecto de equipo. Este tema describe cómo crear una nueva definición de compilación en TFS y cómo controlar la implementación web como parte del proceso de compilación en Team Build.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación e implementación se controla mediante dos archivos de proyecto&#x2014;uno que contiene las instrucciones de compilación que se aplican a todos los entornos de destino y que contiene la configuración específica del entorno de compilación e implementación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general de tarea

Una definición de compilación es el mecanismo que controla cómo y cuándo se producen las compilaciones de proyectos de equipo en TFS. Cada definición de compilación especifica:

- Las cosas que desea generar, como archivos de solución de Visual Studio o archivos de proyecto personalizados de Microsoft Build Engine (MSBuild).
- Los criterios que determinan cuándo se debe realizar una compilación de colocan, como desencadenadores manuales, integración continua (CI), o entradas validadas.
- La ubicación a la que Team Build debería enviar resultados de la compilación, incluidos los artefactos de implementación como paquetes de web y scripts de base de datos.
- La cantidad de tiempo que debe conservarse cada compilación.
- Otros parámetros diferentes del proceso de compilación.

> [!NOTE]
> Para obtener más información sobre las definiciones de compilación, consulte [definir el proceso de compilación](https://msdn.microsoft.com/library/ms181715.aspx).


En este tema le mostrará cómo crear una definición de compilación que usa integración continua, por lo que se desencadena una compilación cuando un desarrollador protege contenido nuevo. Si la compilación se realiza correctamente, el servicio de compilación ejecuta un archivo de proyecto personalizadas para implementar la solución en un entorno de prueba.

Cuando se desencadena una compilación, estas acciones necesitan:

- En primer lugar, Team Build debe compilar la solución. Como parte de este proceso, Team Build invocará la canalización de publicación de Web (WPP) para generar paquetes de implementación web para cada uno de los proyectos de aplicación web en la solución. Compilación de equipo también ejecutará las pruebas unitarias asociadas con la solución.
- Si se produce un error en la compilación de la solución, Team Build debería realizar ninguna otra acción. Errores en las pruebas unitarias deben tratarse como un error de compilación.
- Si se realiza correctamente la compilación de la solución, Team Build debe ejecutar el archivo de proyecto personalizado que controla la implementación de la solución. Como parte de este proceso, Team Build invocará la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) para instalar las aplicaciones web empaquetados en los servidores web de destino e invocará la utilidad VSDBCMD.exe para ejecutar la creación de la base de datos secuencias de comandos en los servidores de base de datos de destino.

Esto ilustra el proceso:

![](creating-a-build-definition-that-supports-deployment/_static/image1.png)

El [Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md) solución de ejemplo incluye un archivo de proyecto de MSBuild personalizado, *Publish.proj*, que se puede ejecutar desde Team Build o MSBuild. Como se describe en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md), este archivo de proyecto define la lógica que implementa los paquetes de web y bases de datos en un entorno de destino. El archivo incluye la lógica que omite la creación y el proceso de empaquetado si se está ejecutando en Team Build, dejando las tareas de implementación para ejecutar. Esto se debe al automatizar la implementación de esta manera, normalmente deseará asegurarse de que la solución se compila correctamente y pasa las pruebas unitarias antes de comenzar el proceso de implementación.

La siguiente sección explica cómo implementar este proceso mediante la creación de una definición de compilación.

> [!NOTE]
> Este procedimiento&#x2014;en que una sola automatizada proceso compilaciones, pruebas y se implementa una solución&#x2014;es probable que sea más adecuado para la implementación en entornos de prueba. Para entornos de ensayo y producción está mucho más probable que desee implementar el contenido desde una compilación anterior que ya ha comprobado y validado en un entorno de prueba. Este enfoque se describe en el tema siguiente, [implementar una compilación específica](deploying-a-specific-build.md).


### <a name="who-performs-this-procedure"></a>¿Que lleva a cabo este procedimiento?

Normalmente, un administrador de TFS realiza este procedimiento. En algunos casos, un líder de equipo de desarrollador puede asumir la responsabilidad de la colección de proyectos de equipo en TFS. Para crear una nueva definición de compilación, deberá ser miembro de la **Project Collection Build Administrators** para la colección de proyectos de equipo que contiene la solución.

## <a name="create-a-build-definition-for-ci-and-deployment"></a>Crear una definición de compilación para la integración continua e implementación

El siguiente procedimiento describe cómo crear una definición de compilación que desencadena la integración continua. Si la compilación se realiza correctamente, la solución se implementa mediante la lógica en un archivo de proyecto de MSBuild personalizado.

**Para crear una definición de compilación para la integración continua e implementación**

1. En Visual Studio 2010, en el **Team Explorer** ventana, expanda el nodo del proyecto de equipo, haga clic en **compilaciones**y, a continuación, haga clic en **nueva definición de compilación**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image2.png)
2. En el **General** ficha, asigne un nombre a la definición de compilación (por ejemplo, **DeployToTest**) y una descripción opcional.
3. En el **desencadenador** ficha, seleccione los criterios en el que desea desencadenar una nueva compilación. Por ejemplo, si desea compilar la solución e implementar en el entorno de prueba cada vez que un desarrollador protege en el nuevo código, seleccione **integración continua**.
4. En el **valores predeterminados de compilación** ficha la **copiar la salida en la siguiente carpeta de entrega de compilación** , escriba la ruta de acceso de convención de nomenclatura Universal (UNC) de la carpeta de entrega (por ejemplo,  **\\TFSBUILD\Drops**).

    ![](creating-a-build-definition-that-supports-deployment/_static/image3.png)

    > [!NOTE]
    > Esta ubicación de destino almacena varias compilaciones, según la directiva de retención configurada. Cuando desea publicar los artefactos de implementación de una compilación concreta en un entorno de ensayo o producción, esto es donde encontrará.
5. En el **proceso** ficha la **archivo proceso de compilación** lista desplegable, deje **DefaultTemplate.xaml** seleccionado. Esta es una de las plantillas de proceso de compilación predeterminada que se agregan a todos los nuevos proyectos de equipo.
6. En el **parámetros del proceso de compilación** , haga clic en el **elementos para compilar** fila y, a continuación, haga clic en el **puntos suspensivos** botón.

    ![](creating-a-build-definition-that-supports-deployment/_static/image4.png)
7. En el **elementos para compilar** cuadro de diálogo, haga clic en **agregar**.
8. Vaya a la ubicación del archivo de solución y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image5.png)
9. En el **elementos para compilar** cuadro de diálogo, haga clic en **agregar**.
10. En el **elementos de tipo** lista desplegable, seleccione **archivos de proyecto de MSBuild**.
11. Vaya a la ubicación del archivo de proyecto personalizada con el que el proceso de implementación de control, seleccione el archivo y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image6.png)
12. El **elementos para compilar** cuadro de diálogo debería aparecer ahora dos elementos. Haga clic en **Aceptar**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image7.png)
13. En el **proceso** ficha la **parámetros del proceso de compilación** de tabla, expanda el **avanzadas** sección.
14. En el **argumentos de MSBuild** de fila, agregue los argumentos de línea de comandos de MSBuild que *cualquier* de los elementos de compilación requiere. En el escenario de solución Contact Manager, estos argumentos son necesarios:

    [!code-console[Main](creating-a-build-definition-that-supports-deployment/samples/sample1.cmd)]

    ![](creating-a-build-definition-that-supports-deployment/_static/image8.png)
15. En este ejemplo:

    1. El **DeployOnBuild = true** y **DeployTarget = paquete** argumentos son necesarios cuando se compila la solución Contact Manager. Esto indica a MSBuild que cree web paquetes de implementación después de compilar cada proyecto de aplicación web, como se describe en [compilar y empaquetar proyectos de aplicación Web](../web-deployment-in-the-enterprise/building-and-packaging-web-application-projects.md).
    2. El **TargetEnvPropsFile** argumento es necesario cuando se compila el *Publish.proj* archivo. Esta propiedad indica la ubicación del archivo de configuración específicos del entorno, como se describe en [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).
16. En el **directiva de retención** pestaña, configure el número de compilaciones de cada tipo que desea conservar según sea necesario.
17. Haga clic en **Guardar**.

## <a name="queue-a-build"></a>Poner en cola una compilación

En este momento, ha creado al menos una nueva definición de compilación. Ahora se ejecutará el proceso de compilación que definen según los desencadenadores que especificó en la definición de compilación.

Si ha configurado la definición de compilación para usar la integración continua, puede probar la definición de compilación de dos maneras:

- Compruebe en algún contenido al proyecto de equipo para desencadenar una compilación automática.
- Poner en cola una compilación manualmente.

**Para poner en cola manualmente una compilación**

1. En el **Team Explorer** , haga clic en la definición de compilación y, a continuación, haga clic en **poner nueva compilación en cola**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image9.png)
2. En el **Poner compilación en cola** cuadro de diálogo, revisar las propiedades de compilación y, a continuación, haga clic en **cola**.

    ![](creating-a-build-definition-that-supports-deployment/_static/image10.png)

Para revisar el progreso y el resultado de una compilación&#x2014;independientemente de si se activó de forma manual o automática&#x2014;haga doble clic en la definición de compilación en el **Team Explorer** ventana. Se abrirá un **Build Explorer** ficha.

![](creating-a-build-definition-that-supports-deployment/_static/image11.png)

Desde aquí, puede solucionar un error de compilación. Si hace doble clic en una compilación individual, puede ver información de resumen y haga clic en a través de los archivos de registro detallada.

![](creating-a-build-definition-that-supports-deployment/_static/image12.png)

Puede usar esta información para solucionar un error de compilación y resolver los posibles problemas antes de intentar otra compilación.

> [!NOTE]
> Es probable que las compilaciones que ejecutan la lógica de implementación a un error hasta que se hayan concedido al servidor de compilación de los permisos necesarios en el entorno de destino. Para obtener más información, consulte [configurar permisos para la implementación de equipo de compilación](configuring-permissions-for-team-build-deployment.md).


## <a name="monitor-the-build-process"></a>Supervisar el proceso de compilación

Proporciona una amplia gama de funcionalidades para ayudarle a supervisar el proceso de compilación TFS. Por ejemplo, TFS puede enviar un correo electrónico o mostrar las alertas en el área de notificación de la barra de tareas cuando se ha completado una compilación. Para obtener más información, consulte [ejecutar y supervisar compilaciones](https://msdn.microsoft.com/library/ms181721.aspx).

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo crear una definición de compilación en TFS. La definición de compilación está configurada para la integración continua, por lo que el proceso de compilación se ejecuta cada vez que un desarrollador protege el contenido al proyecto de equipo. La definición de compilación ejecuta un archivo de proyecto de MSBuild personalizado para implementar paquetes de web y scripts de base de datos en un entorno de servidor de destino.

En orden para una implementación automatizada tener éxito como parte de un proceso de compilación, deberá conceder los permisos adecuados a la cuenta de servicio de compilación en los servidores web de destino y el servidor de base de datos de destino. En este tutorial, el tema final [configurar permisos para la implementación de equipo de compilación](configuring-permissions-for-team-build-deployment.md), se describe cómo identificar y configurar los permisos necesarios para la implementación automatizada de un servidor de Team Build.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre cómo crear definiciones de compilación, véase [crear una definición de compilación básica](https://msdn.microsoft.com/library/ms181716.aspx) y [definir el proceso de compilación](https://msdn.microsoft.com/library/ms181715.aspx). Para obtener instrucciones sobre la puesta en cola compilaciones, consulte [poner en cola una compilación](https://msdn.microsoft.com/library/ms181722.aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-a-tfs-build-server-for-web-deployment.md)
> [Siguiente](deploying-a-specific-build.md)
