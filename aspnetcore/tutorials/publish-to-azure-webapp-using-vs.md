---
title: "Publicar una aplicación de ASP.NET Core en Azure con Visual Studio"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 78571e4a-a143-452d-9cf2-0860f85972e6
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/publish-to-azure-webapp-using-vs
ms.openlocfilehash: c4df5f4551760fbc4ed4b11362d249b24f186e00
ms.sourcegitcommit: 8f5277871eff86134ebf68d3737196cfd4a62c2c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/13/2017
---
# <a name="publish-an-aspnet-core-web-app-to-azure-app-service-using-visual-studio"></a>Publicar una aplicación web de ASP.NET Core en Azure App Service con Visual Studio

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Cesar Blum Silveira](https://github.com/cesarbs)

## <a name="set-up-the-development-environment"></a>Configuración del entorno de desarrollo

* Instale la versión más reciente de [Azure SDK para Visual Studio](https://www.visualstudio.com/features/azure-tools-vs). El SDK instala Visual Studio si aún no lo tiene.

> [!NOTE]
> La instalación del SDK puede tardar más de 30 minutos si el equipo no tiene muchas de las dependencias.

* Instale [.NET Core + las herramientas de Visual Studio](http://go.microsoft.com/fwlink/?LinkID=798306)

* Compruebe la [cuenta de Azure](https://portal.azure.com/). Puede [abrir una cuenta de Azure gratuita](https://azure.microsoft.com/pricing/free-trial/) o [activar las ventajas de suscriptor de Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/).

## <a name="create-a-web-app"></a>Creación de una aplicación web

En la página de inicio de Visual Studio, pulse en **Nuevo proyecto...**

![Página de inicio](publish-to-azure-webapp-using-vs/_static/new_project.png)

También puede usar los menús para crear un nuevo proyecto. Pulse en **Archivo > Nuevo > Proyecto...**

![menú Archivo](publish-to-azure-webapp-using-vs/_static/alt_new_project.png)

Rellene el cuadro de diálogo **Nuevo proyecto**:

* En el panel izquierdo, pulse en **Web**.

* En el panel central, pulse en **Aplicación web de ASP.NET Core (.NET Core)**.

* Pulse en **Aceptar**.

![Cuadro de diálogo Nuevo proyecto](publish-to-azure-webapp-using-vs/_static/new_prj.png)

En el cuadro de diálogo **Nueva aplicación web de ASP.NET Core (.NET Core)**:

* Pulse en **Aplicación web**.

* Compruebe que **Autenticación** está establecido en **Cuentas de usuario individuales**.

* Compruebe que **Host en la nube** **no** está activado.

* Pulse en **Aceptar**.

![Cuadro de diálogo Nueva aplicación web de ASP.NET Core (.NET Core)](publish-to-azure-webapp-using-vs/_static/noath.png)

## <a name="test-the-app-locally"></a>Prueba de la aplicación en local

* Presione **Ctrl-F5** para ejecutar la aplicación en local.

* Pulse en los vínculos **Acerca de** y **Contacto**. Según el tamaño del dispositivo, es posible que tenga que pulsar en el icono de navegación para que se muestren los vínculos.

![Aplicación web abierta en Microsoft Edge en host local](publish-to-azure-webapp-using-vs/_static/show.png)

* Pulse en **Registrar** y registre un nuevo usuario. Puede usar una dirección de correo electrónico ficticia. Al enviar, aparece el siguiente error:

![Error interno del servidor: Error en una operación de base de datos al procesar la solicitud. Excepción de SQL: No se puede abrir la base de datos. La aplicación de las migraciones existentes para el contexto de base de datos de la aplicación puede solucionar este problema.](publish-to-azure-webapp-using-vs/_static/mig.png)

Puede corregir el problema de dos maneras distintas:

* Pulse en **Aplicar migraciones** y, una vez actualizada la página, actualice la página; o

* Ejecute lo siguiente desde un símbolo del sistema en el directorio del proyecto:

  <!-- literal_block {"ids": [], "xml:space": "preserve"} -->

  ```
  dotnet ef database update
     ```

La aplicación muestra el correo electrónico usado para registrar al nuevo usuario y un vínculo **Cerrar sesión**.

![Aplicación web abierta en Microsoft Edge. El vínculo Registrar se sustituye por el texto Hello abc@example.com!](publish-to-azure-webapp-using-vs/_static/hello.png)

## <a name="deploy-the-app-to-azure"></a>Implementación de la aplicación en Azure

Confirme que la aplicación publicada para la implementación no se está ejecutando. Los archivos de la carpeta *publish* quedan bloqueados mientras se ejecuta la aplicación. No se puede llevar a cabo la implementación porque no se pueden copiar los archivos bloqueados.

Desde el Explorador de soluciones, haga clic con el botón derecho en el proyecto y seleccione **Publicar...**

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

En el cuadro de diálogo **Publicar**, pulse en **Microsoft Azure App Service**.

![Cuadro de diálogo Publicar](publish-to-azure-webapp-using-vs/_static/maas1.png)

Pulse en **Nuevo...** para crear un nuevo grupo de recursos. Al crear un grupo de recursos nuevo, le será más fácil eliminar todos los recursos de Azure que se creen en este tutorial.

![Cuadro de diálogo App Service](publish-to-azure-webapp-using-vs/_static/newrg1.png)

Cree un nuevo grupo de recursos y un plan de App Service:

* Pulse en **Nuevo...** junto al grupo de recursos y escriba un nombre para el nuevo grupo de recursos.

* Pulse en **Nuevo...** junto al plan de App Service y seleccione una ubicación cercana. Puede conservar el nombre generado predeterminado.

* Pulse en **Explorar servicios adicionales de Azure** para crear una nueva base de datos.

![Cuadro de diálogo Nuevo grupo de recursos: panel Hospedaje](publish-to-azure-webapp-using-vs/_static/cas.png)

* Pulse en el icono verde **+** para crear una nueva instancia de SQL Database.

![Cuadro de diálogo Nuevo grupo de recursos: panel Servicios](publish-to-azure-webapp-using-vs/_static/sql.png)

* Pulse en **Nuevo...** en el cuadro de diálogo **Configurar base de datos SQL** para crear un nuevo servidor de base de datos.

![Cuadro de diálogo Configurar base de datos SQL](publish-to-azure-webapp-using-vs/_static/conf.png)

* Escriba un nombre de usuario y una contraseña de administrador y luego pulse en **Aceptar**. No olvide el nombre de usuario y la contraseña que ha creado en este paso. Puede conservar el **nombre de servidor** predeterminado.

![Cuadro de diálogo Configurar SQL Server](publish-to-azure-webapp-using-vs/_static/conf_servername.png)

> [!NOTE]
> No se permite "admin" como nombre de usuario de administrador.

* Pulse en **Aceptar** en el cuadro de diálogo **Configurar base de datos SQL**.

![Cuadro de diálogo Configurar base de datos SQL](publish-to-azure-webapp-using-vs/_static/conf_final.png)

* Pulse en **Crear** en el cuadro de diálogo **Crear servicio de aplicaciones**.

![Cuadro de diálogo Crear servicio de aplicaciones](publish-to-azure-webapp-using-vs/_static/create_as.png)

* Pulse en **Siguiente** en el cuadro de diálogo **Publicar**.

![Cuadro de diálogo Publicar: panel Conexión](publish-to-azure-webapp-using-vs/_static/pubc.png)

* En la fase **Configuración** del cuadro de diálogo **Publicar**:

  * Expanda **Bases de datos** y active **Usar esta cadena de conexión en tiempo de ejecución**.

  * Expanda **Migraciones de Entity Framework** y active **Aplicar esta migración al publicar**.

* Pulse en **Publicar** y espere hasta que Visual Studio termine de publicar la aplicación.

![Cuadro de diálogo Publicar: panel Configuración](publish-to-azure-webapp-using-vs/_static/pubs.png)

Visual Studio publica la aplicación en Azure e inicia Cloud App en el explorador.

### <a name="test-your-app-in-azure"></a>Prueba de la aplicación en Azure

* Pruebe los vínculos **Acerca de** y **Contacto**.

* Registre un nuevo usuario.

![Aplicación web abierta en Microsoft Edge en Azure App Service](publish-to-azure-webapp-using-vs/_static/final.png)

### <a name="update-the-app"></a>Actualización de la aplicación

* Edite el archivo de vista `Views/Home/About.cshtml` de Razor y cambie su contenido. Por ejemplo:

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "html", "highlight_args": {"hl_lines": [7]}} -->

```html
@{
       ViewData["Title"] = "About";
   }
   <h2>@ViewData["Title"].</h2>
   <h3>@ViewData["Message"]</h3>

   <p>My updated about page.</p>
   ```

* Haga clic con el botón derecho en el proyecto y vuelva a pulsar en **Publicar...**

![Menú contextual abierto con el vínculo Publicar resaltado](publish-to-azure-webapp-using-vs/_static/pub.png)

* Una vez publicada la aplicación, compruebe que los cambios realizados están disponibles en Azure.

### <a name="clean-up"></a>Limpieza

Cuando haya terminado de probar la aplicación, vaya a [Azure Portal](https://portal.azure.com/) y elimínela.

* Seleccione **Grupos de recursos** y luego pulse en el grupo de recursos que ha creado.

![Azure Portal: Grupos de recursos en el menú lateral](publish-to-azure-webapp-using-vs/_static/portalrg.png)

* En la hoja **Grupo de recursos**, pulse en **Eliminar**.

![Azure Portal: hoja Grupos de recursos](publish-to-azure-webapp-using-vs/_static/rgd.png)

* Escriba el nombre del grupo de recursos y pulse en **Eliminar**. La aplicación y todos los demás recursos que ha creado en este tutorial se han eliminado de Azure.

### <a name="next-steps"></a>Pasos siguientes

* [Introducción a ASP.NET Core MVC y Visual Studio](first-mvc-app/start-mvc.md)

* [Introducción a ASP.NET Core](../index.md)

* [Aspectos básicos](../fundamentals/index.md)
