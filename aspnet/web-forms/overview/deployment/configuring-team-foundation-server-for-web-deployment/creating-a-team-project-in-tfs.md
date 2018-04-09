---
uid: web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
title: Crear un proyecto de equipo en TFS | Documentos de Microsoft
author: jrjlee
description: En este tema se describe cómo crear un nuevo proyecto de equipo en Team Foundation Server (TFS) 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: b28d3e2d-0bb4-4e29-a780-af810b964722
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-team-foundation-server-for-web-deployment/creating-a-team-project-in-tfs
msc.type: authoredcontent
ms.openlocfilehash: 79c069a601c0eafd84ae142241895428052acd29
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-team-project-in-tfs"></a>Crear un proyecto de equipo en TFS
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo crear un nuevo proyecto de equipo en Team Foundation Server (TFS) 2010.


Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluso una aplicación de ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

## <a name="task-overview"></a>Información general sobre tareas

Para aprovisionar y usar un nuevo proyecto de equipo en TFS, debe completar estos pasos de alto nivel:

- Conceder permisos al usuario que se creará el nuevo proyecto de equipo.
- Crear el proyecto de equipo.
- Conceder permisos a los miembros del equipo que vayan a trabajar en el proyecto.
- Compruebe en la parte del contenido.

En este tema le mostrará cómo realizar estos procedimientos, e identificará los usuarios y roles de trabajo que están probables que sean responsables de cada procedimiento. Tenga en cuenta que, dependiendo de la estructura de su organización, cada una de estas tareas puede ser la responsabilidad de una persona diferente.

Las tareas y los tutoriales en este tema se suponen que ha instalado y configurado TFS y que ha creado una colección de proyectos de equipo como parte del proceso de configuración. Para obtener más información sobre estas suposiciones y para obtener más información de fondo en el escenario, consulte [configurar un servidor de compilación de TFS para la implementación Web](configuring-a-tfs-build-server-for-web-deployment.md).

## <a name="grant-permissions-to-the-team-project-creator"></a>Conceder permisos para el creador del proyecto de equipo

Para crear un nuevo proyecto de equipo, necesitará estos permisos:

- Debe tener la **crear nuevos proyectos** permiso en el nivel de aplicación de TFS. Este permiso se concede normalmente mediante la adición de usuarios para el **Project Collection Administrators** grupo de TFS. El **administradores de Team Foundation** grupo global también incluye este permiso.
- Debe tener permiso para crear nuevos sitios de equipo dentro de la colección de sitios de SharePoint que corresponde a la colección de proyectos de equipo TFS. Este permiso se concede normalmente agregando el usuario a un grupo de SharePoint con **Control total** colección de sitios de derechos en SharePoint.
- Si usa características de SQL Server Reporting Services, debe ser miembro de la **Team Foundation Content Manager** rol en Reporting Services.

### <a name="who-performs-these-procedures"></a>¿Que lleva a cabo estos procedimientos?

Normalmente, la persona o grupo que administra la implementación de TFS también lleva a cabo estos procedimientos.

Dado que se trata de un conjunto de permisos con privilegios elevados, nuevos proyectos de equipo se crean normalmente un pequeño subconjunto de usuarios con la responsabilidad de administrar una implementación de TFS. Los desarrolladores no se normalmente le concedan los permisos necesarios para crear nuevos proyectos de equipo.

### <a name="grant-permissions-in-tfs"></a>Conceder permisos en TFS

Si desea que los usuarios puedan crear nuevos proyectos de equipo, es la primera tarea de alto nivel agregar el usuario a la **Project Collection Administrators** grupo de la colección de proyectos de equipo.

**Para agregar un usuario al grupo Project Collection Administrators**

1. En el servidor TFS, en la **iniciar** menú, elija **todos los programas**, haga clic en **Microsoft Team Foundation Server 2010**y, a continuación, haga clic en **Team Foundation Consola de administración de**.
2. En la vista de árbol de navegación, expanda **capa de aplicación**y, a continuación, haga clic en **colecciones de proyectos de equipo**.

    ![](creating-a-team-project-in-tfs/_static/image1.png)
