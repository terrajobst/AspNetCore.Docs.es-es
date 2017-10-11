---
title: "Publicar una aplicación de ASP.NET Core en Azure con Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/05/2017
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: df22852d2daddb2a3faef8404d0d250a6a1697a5
ms.sourcegitcommit: e987c950caae7af9c4ece8a82228caa364e0a5df
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/05/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio

De [Rick Anderson](https://twitter.com/RickAndMSFT), [Cesar Blum Silveira](https://github.com/cesarbs) y [Rachel Appel](https://twitter.com/rachelappel)

## <a name="set-up"></a>Configurar

* Abra una [cuenta gratuita de Azure](https://aka.ms/K5y5yh) si no tiene una. 

## <a name="create-a-web-app"></a>Creación de una aplicación web

En la página de inicio de Visual Studio, seleccione **Archivo > Nuevo > Proyecto...**.

![menú Archivo](publish-to-azure-webapp-using-vs/_static/file_new_project.png)

Rellene el cuadro de diálogo **Nuevo proyecto**:

* En el panel izquierdo, seleccione **.NET Core**.

* En el panel central, seleccione **Aplicación web de ASP.NET Core**.

* Seleccione **Aceptar**.

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core**, haga lo siguiente:

* Seleccione **Aplicación web**.

* Seleccione **Cambiar autenticación**.

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj_2.png)

Se mostrará el cuadro de diálogo **Cambiar autenticación**. 

* Seleccione **Cuentas de usuario individuales**.

* Seleccione **Aceptar** para volver a la **nueva aplicación web de ASP.NET Core** y vuelva a seleccionar **Aceptar**.

![Cuadro de diálogo de autenticación web de ASP.NET Core](publish-to-azure-webapp-using-vs/_static/new_prj_auth.png) 

Visual Studio crea la solución.

## <a name="run-the-app-locally"></a>Probar la aplicación localmente

* Elija **Depurar** y **Iniciar sin depurar** para ejecutar la aplicación localmente.

* Haga clic en los vínculos **Acerca de** y **Contacto** para comprobar que la aplicación web funcione.

![Aplicación web abierta en Microsoft Edge en host local](publish-to-azure-webapp-using-vs/_static/show.png)

* Seleccione **Registrar** y registre a un usuario nuevo. Puede usar una dirección de correo electrónico ficticia. Al enviar la información, la página mostrará el error siguiente:

    *"Error interno del servidor: Error en una operación de base de datos al procesar la solicitud. Excepción de SQL: No se puede abrir la base de datos. La aplicación de las migraciones existentes para el contexto de base de datos de la aplicación puede solucionar este problema".*

* Seleccione **Aplicar migraciones** y, una vez actualizada la página, vuelva a cargarla.

![Error interno del servidor: Error en una operación de base de datos al procesar la solicitud. Excepción de SQL: No se puede abrir la base de datos. La aplicación de las migraciones existentes para el contexto de base de datos de la aplicación puede solucionar este problema.](publish-to-azure-webapp-using-vs/_static/mig.png)

La aplicación muestra el correo electrónico usado para registrar al nuevo usuario y el vínculo **Cerrar sesión**.

![Aplicación web abierta en Microsoft Edge. El vínculo Registrar se sustituye por el texto Hello email@domain.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Implementación de la aplicación en Azure

Cierre la página web, vuelva a Visual Studio y seleccione **Detener depuración** en el menú **Depurar**.

Desde el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Publicar...**

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

En el cuadro de diálogo **Publicar**, seleccione **Microsoft Azure App Service** y haga clic en **Publicar**.

![Cuadro de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

* Asigne un nombre único a la aplicación. 

* Seleccionar una suscripción.

* Seleccione **Nuevo...** junto al grupo de recursos y escriba un nombre para el nuevo grupo de recursos.

* Seleccione **Nuevo...** junto al plan de App Service y seleccione una ubicación cercana. Puede conservar el nombre que se genera de forma predeterminada.

![Cuadro de diálogo App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

* Seleccione la pestaña **Servicios** para crear una base de datos.

* Seleccione el icono verde **+** para crear una instancia de SQL Database.

![Crear una base de datos de SQL Database](publish-to-azure-webapp-using-vs/_static/sql.png)

* Seleccione **Nuevo...** en el cuadro de diálogo **Configurar SQL Database** para crear una base de datos.

![Crear una base de datos y un servidor de SQL Database](publish-to-azure-webapp-using-vs/_static/conf.png)

Se mostrará el cuadro de diálogo **Configurar SQL Server**.

* Escriba un nombre de usuario y una contraseña de administrador y seleccione **Aceptar**. No olvide el nombre de usuario y la contraseña que ha creado en este paso. Puede conservar el **nombre de servidor** predeterminado. 

* Escriba el nombre de la cadena de conexión y el de la base de datos.

> [!NOTE]
> No se permite "admin" como nombre de usuario de administrador.

![Cuadro de diálogo Configurar SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

* Seleccione **Aceptar**.

Visual Studio volverá al cuadro de diálogo **Crear un servicio de App Service**.

* Seleccione **Crear** en el cuadro de diálogo **Crear un servicio de App Service**.

![Cuadro de diálogo Configurar base de datos SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* Haga clic en el vínculo **Configuración** del cuadro de diálogo **Publicar**.

![Cuadro de diálogo Publicar: panel Conexión](publish-to-azure-webapp-using-vs/_static/pubc.png)

En la página **Configuración** del cuadro de diálogo **Publicar**, haga lo siguiente:

  * Expanda **Bases de datos** y active **Usar esta cadena de conexión en tiempo de ejecución**.

  * Expanda **Migraciones de Entity Framework** y active **Aplicar esta migración al publicar**.

* Seleccione **Guardar**. Visual Studio volverá al cuadro de diálogo **Publicar**. 

![Cuadro de diálogo Publicar: panel Configuración](publish-to-azure-webapp-using-vs/_static/pubs.png)

Haga clic en **Publicar**. Visual Studio publica la aplicación en Azure e inicia Cloud App en el explorador.

### <a name="test-your-app-in-azure"></a>Prueba de la aplicación en Azure

* Pruebe los vínculos **Acerca de** y **Contacto**.

* Registre un nuevo usuario.

![Aplicación web abierta en Microsoft Edge en Azure App Service](publish-to-azure-webapp-using-vs/_static/register.png)

### <a name="update-the-app"></a>Actualización de la aplicación

* Edite la página de Razor *Pages/About.cshtml* y modifique su contenido. Por ejemplo, puede editar el párrafo para que incluya el mensaje "¡Hola, ASP.NET Core!":

    [!code-html[About](publish-to-azure-webapp-using-vs/sample/about.cshtml?highlight=9&range=1-9)]

* Haga clic con el botón derecho sobre el proyecto y vuelva a seleccionar **Publicar...**.

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

* Una vez publicada la aplicación, compruebe que los cambios realizados estén disponibles en Azure.

![Comprobar si se ha completado la tarea](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="clean-up"></a>Limpieza

Cuando haya terminado de probar la aplicación, vaya a [Azure Portal](https://portal.azure.com/) y elimínela.

* Seleccione **Grupos de recursos** y, luego, el grupo de recursos que haya creado.

![Azure Portal: Grupos de recursos en el menú lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* En la página **Grupos de recursos**, seleccione **Eliminar**.

![Azure Portal: página Grupos de recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Escriba el nombre del grupo de recursos y seleccione **Eliminar**. La aplicación y todos los demás recursos que ha creado en este tutorial se han eliminado de Azure.

### <a name="next-steps"></a>Pasos siguientes

* [Implementación continua en Azure con Visual Studio y Git](../publishing/azure-continuous-deployment.md)