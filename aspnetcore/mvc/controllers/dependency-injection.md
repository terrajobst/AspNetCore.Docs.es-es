---
title: Inserción de dependencias en controladores en ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo los controladores de ASP.NET Core MVC solicitan sus dependencias explícitamente a través de sus constructores por medio de la inserción de dependencias en ASP.NET Core.
ms.author: riande
ms.date: 10/14/2016
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 23c91a4363223a135c50ceca51e6af22ed69fe3b
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276456"
---
# <a name="dependency-injection-into-controllers-in-aspnet-core"></a>Inserción de dependencias en controladores en ASP.NET Core

<a name="dependency-injection-controllers"></a>

Por [Steve Smith](https://ardalis.com/)

Los controladores de ASP.NET Core MVC deben solicitar sus dependencias explícitamente a través de sus constructores. En algunos casos, puede haber acciones específicas de controlador que requieran un servicio y quizá no tenga sentido realizar la solicitud en el nivel de controlador. En este caso, también puede insertar un servicio como un parámetro del método de acción.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([cómo descargarlo](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Inserción de dependencias

La inserción de dependencias es una técnica que sigue el [principio de inversión de dependencias](http://deviq.com/dependency-inversion-principle/), lo que permite que las aplicaciones consten de módulos de acoplamiento flexible. ASP.NET Core tiene compatibilidad integrada para [inserción de dependencias](../../fundamentals/dependency-injection.md), lo que facilita las tareas de prueba y mantenimiento de las aplicaciones.

## <a name="constructor-injection"></a>Inserción de constructores

La compatibilidad integrada de ASP.NET Core con la inserción de dependencias basada en constructores se extiende a los controladores MVC. Simplemente con agregar un tipo de servicio al controlador como un parámetro de constructor, ASP.NET Core intentará resolver ese tipo mediante su contenedor de servicios integrado. Normalmente, los servicios se definen mediante interfaces, aunque no siempre es así. Por ejemplo, si la aplicación tiene lógica de negocios que depende de la hora actual, también se puede insertar un servicio que recupera la hora (en lugar de codificarla de forma rígida), lo que permitiría superar las pruebas en implementaciones que usan una hora determinada.

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Implementar una interfaz como esta para que utilice el reloj del sistema en tiempo de ejecución no es nada complicado:

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


Teniendo esto implementado, podemos utilizar el servicio en el controlador. En este caso, hemos agregado lógica al método `Index` de `HomeController` para presentar un saludo al usuario en función de la hora del día.

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Si se ejecuta la aplicación ahora, probablemente se producirá un error:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Este error se produce cuando no se ha configurado un servicio en el método `ConfigureServices` de nuestra clase `Startup`. Para especificar que las solicitudes de `IDateTime` deben resolverse mediante una instancia de `SystemDateTime`, agregue la línea resaltada en la siguiente lista a su método `ConfigureServices`:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Este servicio en particular podría implementarse mediante cualquiera de las diversas opciones de duración (`Transient`, `Scoped` o `Singleton`). Consulte [Dependency Injection](../../fundamentals/dependency-injection.md) (Inserción de dependencias) para ver cómo afectará al comportamiento de su servicio cada una de estas opciones de ámbito.

Una vez que se ha configurado el servicio, al ejecutar la aplicación y navegar a la página principal se debería mostrar el mensaje basado en la hora según lo esperado:

![Saludo del servidor](dependency-injection/_static/server-greeting.png)

>[!TIP]
> Vea [Testing controller logic](testing.md) (Comprobación de la lógica de controlador) para obtener información sobre cómo solicitar explícitamente dependencias [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) en controladores para facilitar la comprobación de código.

La inserción de dependencias integrada de ASP.NET Core es compatible con tener un solo constructor para las clases que soliciten servicios. Si se tiene más de un constructor, es posible recibir la siguiente excepción:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Tal como indica el mensaje de error, tener un solo constructor corregiría el problema. También se puede [reemplazar la compatibilidad de inserción de dependencias predeterminada por una implementación de otros fabricantes](../../fundamentals/dependency-injection.md#replacing-the-default-services-container) que sea compatible con varios constructores.

## <a name="action-injection-with-fromservices"></a>Inserción de acción con FromServices

A veces, un servicio solo es necesario para una acción en el controlador. En este caso, puede tener sentido insertar el servicio como un parámetro en el método de acción. Para ello, se marca el parámetro con el atributo `[FromServices]`, tal como se muestra a continuación:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Acceso a la configuración desde un controlador

El acceso a la configuración de la aplicación o a los valores de configuración desde un controlador es un patrón habitual. Este acceso debe utilizar el patrón de opciones que se describe en el artículo sobre [configuración](xref:fundamentals/configuration/index). Por lo general, no es recomendable solicitar la configuración directamente desde el controlador mediante la inserción de dependencias. Un enfoque más adecuado consiste en solicitar una instancia de `IOptions<T>`, donde `T` es la clase de configuración que se necesita.

Para trabajar con el patrón de opciones, debe crear una clase como la siguiente que represente las opciones:

[!code-csharp[](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

A continuación, necesita configurar la aplicación para que use el modelo de opciones y agregar la clase de configuración a la colección de servicios en `ConfigureServices`:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> En la lista anterior, se configura la aplicación para que lea la configuración de un archivo con formato JSON. La configuración también se puede definir completamente en código, como se muestra en el código comentado anterior. Consulte [Configuración](xref:fundamentals/configuration/index) para conocer más opciones de configuración.

Una vez que haya especificado un objeto de configuración fuertemente tipado (en este caso, `SampleWebSettings`) y lo haya agregado a la colección de servicios, podrá solicitarlo desde cualquier método de acción o controlador mediante la solicitud de una instancia de `IOptions<T>` (en este caso, `IOptions<SampleWebSettings>`). El código siguiente muestra cómo se podría solicitar la configuración desde un controlador:

[!code-csharp[](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Seguir el patrón de opciones permite desacoplar entre sí los valores y la configuración, y garantiza que el controlador respete la [separación de intereses](http://deviq.com/separation-of-concerns/), ya que no necesita saber cómo ni dónde encontrar la información de configuración. También hace que sea más fácil realizar una prueba unitaria de la [lógica del controlador](testing.md), puesto que no hay efecto [static cling](http://deviq.com/static-cling/) ni creación directa de instancias de clases de configuración dentro de la clase de controlador.
