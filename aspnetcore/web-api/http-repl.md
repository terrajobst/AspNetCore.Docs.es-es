---
title: Prueba de las API web HTTP REPL
author: scottaddie
description: Obtenga información sobre cómo usar la herramienta global HTTP REPL de .NET Core para examinar y probar una API web de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 07/25/2019
uid: web-api/http-repl
ms.openlocfilehash: d2c5f774595e7a2223e84cc76eecdb9baa04adfe
ms.sourcegitcommit: 776598f71da0d1e4c9e923b3b395d3c3b5825796
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/26/2019
ms.locfileid: "70024803"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="1bee6-103">Prueba de las API web HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="1bee6-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="1bee6-104">Por [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="1bee6-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="1bee6-105">El bucle HTTP read-eval-print (REPL):</span><span class="sxs-lookup"><span data-stu-id="1bee6-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="1bee6-106">Es una herramienta de línea de comandos ligera y multiplataforma con la misma compatibilidad que .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1bee6-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="1bee6-107">Se usa para realizar solicitudes HTTP con el fin de probar las API web de ASP.NET Core (así como otras API) y ver los resultados.</span><span class="sxs-lookup"><span data-stu-id="1bee6-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="1bee6-108">Capaz de probar las API web hospedadas en cualquier entorno, incluidos localhost y Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="1bee6-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="1bee6-109">Se admiten los siguientes [verbos HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods):</span><span class="sxs-lookup"><span data-stu-id="1bee6-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="1bee6-110">DELETE</span><span class="sxs-lookup"><span data-stu-id="1bee6-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="1bee6-111">GET</span><span class="sxs-lookup"><span data-stu-id="1bee6-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="1bee6-112">HEAD</span><span class="sxs-lookup"><span data-stu-id="1bee6-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="1bee6-113">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="1bee6-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="1bee6-114">PATCH</span><span class="sxs-lookup"><span data-stu-id="1bee6-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="1bee6-115">POST</span><span class="sxs-lookup"><span data-stu-id="1bee6-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="1bee6-116">PUT</span><span class="sxs-lookup"><span data-stu-id="1bee6-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="1bee6-117">Para continuar, [vea o descargue la API web de muestra de ASP.NET Core ](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([cómo descargar](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="1bee6-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1bee6-118">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="1bee6-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="1bee6-119">Instalación</span><span class="sxs-lookup"><span data-stu-id="1bee6-119">Installation</span></span>

<span data-ttu-id="1bee6-120">Ejecute el siguiente comando para instalar HTTP REPL:</span><span class="sxs-lookup"><span data-stu-id="1bee6-120">To install the HTTP REPL, run the following command:</span></span>

```console
dotnet tool install -g Microsoft.dotnet-httprepl --version "3.0.0-*"
```

<span data-ttu-id="1bee6-121">Se instala una [herramienta global de .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) desde el paquete NuGet [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl).</span><span class="sxs-lookup"><span data-stu-id="1bee6-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="1bee6-122">Uso</span><span class="sxs-lookup"><span data-stu-id="1bee6-122">Usage</span></span>

<span data-ttu-id="1bee6-123">Tras la correcta instalación de la herramienta, ejecute el siguiente comando para iniciar HTTP REPL:</span><span class="sxs-lookup"><span data-stu-id="1bee6-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
dotnet httprepl
```

<span data-ttu-id="1bee6-124">Para ver los comandos de HTTP REPL disponibles, ejecute uno de los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="1bee6-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
dotnet httprepl -h
```

```console
dotnet httprepl --help
```

<span data-ttu-id="1bee6-125">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="1bee6-125">The following output is displayed:</span></span>

```console
Usage:
  dotnet httprepl [<BASE_ADDRESS>] [options]

Arguments:
  <BASE_ADDRESS> - The initial base address for the REPL.

Options:
  -h|--help - Show help information.

Once the REPL starts, these commands are valid:

Setup Commands:
Use these commands to configure the tool for your API server

connect        Configures the directory structure and base address of the api server
set header     Sets or clears a header for all requests. e.g. `set header content-type application/json`

HTTP Commands:
Use these commands to execute requests against your application.

GET            get - Issues a GET request
POST           post - Issues a POST request
PUT            put - Issues a PUT request
DELETE         delete - Issues a DELETE request
PATCH          patch - Issues a PATCH request
HEAD           head - Issues a HEAD request
OPTIONS        options - Issues a OPTIONS request

Navigation Commands:
The REPL allows you to navigate your URL space and focus on specific APIs that you are working on.

set base       Set the base URI. e.g. `set base http://locahost:5000`
ls             Show all endpoints for the current path
cd             Append the given directory to the currently selected path, or move up a path when using `cd ..`

