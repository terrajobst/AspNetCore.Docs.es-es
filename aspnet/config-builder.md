---
uid: config-builder
title: Generadores de configuración de ASP.NET
author: rick-anderson
description: Obtenga información sobre cómo obtener datos de configuración de orígenes distintos a los valores del archivo web.config desde orígenes externos.
ms.author: riande
ms.date: 10/29/2018
ms.technology: aspnet
msc.type: content
ms.openlocfilehash: 4dcc62573fad13ec8b37b2c59e884eec7ca80b92
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148842"
---
# <a name="configuration-builders-for-aspnet"></a><span data-ttu-id="2ecdd-103">Generadores de configuración de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="2ecdd-103">Configuration builders for ASP.NET</span></span>

<span data-ttu-id="2ecdd-104">Por [Stephen Molloy](https://github.com/StephenMolloy) y [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2ecdd-104">By [Stephen Molloy](https://github.com/StephenMolloy) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2ecdd-105">Los generadores de configuración proporcionan un mecanismo modernas y ágil para las aplicaciones ASP.NET obtener los valores de configuración de orígenes externos.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-105">Configuration builders provide a modern and agile mechanism for ASP.NET apps to get configuration values from external sources.</span></span>

<span data-ttu-id="2ecdd-106">Generadores de configuración:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-106">Configuration builders:</span></span>

* <span data-ttu-id="2ecdd-107">Están disponibles en .NET Framework 4.7.1 y versiones posteriores.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-107">Are available in .NET Framework 4.7.1 and later.</span></span>
* <span data-ttu-id="2ecdd-108">Proporcionan un mecanismo flexible para leer los valores de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-108">Provide a flexible mechanism for reading configuration values.</span></span>
* <span data-ttu-id="2ecdd-109">Solucionar algunas de las necesidades básicas de las aplicaciones cuando se mueven en un contenedor y un entorno centrado en nube.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-109">Address some of the basic needs of apps as they move into a container and cloud focused environment.</span></span>
* <span data-ttu-id="2ecdd-110">Puede utilizarse para mejorar la protección de datos de configuración mediante el uso de orígenes que no estaban disponibles (por ejemplo, Azure Key Vault y el entorno de variables) en el sistema de configuración. NET.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-110">Can be used to improve protection of configuration data by drawing from sources previously unavailable (for example, Azure Key Vault and environment variables) in the .NET configuration system.</span></span>

## <a name="keyvalue-configuration-builders"></a><span data-ttu-id="2ecdd-111">Generadores de configuración de clave/valor</span><span class="sxs-lookup"><span data-stu-id="2ecdd-111">Key/value configuration builders</span></span>

<span data-ttu-id="2ecdd-112">Un escenario común que puede controlarse mediante generadores de configuración es proporcionar un mecanismo de reemplazo básico de pares clave-valor para secciones de configuración que siguen un patrón de clave/valor.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-112">A common scenario that can be handled by configuration builders is to provide a basic key/value replacement mechanism for configuration sections that follow a key/value pattern.</span></span> <span data-ttu-id="2ecdd-113">El concepto de .NET Framework de ConfigurationBuilders no se limita a secciones de configuración específico o patrones.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-113">The .NET Framework concept of ConfigurationBuilders is not limited to specific configuration sections or patterns.</span></span> <span data-ttu-id="2ecdd-114">Sin embargo, muchos de los generadores de configuración en `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funcionan dentro del patrón de clave/valor.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-114">However, many of the configuration builders in `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) work within the key/value pattern.</span></span>

## <a name="keyvalue-configuration-builders-settings"></a><span data-ttu-id="2ecdd-115">Valores de los generadores de configuración de clave/valor</span><span class="sxs-lookup"><span data-stu-id="2ecdd-115">Key/value configuration builders settings</span></span>

<span data-ttu-id="2ecdd-116">Las siguientes opciones se aplican a todos los generadores de configuración de pares clave-valor en `Microsoft.Configuration.ConfigurationBuilders`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-116">The following settings apply to all key/value configuration builders in `Microsoft.Configuration.ConfigurationBuilders`.</span></span>

### <a name="mode"></a><span data-ttu-id="2ecdd-117">Modo</span><span class="sxs-lookup"><span data-stu-id="2ecdd-117">Mode</span></span>

<span data-ttu-id="2ecdd-118">Los generadores de configuración usar un origen externo de la información de clave/valor para rellenar los elementos de clave/valor seleccionado del sistema de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-118">The configuration builders use an external source of key/value information to populate selected key/value elements of the configuration system.</span></span> <span data-ttu-id="2ecdd-119">En concreto, el `<appSettings/>` y `<connectionStrings/>` secciones reciben un tratamiento especial de los generadores de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-119">Specifically, the `<appSettings/>` and `<connectionStrings/>` sections receive special treatment from the configuration builders.</span></span> <span data-ttu-id="2ecdd-120">Los generadores de trabajo en tres modos:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-120">The builders work in three modes:</span></span>

* <span data-ttu-id="2ecdd-121">`Strict` -El modo predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-121">`Strict` - The default mode.</span></span> <span data-ttu-id="2ecdd-122">En este modo, el generador de configuración solo funciona en las secciones de configuración de clave/valor-centric conocido.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-122">In this mode, the configuration builder only operates on well-known key/value-centric configuration sections.</span></span> <span data-ttu-id="2ecdd-123">`Strict` modo enumera cada clave en la sección.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-123">`Strict` mode enumerates each key in the section.</span></span> <span data-ttu-id="2ecdd-124">Si se encuentra una clave coincidente en el origen externo:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-124">If a matching key is found in the external source:</span></span>

   * <span data-ttu-id="2ecdd-125">Los generadores de configuración reemplaza el valor en la sección de configuración resultante con el valor del origen externo.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-125">The configuration builders replace the value in the resulting configuration section with the value from the external source.</span></span>
