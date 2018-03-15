---
uid: web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
title: Ejecutar Scripts de Windows PowerShell desde archivos de proyecto de MSBuild | Documentos de Microsoft
author: jrjlee
description: "En este tema se describe cómo ejecutar un script de Windows PowerShell como parte de un proceso de compilación e implementación. Puede ejecutar una secuencia de comandos localmente (en otras palabras, en la b..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 55f1ae45-fcb5-43a9-8415-fa5b935fc9c9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/advanced-enterprise-web-deployment/running-windows-powershell-scripts-from-msbuild-project-files
msc.type: authoredcontent
ms.openlocfilehash: afee7b0621df42a8bc70fc6f7c4a8fd0383fa83a
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
<a name="running-windows-powershell-scripts-from-msbuild-project-files"></a><span data-ttu-id="9c5ef-104">Ejecutar Scripts de Windows PowerShell desde archivos de proyecto de MSBuild</span><span class="sxs-lookup"><span data-stu-id="9c5ef-104">Running Windows PowerShell Scripts from MSBuild Project Files</span></span>
====================
<span data-ttu-id="9c5ef-105">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="9c5ef-105">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="9c5ef-106">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="9c5ef-106">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="9c5ef-107">En este tema se describe cómo ejecutar un script de Windows PowerShell como parte de un proceso de compilación e implementación.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-107">This topic describes how to run a Windows PowerShell script as part of a build and deployment process.</span></span> <span data-ttu-id="9c5ef-108">Puede ejecutar un script localmente (en otras palabras, en el servidor de compilación) o de forma remota, al igual que en un servidor de base de datos o el servidor web de destino.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-108">You can run a script locally (in other words, on the build server) or remotely, like on a destination web server or database server.</span></span>
> 
> <span data-ttu-id="9c5ef-109">Hay muchas razones por las que podría ejecutar un script posterior a la implementación de Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-109">There are lots of reasons why you might want to run a post-deployment Windows PowerShell script.</span></span> <span data-ttu-id="9c5ef-110">Por ejemplo, puedes:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-110">For example, you might want to:</span></span>
> 
> - <span data-ttu-id="9c5ef-111">Agregue un origen de evento personalizado en el registro.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-111">Add a custom event source to the registry.</span></span>
> - <span data-ttu-id="9c5ef-112">Generar un directorio de sistema de archivos para las cargas.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-112">Generate a file system directory for uploads.</span></span>
> - <span data-ttu-id="9c5ef-113">Limpiar los directorios de compilación.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-113">Clean up build directories.</span></span>
> - <span data-ttu-id="9c5ef-114">Escribir entradas en un archivo de registro personalizado.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-114">Write entries to a custom log file.</span></span>
> - <span data-ttu-id="9c5ef-115">Enviar mensajes de correo electrónico invitar a los usuarios a una aplicación web recién suministradas.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-115">Send emails inviting users to a newly provisioned web application.</span></span>
> - <span data-ttu-id="9c5ef-116">Crear cuentas de usuario con los permisos adecuados.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-116">Create user accounts with the appropriate permissions.</span></span>
> - <span data-ttu-id="9c5ef-117">Configurar la replicación entre instancias de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-117">Configure replication between SQL Server instances.</span></span>
> 
> <span data-ttu-id="9c5ef-118">En este tema le mostrará cómo ejecutar scripts de Windows PowerShell tanto local como remotamente desde un destino personalizado en un archivo de proyecto de Microsoft Build Engine (MSBuild).</span><span class="sxs-lookup"><span data-stu-id="9c5ef-118">This topic will show you how to run Windows PowerShell scripts both locally and remotely from a custom target in a Microsoft Build Engine (MSBuild) project file.</span></span>


<span data-ttu-id="9c5ef-119">Este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación de empresa de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales que utiliza una solución de ejemplo & #x 2014; la [póngase en contacto con el administrador solución](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)& #x 2014; para representar una aplicación web con un nivel de complejidad, incluso una aplicación de ASP.NET MVC 3, Windows realista Servicio de Communication Foundation (WCF) y un proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-119">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager solution](../web-deployment-in-the-enterprise/the-contact-manager-solution.md)&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="9c5ef-120">El método de implementación en el centro de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md), en que el proceso de compilación se controla mediante dos proyectos, archivos de & #x 2014; uno que contiene instrucciones que se aplican a cada entorno de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-120">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="9c5ef-121">En tiempo de compilación, se combina el archivo de proyecto específicas del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-121">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="task-overview"></a><span data-ttu-id="9c5ef-122">Información general sobre tareas</span><span class="sxs-lookup"><span data-stu-id="9c5ef-122">Task Overview</span></span>

