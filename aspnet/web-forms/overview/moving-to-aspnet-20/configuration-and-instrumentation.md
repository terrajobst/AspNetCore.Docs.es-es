---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: "Configuración e instrumentación | Documentos de Microsoft"
author: microsoft
description: "Hay cambios importantes en la configuración y la instrumentación en ASP.NET 2.0. La nueva API de configuración de ASP.NET permite cambios de configuración que se realizan pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: f8d378d3332669ae4606dad8ada06de37e7dfd20
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="configuration-and-instrumentation"></a>Configuración e instrumentación
====================
por [Microsoft](https://github.com/microsoft)

> Hay cambios importantes en la configuración y la instrumentación en ASP.NET 2.0. La nueva API de configuración de ASP.NET permite cambios de configuración que se realizan mediante programación. Además, existen muchas nuevas opciones de configuración Permitir nuevas configuraciones e instrumentación de.


Hay cambios importantes en la configuración y la instrumentación en ASP.NET 2.0. La nueva API de configuración de ASP.NET permite cambios de configuración que se realizan mediante programación. Además, existen muchas nuevas opciones de configuración Permitir nuevas configuraciones e instrumentación de.

En este módulo, trataremos la API de configuración de ASP.NET como lo referente a leer y escribir en archivos de configuración de ASP.NET, y también se explicará instrumentación de ASP.NET. También se explica las nuevas características disponibles en el seguimiento de ASP.NET.

## <a name="aspnet-configuration-api"></a>API de configuración de ASP.NET

La API de configuración de ASP.NET permite desarrollar, implementar y administrar datos de configuración de aplicación mediante una única interfaz de programación. Puede utilizar la API de configuración para desarrollar y modificar mediante programación configuraciones de ASP.NET completas sin editar directamente el XML en los archivos de configuración. Además, puede usar la API de configuración en aplicaciones de consola y scripts que desarrolle, en herramientas de administración basada en Web y en complementos de Microsoft Management Console (MMC).

Las dos herramientas de administración de configuración siguientes utilizan la API de configuración y se incluyen con la versión 2.0 de .NET Framework:

- El complemento MMC de ASP.NET, que usa la API de configuración para simplificar las tareas administrativas, lo que proporciona una vista integrada de datos de configuración local de todos los niveles de la jerarquía de configuración.
- La herramienta de administración de sitios Web, que le permite administrar la configuración para aplicaciones locales y remotas, incluso los sitios hospedados.

La API de configuración de ASP.NET consta de un conjunto de objetos de administración de ASP.NET que puede usar para configurar sitios Web y aplicaciones mediante programación. Objetos de administración se implementan como una biblioteca de clases de .NET Framework. El modelo de programación de la API de configuración ayuda a garantizar la coherencia del código y confiabilidad mediante la aplicación de los tipos de datos en tiempo de compilación. Para que sea más fácil de administrar las configuraciones de aplicaciones, la API de configuración permite ver los datos que se combinan desde todos los puntos de la jerarquía de configuración como una sola colección, en lugar de ver los datos como colecciones independientes de diferentes archivos de configuración. Además, la API de configuración permite manipular las configuraciones de toda la aplicación sin editar directamente el XML en los archivos de configuración. Por último, la API simplifica las tareas de configuración al admitir herramientas administrativas, como la herramienta de administración de sitios Web. La API de configuración simplifica la implementación por lo que permite la creación de archivos de configuración en un equipo y ejecutar secuencias de comandos de configuración en varios equipos.

> [!NOTE]
> La API de configuración no es compatible con la creación de aplicaciones de IIS.


## <a name="working-with-local-and-remote-configuration-settings"></a>Trabajar con valores de configuración locales y remotos

Un objeto de configuración representa la vista combinada de las opciones de configuración que se aplican a una entidad física concreta, como un equipo, o a una entidad lógica, como una aplicación o un sitio Web. La entidad lógica especificada puede existir en el equipo local o en un servidor remoto. Cuando no existe ningún archivo de configuración para una entidad especificada, el objeto de configuración representa la configuración predeterminada de acuerdo con el archivo Machine.config.

Puede obtener un objeto de configuración mediante uno de los métodos de configuración abierto de las siguientes clases:

1. La clase ConfigurationManager, si la entidad es una aplicación cliente.
2. La clase WebConfigurationManager, si la entidad es una aplicación Web.

Estos métodos devolverán un objeto de configuración, que a su vez proporciona los métodos y propiedades necesarios para controlar los archivos de configuración subyacentes. Puede tener acceso a estos archivos para leer o escribir.

### <a name="reading"></a>Lectura

Utilice el método GetSection o GetSectionGroup para leer información de configuración. El usuario o proceso que lee debe tener permisos de lectura en todos los archivos de configuración de la jerarquía.

> [!NOTE]
> Si utiliza un método GetSection estático que toma un parámetro de ruta de acceso, el parámetro de ruta de acceso debe hacer referencia a la aplicación en el que se ejecuta el código. En caso contrario, se omite el parámetro y se devuelve la información de configuración de la aplicación que se está ejecutando.


### <a name="writing"></a>Escritura

Uno de los métodos de guardar se utiliza para escribir información de configuración. El usuario o proceso que escribe debe tener permisos de escritura en el archivo de configuración y el directorio en el nivel de jerarquía de configuración actual, así como permisos de lectura en todos los archivos de configuración de la jerarquía.

Para generar un archivo de configuración que representa los valores de configuración heredados para una entidad especificada, use uno de los métodos de guardar la configuración siguiente:

1. El método Save para crear un nuevo archivo de configuración.
2. El método SaveAs para generar un nuevo archivo de configuración en otra ubicación.

## <a name="configuration-classes-and-namespaces"></a>Espacios de nombres y clases de configuración

Muchas clases de configuración y los métodos son similares entre sí. La tabla siguiente describen las clases de configuración utilizadas con más frecuencia y los espacios de nombres.

| **Espacio de nombres o clase de configuración** | **Descripción** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/en-us/library/system.configuration.aspx) espacio de nombres | Contiene las clases de configuración principales para todas las aplicaciones de .NET Framework. Las clases de controlador de sección sirven para obtener datos de configuración de una sección de métodos, como GetSection y GetSectionGroup. Estos dos métodos son no estáticos. |
| Clase System.Configuration.Configuration | Representa un conjunto de datos de configuración de un equipo, aplicación, directorio Web u otro recurso. Esta clase contiene métodos útiles, como GetSection y GetSectionGroup, para actualizar los valores de configuración y obtener referencias a secciones y grupos de sección. Esta clase se utiliza como un tipo de valor devuelto para los métodos que obtienen datos de configuración en tiempo de diseño, como los métodos de las clases WebConfigurationManager y ConfigurationManager. |
| Espacio de nombres System.Web.Configuration | Contiene las clases de controlador de sección para las secciones de configuración de ASP.NET definen en [opciones de configuración de ASP.NET](https://msdn.microsoft.com/en-us/library/b5ysx397.aspx). Las clases de controlador de sección sirven para obtener datos de configuración de una sección de métodos, como GetSection y GetSectionGroup. |
| Clase System.Web.Configuration.WebConfigurationManager | Proporciona métodos útiles para obtener referencias a los valores de configuración de tiempo de diseño y tiempo de ejecución. Estos métodos utilizan la clase System.Configuration.Configuration como un tipo de valor devuelto. Puede usar el método GetSection estático de esta clase o el método GetSection no estático de la clase System.Configuration.ConfigurationManager indistintamente. Para las configuraciones de aplicación Web, se recomienda la clase System.Web.Configuration.WebConfigurationManager en lugar de la clase System.Configuration.ConfigurationManager. |
| [System.Configuration.Provider](https://msdn.microsoft.com/en-us/library/system.configuration.provider.aspx) espacio de nombres | Proporciona una manera de personalizar y extender el proveedor de configuración. Esta es la clase base para todas las clases de proveedor en el sistema de configuración. |
| [System.Web.Management](https://msdn.microsoft.com/en-us/library/system.web.management.aspx) espacio de nombres | Contiene clases e interfaces para administrar y supervisar el estado de las aplicaciones Web. En sentido estricto, este espacio de nombres no se considera parte de la API de configuración. Por ejemplo, el seguimiento y que desencadenó el evento se realiza mediante las clases de este espacio de nombres. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/en-us/library/system.management.instrumentation.aspx) espacio de nombres | Proporciona las clases necesarias para la instrumentación de aplicaciones para exponer sus datos de administración y eventos a través de Windows Management Instrumentation (WMI) para los consumidores potenciales. Supervisión de estado ASP.NET utiliza WMI para entregar eventos. En sentido estricto, este espacio de nombres no se considera parte de la API de configuración. |

## <a name="reading-from-aspnet-configuration-files"></a>Leer archivos de configuración de ASP.NET

La clase WebConfigurationManager es la clase principal para leer de archivos de configuración de ASP.NET. Existen básicamente tres pasos para leer archivos de configuración de ASP.NET:

1. Obtener un objeto de configuración mediante el método OpenWebConfiguration.
2. Obtener una referencia a la sección deseada en el archivo de configuración.
3. Leer la información deseada del archivo de configuración.

La configuración que representa el objeto no representa un archivo de configuración determinado. En su lugar, representa una vista combinada de la configuración de un equipo, la aplicación o el sitio Web. El ejemplo de código siguiente crea una instancia de un objeto de configuración que representa la configuración de una aplicación Web denominada *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Tenga en cuenta que si no existe la ruta de acceso /ProductInfo, el código anterior devolverá la configuración predeterminada según lo especificado en el archivo machine.config.


Una vez que tenga el objeto de configuración, a continuación, puede usar el método GetSection o GetSectionGroup para profundizar en los valores de configuración. En el ejemplo siguiente se obtiene una referencia a la configuración de suplantación para la aplicación ProductInfo anterior:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Escribir en archivos de configuración de ASP.NET

Como en la lectura de archivos de configuración, la clase WebConfigurationManager es el núcleo para escribir en archivos de configuración de Asp.NET. También hay tres pasos para escribir en archivos de configuración de ASP.NET.

1. Obtener un objeto de configuración mediante el método OpenWebConfiguration.
2. Obtener una referencia a la sección deseada en el archivo de configuración.
3. Escribir la información deseada desde el archivo de configuración mediante el guardar o guardar como método.

Los siguientes cambios en el código la **depurar** atributo de la &lt;compilación&gt; elemento en false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Cuando se ejecuta este código, el **depurar** atributo de la &lt;compilación&gt; elemento se establecerá en false para el *webApp* archivo web.config de la aplicación.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

El espacio de nombres System.Web.Management proporciona las clases e interfaces para administrar y supervisar el estado de las aplicaciones ASP.NET.

El registro se consigue mediante la definición de una regla que asocia los eventos con un proveedor. La regla define el tipo de eventos que se envía al proveedor. Los siguientes eventos de base están disponibles para que se inicie:

| **WebBaseEvent** | La clase de evento de base para todos los eventos. Contiene la secuencia de propiedades para todos los eventos, como el código de evento, código de detalle de evento, fecha y hora que se generó el evento, número, el mensaje de evento y los detalles del evento. |
| --- | --- |
| **WebManagementEvent** | La clase de evento de base para los eventos de administración, como la duración de la aplicación, solicitud, errores y eventos de auditoría. |
| **WebHeartbeatEvent** | El evento generado por la aplicación en intervalos regulares para capturar información de estado en tiempo de ejecución útil. |
| **WebAuditEvent** | La clase base para los eventos de auditoría de seguridad, que se utiliza para marcar condiciones como error de autorización, error de descifrado, *etcetera.* |
| **WebRequestEvent** | La clase base para todos los eventos de solicitud informativo. |
| **WebBaseErrorEvent** | La clase base para todos los eventos que indican que las condiciones de error. |

Los tipos de proveedores disponibles permiten enviar la salida de eventos para el Visor de eventos, SQL Server, Windows Management Instrumentation (WMI) y correo electrónico. Las asignaciones de eventos y proveedores configurados previamente reducen la cantidad de trabajo necesario para obtener el resultado de los eventos registrado.

ASP.NET 2.0 utiliza el registro de eventos proveedor out-of-the-box para registrar eventos basados en dominios de aplicación, iniciar y detener, así como registrar las excepciones no controladas. Esto ayuda a se abordan algunos de los escenarios básicos. Por ejemplo, supongamos que su aplicación inicia una excepción, pero el usuario no guarda el error y no puede reproducirlo. Con la regla de registro de eventos de forma predeterminada, se podrá recopilar la información de excepción y la pila para obtener una mejor idea de qué tipo de error se produjo. Otro ejemplo es aplicable si la aplicación está perdiendo el estado de sesión. En ese caso, puede buscar en el registro de eventos para determinar si se recicla el dominio de aplicación, y por qué se detuvo el dominio de aplicación en primer lugar.

Además, el sistema de supervisión de estado es extensible. Por ejemplo, puede definir los eventos Web personalizados, se activan ellos dentro de la aplicación y, a continuación, definir una regla para enviar la información de evento a un proveedor como el correo electrónico. Esto le permite asociar fácilmente la instrumentación a lo proveedores de seguimiento de estado. Como otro ejemplo, podría desencadenar un evento cada vez que un pedido se procesa y configurar una regla que envía cada evento a la base de datos de SQL Server. También podría desencadenar un evento cuando un usuario no puede iniciar sesión varias veces en una fila y el evento configurado para usar los proveedores basada en correo electrónico.

La configuración de los proveedores predeterminados y los eventos se almacena en el archivo Web.config global. El archivo Web.config global almacena toda la configuración basada en Web que se almacenaron en el archivo Machine.config en ASP.NET 1 x. El archivo Web.config global se encuentra en el siguiente directorio:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

El &lt;healthMonitoring&gt; sección del archivo Web.config global proporciona la configuración predeterminada. Puede invalidar esta configuración o configurar su propia configuración implementando la &lt;healthMonitoring&gt; sección en el archivo Web.config de la aplicación.

El &lt;healthMonitoring&gt; sección del archivo Web.config global contiene los siguientes elementos:

| **proveedores** | Contiene los proveedores de configurado para el Visor de eventos, WMI y SQL Server. |
| --- | --- |
| **eventMappings** | Contiene asignaciones para las diferentes clases de WebBase. Puede ampliar esta lista si generar su propia clase de evento. Generar su propia clase de evento proporciona una granularidad más fina sobre los proveedores de que información que se envía. Por ejemplo, podría configurar las excepciones no controladas se envíen a SQL Server, al enviar sus propios eventos personalizados para enviar por correo electrónico. |
| **reglas** | Vínculos eventMappings al proveedor. |
| **almacenamiento en búfer** | Se usa con los proveedores de SQL Server y el correo electrónico para determinar con qué frecuencia se vuelca los eventos en el proveedor. |

A continuación se muestra un ejemplo de código del archivo Web.config global.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Cómo almacenar eventos en Visor de eventos

Como se mencionó anteriormente, el proveedor de registro de eventos en el evento Visor está configurado para que en el archivo Web.config global. De forma predeterminada, todos los eventos se basan en **WebBaseErrorEvent** y **WebFailureAuditEvent** se registran. Puede agregar más reglas para registrar información adicional en el registro de eventos. Por ejemplo, si desea registrar todos los eventos (*, es decir,*, todos los eventos según **WebBaseEvent**), puede agregar la regla siguiente al archivo Web.config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Esta regla se vincula el **todos los eventos** mapa de eventos para el proveedor de registro de eventos. EventMapping y el proveedor se incluyen en el archivo Web.config global.

## <a name="how-to-store-events-to-sql-server"></a>Cómo almacenar los eventos de SQL Server

Este método usa la **ASPNETDB** base de datos que se genera por Aspnet\_regsql.exe herramienta. El proveedor predeterminado utiliza la cadena de conexión de LocalSqlServer, que utiliza cualquier una basada en el archivo de base de datos en la aplicación\_carpeta de datos o la instancia de SQLExpress local de SQL Server. La cadena de conexión de LocalSqlServer y el SqlProvider se configuran en el archivo Web.config global.

La cadena de conexión de LocalSqlServer en el archivo Web.config global tiene el siguiente aspecto:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Si desea utilizar otra instancia de SQL Server, debe usar Aspnet\_regsql.exe herramienta, que se encuentra en el % windir%\Microsoft.Net\Framework\v2.0.\* carpeta. Utilice Aspnet\_regsql.exe herramienta para generar un personalizado **ASPNETDB** en la instancia de SQL Server, la base de datos, a continuación, agregue la cadena de conexión para el archivo de configuración de aplicaciones y, a continuación, agregar un proveedor mediante el nuevo cadena de conexión. Una vez que tenga el **ASPNETDB** crea la base de datos, deberá establecer una regla para vincular un eventMapping a la sqlProvider.

Si usa el valor predeterminado SqlProvider o configurar su propio proveedor, debe agregar una regla de vincular el proveedor con un mapa de eventos. La siguiente regla vincula el nuevo proveedor que haya creado anteriormente para la **todos los eventos** mapa de eventos. Esta regla se registrarán todos los eventos en función de **WebBaseEvent** y enviarlos a la MySqlWebEventProvider que usará la cadena de conexión MYASPNETDB. El código siguiente agrega una regla para vincular el proveedor con un mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Si desea enviar solo errores a SQL Server, podría agregar la siguiente regla:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Cómo reenviar los eventos de WMI

También puede reenviar los eventos de WMI. El proveedor WMI se configura automáticamente en el archivo Web.config global de forma predeterminada.

En el ejemplo de código siguiente se agrega una regla para reenviar los eventos WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Debe agregar una regla para asociar un eventMapping al proveedor y también una aplicación de agente de escucha WMI para realizar escuchas de los eventos. En el ejemplo de código siguiente se agrega una regla para vincular el proveedor WMI para la **todos los eventos** mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-e-mail"></a>Cómo reenviar los eventos se van a enviar por correo electrónico

También puede reenviar eventos a enviar por correo electrónico. Tenga cuidado con las reglas de eventos que asigne a su proveedor de correo electrónico, como se puede enviar involuntariamente por sí mismo una gran cantidad de información que pueden ser más adecuado para SQL Server o el registro de eventos. Existen dos proveedores de correo electrónico; SimpleMailWebEventProvider y TemplatedMailWebEventProvider. Cada uno tiene los mismos atributos de configuración, con la excepción de los atributos "plantilla" y "detailedTemplateErrors", que solo están disponibles en el TemplatedMailWebEventProvider.

> [!NOTE]
> Ninguno de estos proveedores de correo electrónico se configura automáticamente. Debe agregarlos al archivo Web.config.


La diferencia principal entre estos proveedores de correo dos electrónico es que SimpleMailWebEventProvider envía mensajes de correo electrónico en una plantilla genérica que no se puede modificar. El archivo Web.config de ejemplo agrega este proveedor de correo electrónico a la lista de proveedores configurados mediante la siguiente regla:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

También se agrega la siguiente regla para asociar el proveedor de correo electrónico para la **todos los eventos** mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 seguimiento

Hay tres de las principales mejoras en el seguimiento en ASP.NET 2.0.

1. Nueva funcionalidad integrada de seguimiento
2. Acceso mediante programación a los mensajes de seguimiento
3. Seguimiento de nivel de aplicación mejorado

## <a name="integrated-tracing-functionality"></a>Integra la funcionalidad de seguimiento

Ahora puede enrutar mensajes emitidos por la clase System.Diagnostics.Trace a resultados de seguimiento de ASP.NET y enrutar mensajes emitidos por la traza de ASP.NET a System.Diagnostics.Trace. También puede reenviar los eventos de instrumentación de ASP.NET a System.Diagnostics.Trace. Esta funcionalidad se proporciona mediante la nueva **writeToDiagnosticsTrace** atributo de la &lt;seguimiento&gt; elemento. Cuando este valor booleano es true, los mensajes de seguimiento de ASP.NET se reenvían a la infraestructura de seguimiento de System.Diagnostics para su uso por los agentes de escucha registrados para mostrar los mensajes de seguimiento.

## <a name="programmatic-access-to-trace-messages"></a>Acceso mediante programación a mensajes de seguimiento

ASP.NET 2.0 permite obtener acceso mediante programación a todos los mensajes de seguimiento a través de la **TraceContextRecord** clase y la **TraceRecords** colección. La manera más eficaz de obtener acceso a los mensajes de seguimiento consiste en registrar un **TraceContextEventHandler** delegado (otra característica nueva en ASP.NET 2.0) para controlar la nueva **TraceFinished** eventos. A continuación, puede recorrer los mensajes de seguimiento como desee.

Esto ilustra en el ejemplo de código siguiente:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

En el ejemplo anterior, recorrer en iteración la colección TraceRecords y, a continuación, escribir cada mensaje en la secuencia de respuesta.

## <a name="improved-application-level-tracing"></a>Seguimiento de nivel de aplicación mejorado

Se ha mejorado la traza de nivel de aplicación a través de la introducción del nuevo **mostRecent** atributo de la &lt;seguimiento&gt; elemento. Este atributo especifica si se muestra el resultado del seguimiento de nivel de aplicación más reciente y se descartan los datos de seguimiento anteriores más allá de los límites que se indican mediante requestLimit. Si es false, se muestran los datos de seguimiento para las solicitudes hasta que se alcanza el valor del atributo requestLimit.

## <a name="aspnet-command-line-tools"></a>Herramientas de línea de comandos ASP.NET

Hay varias herramientas de línea de comandos para ayudar en la configuración de ASP.NET. Los programadores de ASP.NET deben estar familiarizados con aspnet\_regiis.exe herramienta. ASP.NET 2.0 proporciona tres otras herramientas de línea de comandos para ayudar en la configuración.

Las herramientas de línea de comandos siguientes están disponibles:

| **Herramienta** | **Uso** |
| --- | --- |
| **ASPNET\_regiis.exe** | Permite registrar ASP.NET con IIS. Hay dos versiones de estas herramientas que se incluyen con ASP.NET 2.0, uno para los sistemas de 32 bits (en la carpeta Framework) y otro para sistemas de 64 bits (en la carpeta Framework64.) No se instalará la versión de 64 bits en un sistema operativo de 32 bits. |
| **ASPNET\_regsql.exe** | La herramienta de registro de SQL Server de ASP.NET se utiliza para crear una base de datos de Microsoft SQL Server para su uso por los proveedores de SQL Server en ASP.NET, o para agregar o quitar opciones de una base de datos existente. Aspnet\_regsql.exe archivo se encuentra en la [carpeta de drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber en el servidor Web. |
| **ASPNET\_regbrowsers.exe** | La herramienta de registro de explorador ASP.NET analiza y compila todas las definiciones del explorador de todo el sistema en un ensamblado e instala al ensamblado en la caché global de ensamblados. La herramienta utiliza los archivos de definición de explorador (. Archivos de explorador) del subdirectorio de exploradores de .NET Framework. La herramienta se puede encontrar en el directorio %SystemRoot%\Microsoft.NET\Framework\version\. |
| **ASPNET\_compiler.exe** | La herramienta de compilación de ASP.NET permite compilar una aplicación Web ASP.NET, en su lugar o para su implementación en una ubicación de destino como un servidor de producción. Compilación en contexto mejora el rendimiento de la aplicación porque los usuarios finales no se produce un retraso en la primera solicitud a la aplicación mientras se compila la aplicación. |

Dado que aspnet\_regiis.exe herramienta no está familiarizado con ASP.NET 2.0, no trataremos aquí.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Herramienta de registro de servidor ASP.NET SQL - aspnet\_regsql.exe

Puede establecer varios tipos de opciones con la herramienta de registro de SQL Server de ASP.NET. Puede especificar una conexión de SQL, especificar qué servicios de aplicaciones ASP.NET utilizan SQL Server para administrar la información, indicar qué base de datos o tabla se utiliza para la dependencia de caché SQL y agregar o quitar la compatibilidad para utilizar SQL Server para almacenar los procedimientos y el estado de sesión.

Varios servicios de aplicación de ASP.NET dependen de un proveedor para administrar el almacenamiento y recuperación de datos desde un origen de datos. Cada proveedor es específico para el origen de datos. ASP.NET incluye un proveedor de SQL Server para las siguientes características ASP.NET:

- Pertenencia (la [SqlMembershipProvider](https://msdn.microsoft.com/en-us/library/system.web.security.sqlmembershipprovider.aspx) clase).
- Administración de roles (el [SqlRoleProvider](https://msdn.microsoft.com/en-us/library/system.web.security.sqlroleprovider.aspx) clase).
- Perfil (el [SqlProfileProvider](https://msdn.microsoft.com/en-us/library/system.web.profile.sqlprofileprovider.aspx) clase).
- Personalización de elementos Web (el [SqlPersonalizationProvider](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) clase).
- Eventos de Web (el [SqlWebEventProvider](https://msdn.microsoft.com/en-us/library/system.web.management.sqlwebeventprovider.aspx) clase).

Al instalar ASP.NET, el archivo Machine.config del servidor incluye elementos de configuración que especifican los proveedores de SQL Server para cada una de las características ASP.NET que se basan en un proveedor. Estos proveedores se configuran de forma predeterminada, para conectarse a una instancia de usuario local de SQL Server Express 2005. Si cambia la cadena de conexión predeterminado utilizada por los proveedores, para poder usar cualquiera de las características ASP.NET configuradas en la configuración del equipo, debe instalar la base de datos de SQL Server y los elementos de la base de datos para la característica elegida mediante Aspnet\_regsql.exe. Si la base de datos que se especifique con la herramienta de registro SQL no existe (aspnetdb será la base de datos predeterminada si no se especifica en la línea de comandos), a continuación, el usuario actual debe tener derechos para crear bases de datos en SQL Server, así como para crear el esquema e elementos dentro de una base de datos.

### <a name="sql-cache-dependency"></a>Dependencia de memoria caché de SQL

Una característica avanzada de caché de resultados ASP.NET es la dependencia de la memoria caché SQL. Dependencia de la memoria caché SQL admite dos modos diferentes de operación: uno que utiliza una implementación de ASP.NET de sondeo de la tabla y un segundo modo que utiliza las características de notificación de consulta de SQL Server 2005. La herramienta de registro SQL puede utilizarse para configurar el modo de sondeo de la tabla de operación.

### <a name="session-state"></a>Estado de la sesión

De forma predeterminada, la información y los valores de estado de sesión se almacenan en memoria dentro del proceso ASP.NET. Como alternativa, puede almacenar datos de la sesión en una base de datos de SQL Server, donde se puede compartir en varios servidores Web. Si la base de datos que especifique para el estado de sesión con la herramienta de registro SQL no existe, el usuario actual debe tener derechos para crear bases de datos en SQL Server y crear elementos de esquema dentro de una base de datos. Si existe la base de datos, el usuario actual debe tener derechos para crear elementos de esquema en la base de datos existente.

Para instalar la base de datos de estado de sesión en SQL Server, ejecute Aspnet\_regsql.exe herramienta y proporcionar la información con el comando siguiente:

- El nombre de SQL Server de la instancia, con el **-S** opción.
- Las credenciales de inicio de sesión para una cuenta que tenga permiso para crear una base de datos en un equipo que ejecuta SQL Server. Utilice la **-E** opción para usar el usuario ha iniciado la sesión o usar el **- U** opción para especificar un identificador de usuario junto con la **-P** opción para especificar una contraseña.
- El **- ssadd** opción de línea de comandos para agregar la base de datos de estado de sesión.

De forma predeterminada, no puede usar Aspnet\_regsql.exe herramienta para instalar la base de datos de estado de sesión en un equipo que ejecuta SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>La herramienta de registro de explorador ASP.NET - aspnet\_regbrowsers.exe

En la versión 1.1 de ASP.NET, el archivo Machine.config incluía una sección denominada &lt;browserCaps&gt;. Esta sección contiene una serie de entradas XML que definen las configuraciones de varios exploradores basándose en una expresión regular. ASP.NET versión 2.0, una nueva. BROWSER define los parámetros de un explorador determinado utilizando entradas XML. Agregar información en una nueva ventana del explorador mediante la adición de una nueva. Archivo de explorador a la carpeta que se encuentra en %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers en el sistema.

Dado que una aplicación no está leyendo un archivo .config cada vez que requiere información del explorador, puede crear una nueva. BROWSER y ejecución Aspnet\_regbrowsers.exe para agregar los cambios necesarios al ensamblado. Esto permite al servidor tener acceso inmediato a la nueva información de explorador por lo que no es necesario apagar alguna de las aplicaciones para recoger la información. Una aplicación puede tener acceso a las funciones del explorador a través de la propiedad del explorador de la solicitud HTTP actual.

Las siguientes opciones están disponibles cuando se ejecuta aspnet\_regbrowser.exe:

| **Opción** | **Descripción** |
| --- | --- |
| **-?** | Muestra Aspnet\_regbbrowsers.exe texto de ayuda en la ventana de comandos. |
| **-i** | Crea el ensamblado de funciones de explorador en tiempo de ejecución y lo instala en la caché global de ensamblados. |
| **-u** | Desinstala el ensamblado de funciones de explorador en tiempo de ejecución de la caché global de ensamblados. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>La herramienta de compilación de ASP.NET - aspnet\_compiler.exe

La herramienta de compilación de ASP.NET se puede usar de dos formas generales: para en contexto y la compilación para la implementación, donde se especifica un directorio de salida de destino.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[Compilar una aplicación en su lugar](https://msdn.microsoft.com/en-us/library/ms229863.aspx)

La herramienta de compilación de ASP.NET puede compilar una aplicación en su lugar, es decir, imita el comportamiento de realización de varias solicitudes a la aplicación, lo que produce la compilación normal. Los usuarios de un sitio precompilado no experimentarán una demora causada por la compilación de la página en la primera solicitud.

Al precompilar un sitio en su lugar, se aplican los siguientes elementos:

- El sitio retiene sus archivos y la estructura de directorios.
- Debe tener los compiladores para todos los lenguajes de programación utilizados por el sitio en el servidor.
- Si los archivos se produce un error de compilación, todo el sitio produce un error en la compilación.

También puede volver a compilar una aplicación en su lugar después de agregar nuevos archivos de origen. La herramienta compila los archivos nuevos o modificados a menos que incluya la **- c** opción.

> [!NOTE]
> Compilación de una aplicación que contiene una aplicación anidada no compila la aplicación anidada. La aplicación anidada se debe compilar por separado.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[Compilar una aplicación para la implementación](https://msdn.microsoft.com/en-us/library/ms229863.aspx)

Compilar una aplicación para la implementación (compilación para una ubicación de destino) especificando el parámetro targetDir. TargetDir puede ser la ubicación final de la aplicación Web o la aplicación compilada se puede implementar más. Mediante el **-u** opción compila la aplicación de tal manera que puede realizar cambios en algunos archivos de la aplicación compilada sin recompilarla. ASPNET\_compiler.exe hace una distinción entre los tipos de archivos estáticos y dinámicos y reacciona ante ellas diferente al crear la aplicación resultante.

- Tipos de archivos estáticos son aquellos que no tienen un compilador asociado o que generan un proveedor, tales como archivos cuya con nombre tienen extensiones como .css, .gif, .htm, .html, .jpg, .js y así sucesivamente. Estos archivos simplemente se copian en la ubicación de destino, con sus posiciones relativas en la estructura de directorios que se conservan.
- Tipos de archivos dinámicos son aquellos que tienen un compilador asociado o que generan un proveedor, incluidos los archivos con extensiones de nombre de archivo específicos de ASP.NET como .asax, .ascx, .ashx, .aspx, Browser,. master y así sucesivamente. La herramienta de compilación de ASP.NET genera ensamblados a partir de estos archivos. Si el **-u** opción se omite, la herramienta también crea archivos con la extensión de nombre de archivo. COMPILED que se asignan los archivos de origen a su ensamblado. Para asegurarse de que se conserva la estructura de directorios de origen de la aplicación, la herramienta genera archivos de marcador de posición en las ubicaciones correspondientes en la aplicación de destino.

Debe utilizar el **-u** opción para indicar que el contenido de la aplicación compilada se puede modificar. De lo contrario, las modificaciones posteriores se pasan por alto o provocarán errores en tiempo de ejecución.

La tabla siguiente describe cómo la compilación de ASP.NET herramienta identificadores diferentes tipos de archivo cuando el **-u** se incluye la opción.

| **Tipo de archivo** | **Acción del compilador** |
| --- | --- |
| .aspx, .ascx,. master | Estos archivos se dividen en marcado y código fuente, que incluye archivos de código subyacente y cualquier código que se incluye en &lt;script runat = "server"&gt; elementos. Código fuente se compila en ensamblados, con nombres que se derivan de un algoritmo hash, y los ensamblados se colocan en el directorio Bin. Cualquier código en línea, es decir, código agregado entre el  **&lt; %**  y  **% &gt;**  corchetes, se incluye con marcado y no compilado. Nuevos archivos con el mismo nombre que los archivos de origen se crea para contener el marcado y se colocan en los directorios de salida correspondiente. |
| .ashx, .asmx | Estos archivos no se compilan y se mueven a los directorios de salida tal y como está y no compila. Si desea tener compilado el código del controlador, coloque el código en archivos de código fuente en la aplicación\_directorio de código. |
| . cs, .vb, .jsl, .cpp (sin incluir los archivos de código subyacente para los tipos de archivo enumerados anteriormente) | Estos archivos se compilan y se incluye como recurso en ensamblados que hacen referencia a ellos. Archivos de código fuente no se copian en el directorio de salida. Si no se hace referencia a un archivo de código, no se compila. |
| Tipos de archivo personalizados | Estos archivos no se compilan. Estos archivos se copian en los directorios de salida correspondiente. |
| Los archivos de código en la aplicación de origen\_subdirectorio de código | Estos archivos se compilan en ensamblados y se colocan en el directorio Bin. |
| archivos .resx y .resource en la aplicación\_GlobalResources subdirectorio | Estos archivos se compilan en ensamblados y se colocan en el directorio Bin. Ninguna aplicación\_GlobalResources subdirectorio se crea en el directorio de salida principal y no hay archivos .resx o .resources que se encuentra en el directorio de origen se copian en los directorios de salida. |
| archivos .resx y .resource en la aplicación\_LocalResources subdirectorio | Estos archivos no se compilan y se copian en los directorios de salida correspondiente. |
| skin archivos en la aplicación\_subdirectorio de temas | Los archivos de skin y tema de estáticos no se compilan y se copian en los directorios de salida correspondiente. |
| Browser archivo Web.config estático tipos ensamblados ya está presentes en el directorio Bin | Estos archivos se copian tal cual en los directorios de salida. |

La tabla siguiente describe cómo la compilación de ASP.NET herramienta identificadores diferentes tipos de archivo cuando el **-u** se omite la opción.

| **Tipo de archivo** | **Acción del compilador** |
| --- | --- |
| .aspx, .asmx, .ashx,. master | Estos archivos se dividen en marcado y código fuente, que incluye archivos de código subyacente y cualquier código que se incluye en &lt;script runat = "server"&gt; elementos. Código fuente se compila en ensamblados, con nombres que se derivan de un algoritmo hash. Los ensamblados resultantes se colocan en el directorio Bin. Cualquier código en línea, es decir, código agregado entre el  **&lt; %**  y  **% &gt;**  corchetes, se incluye con marcado y no compilado. El compilador crea nuevos archivos para contener el marcado con el mismo nombre que los archivos de origen. Los archivos resultantes se colocan en el directorio Bin. El compilador también crea archivos con el mismo nombre que los archivos de origen, pero con la extensión. COMPILED que contienen información de asignación. El archivo. Archivos COMPILADOS se colocan en los directorios de salida correspondiente a la ubicación original de los archivos de origen. |
| .ascx | Estos archivos se dividen en marcado y código fuente. Código fuente se compilan en ensamblados y se coloca en el directorio Bin, con nombres que se derivan de un algoritmo hash. Se genera ningún archivo de marcado. |
| . cs, .vb, .jsl, .cpp (sin incluir los archivos de código subyacente para los tipos de archivo enumerados anteriormente) | Código fuente que hace referencia a los ensamblados generados a partir de archivos .aspx, .ashx o. ascx se compilan en ensamblados y se coloca en el directorio Bin. Se copia ningún archivo de origen. |
| Tipos de archivo personalizados | Estos archivos se compilan como archivos dinámicos. Según el tipo de archivo que se basan, el compilador puede colocar los archivos de asignación en los directorios de salida. |
| Archivos de la aplicación\_subdirectorio de código | Archivos de código fuente de este subdirectorio se compilan en ensamblados y se colocan en el directorio Bin. |
| Archivos de la aplicación\_GlobalResources subdirectorio | Estos archivos se compilan en ensamblados y se colocan en el directorio Bin. Ninguna aplicación\_GlobalResources subdirectorio se crea en el directorio de salida principal. Si el archivo de configuración especifica appliesTo = "All", se copian los archivos .resources y .resx en los directorios de salida. No se copian si se hace referencia a un [BuildProvider](https://msdn.microsoft.com/en-us/library/system.web.configuration.buildprovider.aspx). |
| archivos .resx y .resource en la aplicación\_LocalResources subdirectorio | Estos archivos se compilan en ensamblados con nombres únicos y se colocan en el directorio Bin. No hay archivos .resx o .resource se copian en los directorios de salida. |
| skin archivos en la aplicación\_subdirectorio de temas | Los temas se compilan en ensamblados y se colocan en el directorio Bin. Archivos de código auxiliar se crean para los archivos de skin y se coloca en el directorio de salida correspondiente. Archivos estáticos (como .css) se copian en los directorios de salida. |
| Browser archivo Web.config estático tipos ensamblados ya está presentes en el directorio Bin | Estos archivos se copian tal cual en el directorio de salida. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[Nombres de ensamblado fija](https://msdn.microsoft.com/en-us/library/ms229863.aspx##)

Algunos escenarios, como la implementación de una aplicación Web utilizando a MSI Windows Installer, requieren el uso de nombres de archivo coherentes y contenido, así como estructuras de directorios coherente para identificar los ensamblados o valores de configuración para las actualizaciones. En esos casos, puede usar el **- fixednames** opción para especificar que la herramienta de compilación de ASP.NET debe compilar un ensamblado para cada archivo de código fuente en lugar de usar la donde varias páginas se compilan en ensamblados. Esto puede provocar un gran número de ensamblados, por lo que si le preocupa la escalabilidad debe usar esta opción con precaución.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomen-uslibraryms229863aspx"></a>[Compilación de nombre seguro](https://msdn.microsoft.com/en-us/library/ms229863.aspx##)

El **- aptca**, **- delaysign**, **- keycontainer** y **- keyfile** opciones se proporcionan para que pueda utilizar Aspnet\_ Compiler.exe encarecidamente crear ensamblados con nombre sin usar la [herramienta nombre seguro (Sn.exe)](https://msdn.microsoft.com/en-us/library/k5b5tt23.aspx) por separado. Estas opciones corresponden respectivamente a **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**y  **AssemblyKeyFileAttribute**.

Explicación de estos atributos se queda fuera del ámbito de este curso.

## <a name="labs"></a>Laboratorios

Cada una de las siguientes prácticas se basa en las prácticas anteriores. Debe realizarlas en orden.

## <a name="lab-1-using-the-configuration-api"></a>Práctica 1: Usar la API de configuración

1. Crear un nuevo sitio Web denominado *mod9lab*.
2. Agregue un nuevo archivo de configuración de Web para el sitio.
3. Agregue lo siguiente al archivo web.config:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Esto garantizará que tiene permiso para guardar los cambios en el archivo web.config.

1. Agregar un nuevo control de etiqueta a Default.aspx y cambie el identificador a **lblDebugStatus**.
2. Agregar un nuevo control de botón a Default.aspx.
3. Cambiar Id. del control de botón a **btnToggleDebug** y el texto para **estado de alternancia depurar**.
4. Abra la vista de código para el archivo de código subyacente de Default.aspx y agregue un **con** instrucción para **System.Web.Configuration** como se indica a continuación:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Agregue dos variables privadas a la clase y una página\_Init (método), tal y como se muestra a continuación:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Agregue el código siguiente a la página\_carga:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Guarde y examinar default.aspx. Observe que el control de etiqueta muestra el estado de depuración actual.
2. Haga doble clic en el control de botón en el diseñador y agregue el código siguiente para el evento Click para el control de botón:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Guardar y default.aspx y haga clic en el botón.
2. Abra el archivo web.config después de que todos los botones, haga clic en y observar el **depurar** de atributo en el &lt;compilación&gt; sección.

## <a name="lab-2-logging-application-restarts"></a>Práctica 2: Registro reinicios de la aplicación

En este laboratorio, creará el código que le permitirá activar o desactivar el registro de compilaciones en el Visor de eventos, inicios y cierres de aplicación.

1. Agregar un DropDownList a default.aspx y cambie el identificador a ddlLogAppEvents.
2. Establecer el **AutoPostBack** propiedad DropDownList a **true**.
3. Agregue tres elementos a la colección de elementos de DropDownList. Realizar la **texto** del primer elemento *Select Value* y el valor -1. Realizar la **texto** y **valor** del segundo elemento **True** y **texto** y **valor** del tercer elemento **False**.
4. Agregue una nueva etiqueta a default.aspx. Cambie el identificador a **lblLogAppEvents**.
5. Abra la vista de código subyacente para default.aspx y agregue una nueva declaración de una variable de tipo HealthMonitoringSection tal y como se muestra a continuación:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Agregue el código siguiente en el código existente en la página\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Haga doble clic en DropDownList y agregue el código siguiente al evento SelectedIndexChanged:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Examinar default.aspx.
2. Establece la lista desplegable en **False**.
3. Borrar el registro de aplicación en el Visor de eventos.
4. Haga clic en el botón para cambiar el atributo de depuración para la aplicación.
5. Actualizar el registro de aplicación en el Visor de eventos. 

    1. ¿Se registraron los eventos?
    2. ¿Por qué o por qué no?
6. Establece la lista desplegable en **es True.**
7. Haga clic en el botón para alternar el atributo de depuración para la aplicación.
8. Actualizar el inicio de sesión de aplicación del Visor de eventos. 

    1. ¿Se registraron los eventos?
    2. ¿Cuál era el motivo del apagado de la aplicación?
9. Experimentar con activar y desactivar el registro y examine los cambios realizados en el archivo web.config.

## <a name="more-information"></a>Más información:

ASP.NET del 2.0 modelo de proveedor le permite crear sus propios proveedores de Instrumental de aplicación no solo, pero para muchos otros usos, así como la pertenencia a grupos, perfiles, etcetera. Para obtener información detallada sobre cómo escribir un proveedor personalizado para registrar eventos de aplicación en un archivo de texto, visite [este vínculo](https://msdn.microsoft.com/library/default.asp?url=/library/en-us/dnaspp/html/ASPNETProvMod_Prt6.asp).
