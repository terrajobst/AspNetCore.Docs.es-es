---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Ejecutar secuencias de comandos de Windows PowerShell desde archivos de proyecto de MSBuild | Microsoft Docs
author: jrjlee
description: Este tema describe cómo ejecutar un script de Windows PowerShell como parte de un proceso de compilación e implementación. Puede ejecutar un script localmente (es decir, en la b...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: faedcee480b6c50dc560055206fedbe7af4d5f67
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803154"
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="7e0d0-104">Ejecutar secuencias de comandos de Windows PowerShell desde archivos de proyecto de MSBuild</span><span class="sxs-lookup"><span data-stu-id="7e0d0-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="7e0d0-105">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="7e0d0-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="7e0d0-106">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="7e0d0-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="7e0d0-107">Este tema describe cómo ejecutar un script de Windows PowerShell como parte de un proceso de compilación e implementación.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="7e0d0-108">Puede ejecutar un script localmente (en otras palabras, en el servidor de compilación) o de forma remota, al igual que en un servidor de base de datos o el servidor web de destino.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="7e0d0-109">Hay muchas razones por las que podría interesarle para ejecutar un script de Windows PowerShell posterior a la implementación.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="7e0d0-110">Por ejemplo, puedes:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="7e0d0-111">Agregar un origen de evento personalizado en el registro.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="7e0d0-112">Generar un directorio de sistema de archivos para las cargas.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="7e0d0-113">Limpiar los directorios de compilación.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-113">Clean up build directories.</span></span>
> - <span data-ttu-id="7e0d0-114">Escribir entradas en un archivo de registro personalizado.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="7e0d0-115">Enviar correos electrónicos invitar a los usuarios a una aplicación web recién aprovisionado.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="7e0d0-116">Crear cuentas de usuario con los permisos adecuados.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="7e0d0-117">Configurar la replicación entre instancias de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="7e0d0-118">En este tema le mostrará cómo ejecutar scripts de Windows PowerShell tanto local como remotamente desde un destino personalizado en un archivo de proyecto de Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="7e0d0-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="7e0d0-119">En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [solución Contact Manager](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="7e0d0-120">El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="7e0d0-121">En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="7e0d0-122">Información general de tarea</span><span class="sxs-lookup"><span data-stu-id="7e0d0-122">Task Overview</span></span>

<span data-ttu-id="7e0d0-123">Para ejecutar un script de Windows PowerShell como parte de un proceso de implementación automatizada o paso a paso, necesita completar estas tareas de alto nivel:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="7e0d0-124">Agregue el script de Windows PowerShell a la solución y control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="7e0d0-125">Crear un comando que invoca el script de Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="7e0d0-126">Escape de caracteres XML reservados en el comando.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="7e0d0-127">Crear un destino en el archivo de proyecto de MSBuild personalizado y usar el **Exec** tarea para ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="7e0d0-128">En este tema le mostrará cómo realizar estos procedimientos.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="7e0d0-129">Las tareas y los tutoriales en este tema se suponen que ya está familiarizado con las propiedades y destinos de MSBuild, y de que comprende cómo se utiliza un archivo de proyecto de MSBuild personalizado para controlar un proceso de compilación e implementación.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="7e0d0-130">Para obtener más información, consulte [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="7e0d0-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="7e0d0-131">Crear y agregar secuencias de comandos de Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="7e0d0-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="7e0d0-132">Las tareas de este tema usan un script de Windows PowerShell de ejemplo denominado **LogDeploy.ps1** para ilustrar cómo ejecutar scripts de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="7e0d0-133">El **LogDeploy.ps1** script contiene una función sencilla que escribe una entrada de una línea en un archivo de registro:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.ps1)]