<span data-ttu-id="9c5ef-123">Para ejecutar un script de Windows PowerShell como parte de un proceso de implementación automatizada o paso a paso, debe completar estas tareas de alto nivel:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-123">To run a Windows PowerShell script as part of an automated or single-step deployment process, you'll need to complete these high-level tasks:</span></span>

- <span data-ttu-id="9c5ef-124">Agregue el script de Windows PowerShell para la solución y control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-124">Add the Windows PowerShell script to your solution and to source control.</span></span>
- <span data-ttu-id="9c5ef-125">Crear un comando que invoca la secuencia de comandos de Windows PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-125">Create a command that invokes your Windows PowerShell script.</span></span>
- <span data-ttu-id="9c5ef-126">Escape los caracteres XML reservados en el comando.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-126">Escape any reserved XML characters in your command.</span></span>
- <span data-ttu-id="9c5ef-127">Crear un destino en el archivo de proyecto de MSBuild personalizado y utilizar el **Exec** tarea para ejecutar el comando.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-127">Create a target in your custom MSBuild project file and use the **Exec** task to run your command.</span></span>

<span data-ttu-id="9c5ef-128">En este tema le mostrará cómo realizar estos procedimientos.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-128">This topic will show you how to perform these procedures.</span></span> <span data-ttu-id="9c5ef-129">Las tareas y los tutoriales en este tema se suponen que ya está familiarizado con las propiedades y destinos de MSBuild y saber cómo usar un archivo de proyecto de MSBuild personalizado para dirigir un proceso de compilación e implementación.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-129">The tasks and walkthroughs in this topic assume that you're already familiar with MSBuild targets and properties, and that you understand how to use a custom MSBuild project file to drive a build and deployment process.</span></span> <span data-ttu-id="9c5ef-130">Para obtener más información, consulte [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="9c5ef-130">For more information, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

## <a name="creating-and-adding-windows-powershell-scripts"></a><span data-ttu-id="9c5ef-131">Crear y agregar Scripts de Windows PowerShell</span><span class="sxs-lookup"><span data-stu-id="9c5ef-131">Creating and Adding Windows PowerShell Scripts</span></span>

<span data-ttu-id="9c5ef-132">Las tareas de este tema utilizan un script de Windows PowerShell de ejemplo denominado **LogDeploy.ps1** para ilustrar cómo ejecutar scripts de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-132">The tasks in this topic use a sample Windows PowerShell script named **LogDeploy.ps1** to illustrate how to run scripts from MSBuild.</span></span> <span data-ttu-id="9c5ef-133">El **LogDeploy.ps1** script contiene una función simple que escribe una entrada de línea en un archivo de registro:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-133">The **LogDeploy.ps1** script contains a simple function that writes a single-line entry to a log file:</span></span>


[!code-javascript[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample1.js)]


<span data-ttu-id="9c5ef-134">El **LogDeploy.ps1** secuencia de comandos acepta dos parámetros.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-134">The **LogDeploy.ps1** script accepts two parameters.</span></span> <span data-ttu-id="9c5ef-135">El primer parámetro representa la ruta de acceso completa al archivo de registro a la que desea agregar una entrada, y el segundo parámetro representa el destino de implementación que desea registrar en el archivo de registro.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-135">The first parameter represents the full path to the log file to which you want to add an entry, and the second parameter represents the deployment destination that you want to record in the log file.</span></span> <span data-ttu-id="9c5ef-136">Cuando se ejecuta la secuencia de comandos, agrega una línea al archivo de registro en este formato:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-136">When you run the script, it adds a line to the log file in this format:</span></span>


[!code-html[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample2.html)]


<span data-ttu-id="9c5ef-137">Para realizar la **LogDeploy.ps1** secuencias de comandos disponibles para MSBuild, debe:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-137">To make the **LogDeploy.ps1** script available to MSBuild, you need to:</span></span>

- <span data-ttu-id="9c5ef-138">Agregue la secuencia de comandos para el control de código fuente.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-138">Add the script to source control.</span></span>
- <span data-ttu-id="9c5ef-139">Agregar la secuencia de comandos a la solución en Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-139">Add the script to your solution in Visual Studio 2010.</span></span>

