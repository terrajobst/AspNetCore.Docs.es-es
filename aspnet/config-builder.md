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
# <a name="configuration-builders-for-aspnet"></a>Generadores de configuración de ASP.NET

Por [Stephen Molloy](https://github.com/StephenMolloy) y [Rick Anderson](https://twitter.com/RickAndMSFT)

Los generadores de configuración proporcionan un mecanismo modernas y ágil para las aplicaciones ASP.NET obtener los valores de configuración de orígenes externos.

Generadores de configuración:

* Están disponibles en .NET Framework 4.7.1 y versiones posteriores.
* Proporcionan un mecanismo flexible para leer los valores de configuración.
* Solucionar algunas de las necesidades básicas de las aplicaciones cuando se mueven en un contenedor y un entorno centrado en nube.
* Puede utilizarse para mejorar la protección de datos de configuración mediante el uso de orígenes que no estaban disponibles (por ejemplo, Azure Key Vault y el entorno de variables) en el sistema de configuración. NET.

## <a name="keyvalue-configuration-builders"></a>Generadores de configuración de clave/valor

Un escenario común que puede controlarse mediante generadores de configuración es proporcionar un mecanismo de reemplazo básico de pares clave-valor para secciones de configuración que siguen un patrón de clave/valor. El concepto de .NET Framework de ConfigurationBuilders no se limita a secciones de configuración específico o patrones. Sin embargo, muchos de los generadores de configuración en `Microsoft.Configuration.ConfigurationBuilders` ([github](https://github.com/aspnet/MicrosoftConfigurationBuilders), [NuGet](https://www.nuget.org/packages?q=Microsoft.Configuration.ConfigurationBuilders)) funcionan dentro del patrón de clave/valor.

## <a name="keyvalue-configuration-builders-settings"></a>Valores de los generadores de configuración de clave/valor

Las siguientes opciones se aplican a todos los generadores de configuración de pares clave-valor en `Microsoft.Configuration.ConfigurationBuilders`.

### <a name="mode"></a>Modo

Los generadores de configuración usar un origen externo de la información de clave/valor para rellenar los elementos de clave/valor seleccionado del sistema de configuración. En concreto, el `<appSettings/>` y `<connectionStrings/>` secciones reciben un tratamiento especial de los generadores de configuración. Los generadores de trabajo en tres modos:

* `Strict` -El modo predeterminado. En este modo, el generador de configuración solo funciona en las secciones de configuración de clave/valor-centric conocido. `Strict` modo enumera cada clave en la sección. Si se encuentra una clave coincidente en el origen externo:

   * Los generadores de configuración reemplaza el valor en la sección de configuración resultante con el valor del origen externo.
* `Greedy` -En este modo está estrechamente relacionada con `Strict` modo. En lugar de limitarse a las claves que ya existen en la configuración original:

  * Los generadores de configuración agrega todos los pares clave/valor desde el origen externo en la sección de configuración resultante.

* `Expand` -Se opera en el XML sin formato antes de se analiza en un objeto de sección de configuración. Se puede considerar como una expansión de tokens de una cadena. Cualquier parte de la cadena XML sin formato que coincida con el patrón `${token}` es un candidato para la expansión del token. Si se encuentra ningún valor correspondiente en el origen externo, no se cambia el token. No se limitan a los generadores de este modo el `<appSettings/>` y `<connectionStrings/>` secciones.

El siguiente marcado de *web.config* permite la [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/) en `Strict` modo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebDefault.config?name=snippet)]

El siguiente código lee la `<appSettings/>` y `<connectionStrings/>` se muestra en la anterior *web.config* archivo:

[!code-csharp[Main](config-builder/MyConfigBuilders/About.aspx.cs)]

El código anterior establecerá los valores de propiedad:

* Los valores de la *web.config* archivo si las claves no se establecen en variables de entorno.
* Los valores del entorno de variable, si establece.

Por ejemplo, `ServiceID` contendrá:

* "Valor ServiceID del archivo web.config", si la variable de entorno `ServiceID` no está establecida.
* El valor de la `ServiceID` if de variable de entorno establecido.

La siguiente imagen muestra la `<appSettings/>` claves/valores de los anteriores *web.config* conjunto de archivos en el editor de entorno:

![editor del entorno](config-builder/static/env.png)

Nota: Es posible que deba salir y reiniciar Visual Studio para ver los cambios en las variables de entorno.

### <a name="prefix-handling"></a>Control de prefijo

Los prefijos de la claves pueden simplificar las claves de configuración porque:

* La configuración de .NET Framework es compleja y anidada.
* Orígenes externos de clave/valor normalmente son básicas y sin formato por naturaleza. Por ejemplo, las variables de entorno no están anidadas.

Usar cualquiera de los siguientes métodos para insertar ambos `<appSettings/>` y `<connectionStrings/>` en la configuración a través de las variables de entorno:

* Con el `EnvironmentConfigBuilder` en el valor predeterminado `Strict` modo y los nombres de clave adecuados en el archivo de configuración. El código y el marcado anterior adopta este enfoque. Con este enfoque puede **no** han con el mismo nombre tanto en las claves de `<appSettings/>` y `<connectionStrings/>`.
* Use dos `EnvironmentConfigBuilder`s en `Greedy` modo con prefijos diferentes y `stripPrefix`. Con este enfoque, la aplicación puede leer `<appSettings/>` y `<connectionStrings/>` sin necesidad de actualizar el archivo de configuración. La siguiente sección, [stripPrefix](#stripprefix), se muestra cómo hacerlo.
* Use dos `EnvironmentConfigBuilder`s en `Greedy` modo con prefijos diferentes. Con este enfoque no puede tener nombres duplicados de clave como nombres de clave deben diferenciarse por el prefijo.  Por ejemplo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefix.config?name=snippet&highlight=11-99)]

