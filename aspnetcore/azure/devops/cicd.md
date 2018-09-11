---
title: DevOps con ASP.NET Core y Azure | Integración e implementación continuas
author: CamSoper
description: Una guía que proporciona guías de un extremo a otro sobre cómo crear una canalización de DevOps para una aplicación ASP.NET Core hospedada en Azure.
ms.author: scaddie
ms.date: 08/17/2018
uid: azure/devops/cicd
ms.openlocfilehash: 0bfe1545da4c0778055d7c81c1588d3267d2e711
ms.sourcegitcommit: 57eccdea7d89a62989272f71aad655465f1c600a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/10/2018
ms.locfileid: "44340113"
---
# <a name="continuous-integration-and-deployment"></a>Integración e implementación continuas

En el capítulo anterior, ha creado un repositorio de Git local para la aplicación de lector de fuentes Simple. En este capítulo, podrá publicar ese código en un repositorio de GitHub y crear una canalización de DevOps de Azure Services mediante canalizaciones de Azure. La canalización permite compilaciones continuas y las implementaciones de la aplicación. Cualquier confirmación en el repositorio de GitHub desencadena una compilación y una implementación de la ranura de ensayo de la aplicación Web de Azure.

En esta sección, deberá completar las tareas siguientes:

* Publicar el código de la aplicación en GitHub
* Desconectar de la implementación de Git local
* Creación de una organización de Azure DevOps
* Crear un proyecto de equipo en servicios de Azure DevOps
* Crear una definición de compilación
* Crear una canalización de versiones
* Confirmar cambios en GitHub e implementar automáticamente en Azure
* Examine la canalización de canalizaciones de Azure

## <a name="publish-the-apps-code-to-github"></a>Publicar el código de la aplicación en GitHub

1. Abra una ventana del explorador y vaya a `https://github.com`.
1. Haga clic en el **+** desplegable en el encabezado y seleccione **nuevo repositorio**:

    ![Opción nuevo repositorio de GitHub](media/cicd/github-new-repo.png)

1. Seleccione su cuenta en el **propietario** lista desplegable y escriba *lectura de fuentes simple* en el **nombre del repositorio** cuadro de texto.
1. Haga clic en el **crear repositorio** botón.
1. Abra el shell de comandos del equipo local. Navegue hasta el directorio en el que el *lectura de fuentes simple* se almacena el repositorio de Git.
1. Cambiar el nombre existente *origen* remoto a *ascendente*. Ejecute el siguiente comando:
    ```console
    git remote rename origin upstream
    ```
1. Agregue un nuevo *origen* remoto que apunta a la copia del repositorio en GitHub. Ejecute el siguiente comando:
    ```console
    git remote add origin https://github.com/<GitHub_username>/simple-feed-reader/
    ```
1. Publicar repositorio Git local en el repositorio de GitHub recién creado. Ejecute el siguiente comando:
    ```console
    git push -u origin master
    ```
1. Abra una ventana del explorador y vaya a `https://github.com/<GitHub_username>/simple-feed-reader/`. Validar que el código aparece en el repositorio de GitHub.

## <a name="disconnect-local-git-deployment"></a>Desconectar de la implementación de Git local

Quite la implementación de Git local con los pasos siguientes. Las canalizaciones de Azure (un servicio de Azure DevOps) tanto reemplaza y amplía dicha funcionalidad.

