---
title: Bibliotecas de clases de componentes de ASP.NET Core Razor
author: guardrex
description: Descubra cómo se pueden incluir los componentes en aplicaciones increíbles desde una biblioteca de componentes externos.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2019
uid: blazor/class-libraries
ms.openlocfilehash: 6e93d48bbc684845952c3db8935ccc8b190044b7
ms.sourcegitcommit: f5f0ff65d4e2a961939762fb00e654491a2c772a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/15/2019
ms.locfileid: "69030341"
---
# <a name="aspnet-core-razor-components-class-libraries"></a><span data-ttu-id="79bd5-103">Bibliotecas de clases de componentes de ASP.NET Core Razor</span><span class="sxs-lookup"><span data-stu-id="79bd5-103">ASP.NET Core Razor components class libraries</span></span>

<span data-ttu-id="79bd5-104">Por [Simon Timms](https://github.com/stimms)</span><span class="sxs-lookup"><span data-stu-id="79bd5-104">By [Simon Timms](https://github.com/stimms)</span></span>

<span data-ttu-id="79bd5-105">Los componentes se pueden compartir en una [biblioteca de clases de Razor (RCL)](xref:razor-pages/ui-class) en todos los proyectos.</span><span class="sxs-lookup"><span data-stu-id="79bd5-105">Components can be shared in a [Razor class library (RCL)](xref:razor-pages/ui-class) across projects.</span></span> <span data-ttu-id="79bd5-106">Una *biblioteca de clases de componentes de Razor* se puede incluir desde:</span><span class="sxs-lookup"><span data-stu-id="79bd5-106">A *Razor components class library* can be included from:</span></span>

* <span data-ttu-id="79bd5-107">Otro proyecto de la solución.</span><span class="sxs-lookup"><span data-stu-id="79bd5-107">Another project in the solution.</span></span>
* <span data-ttu-id="79bd5-108">Un paquete de NuGet.</span><span class="sxs-lookup"><span data-stu-id="79bd5-108">A NuGet package.</span></span>
* <span data-ttu-id="79bd5-109">Biblioteca de .NET A la que se hace referencia.</span><span class="sxs-lookup"><span data-stu-id="79bd5-109">A referenced .NET library.</span></span>

<span data-ttu-id="79bd5-110">Del mismo modo que los componentes son tipos regulares de .NET, los componentes proporcionados por un RCL son ensamblados .NET normales.</span><span class="sxs-lookup"><span data-stu-id="79bd5-110">Just as components are regular .NET types, components provided by an RCL are normal .NET assemblies.</span></span>

## <a name="create-an-rcl"></a><span data-ttu-id="79bd5-111">Creación de un RCL</span><span class="sxs-lookup"><span data-stu-id="79bd5-111">Create an RCL</span></span>

<span data-ttu-id="79bd5-112">Siga las instrucciones <xref:blazor/get-started> del artículo para configurar el entorno para el increíble.</span><span class="sxs-lookup"><span data-stu-id="79bd5-112">Follow the guidance in the <xref:blazor/get-started> article to configure your environment for Blazor.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="79bd5-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="79bd5-113">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="79bd5-114">Cree un nuevo proyecto.</span><span class="sxs-lookup"><span data-stu-id="79bd5-114">Create a new project.</span></span>
1. <span data-ttu-id="79bd5-115">Seleccione **biblioteca de clases de Razor**.</span><span class="sxs-lookup"><span data-stu-id="79bd5-115">Select **Razor Class Library**.</span></span> <span data-ttu-id="79bd5-116">Seleccione **Next** (Siguiente).</span><span class="sxs-lookup"><span data-stu-id="79bd5-116">Select **Next**.</span></span>
1. <span data-ttu-id="79bd5-117">En el cuadro de diálogo **crear una nueva biblioteca de clases de Razor** , seleccione **crear**.</span><span class="sxs-lookup"><span data-stu-id="79bd5-117">In the **Create a new Razor class library** dialog, select **Create**.</span></span>
1. <span data-ttu-id="79bd5-118">Proporcione un nombre para el proyecto en el campo **Nombre del proyecto** o acepte el predeterminado.</span><span class="sxs-lookup"><span data-stu-id="79bd5-118">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="79bd5-119">En los ejemplos de este tema se usa el `MyComponentLib1`nombre del proyecto.</span><span class="sxs-lookup"><span data-stu-id="79bd5-119">The examples in this topic use the project name `MyComponentLib1`.</span></span> <span data-ttu-id="79bd5-120">Seleccione **Crear**.</span><span class="sxs-lookup"><span data-stu-id="79bd5-120">Select **Create**.</span></span>
1. <span data-ttu-id="79bd5-121">Agregue RCL a una solución:</span><span class="sxs-lookup"><span data-stu-id="79bd5-121">Add the RCL to a solution:</span></span>
   1. <span data-ttu-id="79bd5-122">Haga clic con el botón secundario en la solución.</span><span class="sxs-lookup"><span data-stu-id="79bd5-122">Right-click the solution.</span></span> <span data-ttu-id="79bd5-123">Seleccione **Agregar** > **proyecto existente**.</span><span class="sxs-lookup"><span data-stu-id="79bd5-123">Select **Add** > **Existing Project**.</span></span>
   1. <span data-ttu-id="79bd5-124">Navegue hasta el archivo de proyecto de RCL.</span><span class="sxs-lookup"><span data-stu-id="79bd5-124">Navigate to the RCL's project file.</span></span>
   1. <span data-ttu-id="79bd5-125">Seleccione el archivo de proyecto de RCL ( *. csproj*).</span><span class="sxs-lookup"><span data-stu-id="79bd5-125">Select the RCL's project file (*.csproj*).</span></span>
1. <span data-ttu-id="79bd5-126">Agregue una referencia a RCL desde la aplicación:</span><span class="sxs-lookup"><span data-stu-id="79bd5-126">Add a reference the RCL from the app:</span></span>
   1. <span data-ttu-id="79bd5-127">Haga clic con el botón derecho en el proyecto de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="79bd5-127">Right-click the app project.</span></span> <span data-ttu-id="79bd5-128">Seleccione **Agregar** > **referencia**.</span><span class="sxs-lookup"><span data-stu-id="79bd5-128">Select **Add** > **Reference**.</span></span>
   1. <span data-ttu-id="79bd5-129">Seleccione el proyecto RCL.</span><span class="sxs-lookup"><span data-stu-id="79bd5-129">Select the RCL project.</span></span> <span data-ttu-id="79bd5-130">Seleccione **Aceptar**.</span><span class="sxs-lookup"><span data-stu-id="79bd5-130">Select **OK**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="79bd5-131">CLI de .NET Core</span><span class="sxs-lookup"><span data-stu-id="79bd5-131">.NET Core CLI</span></span>](#tab/netcore-cli)

1. <span data-ttu-id="79bd5-132">Use la plantilla de biblioteca de clases`razorclasslib`de **Razor** () con el comando [dotnet New](/dotnet/core/tools/dotnet-new) en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="79bd5-132">Use the **Razor Class Library** template (`razorclasslib`) with the [dotnet new](/dotnet/core/tools/dotnet-new) command in a command shell.</span></span> <span data-ttu-id="79bd5-133">En el ejemplo siguiente, se crea un RCL denominado `MyComponentLib1`.</span><span class="sxs-lookup"><span data-stu-id="79bd5-133">In the following example, an RCL is created named `MyComponentLib1`.</span></span> <span data-ttu-id="79bd5-134">La carpeta que contiene `MyComponentLib1` se crea automáticamente cuando se ejecuta el comando:</span><span class="sxs-lookup"><span data-stu-id="79bd5-134">The folder that holds `MyComponentLib1` is created automatically when the command is executed:</span></span>

   ```console
   dotnet new razorclasslib -o MyComponentLib1
   ```

1. <span data-ttu-id="79bd5-135">Para agregar la biblioteca a un proyecto existente, use el comando [dotnet Add Reference](/dotnet/core/tools/dotnet-add-reference) en un shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="79bd5-135">To add the library to an existing project, use the [dotnet add reference](/dotnet/core/tools/dotnet-add-reference) command in a command shell.</span></span> <span data-ttu-id="79bd5-136">En el ejemplo siguiente, el RCL se agrega a la aplicación.</span><span class="sxs-lookup"><span data-stu-id="79bd5-136">In the following example, the RCL is added to the app.</span></span> <span data-ttu-id="79bd5-137">Ejecute el siguiente comando desde la carpeta de proyecto de la aplicación con la ruta de acceso a la biblioteca:</span><span class="sxs-lookup"><span data-stu-id="79bd5-137">Execute the following command from the app's project folder with the path to the library:</span></span>

   ```console
   dotnet add reference {PATH TO LIBRARY}
   ```

---

## <a name="rcls-not-supported-for-client-side-apps"></a><span data-ttu-id="79bd5-138">RCLs no se admite para las aplicaciones del lado cliente</span><span class="sxs-lookup"><span data-stu-id="79bd5-138">RCLs not supported for client-side apps</span></span>

<span data-ttu-id="79bd5-139">En el ASP.NET Core actual de la versión preliminar de 3,0, las bibliotecas de clases de Razor no son compatibles con las aplicaciones de cliente más increíbles.</span><span class="sxs-lookup"><span data-stu-id="79bd5-139">In the current ASP.NET Core 3.0 Preview, Razor class libraries aren't compatible with Blazor client-side apps.</span></span> <span data-ttu-id="79bd5-140">En el caso de las aplicaciones del lado cliente, use una biblioteca de componentes extraordinaria creada `blazorlib` por la plantilla en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="79bd5-140">For Blazor client-side apps, use a Blazor component library created by the `blazorlib` template in a command shell:</span></span>

```console
dotnet new blazorlib -o MyComponentLib1
```

<span data-ttu-id="79bd5-141">Las bibliotecas de componentes `blazorlib` que usan la plantilla pueden incluir archivos estáticos, como imágenes, JavaScript y hojas de estilos.</span><span class="sxs-lookup"><span data-stu-id="79bd5-141">Component libraries using the `blazorlib` template can include static files, such as images, JavaScript, and stylesheets.</span></span> <span data-ttu-id="79bd5-142">En tiempo de compilación, los archivos estáticos se incrustan en el archivo de ensamblado compilado ( *. dll*), lo que permite el consumo de los componentes sin tener que preocuparse de cómo incluir sus recursos.</span><span class="sxs-lookup"><span data-stu-id="79bd5-142">At build time, static files are embedded into the built assembly file (*.dll*), which allows consumption of the components without having to worry about how to include their resources.</span></span> <span data-ttu-id="79bd5-143">Los archivos incluidos en el `content` directorio se marcan como recursos incrustados.</span><span class="sxs-lookup"><span data-stu-id="79bd5-143">Any files included in the `content` directory are marked as an embedded resource.</span></span>

## <a name="consume-a-library-component"></a><span data-ttu-id="79bd5-144">Consumir un componente de biblioteca</span><span class="sxs-lookup"><span data-stu-id="79bd5-144">Consume a library component</span></span>

<span data-ttu-id="79bd5-145">Para consumir los componentes definidos en una biblioteca de otro proyecto, use cualquiera de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="79bd5-145">In order to consume components defined in a library in another project, use either of the following approaches:</span></span>

* <span data-ttu-id="79bd5-146">Utilice el nombre de tipo completo con el espacio de nombres.</span><span class="sxs-lookup"><span data-stu-id="79bd5-146">Use the full type name with the namespace.</span></span>
* <span data-ttu-id="79bd5-147">Use la directiva [ \@Using](xref:mvc/views/razor#using) de Razor.</span><span class="sxs-lookup"><span data-stu-id="79bd5-147">Use Razor's [\@using](xref:mvc/views/razor#using) directive.</span></span> <span data-ttu-id="79bd5-148">Los componentes individuales se pueden agregar por nombre.</span><span class="sxs-lookup"><span data-stu-id="79bd5-148">Individual components can be added by name.</span></span>

<span data-ttu-id="79bd5-149">En los ejemplos siguientes, `MyComponentLib1` es una biblioteca de componentes que `SalesReport` contiene un componente.</span><span class="sxs-lookup"><span data-stu-id="79bd5-149">In the following examples, `MyComponentLib1` is a component library containing a `SalesReport` component.</span></span>

<span data-ttu-id="79bd5-150">Se `SalesReport` puede hacer referencia al componente usando su nombre de tipo completo con el espacio de nombres:</span><span class="sxs-lookup"><span data-stu-id="79bd5-150">The `SalesReport` component can be referenced using its full type name with namespace:</span></span>

```cshtml
<h1>Hello, world!</h1>

Welcome to your new app.

<MyComponentLib1.SalesReport />
```

<span data-ttu-id="79bd5-151">También se puede hacer referencia al componente si la biblioteca se incluye en el ámbito con `@using` una directiva:</span><span class="sxs-lookup"><span data-stu-id="79bd5-151">The component can also be referenced if the library is brought into scope with an `@using` directive:</span></span>

```cshtml
@using MyComponentLib1

<h1>Hello, world!</h1>

Welcome to your new app.

<SalesReport />
```

<span data-ttu-id="79bd5-152">Incluya la `@using MyComponentLib1` Directiva en el archivo *_Import. Razor* de nivel superior para que los componentes de la biblioteca estén disponibles para un proyecto completo.</span><span class="sxs-lookup"><span data-stu-id="79bd5-152">Include the `@using MyComponentLib1` directive in the top-level *_Import.razor* file to make the library's components available to an entire project.</span></span> <span data-ttu-id="79bd5-153">Agregue la Directiva a un archivo *_Import. Razor* en cualquier nivel para aplicar el espacio de nombres a una sola página o a un conjunto de páginas dentro de una carpeta.</span><span class="sxs-lookup"><span data-stu-id="79bd5-153">Add the directive to an *_Import.razor* file at any level to apply the namespace to a single page or set of pages within a folder.</span></span>

## <a name="build-pack-and-ship-to-nuget"></a><span data-ttu-id="79bd5-154">Compilar, empaquetar y enviar a NuGet</span><span class="sxs-lookup"><span data-stu-id="79bd5-154">Build, pack, and ship to NuGet</span></span>

<span data-ttu-id="79bd5-155">Dado que las bibliotecas de componentes son bibliotecas estándar de .NET, empaquetarlas y enviarlas a NuGet no es diferente de empaquetar y enviar cualquier biblioteca a NuGet.</span><span class="sxs-lookup"><span data-stu-id="79bd5-155">Because component libraries are standard .NET libraries, packaging and shipping them to NuGet is no different from packaging and shipping any library to NuGet.</span></span> <span data-ttu-id="79bd5-156">El empaquetado se realiza mediante el comando [dotnet Pack](/dotnet/core/tools/dotnet-pack) en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="79bd5-156">Packaging is performed using the [dotnet pack](/dotnet/core/tools/dotnet-pack) command in a command shell:</span></span>

```console
dotnet pack
```

<span data-ttu-id="79bd5-157">Cargue el paquete en NuGet con el comando [dotnet NuGet Publish](/dotnet/core/tools/dotnet-nuget-push) en un shell de comandos:</span><span class="sxs-lookup"><span data-stu-id="79bd5-157">Upload the package to NuGet using the [dotnet nuget publish](/dotnet/core/tools/dotnet-nuget-push) command in a command shell:</span></span>

```console
dotnet nuget publish
```

<span data-ttu-id="79bd5-158">Al usar la `blazorlib` plantilla, los recursos estáticos se incluyen en el paquete NuGet.</span><span class="sxs-lookup"><span data-stu-id="79bd5-158">When using the `blazorlib` template, static resources are included in the NuGet package.</span></span> <span data-ttu-id="79bd5-159">Los consumidores de la biblioteca reciben automáticamente scripts y hojas de estilos, por lo que no es necesario que los consumidores instalen manualmente los recursos.</span><span class="sxs-lookup"><span data-stu-id="79bd5-159">Library consumers automatically receive scripts and stylesheets, so consumers aren't required to manually install the resources.</span></span>

## <a name="create-a-razor-components-class-library-with-static-assets"></a><span data-ttu-id="79bd5-160">Crear una biblioteca de clases de componentes de Razor con recursos estáticos</span><span class="sxs-lookup"><span data-stu-id="79bd5-160">Create a Razor components class library with static assets</span></span>

<span data-ttu-id="79bd5-161">Un RCL puede incluir recursos estáticos.</span><span class="sxs-lookup"><span data-stu-id="79bd5-161">An RCL can include static assets.</span></span> <span data-ttu-id="79bd5-162">Los recursos estáticos están disponibles para cualquier aplicación que utilice la biblioteca.</span><span class="sxs-lookup"><span data-stu-id="79bd5-162">The static assets are available to any app that consumes the library.</span></span> <span data-ttu-id="79bd5-163">Para obtener más información, consulta <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span><span class="sxs-lookup"><span data-stu-id="79bd5-163">For more information, see <xref:razor-pages/ui-class#create-an-rcl-with-static-assets>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="79bd5-164">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="79bd5-164">Additional resources</span></span>

* <xref:razor-pages/ui-class>