Con el marcado anterior, puede utilizarse el mismo origen de clave/valor sin formato para rellenar la configuración de dos secciones diferentes.

La siguiente imagen muestra la `<appSettings/>` y `<connectionStrings/>` claves/valores de los anteriores *web.config* conjunto de archivos en el editor de entorno:

![editor del entorno](config-builder/static/prefix.png)

El siguiente código lee la `<appSettings/>` y `<connectionStrings/>` claves/valores contenidos en los anteriores *web.config* archivo:

[!code-csharp[Main](config-builder/MyConfigBuilders/Contact.aspx.cs?name=snippet)]

El código anterior establecerá los valores de propiedad:

* Los valores de la *web.config* archivo si las claves no se establecen en variables de entorno.
* Los valores del entorno de variable, si establece.

Por ejemplo, con el anterior *web.config* archivo, se establecen los valores de claves o en la imagen anterior de editor de entorno y el código anterior, los valores siguientes:

|  Key              | Valor |
| ----------------- | ------------ |
|     AppSetting_ServiceID           | AppSetting_ServiceID desde variables de entorno|
|    AppSetting_default            | Valor AppSetting_default desde env |
|       ConnStr_default         | Val ConnStr_default desde env|

### <a name="stripprefix"></a>stripPrefix

`stripPrefix`: un valor booleano, el valor predeterminado es `false`. 

El marcado XML anterior separa la configuración de la aplicación de las cadenas de conexión, pero requiere que todas las claves de la *web.config* archivo que se usará el prefijo especificado. Por ejemplo, el prefijo `AppSetting` deben agregarse a la `ServiceID` clave ("AppSetting_ServiceID"). Con `stripPrefix`, no se utiliza el prefijo en el *web.config* archivo. Se requiere el prefijo en el origen del generador de configuración (por ejemplo, en el entorno). Se prevé que la mayoría de los desarrolladores usarán `stripPrefix`.

Las aplicaciones normalmente eliminan el prefijo. La siguiente *web.config* elimina el prefijo:

[!code-xml[Main](config-builder/MyConfigBuilders/WebPrefixStrip.config?name=snippet&highlight=14,19)]

En la anterior *web.config* archivo, el `default` clave pertenece a ambos el `<appSettings/>` y `<connectionStrings/>`.

La siguiente imagen muestra la `<appSettings/>` y `<connectionStrings/>` claves/valores de los anteriores *web.config* conjunto de archivos en el editor de entorno:

![editor del entorno](config-builder/static/prefix.png)

El siguiente código lee la `<appSettings/>` y `<connectionStrings/>` claves/valores contenidos en los anteriores *web.config* archivo:

[!code-csharp[Main](config-builder/MyConfigBuilders/About2.aspx.cs?name=snippet)]

El código anterior establecerá los valores de propiedad:

* Los valores de la *web.config* archivo si las claves no se establecen en variables de entorno.
* Los valores del entorno de variable, si establece.

Por ejemplo, con el anterior *web.config* archivo, se establecen los valores de claves o en la imagen anterior de editor de entorno y el código anterior, los valores siguientes:

|  Key              | Valor |
| ----------------- | ------------ |
|     ServiceID           | AppSetting_ServiceID desde variables de entorno|
|    default            | Valor AppSetting_default desde env |
|    default         | Val ConnStr_default desde env|

### <a name="tokenpattern"></a>tokenPattern

`tokenPattern`: Cadena, el valor predeterminado es `@"\$\{(\w+)\}"`

El `Expand` comportamiento de los generadores de busca en el XML sin formato para los tokens que se parecen a `${token}`. Búsqueda se realiza con la expresión regular predeterminada `@"\$\{(\w+)\}"`. El juego de caracteres que coincide con `\w` es más estricta que permiten XML y muchos orígenes de configuración. Use `tokenPattern` cuando más caracteres que `@"\$\{(\w+)\}"` son necesarios en el nombre del token.

`tokenPattern`: Cadena:

* Permite a los desarrolladores cambiar la expresión regular que se usa para la coincidencia de token.
* Se realiza ninguna validación para asegurarse de que es un regex no peligrosas, tiene el formato correcto.
* Debe contener un grupo de capturas. La expresión regular completa debe coincidir con todo el token. La primera captura debe ser el nombre del token para buscar en el origen de configuración.

## <a name="configuration-builders-in-microsoftconfigurationconfigurationbuilders"></a>Generadores de configuración en Microsoft.Configuration.ConfigurationBuilders

### <a name="environmentconfigbuilder"></a>EnvironmentConfigBuilder

```xml
<add name="Environment"
    [mode|prefix|stripPrefix|tokenPattern] 
    type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Environment" />
```

El [EnvironmentConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Environment/):

* Es el más sencillo de los generadores de configuración.
* Lee los valores del entorno.
* No tiene ninguna opción de configuración adicionales.
* El `name` valor del atributo es arbitrario.