Shell Commands:
Use these commands to interact with the REPL shell.

clear          Removes all text from the shell
echo [on/off]  Turns request echoing on or off, show the request that was made when using request commands
exit           Exit the shell

REPL Customization Commands:
Use these commands to customize the REPL behavior.

pref [get/set] Allows viewing or changing preferences, e.g. 'pref set editor.command.default 'C:\\Program Files\\Microsoft VS Code\\Code.exe'`
run            Runs the script at the given path. A script is a set of commands that can be typed with one command per line
ui             Displays the Swagger UI page, if available, in the default browser

Use `help <COMMAND>` for more detail on an individual command. e.g. `help get`.
For detailed tool info, see https://aka.ms/http-repl-doc.
```

<span data-ttu-id="1bee6-126">HTTP REPL ofrece la finalización del comando.</span><span class="sxs-lookup"><span data-stu-id="1bee6-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="1bee6-127">Al presionar la tecla <kbd>Tabulador</kbd>, se procesa una iteración en la lista de comandos que completa los caracteres o el punto de conexión de la API que ha escrito.</span><span class="sxs-lookup"><span data-stu-id="1bee6-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="1bee6-128">En las secciones siguientes se describen los comandos disponibles de la CLI.</span><span class="sxs-lookup"><span data-stu-id="1bee6-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="1bee6-129">Conexión a la API web</span><span class="sxs-lookup"><span data-stu-id="1bee6-129">Connect to the web API</span></span>

<span data-ttu-id="1bee6-130">Conéctese a la API web ejecutando el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-130">Connect to a web API by running the following command:</span></span>

```console
dotnet httprepl <ROOT URI>
```

<span data-ttu-id="1bee6-131">`<ROOT URI>` es el URI base de la API web.</span><span class="sxs-lookup"><span data-stu-id="1bee6-131">`<ROOT URI>` is the base URI for the web API.</span></span> <span data-ttu-id="1bee6-132">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-132">For example:</span></span>

```console
dotnet httprepl https://localhost:5001
```

<span data-ttu-id="1bee6-133">También puede ejecutar el comando siguiente en cualquier momento mientras se ejecuta HTTP REPL:</span><span class="sxs-lookup"><span data-stu-id="1bee6-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
connect <ROOT URI>
```

