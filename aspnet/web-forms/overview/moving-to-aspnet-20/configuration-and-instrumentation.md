---
uid: web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
title: Configuración e instrumentación | Microsoft Docs
author: microsoft
description: Hay cambios importantes en la configuración e instrumentación de ASP.NET 2.0. La nueva API de configuración de ASP.NET permite cambios de configuración que se realizan pr...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 21ebbaee-7ed8-45ae-b6c1-c27c88342e48
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/configuration-and-instrumentation
msc.type: authoredcontent
ms.openlocfilehash: dc4e75e8c97228bf14935d6bf4242a036d513816
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399310"
---
<a name="configuration-and-instrumentation"></a>Configuración e instrumentación
====================
por [Microsoft](https://github.com/microsoft)

> Hay cambios importantes en la configuración e instrumentación de ASP.NET 2.0. La nueva API de configuración de ASP.NET permite que se realicen mediante programación los cambios de configuración. Además, existen muchas nuevas opciones de configuración permiten nuevas configuraciones e instrumentación.


Hay cambios importantes en la configuración e instrumentación de ASP.NET 2.0. La nueva API de configuración de ASP.NET permite que se realicen mediante programación los cambios de configuración. Además, existen muchas nuevas opciones de configuración permiten nuevas configuraciones e instrumentación.

En este módulo, trataremos la API de configuración de ASP.NET como lo referente a leer y escribir en archivos de configuración de ASP.NET, y también cubriremos instrumentación de ASP.NET. También cubrimos las nuevas características disponibles en el seguimiento de ASP.NET.

## <a name="aspnet-configuration-api"></a>API de configuración de ASP.NET

La API de configuración de ASP.NET permite desarrollar, implementar y administrar datos de configuración de aplicación mediante el uso de una única interfaz de programación. Puede usar la API de configuración para desarrollar y modificar configuraciones de ASP.NET completas mediante programación sin editar directamente el XML en los archivos de configuración. Además, puede usar la API de configuración en aplicaciones de consola y scripts que desarrolle, las herramientas de administración basada en Web y complementos Microsoft Management Console (MMC).

Las dos herramientas de administración de configuración siguientes utilizan la API de configuración y se incluyen con la versión 2.0 de .NET Framework:

- El complemento MMC de ASP.NET, que usa la API de configuración para simplificar las tareas administrativas, que proporciona una vista integrada de los datos de configuración local de todos los niveles de la jerarquía de configuración.
- La herramienta de administración de sitios Web, que permite administrar la configuración para aplicaciones locales y remotas, incluidos los sitios hospedados.

La API de configuración de ASP.NET consta de un conjunto de objetos de administración de ASP.NET que puede usar para configurar sitios y aplicaciones Web mediante programación. Objetos de administración se implementan como una biblioteca de clases de .NET Framework. El modelo de programación de la API de configuración ayuda a garantizar la coherencia en el código y confiabilidad mediante la aplicación de los tipos de datos en tiempo de compilación. Para que sea más fácil de administrar las configuraciones de aplicaciones, la API de configuración le permite ver los datos que se combinan desde todos los puntos de la jerarquía de configuración como una sola colección, en lugar de visualizar los datos como colecciones independientes procedentes de distintos archivos de configuración. Además, la API de configuración permite manipular las configuraciones de toda la aplicación sin editar directamente el XML en los archivos de configuración. Por último, la API simplifica las tareas de configuración al admitir herramientas administrativas, como la herramienta de administración de sitios Web. La API de configuración simplifica la implementación, que admiten la creación de archivos de configuración en un equipo y ejecute los scripts de configuración en varios equipos.

> [!NOTE]
> La API de configuración no admite la creación de aplicaciones de IIS.


## <a name="working-with-local-and-remote-configuration-settings"></a>Trabajar con valores de configuración Local y remoto

Un objeto de configuración representa la vista combinada de las opciones de configuración que se aplican a una entidad física específica, como un equipo, o a una entidad lógica, como una aplicación o un sitio Web. La entidad lógica especificada puede existir en el equipo local o en un servidor remoto. Cuando no existe ningún archivo de configuración para una entidad especificada, el objeto de configuración representa la configuración predeterminada como se define en el archivo Machine.config.

Puede obtener un objeto de configuración mediante uno de los métodos de configuración abierto de las siguientes clases:

1. La clase ConfigurationManager, si la entidad es una aplicación cliente.
2. La clase WebConfigurationManager, si la entidad es una aplicación Web.

Estos métodos devolverán un objeto de configuración, que a su vez proporciona los métodos y propiedades necesarios para controlar los archivos de configuración subyacente. Puede tener acceso a estos archivos de lectura o escritura.

### <a name="reading"></a>Lectura

Utilice el método GetSection o GetSectionGroup para leer información de configuración. El usuario o proceso que lee debe tener permisos de lectura en todos los archivos de configuración de la jerarquía.

> [!NOTE]
> Si usa un método GetSection estático que toma un parámetro de ruta de acceso, el parámetro de ruta de acceso debe hacer referencia a la aplicación en el que se ejecuta el código. En caso contrario, se omite el parámetro y se devuelve la información de configuración de la aplicación que se está ejecutando.


### <a name="writing"></a>Escritura

Usa uno de los métodos Save para escribir información de configuración. El usuario o proceso que escribe debe tener permisos de escritura en el archivo de configuración y el directorio en el nivel de jerarquía de configuración actual, así como permisos de lectura en todos los archivos de configuración de la jerarquía.

Para generar un archivo de configuración que representa la configuración heredada de una entidad especificada, use uno de los métodos de guardar la configuración siguiente:

1. El método Save para crear un nuevo archivo de configuración.
2. El método SaveAs para generar un nuevo archivo de configuración en otra ubicación.

## <a name="configuration-classes-and-namespaces"></a>Espacios de nombres y clases de configuración

Muchos métodos y clases de configuración son similares entre sí. La tabla siguiente describen las clases de configuración frecuente y los espacios de nombres.

| **Espacio de nombres o clase de configuración** | **Descripción** |
| --- | --- |
| [System.Configuration](https://msdn.microsoft.com/library/system.configuration.aspx) espacio de nombres | Contiene las clases de configuración principal para todas las aplicaciones de .NET Framework. Se utilizan las clases de controlador de sección para obtener datos de configuración de una sección de métodos, como GetSection y GetSectionGroup. Estos dos métodos son no estáticos. |
| Clase System.Configuration.Configuration | Representa un conjunto de datos de configuración para un equipo, aplicación, directorio Web u otro recurso. Esta clase contiene métodos útiles, como GetSection y GetSectionGroup, para actualizar los valores de configuración y obtener referencias a las secciones y grupos. Esta clase se utiliza como un tipo de valor devuelto para los métodos que obtienen datos de configuración de tiempo de diseño, como los métodos de las clases WebConfigurationManager y ConfigurationManager. |
| Espacio de nombres System.Web.Configuration | Contiene las clases de controlador de sección para las secciones de configuración de ASP.NET definen en [opciones de configuración de ASP.NET](https://msdn.microsoft.com/library/b5ysx397.aspx). Se utilizan las clases de controlador de sección para obtener datos de configuración de una sección de métodos, como GetSection y GetSectionGroup. |
| Clase System.Web.Configuration.WebConfigurationManager | Proporciona métodos útiles para obtener referencias a valores de configuración tiempo de ejecución y tiempo de diseño. Estos métodos utilizan la clase System.Configuration.Configuration como un tipo de valor devuelto. Puede usar el método GetSection estático de esta clase o el método GetSection no estático de la clase System.Configuration.ConfigurationManager indistintamente. Para las configuraciones de aplicaciones Web, se recomienda la clase System.Web.Configuration.WebConfigurationManager en lugar de la clase System.Configuration.ConfigurationManager. |
| [System.Configuration.Provider](https://msdn.microsoft.com/library/system.configuration.provider.aspx) espacio de nombres | Proporciona una manera de personalizar y ampliar el proveedor de configuración. Esta es la clase base para todas las clases de proveedor en el sistema de configuración. |
| [System.Web.Management](https://msdn.microsoft.com/library/system.web.management.aspx) espacio de nombres | Contiene clases e interfaces para administrar y supervisar el estado de las aplicaciones Web. En realidad, este espacio de nombres no se considera parte de la API de configuración. Por ejemplo, seguimiento y el desencadenamiento de eventos se consigue mediante las clases de este espacio de nombres. |
| [System.Management.Instrumentation](https://msdn.microsoft.com/library/system.management.instrumentation.aspx) espacio de nombres | Proporciona las clases necesarias para la instrumentación de aplicaciones para exponer la información de administración y sus eventos a través de Windows Management Instrumentation (WMI) para los clientes potenciales. Supervisión de estado ASP.NET utiliza WMI para entregar eventos. En realidad, este espacio de nombres no se considera parte de la API de configuración. |

## <a name="reading-from-aspnet-configuration-files"></a>Leer archivos de configuración de ASP.NET

La clase WebConfigurationManager es la clase principal para leer archivos de configuración de ASP.NET. Hay esencialmente tres pasos para leer los archivos de configuración de ASP.NET:

1. Obtiene un objeto de configuración mediante el método OpenWebConfiguration.
2. Obtenga una referencia a la sección deseada en el archivo de configuración.
3. Lea la información deseada del archivo de configuración.

La configuración de objeto representa no representa un archivo de configuración determinado. En su lugar, representa una vista combinada de la configuración de un equipo, aplicación o sitio Web. Ejemplo de código siguiente crea una instancia de un objeto de configuración que representa la configuración de una aplicación Web denominada *ProductInfo*.

[!code-csharp[Main](configuration-and-instrumentation/samples/sample1.cs)]

> [!NOTE]
> Tenga en cuenta que si no existe la ruta de acceso /ProductInfo, el código anterior devolverá la configuración predeterminada según lo especificado en el archivo machine.config.


Una vez que el objeto de configuración, a continuación, puede usar el método GetSection o GetSectionGroup para profundizar en los valores de configuración. El ejemplo siguiente obtiene una referencia a la configuración de suplantación para la aplicación ProductInfo anterior:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample2.cs)]

## <a name="writing-to-aspnet-configuration-files"></a>Escribir en archivos de configuración de ASP.NET

Como se muestra en la lectura de archivos de configuración, la clase WebConfigurationManager es el núcleo para escribir en archivos de configuración de Asp.NET. También hay tres pasos para escribir en archivos de configuración de ASP.NET.

1. Obtiene un objeto de configuración mediante el método OpenWebConfiguration.
2. Obtenga una referencia a la sección deseada en el archivo de configuración.
3. Escribir la información deseada del archivo de configuración mediante SaveAs o Save (método).

El siguiente código cambia el **depurar** atributo de la &lt;compilación&gt; elemento en false:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample3.cs)]

Cuando se ejecuta este código, el **depurar** atributo de la &lt;compilación&gt; elemento se establecerá en false para el *webApp* archivo web.config de la aplicación.

## <a name="systemwebmanagement-namespace"></a>System.Web.Management Namespace

El espacio de nombres System.Web.Management proporciona las clases e interfaces para administrar y supervisar el estado de las aplicaciones ASP.NET.

El registro se logra definiendo una regla que se asocia a un proveedor de eventos. La regla define el tipo de eventos que se envían al proveedor. Los siguientes eventos de bases están disponibles para que inicie sesión:

| **WebBaseEvent** | La clase de evento base para todos los eventos. Contenga la secuencia de propiedades para todos los eventos como código de evento, código de detalle de evento, la fecha y hora que se generó el evento, número, el mensaje de evento y detalles del evento. |
| --- | --- |
| **WebManagementEvent** | La clase de evento base para eventos de administración, como la duración de la aplicación, solicitudes, errores y eventos de auditoría. |
| **WebHeartbeatEvent** | El evento generado por la aplicación en intervalos regulares para capturar información de estado en tiempo de ejecución útil. |
| **WebAuditEvent** | La clase base para los eventos de auditoría de seguridad, que se usan para marcar las condiciones como error de autorización, error de descifrado, *etcetera.* |
| **WebRequestEvent** | La clase base para todos los eventos de solicitud informativo. |
| **WebBaseErrorEvent** | La clase base para todos los eventos que indica las condiciones de error. |

Los tipos de proveedores disponibles permiten enviar la salida de eventos para el Visor de eventos, SQL Server, Windows Management Instrumentation (WMI) y correo electrónico. Los proveedores configurados previamente y asignaciones de evento reducen la cantidad de trabajo necesario para obtener una salida de evento registrado.

ASP.NET 2.0 utiliza el registro de eventos proveedor-de-fábrica para registrar eventos basados en dominios de aplicación, iniciar y detener, así como para registrar las excepciones no controladas. Esto ayuda a que se tratan algunos de los escenarios básicos. Por ejemplo, supongamos que la aplicación inicia una excepción, pero el usuario no guarda el error y no puede reproducirlo. Con la regla de registro de eventos de forma predeterminada, podrá recopilar la información de excepción y la pila para obtener una mejor idea de qué tipo de error se ha producido. Otro ejemplo es aplicable si la aplicación está perdiendo el estado de sesión. En ese caso, puede buscar en el registro de eventos para determinar si se recicla el dominio de aplicación, y por qué se detuvo el dominio de aplicación en primer lugar.

Además, el sistema de supervisión de estado es extensible. Por ejemplo, puede definir eventos personalizados de Web, se activan ellos dentro de la aplicación y, a continuación, defina una regla para enviar la información del evento a un proveedor como el correo electrónico. Esto le permite asociar fácilmente la instrumentación a los proveedores de supervisión de estado. Como otro ejemplo, podría desencadenar un evento cada vez que se procesa un pedido y se configura una regla que envía cada evento a la base de datos de SQL Server. También puede activar un evento cuando un usuario intenta iniciar sesión varias veces en una fila y establezca el evento a utilizar los proveedores de correo electrónico.

La configuración para los eventos y los proveedores predeterminados se almacena en el archivo Web.config global. El archivo Web.config global almacena toda la configuración basada en Web que se almacenaron en el archivo Machine.config en ASP.NET 1 x. El archivo Web.config global se encuentra en el directorio siguiente:

`%windir%\Microsoft.Net\Framework\v2.0.*\config\Web.config`

El &lt;healthMonitoring&gt; sección del archivo Web.config global proporciona la configuración predeterminada. Puede invalidar esta configuración o configurar su propia configuración implementando la &lt;healthMonitoring&gt; sección en el archivo Web.config para la aplicación.

El &lt;healthMonitoring&gt; sección del archivo Web.config global contiene los siguientes elementos:

| **proveedores** | Contiene los proveedores de configuración de para el Visor de eventos, WMI y SQL Server. |
| --- | --- |
| **eventMappings** | Contiene asignaciones para las distintas clases de WebBase. Puede ampliar esta lista si generar su propia clase de eventos. Generar su propia clase de eventos proporciona una granularidad más fina a través de los proveedores de a que enviar información. Por ejemplo, podría configurar las excepciones no controladas se envíen a SQL Server, al enviar sus propios eventos personalizados al correo electrónico. |
| **reglas** | Proporciona vínculos a eventMappings para el proveedor. |
| **buffering** | Se utiliza con proveedores de correo electrónico y SQL Server para determinar la frecuencia de vaciar los eventos en el proveedor. |

A continuación es un ejemplo de código del archivo Web.config global.

[!code-xml[Main](configuration-and-instrumentation/samples/sample4.xml)]

## <a name="how-to-store-events-to-event-viewer"></a>Cómo almacenar eventos en el Visor de eventos

Como se mencionó anteriormente, el proveedor para registrar eventos en el evento Visor se configura automáticamente en el archivo Web.config global. De forma predeterminada, todos los eventos según **WebBaseErrorEvent** y **WebFailureAuditEvent** se registran. Puede agregar reglas adicionales para registrar información adicional en el registro de eventos. Por ejemplo, si desea registrar todos los eventos (*; es decir,*, todos los eventos según **WebBaseEvent**), puede agregar la regla siguiente al archivo Web.config:

[!code-xml[Main](configuration-and-instrumentation/samples/sample5.xml)]

Esta regla vincularía la **todos los eventos** mapa de eventos para el proveedor de registro de eventos. EventMapping y el proveedor se incluyen en el archivo Web.config global.

## <a name="how-to-store-events-to-sql-server"></a>Cómo almacenar los eventos de SQL Server

Este método usa la **ASPNETDB** base de datos que se genera por Aspnet\_regsql.exe herramienta. El proveedor predeterminado utiliza la cadena de conexión de LocalSqlServer, que usa una basada en el archivo de base de datos en la aplicación\_carpeta de datos o la instancia SQLExpress local de SQL Server. La cadena de conexión de LocalSqlServer y el SqlProvider se configuran en el archivo Web.config global.

La cadena de conexión de LocalSqlServer en el archivo Web.config global tiene este aspecto:

[!code-xml[Main](configuration-and-instrumentation/samples/sample6.xml)]

Si desea utilizar otra instancia de SQL Server, deberá usar Aspnet\_regsql.exe herramienta, que puede encontrarse en el % windir%\Microsoft.Net\Framework\v2.0.\* carpeta. Utilice Aspnet\_regsql.exe herramienta para generar un personalizado **ASPNETDB** en la instancia de SQL Server, la base de datos, a continuación, agregue la cadena de conexión al archivo de configuración de aplicaciones y, a continuación, agregar un proveedor con el nuevo cadena de conexión. Una vez que tenga el **ASPNETDB** crea la base de datos, deberá establecer una regla para vincular un eventMapping para la sqlProvider.

Si usa el valor predeterminado de SqlProvider o configurar su propio proveedor, deberá agregar una regla de vincular el proveedor con un mapa de eventos. La siguiente regla vincula el nuevo proveedor que creó anteriormente para la **todos los eventos** mapa de eventos. Esta regla registrará todos los eventos según **WebBaseEvent** y enviarlos a la MySqlWebEventProvider que va a usar la cadena de conexión MYASPNETDB. El código siguiente agrega una regla para vincular el proveedor con un mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample7.xml)]

