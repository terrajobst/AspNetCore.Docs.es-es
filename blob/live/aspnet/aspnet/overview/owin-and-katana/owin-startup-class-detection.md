---
uid: aspnet/overview/owin-and-katana/owin-startup-class-detection
title: "Detección de clase de inicio OWIN | Documentos de Microsoft"
author: Praburaj
description: "Este tutorial muestra cómo configurar qué clase de inicio OWIN se carga. Para obtener más información sobre OWIN, vea una información general del proyecto Katana. Este tutorial era..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/17/2013
ms.topic: article
ms.assetid: 08257f55-36f4-4e39-9c88-2a5602838c79
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/owin-startup-class-detection
msc.type: authoredcontent
ms.openlocfilehash: a6ac34307b7558ad13684448f339ca74ade9e997
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="owin-startup-class-detection"></a><span data-ttu-id="88ec0-105">Detección de clase de inicio OWIN</span><span class="sxs-lookup"><span data-stu-id="88ec0-105">OWIN Startup Class Detection</span></span>
====================
<span data-ttu-id="88ec0-106">por [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="88ec0-106">by [Praburaj Thiagarajan](https://github.com/Praburaj), [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="88ec0-107">Este tutorial muestra cómo configurar qué clase de inicio OWIN se carga.</span><span class="sxs-lookup"><span data-stu-id="88ec0-107">This tutorial shows how to configure which OWIN startup class is loaded.</span></span> <span data-ttu-id="88ec0-108">Para obtener más información sobre OWIN, consulte [una información general del proyecto Katana](an-overview-of-project-katana.md).</span><span class="sxs-lookup"><span data-stu-id="88ec0-108">For more information on OWIN, see [An Overview of Project Katana](an-overview-of-project-katana.md).</span></span> <span data-ttu-id="88ec0-109">Este tutorial se escribió Rick Anderson ( [ @RickAndMSFT ](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan y Howard Dierking ( [ @howard \_dierking](https://twitter.com/howard_dierking) ).</span><span class="sxs-lookup"><span data-stu-id="88ec0-109">This tutorial was written by Rick Anderson ( [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT) ), Praburaj Thiagarajan, and Howard Dierking ( [@howard\_dierking](https://twitter.com/howard_dierking) ).</span></span>
> 
> ## <a name="prerequisites"></a><span data-ttu-id="88ec0-110">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="88ec0-110">Prerequisites</span></span>
> 
> [<span data-ttu-id="88ec0-111">Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="88ec0-111">Visual Studio 2013</span></span>](https://www.microsoft.com/visualstudio/eng/2013-downloads)


## <a name="owin-startup-class-detection"></a><span data-ttu-id="88ec0-112">Detección de clase de inicio OWIN</span><span class="sxs-lookup"><span data-stu-id="88ec0-112">OWIN Startup Class Detection</span></span>

 <span data-ttu-id="88ec0-113">Todas las aplicaciones OWIN tiene una clase de inicio donde se especifican los componentes de la canalización de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="88ec0-113">Every OWIN Application has a startup class where you specify components for the application pipeline.</span></span> <span data-ttu-id="88ec0-114">Hay diferentes maneras de conectarse la clase de inicio con el tiempo de ejecución, según el modelo de hospedaje que elija (OwinHost, IIS y IIS Express).</span><span class="sxs-lookup"><span data-stu-id="88ec0-114">There are different ways you can connect your startup class with the runtime, depending on the hosting model you choose (OwinHost, IIS, and IIS-Express).</span></span> <span data-ttu-id="88ec0-115">La clase de inicio que se muestra en este tutorial se puede utilizar en cada aplicación de hospedaje.</span><span class="sxs-lookup"><span data-stu-id="88ec0-115">The startup class shown in this tutorial can be used in every hosting application.</span></span> <span data-ttu-id="88ec0-116">La clase de inicio se conecta con el tiempo de ejecución hospedaje mediante uno de estos enfoques:</span><span class="sxs-lookup"><span data-stu-id="88ec0-116">You connect the startup class with the hosting runtime using one of the these approaches:</span></span>  

1. <span data-ttu-id="88ec0-117">**Convención de nomenclatura**: Katana busca una clase denominada `Startup` en el espacio de nombres que coincidan con el nombre del ensamblado o el espacio de nombres global.</span><span class="sxs-lookup"><span data-stu-id="88ec0-117">**Naming Convention**: Katana looks for a class named `Startup` in namespace matching the assembly name or the global namespace.</span></span>
2. <span data-ttu-id="88ec0-118">**Atributo OwinStartup**: este es el enfoque en la mayoría de los desarrolladores le llevará a especificar la clase de inicio.</span><span class="sxs-lookup"><span data-stu-id="88ec0-118">**OwinStartup Attribute**: This is the approach most developers will take to specify the startup class.</span></span> <span data-ttu-id="88ec0-119">El atributo siguiente establecerá la clase de inicio la `TestStartup` clase en el `StartupDemo` espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="88ec0-119">The following attribute will set the startup class to the `TestStartup` class in the `StartupDemo` namespace.</span></span> 

    [!code-csharp[Main](owin-startup-class-detection/samples/sample1.cs)]

 <span data-ttu-id="88ec0-120">El `OwinStartup` atributo invalida la convención de nomenclatura.</span><span class="sxs-lookup"><span data-stu-id="88ec0-120">The `OwinStartup` attribute overrides the naming convention.</span></span> <span data-ttu-id="88ec0-121">También puede especificar un nombre descriptivo con este atributo, sin embargo, con un nombre descriptivo requiere que use también el `appSetting` elemento en el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="88ec0-121">You can also specify a friendly name with this attribute, however, using a friendly name requires you to also use the `appSetting` element in the configuration file.</span></span>
3. <span data-ttu-id="88ec0-122">**El elemento appSetting del archivo de configuración**: el `appSetting` elemento invalida la `OwinStartup` atributo y la convención de nomenclatura.</span><span class="sxs-lookup"><span data-stu-id="88ec0-122">**The appSetting element in the Configuration file**: The `appSetting` element overrides the `OwinStartup` attribute and naming convention.</span></span> <span data-ttu-id="88ec0-123">Puede tener varias clases de inicio (cada mediante un `OwinStartup` atributo) y configurar qué clase de inicio se cargará en un archivo de configuración usando un marcado similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="88ec0-123">You can have multiple startup classes (each using an `OwinStartup` attribute) and configure which startup class will be loaded in a configuration file using markup similar to the following:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample2.xml)]

 <span data-ttu-id="88ec0-124">También se puede utilizar la siguiente clave, que se especifica explícitamente la clase de inicio y el ensamblado:</span><span class="sxs-lookup"><span data-stu-id="88ec0-124">The following key, which explicitly specifies the startup class and assembly can also be used:</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample3.xml)]

 <span data-ttu-id="88ec0-125">El siguiente código XML en el archivo de configuración especifica un nombre de clase descriptivo inicio `ProductionConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-125">The following XML in the configuration file specifies a friendly startup class name of `ProductionConfiguration`.</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample4.xml)]

 <span data-ttu-id="88ec0-126">Se debe utilizar el marcado anterior con el siguiente `OwinStartup` atributo que especifica un nombre descriptivo y hace que la `ProductionStartup2` clase para ejecutar.</span><span class="sxs-lookup"><span data-stu-id="88ec0-126">The above markup must be used with the following `OwinStartup` attribute which specifies a friendly name and causes the `ProductionStartup2` class to run.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample5.cs?highlight=1,16)]
4. <span data-ttu-id="88ec0-127">Para deshabilitar la detección de inicio de OWIN agregar la `appSetting owin:AutomaticAppStartup` con un valor de `"false"` en el archivo web.config.</span><span class="sxs-lookup"><span data-stu-id="88ec0-127">To disable OWIN startup discovery add the `appSetting owin:AutomaticAppStartup` with a value of `"false"` in the web.config file.</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample6.xml)]

## <a name="create-an-aspnet-web-app-using-owin-startup"></a><span data-ttu-id="88ec0-128">Crear una aplicación Web de ASP.NET mediante el inicio de OWIN</span><span class="sxs-lookup"><span data-stu-id="88ec0-128">Create an ASP.NET Web App using OWIN Startup</span></span>

1. <span data-ttu-id="88ec0-129">Crear una aplicación web de Asp.Net vacía y asígnele el nombre **StartupDemo**.</span><span class="sxs-lookup"><span data-stu-id="88ec0-129">Create an empty Asp.Net web application and name it **StartupDemo**.</span></span> <span data-ttu-id="88ec0-130">-Instalar `Microsoft.Owin.Host.SystemWeb` mediante el Administrador de paquetes de NuGet.</span><span class="sxs-lookup"><span data-stu-id="88ec0-130">- Install `Microsoft.Owin.Host.SystemWeb` using the NuGet package manager.</span></span> <span data-ttu-id="88ec0-131">Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**y, a continuación, **Package Manager Console**.</span><span class="sxs-lookup"><span data-stu-id="88ec0-131">From the **Tools** menu, select **Library Package Manager**, and then **Package Manager Console**.</span></span> <span data-ttu-id="88ec0-132">Escriba el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="88ec0-132">Enter the following command:</span></span>  

    [!code-powershell[Main](owin-startup-class-detection/samples/sample7.ps1)]
- <span data-ttu-id="88ec0-133">Agregar una clase de inicio OWIN.</span><span class="sxs-lookup"><span data-stu-id="88ec0-133">Add an OWIN startup class.</span></span> <span data-ttu-id="88ec0-134">En Visual Studio 2013 haga clic en el proyecto y seleccione **Agregar clase**.: en la **Agregar nuevo elemento** diálogo cuadro, escriba *OWIN* en el campo de búsqueda y cambie el nombre a Startup.cs, y, a continuación, haga clic en **agregar**.</span><span class="sxs-lookup"><span data-stu-id="88ec0-134">In Visual Studio 2013 right click the project and select **Add Class**.- In the **Add New Item** dialog box, enter *OWIN* in the search field, and change the name to Startup.cs, and then click **Add**.</span></span>  
  
    ![](owin-startup-class-detection/_static/image1.png)   
  
 <span data-ttu-id="88ec0-135">La próxima vez que se va a agregar un *clase de inicio de Owin*, estará en disponible desde el **agregar** menú.</span><span class="sxs-lookup"><span data-stu-id="88ec0-135">The next time you want to add an *Owin Startup class*, it will be in available from the **Add** menu.</span></span>  
   
    ![](owin-startup-class-detection/_static/image2.png)  
  
 <span data-ttu-id="88ec0-136">Como alternativa, puede hacer clic en el proyecto y seleccione **agregar**, a continuación, seleccione **nuevo elemento**y, a continuación, seleccione la **clase de inicio de Owin**.</span><span class="sxs-lookup"><span data-stu-id="88ec0-136">Alternatively, you can right click the project and select **Add**, then select **New Item**, and then select the **Owin Startup class**.</span></span>  
  
    ![](owin-startup-class-detection/_static/image3.png)  
  
- <span data-ttu-id="88ec0-137">Reemplace el código generado en el *Startup.cs* archivo con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="88ec0-137">Replace the generated code in the *Startup.cs* file with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample8.cs?highlight=5,7,15-28,31-34)]
  
 <span data-ttu-id="88ec0-138">El `app.Use` expresión lambda se usa para registrar el componente de middleware especificado a la canalización OWIN.</span><span class="sxs-lookup"><span data-stu-id="88ec0-138">The `app.Use` lambda expression is used to register the specified middleware component to the OWIN pipeline.</span></span> <span data-ttu-id="88ec0-139">En este caso estamos configurando el registro de las solicitudes entrantes antes de responder a la solicitud entrante.</span><span class="sxs-lookup"><span data-stu-id="88ec0-139">In this case we are setting up logging of incoming requests before responding to the incoming request.</span></span> <span data-ttu-id="88ec0-140">El `next` parámetro es el delegado ( [Func](https://msdn.microsoft.com/en-us/library/bb534960(v=vs.100).aspx) &lt; [tarea](https://msdn.microsoft.com/en-us/library/dd321424(v=vs.100).aspx) &gt; ) para el siguiente componente de la canalización.</span><span class="sxs-lookup"><span data-stu-id="88ec0-140">The `next` parameter is the delegate ( [Func](https://msdn.microsoft.com/en-us/library/bb534960(v=vs.100).aspx) &lt; [Task](https://msdn.microsoft.com/en-us/library/dd321424(v=vs.100).aspx) &gt; ) to the next component in the pipeline.</span></span> <span data-ttu-id="88ec0-141">El `app.Run` expresión lambda enlaza la canalización a las solicitudes entrantes y proporciona el mecanismo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="88ec0-141">The `app.Run` lambda expression hooks up the pipeline to incoming requests and provides the response mechanism.</span></span>
     > [!NOTE]
     > <span data-ttu-id="88ec0-142">En el código anterior se ha marcado como comentario el `OwinStartup` atributo y nos estamos depender de la convención de la ejecución de la clase denominada `Startup` .-presione ***F5*** para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="88ec0-142">In the code above we have commented out the `OwinStartup` attribute and we're relying on the convention of running the class named `Startup` .- Press ***F5*** to run the application.</span></span> <span data-ttu-id="88ec0-143">Actualice la vista varias veces.</span><span class="sxs-lookup"><span data-stu-id="88ec0-143">Hit refresh a few times.</span></span>  
  
    ![](owin-startup-class-detection/_static/image4.png)  
<span data-ttu-id="88ec0-144">Nota: El número mostrado entre las imágenes en este tutorial no coincidirá con el número que aparece.</span><span class="sxs-lookup"><span data-stu-id="88ec0-144">Note: The number shown in the images in this tutorial will not match the number you see.</span></span> <span data-ttu-id="88ec0-145">La cadena de milisegundo se usa para mostrar una nueva respuesta cuando se actualice la página.</span><span class="sxs-lookup"><span data-stu-id="88ec0-145">The millisecond string is used to show a new response when you refresh the page.</span></span>  
 <span data-ttu-id="88ec0-146">Puede ver la información de seguimiento en el **salida** ventana.</span><span class="sxs-lookup"><span data-stu-id="88ec0-146">You can see the trace information in the **Output** window.</span></span>  
  
    ![](owin-startup-class-detection/_static/image5.png)

## <a name="add-more-startup-classes"></a><span data-ttu-id="88ec0-147">Agregar más clases de inicio</span><span class="sxs-lookup"><span data-stu-id="88ec0-147">Add More Startup Classes</span></span>

<span data-ttu-id="88ec0-148">En esta sección, vamos a agregar otra clase de inicio.</span><span class="sxs-lookup"><span data-stu-id="88ec0-148">In this section we'll add another Startup class.</span></span> <span data-ttu-id="88ec0-149">Puede agregar varias clases de inicio OWIN para la aplicación.</span><span class="sxs-lookup"><span data-stu-id="88ec0-149">You can add multiple OWIN startup class to your application.</span></span> <span data-ttu-id="88ec0-150">Por ejemplo, puede crear clases de inicio para el desarrollo, pruebas y producción.</span><span class="sxs-lookup"><span data-stu-id="88ec0-150">For example, you might want to create startup classes for development, testing and production.</span></span>

1. <span data-ttu-id="88ec0-151">Cree una nueva clase de inicio de OWIN y asígnele el nombre `ProductionStartup`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-151">Create a new OWIN Startup class and name it `ProductionStartup`.</span></span>
2. <span data-ttu-id="88ec0-152">Reemplace el código generado con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="88ec0-152">Replace the generated code with the following:</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample9.cs?highlight=14-18)]
3. <span data-ttu-id="88ec0-153">Presione F5 de Control para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="88ec0-153">Press Control F5 to run the app.</span></span> <span data-ttu-id="88ec0-154">El `OwinStartup` atributo especifica que se ejecuta la clase de inicio de producción.</span><span class="sxs-lookup"><span data-stu-id="88ec0-154">The `OwinStartup` attribute specifies the production startup class is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image6.png)
4. <span data-ttu-id="88ec0-155">Cree otra clase de inicio de OWIN y asígnele el nombre `TestStartup`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-155">Create another OWIN Startup class and name it `TestStartup`.</span></span>
5. <span data-ttu-id="88ec0-156">Reemplace el código generado con el siguiente:</span><span class="sxs-lookup"><span data-stu-id="88ec0-156">Replace the generated code with the following:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample10.cs?highlight=6,14-18)]

 <span data-ttu-id="88ec0-157">El `OwinStartup` sobrecarga de atributo anterior especifica `TestingConfiguration` como el *descriptivo* nombre de la clase de inicio.</span><span class="sxs-lookup"><span data-stu-id="88ec0-157">The `OwinStartup` attribute overload above specifies `TestingConfiguration` as the *friendly* name of the Startup class.</span></span>
6. <span data-ttu-id="88ec0-158">Abra la *web.config* de archivos y agregar la clave de inicio de la aplicación OWIN que especifica el nombre descriptivo de la clase de inicio:</span><span class="sxs-lookup"><span data-stu-id="88ec0-158">Open the *web.config* file and add the OWIN App startup key which specifies the friendly name of the Startup class:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample11.xml?highlight=3-5)]
7. <span data-ttu-id="88ec0-159">Presione F5 de Control para ejecutar la aplicación.</span><span class="sxs-lookup"><span data-stu-id="88ec0-159">Press Control F5 to run the app.</span></span> <span data-ttu-id="88ec0-160">El elemento de configuración de aplicación tiene prioridad y la prueba se ejecuta la configuración.</span><span class="sxs-lookup"><span data-stu-id="88ec0-160">The app settings element takes precedent, and the test configuration is run.</span></span>  
  
    ![](owin-startup-class-detection/_static/image7.png)
8. <span data-ttu-id="88ec0-161">Quitar el *descriptivo* nombre de la `OwinStartup` de atributo en la `TestStartup` clase.</span><span class="sxs-lookup"><span data-stu-id="88ec0-161">Remove the *friendly* name from the `OwinStartup` attribute in the `TestStartup` class.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample12.cs)]
9. <span data-ttu-id="88ec0-162">Reemplazar la clave de inicio de la aplicación OWIN en el *web.config* archivo con lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="88ec0-162">Replace the OWIN App startup key in the *web.config* file with the following:</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample13.xml)]
10. <span data-ttu-id="88ec0-163">Revertir la `OwinStartup` atributo de cada clase en el código de atributo predeterminado generado por Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="88ec0-163">Revert the `OwinStartup` attribute in each class to the default attribute code generated by Visual Studio:</span></span>  

    [!code-csharp[Main](owin-startup-class-detection/samples/sample14.cs)]

 <span data-ttu-id="88ec0-164">Cada una de las claves de inicio de la aplicación OWIN siguientes hará que la clase de producción para que se ejecute.</span><span class="sxs-lookup"><span data-stu-id="88ec0-164">Each of the OWIN App startup keys below will cause the production class to run.</span></span> 

    [!code-xml[Main](owin-startup-class-detection/samples/sample15.xml)]

 <span data-ttu-id="88ec0-165">La clave de inicio última especifica el método de configuración de inicio.</span><span class="sxs-lookup"><span data-stu-id="88ec0-165">The last startup key specifies the startup configuration method.</span></span> <span data-ttu-id="88ec0-166">La siguiente clave de inicio de OWIN aplicación le permite cambiar el nombre de la clase de configuración para `MyConfiguration` .</span><span class="sxs-lookup"><span data-stu-id="88ec0-166">The following OWIN App startup key allows you to change the name of the configuration class to `MyConfiguration` .</span></span>

    [!code-xml[Main](owin-startup-class-detection/samples/sample16.xml)]

## <a name="using-owinhostexe"></a><span data-ttu-id="88ec0-167">Usar Owinhost.exe</span><span class="sxs-lookup"><span data-stu-id="88ec0-167">Using Owinhost.exe</span></span>

1. <span data-ttu-id="88ec0-168">Reemplace el archivo Web.config con el marcado siguiente:</span><span class="sxs-lookup"><span data-stu-id="88ec0-168">Replace the Web.config file with the following markup:</span></span>  

    [!code-xml[Main](owin-startup-class-detection/samples/sample17.xml?highlight=3-6)]

 <span data-ttu-id="88ec0-169">La última clave wins, por lo que en este caso `TestStartup` se especifica.</span><span class="sxs-lookup"><span data-stu-id="88ec0-169">The last key wins, so in this case `TestStartup` is specified.</span></span>
2. <span data-ttu-id="88ec0-170">Instalar Owinhost desde el PMC:</span><span class="sxs-lookup"><span data-stu-id="88ec0-170">Install Owinhost from the PMC:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample18.cmd)]
3. <span data-ttu-id="88ec0-171">Vaya a la carpeta de la aplicación (la carpeta que contiene el *Web.config* archivo) y en un símbolo del sistema y escriba:</span><span class="sxs-lookup"><span data-stu-id="88ec0-171">Navigate to the application folder (the folder containing the *Web.config* file) and in a command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample19.cmd)]

 <span data-ttu-id="88ec0-172">Muestra la ventana de comandos:</span><span class="sxs-lookup"><span data-stu-id="88ec0-172">The command window will show:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample20.cmd)]
4. <span data-ttu-id="88ec0-173">Iniciar un explorador con la dirección URL `http://localhost:5000/`.</span><span class="sxs-lookup"><span data-stu-id="88ec0-173">Launch a browser with the URL `http://localhost:5000/`.</span></span>  
  
    ![](owin-startup-class-detection/_static/image8.png)  
  
 <span data-ttu-id="88ec0-174">OwinHost respeta las convenciones de inicio mencionadas anteriormente.</span><span class="sxs-lookup"><span data-stu-id="88ec0-174">OwinHost honored the startup conventions listed above.</span></span>