<span data-ttu-id="1bee6-134">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-134">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001
```

## <a name="manually-point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="1bee6-135">Establecimiento manualmente del documento de Swagger de la API web como destino</span><span class="sxs-lookup"><span data-stu-id="1bee6-135">Manually point to the Swagger document for the web API</span></span>

<span data-ttu-id="1bee6-136">El comando de conexión anterior intentará buscar el documento de Swagger automáticamente.</span><span class="sxs-lookup"><span data-stu-id="1bee6-136">The connect command above will attempt to find the Swagger document automatically.</span></span> <span data-ttu-id="1bee6-137">Si por alguna razón no puede hacerlo, puede especificar el URI del documento de Swagger para la API web mediante la opción `--swagger`:</span><span class="sxs-lookup"><span data-stu-id="1bee6-137">If for some reason it is unable to do so, you can specify the URI of the Swagger document for the web API by using the `--swagger` option:</span></span>

```console
connect <ROOT URI> --swagger <SWAGGER URI>
```

<span data-ttu-id="1bee6-138">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-138">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001 --swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="1bee6-139">Navegación de la API web</span><span class="sxs-lookup"><span data-stu-id="1bee6-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="1bee6-140">Visualización de los puntos de conexión disponibles</span><span class="sxs-lookup"><span data-stu-id="1bee6-140">View available endpoints</span></span>

<span data-ttu-id="1bee6-141">Para enumerar los diferentes puntos de conexión (controladores) en la ruta de acceso actual de la dirección de la API web, ejecute el comando `ls` o `dir`:</span><span class="sxs-lookup"><span data-stu-id="1bee6-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="1bee6-142">Se muestra el siguiente formato de salida:</span><span class="sxs-lookup"><span data-stu-id="1bee6-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="1bee6-143">La salida anterior indica que hay dos controladores disponibles: `Fruits` y `People`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="1bee6-144">Ambos controladores admiten operaciones HTTP GET y POST sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="1bee6-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="1bee6-145">Al navegar a un controlador específico, se revela más información.</span><span class="sxs-lookup"><span data-stu-id="1bee6-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="1bee6-146">Por ejemplo, la salida del comando siguiente muestra que el controlador `Fruits` también admite las operaciones HTTP GET, PUT y DELETE.</span><span class="sxs-lookup"><span data-stu-id="1bee6-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="1bee6-147">Cada una de estas operaciones esperan un parámetro `id` en la ruta:</span><span class="sxs-lookup"><span data-stu-id="1bee6-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="1bee6-148">También puede ejecutar el comando `ui` para abrir la página de la interfaz de usuario de Swagger de la API web en un explorador.</span><span class="sxs-lookup"><span data-stu-id="1bee6-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="1bee6-149">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="1bee6-150">Navegación a un punto de conexión</span><span class="sxs-lookup"><span data-stu-id="1bee6-150">Navigate to an endpoint</span></span>

<span data-ttu-id="1bee6-151">Para navegar a un punto de conexión distinto en la API web, ejecute el comando `cd`:</span><span class="sxs-lookup"><span data-stu-id="1bee6-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="1bee6-152">La ruta de acceso que sigue al comando `cd` no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="1bee6-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="1bee6-153">Se muestra el siguiente formato de salida:</span><span class="sxs-lookup"><span data-stu-id="1bee6-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="1bee6-154">Personalización de HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="1bee6-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="1bee6-155">Los [colores](#set-color-preferences) predeterminados de HTTP REPL se pueden personalizar.</span><span class="sxs-lookup"><span data-stu-id="1bee6-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="1bee6-156">Además, se puede definir un [editor de texto predeterminado](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="1bee6-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="1bee6-157">Las preferencias de HTTP REPL se conservan tanto en la sesión actual como en futuras sesiones.</span><span class="sxs-lookup"><span data-stu-id="1bee6-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="1bee6-158">Una vez modificadas, se almacenan en el archivo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="1bee6-159">Linux</span><span class="sxs-lookup"><span data-stu-id="1bee6-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="1bee6-160">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="1bee6-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="1bee6-161">macOS</span><span class="sxs-lookup"><span data-stu-id="1bee6-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="1bee6-162">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="1bee6-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="1bee6-163">Windows</span><span class="sxs-lookup"><span data-stu-id="1bee6-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="1bee6-164">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="1bee6-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="1bee6-165">El archivo *.httpreplprefs* se carga al inicio; los cambios no se supervisan durante el tiempo en ejecución.</span><span class="sxs-lookup"><span data-stu-id="1bee6-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="1bee6-166">Las modificaciones manuales que se hagan en el archivo solo se aplicarán después de reiniciar la herramienta.</span><span class="sxs-lookup"><span data-stu-id="1bee6-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="1bee6-167">Visualización de la configuración</span><span class="sxs-lookup"><span data-stu-id="1bee6-167">View the settings</span></span>

<span data-ttu-id="1bee6-168">Para ver la configuración disponible, ejecute el comando `pref get`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="1bee6-169">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="1bee6-170">El comando anterior muestra los pares clave-valor disponibles:</span><span class="sxs-lookup"><span data-stu-id="1bee6-170">The preceding command displays the available key-value pairs:</span></span>

```console
colors.json=Green
colors.json.arrayBrace=BoldCyan
colors.json.comma=BoldYellow
colors.json.name=BoldMagenta
colors.json.nameSeparator=BoldWhite
colors.json.objectBrace=Cyan
colors.protocol=BoldGreen
colors.status=BoldYellow
```

### <a name="set-color-preferences"></a><span data-ttu-id="1bee6-171">Establecimiento de las preferencias de color</span><span class="sxs-lookup"><span data-stu-id="1bee6-171">Set color preferences</span></span>

<span data-ttu-id="1bee6-172">Actualmente solo se permite colorear la respuesta para JSON.</span><span class="sxs-lookup"><span data-stu-id="1bee6-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="1bee6-173">Para personalizar el color predeterminado de la herramienta HTTP REPL, busque la clave correspondiente al color que se va a cambiar.</span><span class="sxs-lookup"><span data-stu-id="1bee6-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="1bee6-174">Para obtener instrucciones sobre cómo buscar las claves, consulte la sección [Visualización de la configuración](#view-the-settings).</span><span class="sxs-lookup"><span data-stu-id="1bee6-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="1bee6-175">Por ejemplo, cambie el valor de clave `colors.json` de `Green` a `White`, tal como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="1bee6-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="1bee6-176">Solo se pueden usar los [colores permitidos](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs).</span><span class="sxs-lookup"><span data-stu-id="1bee6-176">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="1bee6-177">Las solicitudes HTTP posteriores muestran la salida con el nuevo color.</span><span class="sxs-lookup"><span data-stu-id="1bee6-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="1bee6-178">Si no se establecen claves de color específicas, se consideran las más genéricas.</span><span class="sxs-lookup"><span data-stu-id="1bee6-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="1bee6-179">Para mostrar este comportamiento de reserva, veamos el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="1bee6-180">Si `colors.json.name` no tiene ningún valor, se usa `colors.json.string`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="1bee6-181">Si `colors.json.string` no tiene ningún valor, se usa `colors.json.literal`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="1bee6-182">Si `colors.json.literal` no tiene ningún valor, se usa `colors.json`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="1bee6-183">Si `colors.json` no tiene ningún valor, se usa el color de texto predeterminado del shell del comando (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="1bee6-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="1bee6-184">Establecimiento del tamaño de sangría</span><span class="sxs-lookup"><span data-stu-id="1bee6-184">Set indentation size</span></span>

<span data-ttu-id="1bee6-185">Actualmente, la personalización del tamaño de sangría de respuesta solo se admite para JSON.</span><span class="sxs-lookup"><span data-stu-id="1bee6-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="1bee6-186">El tamaño predeterminado es de dos espacios.</span><span class="sxs-lookup"><span data-stu-id="1bee6-186">The default size is two spaces.</span></span> <span data-ttu-id="1bee6-187">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-187">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="1bee6-188">Para cambiar el tamaño predeterminado, establezca la clave `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="1bee6-189">Por ejemplo, para usar siempre cuatro espacios:</span><span class="sxs-lookup"><span data-stu-id="1bee6-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="1bee6-190">Las respuestas posteriores respetan el valor de cuatro espacios:</span><span class="sxs-lookup"><span data-stu-id="1bee6-190">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-indentation-size"></a><span data-ttu-id="1bee6-191">Establecimiento del tamaño de sangría</span><span class="sxs-lookup"><span data-stu-id="1bee6-191">Set indentation size</span></span>