<span data-ttu-id="9c5ef-140">No es necesario implementar la secuencia de comandos con el contenido de la solución, independientemente de si tiene previsto ejecutar la secuencia de comandos en el servidor de compilación o en un equipo remoto.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-140">You don't need to deploy the script with your solution content, regardless of whether you plan to run the script on the build server or on a remote computer.</span></span> <span data-ttu-id="9c5ef-141">Es una opción Agregar la secuencia de comandos a una carpeta de soluciones.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-141">One option is to add the script to a solution folder.</span></span> <span data-ttu-id="9c5ef-142">En el ejemplo póngase en contacto con el administrador, dado que desea usar la secuencia de comandos de Windows PowerShell como parte del proceso de implementación, tiene sentido agregar la secuencia de comandos a la carpeta de solución de publicación.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-142">In the Contact Manager example, because you want to use the Windows PowerShell script as part of the deployment process, it makes sense to add the script to the Publish solution folder.</span></span>

![](running-windows-powershell-scripts-from-msbuild-project-files/_static/image1.png)

<span data-ttu-id="9c5ef-143">El contenido de carpetas de la solución se copia para servidores de compilación como material de origen.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-143">The contents of solution folders are copied to build servers as source material.</span></span> <span data-ttu-id="9c5ef-144">Sin embargo, forma ninguna parte de los resultados del proyecto.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-144">However, they form no part of any project output.</span></span>

## <a name="executing-a-windows-powershell-script-on-the-build-server"></a><span data-ttu-id="9c5ef-145">Ejecutar un Script de Windows PowerShell en el servidor de compilación</span><span class="sxs-lookup"><span data-stu-id="9c5ef-145">Executing a Windows PowerShell Script on the Build Server</span></span>

<span data-ttu-id="9c5ef-146">En algunos escenarios, puede que desee ejecutar scripts de Windows PowerShell en el equipo que compila los proyectos.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-146">In some scenarios, you may want to run Windows PowerShell scripts on the computer that builds your projects.</span></span> <span data-ttu-id="9c5ef-147">Por ejemplo, podría utilizar una secuencia de comandos de Windows PowerShell para limpiar las carpetas de compilación o escribir entradas en un archivo de registro personalizado.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-147">For example, you might use a Windows PowerShell script to clean up build folders or write entries to a custom log file.</span></span>

<span data-ttu-id="9c5ef-148">En cuanto a sintaxis, ejecutando un script de Windows PowerShell desde un archivo de proyecto de MSBuild es la misma que ejecutar un script de Windows PowerShell desde un símbolo del sistema normal.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-148">In terms of syntax, running a Windows PowerShell script from an MSBuild project file is the same as running a Windows PowerShell script from a regular command prompt.</span></span> <span data-ttu-id="9c5ef-149">Debe invocar el ejecutable de powershell.exe y utilizar el **: comando** conmutador para proporcionar los comandos de Windows PowerShell que ejecute.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-149">You need to invoke the powershell.exe executable and use the **–command** switch to provide the commands you want Windows PowerShell to run.</span></span> <span data-ttu-id="9c5ef-150">(En Windows PowerShell v2, también puede utilizar el **: archivo** cambiar).</span><span class="sxs-lookup"><span data-stu-id="9c5ef-150">(In Windows PowerShell v2, you can also use the **–file** switch).</span></span> <span data-ttu-id="9c5ef-151">El comando debe tener este formato:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-151">The command should take this format:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample3.cmd)]


<span data-ttu-id="9c5ef-152">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-152">For example:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample4.cmd)]


<span data-ttu-id="9c5ef-153">Si la ruta de acceso a la secuencia de comandos incluye espacios, debe incluir la ruta de acceso de archivo en precedido por un signo de comillas simples.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-153">If the path to your script includes spaces, you need to enclose the file path in single quotes preceded by an ampersand.</span></span> <span data-ttu-id="9c5ef-154">No se puede utilizar comillas dobles, porque ya se ha utilizado para incluir el comando:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-154">You can't use double quotes, because you've already used them to enclose the command:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample5.cmd)]


