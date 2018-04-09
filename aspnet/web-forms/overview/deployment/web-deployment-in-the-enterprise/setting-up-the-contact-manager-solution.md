---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
title: Configurar la solución póngase en contacto con el administrador | Documentos de Microsoft
author: jrjlee
description: En este tema se describe cómo descargar y configurar la solución póngase en contacto con el administrador para que se ejecute localmente en una estación de trabajo de desarrollador.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 200b973c-776b-4a9b-9e82-39fda6120a52
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/setting-up-the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: e8fb24f5b2d96d864d1aa6bc0f78644773de00ab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="setting-up-the-contact-manager-solution"></a>Cómo configurar la solución póngase en contacto con el administrador
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> En este tema se describe cómo descargar y configurar la solución póngase en contacto con el administrador para que se ejecute localmente en una estación de trabajo de desarrollador.


## <a name="system-requirements"></a>Requisitos del sistema

Para ejecutar la solución póngase en contacto con el administrador local y realizar otras tareas que se describen en este tutorial, debe instalar este software en la estación de trabajo de desarrollador:

- Visual Studio 2010 Service Pack 1, Premium o Ultimate Edition
- Servicios de Internet Information Server (IIS) 7.5 Express
- SQL Server Express 2008 R2
- IIS herramienta de implementación Web (Web Deploy) 2.1 o posterior
- ASP.NET 4.0
- ASP.NET MVC 3
- .NET Framework 4
- .NET Framework 3.5 SP1

