---
uid: whitepapers/aspnet-web-deployment-content-map
title: "Implementación de Web de ASP.NET: los recursos recomendados | Documentos de Microsoft"
author: rick-anderson
description: "Este tema proporciona vínculos a documentación (publicar) ASP.NET web recursos sobre cómo implementar aplicaciones en IIS mediante el uso de Visual Studio 2010, Visual De Web..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2014
ms.topic: article
ms.assetid: 58b583cd-c4ab-47a3-8527-8c92c298c91f
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-web-deployment-content-map
msc.type: content
ms.openlocfilehash: 8bded273de1ca7b050d41ddd872d9a1aa68bb314
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-web-deployment---recommended-resources"></a>Recursos recomendados de implementación Web de ASP.NET:
====================
> Este tema proporciona vínculos a documentación (publicar) ASP.NET web recursos sobre cómo implementar aplicaciones en IIS mediante el uso de Visual Studio 2010, Visual Web Developer 2010 y versiones posteriores.
> 
> Si conoce un estupendo blog post, [stackoverflow](http://stackoverflow.com) subproceso, o cualquier otro vínculo a la que podría ser útil, [envíenos un correo electrónico](mailto:aspnetue@microsoft.com?subject=Deployment Content Map) con el vínculo.
> 
> > [!NOTE] 
> > 
> > Muchos de estos recursos describen las características de implementación que están disponibles sólo si instala una versión reciente de la [actualización de publicación de Web de Visual Studio](https://go.microsoft.com/fwlink/?LinkID=208120). Algunas de las características sólo están disponibles en Visual Studio 2012 o Visual Studio 2013.


Este tema contiene las siguientes secciones:

- [Descripción de las opciones de implementación para proyectos web](#understanding)
- [Buscar proveedores para una aplicación ASP.NET de hospedaje](#findinghosting)
- [Implementar una aplicación web de Visual Studio](#fromvs)
- [Implementar una aplicación web, cree e instale un paquete de implementación web](#package)
- [Implementar una aplicación web mediante un proceso de integración continua (CI)](#ci)
- [Uso de transformaciones de Web.config para cambiar la configuración en el archivo Web.config de destino o el archivo app.config durante la implementación](#transforms)
- [Usar parámetros de Web Deploy para cambiar la configuración de la aplicación web de destino durante la implementación](#webdeployparms)
- [Asegurarse de que una aplicación está sin conexión durante la implementación](#appoffline)
- [Implementar una base de datos o cambios a una base de datos como parte de la implementación de aplicaciones web](#databasewithweb)
- [Implementar una base de datos por separado de la implementación de aplicaciones web](#databaseseparate)
- [Implementar una aplicación web que usa la aplicación ASP.NET de servicios como la suscripción y la generación de perfiles](#aspnetmembership)
- [Precompilar para la implementación](#precompiling)
- [Implementar una aplicación web de intranet](#intranet)
- [Automatizar tareas comunes de implementación que no se automatizan de fábrica](#automating)
- [Configurar servidores web para que los desarrolladores pueden implementar aplicaciones web a ellos mediante Web Deploy](#configuringservers)
- [Configuración de servidores para un proveedor de hospedaje](#hostingprovider)
- [Solución de problemas de implementación](#troubleshooting)
- [Obtener ayuda con una pregunta específica de implementación](#gettinghelp)
- [Recursos adicionales](#additional)


<a id="understanding"></a>


## <a name="understanding-deployment-options-for-web-projects"></a>Descripción de las opciones de implementación para proyectos web

- [Información general de la implementación de Web para Visual Studio y ASP.NET](https://msdn.microsoft.com/en-us/library/dd394698.aspx) (MSDN).
- [Cómo implementar un sitio Web de Azure de Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Explica las opciones y los vínculos a los recursos para la implementación de proyectos web de sitios Web para Windows Azure, incluidos [la entrega continua](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) (automatizadas de [control de código fuente](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control.md)), así como con Visual Studio.
- [Mejoras de publicación Web de Visual Studio 2012](../visual-studio/overview/2012/visual-studio-2012-web-publishing-improvements.md) (vídeo de Scott Hanselman).
- [Información general sobre Post para la implementación de Web de VS 2010](http://vishaljoshi.blogspot.com/2009/09/overview-post-for-web-deployment-in-vs.html) (blog de Vishal Joshi). Una entrada de blog anterior, pero algunos de los recursos de Visual Studio 2010 vincula para tener información que sigue siendo pertinente para Visual Studio 2012.


<a id="findinghosting"></a>


## <a name="finding-hosting-providers-for-an-aspnet-application"></a>Buscar proveedores para una aplicación ASP.NET de hospedaje

- [Hospedaje de ASP.NET](https://asp.net/hosting)


<a id="fromvs"></a>


## <a name="deploying-a-web-application-from-visual-studio"></a>Implementar una aplicación web de Visual Studio

- [Cómo implementar un sitio Web de Azure de Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Se explican las opciones y proporciona vínculos a recursos para la implementación de proyectos web para sitios Web de Windows Azure. Incluye una sección acerca de la implementación de Visual Studio.
- [Implementación de Web ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie de tutoriales de parte de 12, se muestra cómo implementar aplicaciones web con las bases de datos de SQL Server. Para la base de datos de implementación utiliza el proveedor dbDacFx y Entity Framework Code First Migrations. También incluye información sobre [transformaciones del archivo Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md), [implementar archivos individuales](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md#specificfiles), [implementación de línea de comandos](../web-forms/overview/deployment/visual-studio-web-deployment/command-line-deployment.md), y [cómo personalizar la web de Visual Studio canalización de publicación mediante la edición de archivos de .pubxml](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md). Se aplica a todos los proyectos web ASP.NET, incluidos los formularios Web Forms, MVC y API Web).
- [Cómo: implementar un uso de un solo clic publicación de proyecto Web en Visual Studio](https://msdn.microsoft.com/en-us/library/dd465337.aspx) (información de referencia para el Asistente para publicación de Web de Visual Studio.)
- [Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). Se trata de una versión anterior de **implementación de Web de ASP.NET con Visual Studio** aparece en la parte superior de esta sección. Ahora principalmente útil para obtener información sobre cómo implementar las bases de datos de SQL Server Compact y cómo migrar de SQL Server Compact a una edición completa de SQL Server.
- [Aplicación de varios niveles .NET usando almacenamiento tablas, colas y Blobs](https://code.msdn.microsoft.com/Windows-Azure-Multi-Tier-eadceb36) (sitio de Microsoft Azure). serie de tutoriales de parte 5, se muestra cómo crear un proyecto de MVC e implementarlo en un servicio de nube de Windows Azure.


<a id="package"></a>
## <a name="deploying-a-web-application-by-creating-and-installing-a-web-deployment-package"></a>Implementar una aplicación web, cree e instale un paquete de implementación web

- [Cómo: crear un paquete de implementación Web en Visual Studio](https://msdn.microsoft.com/en-us/library/dd465323.aspx) (MSDN).
- [Cómo: instalar un paquete de implementación mediante el archivo deploy.cmd creado por Visual Studio](https://msdn.microsoft.com/en-us/library/ff356104.aspx) (MSDN).
- [Usar un paquete de Web Deploy para implementar en IIS en el cuadro de desarrollo y a un host de terceros](http://sedodream.com/2011/11/08/UsingAWebDeployPackageToDeployToIISOnTheDevBoxAndToAThirdPartyHost.aspx) (blog de Sayed Hashimi). Cómo usar el Administrador de IIS para instalar un paquete de implementación en IIS en el equipo local y en un host de la compañía es compatible con el Administrador de IIS para la administración remota.
- [Creación de una Web implementar paquetes de Visual Studio 2010](https://www.iis.net/learn/publish/using-web-deploy/building-a-web-deploy-package-from-visual-studio-2010) (sitio web IIS.NET). Se incluyen instrucciones para la instalación y la creación del paquete de línea de comandos.
- [Una vez publicar desde cualquier lugar del paquete](http://sedodream.com/2012/03/14/PackageWebUpdatedAndVideoBelow.aspx) (blog de Sayed Hashimi). Presenta un paquete de NuGet que automatiza el proceso de transformar el archivo Web.config para varios entornos de destino, por lo que puede implementar un paquete en varios servidores. Vea también el [PackageWeb vídeo](https://www.youtube.com/watch?v=-LvUJFI8CzM) por Sayed Hashimi.

Consulte también la sección siguiente.


<a id="ci"></a>


## <a name="deploying-a-web-application-using-a-continuous-integration-ci-process"></a>Implementar una aplicación web mediante un proceso de integración continua (CI)

- [Integración continua y la entrega continua (creación de aplicaciones de nube reales con Windows Azure).](../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery.md) Capítulo de libros electrónicos que incorpora la integración continua y la entrega continua.
- [Cómo implementar un sitio Web de Azure de Windows](https://docs.microsoft.com/azure/app-service-web/web-sites-deploy). Explica las opciones y los vínculos a los recursos para la implementación de proyectos web para sitios Web de Windows Azure. Incluye una sección acerca de cómo automatizar la implementación desde control de código fuente.
- [Implementar aplicaciones Web en escenarios empresariales](../web-forms/overview/deployment/deploying-web-applications-in-enterprise-scenarios/deploying-web-applications-in-enterprise-scenarios.md). serie de tutoriales de 40 partes, se muestra cómo automatizar la implementación en un proceso de integración continua con Visual Studio 2010 y Team Foundation Server 2010.
- [Dentro de Microsoft Build Engine: usar MSBuild y Team Foundation Build, Sayed Hashimi y William Bartholomew](http://msbuildbook.com). Se trata de un libro, no es un recurso de web, pero es una guía básica para aprender a configurar MSBuild para escenarios de integración continua.
- [Paquete de extensión de MSBuild](https://github.com/mikefourie/MSBuildExtensionPack). Incluye tareas de implementación.
- [Team Foundation Build personalización guía](https://aka.ms/vsarsolutions). Documentación por ALM Rangers sobre la configuración de Team Foundation Server centra en implementación web e incluye tutoriales y vídeos.
- [Transforma SlowCheetah XML desde un servidor CI](http://sedodream.com/2011/12/12/SlowCheetahXMLTransformsFromACIServer.aspx) (blog de Sayed Hashimi). Explica cómo usar SlowCheetah, complemento A Visual Studio para transformar app.config y otros archivos XML.

Vea también [asegurándose de que una aplicación está sin conexión durante la implementación](aspnet-web-deployment-content-map.md#appoffline) más adelante en esta página.


<a id="transforms"></a>


## <a name="using-webconfig-transformations-to-change-settings-in-the-destination-webconfig-file-or-appconfig-file-during-deployment"></a>Uso de transformaciones de Web.config para cambiar la configuración en el archivo Web.config de destino o el archivo app.config durante la implementación

- [Las transformaciones del archivo Web.config](../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md).
- [Sintaxis de transformación de Web.config para la implementación de proyectos Web con Visual Studio](https://msdn.microsoft.com/en-us/library/dd465326.aspx) (MSDN).
- [Web Tools 2012.2 - web.config transformaciones](https://www.youtube.com/watch?v=HdPK8mxpKEI) (vídeo de YouTube de Sayed Hashimi). Muestra cómo configurar y obtener una vista previa de las transformaciones de Web.config.
- [¿Cómo deshabilitar la transformación de Web.config?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#disable_web_config_transformation) (MSDN).
- [¿Cuándo debo utilizar Web Deploy parámetros en lugar de las transformaciones de Web.config?](https://msdn.microsoft.com/en-us/library/ee942158.aspx#web_deploy_parameters) (MSDN).
- [XDT (transformación de documentos XML) que se publicó en codeplex.com](https://blogs.msdn.com/b/webdev/archive/2013/04/23/xdt-xml-document-transform-released-on-codeplex-com.aspx) (blog de desarrollo Web de .NET y herramientas). Anuncia la disponibilidad del código fuente para el motor de transformación del archivo Web.config y se enumeran algunas herramientas que lo utilizan.
- [Sitios Web de Azure de Windows: La aplicación cadenas y trabajo de cadenas de conexión](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog de Microsoft Azure). Una alternativa a Web.config transforma si su entorno de destino es Microsoft Azure sitios Web y desea transformar `appSettings` o `connectionStrings`.


<a id="webdeployparms"></a>


## <a name="using-web-deploy-parameters-to-change-settings-in-the-destination-web-application-during-deployment"></a>Usar parámetros de Web Deploy para cambiar la configuración de la aplicación web de destino durante la implementación

- [Cómo: usar WebDeploy parámetros en un paquete de implementación Web](https://msdn.microsoft.com/en-us/library/ff398068.aspx) (MSDN).
- [MSDeploy: Cómo actualizar la configuración de la aplicación en la publicación basada en el perfil de publicación](http://sedodream.com/2013/03/02/MSDeployHowToUpdateAppSettingsOnPublishBasedOnThePublishProfile.aspx) (blog de Sayed Hashimi). Muestra cómo integrar WebDeploy parámetros en Visual Studio perfiles de publicación.
- [Implementar la parametrización de Web](https://www.iis.net/learn/publish/using-web-deploy/web-deploy-parameterization) (sitio web IIS.NET).
- [Web implementar parametrización en acción](http://vishaljoshi.blogspot.com/2010/07/web-deploy-parameterization-in-action.html) (blog de Vishal Joshi).
- [Web frente a implementar la parametrización. Transformación de Web.config](http://vishaljoshi.blogspot.com/2010/06/parameterization-vs-webconfig.html) (blog de Vishal Joshi).
- [Sitios Web de Azure de Windows: La aplicación cadenas y trabajo de cadenas de conexión](https://blogs.msdn.com/b/windowsazure/archive/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work.aspx) (blog de Microsoft Azure). Una alternativa a Web implementar parámetros si el entorno de destino es Microsoft Azure sitios Web y desea parametrizar `appSettings` o `connectionStrings`.


<a id="appoffline"></a>


## <a name="making-sure-an-application-is-off-line-during-deployment"></a>Asegurarse de que una aplicación está sin conexión durante la implementación

- [Implementación de Web ASP.NET con Visual Studio: implementar una actualización del código](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md). Vea la sección **hacer que la aplicación sin conexión durante la implementación.**
- [Desconectar una aplicación antes de la publicación](https://www.iis.net/learn/publish/deploying-application-packages/taking-an-application-offline-before-publishing) (IIS.net sitio). Explica una característica basada en Web Deploy 3.0 que automatiza el control de una aplicación\_offline.htm archivo. Esta característica no funciona con la aplicación personalizada\_offline.htm archivos.
- [Cómo aprovechar la aplicación web sin conexión durante la publicación](http://sedodream.com/2012/01/08/HowToTakeYourWebAppOfflineDuringPublishing.aspx) (blog de Sayed Hashimi). Cómo automatizar el proceso de uso de una aplicación personalizada\_offline.htm archivo.
- [Web publicar actualizaciones de aplicación sin conexión y usechecksum](https://blogs.msdn.com/b/webdev/archive/2013/10/30/web-publishing-updates-for-app-offline-and-usechecksum.aspx) (blog de desarrollo Web de Microsoft). Otra opción para automatizar el uso de la aplicación\_offline.htm archivo.
- [Web implementar 3.5 RTW](https://blogs.iis.net/msdeploy/archive/2013/07/09/webdeploy-3-5-rtw.aspx) (IIS.net sitio). Nueva característica en 3.5 de implementación Web de aplicación personalizada\_offline.htm archivos.


<a id="databasewithweb"></a>


## <a name="deploying-a-database-or-changes-to-a-database-as-part-of-web-application-deployment"></a>Implementar una base de datos o cambios a una base de datos como parte de la implementación de aplicaciones web

- [Configuración de implementación de base de datos en Visual Studio](https://msdn.microsoft.com/en-us/library/dd394698.aspx#dbdeployment) (MSDN). Información general de opciones para implementar una base de datos con un proyecto web.
- [Implementación de Web ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie de tutoriales de parte de 12, muestra la implementación de base de datos mediante el uso de proveedor dbDacFx y Entity Framework Code First Migrations.
- [Cómo: implementar un servidor Web Publicar proyecto con un solo clic en Visual Studio](https://msdn.microsoft.com/en-us/library/dd465337.aspx) (MSDN).
- [Implementar una aplicación de MVC de ASP.NET 5 segura con pertenencia, OAuth y base de datos SQL a un sitio Web de Azure de Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un tutorial largo que crea e implementa una aplicación que utiliza un único servidor de SQL Server de base de datos tanto para los datos de pertenencia y la aplicación.
- [Implementar una aplicación Web de ASP.NET con SQL Server Compact con Visual Studio](../web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-introduction-1-of-12.md). serie de tutoriales de parte de 12, se muestra cómo implementar bases de datos de SQL Server Compact y cómo migrar de SQL Server Compact a una edición completa de SQL Server.

Vea también implementar una aplicación web mediante la creación y la instalación de un paquete de implementación web y la implementación de una aplicación web mediante un proceso de integración continua (CI) anteriormente en esta página.


<a id="databaseseparate"></a>


## <a name="deploying-a-database-separately-from-web-application-deployment"></a>Implementar una base de datos por separado de la implementación de aplicaciones web

- [Herramientas de datos SQL Server](https://msdn.microsoft.com/en-us/library/hh272686(v=vs.103).aspx) (MSDN).
- [Inclusión de datos en un proyecto de base de datos de SQL Server](https://blogs.msdn.com/b/ssdt/archive/2012/02/02/including-data-in-an-sql-server-database-project.aspx) (blog del equipo de SQL Server Data Tools). Cómo implementar el esquema y los datos al implementar una base de datos.
- [Cómo implementar una base de datos en Windows Azure](https://docs.microsoft.com/azure/sql-database/sql-database-cloud-migrate) (sitio de Microsoft Azure)
- [Migrar bases de datos en Windows Azure SQL Database (anteriormente SQL Azure)](https://msdn.microsoft.com/en-us/library/windowsazure/ee730904.aspx) (MSDN).
- [Migrar una base de datos a SQL Azure mediante SSDT](https://blogs.msdn.com/b/ssdt/archive/2012/04/19/migrating-a-database-to-sql-azure-using-ssdt.aspx) (blog del equipo de SQL Server Data Tools).
- [Migrar aplicaciones centradas en datos a Windows Azure](https://msdn.microsoft.com/en-us/library/jj156154.aspx) (MSDN).
- [Migrar bases de datos SQL Server en Windows Azure SQL Database](https://msdn.microsoft.com/en-us/library/windowsazure/jj156160.aspx) (MSDN).


<a id="aspnetmembership"></a>


## <a name="deploying-a-web-application-that-uses-aspnet-application-services-such-as-membership-and-profiling"></a>Implementar una aplicación web que usa la aplicación ASP.NET de servicios como la suscripción y la generación de perfiles

- [Implementar una aplicación de MVC de ASP.NET 5 segura con pertenencia, OAuth y base de datos SQL a un sitio Web de Azure de Windows](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Un tutorial largo que crea e implementa una aplicación que utiliza un único servidor de SQL Server de base de datos tanto para los datos de pertenencia y la aplicación.
- [Identidad de ASP.NET](https://asp.net/identity/). Recursos para la identidad de ASP.NET.
- [Implementación de Web ASP.NET con Visual Studio](../web-forms/overview/deployment/visual-studio-web-deployment/introduction.md). serie de tutoriales de parte de 12, se muestra cómo implementar una base de datos de pertenencia ASP.NET.
- [Configurar un sitio Web que usa los servicios de aplicación](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-a-website-that-uses-application-services-cs.md). Sitio web proyectos pero también es relevante para los proyectos de aplicación web.
- [Los usuarios y Roles en el sitio Web de producción](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/users-and-roles-on-the-production-website-cs.md). Sitio web proyectos pero también es relevante para los proyectos de aplicación web.


<a id="precompiling"></a>


## <a name="precompiling-for-deployment"></a>Precompilar para la implementación

- [Introducción precompilación del proyecto de aplicación Web de ASP.NET](https://msdn.microsoft.com/en-us/library/aa983464.aspx) (MSDN).
- [Empaquetar/Publicar Web (pestaña), propiedades del proyecto](https://msdn.microsoft.com/en-us/library/dd410108.aspx) (MSDN).
- [Advanced precompilar el cuadro de diálogo Configuración](https://msdn.microsoft.com/en-us/library/hh475319.aspx) (MSDN).


<a id="intranet"></a>


## <a name="deploying-an-intranet-web-application"></a>Implementar una aplicación web de intranet

- [Utilice la opción de autenticación de organización de local (ADFS) con ASP.NET en Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/) (Blog de Vittorio Bertocci.).
- [Cómo crear un sitio de Intranet mediante ASP.NET MVC](https://msdn.microsoft.com/en-us/library/gg703322(VS.98).aspx) (MSDN). Anteriores escritos tutorial para Visual Studio 2010, no refleja los cambios principales en las plantillas de proyecto de intranet introducidas en Visual Studio 2013.


<a id="automating"></a>


## <a name="automating-common-deployment-tasks-that-are-not-automated-out-of-the-box"></a>Automatizar tareas comunes de implementación que no se automatizan de fábrica

- [Implementación de Web ASP.NET con Visual Studio: implementar archivos adicionales](../web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files.md).
- [Configuración de permisos de carpeta de publicación de Web](http://sedodream.com/2011/11/08/SettingFolderPermissionsOnWebPublish.aspx) (blog de Sayed Hashimi).
- [Cómo ampliar el archivo de destinos para incluir la configuración del registro para un paquete de proyecto web](https://blogs.msdn.com/webdevtools/archive/2010/02/09/how-to-extend-target-file-to-include-registry-settings-for-web-project-package.aspx) (blog de herramientas de desarrollo Web).
- [Extender la transformación XML (Web.config)](http://sedodream.com/2010/09/09/ExtendingXMLWebconfigConfigTransformation.aspx) (blog de Sayed Hashimi). Muestra cómo crear transformaciones personalizadas de XDT.
- [Web Deployment Tool (MSDeploy) personalizado proveedor tienen 1](http://sedodream.com/2010/03/11/WebDeploymentToolMSDeployCustomProviderTake1.aspx) (blog de Sayed Hashimi). Muestra cómo crear un proveedor personalizado de Web Deploy.
- [Cómo empaquetar e implementar componentes COM](https://blogs.msdn.com/webdevtools/archive/2010/03/03/how-to-package-and-deploy-com-component.aspx) (blog de herramientas de desarrollo Web).
- [Cómo a ensamblados de .NET de paquete](https://blogs.msdn.com/webdevtools/archive/2010/02/19/how-to-package-com-component.aspx) (blog de herramientas de desarrollo Web). Cómo implementar ensamblados en la GAC.
- [Todo: inicializar la VM de Windows Azure para su servidor Web con IIS, Web Deploy y otras cosas Script](http://www.tugberkugurlu.com/archive/script-out-everything-initialize-your-windows-azure-vm-for-your-web-server-with-iis-web-deploy-and-other-stuff) (blog de Tugberk Ugurlu).


<a id="configuringservers"></a>


## <a name="configuring-web-servers-so-that-developers-can-deploy-web-applications-to-them-using-web-deploy"></a>Configurar servidores web para que los desarrolladores pueden implementar aplicaciones web a ellos mediante Web Deploy

- [Instalación y configuración de Web Deploy para Administrador y las implementaciones no sean administradores](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy) (IIS.net sitio).


<a id="hostingprovider"></a>


## <a name="configuring-servers-for-a-hosting-provider"></a>Configuración de servidores para un proveedor de hospedaje

- [Guía de implementación de hospedaje de Microsoft ASP.NET 4](https://go.microsoft.com/fwlink/?LinkId=191365) (centro de descarga de Microsoft).
- [Generar un archivo XML del perfil](https://www.iis.net/learn/web-hosting/joining-the-web-hosting-gallery/generate-a-profile-xml-file) (IIS.net sitio).


<a id="troubleshooting"></a>


## <a name="troubleshooting-deployment-problems"></a>Solución de problemas de implementación

- [Solución de problemas de sitios Web de Windows Azure en Visual Studio](https://docs.microsoft.com/azure/app-service-web/web-sites-dotnet-troubleshoot-visual-studio) (sitio de Microsoft Azure).
- [Implementación de Web ASP.NET con Visual Studio: solución de problemas de](../web-forms/overview/deployment/visual-studio-web-deployment/troubleshooting.md).
- [Implementación la solución de problemas problemas comunes relacionados con Web](https://www.iis.net/learn/publish/troubleshooting-web-deploy/troubleshooting-common-problems-with-web-deploy).
- [Web implementar códigos de Error](https://www.iis.net/learn/publish/troubleshooting-web-deploy/web-deploy-error-codes) (IIS.net sitio).
- [Preguntas más frecuentes de implementación para Visual Studio y ASP.NET Web](https://msdn.microsoft.com/en-us/library/ee942158.aspx) (MSDN).
- [Principales diferencias entre IIS y el servidor de desarrollo de ASP.NET](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md).
- [Diferencias de configuración comunes entre el desarrollo y producción](../web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs.md).
- [Hospedar aplicaciones ASP.NET de nivel de confianza medio](http://www.4guysfromrolla.com/articles/100307-1.aspx) (4 Guys desde Rolla sitio).


<a id="gettinghelp"></a>


## <a name="getting-help-with-a-specific-deployment-question"></a>Obtener ayuda con una pregunta específica de implementación

- [Foro de implementación y configuración de ASP.NET](https://forums.asp.net/26.aspx/1?Configuration and Deployment).
- [StackOverflow.com](http://www.StackOverflow.com).


<a id="additional"></a>


## <a name="additional-resources"></a>Recursos adicionales

Esta sección proporciona vínculos a recursos adicionales que son útiles para obtener más información sobre cómo usar las herramientas de implementación de Visual Studio e IIS.

Los siguientes blogs suelen contener información sobre la implementación de web de Visual Studio:

- [Herramientas de desarrollo de Web en el blog de Microsoft](https://blogs.msdn.com/b/webdevtools/).
- [Blog de Sayed Hashimi](http://www.sedodream.com/).

Los siguientes recursos proporcionan documentación sobre Web Deploy, el marco de trabajo IIS que Visual Studio usa para realizar tareas de implementación de proyecto de aplicación web. Puede hacer preguntas sobre Web Deploy en el [foro de la herramienta de implementación Web](https://go.microsoft.com/fwlink/?LinkId=149411) en el sitio web de IIS.net.

- [Introducción a Web implementar](https://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy).
- [Instalación y configuración de Web implementación](https://www.iis.net/learn/install/installing-publishing-technologies/installing-and-configuring-web-deploy).
- [El programa de instalación de implementación de secuencias de comandos de PowerShell para la automatización de Web](https://www.iis.net/learn/publish/using-web-deploy/powershell-scripts-for-automating-web-deploy-setup).
- [Herramienta de implementación de Web](https://go.microsoft.com/fwlink/?LinkId=151481). Tabla de nivel superior del nodo de contenido para obtener documentación de Web Deploy en el sitio de TechNet. Incluye información de referencia útil, pero la mayoría de la páginas no se han actualizado para años de TechNet.
- [Namespace Microsoft.Web.Deployment](https://go.microsoft.com/fwlink/?LinkId=148630). Documentación de API, no se ha actualizado desde la versión 1.0.
- [El blog del equipo de implementación Web de Microsoft](https://blogs.iis.net/msdeploy/default.aspx).
- [Pestaña de publicación en el sitio web de IIS.net](https://www.iis.net/learn/publish).
