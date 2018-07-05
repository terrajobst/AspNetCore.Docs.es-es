---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
title: Crear y ejecutar una implementación del comando archivo | Microsoft Docs
author: jrjlee
description: Este tema describe cómo crear un archivo de comandos que le permitirá ejecutar una implementación con los archivos de proyecto de Microsoft Build Engine (MSBuild) como un solo paso, re...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: c61560e9-9f6c-4985-834a-08a3eabf9c3c
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/creating-and-running-a-deployment-command-file
msc.type: authoredcontent
ms.openlocfilehash: fc59920feb5eb48bc8150606b58a1ed4ba60ee92
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395378"
---
<a name="creating-and-running-a-deployment-command-file"></a><span data-ttu-id="97af2-103">Crear y ejecutar un archivo de comandos de implementación</span><span class="sxs-lookup"><span data-stu-id="97af2-103">Creating and Running a Deployment Command File</span></span>
====================
<span data-ttu-id="97af2-104">por [Jason Lee](https://github.com/jrjlee)</span><span class="sxs-lookup"><span data-stu-id="97af2-104">by [Jason Lee](https://github.com/jrjlee)</span></span>

[<span data-ttu-id="97af2-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="97af2-105">Download PDF</span></span>](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> <span data-ttu-id="97af2-106">Este tema describe cómo crear un archivo de comandos que le permitirá ejecutar una implementación con los archivos de proyecto de Microsoft Build Engine (MSBuild) como un proceso paso a paso y repetible.</span><span class="sxs-lookup"><span data-stu-id="97af2-106">This topic describes how to build a command file that will let you run a deployment using Microsoft Build Engine (MSBuild) project files as a single-step, repeatable process.</span></span>


<span data-ttu-id="97af2-107">En este tema forma parte de una serie de tutoriales que se basa en los requisitos de implementación empresarial de una compañía ficticia denominada Fabrikam, Inc. Esta serie de tutoriales usa una solución de ejemplo&#x2014;el [Contact Manager](the-contact-manager-solution.md) solución&#x2014;para representar una aplicación web con un nivel realista de complejidad, incluida una aplicación ASP.NET MVC 3, una comunicación de Windows Servicio Foundation (WCF) y un proyecto de base de datos.</span><span class="sxs-lookup"><span data-stu-id="97af2-107">This topic forms part of a series of tutorials based around the enterprise deployment requirements of a fictional company named Fabrikam, Inc. This tutorial series uses a sample solution&#x2014;the [Contact Manager](the-contact-manager-solution.md) solution&#x2014;to represent a web application with a realistic level of complexity, including an ASP.NET MVC 3 application, a Windows Communication Foundation (WCF) service, and a database project.</span></span>

<span data-ttu-id="97af2-108">El método de implementación en el corazón de estos tutoriales se basa en el enfoque de archivo de proyecto de división descrito en [descripción del proceso de compilación](understanding-the-build-process.md), en que el proceso de compilación se controla mediante dos archivos de proyecto&#x2014;uno que contenga instrucciones que se aplican a todos los entornos de destino y que contiene la configuración de compilación e implementación específica del entorno de compilación.</span><span class="sxs-lookup"><span data-stu-id="97af2-108">The deployment method at the heart of these tutorials is based on the split project file approach described in [Understanding the Build Process](understanding-the-build-process.md), in which the build process is controlled by two project files&#x2014;one containing build instructions that apply to every destination environment, and one containing environment-specific build and deployment settings.</span></span> <span data-ttu-id="97af2-109">En tiempo de compilación, se combina el archivo de proyecto específicos del entorno en el archivo de proyecto independiente del entorno para formar un conjunto completo de las instrucciones de compilación.</span><span class="sxs-lookup"><span data-stu-id="97af2-109">At build time, the environment-specific project file is merged into the environment-agnostic project file to form a complete set of build instructions.</span></span>

## <a name="process-overview"></a><span data-ttu-id="97af2-110">Información general del proceso</span><span class="sxs-lookup"><span data-stu-id="97af2-110">Process Overview</span></span>

<span data-ttu-id="97af2-111">En este tema, obtendrá información sobre cómo crear y ejecutar un archivo de comandos que usa estos archivos de proyecto para realizar una implementación repetible en su entorno de destino.</span><span class="sxs-lookup"><span data-stu-id="97af2-111">In this topic, you'll learn how to create and run a command file that uses these project files to perform a repeatable deployment to your target environment.</span></span> <span data-ttu-id="97af2-112">En esencia, el archivo de comandos simplemente debe contener un comando de MSBuild que:</span><span class="sxs-lookup"><span data-stu-id="97af2-112">Essentially, the command file simply needs to contain an MSBuild command that:</span></span>

- <span data-ttu-id="97af2-113">Indica a MSBuild para ejecutar el entorno agnóstico *Publish.proj* archivo.</span><span class="sxs-lookup"><span data-stu-id="97af2-113">Tells MSBuild to execute the environment-agnostic *Publish.proj* file.</span></span>
- <span data-ttu-id="97af2-114">Indica el *Publish.proj* archivo qué archivo contiene la configuración del proyecto específicas del entorno y dónde encontrarla.</span><span class="sxs-lookup"><span data-stu-id="97af2-114">Tells the *Publish.proj* file which file contains the environment-specific project settings and where to find it.</span></span>

## <a name="create-an-msbuild-command"></a><span data-ttu-id="97af2-115">Crear un comando de MSBuild</span><span class="sxs-lookup"><span data-stu-id="97af2-115">Create an MSBuild Command</span></span>

<span data-ttu-id="97af2-116">Como se describe en [descripción del proceso de compilación](understanding-the-build-process.md), el archivo de proyecto específicos del entorno&#x2014;por ejemplo, *Env Dev.proj*&#x2014;está diseñado para importarse en el entorno agnóstico *Publish.proj* archivo en tiempo de compilación.</span><span class="sxs-lookup"><span data-stu-id="97af2-116">As described in [Understanding the Build Process](understanding-the-build-process.md), the environment-specific project file&#x2014;for example, *Env-Dev.proj*&#x2014;is designed to be imported into the environment-agnostic *Publish.proj* file at build time.</span></span> <span data-ttu-id="97af2-117">Juntos, estos dos archivos proporcionan un conjunto completo de instrucciones que indican a MSBuild cómo compilar e implementar la solución.</span><span class="sxs-lookup"><span data-stu-id="97af2-117">Together, these two files provide a complete set of instructions that tell MSBuild how to build and deploy your solution.</span></span>

<span data-ttu-id="97af2-118">El *Publish.proj* archivo usa un **importar** elemento que se va a importar el archivo de proyecto específicos del entorno.</span><span class="sxs-lookup"><span data-stu-id="97af2-118">The *Publish.proj* file uses an **Import** element to import the environment-specific project file.</span></span>


[!code-xml[Main](creating-and-running-a-deployment-command-file/samples/sample1.xml)]


<span data-ttu-id="97af2-119">Por lo tanto, si usa MSBuild.exe para compilar e implementar la solución Contact Manager, deberá:</span><span class="sxs-lookup"><span data-stu-id="97af2-119">As such, when you use MSBuild.exe to build and deploy the Contact Manager solution, you need to:</span></span>

- <span data-ttu-id="97af2-120">Ejecutar MSBuild.exe en el *Publish.proj* archivo.</span><span class="sxs-lookup"><span data-stu-id="97af2-120">Run MSBuild.exe on the *Publish.proj* file.</span></span>
- <span data-ttu-id="97af2-121">Especifica la ubicación del archivo de proyecto específicos del entorno al proporcionar un parámetro de línea de comandos denominado **TargetEnvPropsFile**.</span><span class="sxs-lookup"><span data-stu-id="97af2-121">Specify the location of the environment-specific project file by supplying a command-line parameter named **TargetEnvPropsFile**.</span></span>

<span data-ttu-id="97af2-122">Para ello, el comando MSBuild debe ser similar a esto:</span><span class="sxs-lookup"><span data-stu-id="97af2-122">To do this, your MSBuild command should resemble this:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample2.cmd)]


