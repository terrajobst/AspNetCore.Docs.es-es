---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
title: Automatizar todas las acciones (creación de aplicaciones de nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: ba6e6baa-9b9f-471f-b39d-b007a3addadc
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/automate-everything
msc.type: authoredcontent
ms.openlocfilehash: 4acf7361bed7e135c73cd46c99d15e12757e4679
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829151"
---
<a name="automate-everything-building-real-world-cloud-apps-with-azure"></a>Automatizar todas las acciones (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener una introducción para el libro electrónico, consulte [el primer capítulo](introduction.md).


Los tres primeros patrones que examinaremos realmente se aplican a cualquier proyecto de desarrollo de software, pero especialmente para los proyectos en la nube. Este patrón es acerca de cómo automatizar las tareas de desarrollo. Es un tema importante porque los procesos manuales son lentos y propensos a errores; automatizar muchas de ellas como posible ayuda a configurar un flujo de trabajo de agile, confiable y rápida. Es importante de forma exclusiva para el desarrollo de nube porque se puede automatizar fácilmente muchas tareas que son difíciles o imposibles de automatizar en un entorno local. Por ejemplo, puede configurar pruebas completa entornos como nuevo servidor web y máquinas virtuales de back-end, bases de datos de blob storage (almacenamiento de archivo), colas, etcetera.

## <a name="devops-workflow"></a>Flujo de trabajo de DevOps

Cada vez más escucha el término "DevOps". El término desarrollado fuera un reconocimiento de que tiene que integrar las tareas de desarrollo y las operaciones con el fin de desarrollar software de forma eficaz. El tipo de flujo de trabajo que desea habilitar es uno en el que puede desarrollar una aplicación, implementarlo, aprenda de uso de producción del mismo, se cambian en respuesta a lo que ha aprendido y repita el ciclo de manera rápida y confiable.

Algunos equipos de desarrollo correcta en la nube implementación varias veces al día en un entorno en vivo. El equipo de Azure que se usa para implementar a un importante actualizar cada 2-3 meses, pero ahora todos los días 2 y 3 y principales libera cada 2-3 semanas de actualizaciones secundarias de versiones. Introducción a ese ritmo realmente le ayudará a dar respuesta a comentarios de los clientes.

Para ello, tendrá que habilitar un ciclo de desarrollo e implementación que es predecible, confiable y repetible y tiene tiempo de ciclo baja.

![Flujo de trabajo de DevOps](automate-everything/_static/image1.png)

En otras palabras, el período de tiempo entre cuando tiene una idea para una característica y cuando los clientes están usando y proporcionar comentarios debe ser lo más corto posible. Los tres primeros patrones: automatizar todo, control de código fuente, y la integración continua y entrega--son toda la información sobre las prácticas recomendadas que se recomiendan para permitir que ese tipo de proceso.

## <a name="azure-management-scripts"></a>Scripts de administración de Azure

En el [Introducción a este libro electrónico](introduction.md), vio la consola basada en web, el Portal de administración de Azure. El portal de administración permite supervisar y administrar todos los recursos que haya implementado en Azure. Es una manera fácil para crear y eliminar servicios como aplicaciones web y las máquinas virtuales, configure estos servicios, supervisar la operación de servicio y así sucesivamente. Es una herramienta excelente, pero su uso es un proceso manual. Si va a desarrollar una aplicación de producción de cualquier tamaño y, especialmente en un entorno de equipo, le recomendamos que vaya a través de la interfaz de usuario con el fin de aprender y explorar Azure portal y, a continuación, automatizar los procesos que se va a realizar repetidamente.

Casi todo lo que puede hacer manualmente en el portal de administración o desde Visual Studio también puede realizarse mediante una llamada a la API de administración de REST. Puede escribir scripts mediante [Windows PowerShell](https://msdn.microsoft.com/library/windowsazure/jj156055.aspx), o puede usar un marco de código abierto, como [Chef](http://www.opscode.com/chef/) o [Puppet](http://puppetlabs.com/puppet/what-is-puppet). También puede usar la herramienta de línea de comandos de Bash en un entorno Mac o Linux. Azure cuenta con API de scripting para los diferentes entornos, y tiene un [API de administración .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx) en caso de que desea escribir código en lugar de la secuencia de comandos.

Para la aplicación Fix It hemos creado algunas secuencias de comandos de Windows PowerShell que automatizan los procesos de creación de un entorno de prueba e implementación del proyecto para ese entorno y revisaremos algunos de los contenidos de esos scripts.

## <a name="environment-creation-script"></a>Script de creación del entorno

El primer script que examinaremos se denomina *New AzureWebsiteEnv.ps1*. Crea un entorno de Azure que puede implementar la Fix It aplicación para las pruebas. Las tareas principales que realiza esta secuencia de comandos son los siguientes:

- Crear una aplicación web.
- Cree una cuenta de almacenamiento. (Necesario para blobs y colas, como verá en los capítulos posteriores).
- Crear un servidor de base de datos SQL y dos bases de datos: una base de datos de aplicación y una base de datos de pertenencia.
- Store configuración de Azure que la aplicación usará para tener acceso a la cuenta de almacenamiento y bases de datos.
- Crear archivos de configuración que se usará para automatizar la implementación.

### <a name="run-the-script"></a>Ejecute el script


> [!NOTE]
> Esta parte del capítulo muestra ejemplos de secuencias de comandos y los comandos que especifique para poder ejecutarlos. Esta una demostración y no ofrece todo lo que necesita saber para poder ejecutar las secuencias de comandos. Para obtener instrucciones paso a paso how-to--it, consulte [Apéndice: la corrección, aplicación de ejemplo](the-fix-it-sample-application.md#deploybase).


Debe instalar la consola de PowerShell de Azure y configurarlo para que funcione con su suscripción de Azure para ejecutar un script de PowerShell que administra los servicios de Azure. Una vez que esté listo, puede ejecutar el Fix It entorno script de creación con un comando como este:

`.\New-AzureWebsiteEnv.ps1 -Name <websitename> -SqlDatabasePassword <password>`

El `Name` parámetro especifica el nombre que se usará al crear las cuentas de base de datos y almacenamiento y el `SqlDatabasePassword` parámetro especifica la contraseña de la cuenta de administrador que se crearán para la base de datos SQL. Hay otros parámetros que puede usar que veremos más adelante.

![Ventana de PowerShell](automate-everything/_static/image2.png)

Cuando finalice el script puede ver en el portal de administración que se creó. Encontrará dos bases de datos:

![Bases de datos](automate-everything/_static/image3.png)

Una cuenta de almacenamiento:

![Cuenta de almacenamiento](automate-everything/_static/image4.png)

Y una aplicación web:

![Sitio Web](automate-everything/_static/image5.png)

En el **configurar** pestaña de la aplicación web, puede ver que tiene la configuración de la cuenta de almacenamiento y las cadenas de conexión de base de datos SQL configuración para la corrección, aplicación.

![appSettings y connectionStrings](automate-everything/_static/image6.png)

El *automatización* carpeta también contiene ahora un  *&lt;websitename&gt;.pubxml* archivo. Este archivo almacena valores que va a usar MSBuild para implementar la aplicación en el entorno de Azure que acaba de crear. Por ejemplo:

[!code-xml[Main](automate-everything/samples/sample1.xml)]

Como puede ver, el script ha creado un entorno de prueba completa, y todo el proceso se realiza en unos 90 segundos.

Si desea que otra persona de su equipo crear un entorno de prueba, puede ejecutar el script. No sólo es rápido, pero también pueden ser seguros que usan un entorno idéntico a la que está usando. No se pudo volver a ser tan seguro de que, si todo el mundo estaba configurando cosas manualmente mediante el uso de la interfaz de usuario del portal de administración.

### <a name="a-look-at-the-scripts"></a>Un vistazo a las secuencias de comandos

Existen realmente tres scripts que realizan este trabajo. Se llama a uno desde la línea de comandos y usa automáticamente los otros dos para realizar algunas de las tareas:

- *Nuevo AzureWebSiteEnv.ps1* es el script principal.

    - *Nuevo AzureStorage.ps1* crea la cuenta de almacenamiento.
    - *Nuevo AzureSql.ps1* crea las bases de datos.

### <a name="parameters-in-the-main-script"></a>Parámetros en el script principal

El script principal, *New AzureWebSiteEnv.ps1*, define varios parámetros:

[!code-powershell[Main](automate-everything/samples/sample2.ps1)]

Se necesitan dos parámetros:

- El nombre de la aplicación web que crea el script. (Esto también se usa para la dirección URL: `<name>.azurewebsites.net`.)
- La contraseña para el nuevo usuario administrativo del servidor de base de datos que crea el script.

Parámetros opcionales le permiten especificar la ubicación del centro de datos (valor predeterminado es "West US"), el nombre de administrador del servidor de base de datos (valor predeterminado es "dbuser") y una regla de firewall para el servidor de base de datos.

### <a name="create-the-web-app"></a>Crear la aplicación web

Lo primero que hace la secuencia de comandos es crear la aplicación web mediante una llamada a la `New-AzureWebsite` cmdlet, pasando a ella la aplicación web de los valores de parámetro de nombre y la ubicación:

[!code-powershell[Main](automate-everything/samples/sample3.ps1?highlight=2)]

### <a name="create-the-storage-account"></a>Crear la cuenta de almacenamiento

A continuación, ejecuta el script principal el <em>New AzureStorage.ps1</em> secuencia de comandos, especificando "<em>&lt;websitename&gt;</em>almacenamiento" para el nombre de cuenta de almacenamiento, y el mismos centro de datos ubicación como la aplicación web.

[!code-powershell[Main](automate-everything/samples/sample4.ps1?highlight=3)]

*Nuevo AzureStorage.ps1* llamadas el `New-AzureStorageAccount` para crear la cuenta de almacenamiento y lo devuelve a la cuenta de los valores de clave de acceso y el nombre. La aplicación necesitará estos valores con el fin de obtener acceso a los blobs y colas de la cuenta de almacenamiento.

[!code-powershell[Main](automate-everything/samples/sample5.ps1?highlight=2)]

Es posible que no siempre desea crear una nueva cuenta de almacenamiento; se podría mejorar la secuencia de comandos mediante la adición de un parámetro que, opcionalmente, se dirige a usar una cuenta de almacenamiento existente.

### <a name="create-the-databases"></a>Crear las bases de datos

El script principal, a continuación, ejecuta el script de creación de la base de datos, *New AzureSql.ps1*, después de configurar la base de datos predeterminada y nombres de las reglas de firewall:

[!code-powershell[Main](automate-everything/samples/sample6.ps1)]

[!code-powershell[Main](automate-everything/samples/sample7.ps1?highlight=2)]

El script de creación de la base de datos recupera la dirección IP del equipo de desarrollo y establece una regla de firewall para que el equipo de desarrollo puede conectarse y administrar el servidor. El script de creación de la base de datos, a continuación, pasa a través de varios pasos para configurar las bases de datos:

- Crea el servidor mediante el uso de la `New-AzureSqlDatabaseServer` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample8.ps1?highlight=1)]
- Crea reglas de firewall para permitir que la máquina de desarrollo para administrar el servidor y habilitar la aplicación web para conectarse a él. 

    [!code-powershell[Main](automate-everything/samples/sample9.ps1?highlight=3,5)]
- Crea un contexto de base de datos que incluya el nombre del servidor y credenciales, mediante el `New-AzureSqlDatabaseServerContext` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample10.ps1?highlight=4)]

    `New-PSCredentialFromPlainText` es una función en el script que llama el `ConvertTo-SecureString` cmdlet para cifrar la contraseña y devuelve un `PSCredential` objeto, el mismo tipo que el `Get-Credential` cmdlet devuelve.
