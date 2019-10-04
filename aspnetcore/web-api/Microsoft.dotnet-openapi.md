---
title: Desarrollo de aplicaciones ASP.NET Core con OpenAPI
author: ryanbrandenburg
description: Muestra cómo usar la herramienta "Microsoft.dotnet-openapi" para agregar referencias a archivos OpenAPI.
ms.author: rybrande
ms.date: 09/26/2019
monikerRange: '>= aspnetcore-3.0'
uid: web-api/Microsoft.dotnet-openapi
ms.openlocfilehash: f5eae9e871bc8efc30d500769adb845ff244a90c
ms.sourcegitcommit: e644258c95dd50a82284f107b9bf3becbc43b2b2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/26/2019
ms.locfileid: "71317771"
---
# <a name="develop-aspnet-core-apps-using-openapi-tools"></a>Desarrollo de aplicaciones ASP.NET Core con herramientas de OpenAPI

Por Ryan Brandenburg

[Microsoft.dotnet-openapi](https://www.nuget.org/packages/Microsoft.dotnet-openapi) es una [herramienta global de .NET Core](/dotnet/core/tools/global-tools) para administrar las referencias de [OpenAPI](https://github.com/OAI/OpenAPI-Specification) dentro de un proyecto.

## <a name="installation"></a>Instalación

Para instalar `Microsoft.dotnet-openapi`, ejecute el siguiente comando:

```dotnetcli
dotnet tool install -g Microsoft.dotnet-openapi
```

## <a name="add"></a>Agregar

Al agregar una referencia de OpenAPI mediante cualquiera de los comandos de esta página, se agrega un elemento `<OpenApiReference />` similar al siguiente al archivo *.csproj*:

```xml
<OpenApiReference Include="openapi.json" />
```

La referencia anterior es necesaria para que la aplicación llame al código de cliente generado.

<!-- TODO: Restore after https://github.com/aspnet/AspNetCore/issues/12738
### Add Project

#### Options

| Short option | Long option | Description | Example |
|-------|------|-------|---------|
| -v|--verbose | Show verbose output. |dotnet openapi add project *-v* ../Ref/ProjRef.csproj |
| -p|--project | The project to operate on. |dotnet openapi add project *--project .\Ref.csproj* ../Ref/ProjRef.csproj |

#### Arguments

|  Argument  | Description | Example |
|-------------|-------------|---------|
| source-file | The source to create a reference from. Must be a project file. |dotnet openapi add project *../Ref/ProjRef.csproj* | -->

### <a name="add-file"></a>Agregar archivo

#### <a name="options"></a>Opciones

| Opción corta| Opción larga| DESCRIPCIÓN | Ejemplo |
|-------|------|-------|---------|
| -v|--verbose | Mostrar resultado detallado. |dotnet openapi add file *-v* .\OpenAPI.json |
| -p|--updateProject | El proyecto sobre el que actuar. |dotnet openapi add file *--updateProject .\Ref.csproj* .\OpenAPI.json |
| -c|--code-generator| El generador de código que se va a aplicar a la referencia. Las opciones son `NSwagCSharp` y `NSwagTypeScript`. Si no se especifica `--code-generator`, la herramienta usa `NSwagCSharp` de forma predeterminada.|dotnet openapi add file .\OpenApi.json --code-generator
| -h|--help|Mostrar información de ayuda.|dotnet openapi add file --help|

#### <a name="arguments"></a>Argumentos

|  Argumento  | DESCRIPCIÓN | Ejemplo |
|-------------|-------------|---------|
| source-file | El origen a partir del cual se va a crear una referencia. Debe ser un archivo de OpenAPI. |dotnet openapi add file *.\OpenAPI.json* |

### <a name="add-url"></a>Agregar dirección URL

#### <a name="options"></a>Opciones

| Opción corta| Opción larga| DESCRIPCIÓN | Ejemplo |
|-------|------|-------------|---------|
| -v|--verbose | Mostrar resultado detallado. |dotnet openapi add url *-v* `https://contoso.com/openapi.json` |
| -p|--updateProject | El proyecto sobre el que actuar. |dotnet openapi add url *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json` |
| -o|--output-file | Dónde colocar la copia local del archivo OpenAPI. |dotnet openapi add url `https://contoso.com/openapi.json` *--output-file myclient.json* |
| -c|--code-generator| El generador de código que se va a aplicar a la referencia. Las opciones son `NSwagCSharp` y `NSwagTypeScript`. |dotnet openapi add file .\OpenApi.json --code-generator
| -h|--help|Mostrar información de ayuda.|dotnet openapi add url --help|

#### <a name="arguments"></a>Argumentos

|  Argumento  | DESCRIPCIÓN | Ejemplo |
|-------------|-------------|---------|
| source-URL | El origen a partir del cual se va a crear una referencia. Debe ser una dirección URL. |dotnet openapi add url `https://contoso.com/openapi.json` |

## <a name="remove"></a>Quitar

Quita la referencia de OpenAPI que coincide con el nombre de archivo dado del archivo *.csproj*. Cuando la referencia de OpenAPI se quita, no se generarán los clientes. Los archivos *.json* y *.yaml* locales se eliminan.

### <a name="options"></a>Opciones

| Opción corta| Opción larga| DESCRIPCIÓN| Ejemplo |
|-------|------|------------|---------|
| -v|--verbose | Mostrar resultado detallado. |dotnet openapi remove *-v*|
| -p|--updateProject | El proyecto sobre el que actuar. |dotnet openapi remove *--updateProject .\Ref.csproj* .\OpenAPI.json |
| -h|--help|Mostrar información de ayuda.|dotnet openapi remove --help|

### <a name="arguments"></a>Argumentos

|  Argumento  | DESCRIPCIÓN| Ejemplo |
| ------------|------------|---------|
| source-file | Origen al que se va a quitar la referencia. |dotnet openapi remove *.\OpenAPI.json* |

## <a name="refresh"></a>Refresh

Actualiza la versión local de un archivo que se descargó usando el contenido más reciente de la dirección URL de descarga.

### <a name="options"></a>Opciones

| Opción corta| Opción larga| DESCRIPCIÓN | Ejemplo |
|-------|------|-------------|---------|
| -v|--verbose | Mostrar resultado detallado. | dotnet openapi refresh *-v* `https://contoso.com/openapi.json` |
| -p|--updateProject | El proyecto sobre el que actuar. | dotnet openapi refresh *--updateProject .\Ref.csproj* `https://contoso.com/openapi.json` |
| -h|--help|Mostrar información de ayuda.|dotnet openapi refresh --help|

### <a name="arguments"></a>Argumentos

|  Argumento  | DESCRIPCIÓN | Ejemplo |
| ------------|-------------|---------|
| source-URL | Dirección URL desde la que se va a actualizar la referencia. | dotnet openapi refresh `https://contoso.com/openapi.json` |
