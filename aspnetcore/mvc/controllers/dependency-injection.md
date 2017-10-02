---
title: "Inyección de dependencia en controladores"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: bc8b4ba3-e9ba-48fd-b1eb-cd48ff6bc7a1
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/controllers/dependency-injection
ms.openlocfilehash: 4c632f521cf314bcf8c84f40c52a580a26a5ceee
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/01/2017
---
# <a name="dependency-injection-into-controllers"></a>Inyección de dependencia en controladores

<a name=dependency-injection-controllers></a>

Por [Steve Smith](https://ardalis.com/)

Controladores MVC de ASP.NET Core deben solicitar sus dependencias explícitamente a través de sus constructores. En algunos casos, las acciones de controlador individuales pueden requerir un servicio y puede que no tenga sentido para solicitar en el nivel de controlador. En este caso, también puede insertar un servicio como un parámetro del método de acción.

[Ver o descargar el código de ejemplo](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/controllers/dependency-injection/sample) ([cómo descargar](xref:tutorials/index#how-to-download-a-sample))

## <a name="dependency-injection"></a>Inyección de dependencia

Inserción de dependencias es una técnica que sigue a la [principio de inversión de dependencia](http://deviq.com/dependency-inversion-principle/), lo cual permite aplicaciones componerse de módulos de acoplamiento flexible. ASP.NET Core tiene compatibilidad integrada para [inyección de dependencia](../../fundamentals/dependency-injection.md), lo que facilita las aplicaciones probar y mantener.

## <a name="constructor-injection"></a>Inyección de constructor

Compatibilidad de ASP.NET Core para la inyección de dependencia basado en constructores se extiende a controladores MVC. Al agregar simplemente un tipo de servicio para el controlador como un parámetro de constructor, ASP.NET Core intentará resolver ese tipo mediante su integración en el contenedor de servicios. Servicios son normalmente, pero no siempre, definir mediante interfaces. Por ejemplo, si la aplicación tiene lógica de negocios que depende de la hora actual, también puede insertar un servicio que recupera el tiempo (en lugar de codificar de forma rígida), lo que permitiría las pruebas pasar de las implementaciones que usan una hora determinada.

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Interfaces/IDateTime.cs)]


Implementar una interfaz como este para que utilice el reloj del sistema en tiempo de ejecución es trivial:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Services/SystemDateTime.cs)]


Teniendo esto en su lugar, podemos utilizar el servicio en el controlador. En este caso, hemos agregado lógica para la `HomeController` `Index` método para presentar un saludo al usuario en función de la hora del día.

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=8,10,12,17,18,19,20,21,22,23,24,25,26,27,28,29,30&range=1-31,51-52)]

Si se ejecuta la aplicación ahora, probablemente se producirá un error:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Unable to resolve service for type 'ControllerDI.Interfaces.IDateTime' while attempting to activate 'ControllerDI.Controllers.HomeController'.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.GetService(IServiceProvider sp, Type type, Type requiredBy, Boolean isDefaultParameterRequired)
```

Este error se produce cuando no se ha configurado un servicio en la `ConfigureServices` método en nuestro `Startup` clase. Para especificar que las solicitudes para `IDateTime` debe resolverse mediante una instancia de `SystemDateTime`, agregue la línea resaltada en la siguiente lista para su `ConfigureServices` método:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=4&range=26-27,42-44)]

> [!NOTE]
> Este servicio en particular podría implementarse mediante cualquiera de las diversas opciones de duración distinta (`Transient`, `Scoped`, o `Singleton`). Vea [inyección de dependencia](../../fundamentals/dependency-injection.md) para comprender cómo cada una de estas opciones de ámbito afectará el comportamiento del servicio.

Una vez que se ha configurado el servicio, ejecutar la aplicación y navegar a la página principal deberían mostrar el mensaje de tiempo según lo esperado:

![Saludo del servidor](dependency-injection/_static/server-greeting.png)

>[!TIP]
> Vea [lógica del controlador de pruebas](testing.md) para obtener información sobre cómo solicitar explícitamente dependencias [http://deviq.com/explicit-dependencies-principle/](http://deviq.com/explicit-dependencies-principle/) en los controladores de facilita código de prueba.

Inyección de dependencia integrados de ASP.NET Core admite solo un constructor único para las clases que soliciten servicios. Si tiene más de un constructor, es posible recibir una excepción que indica:

```
An unhandled exception occurred while processing the request.