5. <span data-ttu-id="88ec0-175">En la ventana de comandos, presione ENTRAR para salir OwinHost.</span><span class="sxs-lookup"><span data-stu-id="88ec0-175">In the command window, press Enter to exit OwinHost.</span></span>
6. <span data-ttu-id="88ec0-176">En el `ProductionStartup` de clases, agregue el siguiente atributo OwinStartup que especifica un nombre descriptivo de *ProductionConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="88ec0-176">In the `ProductionStartup` class, add the following OwinStartup attribute which specifies a friendly name of *ProductionConfiguration*.</span></span>

    [!code-csharp[Main](owin-startup-class-detection/samples/sample21.cs)]
7. <span data-ttu-id="88ec0-177">En el símbolo del sistema y escriba:</span><span class="sxs-lookup"><span data-stu-id="88ec0-177">In the command prompt and type:</span></span> 

    [!code-console[Main](owin-startup-class-detection/samples/sample22.cmd)]

 <span data-ttu-id="88ec0-178">Se carga la clase de inicio de producción.</span><span class="sxs-lookup"><span data-stu-id="88ec0-178">The Production startup class is loaded.</span></span>  
    ![](owin-startup-class-detection/_static/image9.png)  
 <span data-ttu-id="88ec0-179">Nuestra aplicación tiene varias clases de inicio y, en este ejemplo se ha aplazado qué clase de inicio para cargar hasta el tiempo de ejecución.</span><span class="sxs-lookup"><span data-stu-id="88ec0-179">Our application has multiple startup classes, and in this example we have deferred which startup class to load until runtime.</span></span>
8. <span data-ttu-id="88ec0-180">Pruebe las siguientes opciones de inicio en tiempo de ejecución:</span><span class="sxs-lookup"><span data-stu-id="88ec0-180">Test the following runtime startup options:</span></span>

    [!code-console[Main](owin-startup-class-detection/samples/sample23.cmd)]
