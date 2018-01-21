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
# <a name="integration-testing-in-aspnet-core"></a>Pruebas de integración en ASP.NET Core

Por [Steve Smith](https://ardalis.com/)

Pruebas de integración garantizan que los componentes de una aplicación funcionan correctamente cuando se ensamblan juntos. Integración de es compatible con ASP.NET Core pruebas con marcos de pruebas unitarias y un host de web de prueba integrados que puede usarse para controlar las solicitudes sin la sobrecarga de la red.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/testing/integration-testing/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="introduction-to-integration-testing"></a>Introducción a las pruebas de integración

Pruebas de integración comprueban que distintas partes de una aplicación funcionen correctamente entre sí. A diferencia de [pruebas unitarias](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test), pruebas de integración suelen incluyen problemas de infraestructura de la aplicación, como una base de datos, sistema de archivos, recursos de red, o las solicitudes web y las respuestas. Pruebas unitarias utilizar falsificaciones u objetos ficticios en lugar de estos problemas, pero el propósito de las pruebas de integración es confirmar que el sistema funciona según lo previsto con estos sistemas.

Pruebas de integración, ya que éstos ejercer mayor segmentos de código y porque se basan en los elementos de infraestructura, tienden a ser órdenes de magnitud más lentas que las pruebas unitarias. Por lo tanto, es conveniente limitar el número de pruebas de integración que se escribe, especialmente si el mismo comportamiento puede probar con una prueba unitaria.

> [!NOTE]
> Si algunos comportamientos se pueden probar mediante una prueba unitaria o una prueba de integración, prefiera la prueba unitaria, puesto que se suele ser más rápida. Podría tener docenas o cientos de pruebas unitarias con muchas entradas diferentes pero solo una serie de pruebas de integración que abarcan los escenarios más importantes.

Normalmente es el dominio de pruebas unitarias para probar la lógica dentro de sus propios métodos. Probar el funcionamiento de la aplicación dentro de su marco de trabajo, por ejemplo ASP.NET Core o con una base de datos donde pruebas de integración se entra en juego. No tarda demasiado muchas pruebas de integración para confirmar que es capaz de escribir una fila en la base de datos y leerlos. No es necesario probar cada permutación posibles de su código de acceso a datos: solo debe probar lo suficiente como para dar la confianza de que la aplicación funciona correctamente.

## <a name="integration-testing-aspnet-core"></a>Pruebas de integración ASP.NET Core

Para obtener configurado para pruebas de integración de ejecución, debe crear un proyecto de prueba, agregue una referencia a su proyecto web de ASP.NET Core e instale a un ejecutor de pruebas. Este proceso se describe en el [pruebas unitarias](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) documentación, así como instrucciones más detalladas sobre cómo ejecutar pruebas y recomendaciones para asignar nombres a sus pruebas y las clases de prueba.

> [!NOTE]
> Separe las pruebas unitarias y las pruebas de integración con distintos proyectos. Esto ayuda a garantizar que accidentalmente no presentan problemas de infraestructura en las pruebas unitarias y le permite elegir fácilmente el conjunto de pruebas que desea ejecutar.

### <a name="the-test-host"></a>El Host de prueba

ASP.NET Core incluye un host de prueba que se puede agregar a proyectos de prueba de integración y utilizado para hospedar ASP.NET Core aplicaciones, prueba atendiendo las solicitudes sin la necesidad de un host web real. El ejemplo proporcionado incluye un proyecto de prueba de integración que se ha configurado para usar [xUnit](https://xunit.github.io) y el Host de prueba. Usa el `Microsoft.AspNetCore.TestHost` paquete NuGet.

Una vez el `Microsoft.AspNetCore.TestHost` paquete se incluye en el proyecto, podrá crear y configurar un `TestServer` en las pruebas. La prueba siguiente muestra cómo comprobar que una solicitud realizada a la raíz de un sitio devuelve "¡Hello World!" y debe ejecutarse correctamente y el valor predeterminado plantilla Web de ASP.NET Core vacía creada por Visual Studio.

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebDefaultRequestShould.cs?name=snippet_WebDefault&highlight=7,16,22)]

Esta prueba está usando el patrón Assert para organizar Act. El paso de organización se realiza en el constructor, que crea una instancia de `TestServer`. Un objeto configurado `WebHostBuilder` se usará para crear un `TestHost`; en este ejemplo, el `Configure` método desde el sistema sometido a prueba (SUT) `Startup` clase se pasa a la `WebHostBuilder`. Este método se utilizará para configurar la canalización de solicitud de la `TestServer` forma idéntica a cómo se puede configurar el servidor SUT.

En la parte de la acción de la prueba, se realiza una solicitud a la `TestServer` instancia para la ruta de acceso "/" y la respuesta se lee en una cadena. Esta cadena se compara con la cadena esperada de "¡Hello World!". Si coinciden, la prueba se supere; en caso contrario, se produce un error.

Ahora puede agregar algunas pruebas de integración adicionales para confirmar que la funcionalidad de comprobación de primos funciona a través de la aplicación web:

[!code-csharp[Main](../testing/integration-testing/sample/test/PrimeWeb.IntegrationTests/PrimeWebCheckPrimeShould.cs?name=snippet_CheckPrime)]

Tenga en cuenta que realmente no intenta comprobar la corrección del Comprobador de números primos con estas pruebas pero en su lugar que la aplicación web está haciendo lo que espera. Ya tiene cobertura de pruebas unitarias que aporta la confianza en `PrimeService`, como puede ver aquí:

![Explorador de pruebas](integration-testing/_static/test-explorer.png)

Puede aprender más acerca de las pruebas unitarias en el [pruebas unitarias](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test) artículo.


### <a name="integration-testing-mvcrazor"></a>Mvc/Razor de pruebas de integración

Los proyectos de prueba que contienen las vistas de Razor requieren `<PreserveCompilationContext>` se establece en true en la *.csproj* archivo:


```xml
    <PreserveCompilationContext>true</PreserveCompilationContext>
```

Proyectos de este elemento generarán un error similar al siguiente:
```
Microsoft.AspNetCore.Mvc.Razor.Compilation.CompilationFailedException: 'One or more compilation failures occurred:
ooebhccx.1bd(4,62): error CS0012: The type 'Attribute' is defined in an assembly that is not referenced. You must add a reference to assembly 'netstandard, Version=2.0.0.0, Culture=neutral, PublicKeyToken=cc7b13ffcd2ddd51'.
```


## <a name="refactoring-to-use-middleware"></a>Para usar el middleware de refactorización

La refactorización es el proceso de cambiar el código de la aplicación para mejorar su diseño sin cambiar su comportamiento. Lo ideal es que debe realizarse cuando hay un conjunto de pasar las pruebas, ya que estas le ayudan a asegurarse de que comportamiento del sistema sigue siendo la misma antes y después de los cambios. Examinando la manera en que se implementan la lógica de comprobación de primos en la aplicación web `Configure` método, consulte:

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

Este código funciona, pero resulta muy alejado del cómo le gustaría implementar este tipo de funcionalidad en una aplicación de ASP.NET Core, incluso simple que se trata. Imagine lo que el `Configure` método sería si necesita agregar este mucho código cada vez que agregue otro punto de conexión de dirección URL.

Agrega una opción a tener en cuenta [MVC](xref:mvc/overview) a la aplicación y crear un controlador para controlar la comprobación primos. Sin embargo, suponiendo que actualmente no necesita ninguna otra funcionalidad MVC, que es un bit resulte excesiva.

Sin embargo, puede sacar partido de ASP.NET Core [middleware](xref:fundamentals/middleware), que nos ayudarán a encapsular el primos lógica en su propia clase de comprobación y lograr mejor [separación de intereses](http://deviq.com/separation-of-concerns/) en el `Configure` método.

Desea permitir que la ruta de acceso del middleware que usa estar especificado como un parámetro, por lo que espera de la clase de middleware un `RequestDelegate` y un `PrimeCheckerOptions` instancia en su constructor. Si la ruta de acceso de la solicitud no coincide con lo que aparece este middleware configuró para esperar, simplemente llamará al siguiente middleware en la cadena y no hacer nada más. El resto del código de implementación que se encontraba en `Configure` está ahora en el `Invoke` método.

> [!NOTE]
> Ya que el middleware depende de la `PrimeService` servicio, que también está solicitando una instancia de este servicio con el constructor. El marco de trabajo proporcionará este servicio a través de [inyección de dependencia](xref:fundamentals/dependency-injection), suponiendo que se ha configurado, por ejemplo en `ConfigureServices`.

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Middleware/PrimeCheckerMiddleware.cs?highlight=39-63)]

Puesto que este middleware actúa como un punto de conexión en la cadena de delegados de la solicitud cuando coincide con su ruta de acceso, no hay ninguna llamada a `_next.Invoke` cuando este middleware gestiona la solicitud.

Con este middleware en vigor y algunos útil métodos de extensión creados para facilitar la configuración, el refactorizado `Configure` método tiene el siguiente aspecto:

[!code-csharp[Main](../testing/integration-testing/sample/src/PrimeWeb/Startup.cs?highlight=9&range=19-33)]

Después de esta operación de refactorización, esté seguro de que la aplicación web sigue funcionando como antes, ya que las pruebas de integración se superan.

> [!NOTE]
> Es una buena idea para confirmar los cambios al control de código fuente después de completar una refactorización y pasan las pruebas. Si está realizando la práctica de desarrollo controlado por pruebas, [considere la adición de confirmación a la fase del ciclo rojo-verde-refactorizar](https://ardalis.com/rgrc-is-the-new-red-green-refactor-for-test-first-development).

## <a name="resources"></a>Recursos

* [Pruebas unitarias](https://docs.microsoft.com/dotnet/articles/core/testing/unit-testing-with-dotnet-test)
* [Middleware](xref:fundamentals/middleware)
* [Pruebas de controladores](xref:mvc/controllers/testing)