<span data-ttu-id="1bee6-192">Actualmente, la personalización del tamaño de sangría de respuesta solo se admite para JSON.</span><span class="sxs-lookup"><span data-stu-id="1bee6-192">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="1bee6-193">El tamaño predeterminado es de dos espacios.</span><span class="sxs-lookup"><span data-stu-id="1bee6-193">The default size is two spaces.</span></span> <span data-ttu-id="1bee6-194">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-194">For example:</span></span>

```json
[
  {
    "id": 1,
    "name": "Apple"
  },
  {
    "id": 2,
    "name": "Orange"
  },
  {
    "id": 3,
    "name": "Strawberry"
  }
]
```

<span data-ttu-id="1bee6-195">Para cambiar el tamaño predeterminado, establezca la clave `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-195">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="1bee6-196">Por ejemplo, para usar siempre cuatro espacios:</span><span class="sxs-lookup"><span data-stu-id="1bee6-196">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="1bee6-197">Las respuestas posteriores respetan el valor de cuatro espacios:</span><span class="sxs-lookup"><span data-stu-id="1bee6-197">Subsequent responses honor the setting of four spaces:</span></span>

```json
[
    {
        "id": 1,
        "name": "Apple"
    },
    {
        "id": 2,
        "name": "Orange"
    },
    {
        "id": 3,
        "name": "Strawberry"
    }
]
```

### <a name="set-the-default-text-editor"></a><span data-ttu-id="1bee6-198">Establecimiento del editor de texto predeterminado</span><span class="sxs-lookup"><span data-stu-id="1bee6-198">Set the default text editor</span></span>

<span data-ttu-id="1bee6-199">De manera predeterminada, HTTP REPL no tiene ningún editor de texto configurado para su uso.</span><span class="sxs-lookup"><span data-stu-id="1bee6-199">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="1bee6-200">Para probar los métodos de la API web que requieren un cuerpo de la solicitud HTTP, se debe establecer un editor de texto predeterminado.</span><span class="sxs-lookup"><span data-stu-id="1bee6-200">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="1bee6-201">La herramienta HTTP REPL inicia el editor de texto configurado con el único fin de redactar el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1bee6-201">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="1bee6-202">Ejecute el comando siguiente para establecer el editor de texto preferido como predeterminado:</span><span class="sxs-lookup"><span data-stu-id="1bee6-202">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="1bee6-203">En el comando anterior, `<EXECUTABLE>` es la ruta de acceso completa al archivo ejecutable del editor de texto.</span><span class="sxs-lookup"><span data-stu-id="1bee6-203">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="1bee6-204">Por ejemplo, ejecute el comando siguiente para establecer Visual Studio Code como editor de texto predeterminado:</span><span class="sxs-lookup"><span data-stu-id="1bee6-204">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="1bee6-205">Linux</span><span class="sxs-lookup"><span data-stu-id="1bee6-205">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="1bee6-206">macOS</span><span class="sxs-lookup"><span data-stu-id="1bee6-206">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="1bee6-207">Windows</span><span class="sxs-lookup"><span data-stu-id="1bee6-207">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="1bee6-208">Para iniciar el editor de texto predeterminado con argumentos específicos de la CLI, establezca la clave `editor.command.default.arguments`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-208">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="1bee6-209">Por ejemplo, supongamos que Visual Studio Code es el editor de texto predeterminado y que siempre quiere que HTTP REPL abra Visual Studio Code en una nueva sesión con las extensiones deshabilitadas.</span><span class="sxs-lookup"><span data-stu-id="1bee6-209">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="1bee6-210">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="1bee6-210">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

### <a name="set-the-swagger-search-paths"></a><span data-ttu-id="1bee6-211">Establecer las rutas de acceso de búsqueda de Swagger</span><span class="sxs-lookup"><span data-stu-id="1bee6-211">Set the Swagger search paths</span></span>

<span data-ttu-id="1bee6-212">De forma predeterminada, HTTP REPL tiene un conjunto de rutas de acceso relativas que usa para buscar el documento de Swagger al ejecutar el comando `connect` sin la opción `--swagger`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-212">By default, the HTTP REPL has a set of relative paths that it uses to find the Swagger document when executing the `connect` command without the `--swagger` option.</span></span> <span data-ttu-id="1bee6-213">Estas rutas de acceso relativas se combinan con las rutas de acceso raíz y base especificadas en el comando `connect`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-213">These relative paths are combined with the root and base paths specified in the `connect` command.</span></span> <span data-ttu-id="1bee6-214">Las rutas de acceso relativas predeterminadas son:</span><span class="sxs-lookup"><span data-stu-id="1bee6-214">The default relative paths are:</span></span>

- <span data-ttu-id="1bee6-215">*swagger.json*</span><span class="sxs-lookup"><span data-stu-id="1bee6-215">*swagger.json*</span></span>
- <span data-ttu-id="1bee6-216">*swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="1bee6-216">*swagger/v1/swagger.json*</span></span>
- <span data-ttu-id="1bee6-217">*/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="1bee6-217">*/swagger.json*</span></span>
- <span data-ttu-id="1bee6-218">*/swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="1bee6-218">*/swagger/v1/swagger.json*</span></span>

<span data-ttu-id="1bee6-219">Para usar un conjunto diferente de rutas de acceso de búsqueda en el entorno, establezca la preferencia `swagger.searchPaths`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-219">To use a different set of search paths in your environment, set the `swagger.searchPaths` preference.</span></span> <span data-ttu-id="1bee6-220">El valor debe ser una lista delimitada por canalizaciones de rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="1bee6-220">The value must be a pipe-delimited list of relative paths.</span></span> <span data-ttu-id="1bee6-221">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-221">For example:</span></span>

```console
pref set swagger.searchPaths "swagger/v2/swagger.json|swagger/v3/swagger.json
```

## <a name="test-http-get-requests"></a><span data-ttu-id="1bee6-222">Prueba de solicitudes HTTP GET</span><span class="sxs-lookup"><span data-stu-id="1bee6-222">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="1bee6-223">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="1bee6-223">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="1bee6-224">Argumentos</span><span class="sxs-lookup"><span data-stu-id="1bee6-224">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="1bee6-225">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="1bee6-225">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="1bee6-226">Opciones</span><span class="sxs-lookup"><span data-stu-id="1bee6-226">Options</span></span>

<span data-ttu-id="1bee6-227">Las siguientes opciones están disponibles para el comando `get`:</span><span class="sxs-lookup"><span data-stu-id="1bee6-227">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="1bee6-228">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="1bee6-228">Example</span></span>

<span data-ttu-id="1bee6-229">Para emitir una solicitud HTTP GET realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-229">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="1bee6-230">Ejecute el comando `get` en un punto de conexión que lo admita:</span><span class="sxs-lookup"><span data-stu-id="1bee6-230">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="1bee6-231">El comando anterior muestra el siguiente formato de salida:</span><span class="sxs-lookup"><span data-stu-id="1bee6-231">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 03:38:45 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "name": "Scott Hunter"
      },
      {
        "id": 2,
        "name": "Scott Hanselman"
      },
      {
        "id": 3,
        "name": "Scott Guthrie"
      }
    ]


    https://localhost:5001/people~
    ```