- Crea la base de datos de aplicación y la base de datos de pertenencia mediante el `New-AzureSqlDatabase` cmdlet.

    [!code-powershell[Main](automate-everything/samples/sample11.ps1?highlight=2,5)]
- Llama a una función definida localmente de tocreates una cadena de conexión para cada base de datos. La aplicación utilizará estas cadenas de conexión para tener acceso a las bases de datos. 

    [!code-powershell[Main](automate-everything/samples/sample12.ps1?highlight=1-2)]

    Get-SQLAzureDatabaseConnectionString es una función definida en el script que crea la cadena de conexión de los valores de parámetro proporcionados a él.

    [!code-powershell[Main](automate-everything/samples/sample13.ps1?highlight=1)]
- Devuelve una tabla hash con el nombre del servidor de base de datos y las cadenas de conexión.

    [!code-powershell[Main](automate-everything/samples/sample14.ps1)]

La aplicación Fix It usa pertenencia diferente y bases de datos de aplicación. También es posible colocar los datos de suscripción y de aplicación en una sola base de datos.

### <a name="store-app-settings-and-connection-strings"></a>Configuración de la aplicación Store y las cadenas de conexión

Azure tiene una característica que le permite almacenar la configuración y las cadenas de conexión que invalidación automáticamente lo que se devuelve a la aplicación cuando intenta leer el `appSettings` o `connectionStrings` colecciones en el archivo Web.config. Esta es una alternativa a la aplicación [transformaciones de Web.config](../../../../web-forms/overview/deployment/visual-studio-web-deployment/web-config-transformations.md) cuando se implementa. Para obtener más información, consulte [Store datos confidenciales en Azure](source-control.md#appsettings) más adelante en este libro electrónico.

El script de creación del entorno se almacena en Azure, todo ello de la `appSettings` y `connectionStrings` valores que la aplicación necesita acceder a la cuenta de almacenamiento y bases de datos cuando se ejecuta en Azure.

[!code-powershell[Main](automate-everything/samples/sample15.ps1)]

[!code-powershell[Main](automate-everything/samples/sample16.ps1)]

[!code-powershell[Main](automate-everything/samples/sample17.ps1?highlight=2)]

[New Relic](http://newrelic.com/) es un marco de telemetría que se muestra en el [supervisión y telemetría](monitoring-and-telemetry.md) capítulo. El script de creación del entorno también reinicia la aplicación web para asegurarse de que recoge la configuración de New Relic.

[!code-powershell[Main](automate-everything/samples/sample18.ps1?highlight=2)]

### <a name="preparing-for-deployment"></a>Preparación para la implementación

Al final del proceso, el script de creación del entorno llama a dos funciones para crear los archivos que se usará en el script de implementación.

Una de estas funciones crea un perfil de publicación *(&lt;websitename&gt;.pubxml* archivo). El código llama a la API de REST de Azure para obtener la configuración de publicación y guarda la información de un *.publishsettings* archivo. A continuación, usa la información de ese archivo junto con un archivo de plantilla (*pubxml.template*) para crear el *.pubxml* archivo que contiene el perfil de publicación. Este proceso en dos pasos simula lo hace en Visual Studio: descargar un *.publishsettings* archivo e importar que para crear un perfil de publicación.

La otra función usa otro archivo de plantilla (sitio Web-environment.template) para crear un *environment.xml del sitio Web* archivo que contiene la configuración que se usará el script de implementación junto con el *.pubxml*archivo.

### <a name="troubleshooting-and-error-handling"></a>Solución de problemas y control de errores

Las secuencias de comandos son como programas: puede producir un error y, si lo desea saber tanto como sea posible sobre el error y lo que hizo que. Por este motivo, el script de creación del entorno cambia el valor de la `VerbosePreference` variable desde `SilentlyContinue` a `Continue` para que se muestren todos los mensajes detallados. También cambia el valor de la `ErrorActionPreference` variable desde `Continue` a `Stop`, de modo que el script detiene incluso cuando encuentra errores de no terminación:

[!code-powershell[Main](automate-everything/samples/sample19.ps1)]

Antes de que realiza cualquier trabajo, el script almacena la hora de inicio para que puede calcular el tiempo transcurrido cuando haya terminado:

[!code-powershell[Main](automate-everything/samples/sample20.ps1)]

Después de completar su trabajo, la secuencia de comandos muestra el tiempo transcurrido:

[!code-powershell[Main](automate-everything/samples/sample21.ps1)]

Y para cada operación de clave el script escribe los mensajes detallados, por ejemplo:

[!code-powershell[Main](automate-everything/samples/sample22.ps1)]

## <a name="deployment-script"></a>Script de implementación

¿Qué la *New AzureWebsiteEnv.ps1* hace la secuencia de comandos para la creación del entorno, el *publicar AzureWebsite.ps1* hace el script de implementación de aplicaciones.

El script de implementación obtiene el nombre de la aplicación web desde el *environment.xml del sitio Web* archivo creado por el script de creación del entorno.

[!code-powershell[Main](automate-everything/samples/sample23.ps1)]

Obtiene la contraseña de usuario de implementación desde el *.publishsettings* archivo:

[!code-powershell[Main](automate-everything/samples/sample24.ps1)]

Ejecuta el [MSBuild](http://msbuildbook.com/) comando que crea e implementa el proyecto:

[!code-powershell[Main](automate-everything/samples/sample25.ps1)]

Y si ha especificado el `Launch` parámetro en la línea de comandos, llama a la `Show-AzureWebsite` cmdlet para abrir el explorador predeterminado en la dirección URL del sitio Web.

[!code-powershell[Main](automate-everything/samples/sample26.ps1?highlight=3)]

Puede ejecutar el script de implementación con un comando como este:

`.\Publish-AzureWebsite.ps1 ..\MyFixIt\MyFixIt.csproj -Launch`

Y cuando haya terminado, el explorador se abre con el sitio que se ejecutan en la nube en el `<websitename>.azurewebsites.net` dirección URL.

![Corregirlo aplicación implementada en Windows Azure](automate-everything/_static/image7.png)

## <a name="summary"></a>Resumen

Con estas secuencias de comandos puede estar seguro de que los mismos pasos se ejecutará siempre en el mismo orden con las mismas opciones. Esto ayuda a garantizar que cada desarrollador en el equipo no se pierda algo o algo estropearse o implementar un nombre personalizado en su propio equipo que en realidad no funcionará del mismo modo en el entorno de otro miembro del equipo o en producción.

De forma similar, puede automatizar Azure más funciones de administración que tiene que hacer en el portal de administración mediante el uso de la API de REST, las secuencias de comandos de Windows PowerShell, un lenguaje .NET API o una utilidad de Bash que se puede ejecutar en Linux o Mac.

En el [siguiente capítulo](source-control.md) se examinará el código fuente y explique por qué es importante incluir los scripts en el repositorio de código fuente.

## <a name="resources"></a>Recursos

- [Instalar y configurar Windows PowerShell para Azure](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). Explica cómo instalar los cmdlets de PowerShell de Azure y cómo instalar el certificado que necesita en el equipo con el fin de administrar Azure cuenta. Esto es un excelente lugar para empezar a trabajar porque también tiene vínculos a recursos de aprendizaje de PowerShell.
- [Centro de scripts de Azure](https://docs.microsoft.com/azure/automation/automation-runbook-gallery). Portal de WindowsAzure.com a los recursos para el desarrollo de scripts que administran los servicios de Azure, con vínculos a obtención de tutoriales de introducción, el código de fuente y documentación de referencia de cmdlet y scripts de muestra
- [Generador de scripts de fin de semana: Introducción a Azure y PowerShell](http://blogs.technet.com/b/heyscriptingguy/archive/2013/06/22/weekend-scripter-getting-started-with-windows-azure-and-powershell.aspx). En un blog dedicado a Windows PowerShell, esta publicación proporciona una excelente introducción al uso de PowerShell para las funciones de administración de Azure.
- [Instalar y configurar la interfaz de línea de comandos de Azure multiplataforma](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Tutorial de introducción para un marco de secuencias de comandos de Azure que funciona en Mac y Linux, así como Windows sistemas.
- [Sección de herramientas de línea de comandos del tema descargar Azure SDK y herramientas](https://azure.microsoft.com/downloads/). Página del portal de documentación y descargas relacionadas con las herramientas de línea de comandos de Azure.
- [Automatización de todo con las bibliotecas de administración de Azure y .NET](http://www.hanselman.com/blog/PennyPinchingInTheCloudAutomatingEverythingWithTheWindowsAzureManagementLibrariesAndNET.aspx). Scott Hanselman presenta la API de administración de .NET para Azure.
- [Usar secuencias de comandos de Windows PowerShell para publicar en entornos de desarrollo y prueba](https://msdn.microsoft.com/library/azure/dn642480.aspx). Documentación de MSDN que se explica cómo usar los scripts que Visual Studio genera automáticamente para los proyectos web de publicación.
- [PowerShell Tools para Visual Studio 2013](https://visualstudiogallery.msdn.microsoft.com/c9eb3ba8-0c59-4944-9a62-6eee37294597). Extensión de Visual Studio que agrega compatibilidad de idioma para Windows PowerShell en Visual Studio.

> [!div class="step-by-step"]
> [Anterior](introduction.md)
> [Siguiente](source-control.md)
