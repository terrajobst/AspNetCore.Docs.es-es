---
uid: identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
title: "Las prácticas recomendadas para implementar las contraseñas y otros datos confidenciales en ASP.NET y el servicio de aplicación de Azure | Documentos de Microsoft"
author: Rick-Anderson
description: "Este tutorial muestra cómo el código puede almacenar y tener acceso a información segura de forma segura. El punto más importante es que nunca debe almacenar contraseñas u otros Cone..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2015
ms.topic: article
ms.assetid: 97902c66-cb61-4d11-be52-73f962f2db0a
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure
msc.type: authoredcontent
ms.openlocfilehash: 995d9a088e3095f36a01d2adb19ec08e6a6d1b3e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure-app-service"></a>Prácticas recomendadas para implementar las contraseñas y otros datos confidenciales a ASP.NET y el servicio de aplicación de Azure
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Este tutorial muestra cómo el código puede almacenar y tener acceso a información segura de forma segura. El punto más importante es nunca debe almacenar las contraseñas u otros datos confidenciales en el código fuente y no utilice secretos de producción en modo de desarrollo y pruebas.
> 
> El código de ejemplo es una aplicación de consola sencilla de trabajo Web y una aplicación de ASP.NET MVC que necesita tener acceso a una base de datos conexión cadena contraseña, Twilio, Google y SendGrid las claves seguras.
> 
> Configuración y PHP también se menciona en local.


- [Trabajar con contraseñas en el entorno de desarrollo](#pwd)
- [Trabajar con cadenas de conexión en el entorno de desarrollo](#con)
- [Aplicaciones de consola de trabajos Web](#wj)
- [Implementación de secretos en Azure](#da)
- [Notas para local y PHP](#not)
- [Recursos adicionales](#addRes)

<a id="pwd"></a>
## <a name="working-with-passwords-in-the-development-environment"></a>Trabajar con contraseñas en el entorno de desarrollo

Los tutoriales muestra con frecuencia los datos confidenciales en el código fuente, es de esperar que con una salvedad que nunca debería almacenar datos confidenciales en el código fuente. Por ejemplo, mi [aplicación de ASP.NET MVC 5 con correo electrónico y SMS 2FA](../../../mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md) tutorial le muestra lo siguiente en el *web.config* archivo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample1.xml)]

El *web.config* archivo es el código fuente, por lo que estos secretos nunca deben almacenarse en ese archivo. Afortunadamente, la `<appSettings>` elemento tiene un `file` atributo que le permite especificar un archivo externo que contiene valores de configuración de aplicación confidencial. Puede mover todos los secretos a un archivo externo como el archivo externo no está protegido en el árbol de origen. Por ejemplo, en el marcado siguiente, el archivo *AppSettingsSecrets.config* contiene todos los servicios de aplicación:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample2.xml)]

El marcado en el archivo externo (*AppSettingsSecrets.config* en este ejemplo), es el mismo formato que se encuentra en la *web.config* archivo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample3.xml)]

El tiempo de ejecución ASP.NET combina el contenido del archivo externo con el marcado en &lt;appSettings&gt; elemento. El tiempo de ejecución omite el atributo de archivo si no se encuentra el archivo especificado.