1. <span data-ttu-id="1bee6-232">Recupere un registro específico pasando un parámetro al comando `get`:</span><span class="sxs-lookup"><span data-stu-id="1bee6-232">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="1bee6-233">El comando anterior muestra el siguiente formato de salida:</span><span class="sxs-lookup"><span data-stu-id="1bee6-233">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 21 Jun 2019 06:17:57 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 2,
        "name": "Scott Hanselman"
      }
    ]


    https://localhost:5001/people~
    ```

## <a name="test-http-post-requests"></a><span data-ttu-id="1bee6-234">Prueba de solicitudes HTTP POST</span><span class="sxs-lookup"><span data-stu-id="1bee6-234">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="1bee6-235">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="1bee6-235">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="1bee6-236">Argumentos</span><span class="sxs-lookup"><span data-stu-id="1bee6-236">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="1bee6-237">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="1bee6-237">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="1bee6-238">Opciones</span><span class="sxs-lookup"><span data-stu-id="1bee6-238">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="1bee6-239">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="1bee6-239">Example</span></span>

<span data-ttu-id="1bee6-240">Para emitir una solicitud HTTP POST, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-240">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="1bee6-241">Ejecute el comando `post` en un punto de conexión que lo admita:</span><span class="sxs-lookup"><span data-stu-id="1bee6-241">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="1bee6-242">En el comando anterior, el encabezado de solicitud HTTP `Content-Type` se establece para indicar un tipo de medios de cuerpo de la solicitud de JSON.</span><span class="sxs-lookup"><span data-stu-id="1bee6-242">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="1bee6-243">El editor de texto predeterminado abre un archivo *.tmp* con una plantilla JSON que representa el cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="1bee6-243">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="1bee6-244">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-244">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="1bee6-245">Para establecer el editor de texto predeterminado, consulte la sección [Establecimiento del editor de texto predeterminado](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="1bee6-245">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="1bee6-246">Modifique la plantilla JSON para cumplir los requisitos de validación de modelos:</span><span class="sxs-lookup"><span data-stu-id="1bee6-246">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="1bee6-247">Guarde el archivo *.tmp* y cierre el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="1bee6-247">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="1bee6-248">En el shell del comando aparecerá la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-248">The following output appears in the command shell:</span></span>

    ```console
    HTTP/1.1 201 Created
    Content-Type: application/json; charset=utf-8
    Date: Thu, 27 Jun 2019 21:24:18 GMT
    Location: https://localhost:5001/people/4
    Server: Kestrel
    Transfer-Encoding: chunked

    {
      "id": 4,
      "name": "Scott Addie"
    }


    https://localhost:5001/people~
    ```

## <a name="test-http-put-requests"></a><span data-ttu-id="1bee6-249">Prueba de solicitudes HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="1bee6-249">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="1bee6-250">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="1bee6-250">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="1bee6-251">Argumentos</span><span class="sxs-lookup"><span data-stu-id="1bee6-251">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="1bee6-252">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="1bee6-252">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="1bee6-253">Opciones</span><span class="sxs-lookup"><span data-stu-id="1bee6-253">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="1bee6-254">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="1bee6-254">Example</span></span>

<span data-ttu-id="1bee6-255">Para emitir una solicitud HTTP PUT, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-255">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="1bee6-256">*Opcional*: Ejecute el comando `get` para ver los datos antes de modificarlos:</span><span class="sxs-lookup"><span data-stu-id="1bee6-256">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `put` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ put 2 -h Content-Type=application/json
    ```

    <span data-ttu-id="1bee6-257">En el comando anterior, el encabezado de solicitud HTTP `Content-Type` se establece para indicar un tipo de medios de cuerpo de la solicitud de JSON.</span><span class="sxs-lookup"><span data-stu-id="1bee6-257">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="1bee6-258">El editor de texto predeterminado abre un archivo *.tmp* con una plantilla JSON que representa el cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="1bee6-258">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="1bee6-259">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-259">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="1bee6-260">Para establecer el editor de texto predeterminado, consulte la sección [Establecimiento del editor de texto predeterminado](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="1bee6-260">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="1bee6-261">Modifique la plantilla JSON para cumplir los requisitos de validación de modelos:</span><span class="sxs-lookup"><span data-stu-id="1bee6-261">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="1bee6-262">Guarde el archivo *.tmp* y cierre el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="1bee6-262">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="1bee6-263">En el shell del comando aparecerá la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-263">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="1bee6-264">*Opcional*: Emita un comando `get` para ver las modificaciones.</span><span class="sxs-lookup"><span data-stu-id="1bee6-264">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="1bee6-265">Por ejemplo, si ha escrito "Cherry" en el editor de texto, un elemento `get` devuelve lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-265">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:08:20 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Cherry"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-delete-requests"></a><span data-ttu-id="1bee6-266">Prueba de solicitudes HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="1bee6-266">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="1bee6-267">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="1bee6-267">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="1bee6-268">Argumentos</span><span class="sxs-lookup"><span data-stu-id="1bee6-268">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="1bee6-269">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="1bee6-269">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="1bee6-270">Opciones</span><span class="sxs-lookup"><span data-stu-id="1bee6-270">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="1bee6-271">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="1bee6-271">Example</span></span>

