---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: Procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure App Service | Microsoft Docs
author: Rick-Anderson
description: Este tutorial muestra cómo el código puede almacenar y tener acceso a información segura de forma segura. El punto más importante es que nunca debe almacenar contraseñas u otros sen...
ms.author: aspnetcontent
ms.date: 05/21/2015
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 445e7c32baf4316273b0a5901a776684a6c5d73f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832460"
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure App Service
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial muestra cómo el código puede almacenar y tener acceso a información segura de forma segura. El punto más importante es que nunca debe almacenar las contraseñas u otros datos confidenciales en el código fuente y no debe usar secretos de producción en modo de desarrollo y pruebas.
> 
> El código de ejemplo es una aplicación de consola simple trabajo Web y una aplicación de ASP.NET MVC que necesita tener acceso a una base de datos cadena contraseña, Twilio, Google y SendGrid segura claves de conexión.
> 
> Configuración y PHP también se menciona en local.


- [Trabajar con las contraseñas en el entorno de desarrollo](#pwd)
- [Trabajar con cadenas de conexión en el entorno de desarrollo](#con)
- [Aplicaciones de consola de WebJobs](#wj)
- [Implementación de secretos en Azure](#da)
- [Notas de la local y PHP](#not)
- [Recursos adicionales](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Trabajar con las contraseñas en el entorno de desarrollo

Tutoriales con frecuencia mostrarán datos confidenciales en el código fuente, con suerte con una salvedad de que nunca debería almacenar datos confidenciales en el código fuente. Por ejemplo, mi [aplicación ASP.NET MVC 5 con SMS y correo electrónico 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) tutorial muestra lo siguiente en el *web.config* archivo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

El *web.config* archivo es código fuente, por lo que estos secretos no deben almacenarse nunca en ese archivo. Afortunadamente, la `<appSettings>` elemento tiene un `file` atributo que le permite especificar un archivo externo que contiene los valores de configuración de la aplicación confidencial. Puede mover todos los secretos a un archivo externo, siempre y cuando no se comprueba el archivo externo en el árbol de origen. Por ejemplo, en el marcado siguiente, el archivo *AppSettingsSecrets.config* contiene todos los secretos de aplicación:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

El marcado en el archivo externo (*AppSettingsSecrets.config* en este ejemplo), se encuentra el mismo marcado en el *web.config* archivo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

El tiempo de ejecución ASP.NET combina el contenido del archivo externo con el marcado en &lt;appSettings&gt; elemento. El tiempo de ejecución omite el atributo de archivo si no se encuentra el archivo especificado.

> [!WARNING]
> Seguridad: no agregue su *secretos .config* de archivo al proyecto o insertarlo en control de código fuente. De forma predeterminada, Visual Studio establece la `Build Action` a `Content`, lo que significa que el archivo se implementa. Para obtener más información, consulte [¿por qué no todos los archivos en mi carpeta de proyecto se implementa?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Aunque puede usar cualquier extensión de la *secretos .config* archivo, es mejor mantenerlo *.config*, como los archivos de configuración no se sirven mediante IIS. Observe también que el *AppSettingsSecrets.config* archivo es de dos niveles de directorios copia desde el *web.config* de archivos, por lo que es completamente fuera del directorio de la solución. Moviendo el archivo fuera del directorio de la solución, &quot;git agregar \* &quot; no lo agregue al repositorio.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Trabajar con cadenas de conexión en el entorno de desarrollo

Visual Studio crea nuevos proyectos ASP.NET que usan [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB se creó específicamente para el entorno de desarrollo. No requiere una contraseña, por lo tanto, no es necesario hacer nada para evitar que los secretos que se va a comprobar en el código fuente. Algunos equipos de desarrollo usan las versiones completas de SQL Server (u otro DBMS) que requieren una contraseña.

Puede usar el `configSource` atributo para reemplazar toda la `<connectionStrings>` marcado. A diferencia de la `<appSettings>` `file` atributo que combina el marcado, el `configSource` atributo reemplaza el marcado. El marcado siguiente se muestra el `configSource` atributo en el *web.config* archivo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Si usas el `configSource` atributo tal como se muestra arriba para mover las cadenas de conexión a un archivo externo y que Visual Studio cree un nuevo sitio web, no podrá detectar que usa una base de datos y no tendrá la opción de configuración de la base de datos cuando se pu publicar en Azure desde Visual Studio. Si usas el `configSource` atributo, puede usar PowerShell para crear e implementar su sitio web y la base de datos, o puede crear el sitio web y la base de datos en el portal antes de publicar. El [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) script creará un nuevo sitio web y la base de datos.


> [!WARNING]
> Seguridad: a diferencia de la *AppSettingsSecrets.config* archivo, el archivo de las cadenas de conexión externa debe estar en el mismo directorio que la raíz *web.config* de archivos, por lo que tendrá tomar precauciones para asegurarse de que no consultarla en el repositorio de origen.


> [!NOTE]
> **Advertencia de seguridad en el archivo de secretos:** un procedimiento recomendado es no utilizar los secretos de producción en desarrollo y pruebas. Uso de contraseñas de producción de prueba o desarrollo pérdidas de esos secretos.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Aplicaciones de consola de WebJobs

El *app.config* archivo usado por una aplicación de consola no es compatible con rutas de acceso relativas, pero admite rutas de acceso absolutas. Puede usar una ruta de acceso absoluta para mover los secretos fuera del directorio del proyecto. El marcado siguiente muestra los secretos en el *C:\secrets\AppSettingsSecrets.config* archivos y datos no confidenciales en el *app.config* archivo.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Implementación de secretos en Azure

Al implementar la aplicación web en Azure, el *AppSettingsSecrets.config* archivo no se implementará (que es lo que desea). Puede ir a la [Portal de administración de Azure](https://azure.microsoft.com/services/management-portal/) y configurarlas manualmente, para hacerlo:

1. Vaya a [ https://portal.azure.com ](https://portal.azure.com)e inicie sesión con sus credenciales de Azure.
2. Haga clic en **examinar &gt; aplicaciones Web**, a continuación, haga clic en el nombre de la aplicación web.
3. Haga clic en **toda la configuración &gt; configuración de la aplicación**.

El **configuración de la aplicación** y **cadena de conexión** valores invalidan la misma configuración en el *web.config* archivo. En nuestro ejemplo, no se implementó esta configuración a Azure, pero si estas claves se encontraban en el *web.config* archivo, la configuración que se muestra en el portal tendría prioridad.

Una práctica recomendada es seguir un [flujo de trabajo de DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) y usar [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (o de otro marco, como [Chef](http://www.opscode.com/chef/) o [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) para automatizar la configuración de estos valores en Azure. El siguiente script de PowerShell usa [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) para exportar los secretos cifrados en el disco:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

En el script anterior, "Name" es el nombre de la clave secreta, como '&quot;FB\_AppSecret&quot; o "TwitterSecret". Puede ver el archivo de ".credential" creado por la secuencia de comandos en el explorador. El fragmento de código siguiente comprueba cada uno de los archivos de credenciales y establece los secretos de la aplicación web con nombre:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Seguridad: no incluya contraseñas u otros secretos en el script de PowerShell haciendo derrota por lo que el propósito de utilizar un script de PowerShell para implementar los datos confidenciales. El [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) cmdlet ofrece un mecanismo seguro para obtener una contraseña. Mediante un símbolo del sistema de la interfaz de usuario puede evitar la pérdida de una contraseña.


### <a name="deploying-db-connection-strings"></a>Implementación de las cadenas de conexión de base de datos

Las cadenas de conexión de base de datos se controlan de forma similar a la configuración de la aplicación. Si implementa su aplicación web desde Visual Studio, la cadena de conexión se configurarán automáticamente. Puede comprobarlo en el portal. La manera recomendada para establecer la cadena de conexión es con PowerShell. Para obtener un ejemplo de un script de PowerShell los crea un sitio Web y la base de datos y establece la cadena de conexión en el sitio Web, descargue [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) desde el [biblioteca de scripts de Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Notas para PHP

Desde los pares de clave y valor para ambos **configuración de la aplicación** y **las cadenas de conexión** se almacenan en variables de entorno en Azure App Service, los desarrolladores que usan cualquier can de marcos (como PHP) de aplicación web con facilidad recuperar estos valores. Consulte de Stefan Schackow [sitios Web Windows Azure: How Application Strings and Connection Strings Work](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) entrada de blog que se muestra un fragmento de código PHP para leer la configuración de la aplicación y cadenas de conexión.

## <a name="notes-for-on-premises-servers"></a>Notas para los servidores locales

Si va a implementar en los servidores web de forma local, puede ayudar a salvaguardar secretos por [cifrar las secciones de configuración de los archivos de configuración](https://msdn.microsoft.com/library/ff647398.aspx). Como alternativa, puede usar el mismo enfoque recomendado para Azure Websites: mantenga la configuración de desarrollo en los archivos de configuración y utilizar valores de variables de entorno para la configuración de producción. En este caso, sin embargo, tendrá que escribir código de la aplicación para la funcionalidad que es automática en Azure Websites: recuperar la configuración de las variables de entorno y utilizar esos valores en lugar del archivo de configuración o usar el archivo de configuración cuando no se encuentran las variables de entorno.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

Para obtener un ejemplo de un PowerShell script que crea una aplicación web + base de datos, Establece la cadena de conexión y configuración de la aplicación, descarga [New AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) desde el [biblioteca de scripts de Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Consulte de Stefan Schackow [sitios Web de Azure de Windows: cómo cadenas de aplicación y conexión funcionan las cadenas](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Agradecimientos especiales a Barry Dorrans ( [ @blowdart ](https://twitter.com/blowdart) ) y Carlos Farre para la revisión.