3. En el **colecciones de proyectos de equipo** panel, seleccione el proyecto de equipo que desea administrar de colección.

    ![](creating-a-team-project-in-tfs/_static/image2.png)
4. En el **General** , haga clic en **pertenencia al grupo**.

    ![](creating-a-team-project-in-tfs/_static/image3.png)
5. En el **grupos globales** cuadro de diálogo, seleccione la **Project Collection Administrators** de grupo y, a continuación, haga clic en **propiedades**.
6. En el **propiedades de grupo de Team Foundation Server** cuadro de diálogo, seleccione **grupo o usuario de Windows**y, a continuación, haga clic en **agregar**.

    ![](creating-a-team-project-in-tfs/_static/image4.png)
7. En el **Seleccionar usuarios, equipos o grupos** cuadro de diálogo, escriba el nombre de usuario del usuario que desea que pueda crear nuevos proyectos de equipo, haga clic en **comprobar nombres**y, a continuación, haga clic en **Aceptar** .

    ![](creating-a-team-project-in-tfs/_static/image5.png)
8. En el **propiedades de grupo de Team Foundation Server** cuadro de diálogo, haga clic en **Aceptar**.
9. En el **grupos globales** cuadro de diálogo, haga clic en **cerrar**.

### <a name="grant-permissions-in-sharepoint-services"></a>Conceder permisos de SharePoint Services

A continuación, debe conceder al usuario permiso para crear nuevos sitios de equipo en la colección de sitios de SharePoint que corresponde a la colección de proyectos de equipo TFS.

**Para conceder permisos de Control total en la colección de sitios de SharePoint**

1. En la consola de administración de Team Foundation Server, en la **colecciones de proyectos de equipo** , seleccione la colección de proyectos de equipo que desea administrar.
2. En el **sitio de SharePoint** ficha, tenga en cuenta el valor de la **ubicación predeterminada del sitio actual** dirección URL.

    ![](creating-a-team-project-in-tfs/_static/image6.png)
3. Abra Internet Explorer y, a continuación, vaya a la dirección URL que anotó en el paso 2.

    > [!NOTE]
    > Si no ha iniciado sesión Windows como el usuario que creó la colección de proyectos de equipo, debe iniciar sesión en SharePoint como este usuario para poder continuar.
4. En el **acciones del sitio** menú, haga clic en **configuración del sitio**.

    ![](creating-a-team-project-in-tfs/_static/image7.png)
5. En el **configuración del sitio** página, en **usuarios y permisos**, haga clic en **personas y grupos**.
6. En el panel de navegación izquierdo, haga clic en **grupos**.

    ![](creating-a-team-project-in-tfs/_static/image8.png)
7. En el **personas y grupos: todos los grupos de** página, haga clic en **configurar grupos para este sitio**.

    ![](creating-a-team-project-in-tfs/_static/image9.png)

   > [!NOTE]
   > Es posible que reciba un <strong>HTTP 404 no encontrado</strong> error debido a un error de codificación doble de HTTP. Si esto ocurre, reemplace la dirección URL a este:   
   > [<em>dirección URL de la colección de sitios</em>] /\_layouts/permsetup.aspx  
   > Por ejemplo:  
   > http://tfs/sites/Fabrikam%20Web%20Projects/\_layouts/permsetup.aspx
8. En el **configurar grupos para este sitio** página, agregue el usuario que vayan a crear proyectos de equipo para el **propietarios** de grupo y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-team-project-in-tfs/_static/image10.png)