InvalidOperationException: Multiple constructors accepting all given argument types have been found in type 'ControllerDI.Controllers.HomeController'. There should only be one applicable constructor.
Microsoft.Extensions.DependencyInjection.ActivatorUtilities.FindApplicableConstructor(Type instanceType, Type[] argumentTypes, ConstructorInfo& matchingConstructor, Nullable`1[]& parameterMap)
```

Como indica el mensaje de error, puede corregir este problema tiene solo un constructor único. También puede [reemplazar la compatibilidad de inyección de dependencia predeterminada con una implementación de otros fabricantes](../../fundamentals/dependency-injection.md#replacing-the-default-services-container), muchas de que admite varios constructores.

## <a name="action-injection-with-fromservices"></a>Inyección de acción con FromServices

A veces, no es necesario un servicio para más de una acción en el controlador. En este caso, puede tener sentido insertar el servicio como un parámetro al método de acción. Esto se debe marcar el parámetro con el atributo `[FromServices]` como se muestra aquí:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/HomeController.cs?highlight=1&range=33-38)]

## <a name="accessing-settings-from-a-controller"></a>Acceso a la configuración de un controlador

Acceso a la configuración de aplicación o la configuración de un controlador es un patrón común. Este acceso debe utilizar el patrón de opciones se describen en [configuración](../../fundamentals/configuration.md). Por lo general debe no solicitar configuración directamente desde el controlador mediante la inserción de dependencias. Un enfoque más adecuado consiste en solicitud una `IOptions<T>` instancia, donde `T` es la clase de configuración que necesite.

Para trabajar con el patrón de opciones, debe crear una clase que representa las opciones, como esta:

[!code-csharp[Main](dependency-injection/sample/src/ControllerDI/Model/SampleWebSettings.cs)]

Es necesario que configure la aplicación para utilizar el modelo de opciones y agregar la clase de configuración a la colección de servicios en `ConfigureServices`:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Startup.cs?highlight=3,4,5,6,9,16,19&range=14-44)]

> [!NOTE]
> En la lista anterior, nos estamos configurar la aplicación para leer la configuración de un archivo con formato JSON. También puede configurar la configuración completamente en código, como se muestra en el código comentado anterior. Vea [configuración](../../fundamentals/configuration.md) para obtener más opciones de configuración.

Una vez que haya especificado un objeto de configuración fuertemente tipada (en este caso, `SampleWebSettings`) y agregarlo a la colección de servicios, se puede solicitar desde cualquier método de acción o controlador que solicita una instancia de `IOptions<T>` (en este caso, `IOptions<SampleWebSettings>`) . El código siguiente muestra cómo uno podría solicitar la configuración de un controlador:

[!code-csharp[Main](./dependency-injection/sample/src/ControllerDI/Controllers/SettingsController.cs?highlight=3,5,7&range=7-22)]

Seguir el patrón de opciones permite y la configuración se desacople entre sí y garantiza que el controlador es posterior [separación de intereses](http://deviq.com/separation-of-concerns/), ya que no es necesario saber cómo o dónde encontrar la configuración información. También facilita el controlador de la prueba unitaria [lógica del controlador de pruebas](testing.md), ya que no hay ningún [estáticos adhesivos para sus](http://deviq.com/static-cling/) o creación directa de instancias de clases de configuración dentro de la clase de controlador.