<span data-ttu-id="97af2-123">Desde aquí, es un simple paso para mover a una implementación repetible y paso a paso.</span><span class="sxs-lookup"><span data-stu-id="97af2-123">From here, it's a simple step to move to a repeatable, single-step deployment.</span></span> <span data-ttu-id="97af2-124">Todo lo que necesita hacer es agregar el comando de MSBuild en un archivo cmd.</span><span class="sxs-lookup"><span data-stu-id="97af2-124">All you need to do is to add your MSBuild command to a .cmd file.</span></span> <span data-ttu-id="97af2-125">En la solución Contact Manager, la carpeta de publicación incluye un archivo denominado *publicar Dev.cmd* que hace exactamente esto.</span><span class="sxs-lookup"><span data-stu-id="97af2-125">In the Contact Manager solution, the Publish folder includes a file named *Publish-Dev.cmd* that does exactly this.</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample3.cmd)]


> [!NOTE]
> <span data-ttu-id="97af2-126">El **/fl** modificador indica a MSBuild que cree un archivo de registro denominado *msbuild.log* en el directorio de trabajo en el que se invocó MSBuild.exe.</span><span class="sxs-lookup"><span data-stu-id="97af2-126">The **/fl** switch instructs MSBuild to create a log file named *msbuild.log* in the working directory in which MSBuild.exe was invoked.</span></span>