Si quisiera enviar solo los errores a SQL Server, podría agregar la siguiente regla:

[!code-xml[Main](configuration-and-instrumentation/samples/sample8.xml)]

## <a name="how-to-forward-events-to-wmi"></a>Cómo reenviar eventos a WMI

También puede reenviar los eventos WMI. El proveedor de WMI se configura automáticamente en el archivo Web.config global de forma predeterminada.

En el ejemplo de código siguiente se agrega una regla para reenviar los eventos WMI:

[!code-xml[Main](configuration-and-instrumentation/samples/sample9.xml)]

Deberá agregar una regla para asociar un eventMapping para el proveedor y también una aplicación de escucha WMI para escuchar los eventos. En el ejemplo de código siguiente se agrega una regla para vincular el proveedor WMI para la **todos los eventos** mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample10.xml)]

## <a name="how-to-forward-events-to-email"></a>Cómo reenviar eventos al correo electrónico

También puede reenviar eventos al correo electrónico. Tenga cuidado sobre qué reglas de eventos asignan a su proveedor de correo electrónico, como se puede enviar involuntariamente usted mismo una gran cantidad de información que pueden ser más adecuados para SQL Server o el registro de eventos. Existen dos proveedores de correo electrónico; SimpleMailWebEventProvider y TemplatedMailWebEventProvider. Cada uno tiene los mismos atributos de configuración, a excepción de los atributos de "plantilla" y "detailedTemplateErrors", que solo están disponibles en el TemplatedMailWebEventProvider.

