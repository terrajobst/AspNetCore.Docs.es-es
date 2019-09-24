---
title: Bibliotecas de clases de componentes de ASP.NET Core Razor
author: guardrex
description: Descubra cómo se pueden incluir los componentes en aplicaciones increíbles desde una biblioteca de componentes externos.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/23/2019
uid: blazor/class-libraries
ms.openlocfilehash: d9ef276357e95d97b7d89427c5e237aceea7a0d3
ms.sourcegitcommit: 79eeb17604b536e8f34641d1e6b697fb9a2ee21f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/24/2019
ms.locfileid: "71207101"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="c9807-103">Bibliotecas de clases de componentes de ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="c9807-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="c9807-104">Por [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="c9807-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="c9807-105">Los componentes se pueden compartir en una [biblioteca de clases de Razor (RCL)](xref:razor-pages/ui-class) en todos los proyectos.</span><span class="sxs-lookup"><span data-stu-id="c9807-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="c9807-106">Una *biblioteca de clases de componentes de Razor* se puede incluir desde:</span><span class="sxs-lookup"><span data-stu-id="c9807-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="c9807-107">Otro proyecto de la solución.</span><span class="sxs-lookup"><span data-stu-id="c9807-107">Another project in the solution.</span></span>
* <span data-ttu-id="c9807-108">Un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="c9807-108">A NuGet package.</span></span>
* <span data-ttu-id="c9807-109">Biblioteca de .NET A la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="c9807-109">A referenced .NET library.</span></span>

<span data-ttu-id="c9807-110">Del mismo modo que los componentes son tipos regulares de .NET, los componentes proporcionados por un RCL son ensamblados .NET normales.</span><span class="sxs-lookup"><span data-stu-id="c9807-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="c9807-111">Creación de un RCL</span><span class="sxs-lookup"><span data-stu-id="c9807-111">Create an RCL</span></span>