<span data-ttu-id="7e0d0-134">El **LogDeploy.ps1** script acepta dos parámetros.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="7e0d0-135">El primer parámetro representa la ruta de acceso completa al archivo de registro a la que desea agregar una entrada, y el segundo parámetro representa el destino de implementación que desea registrar en el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="7e0d0-136">Al ejecutar el script, agrega una línea al archivo de registro en este formato:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="7e0d0-137">Para realizar la **LogDeploy.ps1** script disponible a MSBuild, debe:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="7e0d0-138">Agregue el script de control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-138">Add the script to source control.</span></span>
- <span data-ttu-id="7e0d0-139">Agregue el script a la solución en Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="7e0d0-140">No es necesario implementar la secuencia de comandos con el contenido de la solución, independientemente de si va a ejecutar el script en el servidor de compilación o en un equipo remoto.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="7e0d0-141">Es una opción Agregar la secuencia de comandos a una carpeta de soluciones.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="7e0d0-142">En el ejemplo Contact Manager, dado que desea usar el script de Windows PowerShell como parte del proceso de implementación, tiene sentido agregar la secuencia de comandos a la carpeta de solución de publicación.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="7e0d0-143">Se copia el contenido de las carpetas de soluciones para los servidores de compilación como material de origen.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="7e0d0-144">Sin embargo, forma ninguna parte de cualquier resultado del proyecto.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="7e0d0-145">Ejecutar un Script de Windows PowerShell en el servidor de compilación</span><span class="sxs-lookup"><span data-stu-id="7e0d0-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="7e0d0-146">En algunos escenarios, puede ejecutar scripts de Windows PowerShell en el equipo que compila los proyectos.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="7e0d0-147">Por ejemplo, podría usar un script de Windows PowerShell para limpiar las carpetas de compilación ni escribir entradas en un archivo de registro personalizado.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="7e0d0-148">En cuanto a sintaxis, ejecutando un script de Windows PowerShell desde un archivo de proyecto de MSBuild es la misma que ejecutar un script de Windows PowerShell desde un símbolo del sistema normal.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="7e0d0-149">Tiene que invocar el ejecutable de powershell.exe y utilizar el **: comando** conmutador para proporcionar los comandos de Windows PowerShell para ejecutar.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="7e0d0-150">(En Windows PowerShell v2, también se puede usar el **: archivo** cambiar).</span><span class="sxs-lookup"><span data-stu-id="7e0d0-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="7e0d0-151">El comando debe tener este formato:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="7e0d0-152">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="7e0d0-153">Si la ruta de acceso para el script incluye espacios, debe encerrarlo entre la ruta de acceso del archivo precedidas por un signo de comillas simples.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="7e0d0-154">No se puede usar comillas dobles, porque ya se ha usado para encerrar el comando:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="7e0d0-155">Hay algunas consideraciones adicionales cuando se invoca este comando de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="7e0d0-156">En primer lugar, debe incluir el **– NonInteractive** marca para asegurarse de que el script se ejecuta silenciosamente.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="7e0d0-157">A continuación, se debe incluir el **– ExecutionPolicy** marca con un valor del argumento correspondiente.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="7e0d0-158">Especifica la directiva de ejecución que Windows PowerShell se aplicará a la secuencia de comandos y le permite invalidar la directiva de ejecución de forma predeterminada, lo que puede impedir la ejecución del script.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="7e0d0-159">Puede elegir entre estos valores de argumento:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="7e0d0-160">Un valor de **Unrestricted** permitirá que Windows PowerShell ejecutar el script, independientemente de si el script está firmado.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="7e0d0-161">Un valor de **RemoteSigned** permitirá que Windows PowerShell ejecutar los scripts sin firmar que se crearon en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="7e0d0-162">Sin embargo, las secuencias de comandos que se crearon en otros lugares deben estar firmados.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="7e0d0-163">(En la práctica, no es muy probable que haya creado un script de Windows PowerShell localmente en un servidor de compilación).</span><span class="sxs-lookup"><span data-stu-id="7e0d0-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="7e0d0-164">Un valor de **AllSigned** permitirá que Windows PowerShell ejecutar sólo scripts firmados.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="7e0d0-165">La directiva de ejecución predeterminada es **Restricted**, lo que impide que Windows PowerShell ejecuta los archivos de script.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="7e0d0-166">Por último, es preciso escape los caracteres XML reservados que se producen en el comando de Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="7e0d0-167">Reemplace las comillas simples con  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="7e0d0-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="7e0d0-168">Reemplace las comillas dobles con  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="7e0d0-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="7e0d0-169">Reemplace los y comerciales con  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="7e0d0-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="7e0d0-170">Al realizar estos cambios, el comando será similar a esto:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="7e0d0-171">En el archivo de proyecto de MSBuild personalizado, puede crear un nuevo destino y usar el **Exec** tarea para ejecutar este comando:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="7e0d0-172">En este ejemplo, tenga en cuenta que:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-172">In this example, note that:</span></span>