1. Abra el [portal de Azure](https://portal.azure.com/)y navegue hasta el *de almacenamiento provisional (mywebapp\<unique_number\>/ensayo)* Web App. La aplicación Web se pueden ubicar rápidamente escribiendo *ensayo* en el cuadro de búsqueda del portal:

    ![término de búsqueda de la aplicación Web de ensayo](media/cicd/portal-search-box.png)

1. Haga clic en **opciones de implementación**. Aparece un nuevo panel. Haga clic en **desconexión** para quitar la configuración de control de código fuente de Git local que se agregó en el capítulo anterior. Confirmar la operación de eliminación, haga clic en el **Sí** botón.
1. Navegue hasta la *mywebapp < unique_number >* App Service. Como recordatorio, cuadro de búsqueda del portal puede utilizarse para localizar rápidamente el servicio de aplicación.
1. Haga clic en **opciones de implementación**. Aparece un nuevo panel. Haga clic en **desconexión** para quitar la configuración de control de código fuente de Git local que se agregó en el capítulo anterior. Confirmar la operación de eliminación, haga clic en el **Sí** botón.

## <a name="create-an-azure-devops-organization"></a>Creación de una organización de Azure DevOps

1. Abra un explorador y navegue hasta la [página de creación de organización de Azure DevOps](https://go.microsoft.com/fwlink/?LinkId=307137).
1. Escriba un nombre único en la **elegir un nombre fácil de recordar** textbox para formar la dirección URL para tener acceso a su organización de DevOps de Azure.
1. Seleccione el **Git** botón de radio, puesto que el código se hospeda en un repositorio de GitHub.
1. Haga clic en el botón **Continuar**. Tras una breve espera, una cuenta y un proyecto de equipo, nombre *MyFirstProject*, se crean.

    ![Página de creación de organización de Azure DevOps](media/cicd/vsts-account-creation.png)

1. Abra el correo electrónico de confirmación que indica que la organización de DevOps de Azure y el proyecto están listos para su uso. Haga clic en el **comience su proyecto** botón:

    ![Botón de su proyecto inicial](media/cicd/vsts-start-project.png)

1. Se abre un explorador para  *\<account_name\>. visualstudio.com*. Haga clic en el *MyFirstProject* vínculo para empezar a configurar la canalización de DevOps del proyecto.

## <a name="configure-the-azure-pipelines-pipeline"></a>Configurar la canalización de canalizaciones de Azure

Hay tres pasos diferentes para completar. Completando los pasos descritos en los resultados de tres secciones siguientes en una canalización de DevOps operativa.

### <a name="grant-azure-devops-access-to-the-github-repository"></a>DevOps de Azure conceder acceso al repositorio de GitHub

1. Expanda el **o compilar código desde un repositorio externo** accordion. Haga clic en el **compilar el programa de instalación** botón:

    ![Botón de compilación del programa de instalación](media/cicd/vsts-setup-build.png)

1. Seleccione el **GitHub** opción desde el **seleccionar un origen de** sección:

    ![Seleccione un origen: GitHub](media/cicd/vsts-select-source.png)

1. Se necesita autorización antes de DevOps de Azure puede acceder al repositorio de GitHub. Escriba *< GitHub_username > GitHub conexión* en el **nombre de la conexión** cuadro de texto. Por ejemplo:

    ![Nombre de la conexión de GitHub](media/cicd/vsts-repo-authz.png)

1. Si está habilitada la autenticación en dos fases en su cuenta de GitHub, se requiere un token de acceso personal. En ese caso, haga clic en el **autorizar con un token de acceso personal de GitHub** vínculo. Consulte la [instrucciones oficiales de creación del token de acceso personal de GitHub](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) para obtener ayuda. Solo el *repositorio* ámbito de permisos es necesaria. En caso contrario, haga clic en el **autorizar el uso de OAuth** botón.
1. Cuando se le solicite, inicie sesión en su cuenta de GitHub. A continuación, seleccione autorizar para conceder acceso a su organización de DevOps de Azure. Si se realiza correctamente, se crea un nuevo extremo de servicio.
1. Haga clic en el botón de puntos suspensivos junto a la **repositorio** botón. Seleccione el *< GitHub_username > / lectura de fuentes simple* repositorio en la lista. Haga clic en el **seleccione** botón.
1. Seleccione el *maestro* bifurcación desde la **rama predeterminada para compilaciones manuales y programadas** lista desplegable. Haga clic en el botón **Continuar**. Aparece la página de selección de plantilla.

### <a name="create-the-build-definition"></a>Crear la definición de compilación

1. En la página de selección de plantilla, escriba *ASP.NET Core* en el cuadro de búsqueda:

    ![Búsqueda de ASP.NET Core en la página de la plantilla](media/cicd/vsts-template-selection.png)

1. Aparecen los resultados de búsqueda de la plantilla. Mantenga el mouse sobre el **ASP.NET Core** plantilla y haga clic en el **aplicar** botón.
1. El **tareas** aparecerá la pestaña de la definición de compilación. Haga clic en el **desencadenadores** ficha.
1. Compruebe el **habilitar la integración continua** cuadro. En el **filtros de rama** , confirme que la **tipo** desplegable se establece en *Include*. Establecer el **especificación de rama** lista desplegable para *maestro*.

    ![Habilitar la configuración de integración continua](media/cicd/vsts-enable-ci.png)

    Esta configuración hace que una compilación desencadenar cuando se inserta cualquier cambio en el *maestro* rama del repositorio de GitHub. La integración continua se prueba en el [confirmar los cambios en GitHub e implementar automáticamente en Azure](#commit-changes-to-github-and-automatically-deploy-to-azure) sección.

1. Haga clic en el **guardar y poner en cola** botón y seleccione el **guardar** opción:

    ![Botón Guardar](media/cicd/vsts-save-build.png)

1. Aparece el cuadro de diálogo modal siguiente:

    ![Guardar definición de compilación - cuadro de diálogo modal](media/cicd/vsts-save-modal.png)

    Use la carpeta predeterminada de *\\*y haga clic en el **guardar** botón.

### <a name="create-the-release-pipeline"></a>Crear la canalización de versiones

1. Haga clic en el **versiones** ficha del proyecto de equipo. Haga clic en el **nueva canalización** botón.

    ![Pestaña versiones - botón nueva definición](media/cicd/vsts-new-release-definition.png)

    Aparece el panel de selección de plantilla.

1. En la página de selección de plantilla, escriba *App Service* en el cuadro de búsqueda:

    ![Cuadro de búsqueda de plantilla de canalización de versión](media/cicd/vsts-release-template-search.png)

1. Aparecen los resultados de búsqueda de la plantilla. Mantenga el mouse sobre el **implementación de Azure App Service con espacio** plantilla y haga clic en el **aplicar** botón. El **canalización** aparecerá la pestaña de la canalización de versiones.

    ![Pestaña de la canalización de canalización de versiones](media/cicd/vsts-release-definition-pipeline.png)

1. Haga clic en el **agregar** situado en la **artefactos** cuadro. El **agregar artefacto** aparecerá el panel:

    ![Canalización de versión: panel de artefacto para agregar](media/cicd/vsts-release-add-artifact.png)

1. Seleccione el **compilar** icono desde la **tipo de origen** sección. Este tipo permite la vinculación de la canalización de versiones para la definición de compilación.
1. Seleccione *MyFirstProject* desde el **proyecto** lista desplegable.
1. Seleccione el nombre de la definición de compilación, *MyFirstProject ASP.NET Core-CI*, desde el **origen (definición de compilación)** lista desplegable.
1. Seleccione *más reciente* desde el **versión predeterminada** lista desplegable. Esta opción crea los artefactos producidos por la ejecución más reciente de la definición de compilación.
1. Reemplace el texto en el **alias de origen** textbox con *Drop*.
1. Haga clic en el botón **Agregar**. El **artefactos** sección se actualizará para mostrar los cambios.
1. Haga clic en el icono de rayo para permitir implementaciones continuas:

    ![Artefactos - icono de rayo de canalización de versiones](media/cicd/vsts-artifacts-lightning-bolt.png)

    Con esta opción habilitada, una implementación se produce cada vez que está disponible una nueva compilación.
1. Un **desencadenador de implementación continua** panel situado a la derecha. Haga clic en el botón de alternancia para habilitar la característica. No es necesario habilitar el **desencadenador de solicitud de incorporación de cambios**.
1. Haga clic en el **agregar** desplegable en el **Generar filtros de rama** sección. Elija la **rama predeterminada de la definición de compilación** opción. Este filtro hace que la versión desencadenar una compilación desde el repositorio de GitHub solo *maestro* rama.
1. Haga clic en el botón **Guardar**. Haga clic en el **Aceptar** botón en el cuadro **guardar** cuadro de diálogo modal.
1. Haga clic en el **entorno 1** cuadro. Un **entorno** panel situado a la derecha. Cambiar el *entorno 1* texto en el **nombre del entorno** textbox a *producción*.

   ![Canalización de versión: cuadro de texto de nombre de entorno](media/cicd/vsts-environment-name-textbox.png)

1. Haga clic en el **1 fase, 2 tareas** vincular en el **producción** cuadro:

    ![Canalización de versiones - link.png del entorno de producción](media/cicd/vsts-production-link.png)

    El **tareas** aparecerá la pestaña del entorno.
1. Haga clic en el **implementar Azure App Service para la ranura** tarea. Su configuración aparece en un panel a la derecha.
1. Seleccione la suscripción asociada con el servicio de aplicación desde el **suscripción de Azure** lista desplegable. Una vez seleccionado, haga clic en el **Authorize** botón.
1. Seleccione *Web App* desde el **tipo de aplicación** lista desplegable.
1. Seleccione *mywebapp / < unique_number / >* desde el **nombre de App service** lista desplegable.
1. Seleccione *AzureTutorial* desde el **grupo de recursos** lista desplegable.
1. Seleccione *ensayo* desde el **ranura** lista desplegable.
1. Haga clic en el botón **Guardar**.
1. Mantenga el mouse sobre el nombre de canalización de versión predeterminado. Haga clic en el icono de lápiz para editarlo. Use *MyFirstProject ASP.NET Core-CD* como el nombre.

    ![Nombre de la canalización de versión](media/cicd/vsts-release-definition-name.png)

1. Haga clic en el botón **Guardar**.

## <a name="commit-changes-to-github-and-automatically-deploy-to-azure"></a>Confirmar cambios en GitHub e implementar automáticamente en Azure

1. Abra *SimpleFeedReader.sln* en Visual Studio.
1. En el Explorador de soluciones, abra *Pages\Index.cshtml*. Cambio `<h2>Simple Feed Reader - V3</h2>` a `<h2>Simple Feed Reader - V4</h2>`.
1. Presione **Ctrl**+**MAYÚS**+**B** para compilar la aplicación.
1. Confirmar el archivo en el repositorio de GitHub. Usar el **cambios** página en Visual Studio *Team Explorer* pestaña o ejecute lo siguiente mediante el shell de comandos de la máquina local:

    ```console
    git commit -a -m "upgraded to V4"
    ```
1. Inserte el cambio el *maestro* bifurcar a la *origen* remoto del repositorio de GitHub:

    ```console
    git push origin master
    ```

    La confirmación que aparece en el repositorio de GitHub *maestro* rama:

    ![Confirmación de GitHub en la rama "master"](media/cicd/github-commit.png)

    Se desencadena la compilación, ya que está habilitada la integración continua en la definición de compilación **desencadenadores** pestaña:

    ![habilitar la integración continua](media/cicd/enable-ci.png)

1. Navegue hasta la **en cola** pestaña de la **canalizaciones de Azure** > **compilaciones** página en los servicios de Azure DevOps. La compilación en cola muestra la rama y confirmar que desencadenó la compilación:

    ![compilación en cola](media/cicd/build-queued.png)

1. Una vez que la compilación se realiza correctamente, se produce una implementación en Azure. Vaya a la aplicación en el explorador. Tenga en cuenta que el texto "V4" aparece en el encabezado:

    ![aplicación actualizada](media/cicd/updated-app-v4.png)

## <a name="examine-the-azure-pipelines-pipeline"></a>Examine la canalización de canalizaciones de Azure

### <a name="build-definition"></a>Definición de compilación

Se creó una definición de compilación con el nombre *MyFirstProject ASP.NET Core-CI*. Tras la finalización, la compilación genera un *.zip* archivo, incluidos los activos a publicarse. La canalización de versiones implementa esos recursos en Azure.

La definición de compilación **tareas** pestaña enumera los pasos individuales que se va a usar. Hay cinco tareas de compilación.

![tareas de definición de compilación](media/cicd/build-definition-tasks.png)

1. **Restaurar** &mdash; ejecuta el `dotnet restore` comando para restaurar los paquetes de NuGet de la aplicación. El paquete predeterminado es de fuente utilizado nuget.org.
1. **Compilar** &mdash; ejecuta el `dotnet build --configuration release` comando para compilar el código de la aplicación. Esto `--configuration` opción se usa para generar una versión optimizada del código, que es adecuado para su implementación en un entorno de producción. Modificar el *BuildConfiguration* variable en la definición de compilación **Variables** pestaña si, por ejemplo, se necesita una configuración de depuración.
1. **Prueba** &mdash; ejecuta el `dotnet test --configuration release --logger trx --results-directory <local_path_on_build_agent>` comando para ejecutar pruebas unitarias de la aplicación. Las pruebas unitarias se ejecutan dentro de cualquier C# proyecto coincidencia la `**/*Tests/*.csproj` patrón global. Los resultados de pruebas se guardan en un *.trx* archivo en la ubicación especificada por el `--results-directory` opción. Si se produce un error en las pruebas, la compilación se produce un error y no está implementada.

    > [!NOTE]
    > Para comprobar el trabajo de pruebas unitarias, modificar *SimpleFeedReader.Tests\Services\NewsServiceTests.cs* intencionadamente interrumpir una de las pruebas. Por ejemplo, cambiar `Assert.True(result.Count > 0);` a `Assert.False(result.Count > 0);` en el `Returns_News_Stories_Given_Valid_Uri` método. Confirme e inserte el cambio a GitHub. La compilación se desencadena y se produce un error. El estado de la canalización de compilación cambia a **no se pudo**. Revertir el cambio, confirmarlo e insertarlo de nuevo. La compilación se realiza correctamente.

1. **Publicar** &mdash; ejecuta el `dotnet publish --configuration release --output <local_path_on_build_agent>` comando para generar un *.zip* archivo con los artefactos que se va a implementarse. El `--output` opción especifica la ubicación de publicación de la *.zip* archivo. Que la ubicación se especifica pasando una [variable predefinida](https://docs.microsoft.com/vsts/pipelines/build/variables) denominado `$(build.artifactstagingdirectory)`. Esa variable se expande a una ruta de acceso local, como *c:\agent\_work\1\a*, en el agente de compilación.
1. **Publicar artefacto** &mdash; Publishes el *.zip* archivo generado por el **publicar** tarea. La tarea acepta el *.zip* ubicación como un parámetro, que es la variable predefinida del archivo `$(build.artifactstagingdirectory)`. El *.zip* archivo se publica como una carpeta denominada *drop*.

Haga clic en la definición de compilación **resumen** vínculo para ver un historial de compilaciones con la definición de:

![historial de la definición de compilación](media/cicd/build-definition-summary.png)

En la página resultante, haga clic en el vínculo correspondiente al número de compilación único:

![página de resumen de definición de compilación](media/cicd/build-definition-completed.png)

Se muestra un resumen de esta compilación concreta. Haga clic en el **artefactos** pestaña y observe el *drop* carpeta generado por la compilación se muestra:

![definición artefactos - carpeta de entrega de compilación](media/cicd/build-definition-artifacts.png)

Use la **descargar** y **explorar** vínculos para inspeccionar los artefactos publicados.

### <a name="release-pipeline"></a>Canalización de versiones

Se creó una canalización de versiones con el nombre *MyFirstProject ASP.NET Core-CD*:

![información general de la canalización de versión](media/cicd/release-definition-overview.png)

Los dos componentes principales de la canalización de versiones son el **artefactos** y **entornos**. Al hacer clic en el cuadro en el **artefactos** sección, muestra el panel siguiente:

![artefactos de la canalización de versión](media/cicd/release-definition-artifacts.png)

El **origen (definición de compilación)** valor representa la definición de compilación al que está vinculada esta canalización de versiones. El *.zip* archivo generado por una ejecución correcta de la definición de compilación se proporciona para el *producción* entorno para la implementación en Azure. Haga clic en el *1 fase, 2 tareas* vincular en el *producción* cuadro del entorno para ver las tareas de canalización de versión:

![tareas de canalización de versión](media/cicd/release-definition-tasks.png)

La canalización de versiones consta de dos tareas: *implementar Azure App Service para la ranura* y *administrar Azure App Service: intercambio de ranura*. Al hacer clic en la primera tarea, se muestra la configuración de la tarea siguiente:

![tarea de implementación de la canalización de versiones](media/cicd/release-definition-task1.png)

La suscripción de Azure, tipo de servicio, nombre de la aplicación web, grupo de recursos y ranura de implementación se definen en la tarea de implementación. El **paquete o carpeta** cuadro de texto contiene el *.zip* ruta de acceso de archivo al extraer e implementar a la *ensayo* ranura de la *mywebapp\<único _número\>*  aplicación web.

Al hacer clic en la tarea de intercambio de ranura, se muestra la configuración de la tarea siguiente:

![tarea de intercambio de ranura de canalización de versión](media/cicd/release-definition-task2.png)

La suscripción, grupo de recursos, tipo de servicio, nombre de la aplicación web y detalles de la ranura de implementación se proporcionan. El **intercambiar con producción** está activada la casilla de verificación. Por lo tanto, los bits implementan para el *ensayo* ranura entran en el entorno de producción.

## <a name="additional-reading"></a>Lecturas adicionales

* [Crear su primera canalización con canalizaciones de Azure](/azure/devops/pipelines/get-started-yaml)
* [Proyecto de compilación y .NET Core](/azure/devops/pipelines/languages/dotnet-core)
* [Implementar una aplicación web con canalizaciones de Azure](/azure/devops/pipelines/targets/webapp)