> [!WARNING]
> Seguridad: no agregue su *secretos .config* de archivo al proyecto o protegerlo en el control de código fuente. De forma predeterminada, Visual Studio establece la `Build Action` a `Content`, lo que significa que el archivo se implementa. Para obtener más información consulte [¿por qué no todos los archivos en la carpeta de proyecto se implementa?](https://msdn.microsoft.com/library/ee942158(v=vs.110).aspx#can_i_exclude_specific_files_or_folders_from_deployment) Aunque puede utilizar cualquier extensión de la *secretos .config* archivo, es mejor mantenerlo *.config*, como archivos de configuración no se sirven mediante IIS. Observe también que la *AppSettingsSecrets.config* archivo está dos niveles por directorio encima de la *web.config* de archivos, por lo que es completamente fuera del directorio de la solución. Moviendo el archivo fuera del directorio de la solución, &quot;agregar git \* &quot; no agregarlo en el repositorio.


<a id="con"></a>
## <a name="working-with-connection-strings-in-the-development-environment"></a>Trabajar con cadenas de conexión en el entorno de desarrollo

Visual Studio crea nuevos proyectos ASP.NET que usan [LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx). LocalDB creada específicamente para el entorno de desarrollo. Que no requiere una contraseña, por lo tanto, no es necesario hacer nada para impedir que los secretos que se va a comprobar en el código fuente. Algunos equipos de desarrollo utilizan las versiones completas de SQL Server (u otros DBMS) que requieren una contraseña.

Puede usar el `configSource` atributo para reemplazar toda la matriz `<connectionStrings>` marcado. A diferencia de la `<appSettings>` `file` atributo que combina el marcado, el `configSource` atributo reemplaza el marcado. El marcado siguiente se muestra la `configSource` de atributo en el *web.config* archivo:

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample4.xml?highlight=1)]

> [!NOTE]
> Si usas el `configSource` atributo tal y como se muestra arriba para mover las cadenas de conexión a un archivo externo y hacer que Visual Studio cree un nuevo sitio web, no será capaz de detectar que usa una base de datos y no tendrá la opción de configuración de la base de datos cuando se pu publicar en Azure desde Visual Studio. Si usas el `configSource` atributo, puede usar PowerShell para crear e implementar el sitio web y la base de datos, o puede crear el sitio web y la base de datos en el portal antes de publicar. El [AzureWebsitewithDB.ps1 New](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) script creará un nuevo sitio web y la base de datos.


> [!WARNING]
> Seguridad: a diferencia de la *AppSettingsSecrets.config* archivo, el archivo de cadenas de conexión externa debe estar en el mismo directorio que la raíz *web.config* de archivos, por lo que tendrá que tomar precauciones para garantizar no se comprueban en el repositorio de origen.


> [!NOTE]
> **Advertencia de seguridad en el archivo de secretos:** un procedimiento recomendado es no utilizar los secretos de producción de prueba y desarrollo. Uso de contraseñas de producción de prueba o desarrollo pérdidas de esos secretos.


<a id="wj"></a>
## <a name="webjobs-console-apps"></a>Aplicaciones de consola de trabajos Web

El *app.config* archivo usado por una aplicación de consola no es compatible con rutas de acceso relativas, pero admiten rutas de acceso absolutas. Puede usar una ruta de acceso absoluta para mover los secretos fuera del directorio de su proyecto. El marcado siguiente muestra los secretos en el *C:\secrets\AppSettingsSecrets.config* archivos y datos no confidenciales en la *app.config* archivo.

[!code-xml[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample5.xml?highlight=2)]

<a id="da"></a>
## <a name="deploying-secrets-to-azure"></a>Implementación de secretos en Azure

