---
title: Migrar de ASP.NET a ASP.NET Core 2.0
author: isaac2004
description: "Este documento de referencia proporciona orientación para migrar aplicaciones existentes de ASP.NET MVC o Web API principales de ASP.NET 2.0."
keywords: "Núcleo de ASP.NET, MVC, migrar"
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc2
ms.openlocfilehash: e0691b276b63ee12d3163ac48d1392696fb97aa6
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/12/2017
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a>Migrar de ASP.NET a ASP.NET Core 2.0

Por [Isaac Levin](https://isaaclevin.com)

Este artículo sirve como una guía de referencia para migrar aplicaciones de ASP.NET a ASP.NET Core 2.0.

## <a name="prerequisites"></a>Requisitos previos

* [.NET core 2.0.0 SDK](https://www.microsoft.com/net/core) o una versión posterior.

## <a name="target-frameworks"></a>Versiones de .NET Framework de destino
Proyectos de ASP.NET Core 2.0 proporcionan a los desarrolladores la flexibilidad de destino es .NET Core, .NET Framework o ambos. Vea [elegir entre .NET Core y .NET Framework para las aplicaciones de servidor](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) para determinar qué plataforma de destino es más adecuado.

Cuando el destino es .NET Framework, proyectos necesitan hacer referencia a paquetes de NuGet individuales.

Como destino .NET Core le permite eliminar numerosas referencias del paquete explícita, gracias a la versión 2.0 de ASP.NET Core [metapackage](xref:fundamentals/metapackage). Instalar el `Microsoft.AspNetCore.All` metapackage en el proyecto:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

Cuando se utiliza el metapackage, ningún paquete que se hace referencia en el metapackage se implementa con la aplicación. El almacén de tiempo de ejecución de .NET Core incluye estos activos, y se precompilan para mejorar el rendimiento. Vea [Microsoft.AspNetCore.All metapackage para ASP.NET Core 2.x](xref:fundamentals/metapackage) para obtener más detalles.

## <a name="project-structure-differences"></a>Diferencias de la estructura de proyecto
El *.csproj* formato de archivo se ha simplificado en ASP.NET Core. Algunos cambios importantes son:
- Inclusión explícita de archivos no es necesario para que se consideran parte del proyecto. Esto reduce el riesgo de conflictos de combinación XML cuando se trabaja en equipos grandes.
- No quedan referencias basada en GUID a otros proyectos, lo cual mejora la legibilidad del archivo.
- El archivo se puede modificar sin descargarlo en Visual Studio:

    ![Editar la opción de menú contextual CSPROJ en Visual Studio de 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Reemplazar el archivo global.asax
ASP.NET Core introdujo un nuevo mecanismo para arrancar una aplicación. El punto de entrada para las aplicaciones ASP.NET es la *Global.asax* archivo. Las tareas como la configuración de enrutamiento y registros de filtro y de área se controlan en el *Global.asax* archivo.

[!code-csharp[Main](samples/globalasax-sample.cs)]

Este enfoque se acopla con la aplicación y el servidor en el que se ha implementado de forma que interfiere con la implementación. En un esfuerzo por desacoplar, [OWIN](http://owin.org/) se introdujo para proporcionar una forma más limpia usar conjuntamente varios marcos de trabajo. OWIN proporciona una canalización para agregar solo los módulos necesarios. El entorno de hospedaje toma un [inicio](xref:fundamentals/startup) función para configurar servicios y la canalización de solicitud de la aplicación. `Startup`registra un conjunto de middleware en la aplicación. Para cada solicitud, la aplicación llama a cada uno de los los componentes de middleware con el puntero principal de una lista vinculada a un conjunto de controladores existente. Cada componente de middleware puede agregar uno o varios controladores a la solicitud de control de canalización. Esto se consigue mediante la devolución de una referencia al controlador que es el nuevo encabezado de la lista. Cada controlador es responsable de recordar e invocar el controlador siguiente en la lista. Con ASP.NET Core, es el punto de entrada a una aplicación `Startup`, y ya no tienen una dependencia en *Global.asax*. Cuando use OWIN con .NET Framework, usar algo parecido a lo siguiente como una canalización:

[!code-csharp[Main](samples/webapi-owin.cs)]

Esto configura las rutas predeterminadas y tiene como valor predeterminado XmlSerialization en Json. Agregue otro Middleware para esta canalización según sea necesario (cargar servicios, opciones de configuración, archivos estáticos, etcetera.).

ASP.NET Core usa un enfoque similar, pero no depende de OWIN para controlar la entrada. En su lugar, que se realiza a través de la *Program.cs* `Main` (método) (similar a las aplicaciones de consola) y `Startup` se carga a través de allí.

[!code-csharp[Main](samples/program.cs)]

`Startup`debe incluir un `Configure` método. En `Configure`, agregar el middleware necesario a la canalización. En el ejemplo siguiente (desde la plantilla de sitio web predeterminado), varios métodos de extensión se utilizan para configurar la canalización con compatibilidad para:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Páginas de errores
* Archivos estáticos
* Núcleo de ASP.NET MVC
* Identidad

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

El host y la aplicación se han separado, lo que proporciona la flexibilidad de pasar a una plataforma diferente en el futuro.

**Nota:** encontrará una referencia más detallada sobre ASP.NET Core inicio y Middleware, [inicio en ASP.NET Core](xref:fundamentals/startup)

## <a name="storing-configurations"></a>Almacenar configuraciones de
ASP.NET es compatible con almacenamiento de la configuración. Esta configuración se utilizan, por ejemplo, para admitir el entorno en el que se implementaron las aplicaciones. Era una práctica común almacenar todos los pares de clave y valor personalizados en el `<appSettings>` sección de la *Web.config* archivo:

[!code-xml[Main](samples/webconfig-sample.xml)]

Aplicaciones leen estos valores mediante la `ConfigurationManager.AppSettings` colección en el `System.Configuration` espacio de nombres:

[!code-csharp[Main](samples/read-webconfig.cs)]

ASP.NET Core puede almacenar datos de configuración de la aplicación en cualquier archivo y cargarlos como parte de arranque de middleware. El archivo predeterminado que se usa en las plantillas de proyecto es *appSettings.JSON que se*:

[!code-json[Main](samples/appsettings-sample.json)]

Cargar este archivo en una instancia de `IConfiguration` dentro de la aplicación se realiza en *Startup.cs*:

[!code-csharp[Main](samples/startup-builder.cs)]

La aplicación lee de `Configuration` para obtener la configuración:

[!code-csharp[Main](samples/read-appsettings.cs)]

Existen extensiones de este enfoque para hacer que el proceso sea más sólida, como el uso de [inyección de dependencia](xref:fundamentals/dependency-injection) (DI) para cargar un servicio con estos valores. El enfoque de DI proporciona un conjunto fuertemente tipada de objetos de configuración.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Nota:** para una referencia más detallada para la configuración básica de ASP.NET, vea [configuración en ASP.NET Core](xref:fundamentals/configuration).

## <a name="native-dependency-injection"></a>Inyección de dependencia nativo
Un objetivo importante al compilar aplicaciones grandes y escalables, es el acoplamiento flexible de componentes y servicios. [Inyección de dependencia](xref:fundamentals/dependency-injection) es una técnica popular para lograr esto, y es un componente nativo de ASP.NET Core.

En las aplicaciones ASP.NET, los desarrolladores se basan en una biblioteca de terceros para implementar la inserción de dependencias. Es una de estas bibliotecas [Unity](https://github.com/unitycontainer/unity), suministrado por Microsoft Patterns & Practices. 

Está implementando un ejemplo de configuración de la inserción de dependencias con Unity `IDependencyResolver` que encapsula un `UnityContainer`:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Cree una instancia de la `UnityContainer`, registrar el servicio y establecer la resolución de dependencia de `HttpConfiguration` a la nueva instancia de `UnityResolver` para el contenedor:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Insertar `IProductRepository` cuando sea necesario:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Dado que inyección de dependencia es parte del núcleo de ASP.NET, puede agregar el servicio en la `ConfigureServices` método *Startup.cs*:

[!code-csharp[Main](samples/configure-services.cs)]

El repositorio se puede insertar en cualquier lugar, como ocurría con Unity.

**Nota:** para una referencia detallada a la inyección de dependencia en ASP.NET Core, vea [inyección de dependencia en ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container)

## <a name="serving-static-files"></a>Enviar archivos estáticos
Una parte importante del desarrollo web es la capacidad para atender activos estáticos, de cliente. Los ejemplos más comunes de los archivos estáticos son HTML, CSS, Javascript e imágenes. Estos archivos se deben guardar en la ubicación de publicación de la aplicación (o la red CDN), y hace referencia por lo que se pueden cargar por una solicitud. Este proceso ha cambiado en ASP.NET Core.

En ASP.NET, archivos estáticos se almacenan en directorios distintos y se hace referencia en las vistas.

En el núcleo de ASP.NET, archivos estáticos se almacenan en la raíz de web de"" (*&lt;raíz del contenido&gt;/wwwroot*), a menos que configure de otra manera. Los archivos se cargan en la canalización de solicitud invocando la `UseStaticFiles` método de extensión de `Startup.Configure`:

[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

**Nota:** si el destino es .NET Framework, instale el paquete NuGet `Microsoft.AspNetCore.StaticFiles`.

Por ejemplo, un recurso de imagen en el *wwwroot/imágenes* carpeta es accesible para el explorador en una ubicación como `http://<app>/images/<imageFileName>`.

**Nota:** para una referencia más detallada para enviar archivos estáticos en ASP.NET Core, vea [Introducción a trabajar con archivos estáticos en ASP.NET Core](xref:fundamentals/static-files).

## <a name="additional-resources"></a>Recursos adicionales
* [Migrar las bibliotecas de .NET Core](https://docs.microsoft.com/dotnet/core/porting/libraries)