<span data-ttu-id="c9807-112">Siga las instrucciones <xref:blazor/get-started> del artículo para configurar el entorno para el increíble.</span><span class="sxs-lookup"><span data-stu-id="c9807-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c9807-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c9807-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="c9807-114">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="c9807-114">Create a new project.</span></span>
1. <span data-ttu-id="c9807-115">Seleccione **biblioteca de clases de Razor**.</span><span class="sxs-lookup"><span data-stu-id="c9807-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="c9807-116">Seleccione **Siguiente**.</span><span class="sxs-lookup"><span data-stu-id="c9807-116">Select **Next**.</span></span>
1. <span data-ttu-id="c9807-117">En el cuadro de diálogo **crear una nueva biblioteca de clases de Razor** , seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="c9807-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="c9807-118">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="c9807-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="c9807-119">En los ejemplos de este tema se usa el `MyComponentLib1`nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="c9807-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="c9807-120">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="c9807-120">Select **Create**.</span></span>
1. <span data-ttu-id="c9807-121">Agregue RCL a una solución:</span><span class="sxs-lookup"><span data-stu-id="c9807-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="c9807-122">Haga clic con el botón secundario en la solución.</span><span class="sxs-lookup"><span data-stu-id="c9807-122">Right-click the solution.</span></span> <span data-ttu-id="c9807-123">Seleccione **Agregar** > **proyecto existente**.</span><span class="sxs-lookup"><span data-stu-id="c9807-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="c9807-124">Navegue hasta el archivo de proyecto de RCL.</span><span class="sxs-lookup"><span data-stu-id="c9807-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="c9807-125">Seleccione el archivo de proyecto de RCL ( *. csproj*).</span><span class="sxs-lookup"><span data-stu-id="c9807-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="c9807-126">Agregue una referencia a RCL desde la aplicación:</span><span class="sxs-lookup"><span data-stu-id="c9807-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="c9807-127">Haga clic con el botón derecho en el proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c9807-127">Right-click the app project.</span></span> <span data-ttu-id="c9807-128">Seleccione **Agregar** > **referencia**.</span><span class="sxs-lookup"><span data-stu-id="c9807-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="c9807-129">Seleccione el proyecto RCL.</span><span class="sxs-lookup"><span data-stu-id="c9807-129">Select the RCL project.</span></span> <span data-ttu-id="c9807-130">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="c9807-130">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="c9807-131">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="c9807-131">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="c9807-132">Use la plantilla de biblioteca de clases`razorclasslib`de **Razor** () con el comando [dotnet New](/dotnet/core/tools/dotnet-new) en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="c9807-132">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="c9807-133">En el ejemplo siguiente, se crea un RCL denominado `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="c9807-133">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="c9807-134">La carpeta que contiene `MyComponentLib1` se crea automáticamente cuando se ejecuta el comando:</span><span class="sxs-lookup"><span data-stu-id="c9807-134">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```dotnetcli
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="c9807-135">Para agregar la biblioteca a un proyecto existente, use el comando [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="c9807-135">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="c9807-136">En el ejemplo siguiente, el RCL se agrega a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="c9807-136">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="c9807-137">Ejecute el siguiente comando desde la carpeta de proyecto de la aplicación con la ruta de acceso a la biblioteca:</span><span class="sxs-lookup"><span data-stu-id="c9807-137">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```dotnetcli
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="consume-a-library-component"></a><span data-ttu-id="c9807-138">Consumir un componente de biblioteca</span><span class="sxs-lookup"><span data-stu-id="c9807-138">Consume a library component</span></span>

<span data-ttu-id="c9807-139">Para consumir los componentes definidos en una biblioteca de otro proyecto, use cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="c9807-139">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="c9807-140">Utilice el nombre de tipo completo con el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="c9807-140">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="c9807-141">Use la directiva [ \@Using](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="c9807-141">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="c9807-142">Los componentes individuales se pueden agregar por nombre.</span><span class="sxs-lookup"><span data-stu-id="c9807-142">Individual components can be added by name.</span></span>

<span data-ttu-id="c9807-143">En los ejemplos siguientes, `MyComponentLib1` es una biblioteca de componentes que `SalesReport` contiene un componente.</span><span class="sxs-lookup"><span data-stu-id="c9807-143">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="c9807-144">Se `SalesReport` puede hacer referencia al componente usando su nombre de tipo completo con el espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="c9807-144">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="c9807-145">También se puede hacer referencia al componente si la biblioteca se incluye en el ámbito con `@using` una directiva:</span><span class="sxs-lookup"><span data-stu-id="c9807-145">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="c9807-146">Incluya la `@using MyComponentLib1` Directiva en el archivo *_Import. Razor* de nivel superior para que los componentes de la biblioteca estén disponibles para un proyecto completo.</span><span class="sxs-lookup"><span data-stu-id="c9807-146">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="c9807-147">Agregue la Directiva a un archivo *_Import. Razor* en cualquier nivel para aplicar el espacio de nombres a una sola página o a un conjunto de páginas dentro de una carpeta.</span><span class="sxs-lookup"><span data-stu-id="c9807-147">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="c9807-148">Compilar, empaquetar y enviar a NuGet</span><span class="sxs-lookup"><span data-stu-id="c9807-148">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="c9807-149">Dado que las bibliotecas de componentes son bibliotecas estándar de .NET, empaquetarlas y enviarlas a NuGet no es diferente de empaquetar y enviar cualquier biblioteca a NuGet.</span><span class="sxs-lookup"><span data-stu-id="c9807-149">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="c9807-150">El empaquetado se realiza mediante el comando [dotnet Pack](/dotnet/core/tools/dotnet-pack) en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="c9807-150">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```dotnetcli
dotnet pack
```

<span data-ttu-id="c9807-151">Cargue el paquete en NuGet con el comando [dotnet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="c9807-151">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```dotnetcli
dotnet nuget publish
```

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="c9807-152">Crear una biblioteca de clases de componentes de Razor con recursos estáticos</span><span class="sxs-lookup"><span data-stu-id="c9807-152">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="c9807-153">Un RCL puede incluir recursos estáticos.</span><span class="sxs-lookup"><span data-stu-id="c9807-153">An RCL can include static assets.</span></span> <span data-ttu-id="c9807-154">Los recursos estáticos están disponibles para cualquier aplicación que utilice la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="c9807-154">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="c9807-155">Para obtener más información, consulta <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="c9807-155">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c9807-156">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="c9807-156">Additional resources</span></span>

* <xref:razor-pages/ui-class>