Con la excepción de Visual Studio 2010, puede descargar e instalar las versiones más recientes de todos los productos y componentes a través de la [instalador de plataforma Web](https://go.microsoft.com/?linkid=9805118).

## <a name="download-and-extract-the-solution"></a>Descargue y extraiga la solución

Puede descargar la aplicación de ejemplo póngase en contacto con el Administrador de la Galería de código de MSDN [aquí](https://code.msdn.microsoft.com/Deploying-Web-Applications-9d9093c0).

## <a name="configure-and-run-the-solution"></a>Configurar y ejecutar la solución

Para configurar y ejecutar la solución póngase en contacto con el administrador del equipo local, debe realizar estos pasos de alto nivel:

1. Si aún no tiene uno, cree una base de datos de servicios de aplicación ASP.NET local con las características de administración de pertenencia y roles habilitadas.
2. Editar cadenas de conexión en el *web.config* archivos para que apunte a la instancia local de SQL Server Express.
3. Ejecute la solución desde Visual Studio 2010.

El resto de esta sección proporciona más instrucciones sobre cómo completar cada una de estas tareas.

**Para crear la base de datos de servicios de aplicación**

1. Abra un símbolo del sistema de Visual Studio 2010. Para ello, en el **iniciar** menú, elija **todos los programas**, haga clic en **Microsoft Visual Studio 2010**, haga clic en **Visual Studio Tools**y, a continuación, Haga clic en **símbolo del sistema de Visual Studio (2010)**.
2. En el símbolo del sistema, escriba el siguiente comando y, a continuación, presione ENTRAR:

    [!code-console[Main](setting-up-the-contact-manager-solution/samples/sample1.cmd)]

    1. Use la **– C** conmutador para especificar la cadena de conexión para el servidor de base de datos.
    2. Use la **– A** conmutador para especificar las características que desea agregar a la base de datos de servicios de la aplicación. En este caso, **m** indica que desea agregar compatibilidad para el proveedor de pertenencia y **r** indica que desea agregar compatibilidad con el Administrador de roles.
    3. Use la **– d** conmutador para especificar un nombre para la base de datos de servicios de aplicación. Si se omite este parámetro, la utilidad creará una base de datos con el nombre predeterminado de **aspnetdb**.
3. Cuando la base de datos se ha creado correctamente, la línea de comandos le mostrará una confirmación.

    ![](setting-up-the-contact-manager-solution/_static/image1.png)

> [!NOTE]
> Para obtener más información sobre aspnet\_regsql utilidad, vea [herramienta de registro de servidor de SQL de ASP.NET (Aspnet\_regsql.exe)](https://msdn.microsoft.com/library/ms229862(v=vs.100).aspx).


El siguiente paso es asegurarse de que las cadenas de conexión de la solución de Contact Manager señalan a la instancia local de SQL Server Express.

**Para actualizar las cadenas de conexión**

1. Abra la solución póngase en contacto con el Administrador de Visual Studio 2010.
2. En el **el Explorador de soluciones** ventana, expanda la **ContactManager.Mvc** del proyecto y, a continuación, haga doble clic en el **Web.config** nodo.

    > [!NOTE]
    > El proyecto ContactManager.Mvc incluye dos *web.config* archivos. Debe editar el archivo de nivel de proyecto.

    ![](setting-up-the-contact-manager-solution/_static/image2.png)
3. En el **connectionStrings** elemento, compruebe que la cadena de conexión con el nombre **ApplicationServices** apunta a la base de datos de servicios de aplicación ASP.NET local.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample2.xml)]
4. En el **el Explorador de soluciones** ventana, expanda la **ContactManager.Service** del proyecto y, a continuación, haga doble clic en el **Web.config** nodo.

    ![](setting-up-the-contact-manager-solution/_static/image3.png)
5. En el **connectionStrings** elemento, en la cadena de conexión denominada **ContactManagerContext**, compruebe que la **origen de datos** propiedad se establece en la instancia local de SQL Express de servidor. No es necesario cambiar nada más en la cadena de conexión.

    [!code-xml[Main](setting-up-the-contact-manager-solution/samples/sample3.xml)]
6. Guarde todos los archivos abiertos.

Ahora estará preparado para ejecutar la solución póngase en contacto con el administrador del equipo local.

> [!NOTE]
> Si sigue estos pasos sin crear primero una base de datos de servicios de aplicación, ASP.NET creará la base de datos la primera vez que intente crear un usuario. Sin embargo, la creación manual de la base de datos ofrece muchas más control sobre el conjunto de características de servicios de aplicación que desea admitir.


**Para ejecutar la solución póngase en contacto con el administrador**

1. En Visual Studio 2010, presione F5.
2. Internet Explorer se inicia y solicita la dirección URL de la aplicación de ASP.NET MVC 3 de póngase en contacto con el administrador. De forma predeterminada, la aplicación muestra el **todos los contactos** página.

    ![](setting-up-the-contact-manager-solution/_static/image4.png)
3. Agregar algunos contactos y, a continuación, compruebe que la aplicación funciona según lo previsto.

    ![](setting-up-the-contact-manager-solution/_static/image5.png)
4. Vaya a `http://localhost:50114/Account/Register` (ajustar la dirección URL si está hospedando la aplicación en un puerto diferente). Agregar un nombre de usuario, una dirección de correo electrónico y una contraseña y comprobar que es capaz de registrar una cuenta correctamente.

    ![](setting-up-the-contact-manager-solution/_static/image6.png)
5. Vaya a `http://localhost:50114/Account/LogOn` (ajustar la dirección URL si está hospedando la aplicación en un puerto diferente). Confirma que eres puedan iniciar sesión con la cuenta que acaba de crear.

    ![](setting-up-the-contact-manager-solution/_static/image7.png)
6. Cierre Internet Explorer para detener la depuración.

## <a name="conclusion"></a>Conclusión

En este momento, la solución póngase en contacto con el administrador debe configurarse totalmente para ejecutarse en el equipo local. Puede usar la solución como una referencia cuando se trabaja a través de los otros temas en este tutorial.

El siguiente tema, [comprender el archivo de proyecto](understanding-the-project-file.md), explica cómo puede utilizar los archivos de proyecto personalizados de Microsoft Build Engine (MSBuild) dentro de la solución póngase en contacto con el administrador para controlar el proceso de implementación.

> [!div class="step-by-step"]
> [Anterior](the-contact-manager-solution.md)
> [Siguiente](understanding-the-project-file.md)
