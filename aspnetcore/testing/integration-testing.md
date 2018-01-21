---
title: "Pruebas de integración en ASP.NET Core"
author: ardalis
description: "Cómo utilizar la integración de ASP.NET Core pruebas para asegurarse de que los componentes de una aplicación funcionen correctamente."
ms.author: riande
manager: wpickett
ms.date: 09/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: testing/integration-testing
ms.openlocfilehash: 8b0d741c05a723ad80fe812254c9a500a9fd9204
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/19/2018
---
# <a name="integration-testing-in-aspnet-core"></a><span data-ttu-id="a29a9-103">Pruebas de integración en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a29a9-103">Integration testing in ASP.NET Core</span></span>

<span data-ttu-id="a29a9-104">Por [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a29a9-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a29a9-105">Pruebas de integración garantizan que los componentes de una aplicación funcionan correctamente cuando se ensamblan juntos.</span><span class="sxs-lookup"><span data-stu-id="a29a9-105">Integration testing ensures that an application's components function correctly when assembled together.</span></span> <span data-ttu-id="a29a9-106">Integración de es compatible con ASP.NET Core pruebas con marcos de pruebas unitarias y un host de web de prueba integrados que puede usarse para controlar las solicitudes sin la sobrecarga de la red.</span><span class="sxs-lookup"><span data-stu-id="a29a9-106">ASP.NET Core supports integration testing using unit test frameworks and a built-in test web host that can be used to handle requests without network overhead.</span></span>

<span data-ttu-id="a29a9-107">[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a29a9-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="introduction-to-integration-testing"></a><span data-ttu-id="a29a9-108">Introducción a las pruebas de integración</span><span class="sxs-lookup"><span data-stu-id="a29a9-108">Introduction to integration testing</span></span>

<span data-ttu-id="a29a9-109">Pruebas de integración comprueban que distintas partes de una aplicación funcionen correctamente entre sí.</span><span class="sxs-lookup"><span data-stu-id="a29a9-109">Integration tests verify that different parts of an application work correctly together.</span></span> <span data-ttu-id="a29a9-110">A diferencia de [pruebas unitarias](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), pruebas de integración suelen incluyen problemas de infraestructura de la aplicación, como una base de datos, sistema de archivos, recursos de red, o las solicitudes web y las respuestas.</span><span class="sxs-lookup"><span data-stu-id="a29a9-110">Unlike [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), integration tests frequently involve application infrastructure concerns, such as a database, file system, network resources, or web requests and responses.</span></span> <span data-ttu-id="a29a9-111">Pruebas unitarias utilizar falsificaciones u objetos ficticios en lugar de estos problemas, pero el propósito de las pruebas de integración es confirmar que el sistema funciona según lo previsto con estos sistemas.</span><span class="sxs-lookup"><span data-stu-id="a29a9-111">Unit tests use fakes or mock objects in place of these concerns, but the purpose of integration tests is to confirm that the system works as expected with these systems.</span></span>

<span data-ttu-id="a29a9-112">Pruebas de integración, ya que éstos ejercer mayor segmentos de código y porque se basan en los elementos de infraestructura, tienden a ser órdenes de magnitud más lentas que las pruebas unitarias.</span><span class="sxs-lookup"><span data-stu-id="a29a9-112">Integration tests, because they exercise larger segments of code and because they rely on infrastructure elements, tend to be orders of magnitude slower than unit tests.</span></span> <span data-ttu-id="a29a9-113">Por lo tanto, es conveniente limitar el número de pruebas de integración que se escribe, especialmente si el mismo comportamiento puede probar con una prueba unitaria.</span><span class="sxs-lookup"><span data-stu-id="a29a9-113">Thus, it's a good idea to limit how many integration tests you write, especially if you can test the same behavior with a unit test.</span></span>

> [!NOTE]
> <span data-ttu-id="a29a9-114">Si algunos comportamientos se pueden probar mediante una prueba unitaria o una prueba de integración, prefiera la prueba unitaria, puesto que se suele ser más rápida.</span><span class="sxs-lookup"><span data-stu-id="a29a9-114">If some behavior can be tested using either a unit test or an integration test, prefer the unit test, since it will be almost always be faster.</span></span> <span data-ttu-id="a29a9-115">Podría tener docenas o cientos de pruebas unitarias con muchas entradas diferentes pero solo una serie de pruebas de integración que abarcan los escenarios más importantes.</span><span class="sxs-lookup"><span data-stu-id="a29a9-115">You might have dozens or hundreds of unit tests with many different inputs but just a handful of integration tests covering the most important scenarios.</span></span>

<span data-ttu-id="a29a9-116">Normalmente es el dominio de pruebas unitarias para probar la lógica dentro de sus propios métodos.</span><span class="sxs-lookup"><span data-stu-id="a29a9-116">Testing the logic within your own methods is usually the domain of unit tests.</span></span> <span data-ttu-id="a29a9-117">Probar el funcionamiento de la aplicación dentro de su marco de trabajo, por ejemplo ASP.NET Core o con una base de datos donde pruebas de integración se entra en juego.</span><span class="sxs-lookup"><span data-stu-id="a29a9-117">Testing how your application works within its framework, for example with ASP.NET Core, or with a database is where integration tests come into play.</span></span> <span data-ttu-id="a29a9-118">No tarda demasiado muchas pruebas de integración para confirmar que es capaz de escribir una fila en la base de datos y leerlos.</span><span class="sxs-lookup"><span data-stu-id="a29a9-118">It doesn't take too many integration tests to confirm that you're able to write a row to the database and read it back.</span></span> <span data-ttu-id="a29a9-119">No es necesario probar cada permutación posibles de su código de acceso a datos: solo debe probar lo suficiente como para dar la confianza de que la aplicación funciona correctamente.</span><span class="sxs-lookup"><span data-stu-id="a29a9-119">You don't need to test every possible permutation of your data access code - you only need to test enough to give you confidence that your application is working properly.</span></span>

## <a name="integration-testing-aspnet-core"></a><span data-ttu-id="a29a9-120">Pruebas de integración ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a29a9-120">Integration testing ASP.NET Core</span></span>

<span data-ttu-id="a29a9-121">Para obtener configurado para pruebas de integración de ejecución, debe crear un proyecto de prueba, agregue una referencia a su proyecto web de ASP.NET Core e instale a un ejecutor de pruebas.</span><span class="sxs-lookup"><span data-stu-id="a29a9-121">To get set up to run integration tests, you'll need to create a test project, add a reference to your ASP.NET Core web project, and install a test runner.</span></span> <span data-ttu-id="a29a9-122">Este proceso se describe en el [pruebas unitarias](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentación, así como instrucciones más detalladas sobre cómo ejecutar pruebas y recomendaciones para asignar nombres a sus pruebas y las clases de prueba.</span><span class="sxs-lookup"><span data-stu-id="a29a9-122">This process is described in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentation, along with more detailed instructions on running tests and recommendations for naming your tests and test classes.</span></span>

> [!NOTE]
> <span data-ttu-id="a29a9-123">Separe las pruebas unitarias y las pruebas de integración con distintos proyectos.</span><span class="sxs-lookup"><span data-stu-id="a29a9-123">Separate your unit tests and your integration tests using different projects.</span></span> <span data-ttu-id="a29a9-124">Esto ayuda a garantizar que accidentalmente no presentan problemas de infraestructura en las pruebas unitarias y le permite elegir fácilmente el conjunto de pruebas que desea ejecutar.</span><span class="sxs-lookup"><span data-stu-id="a29a9-124">This helps ensure you don't accidentally introduce infrastructure concerns into your unit tests and lets you easily choose which set of tests to run.</span></span>

### <a name="the-test-host"></a><span data-ttu-id="a29a9-125">El Host de prueba</span><span class="sxs-lookup"><span data-stu-id="a29a9-125">The Test Host</span></span>

<span data-ttu-id="a29a9-126">ASP.NET Core incluye un host de prueba que se puede agregar a proyectos de prueba de integración y utilizado para hospedar ASP.NET Core aplicaciones, prueba atendiendo las solicitudes sin la necesidad de un host web real.</span><span class="sxs-lookup"><span data-stu-id="a29a9-126">ASP.NET Core includes a test host that can be added to integration test projects and used to host ASP.NET Core applications, serving test requests without the need for a real web host.</span></span> <span data-ttu-id="a29a9-127">El ejemplo proporcionado incluye un proyecto de prueba de integración que se ha configurado para usar [xUnit](https://xunit.github.io) y el Host de prueba.</span><span class="sxs-lookup"><span data-stu-id="a29a9-127">The provided sample includes an integration test project which has been configured to use [xUnit](https://xunit.github.io) and the Test Host.</span></span> <span data-ttu-id="a29a9-128">Usa el `Microsoft.AspNetCore.TestHost` paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="a29a9-128">It uses the `Microsoft.AspNetCore.TestHost` NuGet package.</span></span>

<span data-ttu-id="a29a9-129">Una vez el `Microsoft.AspNetCore.TestHost` paquete se incluye en el proyecto, podrá crear y configurar un `TestServer` en las pruebas.</span><span class="sxs-lookup"><span data-stu-id="a29a9-129">Once the `Microsoft.AspNetCore.TestHost` package is included in the project, you'll be able to create and configure a `TestServer` in your tests.</span></span> <span data-ttu-id="a29a9-130">La prueba siguiente muestra cómo comprobar que una solicitud realizada a la raíz de un sitio devuelve "¡Hello World!"</span><span class="sxs-lookup"><span data-stu-id="a29a9-130">The following test shows how to verify that a request made to the root of a site returns "Hello World!"</span></span> <span data-ttu-id="a29a9-131">y debe ejecutarse correctamente y el valor predeterminado plantilla Web de ASP.NET Core vacía creada por Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a29a9-131">and should run successfully against the default ASP.NET Core Empty Web template created by Visual Studio.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

<span data-ttu-id="a29a9-132">Esta prueba está usando el patrón Assert para organizar Act.</span><span class="sxs-lookup"><span data-stu-id="a29a9-132">This test is using the Arrange-Act-Assert pattern.</span></span> <span data-ttu-id="a29a9-133">El paso de organización se realiza en el constructor, que crea una instancia de `TestServer`.</span><span class="sxs-lookup"><span data-stu-id="a29a9-133">The Arrange step is done in the constructor, which creates an instance of `TestServer`.</span></span> <span data-ttu-id="a29a9-134">Un objeto configurado `WebHostBuilder` se usará para crear un `TestHost`; en este ejemplo, el `Configure` método desde el sistema sometido a prueba (SUT) `Startup` clase se pasa a la `WebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="a29a9-134">A configured `WebHostBuilder` will be used to create a `TestHost`; in this example, the `Configure` method from the system under test (SUT)'s `Startup` class is passed to the `WebHostBuilder`.</span></span> <span data-ttu-id="a29a9-135">Este método se utilizará para configurar la canalización de solicitud de la `TestServer` forma idéntica a cómo se puede configurar el servidor SUT.</span><span class="sxs-lookup"><span data-stu-id="a29a9-135">This method will be used to configure the request pipeline of the `TestServer` identically to how the SUT server would be configured.</span></span>

<span data-ttu-id="a29a9-136">En la parte de la acción de la prueba, se realiza una solicitud a la `TestServer` instancia para la ruta de acceso "/" y la respuesta se lee en una cadena.</span><span class="sxs-lookup"><span data-stu-id="a29a9-136">In the Act portion of the test, a request is made to the `TestServer` instance for the "/" path, and the response is read back into a string.</span></span> <span data-ttu-id="a29a9-137">Esta cadena se compara con la cadena esperada de "¡Hello World!".</span><span class="sxs-lookup"><span data-stu-id="a29a9-137">This string is compared with the expected string of "Hello World!".</span></span> <span data-ttu-id="a29a9-138">Si coinciden, la prueba se supere; en caso contrario, se produce un error.</span><span class="sxs-lookup"><span data-stu-id="a29a9-138">If they match, the test passes; otherwise, it fails.</span></span>

<span data-ttu-id="a29a9-139">Ahora puede agregar algunas pruebas de integración adicionales para confirmar que la funcionalidad de comprobación de primos funciona a través de la aplicación web:</span><span class="sxs-lookup"><span data-stu-id="a29a9-139">Now you can add a few additional integration tests to confirm that the prime checking functionality works via the web application:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

<span data-ttu-id="a29a9-140">Tenga en cuenta que realmente no intenta comprobar la corrección del Comprobador de números primos con estas pruebas pero en su lugar que la aplicación web está haciendo lo que espera.</span><span class="sxs-lookup"><span data-stu-id="a29a9-140">Note that you're not really trying to test the correctness of the prime number checker with these tests but rather that the web application is doing what you expect.</span></span> <span data-ttu-id="a29a9-141">Ya tiene cobertura de pruebas unitarias que aporta la confianza en `PrimeService`, como puede ver aquí:</span><span class="sxs-lookup"><span data-stu-id="a29a9-141">You already have unit test coverage that gives you confidence in `PrimeService`, as you can see here:</span></span>

![Explorador de pruebas](integration-testing/_static/test-explorer.png)

<span data-ttu-id="a29a9-143">Puede aprender más acerca de las pruebas unitarias en el [pruebas unitarias](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) artículo.</span><span class="sxs-lookup"><span data-stu-id="a29a9-143">You can learn more about the unit tests in the [Unit testing](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) article.</span></span>


### <a name="integration-testing-mvcrazor"></a><span data-ttu-id="a29a9-144">Mvc/Razor de pruebas de integración</span><span class="sxs-lookup"><span data-stu-id="a29a9-144">Integration testing Mvc/Razor</span></span>

<span data-ttu-id="a29a9-145">Los proyectos de prueba que contienen las vistas de Razor requieren `<PreserveCompilationContext>` se establece en true en la *.csproj* archivo:</span><span class="sxs-lookup"><span data-stu-id="a29a9-145">Test projects that contain Razor views require `<PreserveCompilationContext>` be set to true in the *.csproj* file:</span></span>


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

<span data-ttu-id="a29a9-146">Proyectos de este elemento generarán un error similar al siguiente:</span><span class="sxs-lookup"><span data-stu-id="a29a9-146">Projects missing this element will generate an error similar to the following:</span></span>
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a><span data-ttu-id="a29a9-147">Para usar el middleware de refactorización</span><span class="sxs-lookup"><span data-stu-id="a29a9-147">Refactoring to use middleware</span></span>

<span data-ttu-id="a29a9-148">La refactorización es el proceso de cambiar el código de la aplicación para mejorar su diseño sin cambiar su comportamiento.</span><span class="sxs-lookup"><span data-stu-id="a29a9-148">Refactoring is the process of changing an application's code to improve its design without changing its behavior.</span></span> <span data-ttu-id="a29a9-149">Lo ideal es que debe realizarse cuando hay un conjunto de pasar las pruebas, ya que estas le ayudan a asegurarse de que comportamiento del sistema sigue siendo la misma antes y después de los cambios.</span><span class="sxs-lookup"><span data-stu-id="a29a9-149">It should ideally be done when there is a suite of passing tests, since these help ensure the system's behavior remains the same before and after the changes.</span></span> <span data-ttu-id="a29a9-150">Examinando la manera en que se implementan la lógica de comprobación de primos en la aplicación web `Configure` método, consulte:</span><span class="sxs-lookup"><span data-stu-id="a29a9-150">Looking at the way in which the prime checking logic is implemented in the web application's `Configure` method, you see:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.Run(async (context) =>
    {
        if (context.Request.Path.Value.Contains("checkprime"))
        {
            int numberToCheck;
            try
            {
                numberToCheck = int.Parse(context.Request.QueryString.Value.Replace("?", ""));
                var primeService = new PrimeService();
                if (primeService.IsPrime(numberToCheck))
                {
                    await context.Response.WriteAsync($"{numberToCheck} is prime!");
                }
                else
                {
                    await context.Response.WriteAsync($"{numberToCheck} is NOT prime!");
                }
            }
            catch
            {
                await context.Response.WriteAsync("Pass in a number to check in the form /checkprime?5");
            }
        }
        else
        {
            await context.Response.WriteAsync("Hello World!");
        }
    });
}
```

<span data-ttu-id="a29a9-151">Este código funciona, pero resulta muy alejado del cómo le gustaría implementar este tipo de funcionalidad en una aplicación de ASP.NET Core, incluso simple que se trata.</span><span class="sxs-lookup"><span data-stu-id="a29a9-151">This code works, but it's far from how you would like to implement this kind of functionality in an ASP.NET Core application, even as simple as this is.</span></span> <span data-ttu-id="a29a9-152">Imagine lo que el `Configure` método sería si necesita agregar este mucho código cada vez que agregue otro punto de conexión de dirección URL.</span><span class="sxs-lookup"><span data-stu-id="a29a9-152">Imagine what the `Configure` method would look like if you needed to add this much code to it every time you add another URL endpoint!</span></span>

<span data-ttu-id="a29a9-153">Agrega una opción a tener en cuenta [MVC](xref:mvc/overview) a la aplicación y crear un controlador para controlar la comprobación primos.</span><span class="sxs-lookup"><span data-stu-id="a29a9-153">One option to consider is adding [MVC](xref:mvc/overview) to the application and creating a controller to handle the prime checking.</span></span> <span data-ttu-id="a29a9-154">Sin embargo, suponiendo que actualmente no necesita ninguna otra funcionalidad MVC, que es un bit resulte excesiva.</span><span class="sxs-lookup"><span data-stu-id="a29a9-154">However, assuming you don't currently need any other MVC functionality, that's a bit overkill.</span></span>

<span data-ttu-id="a29a9-155">Sin embargo, puede sacar partido de ASP.NET Core [middleware](xref:fundamentals/middleware), que nos ayudarán a encapsular el primos lógica en su propia clase de comprobación y lograr mejor [separación de intereses](http://deviq.com/separation-of-concerns/) en el `Configure` método.</span><span class="sxs-lookup"><span data-stu-id="a29a9-155">You can, however, take advantage of ASP.NET Core [middleware](xref:fundamentals/middleware), which will help us encapsulate the prime checking logic in its own class and achieve better [separation of concerns](http://deviq.com/separation-of-concerns/) in the `Configure` method.</span></span>

<span data-ttu-id="a29a9-156">Desea permitir que la ruta de acceso del middleware que usa estar especificado como un parámetro, por lo que espera de la clase de middleware un `RequestDelegate` y un `PrimeCheckerOptions` instancia en su constructor.</span><span class="sxs-lookup"><span data-stu-id="a29a9-156">You want to allow the path the middleware uses to be specified as a parameter, so the middleware class expects a `RequestDelegate` and a `PrimeCheckerOptions` instance in its constructor.</span></span> <span data-ttu-id="a29a9-157">Si la ruta de acceso de la solicitud no coincide con lo que aparece este middleware configuró para esperar, simplemente llamará al siguiente middleware en la cadena y no hacer nada más.</span><span class="sxs-lookup"><span data-stu-id="a29a9-157">If the path of the request doesn't match what this middleware is configured to expect, you simply call the next middleware in the chain and do nothing further.</span></span> <span data-ttu-id="a29a9-158">El resto del código de implementación que se encontraba en `Configure` está ahora en el `Invoke` método.</span><span class="sxs-lookup"><span data-stu-id="a29a9-158">The rest of the implementation code that was in `Configure` is now in the `Invoke` method.</span></span>

> [!NOTE]
> <span data-ttu-id="a29a9-159">Ya que el middleware depende de la `PrimeService` servicio, que también está solicitando una instancia de este servicio con el constructor.</span><span class="sxs-lookup"><span data-stu-id="a29a9-159">Since the middleware depends on the `PrimeService` service, you're also requesting an instance of this service with the constructor.</span></span> <span data-ttu-id="a29a9-160">El marco de trabajo proporcionará este servicio a través de [inyección de dependencia](xref:fundamentals/dependency-injection), suponiendo que se ha configurado, por ejemplo en `ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a29a9-160">The framework will provide this service via [Dependency Injection](xref:fundamentals/dependency-injection), assuming it has been configured, for example in `ConfigureServices`.</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

<span data-ttu-id="a29a9-161">Puesto que este middleware actúa como un punto de conexión en la cadena de delegados de la solicitud cuando coincide con su ruta de acceso, no hay ninguna llamada a `_next.Invoke` cuando este middleware gestiona la solicitud.</span><span class="sxs-lookup"><span data-stu-id="a29a9-161">Since this middleware acts as an endpoint in the request delegate chain when its path matches, there is no call to `_next.Invoke` when this middleware handles the request.</span></span>

<span data-ttu-id="a29a9-162">Con este middleware en vigor y algunos útil métodos de extensión creados para facilitar la configuración, el refactorizado `Configure` método tiene el siguiente aspecto:</span><span class="sxs-lookup"><span data-stu-id="a29a9-162">With this middleware in place and some helpful extension methods created to make configuring it easier, the refactored `Configure` method looks like this:</span></span>

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

<span data-ttu-id="a29a9-163">Después de esta operación de refactorización, esté seguro de que la aplicación web sigue funcionando como antes, ya que las pruebas de integración se superan.</span><span class="sxs-lookup"><span data-stu-id="a29a9-163">Following this refactoring, you're confident that the web application still works as before, since your integration tests are all passing.</span></span>

> [!NOTE]
> <span data-ttu-id="a29a9-164">Es una buena idea para confirmar los cambios al control de código fuente después de completar una refactorización y pasan las pruebas.</span><span class="sxs-lookup"><span data-stu-id="a29a9-164">It's a good idea to commit your changes to source control after you complete a refactoring and your tests pass.</span></span> <span data-ttu-id="a29a9-165">Si está realizando la práctica de desarrollo controlado por pruebas, [considere la adición de confirmación a la fase del ciclo rojo-verde-refactorizar](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span><span class="sxs-lookup"><span data-stu-id="a29a9-165">If you're practicing Test Driven Development, [consider adding Commit to your Red-Green-Refactor cycle](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).</span></span>

## <a name="resources"></a><span data-ttu-id="a29a9-166">Recursos</span><span class="sxs-lookup"><span data-stu-id="a29a9-166">Resources</span></span>

* [<span data-ttu-id="a29a9-167">Pruebas unitarias</span><span class="sxs-lookup"><span data-stu-id="a29a9-167">Unit testing</span></span>](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [<span data-ttu-id="a29a9-168">Middleware</span><span class="sxs-lookup"><span data-stu-id="a29a9-168">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="a29a9-169">Pruebas de controladores</span><span class="sxs-lookup"><span data-stu-id="a29a9-169">Testing controllers</span></span>](xref:mvc/controllers/testing)