> [!NOTE]
> Ninguno de estos proveedores de correo electrónico está configurado para usted. Deberá agregarlos al archivo Web.config.


La diferencia principal entre estos proveedores de correo dos electrónico es que SimpleMailWebEventProvider envía mensajes de correo electrónico en una plantilla genérica que no se puede modificar. El archivo Web.config de ejemplo agrega este proveedor de correo electrónico a la lista de proveedores configurados mediante el uso de la siguiente regla:

[!code-xml[Main](configuration-and-instrumentation/samples/sample11.xml)]

También se agrega la siguiente regla para asociar el proveedor de correo electrónico a la **todos los eventos** mapa de eventos:

[!code-xml[Main](configuration-and-instrumentation/samples/sample12.xml)]

## <a name="aspnet-20-tracing"></a>ASP.NET 2.0 seguimiento

Hay tres importantes mejoras en el seguimiento en ASP.NET 2.0.

1. Funcionalidad integrada de traza
2. Acceso mediante programación a los mensajes de seguimiento
3. Seguimiento de nivel de aplicación mejorado

## <a name="integrated-tracing-functionality"></a>Integra la funcionalidad de seguimiento

Ahora puede enrutar los mensajes emitidos por la clase System.Diagnostics.Trace para resultados de traza de ASP.NET y enrutar los mensajes emitidos por el seguimiento de ASP.NET a System.Diagnostics.Trace. También puede reenviar los eventos de instrumentación de ASP.NET a System.Diagnostics.Trace. Esta funcionalidad se proporciona el nuevo **writeToDiagnosticsTrace** atributo de la &lt;seguimiento&gt; elemento. Cuando este valor booleano es true, los mensajes de seguimiento de ASP.NET se reenvían a la infraestructura de seguimiento de System.Diagnostics para su uso por los agentes de escucha registrados para mostrar los mensajes de seguimiento.

