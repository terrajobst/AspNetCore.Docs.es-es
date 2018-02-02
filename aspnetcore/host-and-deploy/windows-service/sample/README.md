# <a name="custom-webhost-service-sample"></a><span data-ttu-id="95090-101">Ejemplo de servicio personalizado de WebHost</span><span class="sxs-lookup"><span data-stu-id="95090-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="95090-102">Este ejemplo muestra la manera recomendada para hospedar una aplicación de ASP.NET Core en Windows sin utilizar IIS como un servicio de Windows.</span><span class="sxs-lookup"><span data-stu-id="95090-102">This sample shows the recommended way to host an ASP.NET Core app on Windows without using IIS as a Windows Service.</span></span> <span data-ttu-id="95090-103">Este ejemplo muestra las características descritas en [hospedar una aplicación de ASP.NET Core en un servicio de Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="95090-103">This sample demonstrates the features described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="95090-104">Instrucciones</span><span class="sxs-lookup"><span data-stu-id="95090-104">Instructions</span></span>

<span data-ttu-id="95090-105">La aplicación de ejemplo es una aplicación web MVC simple modificada según las instrucciones de [hospedar una aplicación de ASP.NET Core en un servicio de Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="95090-105">The sample app is a simple MVC web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="95090-106">Para ejecutar la aplicación en un servicio, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="95090-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="95090-107">Crear una carpeta en *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="95090-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="95090-108">Publique la aplicación en la carpeta con `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="95090-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="95090-109">El comando moverá activos de la aplicación a la carpeta, incluidos los necesarios `appsettings.json` archivo y `wwwroot` carpeta con su contenido.</span><span class="sxs-lookup"><span data-stu-id="95090-109">The command will move the app's assets to the folder, including the required `appsettings.json` file and the `wwwroot` folder with its contents.</span></span>

1. <span data-ttu-id="95090-110">Abra un **administrador** shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="95090-110">Open an **administrator** command shell.</span></span>

1. <span data-ttu-id="95090-111">Ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="95090-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. <span data-ttu-id="95090-112">En un explorador, vaya a `http://localhost:5000` para comprobar que el servicio se está ejecutando.</span><span class="sxs-lookup"><span data-stu-id="95090-112">In a browser, go to `http://localhost:5000` to verify that the service is running.</span></span>

1. <span data-ttu-id="95090-113">Para detener el servicio, use el comando:</span><span class="sxs-lookup"><span data-stu-id="95090-113">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="95090-114">Si la aplicación no se inicia como se esperaba cuando se ejecuta en un servicio, es una forma rápida de hacer accesibles los mensajes de error agregar un proveedor de registro, como el [proveedor de registro de eventos de Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="95090-114">If the app doesn't start up as expected when running in a service, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="95090-115">Otra opción consiste en comprobar el registro de eventos de aplicación con el Visor de eventos en el sistema.</span><span class="sxs-lookup"><span data-stu-id="95090-115">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="95090-116">Por ejemplo, aquí es una excepción no controlada para un error de FileNotFound en el registro de eventos de aplicación:</span><span class="sxs-lookup"><span data-stu-id="95090-116">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

```console
Application: AspNetCoreService.exe
Framework Version: v4.0.30319
Description: The process was terminated due to an unhandled exception.
Exception Info: System.IO.FileNotFoundException
   at Microsoft.Extensions.Configuration.FileConfigurationProvider.Load(Boolean)
   at Microsoft.Extensions.Configuration.ConfigurationRoot..ctor(System.Collections.Generic.IList`1<Microsoft.Extensions.Configuration.IConfigurationProvider>)
   at Microsoft.Extensions.Configuration.ConfigurationBuilder.Build()
   ...
```
