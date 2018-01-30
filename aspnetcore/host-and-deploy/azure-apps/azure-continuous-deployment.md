---
title: "Implementación continua en Azure con Visual Studio y Git"
author: rick-anderson
description: "Obtenga información sobre cómo crear una aplicación web de ASP.NET Core con Visual Studio e implementarla en Azure App Service con Git para una implementación continua."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/azure-apps/azure-continuous-deployment
ms.openlocfilehash: 78f4eed188323f2f43fafbb69d3fca9b59129ad2
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="continuous-deployment-to-azure-for-aspnet-core-with-visual-studio-and-git"></a>Implementación continua para Azure para ASP.NET Core con Visual Studio y Git

Por [Erik Reitan](https://github.com/Erikre)

Este tutorial muestra cómo crear una aplicación web de ASP.NET Core con Visual Studio e implementarla desde Visual Studio en el servicio de aplicaciones de Azure mediante la implementación continua.

Vea también [Use VSTS to Build and Publish to an Azure Web App with Continuous Deployment](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic) (Usar VSTS para crear y publicar una aplicación web de Azure con la implementación continua), donde se muestra cómo configurar un flujo de trabajo de una entrega continua (CD) para [Azure App Service](/azure/app-service/app-service-web-overview) con Visual Studio Team Services. La entrega continua Azure en Team Services simplifica la configuración una canalización de implementación sólida para publicar actualizaciones para las aplicaciones hospedadas en el servicio de aplicación de Azure. La canalización se puede configurar desde el portal de Azure para crear, ejecutar pruebas, implementar en una ranura de ensayo y, a continuación, implementarse en producción.

> [!NOTE]
> Para completar este tutorial, se requiere una cuenta de Microsoft Azure. Para obtener una cuenta, [activar las ventajas de suscriptor MSDN](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A261C142F) o [registrarse para obtener una prueba gratuita de](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="prerequisites"></a>Requisitos previos

Este tutorial se da por supuesto que está instalado el siguiente software:

* [Visual Studio](https://www.visualstudio.com)
* [.NET core SDK](https://www.microsoft.com/net/download/core) (en tiempo de ejecución y las herramientas)
* [Git](https://git-scm.com/downloads) para Windows

## <a name="create-an-aspnet-core-web-app"></a>Crear una aplicación web de ASP.NET Core

1. Inicie Visual Studio.

1. En el menú **Archivo**, seleccione **Nuevo** > **Proyecto**.

1. Seleccione la plantilla de proyecto **Aplicación web ASP.NET Core**. Aparece en **Instalados** > **Plantillas** > **Visual C#** > **.NET Core**. Dé un nombre al proyecto `SampleWebAppDemo`. Seleccione la opción **Crear nuevo repositorio de Git** y haga clic en **Aceptar**.

   ![Cuadro de diálogo Nuevo proyecto](azure-continuous-deployment/_static/01-new-project.png)

1. En el cuadro de diálogo **Nuevo proyecto ASP.NET Core**, seleccione la plantilla **Vacía** de ASP.NET Core y haga clic en **Aceptar**.

   ![Cuadro de diálogo Nuevo proyecto ASP.NET](azure-continuous-deployment/_static/02-web-site-template.png)

> [!NOTE]
> La versión más reciente de .NET Core es 2.0.

### <a name="running-the-web-app-locally"></a>Ejecutar la aplicación web de forma local

1. Cuando Visual Studio haya acabado de crear la aplicación, ejecútela seleccionando **Depurar** > **Iniciar depuración**. Como alternativa, presione **F5**.

   Visual Studio y la aplicación nueva pueden tardar un poco en inicializarse. Una vez completada, el explorador muestra la aplicación en ejecución.

   ![Ventana del explorador en la que aparece la aplicación en ejecución que muestra "Hello World!"](azure-continuous-deployment/_static/04-browser-runapp.png)

1. Después de revisar la aplicación Web en ejecución, cierre el explorador y seleccione el icono "Detener depuración" en la barra de herramientas de Visual Studio para detener la aplicación.

## <a name="create-a-web-app-in-the-azure-portal"></a>Crear una aplicación web en Azure Portal

Los pasos siguientes para crear una aplicación web en el Portal de Azure:

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).

1. Seleccione **NEW** en la parte superior izquierda de la interfaz del portal.

1. Seleccione **Web y móvil** > **aplicación Web**.

   ![Microsoft Azure Portal: Botón Nuevo: Web y móvil en Marketplace: Botón Aplicación web en Aplicaciones destacadas](azure-continuous-deployment/_static/05-azure-newwebapp.png)

1. En la hoja **Aplicación web**, escriba un valor único para el **nombre del servicio de aplicaciones**.

   ![Hoja Aplicación web](azure-continuous-deployment/_static/06-azure-newappblade.png)

   > [!NOTE]
   > El **nombre servicio de aplicaciones** nombre debe ser único. El portal aplica esta regla cuando se proporciona el nombre. Si proporciona un valor diferente, sustituya ese valor por cada aparición de **SampleWebAppDemo** en este tutorial.

   También en la hoja **Aplicación web**, seleccione un plan o ubicación existente de App Service o bien cree uno. Si crea un nuevo plan, seleccione el nivel de precios, la ubicación y otras opciones. Para obtener más información sobre los planes de servicio de aplicaciones, consulte [información general detallada de planes de servicio de aplicaciones de Azure](/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview).

1. Seleccione **Crear**. Aprovisionar Azure e iniciar la aplicación web.

   ![Azure Portal: Hoja de Essentials SampleWebAppDemo01](azure-continuous-deployment/_static/07-azure-webappblade.png)

## <a name="enable-git-publishing-for-the-new-web-app"></a>Habilitar la publicación de Git para la nueva aplicación web

GIT es un sistema de control de versiones distribuidas que puede usarse para implementar una aplicación web de servicio de aplicaciones de Azure. Código de la aplicación Web se almacena en un repositorio de Git local y el código se implementa en Azure mediante la aplicación en un repositorio remoto.

1. Inicie sesión en el [Portal de Azure](https://portal.azure.com).

1. Seleccione **servicios de aplicaciones** para ver una lista de los servicios de aplicación asociados a la suscripción de Azure.

1. Seleccione la aplicación web que creó en la sección anterior de este tutorial.

1. En la hoja **Implementación**, seleccione **Opciones de implementación** > **Elegir origen** > **Repositorio de Git local**.

   ![Hoja Configuración: Hoja Origen de implementación: Hoja Elegir origen](azure-continuous-deployment/_static/deployment-options.png)

1. Seleccione **Aceptar**.

1. Si las credenciales de implementación para la publicación de una aplicación web u otra aplicación de servicio de aplicaciones todavía no ha configurado previamente, configurarlos ahora:

   * Seleccione **configuración** > **las credenciales de implementación**. El **configurar credenciales de implementación** hoja se muestra.
   * Cree un nombre de usuario y una contraseña. Guarde la contraseña para su uso posterior al configurar Git.
   * Seleccione **Guardar**.

1. En el **aplicación Web** hoja, seleccione **configuración** > **propiedades**. Se muestra la dirección URL del repositorio de Git remoto para implementar en **dirección URL de GIT**.

1. Copie el valor de **Dirección URL de Git** para usarlo más adelante en el tutorial.

   ![Azure Portal: Hoja Propiedades de la aplicación](azure-continuous-deployment/_static/09-azure-giturl.png)

## <a name="publish-the-web-app-to-azure-app-service"></a>Publicar la aplicación web en el servicio de aplicaciones de Azure

En esta sección, creará un repositorio Git local mediante Visual Studio e inserción desde ese repositorio en Azure para implementar la aplicación web. Los pasos son los siguientes:

* Agrega la opción de repositorio remoto utilizando el valor de la dirección URL de GIT, por lo que el repositorio local se puede implementar en Azure.
* Confirmar cambios en el proyecto.
* Inserte los cambios de proyecto desde el repositorio local en el repositorio remoto en Azure.

1. En el **Explorador de soluciones**, haga clic con el botón derecho en **Solución 'SampleWebAppDemo'** y seleccione **Confirmar**. El **Team Explorer** se muestra.

   ![Pestaña Team Explorer: Conexión](azure-continuous-deployment/_static/10-team-explorer.png)

1. En **Team Explorer**, seleccione **Inicio** (icono Inicio) > **Configuración** > **Configuración del repositorio**.

1. En el **controles remotos** sección de la **configuración del repositorio**, seleccione **agregar**. El **agregar remoto** se muestra el cuadro de diálogo.

1. Establezca el **nombre** del repositorio remoto en **Azure-SampleApp**.

1. Establezca el valor de **capturar** a la **dirección URL de Git** que copiado desde Azure anteriormente en este tutorial. Tenga en cuenta que esta es la dirección URL que termina en **.git**.

   ![Cuadro de diálogo Editar Azure-SampleApp remoto](azure-continuous-deployment/_static/11-add-remote.png)

   > [!NOTE]
   > Como alternativa, especifique el repositorio remoto desde el **ventana de comandos** abriendo el **ventana de comandos**, cambiar al directorio del proyecto y escriba el comando. Ejemplo:
   >
   > `git remote add Azure-SampleApp https://me@sampleapp.scm.azurewebsites.net:443/SampleApp.git`

1. Seleccione **Inicio** (icono de Inicio) > **Configuración** > **Configuración global**. Confirme que se establecen el nombre y direcciones de correo. Seleccione **actualización** si es necesario.

1. Seleccione **Inicio** > **Cambios** para volver a la vista **Cambios**.

1. Escriba un mensaje de confirmación, como **inicial Push #1** y seleccione **confirmación**. Esta acción crea un *confirmación* localmente.

   ![Pestaña Team Explorer: Conexión](azure-continuous-deployment/_static/12-initial-commit.png)

   > [!NOTE]
   > Como alternativa, guardar los cambios de la **ventana de comandos** abriendo el **ventana de comandos**, cambiar al directorio del proyecto y escriba los comandos de git. Ejemplo:
   >
   > `git add .`
   >
   > `git commit -am "Initial Push #1"`

1. Seleccione **Inicio** > **Sincronizar** > **Acciones** > **Abrir símbolo del sistema**. El símbolo del sistema se abrirá en el directorio del proyecto.

1. Escriba el siguiente comando en la ventana de comandos:

   `git push -u Azure-SampleApp master`

1. Escriba Azure **las credenciales de implementación** contraseña que creó anteriormente en Azure.

   Este comando inicia el proceso de inserción de los archivos de proyecto local a Azure. El resultado del comando anterior finaliza con el mensaje que indica que la implementación se realizó correctamente.

   ```
   remote: Finished successfully.
   remote: Running post deployment command(s)...
   remote: Deployment successful.
   To https://username@samplewebappdemo01.scm.azurewebsites.net:443/SampleWebAppDemo01.git
   * [new branch]      master -> master
   Branch master set up to track remote branch master from Azure-SampleApp.
   ```

   > [!NOTE]
   > Si se requiere la colaboración en el proyecto, considere la posibilidad de insertar en [GitHub](https://github.com) antes de insertar en Azure.
 
### <a name="verify-the-active-deployment"></a>Comprobar la implementación activa

Compruebe que la transferencia de la aplicación web desde el entorno local a Azure es correcta.

En el [Portal de Azure](https://portal.azure.com), seleccione la aplicación web. Seleccione **implementación** > **opciones de implementación**.

![Azure Portal: Hoja Configuración: Hoja Implementaciones donde se muestra una implementación correcta](azure-continuous-deployment/_static/13-verify-deployment.png)

## <a name="run-the-app-in-azure"></a>Ejecutar la aplicación en Azure

Ahora que la aplicación web se implementa en Azure, ejecute la aplicación.

Esto puede realizarse de dos maneras:

* En el Portal de Azure, localice la hoja de la aplicación web para la aplicación web. Seleccione **examinar** para ver la aplicación en el explorador predeterminado.
* Abra un explorador y escriba la dirección URL de la aplicación web. Ejemplo: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="update-the-web-app-and-republish"></a>Actualización de la aplicación web y volver a publicar

Después de realizar cambios en el código local, vuelva a publicar:

1. En el **Explorador de soluciones** de Visual Studio, abra el archivo *Startup.cs*.

1. En el método `Configure`, modifique el método `Response.WriteAsync` para que aparezca del siguiente modo:

   ```csharp
   await context.Response.WriteAsync("Hello World! Deploy to Azure.");
   ```

1. Guardar los cambios realizados en *Startup.cs*.

1. En el **Explorador de soluciones**, haga clic con el botón derecho en **Solución 'SampleWebAppDemo'** y seleccione **Confirmar**. El **Team Explorer** se muestra.

1. Escriba un mensaje de confirmación, como `Update #2`.

1. Presione el botón **Confirmar** para confirmar los cambios del proyecto.

1. Seleccione **Inicio** > **Sincronizar** > **Acciones** > **Inserción**.

> [!NOTE]
> Como alternativa, inserte los cambios desde el **ventana de comandos** abriendo el **ventana de comandos**, cambiar al directorio del proyecto y escriba un comando de git. Ejemplo:
> 
> `git push -u Azure-SampleApp master`

## <a name="view-the-updated-web-app-in-azure"></a>Ver la aplicación web actualizada en Azure

Ver la aplicación web se actualizó seleccionando **examinar** desde la hoja de la aplicación web en el Portal de Azure, o bien, abra un explorador y escriba la dirección URL de la aplicación web. Ejemplo: `http://SampleWebAppDemo.azurewebsites.net`

## <a name="additional-resources"></a>Recursos adicionales

* [Usar VSTS para generar y publicar en una aplicación Web de Azure con una implementación continua](/vsts/build-release/archive/apps/aspnet/aspnet-4-ci-cd-azure-automatic)
* [Proyecto Kudu](https://github.com/projectkudu/kudu/wiki)