**Nota:** en un entorno de contenedor de Windows, las variables establecidas en tiempo de ejecución solo se insertan en el entorno de proceso de punto de entrada. Las aplicaciones que se ejecutan como un servicio o un proceso que no sean EntryPoint no recopilan estas variables, a menos que en caso contrario, se insertan mediante un mecanismo en el contenedor. Para [IIS](https://github.com/Microsoft/iis-docker/pull/41)/[ASP.NET](https://github.com/Microsoft/aspnet-docker)-basado en contenedores, la versión actual de [ServiceMonitor.exe](https://github.com/Microsoft/iis-docker/pull/41) se encarga de ello en la *DefaultAppPool* solo. Otras variantes de contenedor basadas en Windows que deba desarrollar su propio mecanismo de inserción para los procesos que no sean EntryPoint.

### <a name="usersecretsconfigbuilder"></a>UserSecretsConfigBuilder

> [!WARNING]
> Nunca almacenar contraseñas, cadenas de conexión confidenciales u otros datos confidenciales en el código fuente. Secretos de producción no deben usarse para el desarrollo o prueba.

```xml
<add name="UserSecrets"
    [mode|prefix|stripPrefix|tokenPattern]
    (userSecretsId="{secret string, typically a GUID}" | userSecretsFile="~\secrets.file")
    [optional="true"]
    type="Microsoft.Configuration.ConfigurationBuilders.UserSecretsConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.UserSecrets" />
```

Este generador de configuración proporciona una característica similar a [ASP.NET Core Secret Manager](/aspnet/core/security/app-secrets).

El [UserSecretsConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.UserSecrets/) se puede usar en proyectos de .NET Framework, pero se debe especificar un archivo de secretos. Como alternativa, puede definir el `UserSecretsId` propiedad en el proyecto de archivo y crea el archivo de secretos sin procesar en la ubicación adecuada para su lectura. Para mantener las dependencias externas fuera de su proyecto, el archivo del secreto es con formato XML. El formato XML es un detalle de implementación y el formato no se debería confiar en. Si necesita compartir un *secrets.json* archivo con proyectos de .NET Core, considere el uso de la [SimpleJsonConfigBuilder](#simplejsonconfig). El `SimpleJsonConfigBuilder` para .NET Core también debe considerarse un detalle de implementación sujeta a cambios de formato.

Configuración de atributos de `UserSecretsConfigBuilder`:

* `userSecretsId` -Éste es el método preferido para identificar un archivo de secretos XML. Funciona de manera similar a .NET Core, que usa un `UserSecretsId` propiedad para almacenar este identificador de proyecto. La cadena debe ser única, que no necesita ser un GUID. Con este atributo, el `UserSecretsConfigBuilder` buscar en una ubicación local conocida (`%APPDATA%\Microsoft\UserSecrets\<UserSecrets Id>\secrets.xml`) para un archivo de secretos que pertenecen a este identificador.
* `userSecretsFile` -Un atributo opcional que especifica el archivo que contiene los secretos. El `~` carácter puede usarse al principio para hacer referencia a la raíz de la aplicación. Este atributo o la `userSecretsId` atributo es necesario. Si se especifican ambos, `userSecretsFile` tiene prioridad.
* `optional`: valor booleano, valor predeterminado `true` -impide que una excepción si no se encuentra el archivo de secretos. 
* El `name` valor del atributo es arbitrario.

El archivo de secretos tiene el formato siguiente:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<root>
  <secrets ver="1.0">
    <secret name="secret key name" value="secret value" />
  </secrets>
</root>
```

### <a name="azurekeyvaultconfigbuilder"></a>AzureKeyVaultConfigBuilder

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

El [AzureKeyVaultConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Azure/) lee los valores almacenados en el [Azure Key Vault](/azure/key-vault/key-vault-whatis).

`vaultName` es necesario (el nombre del almacén) o un URI en el almacén. Permitir que el control sobre qué almacén para conectarse a los otros atributos, pero son necesarios si la aplicación no se está ejecutando en un entorno que funciona con `Microsoft.Azure.Services.AppAuthentication`. La biblioteca de autenticación de servicios de Azure se usa para seleccionar automáticamente la información de conexión desde el entorno de ejecución, si es posible. Puede invalidar automáticamente recoger información de conexión al proporcionar una cadena de conexión.

* `vaultName` -Es necesario si `uri` en no proporciona. Especifica el nombre del almacén en su suscripción de Azure desde el que se va a leer los pares clave/valor.
* `connectionString` -Una cadena de conexión que se pueda usar [AzureServiceTokenProvider](https://docs.microsoft.com/en-us/azure/key-vault/service-to-service-authentication#connection-string-support)
* `uri` -Se conecta a otros proveedores de almacén de claves con los valores especificados `uri` valor. Si no se especifica, Azure (`vaultName`) es el proveedor de almacén.
* `version` -Azure Key Vault proporciona una característica de control de versiones para los secretos. Si `version` se especifica, el generador solo recupera los secretos de coincidencia de esta versión.
* `preloadSecretNames` : De forma predeterminada, este generador de consultas **todas** los nombres en el almacén de claves de clave cuando se inicializa. Para evitar la lectura de todos los valores de clave, establezca este atributo en `false`. Si se establece en `false` lee secretos uno cada vez. Una vez puede resultar útil si el almacén permite el acceso "Get" pero no "lista" acceso de lectura. **Nota:** al usar `Greedy` modo, `preloadSecretNames` debe ser `true` (el predeterminado).

### <a name="keyperfileconfigbuilder"></a>KeyPerFileConfigBuilder

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

[KeyPerFileConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.KeyPerFile/) es un generador de configuración básica que utiliza archivos de un directorio como un origen de valores. Nombre de un archivo es la clave y el contenido es el valor. Este generador de configuración puede ser útil cuando se ejecuta en un entorno de contenedor organizada. Los sistemas como Docker Swarm y Kubernetes proporcionan `secrets` a sus contenedores de windows organizada de esta manera clave por archivo.

Detalles de atributos:

* `directoryPath` - Requerido. Especifica una ruta de acceso para buscar los valores. Docker para Windows se almacenan los secretos en el *C:\ProgramData\Docker\secrets* directorio de forma predeterminada.
* `ignorePrefix` -Archivos que comienzan por este prefijo se excluyen. El valor predeterminado es "ignore".".
* `keyDelimiter` -El valor predeterminado es `null`. Si se especifica, el generador de configuración atraviesa varios niveles del directorio, la creación de nombres de clave con este delimitador. Si este valor es `null`, el generador de configuración solo se busca en el nivel superior del directorio.
* `optional` -El valor predeterminado es `false`. Especifica si el generador de configuración debería producir errores si no existe el directorio de origen.

### <a name="simplejsonconfigbuilder"></a>SimpleJsonConfigBuilder

> [!WARNING]
> Nunca almacenar contraseñas, cadenas de conexión confidenciales u otros datos confidenciales en el código fuente. Secretos de producción no deben usarse para el desarrollo o prueba.

```xml
<add name="SimpleJson"
    [mode|prefix|stripPrefix|tokenPattern]
    jsonFile="~\config.json"
    [optional="true"]
    [jsonMode="(Flat|Sectional)"]
    type="Microsoft.Configuration.ConfigurationBuilders.SimpleJsonConfigBuilder,
    Microsoft.Configuration.ConfigurationBuilders.Json" />
```

Proyectos de .NET core con frecuencia emplean archivos JSON para la configuración. El [SimpleJsonConfigBuilder](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Json/) generador permite que los archivos de .NET Core JSON que se usará en .NET Framework. Este generador de configuración es que proporciona una asignación básica de un origen de clave/valor sin formato en áreas clave y valor específico de configuración de .NET Framework. Este generador de configuración **no** proporcionar configuraciones jerárquica. El archivo de copia de seguridad de JSON es similar a un diccionario, no un objeto complejo jerárquico. Puede usarse un archivo jerárquico de varios nivel. Este proveedor `flatten`s la profundidad anexando el nombre de propiedad en cada nivel con `:` como delimitador.

Detalles de atributos:

* `jsonFile` - Requerido. Especifica el archivo JSON para leer. El `~` carácter puede usarse al principio para hacer referencia a la raíz de la aplicación.
* `optional` -Booleano, valor predeterminado es `true`. Evita producir excepciones si no se encuentra el archivo JSON.
* `jsonMode` - `[Flat|Sectional]`. `Flat` es el valor predeterminado. Cuando `jsonMode` es `Flat`, el archivo JSON es un origen único clave-valor sin formato. El `EnvironmentConfigBuilder` y `AzureKeyVaultConfigBuilder` también son orígenes de clave-valor sin formato único. Cuando el `SimpleJsonConfigBuilder` está configurado en `Sectional` modo:

  * El archivo JSON conceptualmente se divide simplemente en el nivel superior en varios diccionarios.
  * Cada uno de los diccionarios solo se aplica a la sección de configuración que coincida con el nombre de propiedad de nivel superior conectado a ellas. Por ejemplo:

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

## <a name="implementing-a-custom-keyvalue-configuration-builder"></a>Implementación de un generador de configuración de clave/valor personalizado

Si los generadores de configuración no satisfacen sus necesidades, puede escribir uno personalizado. La `KeyValueConfigBuilder` clase base que controla la mayoría de problemas de prefijo y modos de sustitución. Sólo necesitan un proyecto de implementación:

* Heredar de la clase base e implementar una fuente básica de pares clave/valor a través de la `GetValue` y `GetAllValues`:
* Agregar el [Microsoft.Configuration.ConfigurationBuilders.Base](https://www.nuget.org/packages/Microsoft.Configuration.ConfigurationBuilders.Base/) al proyecto.

[!code-csharp[Main](config-builder/MyConfigBuilders/MyCustomConfigBuilder.cs)]

La `KeyValueConfigBuilder` clase base proporciona gran parte del comportamiento coherente y profesional a través de clave/valor generadores de configuración.

## <a name="additional-resources"></a>Recursos adicionales

* [Repositorio de GitHub de los generadores de configuración](https://github.com/aspnet/MicrosoftConfigurationBuilders)
* [Autenticación de servicio a servicio en Azure Key Vault mediante .NET](/azure/key-vault/service-to-service-authentication#connection-string-support)
