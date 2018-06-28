# <a name="custom-webhost-service-sample"></a><span data-ttu-id="bcbd0-101">Ejemplo de servicio personalizado de WebHost</span><span class="sxs-lookup"><span data-stu-id="bcbd0-101">Custom WebHost Service Sample</span></span>

<span data-ttu-id="bcbd0-102">En este ejemplo se muestra cómo hospedar una aplicación de ASP.NET Core como un servicio de Windows sin usar IIS.</span><span class="sxs-lookup"><span data-stu-id="bcbd0-102">This sample shows how to host an ASP.NET Core app as a Windows Service without using IIS.</span></span> <span data-ttu-id="bcbd0-103">Se centra en el escenario descrito en [Hospedaje de ASP.NET Core en un servicio de Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="bcbd0-103">This sample demonstrates the scenario described in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

## <a name="instructions"></a><span data-ttu-id="bcbd0-104">Instrucciones</span><span class="sxs-lookup"><span data-stu-id="bcbd0-104">Instructions</span></span>

<span data-ttu-id="bcbd0-105">La aplicación de ejemplo es una aplicación web de Razor Pages modificada según las instrucciones de [Hospedaje de ASP.NET Core en un servicio de Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span><span class="sxs-lookup"><span data-stu-id="bcbd0-105">The sample app is a Razor Pages web app modified according to the instructions in [Host an ASP.NET Core app in a Windows Service](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).</span></span>

<span data-ttu-id="bcbd0-106">Para ejecutar la aplicación en un servicio, realice los pasos siguientes:</span><span class="sxs-lookup"><span data-stu-id="bcbd0-106">To run the app in a service, perform the following steps:</span></span>

1. <span data-ttu-id="bcbd0-107">Cree una carpeta en *c:\svc*.</span><span class="sxs-lookup"><span data-stu-id="bcbd0-107">Create a folder at *c:\svc*.</span></span>

1. <span data-ttu-id="bcbd0-108">Publique la aplicación en la carpeta con `dotnet publish --configuration Release --output c:\\svc`.</span><span class="sxs-lookup"><span data-stu-id="bcbd0-108">Publish the app to the folder with `dotnet publish --configuration Release --output c:\\svc`.</span></span> <span data-ttu-id="bcbd0-109">El comando mueve los recursos de la aplicación a la carpeta *svc*, incluido el archivo `appsettings.json` necesario y la carpeta `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="bcbd0-109">The command moves the app's assets to the *svc* folder, including the required `appsettings.json` file and the `wwwroot` folder.</span></span>

1. <span data-ttu-id="bcbd0-110">Abra un símbolo del sistema de **administrador**.</span><span class="sxs-lookup"><span data-stu-id="bcbd0-110">Open an **administrator** command prompt.</span></span>

1. <span data-ttu-id="bcbd0-111">Ejecute los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="bcbd0-111">Execute the following commands:</span></span>

   ```console
   sc create MyService binPath= "c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

  <span data-ttu-id="bcbd0-112">*El espacio entre el signo igual y el inicio de la cadena de ruta de acceso es obligatorio.*</span><span class="sxs-lookup"><span data-stu-id="bcbd0-112">*The space between the equal sign and the start of the path string is required.*</span></span>

1. <span data-ttu-id="bcbd0-113">En un explorador, vaya a `http://localhost:5000` y compruebe que el servicio se esté ejecutando.</span><span class="sxs-lookup"><span data-stu-id="bcbd0-113">In a browser, navigate to `http://localhost:5000` and verify that the service is running.</span></span> <span data-ttu-id="bcbd0-114">La aplicación redirige al punto de conexión seguro `https://localhost:5001`.</span><span class="sxs-lookup"><span data-stu-id="bcbd0-114">The app redirects to the secure endpoint `https://localhost:5001`.</span></span>

1. <span data-ttu-id="bcbd0-115">Para detener el servicio, use el comando:</span><span class="sxs-lookup"><span data-stu-id="bcbd0-115">To stop the service, use the command:</span></span>

   ```console
   sc stop MyService
   ```

<span data-ttu-id="bcbd0-116">Si la aplicación no se inicia del modo previsto, una forma rápida de poder acceder a los mensajes de error consiste en agregar un proveedor de registro, como el [proveedor de registros de eventos de Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span><span class="sxs-lookup"><span data-stu-id="bcbd0-116">If the app doesn't start as expected, a quick way to make error messages accessible is to add a logging provider, such as the [Windows EventLog provider](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog).</span></span> <span data-ttu-id="bcbd0-117">Otra opción consiste en comprobar el registro de eventos de la aplicación con el Visor de eventos en el sistema.</span><span class="sxs-lookup"><span data-stu-id="bcbd0-117">Another option is to check the Application Event Log using the Event Viewer on the system.</span></span> <span data-ttu-id="bcbd0-118">Por ejemplo, esta es una excepción no controlada de un error de FileNotFound en el registro de eventos de la aplicación:</span><span class="sxs-lookup"><span data-stu-id="bcbd0-118">For example, here's an unhandled exception for a FileNotFound error in the Application Event Log:</span></span>

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
