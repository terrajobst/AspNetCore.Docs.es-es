---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
title: Agregar contenido al Control de código fuente | Microsoft Docs
author: jrjlee
description: En este tema se explica cómo agregar contenido al control de código fuente en Team Foundation Server (TFS) 2010. Describe cómo agregar soluciones y proyectos a un proyecto de equipo...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 86c14aab-c2dd-4f73-b40c-c6d52fa44950
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/adding-content-to-source-control
msc.type: authoredcontent
ms.openlocfilehash: 9fdb2e37f2925273b457157b634865d93e865098
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838376"
---
<a name="adding-content-to-source-control"></a>Agregar contenido al Control de código fuente
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se explica cómo agregar contenido al control de código fuente en Team Foundation Server (TFS) 2010. Describe cómo agregar soluciones y proyectos a un proyecto de equipo en TFS y se explica cómo agregar dependencias externas, como los marcos de trabajo o los ensamblados al control de código fuente.


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

## <a name="task-overview"></a>Información general de tarea

En la mayoría de los casos, todos los miembros del equipo del desarrollador deben ser capaz de agregar contenido al control de código fuente. Para agregar una solución al control de código fuente en TFS, necesita completar estos pasos generales:

- Conectarse a un proyecto de equipo.
- Asignar la estructura de carpetas del proyecto de equipo en el servidor a una estructura de carpetas en el equipo local.
- Control de código fuente, agregue la solución y su contenido.
- Agregue cualquier dependencia externa para el control de código fuente.

En este tema le mostrará cómo realizar estos procedimientos.

Las tareas y los tutoriales en este tema se suponen que ya ha creado un nuevo proyecto de equipo TFS para administrar el contenido. Para obtener más información sobre cómo crear un nuevo proyecto de equipo, consulte [crear un proyecto de equipo en TFS](creating-a-team-project-in-tfs.md).

### <a name="who-performs-these-procedures"></a>¿Que lleva a cabo estos procedimientos?

En la mayoría de los casos, todos los miembros del equipo del desarrollador deberían poder agregar y modificar contenido dentro de los proyectos de equipo específico.

## <a name="connect-to-a-team-project-and-create-a-folder-mapping"></a>Conectarse a un proyecto de equipo y crear una asignación de carpeta

Antes de agregar cualquier contenido al control de código fuente, deberá conectarse a un proyecto de equipo y crear una asignación entre la estructura de carpetas en el servidor y el sistema de archivos en el equipo local.

**Para conectarse a un proyecto de equipo y asignar una ruta de acceso local**

1. En la estación de trabajo de desarrollador, abra Visual Studio 2010.
2. En Visual Studio, en el **equipo** menú, haga clic en **conectar con Team Foundation Server**.

    > [!NOTE]
    > Si ya ha configurado una conexión a un servidor de TFS, puede omitir los pasos 3 a 6.
3. En el **conexión al proyecto de equipo** cuadro de diálogo, haga clic en **servidores**.
4. En el **agregar o quitar Team Foundation Server** cuadro de diálogo, haga clic en **agregar**.
5. En el **agregar Team Foundation Server** cuadro de diálogo, proporcione los detalles de la instancia de TFS y, a continuación, haga clic en **Aceptar**.

    ![](adding-content-to-source-control/_static/image1.png)
6. En el **agregar o quitar Team Foundation Server** cuadro de diálogo, haga clic en **cerrar**.
7. En el **conectar al proyecto de equipo** cuadro de diálogo, seleccione la instancia de TFS que desea conectarse, seleccione el equipo de colección de proyectos, seleccione el proyecto de equipo que desea agregar a y, a continuación, haga clic en **Connect**.

    ![](adding-content-to-source-control/_static/image2.png)
8. En el **Team Explorer** , expanda el proyecto de equipo y, a continuación, haga doble clic en **Control de código fuente**.

    ![](adding-content-to-source-control/_static/image3.png)
9. En el **Explorador de Control de código fuente** , haga clic **no asignado**.

    ![](adding-content-to-source-control/_static/image4.png)
10. En el **mapa** cuadro de diálogo el **carpeta Local** cuadro, vaya a (o crear) una carpeta local para que actúe como la carpeta raíz del proyecto de equipo y, a continuación, haga clic en **mapa**.

    ![](adding-content-to-source-control/_static/image5.png)
11. Cuando se le pedirá que descargue archivos de origen, haga clic en **Sí**.

    ![](adding-content-to-source-control/_static/image6.png)

En este momento, ha asignado la carpeta del servidor para el proyecto de equipo a una carpeta local en la estación de trabajo de desarrollador. También ha descargado todo el contenido del proyecto de equipo a la estructura de carpetas locales. Ahora puede empezar a agregar su propio contenido al control de código fuente.

## <a name="add-projects-and-solutions-to-source-control"></a>Agregar proyectos y soluciones al Control de código fuente

Para agregar proyectos y soluciones al control de código fuente, primero deberá moverlos a la carpeta del proyecto de equipo en el equipo local. A continuación, puede proteger el contenido para sincronizar las adiciones con el servidor.

**Para agregar proyectos al control de código fuente**

