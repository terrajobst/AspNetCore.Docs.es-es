---
title: "Migración de ASP.NET a ASP.NET Core 2.0"
author: isaac2004
description: "Este documento de referencia proporciona orientación para migrar aplicaciones existentes de ASP.NET MVC o ASP.NET Web API a ASP.NET Core 2.0."
keywords: ASP.NET Core,MVC,migrar
ms.author: scaddie
manager: wpickett
ms.date: 08/27/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/proper-to-2x/index
ms.openlocfilehash: 174ce7c99b8b1b65544f931dbb373a4829a64539
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/29/2017
---
# <a name="migrating-from-aspnet-to-aspnet-core-20"></a>Migración de ASP.NET a ASP.NET Core 2.0

Por [Isaac Levin](https://isaaclevin.com)

Este artículo sirve de guía de referencia para migrar aplicaciones de ASP.NET a ASP.NET Core 2.0.

## <a name="prerequisites"></a>Requisitos previos

* [SDK de .NET Core 2.0.0](https://dot.net/core) o versiones posteriores

## <a name="target-frameworks"></a>Versiones de .NET Framework de destino
Los proyectos de ASP.NET Core 2.0 proporcionan a los desarrolladores la flexibilidad de usar la versión .NET Core, .NET Framework o ambas. Vea [Selección entre .NET Core y .NET Framework para aplicaciones de servidor](https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server) para determinar qué plataforma de destino es más adecuada.

Cuando se usa la versión .NET Framework, es necesario que los proyectos hagan referencia a paquetes de NuGet individuales.

Cuando se usa .NET Core, se pueden eliminar numerosas referencias del paquete explícitas, gracias al [metapaquete](xref:fundamentals/metapackage) de ASP.NET Core 2.0. Instale el metapaquete `Microsoft.AspNetCore.All` en el proyecto:

```xml
<ItemGroup>
  <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
</ItemGroup>
```

Cuando se usa el metapaquete, con la aplicación no se implementa ningún paquete al que se hace referencia en el metapaquete. El almacén de tiempo de ejecución de .NET Core incluye estos activos que están compilados previamente para mejorar el rendimiento. Vea [Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x](xref:fundamentals/metapackage) (Metapaquete Microsoft.AspNetCore.All para ASP.NET Core 2.x) para obtener más detalles.

## <a name="project-structure-differences"></a>Diferencias en la estructura de proyecto
El formato de archivo *.csproj* se ha simplificado en ASP.NET Core. Estos son algunos cambios importantes:
- La inclusión explícita de archivos no es necesaria para que se consideren parte del proyecto. Esto reduce el riesgo de conflictos al fusionar XML cuando se trabaja en equipos grandes.
- No hay referencias de GUID a otros proyectos, lo cual mejora la legibilidad del archivo.
- El archivo se puede editar sin descargarlo en Visual Studio:

    ![Edición de la opción de menú contextual CSPROJ en Visual Studio de 2017](_static/EditProjectVs2017.png)

## <a name="globalasax-file-replacement"></a>Reemplazo del archivo Global.asax
ASP.NET Core introdujo un nuevo mecanismo para arrancar una aplicación. El punto de entrada para las aplicaciones ASP.NET es el archivo *Global.asax*. En el archivo *Global.asax* se controlan tareas como la configuración de enrutamiento y los registros de filtro y de área.

[!code-csharp[Main](samples/globalasax-sample.cs)]

Este enfoque acopla la aplicación y el servidor en el que está implementada de forma que interfiere con la implementación. En un esfuerzo por desacoplar, se introdujo [OWIN](http://owin.org/) para ofrecer una forma más limpia de usar varios marcos de trabajo de manera conjunta. OWIN proporciona una canalización para agregar solo los módulos necesarios. El entorno de hospedaje toma una función de [inicio](xref:fundamentals/startup) para configurar servicios y la canalización de solicitud de la aplicación. `Startup` registra un conjunto de middleware en la aplicación. Para cada solicitud, la aplicación llama a cada uno de los componentes de middleware con el puntero principal de una lista vinculada a un conjunto de controladores existente. Cada componente de middleware puede agregar uno o varios controladores a la canalización de control de la solicitud. Esto se consigue mediante la devolución de una referencia al controlador que ahora es el primero de la lista. Cada controlador se encarga de recordar e invocar el controlador siguiente en la lista. Con ASP.NET Core, el punto de entrada a una aplicación es `Startup` y ya no se tiene dependencia de *Global.asax*. Cuando utilice OWIN con .NET Framework, use algo parecido a lo siguiente como canalización:

[!code-csharp[Main](samples/webapi-owin.cs)]

Esto configura las rutas predeterminadas y tiene como valor predeterminado XmlSerialization a través de Json. Agregue otro middleware a esta canalización según sea necesario (carga de servicios, opciones de configuración, archivos estáticos, etcétera).

ASP.NET Core usa un enfoque similar, pero no depende de OWIN para controlar la entrada. En lugar de eso, usa el método *Program.cs* `Main` (similar a las aplicaciones de consola) y `Startup` se carga a través de ahí.

[!code-csharp[Main](samples/program.cs)]

`Startup` debe incluir un método `Configure`. En `Configure`, agregue el middleware necesario a la canalización. En el ejemplo siguiente (de la plantilla de sitio web predeterminada), se usan varios métodos de extensión para configurar la canalización con compatibilidad para:

* [BrowserLink](http://vswebessentials.com/features/browserlink)
* Páginas de error
* Archivos estáticos
* ASP.NET Core MVC
* Identidad

[!code-csharp[Main](../../common/samples/WebApplication1/Startup.cs?highlight=8,9,10,14,17,19,21&start=58&end=84)]

El host y la aplicación se han desacoplado, lo que proporciona la flexibilidad de pasar a una plataforma diferente en el futuro.

**Nota:** Para una referencia más detallada sobre el inicio de ASP.NET Core y middleware en [Startup in ASP.NET Core](xref:fundamentals/startup) (Inicio de aplicaciones en ASP.NET Core).

## <a name="storing-configurations"></a>Almacenamiento de configuraciones
ASP.NET admite el almacenamiento de valores de configuración. Estos valores de configuración se usan, por ejemplo, para admitir el entorno donde se implementan las aplicaciones. Antes se solían almacenar todos los pares de clave y valor personalizados en la sección `<appSettings>` del archivo *Web.config*:

[!code-xml[Main](samples/webconfig-sample.xml)]

Las aplicaciones leen esta configuración mediante la colección `ConfigurationManager.AppSettings` en el espacio de nombres `System.Configuration`:

[!code-csharp[Main](samples/read-webconfig.cs)]

ASP.NET Core puede almacenar datos de configuración para la aplicación en cualquier archivo y cargarlos como parte del arranque de middleware. El archivo predeterminado que se usa en las plantillas de proyecto es *appSettings.JSON*:

[!code-json[Main](samples/appsettings-sample.json)]

La carga de este archivo en una instancia de `IConfiguration` dentro de la aplicación se realiza en *Startup.cs*:

[!code-csharp[Main](samples/startup-builder.cs)]

La aplicación lee de `Configuration` para obtener la configuración:

[!code-csharp[Main](samples/read-appsettings.cs)]

Existen extensiones de este enfoque para lograr que el proceso sea más sólido, como el uso de [inserción de dependencias](xref:fundamentals/dependency-injection) para cargar un servicio con estos valores. El enfoque de la inserción de dependencias proporciona un conjunto fuertemente tipado de objetos de configuración.

````csharp
// Assume AppConfiguration is a class representing a strongly-typed version of AppConfiguration section
services.Configure<AppConfiguration>(Configuration.GetSection("AppConfiguration"));
````

**Nota:** Para una referencia más detallada sobre la configuración de ASP.NET Core, vea [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) (Configuración en ASP.NET Core).

## <a name="native-dependency-injection"></a>Inserción de dependencias nativa
Un objetivo importante al compilar aplicaciones grandes y escalables es lograr el acoplamiento flexible de los componentes y los servicios. La [inserción de dependencias](xref:fundamentals/dependency-injection) es una técnica popular para lograrlo y se trata de un componente nativo de ASP.NET Core.

En las aplicaciones ASP.NET, los desarrolladores se basan en una biblioteca de terceros para implementar la inserción de dependencias. Una de estas bibliotecas es [Unity](https://github.com/unitycontainer/unity), suministrada por Microsoft Patterns & Practices. 

Un ejemplo de configuración de la inserción de dependencias con Unity consiste en implementar `IDependencyResolver` que encapsula un `UnityContainer`:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample8.cs)]

Cree una instancia de `UnityContainer`, registre el servicio y establezca la resolución de dependencias de `HttpConfiguration` en la nueva instancia de `UnityResolver` para el contenedor:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample9.cs)]

Inserte `IProductRepository` cuando sea necesario:

[!code-csharp[Main](../../../aspnet/web-api/overview/advanced/dependency-injection/samples/sample5.cs)]

Dado que la inserción de dependencia forma parte de ASP.NET Core, puede agregar el servicio en el método `ConfigureServices` de *Startup.cs*:

[!code-csharp[Main](samples/configure-services.cs)]

El repositorio se puede insertar en cualquier lugar, como ocurría con Unity.

**Nota:** Para una referencia detallada sobre la inserción de dependencias en ASP.NET Core, vea [Dependency Injection in ASP.NET Core](xref:fundamentals/dependency-injection#replacing-the-default-services-container) (Inserción de dependencias en ASP.NET Core).

## <a name="serving-static-files"></a>Trabajar con archivos estáticos
Una parte importante del desarrollo web es la capacidad de trabajar con activos estáticos de cliente. Los ejemplos más comunes de archivos estáticos son HTML, CSS, JavaScript e imágenes. Estos archivos deben guardarse en la ubicación de publicación de la aplicación (o la red de entrega de contenido) y es necesario hacer referencia a ellos para que una solicitud los pueda cargar. Este proceso ha cambiado en ASP.NET Core.

En ASP.NET, los archivos estáticos se almacenan en directorios distintos y se hace referencia a ellos en las vistas.

En ASP.NET Core, los archivos estáticos se almacenan en la "raíz web" (*&lt;raíz del contenido&gt;/wwwroot*), a menos que se configuren de otra manera. Los archivos se cargan en la canalización de solicitud mediante la invocación del método de extensión `UseStaticFiles` desde `Startup.Configure`:

[!code-csharp[Main](../../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]

**Nota:** Si el destino es .NET Framework, debe instalarse el paquete de NuGet `Microsoft.AspNetCore.StaticFiles`.

Por ejemplo, el explorador puede acceder a un recurso de imagen en la carpeta *wwwroot/images* en una ubicación como `http://<app>/images/<imageFileName>`.

**Nota:** Para una referencia más detallada sobre cómo trabajar con archivos estáticos en ASP.NET Core, vea [Introduction to working with static files in ASP.NET Core](xref:fundamentals/static-files) (Introducción sobre cómo trabajar con archivos estáticos en ASP.NET Core).

## <a name="additional-resources"></a>Recursos adicionales
* [Traslado a .NET Core: bibliotecas](https://docs.microsoft.com/dotnet/core/porting/libraries)
