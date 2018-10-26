---
title: DevOps con ASP.NET Core y Azure | Implementar una aplicación en App Service
author: CamSoper
description: Una guía que proporciona guías de un extremo a otro sobre cómo crear una canalización de DevOps para una aplicación ASP.NET Core hospedada en Azure.
ms.author: casoper
ms.custom: mvc
ms.date: 10/24/2018
uid: azure/devops/deploy-to-app-service
ms.openlocfilehash: 33026ed510aae63a9e580aa5d708f94aad778fca
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090942"
---
# <a name="deploy-an-app-to-app-service"></a>Implementar una aplicación en App Service

[Azure App Service](/azure/app-service/) es plataforma de hospedaje web de Azure. Implementar una aplicación web en Azure App Service puede realizarse manualmente o mediante un proceso automatizado. En esta sección de la guía se describe los métodos de implementación que se pueden desencadenar manualmente o mediante un script mediante la línea de comandos, o desencadenan manualmente con Visual Studio.

En esta sección, podrá realizar las tareas siguientes:

* Descargar y compilar la aplicación de ejemplo.
* Crear una aplicación Web de Azure App Service mediante Azure Cloud Shell.
* Implementar la aplicación de ejemplo en Azure con Git.
* Implementar un cambio en la aplicación mediante Visual Studio.
* Agregar una ranura de ensayo a la aplicación web.
* Implementar una actualización en la ranura de ensayo.
* Intercambiar las ranuras de ensayo y producción.

## <a name="download-and-test-the-app"></a>Descargar y probar la aplicación