<span data-ttu-id="97af2-127">Para implementar o volver a implementar la solución Contact Manager, todo lo que necesita hacer es ejecutar el *publicar Dev.cmd* archivo.</span><span class="sxs-lookup"><span data-stu-id="97af2-127">To deploy or redeploy the Contact Manager solution, all you need to do is run the *Publish-Dev.cmd* file.</span></span> <span data-ttu-id="97af2-128">Al ejecutar el archivo, MSBuild hará lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="97af2-128">When you run the file, MSBuild will:</span></span>

- <span data-ttu-id="97af2-129">Compilar todos los proyectos de la solución.</span><span class="sxs-lookup"><span data-stu-id="97af2-129">Build all the projects in the solution.</span></span>
- <span data-ttu-id="97af2-130">Generar paquetes de web que se puede implementar para los proyectos de aplicación web.</span><span class="sxs-lookup"><span data-stu-id="97af2-130">Generate deployable web packages for the web application projects.</span></span>
- <span data-ttu-id="97af2-131">Generar archivos .dbschema y .deploymanifest para los proyectos de base de datos.</span><span class="sxs-lookup"><span data-stu-id="97af2-131">Generate .dbschema and .deploymanifest files for the database projects.</span></span>
- <span data-ttu-id="97af2-132">Implementar los paquetes de web en el servidor web.</span><span class="sxs-lookup"><span data-stu-id="97af2-132">Deploy the web packages to the web server.</span></span>
- <span data-ttu-id="97af2-133">Implementar la base de datos en el servidor de base de datos.</span><span class="sxs-lookup"><span data-stu-id="97af2-133">Deploy the database to the database server.</span></span>

## <a name="run-the-deployment"></a><span data-ttu-id="97af2-134">Ejecutar la implementación</span><span class="sxs-lookup"><span data-stu-id="97af2-134">Run the Deployment</span></span>

<span data-ttu-id="97af2-135">Cuando haya creado un archivo de comandos para el entorno de destino, debe ser capaz de completar toda la implementación simplemente ejecutando el archivo.</span><span class="sxs-lookup"><span data-stu-id="97af2-135">When you've created a command file for your target environment, you should be able to complete the entire deployment by simply running the file.</span></span>

<span data-ttu-id="97af2-136">**Para implementar la solución Contact Manager en el entorno de prueba**</span><span class="sxs-lookup"><span data-stu-id="97af2-136">**To deploy the Contact Manager solution to your test environment**</span></span>