## <a name="programmatic-access-to-trace-messages"></a>Acceso mediante programación a los mensajes de seguimiento

ASP.NET 2.0 permite el acceso mediante programación a todos los mensajes de seguimiento a través de la **TraceContextRecord** clase y el **TraceRecords** colección. La manera más eficaz de obtener acceso a los mensajes de seguimiento es registrar un **TraceContextEventHandler** delegado (otra característica nueva en ASP.NET 2.0) para controlar el nuevo **TraceFinished** eventos. A continuación, puede recorrer los mensajes de seguimiento como desee.

Ejemplo de código siguiente muestra esto:

[!code-csharp[Main](configuration-and-instrumentation/samples/sample13.cs)]

En el ejemplo anterior, recorra en iteración la colección TraceRecords y, a continuación, escribe cada mensaje en la secuencia de respuesta.

## <a name="improved-application-level-tracing"></a>Seguimiento de nivel de aplicación mejorado

Se ha mejorado el seguimiento de nivel de aplicación a través de la introducción del nuevo **mostRecent** atributo de la &lt;seguimiento&gt; elemento. Este atributo especifica si se muestra el resultado del seguimiento de nivel de aplicación más reciente y se descartan los datos más antiguos de seguimiento más allá de los límites que se indican mediante requestLimit. Si es false, se muestran los datos de seguimiento para las solicitudes hasta que se alcanza el valor del atributo requestLimit.

## <a name="aspnet-command-line-tools"></a>Herramientas de línea de comandos ASP.NET

