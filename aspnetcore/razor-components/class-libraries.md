---
title: Bibliotecas de clases de componentes de Razor
author: guardrex
description: Descubra cómo los componentes se pueden incluir en las aplicaciones de componentes de Razor de una biblioteca de componentes externos.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/14/2019
uid: razor-components/class-libraries
ms.openlocfilehash: 1064ad60d90af15af483ba9bca5ed85fb63c2924
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/19/2019
ms.locfileid: "59515591"
---
# <a name="razor-components-class-libraries"></a><span data-ttu-id="1473f-103">Bibliotecas de clases de componentes de Razor</span><span class="sxs-lookup"><span data-stu-id="1473f-103">Razor Components Class Libraries</span></span>

<span data-ttu-id="1473f-104">Por [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="1473f-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="1473f-105">Los componentes se pueden compartir en las bibliotecas de clases de Razor entre proyectos.</span><span class="sxs-lookup"><span data-stu-id="1473f-105">Components can be shared in Razor class libraries across projects.</span></span> <span data-ttu-id="1473f-106">Los componentes se pueden incluidos desde:</span><span class="sxs-lookup"><span data-stu-id="1473f-106">Components can be included from:</span></span>

* <span data-ttu-id="1473f-107">Otro proyecto en la solución.</span><span class="sxs-lookup"><span data-stu-id="1473f-107">Another project in the solution.</span></span>
* <span data-ttu-id="1473f-108">Un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="1473f-108">A NuGet package.</span></span>
* <span data-ttu-id="1473f-109">Biblioteca de .NET que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="1473f-109">A referenced .NET library.</span></span>

<span data-ttu-id="1473f-110">Como los componentes son tipos de .NET normales, los componentes proporcionados por las bibliotecas de clases de Razor son ensamblados de .NET normales.</span><span class="sxs-lookup"><span data-stu-id="1473f-110">Just as components are regular .NET types, components provided by Razor class libraries are normal .NET assemblies.</span></span>

<span data-ttu-id="1473f-111">Use la `razorclasslib` plantilla (biblioteca de clases de Razor) con el [dotnet nuevo](/dotnet/core/tools/dotnet-new) comando:</span><span class="sxs-lookup"><span data-stu-id="1473f-111">Use the `razorclasslib` (Razor class library) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command:</span></span>

```console
dotnet new razorclasslib -o MyComponentLib1
```

<span data-ttu-id="1473f-112">Agregar archivos de componentes de Razor (*.razor*) a la biblioteca de clases de Razor.</span><span class="sxs-lookup"><span data-stu-id="1473f-112">Add Razor Component files (*.razor*) to the Razor class library.</span></span>

<span data-ttu-id="1473f-113">Para agregar la biblioteca a un proyecto existente, use el [dotnet sln](/dotnet/core/tools/dotnet-sln) comando:</span><span class="sxs-lookup"><span data-stu-id="1473f-113">To add the library to an existing project, use the [dotnet sln](/dotnet/core/tools/dotnet-sln) command:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="1473f-114">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="1473f-114">Visual Studio</span></span>](#tab/visual-studio)

```console
dotnet sln add .\MyComponentLib1
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="1473f-115">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="1473f-115">.NET Core CLI</span></span>](#tab/netcore-cli)

```console
dotnet add WebApplication1 reference MyComponentLib1
```

---

> [!NOTE]
> <span data-ttu-id="1473f-116">Bibliotecas de clases de Razor no son compatibles con las aplicaciones Blazor en ASP.NET Core Preview 3.</span><span class="sxs-lookup"><span data-stu-id="1473f-116">Razor class libraries aren't compatible with Blazor apps in ASP.NET Core Preview 3.</span></span>
>
> <span data-ttu-id="1473f-117">Para crear componentes en una biblioteca que se puede compartir con aplicaciones Blazor y componentes de Razor, use una biblioteca de clases Blazor creada por el `blazorlib` plantilla.</span><span class="sxs-lookup"><span data-stu-id="1473f-117">To create components in a library that can be shared with Blazor and Razor Components apps, use a Blazor class library created by the `blazorlib` template.</span></span>
>
> <span data-ttu-id="1473f-118">Bibliotecas de clases de Razor no admiten recursos estáticos en ASP.NET Core Preview 3.</span><span class="sxs-lookup"><span data-stu-id="1473f-118">Razor class libraries don't support static assets in ASP.NET Core Preview 3.</span></span> <span data-ttu-id="1473f-119">Las bibliotecas de componentes mediante la `blazorlib` plantilla puede incluir los archivos estáticos, como imágenes, JavaScript y hojas de estilos.</span><span class="sxs-lookup"><span data-stu-id="1473f-119">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="1473f-120">En tiempo de compilación, los archivos estáticos se incrustan en el archivo de ensamblado compilado (*.dll*), que permite el consumo de los componentes sin tener que preocuparse sobre cómo incluir sus recursos.</span><span class="sxs-lookup"><span data-stu-id="1473f-120">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="1473f-121">Los archivos incluidos en el `content` directory se marcan como recurso incrustado.</span><span class="sxs-lookup"><span data-stu-id="1473f-121">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="1473f-122">Consumir un componente de la biblioteca</span><span class="sxs-lookup"><span data-stu-id="1473f-122">Consume a library component</span></span>

<span data-ttu-id="1473f-123">Para consumir los componentes definidos en una biblioteca en otro proyecto, el [ @addTagHelper ](xref:mvc/views/tag-helpers/intro#add-helper-label) se debe usar la directiva.</span><span class="sxs-lookup"><span data-stu-id="1473f-123">In order to consume components defined in a library in another project, the [@addTagHelper](xref:mvc/views/tag-helpers/intro#add-helper-label) directive must be used.</span></span> <span data-ttu-id="1473f-124">Los componentes individuales se pueden agregar por nombre.</span><span class="sxs-lookup"><span data-stu-id="1473f-124">Individual components may be added by name.</span></span>

<span data-ttu-id="1473f-125">El formato general de la directiva es:</span><span class="sxs-lookup"><span data-stu-id="1473f-125">The general format of the directive is:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<Component1 />
```

<span data-ttu-id="1473f-126">Por ejemplo, se agrega la siguiente directiva `Component1` de `MyComponentLib1`:</span><span class="sxs-lookup"><span data-stu-id="1473f-126">For example, the following directive adds `Component1` of `MyComponentLib1`:</span></span>

```cshtml
@addTagHelper MyComponentLib1.Component1, MyComponentLib1
```

<span data-ttu-id="1473f-127">Sin embargo, es común para incluir todos los componentes de un ensamblado con un carácter comodín (`*`):</span><span class="sxs-lookup"><span data-stu-id="1473f-127">However, it's common to include all of the components from an assembly using a wildcard (`*`):</span></span>

```cshtml
@addTagHelper *, MyComponentLib1
```

<span data-ttu-id="1473f-128">El `@addTagHelper` directiva puede incluirse en *_ViewImport.cshtml* para que los componentes disponibles para un proyecto completo o aplicada a una sola página o un conjunto de páginas dentro de una carpeta.</span><span class="sxs-lookup"><span data-stu-id="1473f-128">The `@addTagHelper` directive can be included in *_ViewImport.cshtml* to make the components available for an entire project or applied to a single page or set of pages within a folder.</span></span> <span data-ttu-id="1473f-129">Con el `@addTagHelper` la directiva en su lugar, los componentes de la biblioteca de componentes pueden utilizarse como si estuvieran en el mismo ensamblado que la aplicación.</span><span class="sxs-lookup"><span data-stu-id="1473f-129">With the `@addTagHelper` directive in place, the components of the component library can be consumed as if they were in the same assembly as the app.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="1473f-130">Compilación, pack y envío de NuGet</span><span class="sxs-lookup"><span data-stu-id="1473f-130">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="1473f-131">Dado que las bibliotecas de componentes son bibliotecas de .NET standard, empaquetado y envío a NuGet no es diferente de empaquetado y envío de cualquier biblioteca en NuGet.</span><span class="sxs-lookup"><span data-stu-id="1473f-131">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="1473f-132">Empaquetado se realiza mediante el [dotnet pack](/dotnet/core/tools/dotnet-pack) comando:</span><span class="sxs-lookup"><span data-stu-id="1473f-132">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command:</span></span>

```console
dotnet pack
```

<span data-ttu-id="1473f-133">Cargar el paquete en NuGet mediante la [publicar dotnet nuget](/dotnet/core/tools/dotnet-nuget-push) comando:</span><span class="sxs-lookup"><span data-stu-id="1473f-133">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="1473f-134">Cuando se usa el `blazorlib` , plantilla de recursos estáticos se incluyen en el paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="1473f-134">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="1473f-135">Los consumidores de la biblioteca reciben automáticamente los scripts y hojas de estilos, por lo que no están necesarios instalar manualmente los recursos a los consumidores.</span><span class="sxs-lookup"><span data-stu-id="1473f-135">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>