<span data-ttu-id="1bee6-272">Para emitir una solicitud HTTP DELETE, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-272">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="1bee6-273">*Opcional*: Ejecute el comando `get` para ver los datos antes de modificarlos:</span><span class="sxs-lookup"><span data-stu-id="1bee6-273">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:07:32 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 2,
        "data": "Orange"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]

1. Run the `delete` command on an endpoint that supports it:

    ```console
    https://localhost:5001/fruits~ delete 2
    ```

    <span data-ttu-id="1bee6-274">El comando anterior muestra el siguiente formato de salida:</span><span class="sxs-lookup"><span data-stu-id="1bee6-274">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="1bee6-275">*Opcional*: Emita un comando `get` para ver las modificaciones.</span><span class="sxs-lookup"><span data-stu-id="1bee6-275">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="1bee6-276">En este ejemplo, un elemento `get` devuelve lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-276">In this example, a `get` returns the following:</span></span>

    ```console
    https://localhost:5001/fruits~ get
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Sat, 22 Jun 2019 00:16:30 GMT
    Server: Kestrel
    Transfer-Encoding: chunked

    [
      {
        "id": 1,
        "data": "Apple"
      },
      {
        "id": 3,
        "data": "Strawberry"
      }
    ]


    https://localhost:5001/fruits~
    ```