1. En la estación de trabajo de desarrollador, mueva los proyectos y soluciones a una ubicación adecuada dentro de la estructura de carpeta asignada para el proyecto de equipo.

    > [!NOTE]
    > Muchas organizaciones tienen un enfoque preferido cómo deben organizarse proyectos y soluciones con control de código fuente. Para obtener instrucciones sobre cómo estructurar carpetas, consulte [How To: estructura de carpetas de Control de su origen en Team Foundation Server](https://msdn.microsoft.com/library/bb668992.aspx).
2. Abra la solución en Visual Studio 2010.
3. En el **el Explorador de soluciones** , haga clic en la solución y, a continuación, haga clic en **Agregar solución al Control de código fuente**.

    ![](adding-content-to-source-control/_static/image7.png)

    > [!NOTE]
    > En algunos casos, según cómo su organización le gusta contenido de la estructura en TFS, debe agregar proyectos al control de código fuente de forma individual para proporcionar un mayor control sobre cómo se organiza el código fuente.
4. Compruebe que la **Explorador de Control de código fuente** pestaña muestra el contenido que haya agregado dentro de la estructura de carpetas de servidor para el proyecto de equipo.

    ![](adding-content-to-source-control/_static/image8.png)

    > [!NOTE]
    > El **Explorador de Control de código fuente** pestaña muestra el contenido con ninguna otra solicitud dado que ha agregado la solución en una carpeta asignada en el sistema de archivos local. Si la solución se encontraba en una ubicación no asignada, ¿se le para especificar las ubicaciones de carpeta de TFS y el sistema de archivos local.
5. En el **Explorador de Control de código fuente** ficha la **carpetas** panel, haga clic en el proyecto de equipo (por ejemplo, **ContactManager**) y, a continuación, haga clic en **insertar en el repositorio Los cambios pendientes**.
6. En el **comprobar en: archivos de código fuente** cuadro de diálogo, escriba un comentario y, a continuación, haga clic en **proteger**.

    ![](adding-content-to-source-control/_static/image9.png)

En este momento ha agregado la solución al control de código fuente en TFS.

## <a name="add-external-dependencies-to-source-control"></a>Agregar dependencias externas a Control de código fuente

Cuando se agrega un proyecto o solución al control de código fuente, también se agregarán los archivos y carpetas en el proyecto o solución. Sin embargo, en muchos casos, proyectos y soluciones también dependen de las dependencias externas, como los ensamblados locales, para funcionar correctamente. Deberá agregar estos recursos de cualquier control de código fuente para permitir que Team Build y otros miembros del equipo de desarrollo de compilan el código correctamente.

Por ejemplo, la estructura de carpetas para la solución de ejemplo de Contact Manager incluye una carpeta denominada paquetes. Contiene el ensamblado y diversos recursos de apoyo para ADO.NET Entity Framework 4.1. La carpeta de paquetes no forma parte de la solución Contact Manager, pero la solución no se compilará correctamente sin él. Para habilitar Team Build compilar la solución, deberá agregar la carpeta de paquetes para el control de código fuente.

> [!NOTE]
> La inclusión de una carpeta de paquetes es típica de lo que sucede al agregar el Entity Framework, o recursos similares, a la solución mediante la extensión NuGet para Visual Studio 2010.


**Para agregar contenido de los proyectos al control de código fuente**

1. Asegúrese de que los elementos que desea agregar (por ejemplo, la carpeta de paquetes) en una ubicación adecuada dentro de una carpeta asignada en el sistema de archivos local.
2. En Visual Studio 2010, en el **Team Explorer** , expanda el proyecto de equipo y, a continuación, haga doble clic en **Control de código fuente**.

    ![](adding-content-to-source-control/_static/image10.png)
3. En el **Explorador de Control de código fuente** ficha la **carpetas** panel, seleccione la carpeta que contiene el elemento o elementos desea agregar.
4. Haga clic en el **agregar elementos a la carpeta** botón.

    ![](adding-content-to-source-control/_static/image11.png)
5. En el **agregar al Control de código fuente** cuadro de diálogo, seleccione la carpeta o los elementos que desea agregar y, a continuación, haga clic en **siguiente**.

    ![](adding-content-to-source-control/_static/image12.png)
6. En el **elementos excluidos** ficha, seleccione los elementos necesarios que se han excluido automáticamente (por ejemplo, los ensamblados) y, a continuación, haga clic en **incluir elementos**.

    ![](adding-content-to-source-control/_static/image13.png)
7. En el **elementos para agregar** , compruebe que se enumeran todos los archivos que van a incluir y, a continuación, haga clic en **finalizar**.

    ![](adding-content-to-source-control/_static/image14.png)
8. En el **Explorador de Control de código fuente** ventana, haga clic en el **proteger** botón.

    ![](adding-content-to-source-control/_static/image15.png)
9. En el **comprobar en: archivos de código fuente** cuadro de diálogo, escriba un comentario y, a continuación, haga clic en **proteger**.

En este momento, ha agregado las dependencias externas para la solución de control de código fuente.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo conectarse a un proyecto de equipo, asignar una estructura de carpetas y agregar contenido al control de código fuente. Para obtener más información sobre cómo trabajar con elementos en el control de código fuente, consulte [utilizando el Control de versiones](https://msdn.microsoft.com/library/ms181368.aspx).

El tema siguiente, [configurar un servidor de compilación de TFS para la implementación de Web](configuring-a-tfs-build-server-for-web-deployment.md), se describe cómo preparar un servidor TFS Team Build para compilar e implementar la solución.

## <a name="further-reading"></a>Información adicional

Para obtener información más completa sobre cómo trabajar con control de código fuente en TFS, consulte [utilizando el Control de versiones](https://msdn.microsoft.com/library/ms181368.aspx).

> [!div class="step-by-step"]
> [Anterior](creating-a-team-project-in-tfs.md)
> [Siguiente](configuring-a-tfs-build-server-for-web-deployment.md)
