---
title: Hospedaje de ASP.NET Core en un servicio de Windows
author: guardrex
description: Aprenda a hospedar una aplicación ASP.NET Core en un servicio de Windows.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/30/2018
uid: host-and-deploy/windows-service
ms.openlocfilehash: 11913019bfe5d06c259b806fce9cc580a8280ad5
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758198"
---
# <a name="host-aspnet-core-in-a-windows-service"></a><span data-ttu-id="bd31c-103">Hospedaje de ASP.NET Core en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="bd31c-103">Host ASP.NET Core in a Windows Service</span></span>

<span data-ttu-id="bd31c-104">Por [Luke Latham](https://github.com/guardrex) y [Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="bd31c-104">By [Luke Latham](https://github.com/guardrex) and [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="bd31c-105">Una aplicación de ASP.NET Core se puede hospedar en Windows sin usar IIS como [servicio de Windows](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span><span class="sxs-lookup"><span data-stu-id="bd31c-105">An ASP.NET Core app can be hosted on Windows without using IIS as a [Windows Service](/dotnet/framework/windows-services/introduction-to-windows-service-applications).</span></span> <span data-ttu-id="bd31c-106">Cuando se aloja como un servicio de Windows, la aplicación se inicia automáticamente después de reiniciar el equipo.</span><span class="sxs-lookup"><span data-stu-id="bd31c-106">When hosted as a Windows Service, the app automatically starts after reboots.</span></span>

<span data-ttu-id="bd31c-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bd31c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/windows-service/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="convert-a-project-into-a-windows-service"></a><span data-ttu-id="bd31c-108">Convertir un proyecto en un servicio de Windows</span><span class="sxs-lookup"><span data-stu-id="bd31c-108">Convert a project into a Windows Service</span></span>

<span data-ttu-id="bd31c-109">Estos son los cambios mínimos necesarios para configurar un proyecto de ASP.NET Core existente para que se ejecute como un servicio:</span><span class="sxs-lookup"><span data-stu-id="bd31c-109">The following minimum changes are required to set up an existing ASP.NET Core project to run as a service:</span></span>

1. <span data-ttu-id="bd31c-110">El archivo del proyecto:</span><span class="sxs-lookup"><span data-stu-id="bd31c-110">In the project file:</span></span>

   * <span data-ttu-id="bd31c-111">Confirme la presencia del [identificador en tiempo de ejecución](/dotnet/core/rid-catalog) de Windows o agréguelo al `<PropertyGroup>` que contiene la plataforma de destino:</span><span class="sxs-lookup"><span data-stu-id="bd31c-111">Confirm the presence of a Windows [Runtime Identifier (RID)](/dotnet/core/rid-catalog) or add it to the `<PropertyGroup>` that contains the target framework:</span></span>

      ```xml
      <PropertyGroup>
        <TargetFramework>netcoreapp2.2</TargetFramework>
        <RuntimeIdentifier>win7-x64</RuntimeIdentifier>
      </PropertyGroup>
      ```

      <span data-ttu-id="bd31c-112">Para publicar para varios RID:</span><span class="sxs-lookup"><span data-stu-id="bd31c-112">To publish for multiple RIDs:</span></span>

      * <span data-ttu-id="bd31c-113">Proporcione los RID en una lista delimitada por punto y coma.</span><span class="sxs-lookup"><span data-stu-id="bd31c-113">Provide the RIDs in a semicolon-delimited list.</span></span>
      * <span data-ttu-id="bd31c-114">Use el nombre de la propiedad `<RuntimeIdentifiers>` (plural).</span><span class="sxs-lookup"><span data-stu-id="bd31c-114">Use the property name `<RuntimeIdentifiers>` (plural).</span></span>

      <span data-ttu-id="bd31c-115">Para más información, vea el [Catálogo de identificadores de entorno de ejecución (RID) de .NET Core](/dotnet/core/rid-catalog).</span><span class="sxs-lookup"><span data-stu-id="bd31c-115">For more information, see [.NET Core RID Catalog](/dotnet/core/rid-catalog).</span></span>

   * <span data-ttu-id="bd31c-116">Agregue una referencia de paquete de [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span><span class="sxs-lookup"><span data-stu-id="bd31c-116">Add a package reference for [Microsoft.AspNetCore.Hosting.WindowsServices](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.WindowsServices).</span></span>

1. <span data-ttu-id="bd31c-117">Realice los siguientes cambios en `Program.Main`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-117">Make the following changes in `Program.Main`:</span></span>

   * <span data-ttu-id="bd31c-118">Llame a [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) en lugar de a `host.Run`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-118">Call [host.RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice) instead of `host.Run`.</span></span>

   * <span data-ttu-id="bd31c-119">Llame a [UseContentRoot](xref:fundamentals/host/web-host#content-root) y use una ruta de acceso a la ubicación de publicación de la aplicación en lugar de `Directory.GetCurrentDirectory()`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-119">Call [UseContentRoot](xref:fundamentals/host/web-host#content-root) and use a path to the app's published location instead of `Directory.GetCurrentDirectory()`.</span></span>

     [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOnly&highlight=8-9,16)]

1. <span data-ttu-id="bd31c-120">Publique la aplicación con [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), un [perfil de publicación de Visual Studio](xref:host-and-deploy/visual-studio-publish-profiles) o Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="bd31c-120">Publish the app using [dotnet publish](/dotnet/articles/core/tools/dotnet-publish), a [Visual Studio publish profile](xref:host-and-deploy/visual-studio-publish-profiles), or Visual Studio Code.</span></span> <span data-ttu-id="bd31c-121">Al usar Visual Studio, seleccione **FolderProfile** y configure la **Ubicación de destino** antes de seleccionar el botón **Publicar**.</span><span class="sxs-lookup"><span data-stu-id="bd31c-121">When using Visual Studio, select the **FolderProfile** and configure the **Target Location** before selecting the **Publish** button.</span></span>

   <span data-ttu-id="bd31c-122">Para publicar la aplicación de ejemplo mediante herramientas de la interfaz de la línea de comandos (CLI), ejecute el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) en un símbolo del sistema desde la carpeta del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bd31c-122">To publish the sample app using command-line interface (CLI) tools, run the [dotnet publish](/dotnet/core/tools/dotnet-publish) command at a command prompt from the project folder.</span></span> <span data-ttu-id="bd31c-123">El identificador relativo debe especificarse en la propiedad `<RuntimeIdenfifier>` o `<RuntimeIdentifiers>` del archivo del proyecto.</span><span class="sxs-lookup"><span data-stu-id="bd31c-123">The RID must be specified in the `<RuntimeIdenfifier>` (or `<RuntimeIdentifiers>`) property of the project file.</span></span> <span data-ttu-id="bd31c-124">En el ejemplo siguiente, la aplicación se publica en la configuración de lanzamiento para `win7-x64` en tiempo de ejecución en una carpeta creada en *c:\\svc*:</span><span class="sxs-lookup"><span data-stu-id="bd31c-124">In the following example, the app is published in Release configuration for the `win7-x64` runtime to a folder created at *c:\\svc*:</span></span>

   ```console
   dotnet publish --configuration Release --runtime win7-x64 --output c:\svc
   ```

1. <span data-ttu-id="bd31c-125">Cree una cuenta de usuario para el servicio con el comando `net user`:</span><span class="sxs-lookup"><span data-stu-id="bd31c-125">Create a user account for the service using the `net user` command:</span></span>

   ```console
   net user {USER ACCOUNT} {PASSWORD} /add
   ```

   <span data-ttu-id="bd31c-126">Para la aplicación de ejemplo, cree una cuenta de usuario con el nombre `ServiceUser` y una contraseña.</span><span class="sxs-lookup"><span data-stu-id="bd31c-126">For the sample app, create a user account with the name `ServiceUser` and a password.</span></span> <span data-ttu-id="bd31c-127">En el comando siguiente, reemplace `{PASSWORD}` por una [contraseña segura](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span><span class="sxs-lookup"><span data-stu-id="bd31c-127">In the following command, replace `{PASSWORD}` with a [strong password](/windows/security/threat-protection/security-policy-settings/password-must-meet-complexity-requirements).</span></span>

   ```console
   net user ServiceUser {PASSWORD} /add
   ```

   <span data-ttu-id="bd31c-128">Si necesita agregar el usuario a un grupo, use el comando `net localgroup`, donde `{GROUP}` es el nombre del grupo:</span><span class="sxs-lookup"><span data-stu-id="bd31c-128">If you need to add the user to a group, use the `net localgroup` command, where `{GROUP}` is the name of the group:</span></span>

   ```console
   net localgroup {GROUP} {USER ACCOUNT} /add
   ```

   <span data-ttu-id="bd31c-129">Para más información, vea [Service User Accounts](/windows/desktop/services/service-user-accounts) (Cuentas de usuario de servicio).</span><span class="sxs-lookup"><span data-stu-id="bd31c-129">For more information, see [Service User Accounts](/windows/desktop/services/service-user-accounts).</span></span>

1. <span data-ttu-id="bd31c-130">Conceda acceso de escritura, lectura y ejecución a la carpeta de la aplicación con el comando [icacls](/windows-server/administration/windows-commands/icacls):</span><span class="sxs-lookup"><span data-stu-id="bd31c-130">Grant write/read/execute access to the app's folder using the [icacls](/windows-server/administration/windows-commands/icacls) command:</span></span>

   ```console
   icacls "{PATH}" /grant {USER ACCOUNT}:(OI)(CI){PERMISSION FLAGS} /t
   ```

   * <span data-ttu-id="bd31c-131">`{PATH}`: ruta de acceso a la carpeta de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bd31c-131">`{PATH}` &ndash; Path to the app's folder.</span></span>
   * <span data-ttu-id="bd31c-132">`{USER ACCOUNT}`: cuenta del usuario (SID).</span><span class="sxs-lookup"><span data-stu-id="bd31c-132">`{USER ACCOUNT}` &ndash; The user account (SID).</span></span>
   * <span data-ttu-id="bd31c-133">`(OI)`: la marca Object Inherit (Heredar objetos) propaga los permisos a los archivos subordinados.</span><span class="sxs-lookup"><span data-stu-id="bd31c-133">`(OI)` &ndash; The Object Inherit flag propagates permissions to subordinate files.</span></span>
   * <span data-ttu-id="bd31c-134">`(CI)`: la marca Container Inherit (Heredar contenedores) propaga los permisos a las carpetas subordinadas.</span><span class="sxs-lookup"><span data-stu-id="bd31c-134">`(CI)` &ndash; The Container Inherit flag propagates permissions to subordinate folders.</span></span>
   * <span data-ttu-id="bd31c-135">`{PERMISSION FLAGS}`: define los permisos de acceso a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bd31c-135">`{PERMISSION FLAGS}` &ndash; Sets the app's access permissions.</span></span>
     * <span data-ttu-id="bd31c-136">Escritura (`W`)</span><span class="sxs-lookup"><span data-stu-id="bd31c-136">Write (`W`)</span></span>
     * <span data-ttu-id="bd31c-137">Lectura (`R`)</span><span class="sxs-lookup"><span data-stu-id="bd31c-137">Read (`R`)</span></span>
     * <span data-ttu-id="bd31c-138">Ejecución (`X`)</span><span class="sxs-lookup"><span data-stu-id="bd31c-138">Execute (`X`)</span></span>
     * <span data-ttu-id="bd31c-139">Total (`F`)</span><span class="sxs-lookup"><span data-stu-id="bd31c-139">Full (`F`)</span></span>
     * <span data-ttu-id="bd31c-140">Modificación (`M`)</span><span class="sxs-lookup"><span data-stu-id="bd31c-140">Modify (`M`)</span></span>
   * <span data-ttu-id="bd31c-141">`/t`: se aplica de forma recursiva a las carpetas y los archivos subordinados existentes.</span><span class="sxs-lookup"><span data-stu-id="bd31c-141">`/t` &ndash; Apply recursively to existing subordinate folders and files.</span></span>

   <span data-ttu-id="bd31c-142">Para la aplicación de ejemplo publicada en la carpeta *c:\\svc* y en la cuenta `ServiceUser` con permisos de escritura, lectura y ejecución, use el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="bd31c-142">For the sample app published to the *c:\\svc* folder and the `ServiceUser` account with write/read/execute permissions, use the following command:</span></span>

   ```console
   icacls "c:\svc" /grant ServiceUser:(OI)(CI)WRX /t
   ```

   <span data-ttu-id="bd31c-143">Para más información, vea [icacls](/windows-server/administration/windows-commands/icacls).</span><span class="sxs-lookup"><span data-stu-id="bd31c-143">For more information, see [icacls](/windows-server/administration/windows-commands/icacls).</span></span>

1. <span data-ttu-id="bd31c-144">Use la herramienta de línea de comandos [sc.exe](https://technet.microsoft.com/library/bb490995) para crear el servicio.</span><span class="sxs-lookup"><span data-stu-id="bd31c-144">Use the [sc.exe](https://technet.microsoft.com/library/bb490995) command-line tool to create the service.</span></span> <span data-ttu-id="bd31c-145">El valor `binPath` es la ruta de acceso al archivo ejecutable de la aplicación, que incluye el nombre del archivo ejecutable.</span><span class="sxs-lookup"><span data-stu-id="bd31c-145">The `binPath` value is the path to the app's executable, which includes the executable file name.</span></span> <span data-ttu-id="bd31c-146">**El espacio entre el signo igual y las comillas de cada parámetro y valor es obligatorio.**</span><span class="sxs-lookup"><span data-stu-id="bd31c-146">**The space between the equal sign and the quote character of each parameter and value is required.**</span></span>

   ```console
   sc create {SERVICE NAME} binPath= "{PATH}" obj= "{DOMAIN}\{USER ACCOUNT}" password= "{PASSWORD}"
   ```

   * <span data-ttu-id="bd31c-147">`{SERVICE NAME}`: nombre para asignar al servicio en el [Administrador de control de servicios](/windows/desktop/services/service-control-manager).</span><span class="sxs-lookup"><span data-stu-id="bd31c-147">`{SERVICE NAME}` &ndash; The name to assign to the service in [Service Control Manager](/windows/desktop/services/service-control-manager).</span></span>
   * <span data-ttu-id="bd31c-148">`{PATH}`: ruta de acceso al archivo ejecutable del servicio.</span><span class="sxs-lookup"><span data-stu-id="bd31c-148">`{PATH}` &ndash; The path to the service executable.</span></span>
   * <span data-ttu-id="bd31c-149">`{DOMAIN}`, o si la máquina no está unida a un dominio, el nombre de la máquina local, y `{USER ACCOUNT}`: el dominio o el nombre de la máquina local y la cuenta de usuario en los que el servicio se ejecuta.</span><span class="sxs-lookup"><span data-stu-id="bd31c-149">`{DOMAIN}` (or if the machine isn't domain joined, the local machine name) and `{USER ACCOUNT}` &ndash; The domain (or local machine name) and user account under which the service runs.</span></span> <span data-ttu-id="bd31c-150">No **omita** el parámetro `obj`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-150">Do **not** omit the `obj` parameter.</span></span> <span data-ttu-id="bd31c-151">El valor predeterminado de `obj` es la cuenta [LocalSystem](/windows/desktop/services/localsystem-account).</span><span class="sxs-lookup"><span data-stu-id="bd31c-151">The default value for `obj` is the [LocalSystem account](/windows/desktop/services/localsystem-account) account.</span></span> <span data-ttu-id="bd31c-152">Ejecutar un servicio en la cuenta `LocalSystem` plantea un riesgo de seguridad importante.</span><span class="sxs-lookup"><span data-stu-id="bd31c-152">Running a service under the `LocalSystem` account presents a significant security risk.</span></span> <span data-ttu-id="bd31c-153">Ejecute siempre un servicio en una cuenta de usuario con privilegios restringidos en el servidor.</span><span class="sxs-lookup"><span data-stu-id="bd31c-153">Always run a service under a user account with restricted privileges on the server.</span></span>
   * <span data-ttu-id="bd31c-154">`{PASSWORD}`: contraseña de la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="bd31c-154">`{PASSWORD}` &ndash; The user account password.</span></span>

   <span data-ttu-id="bd31c-155">En el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="bd31c-155">In the following example:</span></span>

   * <span data-ttu-id="bd31c-156">El servicio se denomina **MyService**.</span><span class="sxs-lookup"><span data-stu-id="bd31c-156">The service is named **MyService**.</span></span>
   * <span data-ttu-id="bd31c-157">El servicio publicado reside en la carpeta *c:\\svc*.</span><span class="sxs-lookup"><span data-stu-id="bd31c-157">The published service resides in the *c:\\svc* folder.</span></span> <span data-ttu-id="bd31c-158">El ejecutable de la aplicación se denomina *AspNetCoreService.exe*.</span><span class="sxs-lookup"><span data-stu-id="bd31c-158">The app executable is named *AspNetCoreService.exe*.</span></span> <span data-ttu-id="bd31c-159">El valor `binPath` se encuentra entre comillas rectas (").</span><span class="sxs-lookup"><span data-stu-id="bd31c-159">The `binPath` value is enclosed in straight quotation marks (").</span></span>
   * <span data-ttu-id="bd31c-160">El servicio se ejecuta en la cuenta `ServiceUser`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-160">The service runs under the `ServiceUser` account.</span></span> <span data-ttu-id="bd31c-161">Reemplace `{DOMAIN}` por el dominio de la cuenta de usuario o el nombre de la máquina local.</span><span class="sxs-lookup"><span data-stu-id="bd31c-161">Replace `{DOMAIN}` with the user account's domain or local machine name.</span></span> <span data-ttu-id="bd31c-162">Encierre el valor `obj` entre comillas rectas (").</span><span class="sxs-lookup"><span data-stu-id="bd31c-162">Enclose the `obj` value in straight quotation marks (").</span></span> <span data-ttu-id="bd31c-163">Ejemplo: si el sistema de hospedaje es una máquina local denominada `MairaPC`, establezca `obj` en `"MairaPC\ServiceUser"`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-163">Example: If the hosting system is a local machine named `MairaPC`, set `obj` to `"MairaPC\ServiceUser"`.</span></span>
   * <span data-ttu-id="bd31c-164">Reemplace `{PASSWORD}` con la contraseña de la cuenta de usuario.</span><span class="sxs-lookup"><span data-stu-id="bd31c-164">Replace `{PASSWORD}` with the user account's password.</span></span> <span data-ttu-id="bd31c-165">El valor `password` se encuentra entre comillas rectas (").</span><span class="sxs-lookup"><span data-stu-id="bd31c-165">The `password` value is enclosed in straight quotation marks (").</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe" obj= "{DOMAIN}\ServiceUser" password= "{PASSWORD}"
   ```

   > [!IMPORTANT]
   > <span data-ttu-id="bd31c-166">Asegúrese de que existen los espacios entre los signos igual de los parámetros y los valores de los parámetros.</span><span class="sxs-lookup"><span data-stu-id="bd31c-166">Make sure that the spaces between the parameters' equal signs and the parameters' values are present.</span></span>

1. <span data-ttu-id="bd31c-167">Inicie el servicio con el comando `sc start {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-167">Start the service with the `sc start {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="bd31c-168">Use el siguiente comando para iniciar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bd31c-168">To start the sample app service, use the following command:</span></span>

   ```console
   sc start MyService
   ```

   <span data-ttu-id="bd31c-169">Este comando tarda unos segundos en iniciar el servicio.</span><span class="sxs-lookup"><span data-stu-id="bd31c-169">The command takes a few seconds to start the service.</span></span>

1. <span data-ttu-id="bd31c-170">Para comprobar el estado del servicio, use el comando `sc query {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-170">To check the status of the service, use the `sc query {SERVICE NAME}` command.</span></span> <span data-ttu-id="bd31c-171">El estado se notifica como uno de los siguientes valores:</span><span class="sxs-lookup"><span data-stu-id="bd31c-171">The status is reported as one of the following values:</span></span>

   * `START_PENDING`
   * `RUNNING`
   * `STOP_PENDING`
   * `STOPPED`

   <span data-ttu-id="bd31c-172">Use el siguiente comando para comprobar el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bd31c-172">Use the following command to check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

1. <span data-ttu-id="bd31c-173">Si el servicio está en estado `RUNNING` y dicho servicio es una aplicación web, vaya a la aplicación en su ruta de acceso correspondiente (de forma predeterminada, `http://localhost:5000`, que redirige a `https://localhost:5001` cuando se usa el [Middleware de redirección de HTTPS](xref:security/enforcing-ssl)).</span><span class="sxs-lookup"><span data-stu-id="bd31c-173">When the service is in the `RUNNING` state and if the service is a web app, browse the app at its path (by default, `http://localhost:5000`, which redirects to `https://localhost:5001` when using [HTTPS Redirection Middleware](xref:security/enforcing-ssl)).</span></span>

   <span data-ttu-id="bd31c-174">Si es el servicio de la aplicación de ejemplo, vaya a la aplicación en `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-174">For the sample app service, browse the app at `http://localhost:5000`.</span></span>

1. <span data-ttu-id="bd31c-175">Detenga el servicio con el comando `sc stop {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-175">Stop the service with the `sc stop {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="bd31c-176">Con el siguiente comando se detiene el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bd31c-176">The following command stops the sample app service:</span></span>

   ```console
   sc stop MyService
   ```

1. <span data-ttu-id="bd31c-177">Tras un breve intervalo para detener un servicio, desinstale el servicio con el comando `sc delete {SERVICE NAME}`.</span><span class="sxs-lookup"><span data-stu-id="bd31c-177">After a short delay to stop a service, uninstall the service with the `sc delete {SERVICE NAME}` command.</span></span>

   <span data-ttu-id="bd31c-178">Compruebe el estado del servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bd31c-178">Check the status of the sample app service:</span></span>

   ```console
   sc query MyService
   ```

   <span data-ttu-id="bd31c-179">Si el servicio de la aplicación de ejemplo está en estado `STOPPED`, use el siguiente comando para desinstalar el servicio de la aplicación de ejemplo:</span><span class="sxs-lookup"><span data-stu-id="bd31c-179">When the sample app service is in the `STOPPED` state, use the following command to uninstall the sample app service:</span></span>

   ```console
   sc delete MyService
   ```

## <a name="run-the-app-outside-of-a-service"></a><span data-ttu-id="bd31c-180">Ejecutar la aplicación fuera de un servicio</span><span class="sxs-lookup"><span data-stu-id="bd31c-180">Run the app outside of a service</span></span>

<span data-ttu-id="bd31c-181">Probar y depurar una aplicación resulta más sencillo cuando se ejecuta fuera de un servicio, por lo que es habitual agregar código que llame a `RunAsService` solo bajo determinadas condiciones.</span><span class="sxs-lookup"><span data-stu-id="bd31c-181">It's easier to test and debug when running outside of a service, so it's customary to add code that calls `RunAsService` only under certain conditions.</span></span> <span data-ttu-id="bd31c-182">Por ejemplo, la aplicación se puede ejecutar como una aplicación de consola con un argumento de línea de comandos `--console` o si el depurador está asociado:</span><span class="sxs-lookup"><span data-stu-id="bd31c-182">For example, the app can run as a console app with a `--console` command-line argument or if the debugger is attached:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=ServiceOrConsole)]

<span data-ttu-id="bd31c-183">Dado que la configuración de ASP.NET Core requiere pares nombre-valor en los argumentos de línea de comandos, el conmutador `--console` se quita antes de que los argumentos se pasen a [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="bd31c-183">Because ASP.NET Core configuration requires name-value pairs for command-line arguments, the `--console` switch is removed before the arguments are passed to [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

> [!NOTE]
> <span data-ttu-id="bd31c-184">`isService` no se pasa de `Main` a `CreateWebHostBuilder` porque la firma de `CreateWebHostBuilder` debe ser `CreateWebHostBuilder(string[])` para que las [pruebas de integración](xref:test/integration-tests) funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="bd31c-184">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

## <a name="handle-stopping-and-starting-events"></a><span data-ttu-id="bd31c-185">Controlar los eventos de inicio y detención</span><span class="sxs-lookup"><span data-stu-id="bd31c-185">Handle stopping and starting events</span></span>

<span data-ttu-id="bd31c-186">Para controlar los eventos [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted) y [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping), haga los siguientes cambios adicionales:</span><span class="sxs-lookup"><span data-stu-id="bd31c-186">To handle [OnStarting](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarting), [OnStarted](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstarted), and [OnStopping](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice.onstopping) events, make the following additional changes:</span></span>

1. <span data-ttu-id="bd31c-187">Cree una clase que se derive de la clase [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span><span class="sxs-lookup"><span data-stu-id="bd31c-187">Create a class that derives from [WebHostService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=NoLogging)]

2. <span data-ttu-id="bd31c-188">Cree un método de extensión para [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) que pase el elemento `WebHostService` personalizado a [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span><span class="sxs-lookup"><span data-stu-id="bd31c-188">Create an extension method for [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) that passes the custom `WebHostService` to [ServiceBase.Run](/dotnet/api/system.serviceprocess.servicebase.run):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/WebHostServiceExtensions.cs?name=ExtensionsClass)]

3. <span data-ttu-id="bd31c-189">En `Program.Main`, llame al nuevo método de extensión, `RunAsCustomService`, en lugar de a [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span><span class="sxs-lookup"><span data-stu-id="bd31c-189">In `Program.Main`, call the new extension method, `RunAsCustomService`, instead of [RunAsService](/dotnet/api/microsoft.aspnetcore.hosting.windowsservices.webhostwindowsserviceextensions.runasservice):</span></span>

   [!code-csharp[](windows-service/samples/2.x/AspNetCoreService/Program.cs?name=HandleStopStart&highlight=17)]

   > [!NOTE]
   > <span data-ttu-id="bd31c-190">`isService` no se pasa de `Main` a `CreateWebHostBuilder` porque la firma de `CreateWebHostBuilder` debe ser `CreateWebHostBuilder(string[])` para que las [pruebas de integración](xref:test/integration-tests) funcionen correctamente.</span><span class="sxs-lookup"><span data-stu-id="bd31c-190">`isService` isn't passed from `Main` into `CreateWebHostBuilder` because the signature of `CreateWebHostBuilder` must be `CreateWebHostBuilder(string[])` in order for [integration testing](xref:test/integration-tests) to work properly.</span></span>

<span data-ttu-id="bd31c-191">Si el código `WebHostService` personalizado requiere un servicio de inserción de dependencias (como un registrador), obténgalo de la propiedad [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services):</span><span class="sxs-lookup"><span data-stu-id="bd31c-191">If the custom `WebHostService` code requires a service from dependency injection (such as a logger), obtain it from the [IWebHost.Services](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost.services) property:</span></span>

[!code-csharp[](windows-service/samples/2.x/AspNetCoreService/CustomWebHostService.cs?name=Logging&highlight=7-8)]

## <a name="proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="bd31c-192">Escenarios de servidor proxy y equilibrador de carga</span><span class="sxs-lookup"><span data-stu-id="bd31c-192">Proxy server and load balancer scenarios</span></span>

<span data-ttu-id="bd31c-193">Los servicios que interactúan con las solicitudes de Internet o de una red corporativa y están detrás de un proxy o de un equilibrador de carga podrían requerir configuración adicional.</span><span class="sxs-lookup"><span data-stu-id="bd31c-193">Services that interact with requests from the Internet or a corporate network and are behind a proxy or load balancer might require additional configuration.</span></span> <span data-ttu-id="bd31c-194">Para obtener más información, vea <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="bd31c-194">For more information, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

## <a name="configure-https"></a><span data-ttu-id="bd31c-195">Configuración de HTTPS</span><span class="sxs-lookup"><span data-stu-id="bd31c-195">Configure HTTPS</span></span>

<span data-ttu-id="bd31c-196">Para configurar el servicio con un punto de conexión seguro:</span><span class="sxs-lookup"><span data-stu-id="bd31c-196">To configure the service with a secure endpoint:</span></span>

1. <span data-ttu-id="bd31c-197">Cree un certificado X.509 para el sistema de hospedaje mediante la adquisición del certificado de la plataforma y con mecanismos de implementación.</span><span class="sxs-lookup"><span data-stu-id="bd31c-197">Create an X.509 certificate for the hosting system using your platform's certificate acquisition and deployment mechanisms.</span></span>
1. <span data-ttu-id="bd31c-198">Especifique una [configuración de punto de conexión HTTPS de servidor Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) para usar el certificado.</span><span class="sxs-lookup"><span data-stu-id="bd31c-198">Specify a [Kestrel server HTTPS endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) to use the certificate.</span></span>

<span data-ttu-id="bd31c-199">No se admite el uso del certificado de desarrollo HTTPS de ASP.NET Core para proteger un punto de conexión de servicio.</span><span class="sxs-lookup"><span data-stu-id="bd31c-199">Use of the ASP.NET Core HTTPS development certificate to secure a service endpoint isn't supported.</span></span>

## <a name="current-directory-and-content-root"></a><span data-ttu-id="bd31c-200">Directorio actual y raíz del contenido</span><span class="sxs-lookup"><span data-stu-id="bd31c-200">Current directory and content root</span></span>

<span data-ttu-id="bd31c-201">El directorio de trabajo actual devuelto por una llamada a `Directory.GetCurrentDirectory()` para un servicio de Windows es la carpeta *C:\\WINDOWS\\system32*.</span><span class="sxs-lookup"><span data-stu-id="bd31c-201">The current working directory returned by calling `Directory.GetCurrentDirectory()` for a Windows Service is the *C:\\WINDOWS\\system32* folder.</span></span> <span data-ttu-id="bd31c-202">La carpeta *system32* no es una ubicación adecuada para almacenar los archivos de un servicio (por ejemplo los archivos de configuración).</span><span class="sxs-lookup"><span data-stu-id="bd31c-202">The *system32* folder isn't a suitable location to store a service's files (for example, settings files).</span></span> <span data-ttu-id="bd31c-203">Use uno de los enfoques siguientes para mantener y acceder a los recursos y los archivos de configuración de un servicio con [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) cuando se usa una interfaz [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span><span class="sxs-lookup"><span data-stu-id="bd31c-203">Use one of the following approaches to maintain and access a service's assets and settings files with [FileConfigurationExtensions.SetBasePath](/dotnet/api/microsoft.extensions.configuration.fileconfigurationextensions.setbasepath) when using an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder):</span></span>

* <span data-ttu-id="bd31c-204">Use la ruta de acceso raíz del contenido.</span><span class="sxs-lookup"><span data-stu-id="bd31c-204">Use the content root path.</span></span> <span data-ttu-id="bd31c-205">`IHostingEnvironment.ContentRootPath` es la misma ruta de acceso proporcionada al argumento `binPath` cuando se crea el servicio.</span><span class="sxs-lookup"><span data-stu-id="bd31c-205">The `IHostingEnvironment.ContentRootPath` is the same path provided to the `binPath` argument when the service is created.</span></span> <span data-ttu-id="bd31c-206">En lugar de usar `Directory.GetCurrentDirectory()` para crear rutas de acceso a los archivos de configuración, use la ruta de acceso raíz del contenido y mantenga los archivos en la raíz de contenido de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="bd31c-206">Instead of using `Directory.GetCurrentDirectory()` to create paths to settings files, use the content root path and maintain the files in the app's content root.</span></span>
* <span data-ttu-id="bd31c-207">Almacene los archivos en una ubicación adecuada en el disco.</span><span class="sxs-lookup"><span data-stu-id="bd31c-207">Store the files in a suitable location on disk.</span></span> <span data-ttu-id="bd31c-208">Especifique una ruta de acceso absoluta con `SetBasePath` a la carpeta que contiene los archivos.</span><span class="sxs-lookup"><span data-stu-id="bd31c-208">Specify an absolute path with `SetBasePath` to the folder containing the files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd31c-209">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="bd31c-209">Additional resources</span></span>

* <span data-ttu-id="bd31c-210">[Kestrel: configuración de punto de conexión](xref:fundamentals/servers/kestrel#endpoint-configuration) (configuración de HTTPS y compatibilidad de SNI incluidas)</span><span class="sxs-lookup"><span data-stu-id="bd31c-210">[Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) (includes HTTPS configuration and SNI support)</span></span>
* <xref:fundamentals/host/web-host>
