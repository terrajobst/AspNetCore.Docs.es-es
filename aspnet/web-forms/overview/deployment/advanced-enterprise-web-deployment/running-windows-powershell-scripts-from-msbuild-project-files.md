---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Ejecutar secuencias de comandos de Windows PowerShell desde archivos de proyecto de MSBuild | Microsoft Docs
author: jrjlee
description: Este tema describe cómo ejecutar un script de Windows PowerShell como parte de un proceso de compilación e implementación. Puede ejecutar un script localmente (es decir, en la b...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: ddb658d8a8f224a7c417321df3e17ce0610d2473
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362900"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a>Ejecutar secuencias de comandos de Windows PowerShell desde archivos de proyecto de MSBuild
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo ejecutar un script de Windows PowerShell como parte de un proceso de compilación e implementación. Puede ejecutar un script localmente (en otras palabras, en el servidor de compilación) o de forma remota, al igual que en un servidor de base de datos o el servidor web de destino.
> 
> Hay muchas razones por las que podría interesarle para ejecutar un script de Windows PowerShell posterior a la implementación. Por ejemplo, puedes:
> 
> - Agregar un origen de evento personalizado en el registro.
> - Generar un directorio de sistema de archivos para las cargas.
> - Limpiar los directorios de compilación.
> - Escribir entradas en un archivo de registro personalizado.
> - Enviar correos electrónicos invitar a los usuarios a una aplicación web recién aprovisionado.
> - Crear cuentas de usuario con los permisos adecuados.
> - Configurar la replicación entre instancias de SQL Server.
> 
> En este tema le mostrará cómo ejecutar scripts de Windows PowerShell tanto local como remotamente desde un destino personalizado en un archivo de proyecto de Microsoft Build Engine (MSBuild).


En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.

El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación. En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.

## <a name="task-overview"></a>Información general de tarea

Para ejecutar un script de Windows PowerShell como parte de un proceso de implementación automatizada o paso a paso, necesita completar estas tareas de alto nivel:

- Agregue el script de Windows PowerShell a la solución y control de código fuente.
- Crear un comando que invoca el script de Windows PowerShell.
- Escape de caracteres XML reservados en el comando.
- Crear un destino en el archivo de proyecto de MSBuild personalizado y usar el **Exec** tarea para ejecutar el comando.

En este tema le mostrará cómo realizar estos procedimientos. Las tareas y los tutoriales en este tema se suponen que ya está familiarizado con las propiedades y destinos de MSBuild, y de que comprende cómo se utiliza un archivo de proyecto de MSBuild personalizado para controlar un proceso de compilación e implementación. Para obtener más información, consulte [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

## <a name="creating-and-adding-windows-powershell-scripts"></a>Crear y agregar secuencias de comandos de Windows PowerShell

Las tareas de este tema usan un script de Windows PowerShell de ejemplo denominado **LogDeploy.ps1** para ilustrar cómo ejecutar scripts de MSBuild. El **LogDeploy.ps1** script contiene una función sencilla que escribe una entrada de una línea en un archivo de registro:


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


El **LogDeploy.ps1** script acepta dos parámetros. El primer parámetro representa la ruta de acceso completa al archivo de registro a la que desea agregar una entrada, y el segundo parámetro representa el destino de implementación que desea registrar en el archivo de registro. Al ejecutar el script, agrega una línea al archivo de registro en este formato:


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


Para realizar la **LogDeploy.ps1** script disponible a MSBuild, debe:

- Agregue el script de control de código fuente.
- Agregue el script a la solución en Visual Studio 2010.

No es necesario implementar la secuencia de comandos con el contenido de la solución, independientemente de si va a ejecutar el script en el servidor de compilación o en un equipo remoto. Es una opción Agregar la secuencia de comandos a una carpeta de soluciones. En el ejemplo Contact Manager, dado que desea usar el script de Windows PowerShell como parte del proceso de implementación, tiene sentido agregar la secuencia de comandos a la carpeta de solución de publicación.

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

Se copia el contenido de las carpetas de soluciones para los servidores de compilación como material de origen. Sin embargo, forma ninguna parte de cualquier resultado del proyecto.

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a>Ejecutar un Script de Windows PowerShell en el servidor de compilación

En algunos escenarios, puede ejecutar scripts de Windows PowerShell en el equipo que compila los proyectos. Por ejemplo, podría usar un script de Windows PowerShell para limpiar las carpetas de compilación ni escribir entradas en un archivo de registro personalizado.

En cuanto a sintaxis, ejecutando un script de Windows PowerShell desde un archivo de proyecto de MSBuild es la misma que ejecutar un script de Windows PowerShell desde un símbolo del sistema normal. Tiene que invocar el ejecutable de powershell.exe y utilizar el **: comando** conmutador para proporcionar los comandos de Windows PowerShell para ejecutar. (En Windows PowerShell v2, también se puede usar el **: archivo** cambiar). El comando debe tener este formato:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


Por ejemplo:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


Si la ruta de acceso para el script incluye espacios, debe encerrarlo entre la ruta de acceso del archivo precedidas por un signo de comillas simples. No se puede usar comillas dobles, porque ya se ha usado para encerrar el comando:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


Hay algunas consideraciones adicionales cuando se invoca este comando de MSBuild. En primer lugar, debe incluir el **– NonInteractive** marca para asegurarse de que el script se ejecuta silenciosamente. A continuación, se debe incluir el **– ExecutionPolicy** marca con un valor del argumento correspondiente. Especifica la directiva de ejecución que Windows PowerShell se aplicará a la secuencia de comandos y le permite invalidar la directiva de ejecución de forma predeterminada, lo que puede impedir la ejecución del script. Puede elegir entre estos valores de argumento:

- Un valor de **Unrestricted** permitirá que Windows PowerShell ejecutar el script, independientemente de si el script está firmado.
- Un valor de **RemoteSigned** permitirá que Windows PowerShell ejecutar los scripts sin firmar que se crearon en el equipo local. Sin embargo, las secuencias de comandos que se crearon en otros lugares deben estar firmados. (En la práctica, no es muy probable que haya creado un script de Windows PowerShell localmente en un servidor de compilación).
- Un valor de **AllSigned** permitirá que Windows PowerShell ejecutar sólo scripts firmados.

La directiva de ejecución predeterminada es **Restricted**, lo que impide que Windows PowerShell ejecuta los archivos de script.

Por último, es preciso escape los caracteres XML reservados que se producen en el comando de Windows PowerShell:

- Reemplace las comillas simples con  **&amp;apos;**
- Reemplace las comillas dobles con  **&amp;quot;**
- Reemplace los y comerciales con  **&amp;amp;**

- Al realizar estos cambios, el comando será similar a esto:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


En el archivo de proyecto de MSBuild personalizado, puede crear un nuevo destino y usar el **Exec** tarea para ejecutar este comando:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


En este ejemplo, tenga en cuenta que:

- Las variables, como los valores de parámetro y la ubicación del ejecutable de Windows PowerShell, se declaran como propiedades de MSBuild.
- Las condiciones se incluyen para permitir a los usuarios reemplazar estos valores desde la línea de comandos.
- El **MSDeployComputerName** propiedad se declara en otra parte del archivo de proyecto.

Cuando este destino se ejecuta como parte del proceso de compilación, Windows PowerShell ejecutará su comando y escribir una entrada de registro en el archivo especificado.

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a>Ejecutar un Script de Windows PowerShell en un equipo remoto

Windows PowerShell es capaz de ejecutar las secuencias de comandos en equipos remotos a través de [administración remota de Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM). Para ello, deberá usar el [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet. Esto le permite ejecutar el script en uno o más equipos remotos sin copiar la secuencia de comandos en los equipos remotos. Los resultados se devuelven en el equipo local desde el que se ejecutó la secuencia de comandos.

> [!NOTE]
> Antes de usar el **Invoke-Command** cmdlet para ejecutar Windows PowerShell scripts en un equipo remoto, deberá configurar un agente de escucha de WinRM para aceptar mensajes remotos. Para ello, ejecute el comando **winrm quickconfig** en el equipo remoto. Para obtener más información, consulte [instalación y configuración de Windows administración remota](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).


Desde una ventana de Windows PowerShell, podría utilizar esta sintaxis para ejecutar el **LogDeploy.ps1** secuencia de comandos en un equipo remoto:


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> Existen diversas formas de usar **Invoke-Command** para ejecutar un script de archivo, pero este enfoque es más sencillo cuando necesite proporcionar los valores de parámetro y administrar rutas de acceso con espacios.


Cuando se ejecuta desde un símbolo del sistema, debe invocar el ejecutable de Windows PowerShell y use la **: comando** parámetro para proporcionar las instrucciones:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


Como antes, debe proporcionar algunos modificadores adicionales y escape caracteres XML reservados cuando ejecute el comando de MSBuild:


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


Por último, como antes, puede usar el **Exec** tarea dentro de un destino de MSBuild personalizado para ejecutar el comando:


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


Cuando este destino se ejecuta como parte del proceso de compilación, Windows PowerShell se ejecutará el script en el equipo que especificó en el **– computername** argumento.

## <a name="conclusion"></a>Conclusión

En este tema se describe cómo ejecutar un script de Windows PowerShell desde un archivo de proyecto de MSBuild. Puede usar este enfoque para ejecutar un script de Windows PowerShell, ya sea localmente o en un equipo remoto, como parte de un proceso de compilación e implementación automatizado o paso a paso.

## <a name="further-reading"></a>Información adicional

Para instrucciones sobre cómo firmar scripts de Windows PowerShell y administrar las directivas de ejecución, consulte [ejecutar Scripts de Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx). Para obtener instrucciones sobre cómo ejecutar comandos de Windows PowerShell desde un equipo remoto, consulte [ejecutar comandos remotos](https://technet.microsoft.com/library/dd819505.aspx).

Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, consulte [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).

> [!div class="step-by-step"]
> [Anterior](taking-web-applications-offline-with-web-deploy.md)
> [Siguiente](troubleshooting-the-packaging-process.md)
