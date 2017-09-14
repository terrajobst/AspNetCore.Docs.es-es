---
title: "Configuración de ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo usar la API de configuración para configurar una aplicación ASP.NET Core desde varios orígenes."
keywords: "Núcleo de ASP.NET, configuración, JSON, configuración"
ms.author: riande
manager: wpickett
ms.date: 6/24/2017
ms.topic: article
ms.assetid: b3a5984d-e172-42eb-8a48-547e4acb6806
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/configuration
ms.openlocfilehash: 041bb04a3a3699a166a03338865da154403d8c07
ms.sourcegitcommit: f535ce61c6a5e615bc6399b5d763c734396231f4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/13/2017
---
<a name=fundamentals-configuration></a>

# <a name="configuration-in-aspnet-core"></a>Configuración de ASP.NET Core

[Rick Anderson](https://twitter.com/RickAndMSFT), [marca Michaelis](http://intellitect.com/author/mark-michaelis/), [Steve Smith](https://ardalis.com/), y [Daniel Roth](https://github.com/danroth27)

La API de configuración proporciona una manera de configurar una aplicación basada en una lista de pares nombre / valor. Se lee la configuración en tiempo de ejecución de varios orígenes. Los pares de nombre-valor se pueden agrupar en una jerarquía multinivel. Existen proveedores de configuración para:

* Formatos de archivo (INI, JSON y XML)
* Argumentos de línea de comandos
* Variables de entorno
* Objetos de .NET en memoria
* Un almacén de usuario cifrado
* [Almacén de claves de Azure](xref:security/key-vault-configuration)
* Proveedores personalizados, que se instale o cree

Cada valor de configuración se asigna a una clave de cadena. No hay compatibilidad de enlace integrado para deserializar la configuración en un personalizado [POCO](https://wikipedia.org/wiki/Plain_Old_CLR_Object) objeto (una clase .NET simple con propiedades).

[Ver o descargar el código de ejemplo](https://github.com/aspnet/docs/tree/master/aspnetcore/fundamentals/configuration/sample)

## <a name="simple-configuration"></a>Muestra una configuración sencilla

La siguiente aplicación de consola utiliza el proveedor de configuración de JSON:

[!code-csharp[Main](configuration/sample/ConfigJson/Program.cs)]

La aplicación lee y muestra los valores de configuración siguientes:

[!code-json[Main](configuration/sample/ConfigJson/appsettings.json)]

Configuración consta de una lista jerárquica de los pares de nombre / valor en el que los nodos están separados por un coma. Para recuperar un valor, tener acceso a la `Configuration` indizador con la correspondiente clave del elemento:

```csharp
Console.WriteLine($"option1 = {Configuration["subsection:suboption1"]}");
```

Para trabajar con matrices en orígenes de configuración con formato JSON, usar un índice de matriz como parte de la cadena separada por dos puntos. En el ejemplo siguiente se obtiene el nombre del primer elemento en el anterior `wizards` matriz:

```csharp
Console.Write($"{Configuration["wizards:0:Name"]}, ");
```

Pares de nombre / valor que se escriben en integrado en `Configuration` proveedores son **no** es persistente, sin embargo, puede crear un proveedor personalizado que guarda los valores. Vea [proveedor de configuración personalizada](xref:fundamentals/configuration#custom-config-providers).

En el ejemplo anterior utiliza el indizador de configuración para leer los valores. Acceso a la configuración fuera de `Startup`, use la [patrón opciones](xref:fundamentals/configuration#options-config-objects). El *patrón opciones* se muestra más adelante en este artículo.

Es habitual tener distintos valores de configuración para los entornos diferentes, por ejemplo, desarrollo, prueba y producción. El `CreateDefaultBuilder` método de extensión en una aplicación de ASP.NET Core 2.x (o mediante `AddJsonFile` y `AddEnvironmentVariables` directamente en una aplicación de ASP.NET Core 1.x) agrega proveedores de configuración para leer archivos JSON y sistema de orígenes de configuración:

* *appsettings.json*
* *appSettings. \<EnvironmentName > .json*
* variables de entorno

Vea [AddJsonFile](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.jsonconfigurationextensions) para obtener una explicación de los parámetros. `reloadOnChange`solo se admite en ASP.NET Core 1.1 y versiones posteriores. 

Orígenes de configuración se leen en el orden en que se especifican. En el código anterior, las variables de entorno se leen de la última. Los valores de configuración establecido a través del entorno podría reemplazarlas que se establecen en los dos proveedores anteriores.

El entorno se establece normalmente en uno de `Development`, `Staging`, o `Production`. Vea [trabajar con varios entornos](environments.md) para obtener más información.

Consideraciones sobre la configuración:

* `IOptionsSnapshot`puede volver a cargar los datos de configuración, cuando el estado cambia. Use `IOptionsSnapshot` si tiene que volver a cargar los datos de configuración.  Vea [IOptionsSnapshot](#ioptionssnapshot) para obtener más información.
* Claves de configuración que distinguen mayúsculas de minúsculas.
* Una práctica recomendada es especificar variables de entorno por último, para que el entorno local puedan reemplazar todo lo establecido en los archivos de configuración implementada.
* **Nunca** almacenar contraseñas u otros datos confidenciales en el código de proveedor de configuración o en archivos de configuración de texto sin formato. No utilice secretos de producción en el desarrollo o entornos de prueba. En su lugar, especifique secretos fuera del árbol del proyecto, por lo que no se pueden confirmar accidentalmente en el repositorio. Obtenga más información sobre [trabajar con varios entornos](environments.md) y administrar [almacenamiento seguro para la ejecución de secretos de aplicación durante el desarrollo](../security/app-secrets.md).
* Si `:` no se puede utilizar en variables de entorno en el sistema, reemplace `:` con `__` (subrayado doble).

<a name=options-config-objects></a>

## <a name="using-options-and-configuration-objects"></a>Uso de opciones y objetos de configuración

El patrón de opciones usa las clases de opciones personalizadas para representar un grupo de valores relacionados. Se recomienda crear clases desacopladas para cada característica dentro de la aplicación. Clases desacopladas siguen:

* El [principio de segregación de interfaz (ISP)](http://deviq.com/interface-segregation-principle/) : clases dependen únicamente de los valores de configuración que se usan.
* [Separación de intereses](http://deviq.com/separation-of-concerns/) : configuración para las distintas partes de la aplicación no es dependientes o acoplamiento entre sí.

La clase de opciones debe ser no abstracto con un constructor público sin parámetros. Por ejemplo:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MyOptions.cs)]

<a name=options-example></a>

En el código siguiente, se habilita el proveedor de configuración de JSON. La `MyOptions` clase se agrega al contenedor de servicios y se enlaza a la configuración.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup.cs?name=snippet1&highlight=8,20-21)]

El siguiente [controlador](../mvc/controllers/index.md) utiliza [constructor inyección de dependencia](xref:fundamentals/dependency-injection#what-is-dependency-injection) en [ `IOptions<TOptions>` ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.options.ioptions-1) acceso a la configuración:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController.cs?name=snippet1)]

Con el siguiente *appSettings.JSON que se* archivo:

[!code-json[Main](configuration/sample/UsingOptions/appsettings1.json)]

El `HomeController.Index` método `option1 = value1_from_json, option2 = 2`.

Las aplicaciones típicas no enlazar toda la configuración a un archivo de opciones de inicio único. Más adelante voy a explicar cómo usar `GetSection` para enlazar a una sección.

En el código siguiente, un segundo `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios. Usa un delegado para configurar el enlace con `MyOptions`.

[!code-csharp[Main](configuration/sample/UsingOptions/Startup2.cs?name=snippet1&highlight=9-13)]

Puede agregar varios proveedores de configuración. Proveedores de configuración están disponibles en paquetes de NuGet. Se aplican en orden que están registrados.

Cada llamada a `Configure<TOptions>` agrega un `IConfigureOptions<TOptions>` servicio al contenedor de servicios. En el ejemplo anterior, los valores de `Option1` y `Option2` se especifican en *appSettings.JSON que se* --pero el valor de `Option1` se reemplaza por el delegado configurado. 

Cuando se habilita más de un servicio de configuración, la última fuente de configuración especificado "gana" (que establece el valor de configuración). En el código anterior, el `HomeController.Index` método `option1 = value1_from_action, option2 = 2`.

Al enlazar opciones de configuración, cada propiedad en el tipo de opciones está enlazado a una clave de configuración del formulario `property[:sub-property:]`. Por ejemplo, el `MyOptions.Option1` propiedad se enlaza a la clave `Option1`, que se lee desde el `option1` propiedad en *appSettings.JSON que se*. Más adelante en este artículo, se muestra un ejemplo de la subpropiedad.

En el código siguiente, una tercera `IConfigureOptions<TOptions>` servicio se agrega al contenedor de servicios. Enlaza `MySubOptions` a la sección `subsection` de la *appSettings.JSON que se* archivo:

[!code-csharp[Main](configuration/sample/UsingOptions/Startup3.cs?name=snippet1&highlight=16-17)]

Nota: Este método de extensión requiere el `Microsoft.Extensions.Options.ConfigurationExtensions` paquete NuGet.

Con los siguientes *appSettings.JSON que se* archivo:

[!code-json[Main](configuration/sample/UsingOptions/appsettings.json)]

La `MySubOptions` clase:

[!code-csharp[Main](configuration/sample/UsingOptions/Models/MySubOptions.cs?name=snippet1)]

Con los siguientes `Controller`:

[!code-csharp[Main](configuration/sample/UsingOptions/Controllers/HomeController2.cs?name=snippet1)]

`subOption1 = subvalue1_from_json, subOption2 = 200`se devuelve.

También puede proporcionar opciones en un modelo de vista o insertar `IOptions<TOptions>` directamente en una vista:

[!code-html[Main](configuration/sample/UsingOptions/Views/Home/Index.cshtml?highlight=3-4,16-17,20-21)]

<a name=in-memory-provider></a>

## <a name="ioptionssnapshot"></a>IOptionsSnapshot

*Requiere ASP.NET Core 1.1 o posterior.*

`IOptionsSnapshot`permite volver a cargar los datos de configuración cuando ha cambiado el archivo de configuración. Tiene una sobrecarga mínima. Usar `IOptionsSnapshot` con `reloadOnChange: true`, las opciones están enlazadas a `IConfiguration` y volver a cargar cuando cambia.

El siguiente ejemplo se muestra cómo un nuevo `IOptionsSnapshot` se crea después *config.json* cambios. Las solicitudes realizadas al servidor devolverán la misma hora *config.json* tiene **no** cambiado. La primera solicitud después de *config.json* cambios mostrará una nueva hora.

[!code-csharp[Main](configuration/sample/IOptionsSnapshot2/Startup.cs?name=snippet1&highlight=1-9,13-18,32,33,52,53)]

La siguiente imagen muestra la salida de servidor:

![que muestra de imagen de explorador "última actualización: 11/22/2016 4:43 PM"](configuration/_static/first.png)

Actualizar el explorador no cambia el valor de mensaje o el tiempo que se muestra (cuando *config.json* no ha cambiado).

Cambiar y guardar la *config.json* y, a continuación, actualice el explorador:

![que muestra de imagen de explorador "se actualizó por última vez a e: 11/22/2016 4:53 P.M."](configuration/_static/change.png)

## <a name="in-memory-provider-and-binding-to-a-poco-class"></a>Proveedor en memoria y el enlace a una clase POCO

El ejemplo siguiente muestra cómo usar el proveedor en memoria y enlazar a una clase:

[!code-csharp[Main](configuration/sample/InMemory/Program.cs)]

Valores de configuración se devuelven como cadenas, pero el enlace permite la construcción de objetos. Enlace le permite recuperar objetos POCO o gráficos de objetos incluso todo. El ejemplo siguiente muestra cómo enlazar a `MyWindow` y utiliza el patrón de opciones con una aplicación de MVC de ASP.NET Core:

[!code-csharp[Main](configuration/sample/WebConfigBind/MyWindow.cs)]

[!code-json[Main](configuration/sample/WebConfigBind/appsettings.json)]

Enlazar la clase personalizada de `ConfigureServices` al crear el host:

[!code-csharp[Main](configuration/sample/WebConfigBind/Program.cs?name=snippet1&highlight=3-4)]

Mostrar la configuración de la `HomeController`:

[!code-csharp[Main](configuration/sample/WebConfigBind/Controllers/HomeController.cs)]

### <a name="getvalue"></a>GetValue

El ejemplo siguiente se muestra la [GetValue<T> ](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationbinder#Microsoft_Extensions_Configuration_ConfigurationBinder_GetValue_Microsoft_Extensions_Configuration_IConfiguration_System_Type_System_String_System_Object_) método de extensión:

[!code-csharp[Main](configuration/sample/InMemoryGetValue/Program.cs?highlight=27-29)]

El ConfigurationBinder `GetValue<T>` método le permite especificar un valor predeterminado (80 en el ejemplo). `GetValue<T>`es para escenarios sencillos y no se enlaza con secciones completas. `GetValue<T>`Obtiene los valores escalares de `GetSection(key).Value` convertir a un tipo específico.

## <a name="binding-to-an-object-graph"></a>Enlazar a un gráfico de objetos

Puede enlazar de forma recursiva para cada objeto en una clase. Tenga en cuenta la siguiente `AppOptions` clase:

[!code-csharp[Main](configuration/sample/ObjectGraph/AppOptions.cs)]

El ejemplo siguiente se enlaza a la `AppOptions` clase:

[!code-csharp[Main](configuration/sample/ObjectGraph/Program.cs?highlight=15-16)]

**Núcleo de ASP.NET 1.1** y versiones posteriores pueden usar `Get<T>`, que funciona con secciones completas. `Get<T>`puede ser más convienent que el uso de `Bind`. El código siguiente muestra cómo utilizar `Get<T>` con el ejemplo anterior:

```csharp
var appConfig = config.GetSection("App").Get<AppOptions>();
```

Con los siguientes *appSettings.JSON que se* archivo:

[!code-json[Main](configuration/sample/ObjectGraph/appsettings.json)]

El programa mostrará `Height 11`.

El código siguiente puede utilizarse para la unidad de la configuración de pruebas:

```csharp
[Fact]
public void CanBindObjectTree()
{
    var dict = new Dictionary<string, string>
        {
            {"App:Profile:Machine", "Rick"},
            {"App:Connection:Value", "connectionstring"},
            {"App:Window:Height", "11"},
            {"App:Window:Width", "11"}
        };
    var builder = new ConfigurationBuilder();
    builder.AddInMemoryCollection(dict);
    var config = builder.Build();

    var options = new AppOptions();
    config.GetSection("App").Bind(options);

    Assert.Equal("Rick", options.Profile.Machine);
    Assert.Equal(11, options.Window.Height);
    Assert.Equal(11, options.Window.Width);
    Assert.Equal("connectionstring", options.Connection.Value);
}
```

<a name=custom-config-providers></a>

## <a name="basic-sample-of-entity-framework-custom-provider"></a>Ejemplo básico de proveedor personalizado de Entity Framework

En esta sección, se crea un proveedor de configuración básica que lee los pares nombre / valor de una base de datos usan EF. 

Definir una `ConfigurationValue` entidad para almacenar los valores de configuración en la base de datos:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationValue.cs)]

Agregar un `ConfigurationContext` para almacenar y tener acceso a los valores configurados:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/ConfigurationContext.cs?name=snippet1)]

Cree una clase que implemente [IConfigurationSource](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.iconfigurationsource):

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationSource.cs?highlight=7)]

Crear el proveedor de configuración personalizados heredando de [ConfigurationProvider](https://docs.microsoft.com/aspnet/core/api/microsoft.extensions.configuration.configurationprovider).  El proveedor de configuración inicializa la base de datos cuando está vacía:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkConfigurationProvider.cs?highlight=9,18-31,38-39)]

Cuando se ejecuta el ejemplo, se muestran los valores de resaltado de la base de datos ("value_from_ef_1" y "value_from_ef_2").

Puede agregar un `EFConfigSource` método de extensión para agregar el origen de configuración:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/EntityFrameworkExtensions.cs?highlight=12)]

El código siguiente muestra cómo utilizar la opción de instalación `EFConfigProvider`:

[!code-csharp[Main](configuration/sample/CustomConfigurationProvider/Program.cs?highlight=21-26)]

Tenga en cuenta el ejemplo agrega personalizado `EFConfigProvider` una vez que el proveedor JSON, por lo que cualquier configuración de la base de datos invalidará la configuración de la *appSettings.JSON que se* archivo.

Con los siguientes *appSettings.JSON que se* archivo:

[!code-json[Main](configuration/sample/CustomConfigurationProvider/appsettings.json)]

Se muestra lo siguiente:

```console
key1=value_from_ef_1
key2=value_from_ef_2
key3=value_from_json_3
```

## <a name="commandline-configuration-provider"></a>Proveedor de configuración de la línea de comandos

El ejemplo siguiente habilita al proveedor de configuración de la línea de comandos de última:

[!code-csharp[Main](configuration/sample/CommandLine/Program.cs)]

Para pasar valores de configuración, utilice lo siguiente:

```console
dotnet run /Profile:MachineName=Bob /App:MainWindow:Left=1234
```

Que muestra:

```console
Hello Bob
Left 1234
```

El `GetSwitchMappings` método le permite usar `-` en lugar de `/` y elimina los prefijos de subclaves iniciales. Por ejemplo:

```console
dotnet run -MachineName=Bob -Left=7734
```

Muestra:

```console
Hello Bob
Left 7734
```

Argumentos de línea de comandos deben incluir un valor (puede ser null). Por ejemplo:

```console
dotnet run /Profile:MachineName=
```

Es correcto, pero

```console
dotnet run /Profile:MachineName
```

se produce una excepción. Si especifica un prefijo de modificador de línea de comandos de - o bien para que no hay ninguna asignación de conmutador correspondiente, se producirá una excepción.

## <a name="the-webconfig-file"></a>El archivo web.config

A *web.config* archivo es necesario cuando se hospeda la aplicación en IIS o IIS Express. *Web.config* activa el AspNetCoreModule en IIS para iniciar la aplicación. Configuración de *web.config* habilitar la AspNetCoreModule en IIS para que inicie la aplicación y configurar otros módulos y la configuración de IIS. Si usa Visual Studio y eliminar *web.config*, Visual Studio creará uno nuevo.

### <a name="additional-notes"></a>Notas adicionales

* Inyección de dependencia (DI) no se establece hasta después de `ConfigureServices` se invoca.
* El sistema de configuración no es compatible con DI.
* `IConfiguration`tiene dos especializaciones:
  * `IConfigurationRoot`Se usa para el nodo raíz. Puede desencadenar una recarga.
  * `IConfigurationSection`Representa una sección de valores de configuración. El `GetSection` y `GetChildren` métodos devuelven un `IConfigurationSection`.

### <a name="additional-resources"></a>Recursos adicionales

* [Trabajar con varios entornos](environments.md)
* [Almacenamiento seguro de secretos de aplicación durante el desarrollo](../security/app-secrets.md)
* [Inserción de dependencias](dependency-injection.md)
* [Proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration)
