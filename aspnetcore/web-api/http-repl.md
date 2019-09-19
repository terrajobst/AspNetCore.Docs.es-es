---
title: Prueba de las API web HTTP REPL
author: scottaddie
description: Obtenga información sobre cómo usar la herramienta global HTTP REPL de .NET Core para examinar y probar una API web de ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 08/29/2019
uid: web-api/http-repl
ms.openlocfilehash: 8ef49797fed3379e33810f311bfc474e524122e0
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082583"
---
# <a name="test-web-apis-with-the-http-repl"></a><span data-ttu-id="10bc5-103">Prueba de las API web HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="10bc5-103">Test web APIs with the HTTP REPL</span></span>

<span data-ttu-id="10bc5-104">Por [Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="10bc5-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="10bc5-105">El bucle HTTP read-eval-print (REPL):</span><span class="sxs-lookup"><span data-stu-id="10bc5-105">The HTTP Read-Eval-Print Loop (REPL) is:</span></span>

* <span data-ttu-id="10bc5-106">Es una herramienta de línea de comandos ligera y multiplataforma con la misma compatibilidad que .NET Core.</span><span class="sxs-lookup"><span data-stu-id="10bc5-106">A lightweight, cross-platform command-line tool that's supported everywhere .NET Core is supported.</span></span>
* <span data-ttu-id="10bc5-107">Se usa para realizar solicitudes HTTP con el fin de probar las API web de ASP.NET Core (así como otras API) y ver los resultados.</span><span class="sxs-lookup"><span data-stu-id="10bc5-107">Used for making HTTP requests to test ASP.NET Core web APIs (and non-ASP.NET Core web APIs) and view their results.</span></span>
* <span data-ttu-id="10bc5-108">Capaz de probar las API web hospedadas en cualquier entorno, incluidos localhost y Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="10bc5-108">Capable of testing web APIs hosted in any environment, including localhost and Azure App Service.</span></span>

<span data-ttu-id="10bc5-109">Se admiten los siguientes [verbos HTTP](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods):</span><span class="sxs-lookup"><span data-stu-id="10bc5-109">The following [HTTP verbs](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods) are supported:</span></span>

* [<span data-ttu-id="10bc5-110">DELETE</span><span class="sxs-lookup"><span data-stu-id="10bc5-110">DELETE</span></span>](#test-http-delete-requests)
* [<span data-ttu-id="10bc5-111">GET</span><span class="sxs-lookup"><span data-stu-id="10bc5-111">GET</span></span>](#test-http-get-requests)
* [<span data-ttu-id="10bc5-112">HEAD</span><span class="sxs-lookup"><span data-stu-id="10bc5-112">HEAD</span></span>](#test-http-head-requests)
* [<span data-ttu-id="10bc5-113">OPTIONS</span><span class="sxs-lookup"><span data-stu-id="10bc5-113">OPTIONS</span></span>](#test-http-options-requests)
* [<span data-ttu-id="10bc5-114">PATCH</span><span class="sxs-lookup"><span data-stu-id="10bc5-114">PATCH</span></span>](#test-http-patch-requests)
* [<span data-ttu-id="10bc5-115">POST</span><span class="sxs-lookup"><span data-stu-id="10bc5-115">POST</span></span>](#test-http-post-requests)
* [<span data-ttu-id="10bc5-116">PUT</span><span class="sxs-lookup"><span data-stu-id="10bc5-116">PUT</span></span>](#test-http-put-requests)

<span data-ttu-id="10bc5-117">Para continuar, [vea o descargue la API web de muestra de ASP.NET Core ](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([cómo descargar](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="10bc5-117">To follow along, [view or download the sample ASP.NET Core web API](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/http-repl/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="10bc5-118">Requisitos previos</span><span class="sxs-lookup"><span data-stu-id="10bc5-118">Prerequisites</span></span>

* [!INCLUDE [2.1-SDK](~/includes/2.1-SDK.md)]

## <a name="installation"></a><span data-ttu-id="10bc5-119">Instalación</span><span class="sxs-lookup"><span data-stu-id="10bc5-119">Installation</span></span>

<span data-ttu-id="10bc5-120">Ejecute el siguiente comando para instalar HTTP REPL:</span><span class="sxs-lookup"><span data-stu-id="10bc5-120">To install the HTTP REPL, run the following command:</span></span>

```dotnetcli
dotnet tool install -g Microsoft.dotnet-httprepl --version "3.0.0-*"
```

<span data-ttu-id="10bc5-121">Se instala una [herramienta global de .NET Core](/dotnet/core/tools/global-tools#install-a-global-tool) desde el paquete NuGet [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl).</span><span class="sxs-lookup"><span data-stu-id="10bc5-121">A [.NET Core Global Tool](/dotnet/core/tools/global-tools#install-a-global-tool) is installed from the [Microsoft.dotnet-httprepl](https://www.nuget.org/packages/Microsoft.dotnet-httprepl) NuGet package.</span></span>

## <a name="usage"></a><span data-ttu-id="10bc5-122">Uso</span><span class="sxs-lookup"><span data-stu-id="10bc5-122">Usage</span></span>

<span data-ttu-id="10bc5-123">Tras la correcta instalación de la herramienta, ejecute el siguiente comando para iniciar HTTP REPL:</span><span class="sxs-lookup"><span data-stu-id="10bc5-123">After successful installation of the tool, run the following command to start the HTTP REPL:</span></span>

```console
httprepl
```

<span data-ttu-id="10bc5-124">Para ver los comandos de HTTP REPL disponibles, ejecute uno de los siguientes comandos:</span><span class="sxs-lookup"><span data-stu-id="10bc5-124">To view the available HTTP REPL commands, run one of the following commands:</span></span>

```console
httprepl -h
```

```console
httprepl --help
```

<span data-ttu-id="10bc5-125">Se muestra el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="10bc5-125">The following output is displayed:</span></span>

```console
Usage:
  httprepl [<BASE_ADDRESS>] [options]

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

<span data-ttu-id="10bc5-126">HTTP REPL ofrece la finalización del comando.</span><span class="sxs-lookup"><span data-stu-id="10bc5-126">The HTTP REPL offers command completion.</span></span> <span data-ttu-id="10bc5-127">Al presionar la tecla <kbd>Tabulador</kbd>, se procesa una iteración en la lista de comandos que completa los caracteres o el punto de conexión de la API que ha escrito.</span><span class="sxs-lookup"><span data-stu-id="10bc5-127">Pressing the <kbd>Tab</kbd> key iterates through the list of commands that complete the characters or API endpoint that you typed.</span></span> <span data-ttu-id="10bc5-128">En las secciones siguientes se describen los comandos disponibles de la CLI.</span><span class="sxs-lookup"><span data-stu-id="10bc5-128">The following sections outline the available CLI commands.</span></span>

## <a name="connect-to-the-web-api"></a><span data-ttu-id="10bc5-129">Conexión a la API web</span><span class="sxs-lookup"><span data-stu-id="10bc5-129">Connect to the web API</span></span>

<span data-ttu-id="10bc5-130">Conéctese a la API web ejecutando el comando siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-130">Connect to a web API by running the following command:</span></span>

```console
httprepl <ROOT URI>
```

<span data-ttu-id="10bc5-131">`<ROOT URI>` es el URI base de la API web.</span><span class="sxs-lookup"><span data-stu-id="10bc5-131">`<ROOT URI>` is the base URI for the web API.</span></span> <span data-ttu-id="10bc5-132">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-132">For example:</span></span>

```console
httprepl https://localhost:5001
```

<span data-ttu-id="10bc5-133">También puede ejecutar el comando siguiente en cualquier momento mientras se ejecuta HTTP REPL:</span><span class="sxs-lookup"><span data-stu-id="10bc5-133">Alternatively, run the following command at any time while the HTTP REPL is running:</span></span>

```console
connect <ROOT URI>
```

<span data-ttu-id="10bc5-134">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-134">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001
```

## <a name="manually-point-to-the-swagger-document-for-the-web-api"></a><span data-ttu-id="10bc5-135">Establecimiento manualmente del documento de Swagger de la API web como destino</span><span class="sxs-lookup"><span data-stu-id="10bc5-135">Manually point to the Swagger document for the web API</span></span>

<span data-ttu-id="10bc5-136">El comando de conexión anterior intentará buscar el documento de Swagger automáticamente.</span><span class="sxs-lookup"><span data-stu-id="10bc5-136">The connect command above will attempt to find the Swagger document automatically.</span></span> <span data-ttu-id="10bc5-137">Si por alguna razón no puede hacerlo, puede especificar el URI del documento de Swagger para la API web mediante la opción `--swagger`:</span><span class="sxs-lookup"><span data-stu-id="10bc5-137">If for some reason it is unable to do so, you can specify the URI of the Swagger document for the web API by using the `--swagger` option:</span></span>

```console
connect <ROOT URI> --swagger <SWAGGER URI>
```

<span data-ttu-id="10bc5-138">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-138">For example:</span></span>

```console
(Disconnected)~ connect https://localhost:5001 --swagger /swagger/v1/swagger.json
```

## <a name="navigate-the-web-api"></a><span data-ttu-id="10bc5-139">Navegación de la API web</span><span class="sxs-lookup"><span data-stu-id="10bc5-139">Navigate the web API</span></span>

### <a name="view-available-endpoints"></a><span data-ttu-id="10bc5-140">Visualización de los puntos de conexión disponibles</span><span class="sxs-lookup"><span data-stu-id="10bc5-140">View available endpoints</span></span>

<span data-ttu-id="10bc5-141">Para enumerar los diferentes puntos de conexión (controladores) en la ruta de acceso actual de la dirección de la API web, ejecute el comando `ls` o `dir`:</span><span class="sxs-lookup"><span data-stu-id="10bc5-141">To list the different endpoints (controllers) at the current path of the web API address, run the `ls` or `dir` command:</span></span>

```console
https://localhot:5001/~ ls
```

<span data-ttu-id="10bc5-142">Se muestra el siguiente formato de salida:</span><span class="sxs-lookup"><span data-stu-id="10bc5-142">The following output format is displayed:</span></span>

```console
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="10bc5-143">La salida anterior indica que hay dos controladores disponibles: `Fruits` y `People`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-143">The preceding output indicates that there are two controllers available: `Fruits` and `People`.</span></span> <span data-ttu-id="10bc5-144">Ambos controladores admiten operaciones HTTP GET y POST sin parámetros.</span><span class="sxs-lookup"><span data-stu-id="10bc5-144">Both controllers support parameterless HTTP GET and POST operations.</span></span>

<span data-ttu-id="10bc5-145">Al navegar a un controlador específico, se revela más información.</span><span class="sxs-lookup"><span data-stu-id="10bc5-145">Navigating into a specific controller reveals more detail.</span></span> <span data-ttu-id="10bc5-146">Por ejemplo, la salida del comando siguiente muestra que el controlador `Fruits` también admite las operaciones HTTP GET, PUT y DELETE.</span><span class="sxs-lookup"><span data-stu-id="10bc5-146">For example, the following command's output shows the `Fruits` controller also supports HTTP GET, PUT, and DELETE operations.</span></span> <span data-ttu-id="10bc5-147">Cada una de estas operaciones esperan un parámetro `id` en la ruta:</span><span class="sxs-lookup"><span data-stu-id="10bc5-147">Each of these operations expects an `id` parameter in the route:</span></span>

```console
https://localhost:5001/fruits~ ls
.      [get|post]
..     []
{id}   [get|put|delete]

https://localhost:5001/fruits~
```

<span data-ttu-id="10bc5-148">También puede ejecutar el comando `ui` para abrir la página de la interfaz de usuario de Swagger de la API web en un explorador.</span><span class="sxs-lookup"><span data-stu-id="10bc5-148">Alternatively, run the `ui` command to open the web API's Swagger UI page in a browser.</span></span> <span data-ttu-id="10bc5-149">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-149">For example:</span></span>

```console
https://localhost:5001/~ ui
```

### <a name="navigate-to-an-endpoint"></a><span data-ttu-id="10bc5-150">Navegación a un punto de conexión</span><span class="sxs-lookup"><span data-stu-id="10bc5-150">Navigate to an endpoint</span></span>

<span data-ttu-id="10bc5-151">Para navegar a un punto de conexión distinto en la API web, ejecute el comando `cd`:</span><span class="sxs-lookup"><span data-stu-id="10bc5-151">To navigate to a different endpoint on the web API, run the `cd` command:</span></span>

```console
https://localhost:5001/~ cd people
```

<span data-ttu-id="10bc5-152">La ruta de acceso que sigue al comando `cd` no distingue mayúsculas de minúsculas.</span><span class="sxs-lookup"><span data-stu-id="10bc5-152">The path following the `cd` command is case insensitive.</span></span> <span data-ttu-id="10bc5-153">Se muestra el siguiente formato de salida:</span><span class="sxs-lookup"><span data-stu-id="10bc5-153">The following output format is displayed:</span></span>

```console
/people    [get|post]

https://localhost:5001/people~
```

## <a name="customize-the-http-repl"></a><span data-ttu-id="10bc5-154">Personalización de HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="10bc5-154">Customize the HTTP REPL</span></span>

<span data-ttu-id="10bc5-155">Los [colores](#set-color-preferences) predeterminados de HTTP REPL se pueden personalizar.</span><span class="sxs-lookup"><span data-stu-id="10bc5-155">The HTTP REPL's default [colors](#set-color-preferences) can be customized.</span></span> <span data-ttu-id="10bc5-156">Además, se puede definir un [editor de texto predeterminado](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="10bc5-156">Additionally, a [default text editor](#set-the-default-text-editor) can be defined.</span></span> <span data-ttu-id="10bc5-157">Las preferencias de HTTP REPL se conservan tanto en la sesión actual como en futuras sesiones.</span><span class="sxs-lookup"><span data-stu-id="10bc5-157">The HTTP REPL preferences are persisted across the current session and are honored in future sessions.</span></span> <span data-ttu-id="10bc5-158">Una vez modificadas, se almacenan en el archivo siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-158">Once modified, the preferences are stored in the following file:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="10bc5-159">Linux</span><span class="sxs-lookup"><span data-stu-id="10bc5-159">Linux</span></span>](#tab/linux)

<span data-ttu-id="10bc5-160">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="10bc5-160">*%HOME%/.httpreplprefs*</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="10bc5-161">macOS</span><span class="sxs-lookup"><span data-stu-id="10bc5-161">macOS</span></span>](#tab/macos)

<span data-ttu-id="10bc5-162">*%HOME%/.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="10bc5-162">*%HOME%/.httpreplprefs*</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="10bc5-163">Windows</span><span class="sxs-lookup"><span data-stu-id="10bc5-163">Windows</span></span>](#tab/windows)

<span data-ttu-id="10bc5-164">*%USERPROFILE%\\.httpreplprefs*</span><span class="sxs-lookup"><span data-stu-id="10bc5-164">*%USERPROFILE%\\.httpreplprefs*</span></span>

---

<span data-ttu-id="10bc5-165">El archivo *.httpreplprefs* se carga al inicio; los cambios no se supervisan durante el tiempo en ejecución.</span><span class="sxs-lookup"><span data-stu-id="10bc5-165">The *.httpreplprefs* file is loaded on startup and not monitored for changes at runtime.</span></span> <span data-ttu-id="10bc5-166">Las modificaciones manuales que se hagan en el archivo solo se aplicarán después de reiniciar la herramienta.</span><span class="sxs-lookup"><span data-stu-id="10bc5-166">Manual modifications to the file take effect only after restarting the tool.</span></span>

### <a name="view-the-settings"></a><span data-ttu-id="10bc5-167">Visualización de la configuración</span><span class="sxs-lookup"><span data-stu-id="10bc5-167">View the settings</span></span>

<span data-ttu-id="10bc5-168">Para ver la configuración disponible, ejecute el comando `pref get`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-168">To view the available settings, run the `pref get` command.</span></span> <span data-ttu-id="10bc5-169">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-169">For example:</span></span>

```console
https://localhost:5001/~ pref get
```

<span data-ttu-id="10bc5-170">El comando anterior muestra los pares clave-valor disponibles:</span><span class="sxs-lookup"><span data-stu-id="10bc5-170">The preceding command displays the available key-value pairs:</span></span>

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

### <a name="set-color-preferences"></a><span data-ttu-id="10bc5-171">Establecimiento de las preferencias de color</span><span class="sxs-lookup"><span data-stu-id="10bc5-171">Set color preferences</span></span>

<span data-ttu-id="10bc5-172">Actualmente solo se permite colorear la respuesta para JSON.</span><span class="sxs-lookup"><span data-stu-id="10bc5-172">Response colorization is currently supported for JSON only.</span></span> <span data-ttu-id="10bc5-173">Para personalizar el color predeterminado de la herramienta HTTP REPL, busque la clave correspondiente al color que se va a cambiar.</span><span class="sxs-lookup"><span data-stu-id="10bc5-173">To customize the default HTTP REPL tool coloring, locate the key corresponding to the color to be changed.</span></span> <span data-ttu-id="10bc5-174">Para obtener instrucciones sobre cómo buscar las claves, consulte la sección [Visualización de la configuración](#view-the-settings).</span><span class="sxs-lookup"><span data-stu-id="10bc5-174">For instructions on how to find the keys, see the [View the settings](#view-the-settings) section.</span></span> <span data-ttu-id="10bc5-175">Por ejemplo, cambie el valor de clave `colors.json` de `Green` a `White`, tal como se indica a continuación:</span><span class="sxs-lookup"><span data-stu-id="10bc5-175">For example, change the `colors.json` key value from `Green` to `White` as follows:</span></span>

```console
https://localhost:5001/people~ pref set colors.json White
```

<span data-ttu-id="10bc5-176">Solo se pueden usar los [colores permitidos](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs).</span><span class="sxs-lookup"><span data-stu-id="10bc5-176">Only the [allowed colors](https://github.com/aspnet/HttpRepl/blob/01d5c3c3373e98fe566ff5ef8a17c571de880293/src/Microsoft.Repl/ConsoleHandling/AllowedColors.cs) may be used.</span></span> <span data-ttu-id="10bc5-177">Las solicitudes HTTP posteriores muestran la salida con el nuevo color.</span><span class="sxs-lookup"><span data-stu-id="10bc5-177">Subsequent HTTP requests display output with the new coloring.</span></span>

<span data-ttu-id="10bc5-178">Si no se establecen claves de color específicas, se consideran las más genéricas.</span><span class="sxs-lookup"><span data-stu-id="10bc5-178">When specific color keys aren't set, more generic keys are considered.</span></span> <span data-ttu-id="10bc5-179">Para mostrar este comportamiento de reserva, veamos el ejemplo siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-179">To demonstrate this fallback behavior, consider the following example:</span></span>

* <span data-ttu-id="10bc5-180">Si `colors.json.name` no tiene ningún valor, se usa `colors.json.string`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-180">If `colors.json.name` doesn't have a value, `colors.json.string` is used.</span></span>
* <span data-ttu-id="10bc5-181">Si `colors.json.string` no tiene ningún valor, se usa `colors.json.literal`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-181">If `colors.json.string` doesn't have a value, `colors.json.literal` is used.</span></span>
* <span data-ttu-id="10bc5-182">Si `colors.json.literal` no tiene ningún valor, se usa `colors.json`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-182">If `colors.json.literal` doesn't have a value, `colors.json` is used.</span></span> 
* <span data-ttu-id="10bc5-183">Si `colors.json` no tiene ningún valor, se usa el color de texto predeterminado del shell del comando (`AllowedColors.None`).</span><span class="sxs-lookup"><span data-stu-id="10bc5-183">If `colors.json` doesn't have a value, the command shell's default text color (`AllowedColors.None`) is used.</span></span>

### <a name="set-indentation-size"></a><span data-ttu-id="10bc5-184">Establecimiento del tamaño de sangría</span><span class="sxs-lookup"><span data-stu-id="10bc5-184">Set indentation size</span></span>

<span data-ttu-id="10bc5-185">Actualmente, la personalización del tamaño de sangría de respuesta solo se admite para JSON.</span><span class="sxs-lookup"><span data-stu-id="10bc5-185">Response indentation size customization is currently supported for JSON only.</span></span> <span data-ttu-id="10bc5-186">El tamaño predeterminado es de dos espacios.</span><span class="sxs-lookup"><span data-stu-id="10bc5-186">The default size is two spaces.</span></span> <span data-ttu-id="10bc5-187">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-187">For example:</span></span>

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

<span data-ttu-id="10bc5-188">Para cambiar el tamaño predeterminado, establezca la clave `formatting.json.indentSize`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-188">To change the default size, set the `formatting.json.indentSize` key.</span></span> <span data-ttu-id="10bc5-189">Por ejemplo, para usar siempre cuatro espacios:</span><span class="sxs-lookup"><span data-stu-id="10bc5-189">For example, to always use four spaces:</span></span>

```console
pref set formatting.json.indentSize 4
```

<span data-ttu-id="10bc5-190">Las respuestas posteriores respetan el valor de cuatro espacios:</span><span class="sxs-lookup"><span data-stu-id="10bc5-190">Subsequent responses honor the setting of four spaces:</span></span>

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

### <a name="set-the-default-text-editor"></a><span data-ttu-id="10bc5-191">Establecimiento del editor de texto predeterminado</span><span class="sxs-lookup"><span data-stu-id="10bc5-191">Set the default text editor</span></span>

<span data-ttu-id="10bc5-192">De manera predeterminada, HTTP REPL no tiene ningún editor de texto configurado para su uso.</span><span class="sxs-lookup"><span data-stu-id="10bc5-192">By default, the HTTP REPL has no text editor configured for use.</span></span> <span data-ttu-id="10bc5-193">Para probar los métodos de la API web que requieren un cuerpo de la solicitud HTTP, se debe establecer un editor de texto predeterminado.</span><span class="sxs-lookup"><span data-stu-id="10bc5-193">To test web API methods requiring an HTTP request body, a default text editor must be set.</span></span> <span data-ttu-id="10bc5-194">La herramienta HTTP REPL inicia el editor de texto configurado con el único fin de redactar el cuerpo de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="10bc5-194">The HTTP REPL tool launches the configured text editor for the sole purpose of composing the request body.</span></span> <span data-ttu-id="10bc5-195">Ejecute el comando siguiente para establecer el editor de texto preferido como predeterminado:</span><span class="sxs-lookup"><span data-stu-id="10bc5-195">Run the following command to set your preferred text editor as the default:</span></span>

```console
pref set editor.command.default "<EXECUTABLE>"
```

<span data-ttu-id="10bc5-196">En el comando anterior, `<EXECUTABLE>` es la ruta de acceso completa al archivo ejecutable del editor de texto.</span><span class="sxs-lookup"><span data-stu-id="10bc5-196">In the preceding command, `<EXECUTABLE>` is the full path to the text editor's executable file.</span></span> <span data-ttu-id="10bc5-197">Por ejemplo, ejecute el comando siguiente para establecer Visual Studio Code como editor de texto predeterminado:</span><span class="sxs-lookup"><span data-stu-id="10bc5-197">For example, run the following command to set Visual Studio Code as the default text editor:</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="10bc5-198">Linux</span><span class="sxs-lookup"><span data-stu-id="10bc5-198">Linux</span></span>](#tab/linux)

```console
pref set editor.command.default "/usr/bin/code"
```

# <a name="macostabmacos"></a>[<span data-ttu-id="10bc5-199">macOS</span><span class="sxs-lookup"><span data-stu-id="10bc5-199">macOS</span></span>](#tab/macos)

```console
pref set editor.command.default "/Applications/Visual Studio Code.app/Contents/Resources/app/bin/code"
```

# <a name="windowstabwindows"></a>[<span data-ttu-id="10bc5-200">Windows</span><span class="sxs-lookup"><span data-stu-id="10bc5-200">Windows</span></span>](#tab/windows)

```console
pref set editor.command.default "C:\Program Files\Microsoft VS Code\Code.exe"
```

---

<span data-ttu-id="10bc5-201">Para iniciar el editor de texto predeterminado con argumentos específicos de la CLI, establezca la clave `editor.command.default.arguments`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-201">To launch the default text editor with specific CLI arguments, set the `editor.command.default.arguments` key.</span></span> <span data-ttu-id="10bc5-202">Por ejemplo, supongamos que Visual Studio Code es el editor de texto predeterminado y que siempre quiere que HTTP REPL abra Visual Studio Code en una nueva sesión con las extensiones deshabilitadas.</span><span class="sxs-lookup"><span data-stu-id="10bc5-202">For example, assume Visual Studio Code is the default text editor and that you always want the HTTP REPL to open Visual Studio Code in a new session with extensions disabled.</span></span> <span data-ttu-id="10bc5-203">Ejecute el siguiente comando:</span><span class="sxs-lookup"><span data-stu-id="10bc5-203">Run the following command:</span></span>

```console
pref set editor.command.default.arguments "--disable-extensions --new-window"
```

### <a name="set-the-swagger-search-paths"></a><span data-ttu-id="10bc5-204">Establecer las rutas de acceso de búsqueda de Swagger</span><span class="sxs-lookup"><span data-stu-id="10bc5-204">Set the Swagger search paths</span></span>

<span data-ttu-id="10bc5-205">De forma predeterminada, HTTP REPL tiene un conjunto de rutas de acceso relativas que usa para buscar el documento de Swagger al ejecutar el comando `connect` sin la opción `--swagger`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-205">By default, the HTTP REPL has a set of relative paths that it uses to find the Swagger document when executing the `connect` command without the `--swagger` option.</span></span> <span data-ttu-id="10bc5-206">Estas rutas de acceso relativas se combinan con las rutas de acceso raíz y base especificadas en el comando `connect`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-206">These relative paths are combined with the root and base paths specified in the `connect` command.</span></span> <span data-ttu-id="10bc5-207">Las rutas de acceso relativas predeterminadas son:</span><span class="sxs-lookup"><span data-stu-id="10bc5-207">The default relative paths are:</span></span>

- <span data-ttu-id="10bc5-208">*swagger.json*</span><span class="sxs-lookup"><span data-stu-id="10bc5-208">*swagger.json*</span></span>
- <span data-ttu-id="10bc5-209">*swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="10bc5-209">*swagger/v1/swagger.json*</span></span>
- <span data-ttu-id="10bc5-210">*/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="10bc5-210">*/swagger.json*</span></span>
- <span data-ttu-id="10bc5-211">*/swagger/v1/swagger.json*</span><span class="sxs-lookup"><span data-stu-id="10bc5-211">*/swagger/v1/swagger.json*</span></span>

<span data-ttu-id="10bc5-212">Para usar un conjunto diferente de rutas de acceso de búsqueda en el entorno, establezca la preferencia `swagger.searchPaths`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-212">To use a different set of search paths in your environment, set the `swagger.searchPaths` preference.</span></span> <span data-ttu-id="10bc5-213">El valor debe ser una lista delimitada por canalizaciones de rutas de acceso relativas.</span><span class="sxs-lookup"><span data-stu-id="10bc5-213">The value must be a pipe-delimited list of relative paths.</span></span> <span data-ttu-id="10bc5-214">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-214">For example:</span></span>

```console
pref set swagger.searchPaths "swagger/v2/swagger.json|swagger/v3/swagger.json"
```

## <a name="test-http-get-requests"></a><span data-ttu-id="10bc5-215">Prueba de solicitudes HTTP GET</span><span class="sxs-lookup"><span data-stu-id="10bc5-215">Test HTTP GET requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="10bc5-216">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="10bc5-216">Synopsis</span></span>

```console
get <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="10bc5-217">Argumentos</span><span class="sxs-lookup"><span data-stu-id="10bc5-217">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="10bc5-218">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="10bc5-218">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="10bc5-219">Opciones</span><span class="sxs-lookup"><span data-stu-id="10bc5-219">Options</span></span>

<span data-ttu-id="10bc5-220">Las siguientes opciones están disponibles para el comando `get`:</span><span class="sxs-lookup"><span data-stu-id="10bc5-220">The following options are available for the `get` command:</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="10bc5-221">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="10bc5-221">Example</span></span>

<span data-ttu-id="10bc5-222">Para emitir una solicitud HTTP GET realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-222">To issue an HTTP GET request:</span></span>

1. <span data-ttu-id="10bc5-223">Ejecute el comando `get` en un punto de conexión que lo admita:</span><span class="sxs-lookup"><span data-stu-id="10bc5-223">Run the `get` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ get
    ```

    <span data-ttu-id="10bc5-224">El comando anterior muestra el siguiente formato de salida:</span><span class="sxs-lookup"><span data-stu-id="10bc5-224">The preceding command displays the following output format:</span></span>

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

1. <span data-ttu-id="10bc5-225">Recupere un registro específico pasando un parámetro al comando `get`:</span><span class="sxs-lookup"><span data-stu-id="10bc5-225">Retrieve a specific record by passing a parameter to the `get` command:</span></span>

    ```console
    https://localhost:5001/people~ get 2
    ```

    <span data-ttu-id="10bc5-226">El comando anterior muestra el siguiente formato de salida:</span><span class="sxs-lookup"><span data-stu-id="10bc5-226">The preceding command displays the following output format:</span></span>

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

## <a name="test-http-post-requests"></a><span data-ttu-id="10bc5-227">Prueba de solicitudes HTTP POST</span><span class="sxs-lookup"><span data-stu-id="10bc5-227">Test HTTP POST requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="10bc5-228">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="10bc5-228">Synopsis</span></span>

```console
post <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="10bc5-229">Argumentos</span><span class="sxs-lookup"><span data-stu-id="10bc5-229">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="10bc5-230">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="10bc5-230">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="10bc5-231">Opciones</span><span class="sxs-lookup"><span data-stu-id="10bc5-231">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="10bc5-232">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="10bc5-232">Example</span></span>

<span data-ttu-id="10bc5-233">Para emitir una solicitud HTTP POST, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-233">To issue an HTTP POST request:</span></span>

1. <span data-ttu-id="10bc5-234">Ejecute el comando `post` en un punto de conexión que lo admita:</span><span class="sxs-lookup"><span data-stu-id="10bc5-234">Run the `post` command on an endpoint that supports it:</span></span>

    ```console
    https://localhost:5001/people~ post -h Content-Type=application/json
    ```

    <span data-ttu-id="10bc5-235">En el comando anterior, el encabezado de solicitud HTTP `Content-Type` se establece para indicar un tipo de medios de cuerpo de la solicitud de JSON.</span><span class="sxs-lookup"><span data-stu-id="10bc5-235">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="10bc5-236">El editor de texto predeterminado abre un archivo *.tmp* con una plantilla JSON que representa el cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="10bc5-236">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="10bc5-237">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-237">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="10bc5-238">Para establecer el editor de texto predeterminado, consulte la sección [Establecimiento del editor de texto predeterminado](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="10bc5-238">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="10bc5-239">Modifique la plantilla JSON para cumplir los requisitos de validación de modelos:</span><span class="sxs-lookup"><span data-stu-id="10bc5-239">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 0,
      "name": "Scott Addie"
    }
    ```

1. <span data-ttu-id="10bc5-240">Guarde el archivo *.tmp* y cierre el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="10bc5-240">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="10bc5-241">En el shell del comando aparecerá la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-241">The following output appears in the command shell:</span></span>

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

## <a name="test-http-put-requests"></a><span data-ttu-id="10bc5-242">Prueba de solicitudes HTTP PUT</span><span class="sxs-lookup"><span data-stu-id="10bc5-242">Test HTTP PUT requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="10bc5-243">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="10bc5-243">Synopsis</span></span>

```console
put <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="10bc5-244">Argumentos</span><span class="sxs-lookup"><span data-stu-id="10bc5-244">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="10bc5-245">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="10bc5-245">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="10bc5-246">Opciones</span><span class="sxs-lookup"><span data-stu-id="10bc5-246">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

### <a name="example"></a><span data-ttu-id="10bc5-247">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="10bc5-247">Example</span></span>

<span data-ttu-id="10bc5-248">Para emitir una solicitud HTTP PUT, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-248">To issue an HTTP PUT request:</span></span>

1. <span data-ttu-id="10bc5-249">*Opcional*: Ejecute el comando `get` para ver los datos antes de modificarlos:</span><span class="sxs-lookup"><span data-stu-id="10bc5-249">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="10bc5-250">En el comando anterior, el encabezado de solicitud HTTP `Content-Type` se establece para indicar un tipo de medios de cuerpo de la solicitud de JSON.</span><span class="sxs-lookup"><span data-stu-id="10bc5-250">In the preceding command, the `Content-Type` HTTP request header is set to indicate a request body media type of JSON.</span></span> <span data-ttu-id="10bc5-251">El editor de texto predeterminado abre un archivo *.tmp* con una plantilla JSON que representa el cuerpo de la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="10bc5-251">The default text editor opens a *.tmp* file with a JSON template representing the HTTP request body.</span></span> <span data-ttu-id="10bc5-252">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-252">For example:</span></span>

    ```json
    {
      "id": 0,
      "name": ""
    }
    ```

    > [!TIP]
    > <span data-ttu-id="10bc5-253">Para establecer el editor de texto predeterminado, consulte la sección [Establecimiento del editor de texto predeterminado](#set-the-default-text-editor).</span><span class="sxs-lookup"><span data-stu-id="10bc5-253">To set the default text editor, see the [Set the default text editor](#set-the-default-text-editor) section.</span></span>

1. <span data-ttu-id="10bc5-254">Modifique la plantilla JSON para cumplir los requisitos de validación de modelos:</span><span class="sxs-lookup"><span data-stu-id="10bc5-254">Modify the JSON template to satisfy model validation requirements:</span></span>

    ```json
    {
      "id": 2,
      "name": "Cherry"
    }
    ```

1. <span data-ttu-id="10bc5-255">Guarde el archivo *.tmp* y cierre el editor de texto.</span><span class="sxs-lookup"><span data-stu-id="10bc5-255">Save the *.tmp* file, and close the text editor.</span></span> <span data-ttu-id="10bc5-256">En el shell del comando aparecerá la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-256">The following output appears in the command shell:</span></span>

    ```console
    [main 2019-06-28T17:27:01.805Z] update#setState idle
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:28:21 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="10bc5-257">*Opcional*: Emita un comando `get` para ver las modificaciones.</span><span class="sxs-lookup"><span data-stu-id="10bc5-257">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="10bc5-258">Por ejemplo, si ha escrito "Cherry" en el editor de texto, un elemento `get` devuelve lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-258">For example, if you typed "Cherry" in the text editor, a `get` returns the following:</span></span>

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

## <a name="test-http-delete-requests"></a><span data-ttu-id="10bc5-259">Prueba de solicitudes HTTP DELETE</span><span class="sxs-lookup"><span data-stu-id="10bc5-259">Test HTTP DELETE requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="10bc5-260">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="10bc5-260">Synopsis</span></span>

```console
delete <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="10bc5-261">Argumentos</span><span class="sxs-lookup"><span data-stu-id="10bc5-261">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="10bc5-262">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="10bc5-262">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="10bc5-263">Opciones</span><span class="sxs-lookup"><span data-stu-id="10bc5-263">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

### <a name="example"></a><span data-ttu-id="10bc5-264">Ejemplo</span><span class="sxs-lookup"><span data-stu-id="10bc5-264">Example</span></span>

<span data-ttu-id="10bc5-265">Para emitir una solicitud HTTP DELETE, realice lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-265">To issue an HTTP DELETE request:</span></span>

1. <span data-ttu-id="10bc5-266">*Opcional*: Ejecute el comando `get` para ver los datos antes de modificarlos:</span><span class="sxs-lookup"><span data-stu-id="10bc5-266">*Optional*: Run the `get` command to view the data before modifying it:</span></span>

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

    <span data-ttu-id="10bc5-267">El comando anterior muestra el siguiente formato de salida:</span><span class="sxs-lookup"><span data-stu-id="10bc5-267">The preceding command displays the following output format:</span></span>

    ```console
    HTTP/1.1 204 No Content
    Date: Fri, 28 Jun 2019 17:36:42 GMT
    Server: Kestrel
    ```

1. <span data-ttu-id="10bc5-268">*Opcional*: Emita un comando `get` para ver las modificaciones.</span><span class="sxs-lookup"><span data-stu-id="10bc5-268">*Optional*: Issue a `get` command to see the modifications.</span></span> <span data-ttu-id="10bc5-269">En este ejemplo, un elemento `get` devuelve lo siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-269">In this example, a `get` returns the following:</span></span>

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

## <a name="test-http-patch-requests"></a><span data-ttu-id="10bc5-270">Prueba de solicitudes HTTP PATCH</span><span class="sxs-lookup"><span data-stu-id="10bc5-270">Test HTTP PATCH requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="10bc5-271">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="10bc5-271">Synopsis</span></span>

```console
patch <PARAMETER> [-c|--content] [-f|--file] [-h|--header] [--no-body] [-F|--no-formatting] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="10bc5-272">Argumentos</span><span class="sxs-lookup"><span data-stu-id="10bc5-272">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="10bc5-273">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="10bc5-273">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="10bc5-274">Opciones</span><span class="sxs-lookup"><span data-stu-id="10bc5-274">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

[!INCLUDE [HTTP request body CLI options](~/includes/http-repl/requires-body-options.md)]

## <a name="test-http-head-requests"></a><span data-ttu-id="10bc5-275">Prueba de solicitudes HTTP HEAD</span><span class="sxs-lookup"><span data-stu-id="10bc5-275">Test HTTP HEAD requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="10bc5-276">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="10bc5-276">Synopsis</span></span>

```console
head <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="10bc5-277">Argumentos</span><span class="sxs-lookup"><span data-stu-id="10bc5-277">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="10bc5-278">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="10bc5-278">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="10bc5-279">Opciones</span><span class="sxs-lookup"><span data-stu-id="10bc5-279">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="test-http-options-requests"></a><span data-ttu-id="10bc5-280">Prueba de solicitudes HTTP OPTIONS</span><span class="sxs-lookup"><span data-stu-id="10bc5-280">Test HTTP OPTIONS requests</span></span>

### <a name="synopsis"></a><span data-ttu-id="10bc5-281">Sinopsis</span><span class="sxs-lookup"><span data-stu-id="10bc5-281">Synopsis</span></span>

```console
options <PARAMETER> [-F|--no-formatting] [-h|--header] [--response] [--response:body] [--response:headers] [-s|--streaming]
```

### <a name="arguments"></a><span data-ttu-id="10bc5-282">Argumentos</span><span class="sxs-lookup"><span data-stu-id="10bc5-282">Arguments</span></span>

`PARAMETER`

<span data-ttu-id="10bc5-283">Se trata del parámetro de ruta, si existe, que espera el método de acción del controlador asociado.</span><span class="sxs-lookup"><span data-stu-id="10bc5-283">The route parameter, if any, expected by the associated controller action method.</span></span>

### <a name="options"></a><span data-ttu-id="10bc5-284">Opciones</span><span class="sxs-lookup"><span data-stu-id="10bc5-284">Options</span></span>

[!INCLUDE [standard CLI options](~/includes/http-repl/standard-options.md)]

## <a name="set-http-request-headers"></a><span data-ttu-id="10bc5-285">Establecimiento de encabezados de solicitudes HTTP</span><span class="sxs-lookup"><span data-stu-id="10bc5-285">Set HTTP request headers</span></span>

<span data-ttu-id="10bc5-286">Para establecer un encabezado de solicitud HTTP, use uno de los métodos siguientes:</span><span class="sxs-lookup"><span data-stu-id="10bc5-286">To set an HTTP request header, use one of the following approaches:</span></span>

1. <span data-ttu-id="10bc5-287">Establézcalo insertado con la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="10bc5-287">Set inline with the HTTP request.</span></span> <span data-ttu-id="10bc5-288">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-288">For example:</span></span>

  ```console
  https://localhost:5001/people~ post -h Content-Type=application/json
  ```

  <span data-ttu-id="10bc5-289">Con el método anterior, cada encabezado de solicitud HTTP distinto requiere su propia opción `-h`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-289">With the preceding approach, each distinct HTTP request header requires its own `-h` option.</span></span>

1. <span data-ttu-id="10bc5-290">Establézcalo antes de enviar la solicitud HTTP.</span><span class="sxs-lookup"><span data-stu-id="10bc5-290">Set before sending the HTTP request.</span></span> <span data-ttu-id="10bc5-291">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-291">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type application/json
  ```

  <span data-ttu-id="10bc5-292">Al establecer el encabezado antes de enviar una solicitud, este permanece establecido mientras dure la sesión de shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="10bc5-292">When setting the header before sending a request, the header remains set for the duration of the command shell session.</span></span> <span data-ttu-id="10bc5-293">Para borrar el encabezado, proporcione un valor vacío.</span><span class="sxs-lookup"><span data-stu-id="10bc5-293">To clear the header, provide an empty value.</span></span> <span data-ttu-id="10bc5-294">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-294">For example:</span></span>

  ```console
  https://localhost:5001/people~ set header Content-Type
  ```

## <a name="toggle-http-request-display"></a><span data-ttu-id="10bc5-295">Alternancia de la pantalla de solicitud HTTP</span><span class="sxs-lookup"><span data-stu-id="10bc5-295">Toggle HTTP request display</span></span>

<span data-ttu-id="10bc5-296">De manera predeterminada, se suprime la pantalla de solicitud HTTP que se está enviando.</span><span class="sxs-lookup"><span data-stu-id="10bc5-296">By default, display of the HTTP request being sent is suppressed.</span></span> <span data-ttu-id="10bc5-297">Es posible cambiar la configuración correspondiente mientras dure la sesión de shell de comandos.</span><span class="sxs-lookup"><span data-stu-id="10bc5-297">It's possible to change the corresponding setting for the duration of the command shell session.</span></span>

### <a name="enable-request-display"></a><span data-ttu-id="10bc5-298">Habilitación de la pantalla de solicitudes</span><span class="sxs-lookup"><span data-stu-id="10bc5-298">Enable request display</span></span>

<span data-ttu-id="10bc5-299">Vea la solicitud HTTP que se envía mediante la ejecución del comando `echo on`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-299">View the HTTP request being sent by running the `echo on` command.</span></span> <span data-ttu-id="10bc5-300">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-300">For example:</span></span>

```console
https://localhost:5001/people~ echo on
Request echoing is on
```

<span data-ttu-id="10bc5-301">Las solicitudes HTTP posteriores de la sesión actual muestran los encabezados de la solicitud.</span><span class="sxs-lookup"><span data-stu-id="10bc5-301">Subsequent HTTP requests in the current session display the request headers.</span></span> <span data-ttu-id="10bc5-302">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-302">For example:</span></span>

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

### <a name="disable-request-display"></a><span data-ttu-id="10bc5-303">Deshabilitación de la pantalla de solicitudes</span><span class="sxs-lookup"><span data-stu-id="10bc5-303">Disable request display</span></span>

<span data-ttu-id="10bc5-304">Suprima la pantalla de solicitud HTTP que se envía mediante la ejecución del comando `echo off`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-304">Suppress display of the HTTP request being sent by running the `echo off` command.</span></span> <span data-ttu-id="10bc5-305">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-305">For example:</span></span>

```console
https://localhost:5001/people~ echo off
Request echoing is off
```

## <a name="run-a-script"></a><span data-ttu-id="10bc5-306">Ejecución de un script</span><span class="sxs-lookup"><span data-stu-id="10bc5-306">Run a script</span></span>

<span data-ttu-id="10bc5-307">Si ejecuta con frecuencia el mismo conjunto de comandos de HTTP REPL, considere la posibilidad de almacenarlos en un archivo de texto.</span><span class="sxs-lookup"><span data-stu-id="10bc5-307">If you frequently execute the same set of HTTP REPL commands, consider storing them in a text file.</span></span> <span data-ttu-id="10bc5-308">Los comandos del archivo toman el mismo formulario que los ejecutados manualmente en la línea de comandos.</span><span class="sxs-lookup"><span data-stu-id="10bc5-308">Commands in the file take the same form as those executed manually on the command line.</span></span> <span data-ttu-id="10bc5-309">Los comandos se pueden ejecutar en un modo por lotes mediante el comando `run`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-309">The commands can be executed in a batched fashion using the `run` command.</span></span> <span data-ttu-id="10bc5-310">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-310">For example:</span></span>

1. <span data-ttu-id="10bc5-311">Cree un archivo de texto que contenga un conjunto de comandos delimitados por línea nueva.</span><span class="sxs-lookup"><span data-stu-id="10bc5-311">Create a text file containing a set of newline-delimited commands.</span></span> <span data-ttu-id="10bc5-312">Para ilustrarlo, considere la posibilidad de usar un archivo *people-script.txt* que contenga los comandos siguientes:</span><span class="sxs-lookup"><span data-stu-id="10bc5-312">To illustrate, consider a *people-script.txt* file containing the following commands:</span></span>

    ```text
    set base https://localhost:5001
    ls
    cd People
    ls
    get 1
    ```

1. <span data-ttu-id="10bc5-313">Ejecute el comando `run`, pasando la ruta de acceso del archivo de texto.</span><span class="sxs-lookup"><span data-stu-id="10bc5-313">Execute the `run` command, passing in the text file's path.</span></span> <span data-ttu-id="10bc5-314">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="10bc5-314">For example:</span></span>

    ```console
    https://localhost:5001/~ run C:\http-repl-scripts\people-script.txt
    ```

    <span data-ttu-id="10bc5-315">Aparece el siguiente resultado:</span><span class="sxs-lookup"><span data-stu-id="10bc5-315">The following output appears:</span></span>

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

## <a name="clear-the-output"></a><span data-ttu-id="10bc5-316">Borrado de la salida</span><span class="sxs-lookup"><span data-stu-id="10bc5-316">Clear the output</span></span>

<span data-ttu-id="10bc5-317">Para quitar todas las salidas escritas en el shell de comandos mediante la herramienta REPL HTTP, ejecute el comando `clear` o `cls`.</span><span class="sxs-lookup"><span data-stu-id="10bc5-317">To remove all output written to the command shell by the HTTP REPL tool, run the `clear` or `cls` command.</span></span> <span data-ttu-id="10bc5-318">Para ilustrarlo, imagine que el shell de comandos contiene la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-318">To illustrate, imagine the command shell contains the following output:</span></span>

```console
httprepl https://localhost:5001
(Disconnected)~ set base "https://localhost:5001"
Using swagger metadata from https://localhost:5001/swagger/v1/swagger.json

https://localhost:5001/~ ls
.        []
Fruits   [get|post]
People   [get|post]

https://localhost:5001/~
```

<span data-ttu-id="10bc5-319">Ejecute el comando siguiente para borrar la salida:</span><span class="sxs-lookup"><span data-stu-id="10bc5-319">Run the following command to clear the output:</span></span>

```console
https://localhost:5001/~ clear
```

<span data-ttu-id="10bc5-320">Después de ejecutar el comando anterior, el shell de comandos solo contiene la salida siguiente:</span><span class="sxs-lookup"><span data-stu-id="10bc5-320">After running the preceding command, the command shell contains only the following output:</span></span>

```console
https://localhost:5001/~
```

## <a name="additional-resources"></a><span data-ttu-id="10bc5-321">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="10bc5-321">Additional resources</span></span>

* [<span data-ttu-id="10bc5-322">Solicitudes de API REST</span><span class="sxs-lookup"><span data-stu-id="10bc5-322">REST API requests</span></span>](https://github.com/microsoft/api-guidelines/blob/vNext/Guidelines.md#74-supported-methods)
* [<span data-ttu-id="10bc5-323">Repositorio GitHub de HTTP REPL</span><span class="sxs-lookup"><span data-stu-id="10bc5-323">HTTP REPL GitHub repository</span></span>](https://github.com/aspnet/HttpRepl)