<span data-ttu-id="9c5ef-155">Hay algunas consideraciones adicionales cuando se invoca este comando de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-155">There are a few additional considerations when you invoke this command from MSBuild.</span></span> <span data-ttu-id="9c5ef-156">En primer lugar, debe incluir el **: NonInteractive** marca para asegurarse de que el script se ejecuta silenciosamente.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-156">First, you should include the **–NonInteractive** flag to ensure that the script executes quietly.</span></span> <span data-ttu-id="9c5ef-157">A continuación, debe incluir el **– ExecutionPolicy** marca con un valor de argumento apropiada.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-157">Next, you should include the **–ExecutionPolicy** flag with an appropriate argument value.</span></span> <span data-ttu-id="9c5ef-158">Especifica la directiva de ejecución que Windows PowerShell se aplicará a la secuencia de comandos y le permite invalidar la directiva de ejecución predeterminada, lo que puede impedir la ejecución del script.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-158">This specifies the execution policy that Windows PowerShell will apply to your script and allows you to override the default execution policy, which may prevent execution of your script.</span></span> <span data-ttu-id="9c5ef-159">Puede elegir entre estos valores de argumento:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-159">You can choose from these argument values:</span></span>

- <span data-ttu-id="9c5ef-160">Un valor de **Unrestricted** permitirá que Windows PowerShell ejecutar el script, independientemente de si está firmado el script.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-160">A value of **Unrestricted** will allow Windows PowerShell to execute your script, regardless of whether the script is signed.</span></span>
- <span data-ttu-id="9c5ef-161">Un valor de **RemoteSigned** permitirá que Windows PowerShell ejecutar scripts sin firmar que se crearon en el equipo local.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-161">A value of **RemoteSigned** will allow Windows PowerShell to execute unsigned scripts that were created on the local machine.</span></span> <span data-ttu-id="9c5ef-162">Sin embargo, deben firmar las secuencias de comandos que se crearon en otros lugares.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-162">However, scripts that were created elsewhere must be signed.</span></span> <span data-ttu-id="9c5ef-163">(En la práctica, es muy poco probable que haya creado un script de Windows PowerShell localmente en un servidor de compilación).</span><span class="sxs-lookup"><span data-stu-id="9c5ef-163">(In practice, you're very unlikely to have created a Windows PowerShell script locally on a build server).</span></span>
- <span data-ttu-id="9c5ef-164">Un valor de **AllSigned** permitirá que Windows PowerShell ejecutar sólo scripts firmados.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-164">A value of **AllSigned** will allow Windows PowerShell to execute signed scripts only.</span></span>

<span data-ttu-id="9c5ef-165">La directiva de ejecución predeterminada es **Restricted**, lo que impide que Windows PowerShell ejecuta los archivos de script.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-165">The default execution policy is **Restricted**, which prevents Windows PowerShell from running any script files.</span></span>

<span data-ttu-id="9c5ef-166">Por último, necesitará escape los caracteres XML reservados que se producen en el comando de Windows PowerShell:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-166">Finally, you need to escape any reserved XML characters that occur in your Windows PowerShell command:</span></span>

- <span data-ttu-id="9c5ef-167">Reemplace las comillas simples con  **&amp;apos;**</span><span class="sxs-lookup"><span data-stu-id="9c5ef-167">Replace single quotes with **&amp;apos;**</span></span>
- <span data-ttu-id="9c5ef-168">Reemplace las comillas dobles con  **&amp;quot;**</span><span class="sxs-lookup"><span data-stu-id="9c5ef-168">Replace double quotes with **&amp;quot;**</span></span>
- <span data-ttu-id="9c5ef-169">Reemplazar y comerciales con  **&amp;amp;**</span><span class="sxs-lookup"><span data-stu-id="9c5ef-169">Replace ampersands with **&amp;amp;**</span></span>

- <span data-ttu-id="9c5ef-170">Al realizar estos cambios, el comando se parecería al siguiente:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-170">When you make these changes, your command will resemble this:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample6.cmd)]


<span data-ttu-id="9c5ef-171">En el archivo de proyecto de MSBuild personalizado, puede crear un nuevo destino y utilizar el **Exec** tarea para ejecutar este comando:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-171">Within your custom MSBuild project file, you can create a new target and use the **Exec** task to run this command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample7.xml)]


<span data-ttu-id="9c5ef-172">En este ejemplo, tenga en cuenta:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-172">In this example, note that:</span></span>

- <span data-ttu-id="9c5ef-173">Las variables, como valores de parámetro y la ubicación del ejecutable de Windows PowerShell, se declaran como propiedades de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-173">Any variables, like parameter values and the location of the Windows PowerShell executable, are declared as MSBuild properties.</span></span>
- <span data-ttu-id="9c5ef-174">Las condiciones se incluyen para permitir a los usuarios reemplazar estos valores desde la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-174">Conditions are included to enable users to override these values from the command line.</span></span>
- <span data-ttu-id="9c5ef-175">El **MSDeployComputerName** propiedad se declara en otra parte en el archivo de proyecto.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-175">The **MSDeployComputerName** property is declared elsewhere in the project file.</span></span>

<span data-ttu-id="9c5ef-176">Cuando se ejecuta este destino como parte del proceso de compilación, Windows PowerShell se ejecute el comando y escribir una entrada de registro en el archivo especificado.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-176">When you execute this target as part of your build process, Windows PowerShell will run your command and write a log entry to the file you specified.</span></span>

## <a name="executing-a-windows-powershell-script-on-a-remote-computer"></a><span data-ttu-id="9c5ef-177">Ejecutar un Script de Windows PowerShell en un equipo remoto</span><span class="sxs-lookup"><span data-stu-id="9c5ef-177">Executing a Windows PowerShell Script on a Remote Computer</span></span>

<span data-ttu-id="9c5ef-178">Windows PowerShell es capaz de ejecutar las secuencias de comandos en equipos remotos a través de [administración remota de Windows](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span><span class="sxs-lookup"><span data-stu-id="9c5ef-178">Windows PowerShell is capable of running scripts on remote computers through [Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384426.aspx) (WinRM).</span></span> <span data-ttu-id="9c5ef-179">Para ello, debe usar el [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-179">To do this, you need to use the [Invoke-Command](https://technet.microsoft.com/library/dd347578.aspx) cmdlet.</span></span> <span data-ttu-id="9c5ef-180">Esto le permite ejecutar el script en uno o más equipos remotos sin necesidad de copiar la secuencia de comandos a los equipos remotos.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-180">This lets you execute your script against one or more remote computers without copying the script to the remote computers.</span></span> <span data-ttu-id="9c5ef-181">Los resultados se devuelven en el equipo local desde el que se ejecutó el script.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-181">Any results are returned to the local computer from which you ran the script.</span></span>

> [!NOTE]
> <span data-ttu-id="9c5ef-182">Antes de usar el **Invoke-Command** secuencias de comandos de cmdlet para ejecutar Windows PowerShell en un equipo remoto, debe configurar un agente de escucha de WinRM para aceptar mensajes remotos.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-182">Before you use the **Invoke-Command** cmdlet to execute Windows PowerShell scripts on a remote computer, you need to configure a WinRM listener to accept remote messages.</span></span> <span data-ttu-id="9c5ef-183">Para ello, ejecute el comando **winrm quickconfig** en el equipo remoto.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-183">You can do this by running the command **winrm quickconfig** on the remote computer.</span></span> <span data-ttu-id="9c5ef-184">Para obtener más información, consulte [instalación y configuración para la administración remota de Windows](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span><span class="sxs-lookup"><span data-stu-id="9c5ef-184">For more information, see [Installation and Configuration for Windows Remote Management](https://msdn.microsoft.com/library/windows/desktop/aa384372(v=vs.85).aspx).</span></span>


<span data-ttu-id="9c5ef-185">Desde una ventana de Windows PowerShell, podría utilizar esta sintaxis para ejecutar la **LogDeploy.ps1** secuencia de comandos en un equipo remoto:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-185">From a Windows PowerShell window, you'd use this syntax to run the **LogDeploy.ps1** script on a remote computer:</span></span>


[!code-powershell[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample8.ps1)]


> [!NOTE]
> <span data-ttu-id="9c5ef-186">Hay varias otras formas de uso **Invoke-Command** para ejecutar un script de archivo, pero este enfoque es más sencillo cuando necesite proporcionar valores de parámetro y administrar rutas de acceso con espacios.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-186">There are various other ways of using **Invoke-Command** to run a script file, but this approach is the most straightforward when you need to provide parameter values and manage paths with spaces.</span></span>


<span data-ttu-id="9c5ef-187">Cuando se ejecuta en un símbolo del sistema, debe invocar el ejecutable de Windows PowerShell y utilizar el **: comando** parámetro para proporcionar las instrucciones:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-187">When you run this from a command prompt, you need to invoke the Windows PowerShell executable and use the **–command** parameter to provide your instructions:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample9.cmd)]


<span data-ttu-id="9c5ef-188">Como antes, debe proporcionar algunos conmutadores adicionales y escape los caracteres XML reservados al ejecutar el comando de MSBuild:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-188">As before, you need to provide some additional switches and escape any reserved XML characters when you run the command from MSBuild:</span></span>


[!code-console[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample10.cmd)]


<span data-ttu-id="9c5ef-189">Por último, al igual que antes, puede usar el **Exec** tarea dentro de un destino de MSBuild personalizado para ejecutar el comando:</span><span class="sxs-lookup"><span data-stu-id="9c5ef-189">Finally, as before, you can use the **Exec** task within a custom MSBuild target to execute your command:</span></span>


[!code-xml[Main](running-windows-powershell-scripts-from-msbuild-project-files/samples/sample11.xml)]


<span data-ttu-id="9c5ef-190">Cuando se ejecuta este destino como parte del proceso de compilación, Windows PowerShell se ejecutará la secuencia de comandos en el equipo especificado en el **– computername** argumento.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-190">When you execute this target as part of your build process, Windows PowerShell will run your script on the computer you specified in the **–computername** argument.</span></span>

## <a name="conclusion"></a><span data-ttu-id="9c5ef-191">Conclusión</span><span class="sxs-lookup"><span data-stu-id="9c5ef-191">Conclusion</span></span>

<span data-ttu-id="9c5ef-192">En este tema se describe cómo ejecutar un script de Windows PowerShell desde un archivo de proyecto de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-192">This topic described how to run a Windows PowerShell script from an MSBuild project file.</span></span> <span data-ttu-id="9c5ef-193">Puede usar este enfoque para ejecutar un script de Windows PowerShell, ya sea localmente o en un equipo remoto, como parte de un proceso de compilación e implementación automatizado o paso a paso.</span><span class="sxs-lookup"><span data-stu-id="9c5ef-193">You can use this approach to run a Windows PowerShell script, either locally or on a remote computer, as part of an automated or single-step build and deployment process.</span></span>

## <a name="further-reading"></a><span data-ttu-id="9c5ef-194">Información adicional</span><span class="sxs-lookup"><span data-stu-id="9c5ef-194">Further Reading</span></span>

<span data-ttu-id="9c5ef-195">Para obtener instrucciones sobre la firma de scripts de Windows PowerShell y administrar las directivas de ejecución, consulte [ejecutar Scripts de Windows PowerShell](https://technet.microsoft.com/library/ee176949.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c5ef-195">For guidance on signing Windows PowerShell scripts and managing execution policies, see [Running Windows PowerShell Scripts](https://technet.microsoft.com/library/ee176949.aspx).</span></span> <span data-ttu-id="9c5ef-196">Para obtener instrucciones acerca de cómo ejecutar comandos de Windows PowerShell desde un equipo remoto, consulte [ejecutar comandos remotos](https://technet.microsoft.com/library/dd819505.aspx).</span><span class="sxs-lookup"><span data-stu-id="9c5ef-196">For guidance on running Windows PowerShell commands from a remote computer, see [Running Remote Commands](https://technet.microsoft.com/library/dd819505.aspx).</span></span>

<span data-ttu-id="9c5ef-197">Para obtener más información sobre el uso de archivos de proyecto de MSBuild personalizados para controlar el proceso de implementación, consulte [comprender el archivo de proyecto](../web-deployment-in-the-enterprise/understanding-the-project-file.md) y [descripción del proceso de compilación](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span><span class="sxs-lookup"><span data-stu-id="9c5ef-197">For more information on using custom MSBuild project files to control the deployment process, see [Understanding the Project File](../web-deployment-in-the-enterprise/understanding-the-project-file.md) and [Understanding the Build Process](../web-deployment-in-the-enterprise/understanding-the-build-process.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="9c5ef-198">[Anterior](taking-web-applications-offline-with-web-deploy.md)
[Siguiente](troubleshooting-the-packaging-process.md)</span><span class="sxs-lookup"><span data-stu-id="9c5ef-198">[Previous](taking-web-applications-offline-with-web-deploy.md)
[Next](troubleshooting-the-packaging-process.md)</span></span>