* <span data-ttu-id="2ecdd-126">`Greedy` -En este modo está estrechamente relacionada con `Strict` modo.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-126">`Greedy` - This mode is closely related to `Strict` mode.</span></span> <span data-ttu-id="2ecdd-127">En lugar de limitarse a las claves que ya existen en la configuración original:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-127">Rather than being limited to keys that already exist in the original configuration:</span></span>

  * <span data-ttu-id="2ecdd-128">Los generadores de configuración agrega todos los pares clave/valor desde el origen externo en la sección de configuración resultante.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-128">The configuration builders adds all key/value pairs from the external source into the resulting configuration section.</span></span>

* <span data-ttu-id="2ecdd-129">`Expand` -Se opera en el XML sin formato antes de se analiza en un objeto de sección de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-129">`Expand` - Operates on the raw XML before it's parsed into a configuration section object.</span></span> <span data-ttu-id="2ecdd-130">Se puede considerar como una expansión de tokens de una cadena.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-130">It can be thought of as an expansion of tokens in a string.</span></span> <span data-ttu-id="2ecdd-131">Cualquier parte de la cadena XML sin formato que coincida con el patrón `${token}` es un candidato para la expansión del token.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-131">Any part of the raw XML string that matches the pattern `${token}` is a candidate for token expansion.</span></span> <span data-ttu-id="2ecdd-132">Si se encuentra ningún valor correspondiente en el origen externo, no se cambia el token.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-132">If no corresponding value is found in the external source, then the token is not changed.</span></span> <span data-ttu-id="2ecdd-133">No se limitan a los generadores de este modo el `<appSettings/>` y `<connectionStrings/>` secciones.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-133">Builders in this mode are not limited to the `<appSettings/>` and `<connectionStrings/>` sections.</span></span>

<span data-ttu-id="2ecdd-134">El siguiente marcado de *web.config* permite la [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) en `Strict` modo:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-134">The following markup from *web.config* enables the [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) in `Strict` mode:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

<span data-ttu-id="2ecdd-135">El siguiente código lee la `<appSettings/>` y `<connectionStrings/>` se muestra en la anterior *web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-135">The following code reads the `<appSettings/>` and `<connectionStrings/>` shown in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

<span data-ttu-id="2ecdd-136">El código anterior establecerá los valores de propiedad:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-136">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="2ecdd-137">Los valores de la *web.config* archivo si las claves no se establecen en variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-137">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="2ecdd-138">Los valores del entorno de variable, si establece.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-138">The values of the environment variable, if set.</span></span>

<span data-ttu-id="2ecdd-139">Por ejemplo, `ServiceID` contendrá:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-139">For example, `ServiceID` will contain:</span></span>

* <span data-ttu-id="2ecdd-140">"Valor ServiceID del archivo web.config", si la variable de entorno `ServiceID` no está establecida.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-140">"ServiceID value from web.config", if the environment variable `ServiceID` is not set.</span></span>
* <span data-ttu-id="2ecdd-141">El valor de la `ServiceID` if de variable de entorno establecido.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-141">The value of the `ServiceID` environment variable, if set.</span></span>

<span data-ttu-id="2ecdd-142">La siguiente imagen muestra la `<appSettings/>` claves/valores de los anteriores *web.config* conjunto de archivos en el editor de entorno:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-142">The following image shows the `<appSettings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![editor del entorno](config-builder/static/env.png)

<span data-ttu-id="2ecdd-144">Nota: Es posible que deba salir y reiniciar Visual Studio para ver los cambios en las variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-144">Note: You might need to exit and restart Visual Studio to see changes in environment variables.</span></span>

### <a name="prefix-handling"></a><span data-ttu-id="2ecdd-145">Control de prefijo</span><span class="sxs-lookup"><span data-stu-id="2ecdd-145">Prefix handling</span></span>

<span data-ttu-id="2ecdd-146">Los prefijos de la claves pueden simplificar las claves de configuración porque:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-146">Key prefixes can simplify setting keys because:</span></span>

* <span data-ttu-id="2ecdd-147">La configuración de .NET Framework es compleja y anidada.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-147">The .NET Framework configuration is complex and nested.</span></span>
* <span data-ttu-id="2ecdd-148">Orígenes externos de clave/valor normalmente son básicas y sin formato por naturaleza.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-148">External key/value sources are commonly basic and flat by nature.</span></span> <span data-ttu-id="2ecdd-149">Por ejemplo, las variables de entorno no están anidadas.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-149">For example, environment variables are not nested.</span></span>

<span data-ttu-id="2ecdd-150">Usar cualquiera de los siguientes métodos para insertar ambos `<appSettings/>` y `<connectionStrings/>` en la configuración a través de las variables de entorno:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-150">Use any of the following approaches to inject both `<appSettings/>` and `<connectionStrings/>` into the configuration via environment variables:</span></span>