- <span data-ttu-id="7e0d0-173">Las variables, como los valores de parámetro y la ubicación del ejecutable de Windows PowerShell, se declaran como propiedades de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="7e0d0-174">Las condiciones se incluyen para permitir a los usuarios reemplazar estos valores desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="7e0d0-175">El **MSDeployComputerName** propiedad se declara en otra parte del archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="7e0d0-176">Cuando este destino se ejecuta como parte del proceso de compilación, Windows PowerShell ejecutará su comando y escribir una entrada de registro en el archivo especificado.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="7e0d0-177">Ejecutar un Script de Windows PowerShell en un equipo remoto</span><span class="sxs-lookup"><span data-stu-id="7e0d0-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="7e0d0-178">Windows PowerShell es capaz de ejecutar las secuencias de comandos en equipos remotos a través de [administración remota de Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="7e0d0-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="7e0d0-179">Para ello, deberá usar el [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="7e0d0-180">Esto le permite ejecutar el script en uno o más equipos remotos sin copiar la secuencia de comandos en los equipos remotos.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="7e0d0-181">Los resultados se devuelven en el equipo local desde el que se ejecutó la secuencia de comandos.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="7e0d0-182">Antes de usar el **Invoke-Command** cmdlet para ejecutar Windows PowerShell scripts en un equipo remoto, deberá configurar un agente de escucha de WinRM para aceptar mensajes remotos.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="7e0d0-183">Para ello, ejecute el comando **winrm quickconfig** en el equipo remoto.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="7e0d0-184">Para obtener más información, consulte [instalación y configuración de Windows administración remota](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="7e0d0-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="7e0d0-185">Desde una ventana de Windows PowerShell, podría utilizar esta sintaxis para ejecutar el **LogDeploy.ps1** secuencia de comandos en un equipo remoto:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="7e0d0-186">Existen diversas formas de usar **Invoke-Command** para ejecutar un script de archivo, pero este enfoque es más sencillo cuando necesite proporcionar los valores de parámetro y administrar rutas de acceso con espacios.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="7e0d0-187">Cuando se ejecuta desde un símbolo del sistema, debe invocar el ejecutable de Windows PowerShell y use la **: comando** parámetro para proporcionar las instrucciones:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="7e0d0-188">Como antes, debe proporcionar algunos modificadores adicionales y escape caracteres XML reservados cuando ejecute el comando de MSBuild:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="7e0d0-189">Por último, como antes, puede usar el **Exec** tarea dentro de un destino de MSBuild personalizado para ejecutar el comando:</span><span class="sxs-lookup"><span data-stu-id="7e0d0-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="7e0d0-190">Cuando este destino se ejecuta como parte del proceso de compilación, Windows PowerShell se ejecutará el script en el equipo que especificó en el **– computername** argumento.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="7e0d0-191">Conclusión</span><span class="sxs-lookup"><span data-stu-id="7e0d0-191">Conclusion</span></span>

<span data-ttu-id="7e0d0-192">En este tema se describe cómo ejecutar un script de Windows PowerShell desde un archivo de proyecto de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="7e0d0-193">Puede usar este enfoque para ejecutar un script de Windows PowerShell, ya sea localmente o en un equipo remoto, como parte de un proceso de compilación e implementación automatizado o paso a paso.</span><span class="sxs-lookup"><span data-stu-id="7e0d0-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="7e0d0-194">Información adicional</span><span class="sxs-lookup"><span data-stu-id="7e0d0-194">Further Reading</span></span>

<span data-ttu-id="7e0d0-195">Para instrucciones sobre cómo firmar scripts de Windows PowerShell y administrar las directivas de ejecución, consulte [ejecutar Scripts de Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="7e0d0-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="7e0d0-196">Para obtener instrucciones sobre cómo ejecutar comandos de Windows PowerShell desde un equipo remoto, consulte [ejecutar comandos remotos](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="7e0d0-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="7e0d0-197">Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, consulte [descripción del archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="7e0d0-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7e0d0-198">[Anterior](taking-web-applications-offline-with-web-deploy.md)
> [Siguiente](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="7e0d0-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
