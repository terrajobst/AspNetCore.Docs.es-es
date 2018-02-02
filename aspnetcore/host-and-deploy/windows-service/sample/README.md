# <a name="custom-webhost-service-sample"></a>Ejemplo de servicio personalizado de WebHost

Este ejemplo muestra la manera recomendada para hospedar una aplicación de ASP.NET Core en Windows sin utilizar IIS como un servicio de Windows. Este ejemplo muestra las características descritas en [hospedar una aplicación de ASP.NET Core en un servicio de Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

## <a name="instructions"></a>Instrucciones

La aplicación de ejemplo es una aplicación web MVC simple modificada según las instrucciones de [hospedar una aplicación de ASP.NET Core en un servicio de Windows](https://docs.microsoft.com/aspnet/core/host-and-deploy/windows-service).

Para ejecutar la aplicación en un servicio, realice los pasos siguientes:

1. Crear una carpeta en *c:\svc*.

1. Publique la aplicación en la carpeta con `dotnet publish --configuration Release --output c:\\svc`. El comando moverá activos de la aplicación a la carpeta, incluidos los necesarios `appsettings.json` archivo y `wwwroot` carpeta con su contenido.

1. Abra un **administrador** shell de comandos.

1. Ejecute los siguientes comandos:

   ```console
   sc create MyService binPath="c:\svc\aspnetcoreservice.exe"
   sc start MyService
   ```

1. En un explorador, vaya a `http://localhost:5000` para comprobar que el servicio se está ejecutando.

1. Para detener el servicio, use el comando:

   ```console
   sc stop MyService
   ```

Si la aplicación no se inicia como se esperaba cuando se ejecuta en un servicio, es una forma rápida de hacer accesibles los mensajes de error agregar un proveedor de registro, como el [proveedor de registro de eventos de Windows](https://docs.microsoft.com/aspnet/core/fundamentals/logging/index#eventlog). Otra opción consiste en comprobar el registro de eventos de aplicación con el Visor de eventos en el sistema. Por ejemplo, aquí es una excepción no controlada para un error de FileNotFound en el registro de eventos de aplicación:

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