Hay varias herramientas de línea de comandos para ayudar en la configuración de ASP.NET. Los desarrolladores de ASP.NET deben estar familiarizados con aspnet\_regiis.exe herramienta. ASP.NET 2.0 proporciona tres otras herramientas de línea de comandos para ayudar en la configuración.

Están disponibles las siguientes herramientas de línea de comandos:

| **Herramienta** | **Use** |
| --- | --- |
| **aspnet\_regiis.exe** | Permite el registro de ASP.NET con IIS. Hay dos versiones de las herramientas de este que se incluyen con ASP.NET 2.0, uno para los sistemas de 32 bits (en la carpeta Framework) y otro para los sistemas de 64 bits (en la carpeta Framework64.) No se instalará la versión de 64 bits en un sistema operativo de 32 bits. |
| **aspnet\_regsql.exe** | La herramienta de registro de SQL Server de ASP.NET se utiliza para crear una base de datos de Microsoft SQL Server para su uso por los proveedores de SQL Server en ASP.NET, o para agregar o quitar opciones de una base de datos existente. Aspnet\_regsql.exe archivo se encuentra en [drive:]\WINDOWS\Microsoft.NET\Framework\versionNumber carpeta en el servidor Web. |
| **aspnet\_regbrowsers.exe** | La herramienta de registro de explorador ASP.NET analiza y compila todas las definiciones de explorador del sistema en un ensamblado e instala al ensamblado en la caché global de ensamblados. La herramienta usa los archivos de definición de explorador (. Los archivos del explorador) desde el subdirectorio de exploradores de .NET Framework. La herramienta puede encontrarse en el directorio %SystemRoot%\Microsoft.NET\Framework\version\. |
| **aspnet\_compiler.exe** | La herramienta de compilación de ASP.NET le permite compilar una aplicación Web ASP.NET, en su lugar o para su implementación en una ubicación de destino como un servidor de producción. Compilación en contexto mejora el rendimiento de la aplicación porque los usuarios finales no encontró un retraso en la primera solicitud a la aplicación mientras se compila la aplicación. |

Dado que aspnet\_regiis.exe herramienta no está familiarizado con ASP.NET 2.0, se describe aquí no.

## <a name="aspnet-sql-server-registration-tool---aspnetregsqlexe"></a>Herramienta de registro de servidor SQL de ASP.NET - aspnet\_regsql.exe

Puede establecer varios tipos de opciones mediante la herramienta de registro de SQL Server de ASP.NET. Puede especificar una conexión de SQL, especifique qué servicios de aplicación ASP.NET usan SQL Server para administrar la información, indicar qué base de datos o tabla se utiliza para la dependencia de caché SQL y agregar o quitar la compatibilidad para utilizar SQL Server para almacenar el estado de sesión y los procedimientos.

Varios servicios de aplicación de ASP.NET se basan en un proveedor para administrar, almacenar y recuperar datos desde un origen de datos. Cada proveedor es específico del origen de datos. ASP.NET incluye un proveedor de SQL Server para las siguientes características ASP.NET:

- Pertenencia (el [SqlMembershipProvider](https://msdn.microsoft.com/library/system.web.security.sqlmembershipprovider.aspx) clase).
- Administración de roles (el [SqlRoleProvider](https://msdn.microsoft.com/library/system.web.security.sqlroleprovider.aspx) clase).
- Perfil (el [SqlProfileProvider](https://msdn.microsoft.com/library/system.web.profile.sqlprofileprovider.aspx) clase).
- Personalización de elementos Web (el [SqlPersonalizationProvider](https://msdn.microsoft.com/library/system.web.ui.webcontrols.webparts.sqlpersonalizationprovider.aspx) clase).
- Eventos de Web (el [SqlWebEventProvider](https://msdn.microsoft.com/library/system.web.management.sqlwebeventprovider.aspx) clase).

Al instalar ASP.NET, el archivo Machine.config del servidor incluye elementos de configuración que especifican los proveedores de SQL Server para cada una de las características de ASP.NET que se basan en un proveedor. Estos proveedores se configuran de forma predeterminada, para conectarse a una instancia de usuario local de SQL Server Express 2005. Si cambia la cadena de conexión predeterminada utilizada por los proveedores, a continuación, para poder usar cualquiera de las características ASP.NET configuradas en la configuración del equipo, debe instalar la base de datos de SQL Server y los elementos de la base de datos para la característica elegida mediante Aspnet\_regsql.exe. Si aún no existe la base de datos que se especifique con la herramienta de registro SQL (aspnetdb será la base de datos predeterminada si no se especifica en la línea de comandos), a continuación, el usuario actual debe tener derechos para crear bases de datos en SQL Server y crear esquemas elementos dentro de una base de datos.

### <a name="sql-cache-dependency"></a>Dependencia de memoria caché de SQL

Una característica avanzada de caché de resultados ASP.NET es la dependencia de caché de SQL. Dependencia de caché de SQL admite dos modos diferentes de operación: uno que usa una implementación de ASP.NET de sondeo de la tabla y un segundo modelo que usa las características de notificación de consulta de SQL Server 2005. La herramienta de registro SQL puede utilizarse para configurar el modo de sondeo de la tabla de la operación.

### <a name="session-state"></a>Estado de la sesión

De forma predeterminada, la información y los valores de estado de sesión se almacenan en memoria en el proceso ASP.NET. Como alternativa, puede almacenar datos de la sesión en una base de datos de SQL Server, donde se puede compartir por varios servidores Web. Si la base de datos que especifique para el estado de sesión con la herramienta de registro SQL no existe, el usuario actual debe tener derechos para crear bases de datos en SQL Server y crear elementos de esquema dentro de una base de datos. Si existe la base de datos, el usuario actual debe tener derechos para crear elementos de esquema en la base de datos existente.

Para instalar la base de datos de estado de sesión en SQL Server, ejecute Aspnet\_regsql.exe herramienta y proporcionar la información con el comando siguiente:

- El nombre del servidor SQL de instancia, mediante la **-S** opción.
- Las credenciales de inicio de sesión para una cuenta que tenga permiso para crear una base de datos en un equipo que ejecuta SQL Server. Utilice la **-E** opción para usar el usuario ha iniciado la sesión o usar el **- U** opción para especificar un identificador de usuario junto con el **-P** opción para especificar una contraseña.
- El **- ssadd** opción de línea de comandos para agregar la base de datos de estado de sesión.

De forma predeterminada, no se puede usar Aspnet\_regsql.exe herramienta para instalar la base de datos de estado de sesión en un equipo que ejecuta SQL Server 2005 Express Edition.

### <a name="the-aspnet-browser-registration-tool---aspnetregbrowsersexe"></a>La herramienta de registro de explorador ASP.NET - aspnet\_regbrowsers.exe

En la versión 1.1 de ASP.NET, el archivo Machine.config contiene una sección denominada &lt;browserCaps&gt;. Esta sección contiene una serie de entradas XML que definen las configuraciones de varios exploradores basándose en una expresión regular. En ASP.NET 2.0, una nueva. BROWSER define los parámetros de un explorador determinado utilizando entradas XML. Agregar información sobre un nuevo explorador agregando una nueva. Archivo de explorador a la carpeta ubicada en %SystemRoot%\Microsoft.NET\Framework\version\CONFIG\Browsers en el sistema.

Dado que una aplicación no está leyendo un archivo .config cada vez que requiere información del explorador, puede crear una nueva. Archivo del explorador y ejecución Aspnet\_regbrowsers.exe para agregar los cambios necesarios para el ensamblado. Esto permite que el servidor de acceso inmediato a la nueva información del explorador por lo que no es necesario apagar las aplicaciones para recoger la información. Una aplicación puede tener acceso a las capacidades del explorador a través de la propiedad del explorador de la solicitud HTTP actual.

Las siguientes opciones están disponibles cuando se ejecuta aspnet\_regbrowser.exe:

| **Opción** | **Descripción** |
| --- | --- |
| **-?** | Muestra el Aspnet\_regbbrowsers.exe texto de ayuda en la ventana de comandos. |
| **-i** | Crea el ensamblado de funciones de explorador en tiempo de ejecución y lo instala en la caché global de ensamblados. |
| **-u** | Desinstala el ensamblado de funciones de explorador en tiempo de ejecución de la caché global de ensamblados. |

## <a name="the-aspnet-compilation-tool---aspnetcompilerexe"></a>La herramienta de compilación de ASP.NET - aspnet\_compiler.exe

La herramienta de compilación de ASP.NET puede usarse de dos formas generales: para la compilación para la implementación, donde se especifica un directorio de salida de destino y la compilación en contexto.

### <a name="compiling-an-application-in-placehttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Compilar una aplicación en su lugar](https://msdn.microsoft.com/library/ms229863.aspx)

La herramienta de compilación de ASP.NET puede compilar una aplicación en su lugar, es decir, imita el comportamiento de realización de varias solicitudes a la aplicación, lo cual provoca una compilación normal. Los usuarios de un sitio precompilado no experimentarán una demora causada por la compilación de la página en la primera solicitud.

Al precompilar un sitio en su lugar, se aplican los siguientes elementos:

- El sitio retiene sus archivos y la estructura de directorios.
- Debe tener los compiladores para todos los lenguajes de programación utilizados por el sitio en el servidor.
- Si los archivos se produce un error de compilación, todo el sitio se produce un error compilación.

También puede volver a compilar una aplicación en su lugar después de agregar nuevos archivos de origen. La herramienta compila los archivos nuevos o modificados a menos que incluya el **- c** opción.

> [!NOTE]
> Compilación de una aplicación que contiene una aplicación anidada no compila la aplicación anidada. La aplicación anidada se debe compilar por separado.


### <a name="compiling-an-application-for-deploymenthttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Compilar una aplicación para la implementación](https://msdn.microsoft.com/library/ms229863.aspx)

Compilar una aplicación para la implementación (compilación de una ubicación de destino) especificando el parámetro targetDir. TargetDir puede ser la ubicación final de la aplicación Web o la aplicación compilada se puede implementar más. Mediante el **-u** opción compila la aplicación de tal forma que puede realizar cambios en algunos archivos de la aplicación compilada sin recompilarla. ASPNET\_compiler.exe hace una distinción entre los tipos de archivos estáticos y dinámicos y se controla de manera diferente al crear la aplicación resultante.

- Tipos de archivos estáticos son aquellos que no tienen un compilador asociado o que generan un proveedor, tales como archivos cuya con nombre tienen extensiones como .css, .gif, .htm, .html, .jpg, .js y así sucesivamente. Estos archivos simplemente se copian en la ubicación de destino, con sus posiciones relativas en la estructura de directorios que se conservan.
- Tipos de archivos dinámicos son aquellos que tienen un compilador asociado o generan un proveedor, incluidos los archivos con extensiones de nombre de archivo específicos de ASP.NET como .asax, .ascx, .ashx, .aspx, .browser,. master y así sucesivamente. La herramienta de compilación de ASP.NET genera ensamblados a partir de estos archivos. Si el **-u** opción se omite, la herramienta también crea archivos con la extensión de nombre de archivo. COMPILED que los archivos de origen se asignan a su ensamblado. Para asegurarse de que se conserva la estructura de directorios de origen de la aplicación, la herramienta genera archivos de marcador de posición en las ubicaciones correspondientes en la aplicación de destino.

Debe usar el **-u** opción para indicar que se puede modificar el contenido de la aplicación compilada. En caso contrario, las modificaciones posteriores se omiten o provocan errores de tiempo de ejecución.

En la tabla siguiente se describe cómo la compilación de ASP.NET herramienta identificadores diferentes tipos de archivo cuando el **-u** se incluye la opción.

| **Tipo de archivo** | **Acción del compilador** |
| --- | --- |
| .ascx, .aspx, .master | Estos archivos se dividen en marcado y código fuente, lo que incluye archivos de código subyacente y cualquier código que se encierra entre &lt;script runat = "server"&gt; elementos. Código fuente se compila en ensamblados con nombres que se derivan de un algoritmo hash, y los ensamblados se colocan en el directorio Bin. Cualquier código en línea, es decir, el código encerrado entre el **&lt; %** y **% &gt;** corchetes, se incluye con marcado y no compilados. Nuevos archivos con el mismo nombre que los archivos de origen se crea para contener el marcado y se colocan en los directorios de salida correspondiente. |
| .ashx, .asmx | Estos archivos no se compilan y se mueven a los directorios de salida tal cual y no compilados. Si desea que el código del controlador compilado, colocar el código en archivos de código fuente en la aplicación\_directorio de código. |
| . cs, .vb, .jsl, .cpp (sin incluir los archivos de código subyacente para los tipos de archivo enumerados anteriormente) | Estos archivos se compilan y se incluye como recurso en los ensamblados que hacen referencia a ellos. Archivos de código fuente no se copian en el directorio de salida. Si no se hace referencia a un archivo de código, no se compila. |
| Tipos de archivo personalizados | Estos archivos no se compilan. Estos archivos se copian en los directorios de salida correspondiente. |
| Los archivos de código en la aplicación de origen\_subdirectorio de código | Estos archivos se compilan en ensamblados y se coloca en el directorio Bin. |
| archivos .resx y .resource en la aplicación\_GlobalResources subdirectorio | Estos archivos se compilan en ensamblados y se coloca en el directorio Bin. No hay ninguna aplicación\_GlobalResources subdirectorio se crea en el directorio de salida principal y no hay archivos .resx o .resources ubicados en el directorio de origen se copian en los directorios de salida. |
| archivos .resx y .resource en la aplicación\_LocalResources subdirectorio | Estos archivos no se compilan y se copian en los directorios de salida correspondiente. |
| en la aplicación de los archivos .skin\_subdirectorio de temas | Los archivos .skin y tema de estáticos no se compilan y se copian en los directorios de salida correspondiente. |
| archivo Web.config estático .browser tipos ensamblados ya está presentes en el directorio Bin | Estos archivos se copian tal como están en los directorios de salida. |

En la tabla siguiente se describe cómo la compilación de ASP.NET herramienta identificadores diferentes tipos de archivo cuando el **-u** se omite la opción.

| **Tipo de archivo** | **Acción del compilador** |
| --- | --- |
| .aspx, .asmx, .ashx, .master | Estos archivos se dividen en marcado y código fuente, lo que incluye archivos de código subyacente y cualquier código que se encierra entre &lt;script runat = "server"&gt; elementos. Código fuente se compila en ensamblados con nombres que se derivan de un algoritmo hash. Los ensamblados resultantes se colocan en el directorio Bin. Cualquier código en línea, es decir, el código encerrado entre el **&lt; %** y **% &gt;** corchetes, se incluye con marcado y no compilados. El compilador crea nuevos archivos para contener el marcado con el mismo nombre que los archivos de origen. Estos archivos resultantes se colocan en el directorio Bin. El compilador también crea archivos con el mismo nombre que los archivos de origen, pero con la extensión. COMPILED que contienen información de asignación. El archivo. Archivos COMPILADOS se colocan en los directorios de salida correspondiente a la ubicación original de los archivos de origen. |
| .ascx | Estos archivos se dividen en marcado y código fuente. Código fuente se compilan en ensamblados y se coloca en el directorio Bin, con nombres que se derivan de un algoritmo hash. Se genera ningún archivo de marcado. |
| . cs, .vb, .jsl, .cpp (sin incluir los archivos de código subyacente para los tipos de archivo enumerados anteriormente) | Código fuente que se hace referencia a los ensamblados generados a partir de archivos .aspx, .ashx o .ascx se compilan en ensamblados y se colocan en el directorio Bin. Se copia ningún archivo de origen. |
| Tipos de archivo personalizados | Estos archivos se compilan como archivos dinámicos. Según el tipo de archivo que se basan, el compilador puede colocar los archivos de asignación en los directorios de salida. |
| Los archivos en la aplicación\_subdirectorio de código | Archivos de código fuente de este subdirectorio se compilan en ensamblados y se colocan en el directorio Bin. |
| Los archivos en la aplicación\_GlobalResources subdirectorio | Estos archivos se compilan en ensamblados y se coloca en el directorio Bin. No hay ninguna aplicación\_GlobalResources subdirectorio se crea en el directorio de salida principal. Si el archivo de configuración especifica appliesTo = "All", los archivos .resx y .resources se copian en los directorios de salida. No se copian si hace referencia a un [BuildProvider](https://msdn.microsoft.com/library/system.web.configuration.buildprovider.aspx). |
| archivos .resx y .resource en la aplicación\_LocalResources subdirectorio | Estos archivos se compilan en ensamblados con nombres únicos y se coloca en el directorio Bin. No hay archivos .resx o .resource se copian en los directorios de salida. |
| en la aplicación de los archivos .skin\_subdirectorio de temas | Los temas se compilan en ensamblados y se colocan en el directorio Bin. Archivos de código auxiliar se crean para archivos .skin y se colocan en el directorio de salida correspondiente. Los archivos estáticos (como .css) se copian en los directorios de salida. |
| archivo Web.config estático .browser tipos ensamblados ya está presentes en el directorio Bin | Estos archivos se copian tal cual en el directorio de salida. |

### <a name="fixed-assembly-nameshttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Nombres de ensamblado fija](https://msdn.microsoft.com/library/ms229863.aspx##)

Algunos escenarios, como la implementación de una aplicación Web mediante el instalador de Windows de MSI, requieren el uso de nombres de archivo coherentes y contenido, así como las estructuras de directorios coherente para identificar los ensamblados o los valores de configuración para las actualizaciones. En esos casos, puede usar el **- fixednames** opción para especificar que la herramienta de compilación de ASP.NET debe compilar un ensamblado para cada archivo de código fuente en lugar de usar where varias páginas se compilan en ensamblados. Esto puede provocar un gran número de ensamblados, por lo que si le preocupa con escalabilidad debe usar esta opción con precaución.

### <a name="strong-name-compilationhttpsmsdnmicrosoftcomlibraryms229863aspx"></a>[Compilación de nombre seguro](https://msdn.microsoft.com/library/ms229863.aspx##)

El **- aptca**, **- delaysign**, **- keycontainer** y **- keyfile** se proporcionan opciones para que pueda usar Aspnet\_ Compiler.exe encarecidamente crear ensamblados con nombre sin usar el [herramienta nombre seguro (Sn.exe)](https://msdn.microsoft.com/library/k5b5tt23.aspx) por separado. Estas opciones corresponden respectivamente a **AllowPartiallyTrustedCallersAttribute**, **AssemblyDelaySignAttribute**, **AssemblyKeyNameAttribute**y  **AssemblyKeyFileAttribute**.

Descripción de estos atributos está fuera del ámbito de este curso.

## <a name="labs"></a>Laboratorios

Cada una de las siguientes prácticas se basa en los laboratorios anteriores. Deberá hacerlo en orden.

## <a name="lab-1-using-the-configuration-api"></a>Laboratorio 1: Uso de la API de configuración

1. Crear un nuevo sitio Web llamado *mod9lab*.
2. Agregar un nuevo archivo de configuración Web al sitio.
3. Agregue lo siguiente al archivo web.config:


[!code-xml[Main](configuration-and-instrumentation/samples/sample14.xml)]

Esto garantizará que tiene permiso para guardar los cambios en el archivo web.config.

1. Agregar un nuevo control de etiqueta a Default.aspx y cambie el identificador a **lblDebugStatus**.
2. Agregar un nuevo control de botón a Default.aspx.
3. Cambiar el identificador del control de botón a **btnToggleDebug** y el texto para **Alternar estado de depuración**.
4. Abra la vista de código para el archivo de código subyacente de Default.aspx y agregue un **mediante** instrucción para **System.Web.Configuration** como sigue:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample15.cs)]

1. Agregue dos variables privadas a la clase y una página\_Init (método) tal como se muestra a continuación:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample16.cs)]

1. Agregue el código siguiente a la página\_carga:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample17.cs)]

1. Guarde y examinar default.aspx. Tenga en cuenta que el control de etiqueta muestra el estado de depuración actual.
2. Haga doble clic en el control de botón en el diseñador y agregue el siguiente código para el evento Click para el control de botón:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample18.cs)]

1. Guarde y examinar default.aspx y haga clic en el botón.
2. Abra el archivo web.config después de cada botón, haga clic en y observe el **depurar** atributo en el &lt;compilación&gt; sección.

## <a name="lab-2-logging-application-restarts"></a>Práctica 2: Registro reinicios de la aplicación

En este laboratorio, creará el código que le permitirá activar o desactivar el registro de recompilaciones en el Visor de eventos, inicios y cierres de aplicación.

1. Agregue un DropDownList a default.aspx y cambie el identificador a ddlLogAppEvents.
2. Establecer el **AutoPostBack** propiedad DropDownList para **true**.
3. Agregue tres elementos a la colección de elementos de la DropDownList. Realizar el **texto** del primer elemento *Select Value* y el valor -1. Realizar el **texto** y **valor** del segundo elemento **True** y **texto** y **valor** del tercer elemento **False**.
4. Agregar una nueva etiqueta a default.aspx. Cambie el identificador a **lblLogAppEvents**.
5. Abra la vista de código subyacente para default.aspx y agregue una nueva declaración de una variable de tipo HealthMonitoringSection tal como se muestra a continuación:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample19.cs)]

1. Agregue el código siguiente en el código existente en la página\_Init:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample20.cs)]