Para obtener más información acerca de cómo habilitar a los usuarios crear nuevos proyectos de equipo dentro de una colección de proyectos de equipo, consulte [establecer permisos de administrador para colecciones de proyectos de equipo](https://msdn.microsoft.com/library/dd547204.aspx).

## <a name="create-a-new-team-project-and-add-users"></a>Crear un nuevo proyecto de equipo y agregar usuarios

Una vez que tenga los permisos necesarios, puede usar el **Team Explorer** ventana en Visual Studio 2010 para crear un nuevo proyecto de equipo. Este enfoque proporciona a un asistente que recopila toda la información necesaria y realiza las tareas necesarias en TFS, SharePoint y SQL Server Reporting Services. También debe conceder permisos en el nuevo proyecto de equipo a los miembros del equipo del desarrollador, para que puedan agregar y modificar contenido.

### <a name="who-performs-these-procedures"></a>¿Que lleva a cabo estos procedimientos?

Normalmente un administrador de TFS o un jefe de equipo del desarrollador lleva a cabo estos procedimientos.

### <a name="create-a-new-team-project"></a>Cree un nuevo proyecto de equipo

El procedimiento siguiente describe cómo crear un nuevo proyecto de equipo en TFS 2010.

**Para crear un nuevo proyecto de equipo**

1. En el **iniciar** menú, elija **todos los programas**, haga clic en **Microsoft Visual Studio 2010**, haga clic en **Microsoft Visual Studio 2010**, y, a continuación, haga clic en **ejecutar como administrador**.

    > [!NOTE]
    > Si no ejecuta Visual Studio 2010 como administrador, se producirá un error en el Asistente para nuevo proyecto de equipo en el último paso.
2. Si el **User Account Control** aparece el cuadro de diálogo, haga clic en **Sí**.
3. En Visual Studio, en el **equipo** menú, haga clic en **conectar con Team Foundation Server**.

    > [!NOTE]
    > Si ya ha configurado una conexión a un servidor TFS, puede omitir los pasos del 4 al 7.
4. En el **conexión al proyecto de equipo** cuadro de diálogo, haga clic en **servidores**.
5. En el **agregar o quitar Team Foundation Server** cuadro de diálogo, haga clic en **agregar**.
6. En el **agregar Team Foundation Server** cuadro de diálogo, proporcione los detalles de la instancia de TFS y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-team-project-in-tfs/_static/image11.png)
7. En el **agregar o quitar Team Foundation Server** cuadro de diálogo, haga clic en **cerrar**.
8. En el **conectar al proyecto de equipo** cuadro de diálogo, seleccione la instancia TFS que desea conectarse a la red, seleccione el equipo de proyecto recopilación que desea agregar a y, a continuación, haga clic en **conectar**.

    ![](creating-a-team-project-in-tfs/_static/image12.png)
9. En el **Team Explorer** ventana, haga que el equipo de la colección de proyectos y, a continuación, haga clic en **nuevo proyecto de equipo**.

    ![](creating-a-team-project-in-tfs/_static/image13.png)
10. En el **nuevo proyecto de equipo** cuadro de diálogo, proporcione un nombre y una descripción para el proyecto de equipo y, a continuación, haga clic en **siguiente**.

    > [!NOTE]
    > Si el proyecto de equipo incluye espacios, pueden darse algunos problemas cuando llegue a utilizar la herramienta de implementación Web de Internet Information Services (IIS) (Web Deploy) para implementar paquetes de la ruta de acceso de salida. Espacios en la ruta de acceso pueden hacer mucho más difícil ejecutar comandos de Web Deploy.

    ![](creating-a-team-project-in-tfs/_static/image14.png)
