---
title: "Implementación continua en Azure con Visual Studio y Git"
author: rick-anderson
description: "Aprenda a crear una aplicación web de ASP.NET Core con Visual Studio y a implementarla en Azure App Service con Git para una implementación continua."
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 2707c7a8-2350-4304-9856-fda58e5c0a16
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/azure-continuous-deployment
ms.openlocfilehash: a9efad38b1c75bd3a186b4ec85861357ecf744b9
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Implementación continua en Azure para ASP.NET Core con Visual Studio y Git

Por [Erik Reitan](https://github.com/Erikre)

En este tutorial se muestra cómo crear una aplicación web de ASP.NET Core con Visual Studio y a implementarla desde Visual Studio en Azure App Service mediante una implementación continua. 

Vea también [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic) (Usar VSTS para crear y publicar una aplicación web de Azure con la implementación continua), donde se muestra cómo configurar un flujo de trabajo de una entrega continua (CD) para [Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/) con Visual Studio Team Services. La entrega continua de Azure en Team Services simplifica la configuración de una canalización de implementación sólida para publicar actualizaciones de la aplicación en Azure App Service. La canalización se puede configurar desde Azure Portal para crear y ejecutar pruebas, implementarlas en un espacio de ensayo y luego implementarlas en un entorno de producción.

> [!NOTE]
> Para seguir este tutorial necesita una cuenta de Microsoft Azure. Si no tiene una, puede [activar las ventajas como suscriptor a MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F) o [registrarse para obtener una evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se da por hecho que ya ha instalado lo siguiente:

* [Visual Studio](https://www.visualstudio.com)

* [ASP.NET Core](https://download.microsoft.com/download/F/6/E/F6ECBBCC-B02F-424E-8E03-D47E9FA631B7/DotNetCore.1.0.1-VS2015Tools.Preview2.0.3.exe) (tiempo de ejecución y herramientas)

* [Git](https://git-scm.com/downloads) para Windows

## <a name="create-an-aspnet-core-web-app"></a>Crear una aplicación web de ASP.NET Core

1. Inicie Visual Studio.

2. En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.

3. Seleccione la plantilla de proyecto **Aplicación web ASP.NET**. Aparece en **Instalados** > **Plantillas** > **Visual C#** > **Web**. Dé un nombre al proyecto `SampleWebAppDemo`. Seleccione la opción **Crear nuevo repositorio de Git** y haga clic en **Aceptar**.

   ![Cuadro de diálogo Nuevo proyecto](azure-continuous-deployment/_static/01-new-project.png)

4. En el cuadro de diálogo **Nuevo proyecto ASP.NET**, seleccione la plantilla **Vacía** de ASP.NET Core y haga clic en **Aceptar**.

   ![Cuadro de diálogo Nuevo proyecto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)


### <a name="running-the-web-app-locally"></a>Ejecutar la aplicación web de forma local

1. Cuando Visual Studio haya acabado de crear la aplicación, ejecútela seleccionando **Depurar** -> **Iniciar depuración**. Como alternativa puede pulsar **F5**.

   Visual Studio y la aplicación nueva pueden tardar un poco en inicializarse. Cuando se hayan inicializado, el explorador mostrará la aplicación en ejecución.

   ![Ventana del explorador en la que aparece la aplicación en ejecución que muestra "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

2. Después de revisar la aplicación web en ejecución, cierre el explorador y haga clic en el icono "Detener depuración" de la barra de herramientas de Visual Studio para detener la aplicación.

## <a name="create-a-web-app-in-the-azure-portal"></a>Crear una aplicación web en Azure Portal

Los siguientes pasos le guiarán por el proceso de creación de una aplicación web en Azure Portal.

1. Inicie sesión en [Azure Portal](https://portal.azure.com).
2. Pulse **Nuevo** en la parte superior izquierda del portal.
3. Pulse **Web y móvil** > **Aplicación web**.

    ![Microsoft Azure Portal: Botón Nuevo: Web y móvil en Marketplace: Botón Aplicación web en Aplicaciones destacadas](azure-continuous-deployment/_static/05-azure-newwebapp.png)

4.  En la hoja **Aplicación web**, escriba un valor único para el **nombre del servicio de aplicaciones**.

    ![Hoja Aplicación web](azure-continuous-deployment/_static/06-azure-newappblade.png)

    >[!NOTE]
    >El **nombre del servicio de aplicaciones** debe ser único. El portal aplicará esta regla al intentar escribir el nombre. Después de escribir un valor diferente, deberá sustituir dicho valor en todas las apariciones de **SampleWebAppDemo** que vea en este tutorial.

    &nbsp;
    
    También en la hoja **Aplicación web**, seleccione un plan o ubicación existente de App Service o bien cree uno. Si crea un plan, seleccione el plan de tarifa, la ubicación y otras opciones. Para más información sobre los planes de App Service, vea [Introducción detallada sobre los planes de Azure App Service](https://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/).

5.  Haga clic en **Crear**. Azure aprovisionará e iniciará la aplicación web.

    ![Azure Portal: Hoja de Essentials SampleWebAppDemo01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Habilitar la publicación de Git para la nueva aplicación web

Git es un sistema de control de versiones distribuido que puede usar para implementar su aplicación web de Azure App Service. Va a almacenar el código que escriba para la aplicación web en un repositorio local de Git y va a implementar el código en Azure insertándolo en un repositorio remoto.

1. Inicie sesión en [Azure Portal](https://portal.azure.com), si aún no lo ha hecho.

2. Haga clic en **Examinar**, opción que se encuentra en la parte inferior del panel de navegación.

3. Haga clic en **Aplicaciones web** para ver una lista de las aplicaciones web asociadas a su suscripción de Azure.

4. Seleccione la aplicación web que creó en la sección anterior de este tutorial.

5. Si no se muestra la hoja **Configuración**, seleccione **Configuración** en la hoja **Aplicación web**.

6. En la hoja **Configuración**, seleccione **Origen de implementación** > **Elegir origen** > **Repositorio de Git local**.

   ![Hoja Configuración: Hoja Origen de implementación: Hoja Elegir origen](azure-continuous-deployment/_static/08-azure-localrepository.png)

7. Haga clic en **Aceptar**.

8. Si no ha configurado antes las credenciales de implementación para publicar una aplicación web u otra aplicación de App Service, configúrelas ahora:

   * Haga clic en **Configuración** > **Credenciales de implementación**. Se mostrará la hoja **Configurar credenciales de implementación**.

   * Cree un nombre de usuario y una contraseña.  Necesitará esta contraseña cuando configure Git.

   * Haga clic en **Guardar**.

9. En la hoja **Aplicación web**, haga clic en **Configuración** > **Propiedades**. La dirección URL del repositorio remoto de Git en el que va a efectuar la implementación aparece en **Dirección URL de Git**.

10. Copie el valor de **Dirección URL de Git** para usarlo más adelante en el tutorial.

   ![Azure Portal: Hoja Propiedades de la aplicación](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-your-web-app-to-azure-app-service"></a>Publicar la aplicación web en Azure App Service

En esta sección crearemos un repositorio local de Git con Visual Studio y lo insertaremos desde ese repositorio a Azure para implementar la aplicación web. Los pasos son los siguientes:

   * Agregue la opción del repositorio remoto con el valor de la dirección URL de Git, de modo que pueda implementar el repositorio local en Azure.

   * Confirme los cambios del proyecto.

   * Inserte los cambios del proyecto del repositorio local en el repositorio remoto de Azure.

&nbsp;
   
1.  En el **Explorador de soluciones**, haga clic con el botón derecho en **Solución 'SampleWebAppDemo'** y seleccione **Confirmar**. Aparecerá **Team Explorer**.

    ![Pestaña Team Explorer: Conexión](azure-continuous-deployment/_static/10-team-explorer.png)

2.  En **Team Explorer**, seleccione **Inicio** (icono Inicio) > **Configuración** > **Configuración del repositorio**.

3.  En la sección **Remotos** de **Configuración del repositorio**, seleccione **Agregar**. Aparecerá el cuadro de diálogo **Agregar remoto**.

4.  Establezca el **nombre** del repositorio remoto en **Azure-SampleApp**.

5.  Establezca el valor de **Recuperar** en la **dirección URL de Git** que ha copiado de Azure en este tutorial. Tenga en cuenta que esta es la dirección URL que termina en **.git**.

    ![Cuadro de diálogo Editar Azure-SampleApp remoto](azure-continuous-deployment/_static/11-add-remote.png)

    >[!NOTE]
    >Como alternativa, puede especificar el repositorio remoto desde la **ventana de comandos**. Para ello, abra la **ventana de comandos**, vaya al directorio del proyecto y escriba el comando. Por ejemplo:`git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

6.  Seleccione **Inicio** (icono de Inicio) > **Configuración** > **Configuración global**. Asegúrese de haber establecido el nombre y la dirección de correo electrónico. También deberá seleccionar **Actualizar**.

7.  Seleccione **Inicio** > **Cambios** para volver a la vista **Cambios**.

8.  Escriba un mensaje de confirmación, como **Inserción inicial n.º 1**, y haga clic en **Confirmar**. Esta acción creará una *confirmación* de forma local. Luego deberá efectuar una *sincronización* con Azure.

    ![Pestaña Team Explorer: Conexión](azure-continuous-deployment/_static/12-initial-commit.png)

    >[!NOTE]
    >Como alternativa, puede confirmar los cambios desde la **ventana de comandos**. Para ello, abra la **ventana de comandos**, vaya al directorio del proyecto y escriba los comandos git. Por ejemplo:
    >
    >`git add .`
    >
    >`git commit -am "Initial Push #1"`

9.  Seleccione **Inicio** > **Sincronizar** > **Acciones** > **Abrir símbolo del sistema**. Se abrirá el símbolo del sistema en el directorio del proyecto.

10.  Escriba el siguiente comando en la ventana de comandos:

    `git push -u Azure-SampleApp master`

11.  Escriba la contraseña de sus **credenciales de implementación** de Azure que creó anteriormente en Azure.

    >[!NOTE]
    >La contraseña no se verá a medida que la escriba.

    Este comando iniciará el proceso de inserción de los archivos locales del proyecto en Azure. La salida del comando anterior finaliza con un mensaje que indica que la implementación se efectuó correctamente.
        
    ```
    remote: Finished successfully.
    remote: Running post deployment command(s)...
    remote: Deployment successful.
    To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
    * [new branch]      master -> master
    Branch master set up to track remote branch master from Azure-SampleApp.
    ```
    > [!NOTE]
    > Si necesita colaborar en un proyecto, piense en la posibilidad de efectuar una inserción en [GitHub](https://github.com) mientras efectúa una inserción en Azure.
 
### <a name="verify-the-active-deployment"></a>Comprobar la implementación activa

Puede comprobar que transfirió correctamente la aplicación web del entorno local a Azure. Verá que aparece la implementación correcta.

1. En [Azure Portal](https://portal.azure.com), seleccione la aplicación web. Luego, seleccione **Configuración** > **Implementación continua**.

   ![Azure Portal: Hoja Configuración: Hoja Implementaciones donde se muestra una implementación correcta](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Ejecutar la aplicación en Azure

Ahora que ha implementado la aplicación web en Azure, puede ejecutarla.

Esto se puede hacer de dos maneras:

* En Azure Portal, localice la hoja Aplicación web de su aplicación web y haga clic en **Examinar** para ver la aplicación en su explorador predeterminado.

* Abra un explorador y escriba la dirección URL de la aplicación web. Por ejemplo:

  `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-your-web-app-and-republish"></a>Actualizar la aplicación web y volver a publicarla

Después de realizar cambios en el código local, puede volver a publicar la aplicación web.

1.  En el **Explorador de soluciones** de Visual Studio, abra el archivo *Startup.cs*.

2.  En el método `Configure`, modifique el método `Response.WriteAsync` para que aparezca del siguiente modo:

    ```aspx-cs
    await context.Response.WriteAsync("Hello World! Deploy to Azure.");
    ```
3.  Guarde los cambios en *Startup.cs*.

4.  En el **Explorador de soluciones**, haga clic con el botón derecho en **Solución 'SampleWebAppDemo'** y seleccione **Confirmar**. Aparecerá **Team Explorer**.

5.  Escriba un mensaje de confirmación, como el siguiente:

    ```none
    Update #2
    ```

6.  Presione el botón **Confirmar** para confirmar los cambios del proyecto.

7.  Seleccione **Inicio** > **Sincronizar** > **Acciones** > **Inserción**.

>[!NOTE]
>Como alternativa, puede insertar los cambios desde la **ventana de comandos**. Para ello, abra la **ventana de comandos**, vaya al directorio del proyecto y escriba un comando git. Por ejemplo:
>
>`git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Ver la aplicación web actualizada en Azure

Para ver la aplicación web actualizada, seleccione **Examinar** en la hoja de la aplicación web de Azure Portal o abra un explorador y escriba la dirección URL de la aplicación web. Por ejemplo:

   `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Recursos adicionales

* [Publicación e implementación](index.md)

* [Proyecto Kudu](https://github.com/projectkudu/kudu/wiki)