## <a name="test-http-patch-requests"></a><span data-ttu-id="1bee6-277">Prueba de solicitudes HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="1bee6-277">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="1bee6-278">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="1bee6-278">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="1bee6-279">Argumentos</span><span class="sxs-lookup"><span data-stu-id="1bee6-279">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="1bee6-280">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="1bee6-280">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="1bee6-281">Opciones</span><span class="sxs-lookup"><span data-stu-id="1bee6-281">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="1bee6-282">Prueba de solicitudes HTTP HEAD</span><span class="sxs-lookup"><span data-stu-id="1bee6-282">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="1bee6-283">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="1bee6-283">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="1bee6-284">Argumentos</span><span class="sxs-lookup"><span data-stu-id="1bee6-284">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="1bee6-285">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="1bee6-285">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="1bee6-286">Opciones</span><span class="sxs-lookup"><span data-stu-id="1bee6-286">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="1bee6-287">Prueba de solicitudes HTTP OPTIONS</span><span class="sxs-lookup"><span data-stu-id="1bee6-287">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="1bee6-288">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="1bee6-288">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="1bee6-289">Argumentos</span><span class="sxs-lookup"><span data-stu-id="1bee6-289">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="1bee6-290">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="1bee6-290">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="1bee6-291">Opciones</span><span class="sxs-lookup"><span data-stu-id="1bee6-291">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="1bee6-292">Establecimiento de encabezados de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="1bee6-292">Set HTTP request headers</span></span>

<span data-ttu-id="1bee6-293">Para establecer un encabezado de solicitud HTTP, use uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="1bee6-293">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="1bee6-294">Establézcalo insertado con la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="1bee6-294">Set inline with the HTTP request.</span></span> <span data-ttu-id="1bee6-295">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-295">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="1bee6-296">Con el método anterior, cada encabezado de solicitud HTTP distinto requiere su propia opción `-h`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-296">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="1bee6-297">Establézcalo antes de enviar la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="1bee6-297">Set before sending the HTTP request.</span></span> <span data-ttu-id="1bee6-298">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-298">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="1bee6-299">Al establecer el encabezado antes de enviar una solicitud, este permanece establecido mientras dure la sesión de shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="1bee6-299">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="1bee6-300">Para borrar el encabezado, proporcione un valor vacío.</span><span class="sxs-lookup"><span data-stu-id="1bee6-300">To clear the header, provide an empty value.</span></span> <span data-ttu-id="1bee6-301">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-301">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="1bee6-302">Alternancia de la pantalla de solicitud HTTP</span><span class="sxs-lookup"><span data-stu-id="1bee6-302">Toggle HTTP request display</span></span>

<span data-ttu-id="1bee6-303">De manera predeterminada, se suprime la pantalla de solicitud HTTP que se está enviando.</span><span class="sxs-lookup"><span data-stu-id="1bee6-303">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="1bee6-304">Es posible cambiar la configuración correspondiente mientras dure la sesión de shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="1bee6-304">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="1bee6-305">Habilitación de la pantalla de solicitudes</span><span class="sxs-lookup"><span data-stu-id="1bee6-305">Enable request display</span></span>

