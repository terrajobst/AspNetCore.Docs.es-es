# <a name="aspnet-core-web-api-controller-sample"></a>Ejemplo de controlador de ASP.NET Core Web API

Esta aplicación de ejemplo consta de los siguientes proyectos:

- **WebApiSample.Api**: proyecto de ASP.NET Core 2.1 que tiene como destino .NET Core 2.1.
- **WebApiSample.Api.Pre21**: proyecto de ASP.NET Core 2.0 que tiene como destino .NET Core 2.0.
- **WebApiSample.DataAccess**: biblioteca de clases .NET Standard 2.0 que actúa como nivel de acceso a datos en ambos proyectos de Web API.

En este ejemplo se ilustran las variaciones en la creación del controlador de Web API:

- [Derivación de una clase desde ControllerBase](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [Anotación de una clase con ApiControllerAttribute](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