* <span data-ttu-id="2ecdd-151">Con el `EnvironmentConfigBuilder` en el valor predeterminado `Strict` modo y los nombres de clave adecuados en el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-151">With the `EnvironmentConfigBuilder` in the default `Strict` mode and the appropriate key names in the configuration file.</span></span> <span data-ttu-id="2ecdd-152">El código y el marcado anterior adopta este enfoque.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-152">The preceding code and markup takes this approach.</span></span> <span data-ttu-id="2ecdd-153">Con este enfoque puede **no** han con el mismo nombre tanto en las claves de `<appSettings/>` y `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-153">Using this approach you can **not** have identically named keys in both `<appSettings/>` and `<connectionStrings/>`.</span></span>
* <span data-ttu-id="2ecdd-154">Use dos `EnvironmentConfigBuilder`s en `Greedy` modo con prefijos diferentes y `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-154">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes and `stripPrefix`.</span></span> <span data-ttu-id="2ecdd-155">Con este enfoque, la aplicación puede leer `<appSettings/>` y `<connectionStrings/>` sin necesidad de actualizar el archivo de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-155">With this approach, the app can read `<appSettings/>` and `<connectionStrings/>` without needing to update the configuration file.</span></span> <span data-ttu-id="2ecdd-156">La siguiente sección, [stripPrefix](#stripprefix), se muestra cómo hacerlo.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-156">The next section,  [stripPrefix](#stripprefix),  shows how to do this.</span></span>
* <span data-ttu-id="2ecdd-157">Use dos `EnvironmentConfigBuilder`s en `Greedy` modo con prefijos diferentes.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-157">Use two `EnvironmentConfigBuilder`s in `Greedy` mode with distinct prefixes.</span></span> <span data-ttu-id="2ecdd-158">Con este enfoque no puede tener nombres duplicados de clave como nombres de clave deben diferenciarse por el prefijo.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-158">With this approach you can't have duplicate key names as key names must differ by prefix.</span></span>  <span data-ttu-id="2ecdd-159">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-159">For example:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

<span data-ttu-id="2ecdd-160">Con el marcado anterior, puede utilizarse el mismo origen de clave/valor sin formato para rellenar la configuración de dos secciones diferentes.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-160">With the preceding markup, the same flat key/value source can be used to populate configuration for two different sections.</span></span>

<span data-ttu-id="2ecdd-161">La siguiente imagen muestra la `<appSettings/>` y `<connectionStrings/>` claves/valores de los anteriores *web.config* conjunto de archivos en el editor de entorno:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-161">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![editor del entorno](config-builder/static/prefix.png)

<span data-ttu-id="2ecdd-163">El siguiente código lee la `<appSettings/>` y `<connectionStrings/>` claves/valores contenidos en los anteriores *web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-163">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

<span data-ttu-id="2ecdd-164">El código anterior establecerá los valores de propiedad:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-164">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="2ecdd-165">Los valores de la *web.config* archivo si las claves no se establecen en variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-165">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="2ecdd-166">Los valores del entorno de variable, si establece.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-166">The values of the environment variable, if set.</span></span>

<span data-ttu-id="2ecdd-167">Por ejemplo, con el anterior *web.config* archivo, se establecen los valores de claves o en la imagen anterior de editor de entorno y el código anterior, los valores siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-167">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="2ecdd-168">Key</span><span class="sxs-lookup"><span data-stu-id="2ecdd-168">Key</span></span>              | <span data-ttu-id="2ecdd-169">Valor</span><span class="sxs-lookup"><span data-stu-id="2ecdd-169">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="2ecdd-170">AppSetting_ServiceID</span><span class="sxs-lookup"><span data-stu-id="2ecdd-170">AppSetting_ServiceID</span></span>           | <span data-ttu-id="2ecdd-171">AppSetting_ServiceID desde variables de entorno</span><span class="sxs-lookup"><span data-stu-id="2ecdd-171">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="2ecdd-172">AppSetting_default</span><span class="sxs-lookup"><span data-stu-id="2ecdd-172">AppSetting_default</span></span>            | <span data-ttu-id="2ecdd-173">Valor AppSetting_default desde env</span><span class="sxs-lookup"><span data-stu-id="2ecdd-173">AppSetting_default value from env</span></span> |
|       <span data-ttu-id="2ecdd-174">ConnStr_default</span><span class="sxs-lookup"><span data-stu-id="2ecdd-174">ConnStr_default</span></span>         | <span data-ttu-id="2ecdd-175">Val ConnStr_default desde env</span><span class="sxs-lookup"><span data-stu-id="2ecdd-175">ConnStr_default val from env</span></span>|

### <a name="stripprefix"></a><span data-ttu-id="2ecdd-176">stripPrefix</span><span class="sxs-lookup"><span data-stu-id="2ecdd-176">stripPrefix</span></span>

<span data-ttu-id="2ecdd-177">`stripPrefix`: un valor booleano, el valor predeterminado es `false`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-177">`stripPrefix`: boolean, defaults to `false`.</span></span> 

<span data-ttu-id="2ecdd-178">El marcado XML anterior separa la configuración de la aplicación de las cadenas de conexión, pero requiere que todas las claves de la *web.config* archivo que se usará el prefijo especificado.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-178">The preceding XML markup separates app settings from connection strings but requires all the keys in the *web.config* file to use the specified prefix.</span></span> <span data-ttu-id="2ecdd-179">Por ejemplo, el prefijo `AppSetting` deben agregarse a la `ServiceID` clave ("AppSetting_ServiceID").</span><span class="sxs-lookup"><span data-stu-id="2ecdd-179">For example, the prefix `AppSetting` must be added to the `ServiceID` key ("AppSetting_ServiceID").</span></span> <span data-ttu-id="2ecdd-180">Con `stripPrefix`, no se utiliza el prefijo en el *web.config* archivo.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-180">With `stripPrefix`, the prefix is not used in the *web.config* file.</span></span> <span data-ttu-id="2ecdd-181">Se requiere el prefijo en el origen del generador de configuración (por ejemplo, en el entorno). Se prevé que la mayoría de los desarrolladores usarán `stripPrefix`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-181">The prefix is required in the configuration builder source (for example, in the environment.) We anticipate most developers will use `stripPrefix`.</span></span>

<span data-ttu-id="2ecdd-182">Las aplicaciones normalmente eliminan el prefijo.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-182">Applications typically strip off the prefix.</span></span> <span data-ttu-id="2ecdd-183">La siguiente *web.config* elimina el prefijo:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-183">The following *web.config* strips the prefix:</span></span>

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

<span data-ttu-id="2ecdd-184">En la anterior *web.config* archivo, el `default` clave pertenece a ambos el `<appSettings/>` y `<connectionStrings/>`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-184">In the preceding *web.config* file, the `default` key is in both the `<appSettings/>` and `<connectionStrings/>`.</span></span>

<span data-ttu-id="2ecdd-185">La siguiente imagen muestra la `<appSettings/>` y `<connectionStrings/>` claves/valores de los anteriores *web.config* conjunto de archivos en el editor de entorno:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-185">The following image shows the `<appSettings/>` and `<connectionStrings/>` keys/values from the preceding *web.config* file set in the environment editor:</span></span>

![editor del entorno](config-builder/static/prefix.png)

<span data-ttu-id="2ecdd-187">El siguiente código lee la `<appSettings/>` y `<connectionStrings/>` claves/valores contenidos en los anteriores *web.config* archivo:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-187">The following code reads the `<appSettings/>` and `<connectionStrings/>` keys/values contained in the preceding *web.config* file:</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

<span data-ttu-id="2ecdd-188">El código anterior establecerá los valores de propiedad:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-188">The preceding code will set the property values to:</span></span>

* <span data-ttu-id="2ecdd-189">Los valores de la *web.config* archivo si las claves no se establecen en variables de entorno.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-189">The values in the *web.config* file if the keys are not set in environment variables.</span></span>
* <span data-ttu-id="2ecdd-190">Los valores del entorno de variable, si establece.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-190">The values of the environment variable, if set.</span></span>

<span data-ttu-id="2ecdd-191">Por ejemplo, con el anterior *web.config* archivo, se establecen los valores de claves o en la imagen anterior de editor de entorno y el código anterior, los valores siguientes:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-191">For example, using the previous *web.config* file, the keys/values in the previous  environment editor image, and the previous code, the following values are set:</span></span>

|  <span data-ttu-id="2ecdd-192">Key</span><span class="sxs-lookup"><span data-stu-id="2ecdd-192">Key</span></span>              | <span data-ttu-id="2ecdd-193">Valor</span><span class="sxs-lookup"><span data-stu-id="2ecdd-193">Value</span></span> |
| ----------------- | ------------ |
|     <span data-ttu-id="2ecdd-194">ServiceID</span><span class="sxs-lookup"><span data-stu-id="2ecdd-194">ServiceID</span></span>           | <span data-ttu-id="2ecdd-195">AppSetting_ServiceID desde variables de entorno</span><span class="sxs-lookup"><span data-stu-id="2ecdd-195">AppSetting_ServiceID from env variables</span></span>|
|    <span data-ttu-id="2ecdd-196">default</span><span class="sxs-lookup"><span data-stu-id="2ecdd-196">default</span></span>            | <span data-ttu-id="2ecdd-197">Valor AppSetting_default desde env</span><span class="sxs-lookup"><span data-stu-id="2ecdd-197">AppSetting_default value from env</span></span> |
|    <span data-ttu-id="2ecdd-198">default</span><span class="sxs-lookup"><span data-stu-id="2ecdd-198">default</span></span>         | <span data-ttu-id="2ecdd-199">Val ConnStr_default desde env</span><span class="sxs-lookup"><span data-stu-id="2ecdd-199">ConnStr_default val from env</span></span>|

### <a name="tokenpattern"></a><span data-ttu-id="2ecdd-200">tokenPattern</span><span class="sxs-lookup"><span data-stu-id="2ecdd-200">tokenPattern</span></span>

<span data-ttu-id="2ecdd-201">`tokenPattern`: Cadena, el valor predeterminado es `@"\$\{(\w+)\}"`</span><span class="sxs-lookup"><span data-stu-id="2ecdd-201">`tokenPattern`: String, defaults to `@"\$\{(\w+)\}"`</span></span>

<span data-ttu-id="2ecdd-202">El `Expand` comportamiento de los generadores de busca en el XML sin formato para los tokens que se parecen a `${token}`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-202">The `Expand` behavior of the builders searches the raw XML for tokens that look like `${token}`.</span></span> <span data-ttu-id="2ecdd-203">Búsqueda se realiza con la expresión regular predeterminada `@"\$\{(\w+)\}"`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-203">Searching is done with the default regular expression `@"\$\{(\w+)\}"`.</span></span> <span data-ttu-id="2ecdd-204">El juego de caracteres que coincide con `\w` es más estricta que permiten XML y muchos orígenes de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-204">The set of characters that matches `\w` is more strict than XML and many configuration sources allow.</span></span> <span data-ttu-id="2ecdd-205">Use `tokenPattern` cuando más caracteres que `@"\$\{(\w+)\}"` son necesarios en el nombre del token.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-205">Use `tokenPattern` when more characters than `@"\$\{(\w+)\}"` are required in the token name.</span></span>

<span data-ttu-id="2ecdd-206">`tokenPattern`: Cadena:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-206">`tokenPattern`: String:</span></span>

* <span data-ttu-id="2ecdd-207">Permite a los desarrolladores cambiar la expresión regular que se usa para la coincidencia de token.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-207">Allows developers to change the regex that is used for token matching.</span></span>
* <span data-ttu-id="2ecdd-208">Se realiza ninguna validación para asegurarse de que es un regex no peligrosas, tiene el formato correcto.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-208">No validation is done to make sure it is a well-formed, non-dangerous regex.</span></span>
* <span data-ttu-id="2ecdd-209">Debe contener un grupo de capturas.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-209">It must contain a capture group.</span></span> <span data-ttu-id="2ecdd-210">La expresión regular completa debe coincidir con todo el token.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-210">The entire regex must match the entire token.</span></span> <span data-ttu-id="2ecdd-211">La primera captura debe ser el nombre del token para buscar en el origen de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-211">The first capture must be the token name to look up in the configuration source.</span></span>

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a><span data-ttu-id="2ecdd-212">Generadores de configuración en Microsoft.Configuration.ConfigurationBuilders</span><span class="sxs-lookup"><span data-stu-id="2ecdd-212">Configuration builders in Microsoft.Configuration.ConfigurationBuilders</span></span>

### <a name="environmentconfigbuilder"></a><span data-ttu-id="2ecdd-213">EnvironmentConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="2ecdd-213">EnvironmentConfigBuilder</span></span>

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

<span data-ttu-id="2ecdd-214">El [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span><span class="sxs-lookup"><span data-stu-id="2ecdd-214">The [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):</span></span>

* <span data-ttu-id="2ecdd-215">Es el más sencillo de los generadores de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-215">Is the simplest of the configuration builders.</span></span>
* <span data-ttu-id="2ecdd-216">Lee los valores del entorno.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-216">Reads values from the environment.</span></span>
* <span data-ttu-id="2ecdd-217">No tiene ninguna opción de configuración adicionales.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-217">Does not have any additional configuration options.</span></span>
* <span data-ttu-id="2ecdd-218">El `name` valor del atributo es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-218">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="2ecdd-219">**Nota:** en un entorno de contenedor de Windows, las variables establecidas en tiempo de ejecución solo se insertan en el entorno de proceso de punto de entrada.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-219">**Note:** In a Windows container environment, variables set at run time are only injected into the EntryPoint process environment.</span></span> <span data-ttu-id="2ecdd-220">Las aplicaciones que se ejecutan como un servicio o un proceso que no sean EntryPoint no recopilan estas variables, a menos que en caso contrario, se insertan mediante un mecanismo en el contenedor.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-220">Apps that run as a service or a non-EntryPoint process do not pick up these variables unless they are otherwise injected through a mechanism in the container.</span></span> <span data-ttu-id="2ecdd-221">Para [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-basado en contenedores, la versión actual de [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) se encarga de ello en la *DefaultAppPool* solo.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-221">For [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-based containers, the current version of [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) handles this in the *DefaultAppPool* only.</span></span> <span data-ttu-id="2ecdd-222">Otras variantes de contenedor basadas en Windows que deba desarrollar su propio mecanismo de inserción para los procesos que no sean EntryPoint.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-222">Other Windows-based container variants may need to develop their own injection mechanism for non-EntryPoint processes.</span></span>

### <a name="usersecretsconfigbuilder"></a><span data-ttu-id="2ecdd-223">UserSecretsConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="2ecdd-223">UserSecretsConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="2ecdd-224">Nunca almacenar contraseñas, cadenas de conexión confidenciales u otros datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-224">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="2ecdd-225">Secretos de producción no deben usarse para el desarrollo o prueba.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-225">Production secrets should not be used for development or test.</span></span>

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

<span data-ttu-id="2ecdd-226">Este generador de configuración proporciona una característica similar a [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="2ecdd-226">This configuration builder provides a feature similar to [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).</span></span>

<span data-ttu-id="2ecdd-227">El [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) se puede usar en proyectos de .NET Framework, pero se debe especificar un archivo de secretos.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-227">The [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) can be used in .NET Framework projects, but a secrets file must be specified.</span></span> <span data-ttu-id="2ecdd-228">Como alternativa, puede definir el `UserSecretsId` propiedad en el proyecto de archivo y crea el archivo de secretos sin procesar en la ubicación adecuada para su lectura.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-228">Alternatively, you can define the `UserSecretsId` property in the project file and create the raw secrets file in the correct location for reading.</span></span> <span data-ttu-id="2ecdd-229">Para mantener las dependencias externas fuera de su proyecto, el archivo del secreto es con formato XML.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-229">To keep external dependencies out of your project, the secret file is XML formatted.</span></span> <span data-ttu-id="2ecdd-230">El formato XML es un detalle de implementación y el formato no se debería confiar en.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-230">The XML formatting is an implementation detail, and the format should not be relied upon.</span></span> <span data-ttu-id="2ecdd-231">Si necesita compartir un *secrets.json* archivo con proyectos de .NET Core, considere el uso de la [SimpleJsonConfigBuilder](#simplejsonconfig).</span><span class="sxs-lookup"><span data-stu-id="2ecdd-231">If you need to share a *secrets.json* file with .NET Core projects, consider using the [SimpleJsonConfigBuilder](#simplejsonconfig).</span></span> <span data-ttu-id="2ecdd-232">El `SimpleJsonConfigBuilder` para .NET Core también debe considerarse un detalle de implementación sujeta a cambios de formato.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-232">The `SimpleJsonConfigBuilder` format for .NET Core should also be considered an implementation detail subject to change.</span></span>

<span data-ttu-id="2ecdd-233">Configuración de atributos de `UserSecretsConfigBuilder`:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-233">Configuration attributes for `UserSecretsConfigBuilder`:</span></span>

* <span data-ttu-id="2ecdd-234">`userSecretsId` -Éste es el método preferido para identificar un archivo de secretos XML.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-234">`userSecretsId` - This is the preferred method for identifying an XML secrets file.</span></span> <span data-ttu-id="2ecdd-235">Funciona de manera similar a .NET Core, que usa un `UserSecretsId` propiedad para almacenar este identificador de proyecto.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-235">It works similar to .NET Core, which uses a `UserSecretsId` project property to store this identifier.</span></span> <span data-ttu-id="2ecdd-236">La cadena debe ser única, que no necesita ser un GUID.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-236">The string must be unique, it doesn't need to be a GUID.</span></span> <span data-ttu-id="2ecdd-237">Con este atributo, el `UserSecretsConfigBuilder` buscar en una ubicación local conocida (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) para un archivo de secretos que pertenecen a este identificador.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-237">With this attribute, the `UserSecretsConfigBuilder` look in a well-known local location (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) for a secrets file belonging to this identifier.</span></span>
* <span data-ttu-id="2ecdd-238">`userSecretsFile` -Un atributo opcional que especifica el archivo que contiene los secretos.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-238">`userSecretsFile` - An optional attribute specifying the file containing the secrets.</span></span> <span data-ttu-id="2ecdd-239">El `~` carácter puede usarse al principio para hacer referencia a la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-239">The `~` character can be used at the start to reference the application root.</span></span> <span data-ttu-id="2ecdd-240">Este atributo o la `userSecretsId` atributo es necesario.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-240">Either this attribute or the `userSecretsId` attribute is required.</span></span> <span data-ttu-id="2ecdd-241">Si se especifican ambos, `userSecretsFile` tiene prioridad.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-241">If both are specified, `userSecretsFile` takes precedence.</span></span>
* <span data-ttu-id="2ecdd-242">`optional`: valor booleano, valor predeterminado `true` -impide que una excepción si no se encuentra el archivo de secretos.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-242">`optional`: boolean, default value `true` - Prevents an exception if the secrets file cannot be found.</span></span> 
* <span data-ttu-id="2ecdd-243">El `name` valor del atributo es arbitrario.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-243">The `name` attribute value is arbitrary.</span></span>

<span data-ttu-id="2ecdd-244">El archivo de secretos tiene el formato siguiente:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-244">The secrets file has the following format:</span></span>

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a><span data-ttu-id="2ecdd-245">AzureKeyVaultConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="2ecdd-245">AzureKeyVaultConfigBuilder</span></span>

```xml
<add name="AzureKeyVault"
    [mode|prefix|stripPrefix|tokenPattern]
    (vaultName="MyVaultName" |
     uri="https:/MyVaultName.vault.azure.net")
    [connectionString="connection string"]
    [version="secrets version"]
    [preloadSecretNames="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.AzureKeyVaultConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Azure" />
```

<span data-ttu-id="2ecdd-246">El [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) lee los valores almacenados en el [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span><span class="sxs-lookup"><span data-stu-id="2ecdd-246">The [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) reads values stored in the [Azure Key Vault](/azure/key-vault/key-vault-whatis).</span></span>

<span data-ttu-id="2ecdd-247">`vaultName` es necesario (el nombre del almacén) o un URI en el almacén.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-247">`vaultName` is required (either the name of the vault or a URI to the vault).</span></span> <span data-ttu-id="2ecdd-248">Permitir que el control sobre qué almacén para conectarse a los otros atributos, pero son necesarios si la aplicación no se está ejecutando en un entorno que funciona con `Microsoft.Azure.Services.AppAuthentication`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-248">The other attributes allow control about which vault to connect to, but are only necessary if the application is not running in an environment that works with `Microsoft.Azure.Services.AppAuthentication`.</span></span> <span data-ttu-id="2ecdd-249">La biblioteca de autenticación de servicios de Azure se usa para seleccionar automáticamente la información de conexión desde el entorno de ejecución, si es posible.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-249">The Azure Services Authentication library is used to automatically pick up connection information from the execution environment if possible.</span></span> <span data-ttu-id="2ecdd-250">Puede invalidar automáticamente recoger información de conexión al proporcionar una cadena de conexión.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-250">You can override automatically pick up of connection information by providing a connection string.</span></span>

* <span data-ttu-id="2ecdd-251">`vaultName` -Es necesario si `uri` en no proporciona.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-251">`vaultName` - Required if `uri` in not provided.</span></span> <span data-ttu-id="2ecdd-252">Especifica el nombre del almacén en su suscripción de Azure desde el que se va a leer los pares clave/valor.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-252">Specifies the name of the vault in your Azure subscription from which to read key/value pairs.</span></span>
* <span data-ttu-id="2ecdd-253">`connectionString` -Una cadena de conexión que se pueda usar [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span><span class="sxs-lookup"><span data-stu-id="2ecdd-253">`connectionString` - A connection string usable by [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)</span></span>
* <span data-ttu-id="2ecdd-254">`uri` -Se conecta a otros proveedores de almacén de claves con los valores especificados `uri` valor.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-254">`uri` - Connects to other Key Vault providers with the specified `uri` value.</span></span> <span data-ttu-id="2ecdd-255">Si no se especifica, Azure (`vaultName`) es el proveedor de almacén.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-255">If not specified, Azure (`vaultName`) is the vault provider.</span></span>
* <span data-ttu-id="2ecdd-256">`version` -Azure Key Vault proporciona una característica de control de versiones para los secretos.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-256">`version` - Azure Key Vault provides a versioning feature for secrets.</span></span> <span data-ttu-id="2ecdd-257">Si `version` se especifica, el generador solo recupera los secretos de coincidencia de esta versión.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-257">If `version` is specified, the builder only retrieves secrets matching this version.</span></span>
* <span data-ttu-id="2ecdd-258">`preloadSecretNames` : De forma predeterminada, este generador de consultas **todas** los nombres en el almacén de claves de clave cuando se inicializa.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-258">`preloadSecretNames` - By default, this builder querys **all** key names in the key vault when it is initialized.</span></span> <span data-ttu-id="2ecdd-259">Para evitar la lectura de todos los valores de clave, establezca este atributo en `false`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-259">To prevent reading all key values, set this attribute to `false`.</span></span> <span data-ttu-id="2ecdd-260">Si se establece en `false` lee secretos uno cada vez.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-260">Setting this to `false` reads secrets one at a time.</span></span> <span data-ttu-id="2ecdd-261">Una vez puede resultar útil si el almacén permite el acceso "Get" pero no "lista" acceso de lectura.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-261">Reading secrets one at a time can useful if the vault allows "Get" access but not "List" access.</span></span> <span data-ttu-id="2ecdd-262">**Nota:** al usar `Greedy` modo, `preloadSecretNames` debe ser `true` (el predeterminado).</span><span class="sxs-lookup"><span data-stu-id="2ecdd-262">**Note:** When using `Greedy` mode, `preloadSecretNames` must be `true` (the default.)</span></span>

### <a name="keyperfileconfigbuilder"></a><span data-ttu-id="2ecdd-263">KeyPerFileConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="2ecdd-263">KeyPerFileConfigBuilder</span></span>

```xml
<add name="KeyPerFile"
    [mode|prefix|stripPrefix|tokenPattern]
    (directoryPath="PathToSourceDirectory")
    [ignorePrefix="ignore."]
    [keyDelimiter=":"]
    [optional="false"]
    type="Microsoft.Configuration.ConfigurationBuilders.KeyPerFileConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.KeyPerFile" />
```

<span data-ttu-id="2ecdd-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) es un generador de configuración básica que utiliza archivos de un directorio como un origen de valores.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-264">[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) is a basic configuration builder that uses a directory's files as a source of values.</span></span> <span data-ttu-id="2ecdd-265">Nombre de un archivo es la clave y el contenido es el valor.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-265">A file's name is the key, and the contents are the value.</span></span> <span data-ttu-id="2ecdd-266">Este generador de configuración puede ser útil cuando se ejecuta en un entorno de contenedor organizada.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-266">This configuration builder can be useful when running in an orchestrated container environment.</span></span> <span data-ttu-id="2ecdd-267">Los sistemas como Docker Swarm y Kubernetes proporcionan `secrets` a sus contenedores de windows organizada de esta manera clave por archivo.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-267">Systems like Docker Swarm and Kubernetes provide `secrets` to their orchestrated windows containers in this key-per-file manner.</span></span>

<span data-ttu-id="2ecdd-268">Detalles de atributos:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-268">Attribute details:</span></span>

* <span data-ttu-id="2ecdd-269">`directoryPath` - Requerido.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-269">`directoryPath` - Required.</span></span> <span data-ttu-id="2ecdd-270">Especifica una ruta de acceso para buscar los valores.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-270">Specifies a path to look in for values.</span></span> <span data-ttu-id="2ecdd-271">Docker para Windows se almacenan los secretos en el *C:\ProgramData\Docker\secrets* directorio de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-271">Docker for Windows secrets are stored in the *C:\ProgramData\Docker\secrets* directory by default.</span></span>
* <span data-ttu-id="2ecdd-272">`ignorePrefix` -Archivos que comienzan por este prefijo se excluyen.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-272">`ignorePrefix` - Files that start with this prefix are excluded.</span></span> <span data-ttu-id="2ecdd-273">El valor predeterminado es "ignore".".</span><span class="sxs-lookup"><span data-stu-id="2ecdd-273">Defaults to "ignore.".</span></span>
* <span data-ttu-id="2ecdd-274">`keyDelimiter` -El valor predeterminado es `null`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-274">`keyDelimiter` - Default value is `null`.</span></span> <span data-ttu-id="2ecdd-275">Si se especifica, el generador de configuración atraviesa varios niveles del directorio, la creación de nombres de clave con este delimitador.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-275">If specified, the configuration builder traverses multiple levels of the directory, building up key names with this delimiter.</span></span> <span data-ttu-id="2ecdd-276">Si este valor es `null`, el generador de configuración solo se busca en el nivel superior del directorio.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-276">If this value is `null`, the configuration builder only looks at the top level of the directory.</span></span>
* <span data-ttu-id="2ecdd-277">`optional` -El valor predeterminado es `false`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-277">`optional` -  Default value is `false`.</span></span> <span data-ttu-id="2ecdd-278">Especifica si el generador de configuración debería producir errores si no existe el directorio de origen.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-278">Specifies whether the configuration builder should cause errors if the source directory doesn't exist.</span></span>

### <a name="simplejsonconfigbuilder"></a><span data-ttu-id="2ecdd-279">SimpleJsonConfigBuilder</span><span class="sxs-lookup"><span data-stu-id="2ecdd-279">SimpleJsonConfigBuilder</span></span>

> [!WARNING]
> <span data-ttu-id="2ecdd-280">Nunca almacenar contraseñas, cadenas de conexión confidenciales u otros datos confidenciales en el código fuente.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-280">Never store passwords, sensitive connection strings, or other sensitive data in source code.</span></span> <span data-ttu-id="2ecdd-281">Secretos de producción no deben usarse para el desarrollo o prueba.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-281">Production secrets should not be used for development or test.</span></span>

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

<span data-ttu-id="2ecdd-282">Proyectos de .NET core con frecuencia emplean archivos JSON para la configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-282">.NET Core projects frequently use JSON files for configuration.</span></span> <span data-ttu-id="2ecdd-283">El [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) generador permite que los archivos de .NET Core JSON que se usará en .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-283">The [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) builder allows .NET Core JSON files to be used in the .NET Framework.</span></span> <span data-ttu-id="2ecdd-284">Este generador de configuración es que proporciona una asignación básica de un origen de clave/valor sin formato en áreas clave y valor específico de configuración de .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-284">This configuration builder is provides a basic mapping from a flat key/value source into specific key/value areas of .NET Framework configuration.</span></span> <span data-ttu-id="2ecdd-285">Este generador de configuración **no** proporcionar configuraciones jerárquica.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-285">This configuration builder does **not** provide for hierarchical configurations.</span></span> <span data-ttu-id="2ecdd-286">El archivo de copia de seguridad de JSON es similar a un diccionario, no un objeto complejo jerárquico.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-286">The JSON backing file is similar to a dictionary, not a complex hierarchical object.</span></span> <span data-ttu-id="2ecdd-287">Puede usarse un archivo jerárquico de varios nivel.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-287">A multi-level hierarchical file can be used.</span></span> <span data-ttu-id="2ecdd-288">Este proveedor `flatten`s la profundidad anexando el nombre de propiedad en cada nivel con `:` como delimitador.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-288">This provider `flatten`s the depth by appending the property name at each level using `:` as a delimiter.</span></span>

<span data-ttu-id="2ecdd-289">Detalles de atributos:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-289">Attribute details:</span></span>

* <span data-ttu-id="2ecdd-290">`jsonFile` - Requerido.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-290">`jsonFile` - Required.</span></span> <span data-ttu-id="2ecdd-291">Especifica el archivo JSON para leer.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-291">Specifies the JSON file to read from.</span></span> <span data-ttu-id="2ecdd-292">El `~` carácter puede usarse al principio para hacer referencia a la raíz de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-292">The `~` character can be used at the start to reference the app root.</span></span>
* <span data-ttu-id="2ecdd-293">`optional` -Booleano, valor predeterminado es `true`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-293">`optional` - Boolean,  default value is `true`.</span></span> <span data-ttu-id="2ecdd-294">Evita producir excepciones si no se encuentra el archivo JSON.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-294">Prevents throwing exceptions if the JSON file cannot be found.</span></span>
* <span data-ttu-id="2ecdd-295">`jsonMode` - `[Flat|Sectional]`.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-295">`jsonMode` - `[Flat|Sectional]`.</span></span> <span data-ttu-id="2ecdd-296">`Flat` es el valor predeterminado.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-296">`Flat` is the default.</span></span> <span data-ttu-id="2ecdd-297">Cuando `jsonMode` es `Flat`, el archivo JSON es un origen único clave-valor sin formato.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-297">When `jsonMode` is `Flat`, the JSON file is a single flat key/value source.</span></span> <span data-ttu-id="2ecdd-298">El `EnvironmentConfigBuilder` y `AzureKeyVaultConfigBuilder` también son orígenes de clave-valor sin formato único.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-298">The `EnvironmentConfigBuilder` and `AzureKeyVaultConfigBuilder` are also single flat key/value sources.</span></span> <span data-ttu-id="2ecdd-299">Cuando el `SimpleJsonConfigBuilder` está configurado en `Sectional` modo:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-299">When the `SimpleJsonConfigBuilder` is configured in `Sectional` mode:</span></span>

  * <span data-ttu-id="2ecdd-300">El archivo JSON conceptualmente se divide simplemente en el nivel superior en varios diccionarios.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-300">The JSON file is conceptually divided just at the top level into multiple dictionaries.</span></span>
  * <span data-ttu-id="2ecdd-301">Cada uno de los diccionarios solo se aplica a la sección de configuración que coincida con el nombre de propiedad de nivel superior conectado a ellas.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-301">Each of the dictionaries is only applied to the configuration section that matches the top-level property name attached to them.</span></span> <span data-ttu-id="2ecdd-302">Por ejemplo:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-302">For example:</span></span>

```json
    {
        "appSettings" : {
            "setting1" : "value1",
            "setting2" : "value2",
            "complex" : {
                "setting1" : "complex:value1",
                "setting2" : "complex:value2",
            }
        }
    }
```

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a><span data-ttu-id="2ecdd-303">Implementación de un generador de configuración de clave/valor personalizado</span><span class="sxs-lookup"><span data-stu-id="2ecdd-303">Implementing a custom key/value configuration builder</span></span>

<span data-ttu-id="2ecdd-304">Si los generadores de configuración no satisfacen sus necesidades, puede escribir uno personalizado.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-304">If the configuration builders don't meet your needs, you can write a custom one.</span></span> <span data-ttu-id="2ecdd-305">La `KeyValueConfigBuilder` clase base que controla la mayoría de problemas de prefijo y modos de sustitución.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-305">The `KeyValueConfigBuilder` base class handles substitution modes and most prefix concerns.</span></span> <span data-ttu-id="2ecdd-306">Sólo necesitan un proyecto de implementación:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-306">An implementing project need only:</span></span>

* <span data-ttu-id="2ecdd-307">Heredar de la clase base e implementar una fuente básica de pares clave/valor a través de la `GetValue` y `GetAllValues`:</span><span class="sxs-lookup"><span data-stu-id="2ecdd-307">Inherit from the base class and implement a basic source of key/value pairs through the `GetValue` and `GetAllValues`:</span></span>
* <span data-ttu-id="2ecdd-308">Agregar el [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) al proyecto.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-308">Add the [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) to the project.</span></span>

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

<span data-ttu-id="2ecdd-309">La `KeyValueConfigBuilder` clase base proporciona gran parte del comportamiento coherente y profesional a través de clave/valor generadores de configuración.</span><span class="sxs-lookup"><span data-stu-id="2ecdd-309">The `KeyValueConfigBuilder` base class provides much of the work and consistent behavior across key/value configuration builders.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2ecdd-310">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="2ecdd-310">Additional resources</span></span>

* [<span data-ttu-id="2ecdd-311">Repositorio de GitHub de los generadores de configuración</span><span class="sxs-lookup"><span data-stu-id="2ecdd-311">Configuration Builders GitHub repository</span></span>](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [<span data-ttu-id="2ecdd-312">Autenticación de servicio a servicio en Azure Key Vault mediante .NET</span><span class="sxs-lookup"><span data-stu-id="2ecdd-312">Service-to-service authentication to Azure Key Vault using .NET</span></span>](/azure/key-vault/service-to-service-authentication#connection-string-support)