<span data-ttu-id="1bee6-306">Vea la solicitud HTTP que se envía mediante la ejecución del comando `echo on`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-306">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="1bee6-307">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-307">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="1bee6-308">Las solicitudes HTTP posteriores de la sesión actual muestran los encabezados de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="1bee6-308">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="1bee6-309">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-309">For example:</span></span>

```console
https://localhost:5001/people~ post

[main 2019-06-28T18:50:11.930Z] update#setState idle
Request to https://localhost:5001...

POST /people HTTP/1.1
Content-Length: 41
Content-Type: application/json
User-Agent: HTTP-REPL

{
  "id": 0,
  "name": "Scott Addie"
}

Response from https://localhost:5001...

HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Date: Fri, 28 Jun 2019 18:50:21 GMT
Location: https://localhost:5001/people/4
Server: Kestrel
Transfer-Encoding: chunked

{
  "id": 4,
  "name": "Scott Addie"
}


https://localhost:5001/people~
```

### <a name="disable-request-display"></a><span data-ttu-id="1bee6-310">Deshabilitación de la pantalla de solicitudes</span><span class="sxs-lookup"><span data-stu-id="1bee6-310">Disable request display</span></span>

<span data-ttu-id="1bee6-311">Suprima la pantalla de solicitud HTTP que se envía mediante la ejecución del comando `echo off`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-311">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="1bee6-312">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-312">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="1bee6-313">Ejecución de un script</span><span class="sxs-lookup"><span data-stu-id="1bee6-313">Run a script</span></span>

<span data-ttu-id="1bee6-314">Si ejecuta con frecuencia el mismo conjunto de comandos de HTTP REPL, considere la posibilidad de almacenarlos en un archivo de texto.</span><span class="sxs-lookup"><span data-stu-id="1bee6-314">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="1bee6-315">Los comandos del archivo toman el mismo formulario que los ejecutados manualmente en la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="1bee6-315">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="1bee6-316">Los comandos se pueden ejecutar en un modo por lotes mediante el comando `run`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-316">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="1bee6-317">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-317">For example:</span></span>

1. <span data-ttu-id="1bee6-318">Cree un archivo de texto que contenga un conjunto de comandos delimitados por línea nueva.</span><span class="sxs-lookup"><span data-stu-id="1bee6-318">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="1bee6-319">Para ilustrarlo, considere la posibilidad de usar un archivo *people-script.txt* que contenga los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="1bee6-319">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="1bee6-320">Ejecute el comando `run`, pasando la ruta de acceso del archivo de texto.</span><span class="sxs-lookup"><span data-stu-id="1bee6-320">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="1bee6-321">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="1bee6-321">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="1bee6-322">Aparece el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="1bee6-322">The following output appears:</span></span>

    ```console
    https://localhost:5001/~ set base https://localhost:5001
    Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json
    
    https://localhost:5001/~ ls
    .        []
    Fruits   [get|post]
    People   [get|post]
    
    https://localhost:5001/~ cd People
    /People    [get|post]
    
    https://localhost:5001/People~ ls
    .      [get|post]
    ..     []
    {id}   [get|put|delete]
    
    https://localhost:5001/People~ get 1
    HTTP/1.1 200 OK
    Content-Type: application/json; charset=utf-8
    Date: Fri, 12 Jul 2019 19:20:10 GMT
    Server: Kestrel
    Transfer-Encoding: chunked
    
    {
      "id": 1,
      "name": "Scott Hunter"
    }
    
    
    https://localhost:5001/People~
    ```

## <a name="clear-the-output"></a><span data-ttu-id="1bee6-323">Borrado de la salida</span><span class="sxs-lookup"><span data-stu-id="1bee6-323">Clear the output</span></span>

<span data-ttu-id="1bee6-324">Para quitar todas las salidas escritas en el shell de comandos mediante la herramienta REPL HTTP, ejecute el comando `clear` o `cls`.</span><span class="sxs-lookup"><span data-stu-id="1bee6-324">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="1bee6-325">Para ilustrarlo, imagine que el shell de comandos contiene la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-325">To illustrate, imagine the command shell contains the following output:</span></span>

```console
dotnet httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="1bee6-326">Ejecute el comando siguiente para borrar la salida:</span><span class="sxs-lookup"><span data-stu-id="1bee6-326">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="1bee6-327">Después de ejecutar el comando anterior, el shell de comandos solo contiene la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="1bee6-327">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="1bee6-328">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="1bee6-328">Additional resources</span></span>

* [<span data-ttu-id="1bee6-329">Solicitudes de API REST</span><span class="sxs-lookup"><span data-stu-id="1bee6-329">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="1bee6-330">Repositorio GitHub de HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="1bee6-330">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