Al implementar la aplicación web en Azure, el *AppSettingsSecrets.config* no se implementarán archivo (es decir, lo que desea). También puede ir a la [Portal de administración de Azure](https://azure.microsoft.com/services/management-portal/) y configurarlas manualmente, para ello:

1. Vaya a [https://portal.azure.com](https://portal.azure.com)e inicie sesión con sus credenciales de Azure.
2. Haga clic en **examinar &gt; aplicaciones Web**, a continuación, haga clic en el nombre de la aplicación web.
3. Haga clic en **toda la configuración de &gt; configuración de la aplicación**.

El **configuración de la aplicación** y **cadena de conexión** valores invalidan los mismos valores en la *web.config* archivo. En nuestro ejemplo, no se implementó esta configuración a Azure, pero si estas claves se encontraban en el *web.config* archivo, la configuración se muestra en el portal tendría prioridad.

Una práctica recomendada es seguir un [flujo de trabajo de DevOps](../../../aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything.md) y usar [Azure PowerShell](https://azure.microsoft.com/documentation/articles/install-configure-powershell/) (o en otro marco de trabajo como [Chef](http://www.opscode.com/chef/) o [Puppet](http://puppetlabs.com/puppet/what-is-puppet)) a automatizar la configuración de estos valores en Azure. El siguiente script de PowerShell usa [Export-CliXml](http://www.powershellcookbook.com/recipe/PukO/securely-store-credentials-on-disk) para exportar los secretos cifrados en el disco:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample6.ps1)]

En el script anterior, 'Name' es el nombre de la clave secreta, como '&quot;FB\_AppSecret&quot; o "TwitterSecret". Puede ver el archivo de ".credential" creado por la secuencia de comandos en el explorador. El siguiente fragmento de pruebas cada uno de los archivos de credencial y establece los secretos de la aplicación web con nombre:

[!code-powershell[Main](best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure/samples/sample7.ps1)]

> [!WARNING]
> Seguridad: no incluya contraseñas u otros secretos en el script de PowerShell, haciendo así fracasará el propósito de utilizar un script de PowerShell para implementar los datos confidenciales. El [Get-Credential](https://technet.microsoft.com/library/hh849815.aspx) proporciona un mecanismo de seguridad para obtener una contraseña. Uso de una solicitud de interfaz de usuario puede evitar la pérdida de una contraseña.


### <a name="deploying-db-connection-strings"></a>Implementación de las cadenas de conexión de base de datos

Las cadenas de conexión de base de datos se controlan de forma similar a la configuración de la aplicación. Si implementa la aplicación web de Visual Studio, la cadena de conexión se configurarán automáticamente. Puede comprobarlo en el portal. La manera recomendada para establecer la cadena de conexión es con PowerShell. Para obtener un ejemplo de un script de PowerShell la crea un sitio Web y la base de datos y establece la cadena de conexión en el sitio Web, descargue [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) desde el [biblioteca de secuencia de comandos de Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure).

<a id="not"></a>
## <a name="notes-for-php"></a>Notas para PHP

Desde los pares de clave y valor para ambos **configuración de la aplicación** y **las cadenas de conexión** se almacenan en variables de entorno en el servicio de aplicación de Azure, los programadores que utilizan fácilmente cualquier can de marcos (como PHP) de aplicación web recuperar estos valores. Consulte de Stefan Schackow [sitios Web Windows Azure: cómo las cadenas de la aplicación y el trabajo de las cadenas de conexión](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/) entrada de blog que muestra un fragmento de código PHP para leer la configuración de la aplicación y cadenas de conexión.

## <a name="notes-for-on-premises-servers"></a>Notas para los servidores locales

Si va a implementar en los servidores web locales, puede ayudar a secretos seguros por [cifrar las secciones de configuración de archivos de configuración](https://msdn.microsoft.com/library/ff647398.aspx). Como alternativa, puede utilizar el mismo enfoque recomendado para los sitios Web de Azure: mantener la configuración de desarrollo en archivos de configuración y usar los valores de variables de entorno para la configuración de producción. En este caso, sin embargo, tendrá que escribir código de aplicación para la funcionalidad que es automática en sitios Web de Azure: recuperar la configuración de las variables de entorno y utilizar esos valores en lugar del archivo de configuración o usar el archivo de configuración cuando no se encuentran las variables de entorno.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

Para obtener un ejemplo de un PowerShell script que crea una aplicación web + la base de datos, Establece la cadena de conexión + la configuración de la aplicación, la descarga [New-AzureWebsitewithDB.ps1](https://gallery.technet.microsoft.com/scriptcenter/Ultimate-Create-Web-SQL-DB-9e0fdfd3) desde el [biblioteca de secuencia de comandos de Azure](https://gallery.technet.microsoft.com/scriptcenter/site/search?f%5B0%5D.Type=RootCategory&amp;f%5B0%5D.Value=WindowsAzure). 

Consulte de Stefan Schackow [sitios Web de Azure de Windows: cómo las cadenas de la aplicación y conexión funcionan las cadenas](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/)


Agradecimientos Barry Dorrans especiales ( [ @blowdart ](https://twitter.com/blowdart) ) y Carlos Farre para revisar.