1. <span data-ttu-id="97af2-137">En la estación de trabajo de desarrollador, abra el Explorador de Windows y, a continuación, vaya a la ubicación de la *publicar Dev.cmd* archivo.</span><span class="sxs-lookup"><span data-stu-id="97af2-137">On your developer workstation, open Windows Explorer, and then browse to the location of the *Publish-Dev.cmd* file.</span></span>
2. <span data-ttu-id="97af2-138">Haga doble clic en el archivo para ejecutarlo.</span><span class="sxs-lookup"><span data-stu-id="97af2-138">Double-click the file to run it.</span></span>
3. <span data-ttu-id="97af2-139">Si un **abrir archivo-Advertencia de seguridad** aparece el cuadro de diálogo, haga clic en **ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="97af2-139">If an **Open File – Security Warning** dialog box appears, click **Run**.</span></span>
4. <span data-ttu-id="97af2-140">Si los valores de configuración y los servidores de prueba se configuran correctamente, la ventana de símbolo del sistema mostrará un **compilación correcta** cuando MSBuild ha finalizado el procesamiento de los archivos de proyecto del mensaje.</span><span class="sxs-lookup"><span data-stu-id="97af2-140">If your configuration settings and test servers are set up correctly, the Command Prompt window will show a **Build succeeded** message when MSBuild has finished processing the project files.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image1.png)
5. <span data-ttu-id="97af2-141">Si se trata de la primera vez que se ha implementado la solución a este entorno, deberá agregar la cuenta de equipo de servidor web de prueba para el **db\_datawriter** y **db\_datareader**roles en el **ContactManager** base de datos.</span><span class="sxs-lookup"><span data-stu-id="97af2-141">If this is the first time you've deployed the solution to this environment, you'll need to add the test web server machine account to the **db\_datawriter** and **db\_datareader** roles on the **ContactManager** database.</span></span> <span data-ttu-id="97af2-142">Este procedimiento se describe en [configurar un servidor de base de datos para publicación en Web implementar](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="97af2-142">This procedure is described in [Configure a Database Server for Web Deploy Publishing](../configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing.md).</span></span>

    > [!NOTE]
    > <span data-ttu-id="97af2-143">Sólo debe asignar estos permisos al crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="97af2-143">You only need to assign these permissions when you create the database.</span></span> <span data-ttu-id="97af2-144">De forma predeterminada, el proceso de compilación no volverá a la base de datos en cada implementación&#x2014;en su lugar, comparará la base de datos existente en el esquema más reciente y realizar solo los cambios necesarios.</span><span class="sxs-lookup"><span data-stu-id="97af2-144">By default, the build process will not recreate the database on every deployment&#x2014;instead, it will compare the existing database to the latest schema and make only the changes required.</span></span> <span data-ttu-id="97af2-145">Como resultado, solo deberá asignar estos roles de base de datos la primera vez que implemente la solución.</span><span class="sxs-lookup"><span data-stu-id="97af2-145">As a result, you should only need to map these database roles the first time you deploy the solution.</span></span>
6. <span data-ttu-id="97af2-146">Abra Internet Explorer y vaya a la dirección URL de la aplicación de Contact Manager (por ejemplo, `http://testweb1:85/ContactManager/`).</span><span class="sxs-lookup"><span data-stu-id="97af2-146">Open Internet Explorer and browse to the URL of the Contact Manager application (for example, `http://testweb1:85/ContactManager/`).</span></span>
7. <span data-ttu-id="97af2-147">Compruebe que la aplicación funciona según lo previsto y podrá agregar contactos.</span><span class="sxs-lookup"><span data-stu-id="97af2-147">Verify that the application works as expected and you're able to add contacts.</span></span>

    ![](creating-and-running-a-deployment-command-file/_static/image2.png)

## <a name="conclusion"></a><span data-ttu-id="97af2-148">Conclusión</span><span class="sxs-lookup"><span data-stu-id="97af2-148">Conclusion</span></span>

<span data-ttu-id="97af2-149">Creación de un archivo de comandos que contiene las instrucciones de MSBuild proporciona una manera rápida y sencilla de crear e implementar una solución de varios proyectos en un entorno de destino específico.</span><span class="sxs-lookup"><span data-stu-id="97af2-149">Creating a command file containing your MSBuild instructions provides you with a quick and easy way of building and deploying a multi-project solution to a specific destination environment.</span></span> <span data-ttu-id="97af2-150">Si tiene que implementar la solución repetidamente a varios entornos de destino, puede crear varios archivos de comandos.</span><span class="sxs-lookup"><span data-stu-id="97af2-150">If you need to repeatedly deploy your solution to multiple destination environments, you can create multiple command files.</span></span> <span data-ttu-id="97af2-151">En cada archivo de comandos, el comando MSBuild compilará el mismo archivo de proyecto universal, pero especificará un archivo de proyecto específicos del entorno diferente.</span><span class="sxs-lookup"><span data-stu-id="97af2-151">In each command file, the MSBuild command will build the same universal project file, but it will specify a different environment-specific project file.</span></span> <span data-ttu-id="97af2-152">Por ejemplo, un archivo de comandos para publicar en un desarrollador o el entorno de prueba podría contener este comando de MSBuild:</span><span class="sxs-lookup"><span data-stu-id="97af2-152">For example, a command file to publish to a developer or test environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample4.cmd)]


<span data-ttu-id="97af2-153">Un archivo de comandos para publicar en un entorno de ensayo puede contener este comando de MSBuild:</span><span class="sxs-lookup"><span data-stu-id="97af2-153">A command file to publish to a staging environment might contain this MSBuild command:</span></span>


[!code-console[Main](creating-and-running-a-deployment-command-file/samples/sample5.cmd)]


> [!NOTE]
> <span data-ttu-id="97af2-154">Para obtener instrucciones sobre cómo personalizar los archivos de proyecto específicos del entorno para sus propios entornos de servidor, consulte [configurar propiedades de implementación de un entorno de destino](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span><span class="sxs-lookup"><span data-stu-id="97af2-154">For guidance on how to customize the environment-specific project files for your own server environments, see [Configure Deployment Properties for a Target Environment](../configuring-server-environments-for-web-deployment/configuring-deployment-properties-for-a-target-environment.md).</span></span>


<span data-ttu-id="97af2-155">También puede personalizar el proceso de compilación para cada entorno reemplazar propiedades o estableciendo diversos otros modificadores en el comando de MSBuild.</span><span class="sxs-lookup"><span data-stu-id="97af2-155">You can also customize the build process for each environment by overriding properties or setting various other switches in your MSBuild command.</span></span> <span data-ttu-id="97af2-156">Para obtener más información, consulte [referencia de línea de comandos de MSBuild](https://msdn.microsoft.com/library/ms164311.aspx).</span><span class="sxs-lookup"><span data-stu-id="97af2-156">For more information, see [MSBuild Command Line Reference](https://msdn.microsoft.com/library/ms164311.aspx).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="97af2-157">[Anterior](deploying-database-projects.md)
> [Siguiente](manually-installing-web-packages.md)</span><span class="sxs-lookup"><span data-stu-id="97af2-157">[Previous](deploying-database-projects.md)
[Next](manually-installing-web-packages.md)</span></span>