La aplicación se usa en esta guía es una aplicación de ASP.NET Core pregenerada [Simple lector de fuentes](https://github.com/Azure-Samples/simple-feed-reader/). Es una aplicación de páginas de Razor que usa el `Microsoft.SyndicationFeed.ReaderWriter` API para recuperar una fuente RSS/Atom y mostrar los elementos de noticias de una lista.

No dude en revisar el código, pero es importante entender que no hay nada especial acerca de esta aplicación. Es simplemente una sencilla aplicación de ASP.NET Core con fines ilustrativos.

Desde un shell de comandos, descargar el código, compile el proyecto y ejecútelo como se indica a continuación.

> *Nota: Los usuarios de Linux/macOS deben realizar cambios correspondientes de las rutas de acceso, por ejemplo, con barra diagonal (`/`) en lugar de barra diagonal inversa (`\`).*

1. Clone el código en una carpeta en el equipo local.

    ```console
    git clone https://github.com/Azure-Samples/simple-feed-reader/
    ```

2. Cambiar la carpeta de trabajo para el *lectura de fuentes simple* carpeta que se creó.

    ```console
    cd .\simple-feed-reader\SimpleFeedReader
    ```

3. Restaure los paquetes y compile la solución.

    ```console
    dotnet build
    ```

4. Ejecute la aplicación.

    ```console
    dotnet run
    ```

    ![El comando dotnet run es correcto](./media/deploying-to-app-service/dotnet-run.png)

5. Abra un explorador y vaya a `http://localhost:5000`. La aplicación le permite escribir o pegar una dirección URL de fuente de distribución y ver una lista de elementos de noticias.

     ![La aplicación muestra el contenido de una fuente RSS](./media/deploying-to-app-service/app-in-browser.png)

6. Cuando esté satisfecho la aplicación funciona correctamente, cierra presionando **Ctrl**+**C** en el shell de comandos.

## <a name="create-the-azure-app-service-web-app"></a>Crear la aplicación Web de Azure App Service

Para implementar la aplicación, debe crear un servicio de aplicaciones [Web App](/azure/app-service/app-service-web-overview). Tras la creación de la aplicación Web, deberá implementar en él desde el equipo local mediante Git.

1. Inicie sesión en el [Azure Cloud Shell](https://shell.azure.com/bash). Nota: Al iniciar sesión por primera vez, Cloud Shell le insta a crear una cuenta de almacenamiento para archivos de configuración. Acepte los valores predeterminados o proporcione un nombre único.

2. Usar Cloud Shell para conocer los pasos siguientes.

    a. Declare una variable para almacenar el nombre de la aplicación web. El nombre debe ser único para su uso en la dirección URL predeterminada. Mediante el `$RANDOM` función Bash para construir el nombre garantiza la exclusividad y da como resultado el formato `webappname99999`.

    ```console
    webappname=mywebapp$RANDOM
    ```

    b. Cree un grupo de recursos. Grupos de recursos proporcionan un medio para agregar los recursos de Azure pueden administrarse como un grupo.

    ```azure-cli
    az group create --location centralus --name AzureTutorial
    ```

    El `az` comando invoca el [CLI de Azure](/cli/azure/). Se puede ejecutar la CLI localmente, pero usarlo en Cloud Shell permite ahorrar tiempo y la configuración.

    c. Crear un plan de App Service en el nivel S1. Un plan de App Service es una agrupación de aplicaciones web que comparten el mismo plan de tarifa. El nivel de S1 no está disponible, pero es obligatorio para la característica de espacios de almacenamiento provisional.

    ```azure-cli
    az appservice plan create --name $webappname --resource-group AzureTutorial --sku S1
    ```

    d. Crear el recurso de aplicación web mediante el plan de App Service en el mismo grupo de recursos.

    ```azure-cli
    az webapp create --name $webappname --resource-group AzureTutorial --plan $webappname
    ```

    e. Establecer las credenciales de implementación. Estas credenciales de implementación se aplican a todas las aplicaciones web en su suscripción. No utilice caracteres especiales en el nombre de usuario.

    ```azure-cli
    az webapp deployment user set --user-name REPLACE_WITH_USER_NAME --password REPLACE_WITH_PASSWORD
    ```

    f. Configurar la aplicación web para que acepte las implementaciones de local Git y mostrar el *dirección URL de la implementación de Git*. **Tenga en cuenta esta dirección URL para consultarla más tarde**.

    ```azure-cli
    echo Git deployment URL: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --query url --output tsv)
    ```

    g. Mostrar el *URL de aplicación web*. Vaya a esta dirección URL para ver la aplicación web en blanco. **Tenga en cuenta esta dirección URL para consultarla más tarde**.

    ```console
    echo Web app URL: http://$webappname.azurewebsites.net
    ```

3. Mediante un shell de comandos en el equipo local, vaya a la carpeta del proyecto de la aplicación web (por ejemplo, `.\simple-feed-reader\SimpleFeedReader`). Ejecute los comandos siguientes para configurar Git para insertar en la dirección URL de implementación:

    a. Agregue la dirección URL remota en el repositorio local.

    ```console
    git remote add azure-prod GIT_DEPLOYMENT_URL
    ```

    b. Insertar local *maestro* bifurcar a la *azure prod* del remoto *maestro* rama.

    ```console
    git push azure-prod master
    ```

    Se le pedirá las credenciales de implementación que creó anteriormente. Observe la salida en el shell de comandos. Azure compila la aplicación de ASP.NET Core de forma remota.

4. En un explorador, vaya a la *URL de aplicación Web* y tenga en cuenta la aplicación se ha generado e implementado. Cambios adicionales se pueden confirmados en el repositorio de Git local con `git commit`. Estos cambios se insertan en Azure a la anterior `git push` comando.

## <a name="deployment-with-visual-studio"></a>Implementación con Visual Studio

> *Nota: Esta sección solo se aplica a Windows. Los usuarios de Linux y macOS deben realizar el cambio que se describe en el paso 2 a continuación. Guarde el archivo y confirme el cambio en el repositorio local con `git commit`. Por último, inserte el cambio con `git push`, como en la primera sección.*

La aplicación ya se ha implementado desde el shell de comandos. Vamos a usar herramientas integradas de Visual Studio para implementar una actualización en la aplicación. En segundo plano, Visual Studio realiza la misma tarea, como las herramientas de línea de comandos, pero dentro de la interfaz de usuario familiar de Visual Studio.

1. Abra *SimpleFeedReader.sln* en Visual Studio.
2. En el Explorador de soluciones, abra *Pages\Index.cshtml*. Cambio `<h2>Simple Feed Reader</h2>` a `<h2>Simple Feed Reader - V2</h2>`.
3. Presione **Ctrl**+**MAYÚS**+**B** para compilar la aplicación.
4. En el Explorador de soluciones, haga doble clic en el proyecto y haga clic en **publicar**.

    ![Haga clic en, publicar](./media/deploying-to-app-service/publish.png)
5. Visual Studio puede crear un nuevo recurso de App Service, pero esta actualización se publicará a través de la implementación existente. En el **elegir un destino de publicación** cuadro de diálogo, seleccione **App Service** en la lista de la izquierda y, a continuación, seleccione **seleccionar existente**. Haga clic en **Publicar**.
6. En el **App Service** cuadro de diálogo, confirme que Microsoft o cuenta profesional que se usa para crear la suscripción de Azure se muestra en la esquina superior derecha. Si no es así, haga clic en la lista desplegable y agréguelo.
7. Confirme que la Azure correcta **suscripción** está seleccionada. Para **vista**, seleccione **grupo de recursos**. Expanda el **AzureTutorial** grupo de recursos y, a continuación, seleccione la aplicación web existente. Haga clic en **Aceptar**.

    ![Publicar el cuadro de diálogo App Service](./media/deploying-to-app-service/publish-dialog.png)

Visual Studio genera e implementa la aplicación en Azure. Vaya a la dirección URL de aplicación web. Validar que el `<h2>` modificación del elemento está en funcionamiento.

![La aplicación con el título modificada](./media/deploying-to-app-service/app-v2.png)

## <a name="deployment-slots"></a>Ranuras de implementación

Ranuras de implementación admiten el almacenamiento provisional de los cambios sin afectar a la aplicación que se ejecuta en producción. Una vez que un equipo de control de calidad se valida la versión de ensayo de la aplicación, se pueden intercambiar espacios de ensayo y producción. La aplicación en ensayo se promueve a producción de esta manera. Los siguientes pasos crean una ranura de ensayo, implementación algunos cambios en él e intercambiar la ranura de ensayo a producción después de la comprobación.

1. Inicie sesión en el [Azure Cloud Shell](https://shell.azure.com/bash), si no ha iniciado sesión.
2. Creación de la ranura de ensayo.

    a. Cree una ranura de implementación con el nombre *ensayo*.

    ```azure-cli
    az webapp deployment slot create --name $webappname --resource-group AzureTutorial --slot staging
    ```

    b. Configurar el espacio de ensayo para usar la implementación de local Git y obtenga el **ensayo** dirección URL de implementación. **Tenga en cuenta esta dirección URL para consultarla más tarde**.

    ```azure-cli
    echo Git deployment URL for staging: $(az webapp deployment source config-local-git --name $webappname --resource-group AzureTutorial --slot staging --query url --output tsv)
    ```

    c. Mostrar la dirección URL de la ranura de ensayo. Vaya a la dirección URL para ver el espacio de ensayo vacío. **Tenga en cuenta esta dirección URL para consultarla más tarde**.

    ```console
    echo Staging web app URL: http://$webappname-staging.azurewebsites.net
    ```

3. En un editor de texto o Visual Studio, modifique *Pages/index.cshtml* nuevo para que el `<h2>` lee el elemento `<h2>Simple Feed Reader - V3</h2>` y guarde el archivo.

4. Confirmar el archivo en el repositorio de Git local, mediante el **cambios** página en Visual Studio *Team Explorer* ficha, o escribiendo lo siguiente mediante el shell de comandos del equipo local:

    ```console
    git commit -a -m "upgraded to V3"
    ```
5. Mediante el shell de comandos de la máquina local, agregue la URL de la implementación de ensayo como un Git remoto e insertar los cambios confirmados:

    a. Agregue la dirección URL remota para el almacenamiento provisional en el repositorio de Git local.

    ```console
    git remote add azure-staging <Git_staging_deployment_URL>
    ```

    b. Insertar local *maestro* bifurcar a la *ensayo de azure* del remoto *maestro* rama.

    ```console
    git push azure-staging master
    ```

    Espere mientras Azure crea e implementa la aplicación.

6. Para comprobar que se ha implementado V3 en la ranura de ensayo, abra dos ventanas del explorador. En una ventana, desplácese a la URL de aplicación web original. En la ventana, vaya a la URL de aplicación web provisional. La dirección URL de producción actúa V2 de la aplicación. La URL de ensayo sirve V3 de la aplicación.

    ![Comparación de las ventanas del explorador](./media/deploying-to-app-service/ready-to-swap.png)

7. En Cloud Shell, colocar la ranura de ensayo comprobado/preparado-up en producción.

    ```azure-cli
    az webapp deployment slot swap --name $webappname --resource-group AzureTutorial --slot staging
    ```

8. Compruebe que el intercambio se produjo al actualizar las ventanas del explorador de dos.

    ![Comparación de las ventanas del explorador después del intercambio](./media/deploying-to-app-service/swapped.png)

## <a name="summary"></a>Resumen

En esta sección, se completaron las siguientes tareas:

* Descargado y compilado la aplicación de ejemplo.
* Crea una aplicación Web de Azure App Service mediante Azure Cloud Shell.
* Implementar la aplicación de ejemplo en Azure mediante Git.
* Implementar un cambio en la aplicación mediante Visual Studio.
* Agrega una ranura de ensayo a la aplicación web.
* Implementar una actualización en la ranura de ensayo.
* Intercambiar las ranuras de ensayo y producción.

En la sección siguiente, obtendrá información sobre cómo crear una canalización de DevOps con canalizaciones de Azure.

## <a name="additional-reading"></a>Lecturas adicionales

* [Introducción a Web Apps](/azure/app-service/app-service-web-overview)
* [Compilar una aplicación web de .NET Core y SQL Database en Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [Configurar credenciales de implementación para Azure App Service](/azure/app-service/app-service-deployment-credentials)
* [Configurar entornos de ensayo en Azure App Service](/azure/app-service/web-sites-staged-publishing)