11. En el **seleccione una plantilla de proceso** , seleccione la plantilla de proceso que desea usar para administrar el proceso de desarrollo y, a continuación, haga clic en **siguiente**.

    > [!NOTE]
    > Para obtener más información sobre plantillas de proceso de TFS, consulte [plantillas de proceso y las herramientas](https://msdn.microsoft.com/vstudio/aa718795).
12. En el **configuración del sitio de equipo** página, no modifique la configuración predeterminada y, a continuación, haga clic en **siguiente**.
13. Esta opción crea o identifica un sitio de grupo de SharePoint que está asociado con el proyecto de equipo TFS. El equipo de desarrollo puede utilizar este sitio para administrar la documentación, participar en los hilos de discusión, crear páginas wiki y realice otras tareas que no están relacionados con el código. Para obtener más información, consulte [las interacciones entre los productos de SharePoint y Team Foundation Server](https://msdn.microsoft.com/library/ms253177.aspx).
14. En el **especificar configuración de Control de código fuente** página, no modifique la configuración predeterminada y, a continuación, haga clic en **siguiente**.
15. Esta configuración identifica o crea la ubicación en la jerarquía de carpetas TFS que actuará como una carpeta raíz para el contenido.
16. En el **Confirmar configuración del proyecto de equipo** página, haga clic en **finalizar**.
17. Cuando el nuevo proyecto de equipo se crea correctamente, en la **proyecto de equipo creado** página, haga clic en **cerrar**.

### <a name="add-users-to-a-team-project"></a>Agregar usuarios a un proyecto de equipo

Ahora que ha creado el nuevo proyecto de equipo, puede conceder permisos a los usuarios para que puedan empezar a agregar y colaborar en el contenido.

**Para agregar usuarios a un proyecto de equipo**

1. En Visual Studio 2010, en la **Team Explorer** ventana, haga clic en el proyecto de equipo, seleccione **configuración del proyecto de equipo**y, a continuación, haga clic en **pertenencia al grupo**.

    ![](creating-a-team-project-in-tfs/_static/image15.png)
2. Para que los usuarios puedan agregar, modificar y quitar el código sometido a control de código fuente, debe agregarlo a la **colaboradores** grupo.
3. En el **grupos proyecto** cuadro de diálogo, seleccione la **colaboradores** de grupo y, a continuación, haga clic en **propiedades**.

    ![](creating-a-team-project-in-tfs/_static/image16.png)
4. En el **propiedades de grupo de Team Foundation Server** cuadro de diálogo, seleccione **grupo o usuario de Windows**y, a continuación, haga clic en **agregar**.

    ![](creating-a-team-project-in-tfs/_static/image17.png)
5. En el **Seleccionar usuarios, equipos o grupos** cuadro de diálogo, escriba el nombre de usuario del usuario que desea agregar al proyecto de equipo, haga clic en **comprobar nombres**y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-team-project-in-tfs/_static/image18.png)
6. En el **propiedades de grupo de Team Foundation Server** cuadro de diálogo, haga clic en **Aceptar**.
7. En el **grupos proyecto** cuadro de diálogo, haga clic en **cerrar**.

## <a name="conclusion"></a>Conclusión

En este punto, está listo para usar el nuevo proyecto de equipo y el equipo de desarrolladores puede empezar a agregar contenido y colaborar en el proceso de desarrollo.

El siguiente tema, [agregar contenido al Control de código fuente](adding-content-to-source-control.md), se describe cómo agregar contenido al control de código fuente.

## <a name="further-reading"></a>Información adicional

Para obtener más amplia información sobre la creación de proyectos de equipo en TFS, consulte [crear un proyecto de equipo](https://msdn.microsoft.com/library/ms181477(v=VS.100).aspx). Para obtener más información acerca de cómo habilitar a los usuarios crear nuevos proyectos de equipo dentro de una colección de proyectos de equipo, consulte [establecer permisos de administrador para colecciones de proyectos de equipo](https://msdn.microsoft.com/library/dd547204.aspx). Para obtener más información sobre cómo agregar usuarios a proyectos de equipo, consulte [agregar usuarios a proyectos de equipo](https://msdn.microsoft.com/library/bb558971.aspx).

> [!div class="step-by-step"]
> [Anterior](configuring-team-foundation-server-for-web-deployment.md)
> [Siguiente](adding-content-to-source-control.md)