1. Haga doble clic en DropDownList y agregue el código siguiente al evento SelectedIndexChanged:


[!code-csharp[Main](configuration-and-instrumentation/samples/sample21.cs)]

1. Examinar default.aspx.
2. Establezca la lista desplegable **False**.
3. Borrar el registro de aplicación en el Visor de eventos.
4. Haga clic en el botón para cambiar el atributo de depuración para la aplicación.
5. Actualizar el registro de aplicación en el Visor de eventos. 

    1. ¿Se registraron los eventos?
    2. ¿Por qué o por qué no?
6. Establezca la lista desplegable **es True.**
7. Haga clic en el botón para alternar el atributo de depuración para la aplicación.
8. Actualizar el inicio de sesión de aplicación del Visor de eventos. 

    1. ¿Se registraron los eventos?
    2. ¿Cuál fue el motivo del apagado de la aplicación?
9. Experimentar con activar y desactivar el registro y observe los cambios realizados en el archivo web.config.

## <a name="more-information"></a>Más información:

ASP.NET del 2.0 modelo de proveedor le permite crear sus propios proveedores de instrumentación de aplicaciones no solo, pero para muchos otros usos, así como la pertenencia a grupos, perfiles, etcetera. Para obtener información detallada sobre cómo escribir un proveedor personalizado para registrar eventos de aplicación en un archivo de texto, visite [este vínculo](https://msdn.microsoft.com/library/default.asp?url=/library/dnaspp/html/ASPNETProvMod_Prt6.asp).
