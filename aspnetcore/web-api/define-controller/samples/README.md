# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="a3a54-101">Ejemplo de controlador de ASP.NET Core Web API</span><span class="sxs-lookup"><span data-stu-id="a3a54-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="a3a54-102">Esta aplicación de ejemplo consta de los siguientes proyectos:</span><span class="sxs-lookup"><span data-stu-id="a3a54-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="a3a54-103">\**WebApiSample.Api.22*: proyecto de ASP.NET Core 2.2 que tiene como destino .NET Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="a3a54-103">\**WebApiSample.Api.22*: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="a3a54-104">**WebApiSample.Api.21**: proyecto de ASP.NET Core 2.1 que tiene como destino .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a3a54-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="a3a54-105">**WebApiSample.Api.Pre21**: proyecto de ASP.NET Core 2.0 que tiene como destino .NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="a3a54-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="a3a54-106">**WebApiSample.DataAccess**: biblioteca de clases .NET Standard 2.0 que actúa como nivel de acceso a datos en ambos proyectos de Web API.</span><span class="sxs-lookup"><span data-stu-id="a3a54-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="a3a54-107">En este ejemplo se ilustran las variaciones en la creación del controlador de Web API:</span><span class="sxs-lookup"><span data-stu-id="a3a54-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="a3a54-108">Derivación de una clase desde ControllerBase</span><span class="sxs-lookup"><span data-stu-id="a3a54-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="a3a54-109">Anotación de una clase con ApiControllerAttribute</span><span class="sxs-lookup"><span data-stu-id="a3a54-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
