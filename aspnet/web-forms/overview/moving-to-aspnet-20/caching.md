---
uid: web-forms/overview/moving-to-aspnet-20/caching
title: Almacenamiento en caché | Microsoft Docs
author: microsoft
description: Una descripción del almacenamiento en caché es importante para una aplicación ASP.NET de buen rendimiento. ASP.NET 1.x que ofrecen tres opciones distintas para el almacenamiento en caché; almacenamiento en caché, de salida...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 2bb109d2-e299-46ea-9054-fa0263b59165
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/caching
msc.type: authoredcontent
ms.openlocfilehash: 4f52a88680db54de6271b17bd52cbdace66425e9
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37387704"
---
<a name="caching"></a>Almacenamiento en memoria caché
====================
por [Microsoft](https://github.com/microsoft)

> Una descripción del almacenamiento en caché es importante para una aplicación ASP.NET de buen rendimiento. ASP.NET 1.x que ofrecen tres opciones distintas para el almacenamiento en caché; caché de resultados, almacenamiento en caché y la API de caché.


Una descripción del almacenamiento en caché es importante para una aplicación ASP.NET de buen rendimiento. ASP.NET 1.x que ofrecen tres opciones distintas para el almacenamiento en caché; caché de resultados, almacenamiento en caché y la API de caché. ASP.NET 2.0 ofrece las tres de estos métodos, pero agrega algunas importantes características adicionales. Hay varias nuevas dependencias de caché y los desarrolladores ahora tienen la opción de crear dependencias de caché personalizadas también. La configuración de almacenamiento en caché también se han mejorado considerablemente en ASP.NET 2.0.

## <a name="new-features"></a>Características nuevas

## <a name="cache-profiles"></a>Perfiles de memoria caché

Perfiles de caché permiten a los desarrolladores definir la configuración de caché específica que, a continuación, se puede aplicar a páginas individuales. Por ejemplo, si tiene algunas páginas que deben haber expirado de memoria caché después de 12 horas, puede crear fácilmente un perfil de caché que se puede aplicar a esas páginas. Para agregar un nuevo perfil de caché, use el &lt;outputCacheSettings&gt; sección del archivo de configuración. Por ejemplo, a continuación es la configuración de un perfil de caché denominado *twoday* que configura una duración de caché de 12 horas.

[!code-xml[Main](caching/samples/sample1.xml)]

Para aplicar este perfil de caché en una página concreta, use el atributo CacheProfile de la directiva @ OutputCache tal como se muestra a continuación:

[!code-aspx[Main](caching/samples/sample2.aspx)]

## <a name="custom-cache-dependencies"></a>Dependencias de caché personalizadas

Los desarrolladores de ASP.NET 1.x también lloramos para las dependencias de caché personalizada. En ASP.NET 1.x, la clase CacheDependency selló que los desarrolladores impedía de derivar sus propias clases. En ASP.NET 2.0, se quita esa limitación y los desarrolladores son gratuitos desarrollar sus propias dependencias de caché personalizadas. La clase CacheDependency permite la creación de una dependencia de caché personalizada basada en archivos, directorios o claves de caché.

Por ejemplo, el código siguiente crea una nueva dependencia de caché personalizada basada en un archivo llamado stuff.xml ubicado en la raíz de la aplicación Web:

[!code-csharp[Main](caching/samples/sample3.cs)]

En este escenario, cuando el archivo stuff.xml cambia, se invalida el elemento en caché.

También es posible crear una dependencia de caché personalizadas mediante las claves de caché. Con este método, la eliminación de la clave de caché, invalidará los datos en caché. Esto se ilustra en el siguiente ejemplo:

[!code-csharp[Main](caching/samples/sample4.cs)]

Para invalidar el elemento que se insertó anteriormente, basta con quitar el elemento que se insertó en la memoria caché para que actúe como la clave de caché.

[!code-csharp[Main](caching/samples/sample5.cs)]

Tenga en cuenta que la clave del elemento que actúa como la clave de caché debe ser el mismo que el valor agregado a la matriz de claves de caché.

## <a name="polling-based-sql-cache-dependenciesemalso-called-table-based-dependenciesem"></a>Dependencias de caché de SQL basados en sondeos<em>(también denominado dependencias basadas en la tabla)</em>

SQL Server 7 y 2000 utilizan el modelo basado en sondeo de dependencias de caché de SQL. El modelo basado en sondeo usa un desencadenador en una tabla de base de datos que se desencadena cuando cambian los datos de la tabla. Las actualizaciones que desencadenen un **changeId** campo en la tabla de notificación que ASP.NET comprueba periódicamente. Si el **changeId** campo se ha actualizado, ASP.NET sepa que los datos han cambiado y no invalida los datos almacenados en caché.

> [!NOTE]
> SQL Server 2005 también puede usar el modelo basado en sondeo, pero dado que el modelo basado en sondeo no es el modelo más eficiente, es aconsejable utilizar un modelo basado en consultas (descrito más adelante) con SQL Server 2005.


En orden para una dependencia de caché SQL mediante el modelo basado en sondeo funcione correctamente, las tablas deben tener habilitadas las notificaciones. Esto puede realizarse mediante programación utilizando la clase SqlCacheDependencyAdmin o mediante el uso de aspnet\_regsql.exe utilidad.

La línea de comandos siguiente registra la tabla Products en la base de datos de Northwind que se encuentra en una instancia de SQL Server denominada *dbase* dependencia de caché de SQL.

[!code-console[Main](caching/samples/sample6.cmd)]

El siguiente es una explicación de los modificadores de línea de comandos utilizados en el comando anterior:

| **Conmutador de línea de comandos** | **Propósito** |
| --- | --- |
| -S *server* | Especifica el nombre del servidor. |
| -ed | Especifica que se debe habilitar la base de datos para la dependencia de caché SQL. |
| -d *base de datos\_nombre* | Especifica el nombre de base de datos que debe habilitarse para la dependencia de caché SQL. |
| -E | Especifica que aspnet\_regsql debe utilizar la autenticación de Windows al conectarse a la base de datos. |
| -et | Especifica que se va a habilitar una tabla de base de datos para la dependencia de caché SQL. |
| -t *tabla\_nombre* | Especifica el nombre de la tabla de base de datos para habilitar la dependencia de caché de SQL. |

> [!NOTE]
> Hay otros modificadores disponibles para aspnet\_regsql.exe. ¿Para obtener una lista completa, ejecute aspnet\_regsql.exe-? desde una línea de comandos.


Cuando este comando ejecuta los siguientes cambios se realizan en la base de datos de SQL Server:

- Un **AspNet\_SqlCacheTablesForChangeNotification** se agrega la tabla. Esta tabla contiene una fila por cada tabla en la base de datos para el que se ha habilitado una dependencia de caché SQL.
- Los siguientes procedimientos almacenados se crean dentro de la base de datos:


| AspNet\_SqlCachePollingStoredProcedure | Consulta AspNet\_SqlCacheTablesForChangeNotification tabla y devuelve todas las tablas habilitadas para la dependencia de caché de SQL y el valor de changeId para cada tabla. Este procedimiento almacenado se utiliza para el sondeo para determinar si los datos han cambiado. |
| --- | --- |
| AspNet\_SqlCacheQueryRegisteredTablesStoredProcedure | Devuelve todas las tablas habilitadas para la dependencia de caché SQL consultando AspNet\_SqlCacheTablesForChangeNotification tabla y devuelve todas las tablas habilitadas para SQL de la dependencia de caché. |
| AspNet\_SqlCacheRegisterTableStoredProcedure | Registra una tabla para la dependencia de caché SQL mediante la adición de la entrada necesaria en la tabla de notificación y agrega el desencadenador. |
| AspNet\_SqlCacheUnRegisterTableStoredProcedure | Anula el registro de una tabla para la dependencia de caché SQL mediante la eliminación de la entrada en la tabla de notificación y elimina el desencadenador. |
| AspNet\_SqlCacheUpdateChangeIdStoredProcedure | Actualiza la tabla de notificación al incrementar el changeId para la tabla modificada. ASP.NET usa este valor para determinar si los datos han cambiado. Como se indica a continuación, este procedimiento almacenado se ejecuta el desencadenador que se crea cuando se habilita en la tabla. |


- Llama un desencadenador de SQL Server ***tabla\_nombre *\_AspNet\_SqlCacheNotification\_desencadenador** se crea para la tabla. Este desencadenador ejecuta AspNet\_SqlCacheUpdateChangeIdStoredProcedure cuando se realiza una INSERCIÓN, actualización o eliminación en la tabla.
- Un rol de SQL Server denominada **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** se agrega a la base de datos.

El **aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess** rol de SQL Server tiene permisos de ejecución para AspNet\_SqlCachePollingStoredProcedure. Para el modelo de sondeo para que funcione correctamente, debe agregar su cuenta de proceso para aspnet\_ChangeNotification\_ReceiveNotificationsOnlyAccess rol. Aspnet\_regsql.exe herramienta no hará esto por usted.

### <a name="configuring-polling-based-sql-cache-dependencies"></a>Configurar dependencias de caché de SQL basados en sondeos

Hay varios pasos que son necesarios para configurar las dependencias de caché SQL basados en sondeos. El primer paso es habilitar la base de datos y la tabla según se explicó anteriormente. Una vez completado este paso, el resto de la configuración es la siguiente:

- Configuración del archivo de configuración de ASP.NET.
- Configuración de SqlCacheDependency

### <a name="configuring-the-aspnet-configuration-file"></a>Configuración del archivo de configuración de ASP.NET

Además de agregar una cadena de conexión como se describe en un módulo anterior, también debe configurar un &lt;caché&gt; elemento con un &lt;sqlCacheDependency&gt; elemento tal y como se muestra a continuación:

[!code-xml[Main](caching/samples/sample7.xml)]

Esta configuración permite que una dependencia de caché SQL en el *pubs* base de datos. Tenga en cuenta que el pollTime atributo en el &lt;sqlCacheDependency&gt; elemento el valor predeterminado es 60000 milisegundos o 1 minuto. (Este valor no puede ser inferior a 500 milisegundos). En este ejemplo, el &lt;agregar&gt; elemento agrega una nueva base de datos e invalida el pollTime, si se establece en 9000000 milisegundos.

#### <a name="configuring-the-sqlcachedependency"></a>Configuración de SqlCacheDependency

El siguiente paso es configurar SqlCacheDependency. La manera más fácil de hacerlo es especificar el valor del atributo SqlDependency en la directiva @ Outcache como sigue:

[!code-aspx[Main](caching/samples/sample8.aspx)]

En la directiva @ OutputCache anterior, una dependencia de caché SQL está configurada para el *autores* de tabla en la *pubs* base de datos. Se pueden configurar varias dependencias, sepárelos con punto y coma de este modo:

[!code-aspx[Main](caching/samples/sample9.aspx)]

Otro método de configuración de SqlCacheDependency es hacerlo mediante programación. El código siguiente crea una nueva dependencia de caché SQL en el *autores* de tabla en la *pubs* base de datos.

[!code-csharp[Main](caching/samples/sample10.cs)]

Una de las ventajas de definir mediante programación la dependencia de caché SQL es que puede controlar las excepciones que se produzcan. Por ejemplo, si se intenta definir una dependencia de caché SQL para una base de datos que no se ha habilitado para la notificación, un **DatabaseNotEnabledForNotificationException** se producirá la excepción. En ese caso, puede intentar volver a habilitar la base de datos para las notificaciones mediante una llamada a la **SqlCacheDependencyAdmin.EnableNotifications** método y pásele el nombre de la base de datos.

Del mismo modo, si se intenta definir una dependencia de caché SQL para una tabla que no se ha habilitado para la notificación, un **TableNotEnabledForNotificationException** se iniciará. A continuación, puede llamar a la **SqlCacheDependencyAdmin.EnableTableForNotifications** método pasándole el nombre de la base de datos y el nombre de la tabla.

Ejemplo de código siguiente muestra cómo configurar correctamente el control de excepciones al configurar una dependencia de caché SQL.

[!code-csharp[Main](caching/samples/sample11.cs)]

Más información: [https://msdn.microsoft.com/library/t9x04ed2.aspx](https://msdn.microsoft.com/library/t9x04ed2.aspx)

## <a name="query-based-sql-cache-dependencies-sql-server-2005-only"></a>Dependencias de caché basadas en consultas SQL (sólo en SQL Server 2005)

Cuando se usa SQL Server 2005 para la dependencia de caché SQL, el modelo basado en sondeo no es necesario. Cuando se usa con SQL Server 2005, las dependencias de caché SQL se comunican directamente a través de conexiones de SQL a la instancia de SQL Server (es necesaria ninguna configuración adicional) con las notificaciones de consulta de SQL Server 2005.

La manera más sencilla de habilitar la notificación de consulta es hacerlo mediante declaración estableciendo el **SqlCacheDependency** atributo del objeto de origen de datos a **CommandNotification** y establecer el **EnableCaching** atributo **true**. Con este método, se requiere ningún código. Si el resultado de un comando ejecuta en los datos de los cambios del origen, invalidará los datos de la memoria caché.

El ejemplo siguiente configura un control de origen de datos para la dependencia de caché SQL:

[!code-aspx[Main](caching/samples/sample12.aspx)]

En este caso, si la consulta especificada en el **SelectCommand** devuelve un resultado diferente a la hizo inicialmente, se invalidan los resultados de la que se almacenan en caché.

También puede especificar que todos los orígenes de datos esté habilitada para las dependencias de caché SQL estableciendo el **SqlDependency** atributo de la **@ OutputCache** la directiva a **CommandNotification** . El ejemplo siguiente ilustra esto.

[!code-aspx[Main](caching/samples/sample13.aspx)]

> [!NOTE]
> Para obtener más información sobre las notificaciones de consulta en SQL Server 2005, vea los libros en pantalla de SQL Server.


Otro método de configuración de una dependencia de caché basada en consulta SQL es hacerlo mediante programación con la clase SqlCacheDependency. Ejemplo de código siguiente muestra cómo se consigue.

[!code-csharp[Main](caching/samples/sample14.cs)]

Más información: [https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp](https://msdn.microsoft.com/library/default.asp?url=/library/enus/dnvs05/html/querynotification.asp)

## <a name="post-cache-substitution"></a>Substitución posterior a la caché

Almacenamiento en caché una página puede aumentar drásticamente el rendimiento de una aplicación Web. Sin embargo, en algunos casos necesita la mayor parte de la página en la memoria caché y algunos fragmentos de la página deben ser dinámicos. Por ejemplo, si crea una página de noticias que es totalmente estática durante períodos de tiempo, puede establecer la página completa en la memoria caché. Si desea incluir un anuncio que se puede cambiar en cada solicitud de página, la parte de la página que contiene el anuncio debe ser dinámico. Para que pueda almacenar en caché una página pero sustituir algún contenido dinámicamente, puede utilizar la sustitución posterior a la caché de ASP.NET. Con la sustitución posterior a la caché, toda la página está almacenado en caché con partes específicas marcadas como exento de almacenamiento en caché de salida. En el ejemplo de los anuncios, el control AdRotator permite aprovechar las ventajas de la sustitución posterior a la caché para que los anuncios creados dinámicamente para cada usuario y para cada actualización de la página.

Hay tres maneras de implementar la sustitución posterior a la caché:

- De forma declarativa, mediante el control de sustitución.
- Mediante programación, con la API de control de sustitución.
- Implícitamente, mediante el control AdRotator.

### <a name="substitution-control"></a>Control de sustitución

El control de sustitución de ASP.NET especifica una sección de una página almacenada en caché que se crean de forma dinámica en lugar de almacenar en caché. Coloque un control de sustitución en la ubicación en la página donde desea que aparezca el contenido dinámico. En tiempo de ejecución, el control de sustitución llama a un método que se especifique con la propiedad MethodName. El método debe devolver una cadena, que, a continuación, reemplaza el contenido del control de sustitución. El método debe ser un método estático en el control de página o control de usuario que lo contiene. Uso del control de sustitución hace que se almacena en caché del lado cliente para que cambien a almacenamiento en caché de servidor, para que la página no se almacenarán en el cliente. Esto garantiza que las solicitudes futuras a la página de llamar al método nuevo para generar contenido dinámico.

### <a name="substitution-api"></a>API de sustitución

Para crear contenido dinámico para una página almacenada en caché mediante programación, puede llamar a la [WriteSubstitution](https://msdn.microsoft.com/library/system.web.httpresponse.writesubstitution.aspx) método en el código de página, pasándole el nombre de un método como parámetro. El método que controla la creación del contenido dinámico toma un único [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx) parámetro y devuelve una cadena. La cadena devuelta es el contenido que se sustituirá en la ubicación especificada. Una ventaja de una llamada al método WriteSubstitution en lugar de usar el control de sustitución mediante declaración es que puede llamar a un método de cualquier objeto en lugar de llamar a un método estático de la página o el objeto UserControl.

Una llamada al método WriteSubstitution hace que se almacena en caché del lado cliente para que cambien a almacenamiento en caché de servidor, para que la página no se almacenarán en el cliente. Esto garantiza que las solicitudes futuras a la página de llamar al método nuevo para generar contenido dinámico.

### <a name="adrotator-control"></a>Control AdRotator

AdRotator implementa el control de servidor admite internamente para la sustitución posterior a la caché. Si coloca un control AdRotator en la página, representará anuncios únicos en cada solicitud, independientemente de si se almacena en caché la página primaria. Como resultado, una página que incluye un control AdRotator es sólo en caché del lado servidor.

## <a name="controlcachepolicy-class"></a>Clase ControlCachePolicy

La clase ControlCachePolicy permite el control de fragmento de almacenamiento en caché mediante los controles de usuario mediante programación. ASP.NET incrusta los controles de usuario dentro de un [BasePartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.aspx) instancia. La clase BasePartialCachingControl representa un control de usuario que tiene habilitada la memoria caché de salida.

Al acceder a la [BasePartialCachingControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.basepartialcachingcontrol.cachepolicy.aspx) propiedad de un [PartialCachingControl](https://msdn.microsoft.com/library/system.web.ui.partialcachingcontrol.aspx) control, siempre recibirá un objeto ControlCachePolicy válido. Sin embargo, si tiene acceso a la [UserControl.CachePolicy](https://msdn.microsoft.com/library/system.web.ui.usercontrol.cachepolicy.aspx) propiedad de un [UserControl](https://msdn.microsoft.com/library/system.web.ui.usercontrol.aspx) control, recibir un objeto ControlCachePolicy válido sólo si el control de usuario ya está contenido en un Control BasePartialCachingControl. Si no se ajusta, el objeto ControlCachePolicy devuelto por la propiedad producirá excepciones cuando se intenta manipular, porque no tiene un BasePartialCachingControl asociado. Para determinar si una instancia del control de usuario admite el almacenamiento en caché sin generar excepciones, inspeccione la [SupportsCaching](https://msdn.microsoft.com/library/system.web.ui.controlcachepolicy.supportscaching.aspx) propiedad.

Uso de la clase ControlCachePolicy es una de varias maneras de que habilitar la caché de resultados. En la lista siguiente se describe los métodos que puede usar para habilitar el almacenamiento en caché de salida:

- Use la [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) directiva para habilitar el almacenamiento en caché en escenarios declarativos de salida.
- Use la [PartialCachingAttribute](https://msdn.microsoft.com/library/system.web.ui.partialcachingattribute.aspx) atributo para habilitar el almacenamiento en caché para un control de usuario en un archivo de código subyacente.
- Utilice la clase ControlCachePolicy para especificar la configuración de caché en escenarios de programación en el que está trabajando con BasePartialCachingControl instancias que se han habilitado para caché mediante uno de los métodos anteriores y carga dinámicamente mediante el [System.Web.UI.TemplateControl.LoadControl](https://msdn.microsoft.com/library/system.web.ui.templatecontrol.loadcontrol.aspx) método.

Se puede manipular correctamente una instancia ControlCachePolicy solo entre las fases de Init y PreRender del ciclo de vida del control. Si se modifica un objeto ControlCachePolicy después de la fase de preprocesamiento, ASP.NET genera una excepción porque los cambios realizados después de representa el control no afectan realmente a la configuración de la caché (un control se almacena en caché durante la fase de representación). Por último, una instancia del control de usuario (y, por tanto, su objeto ControlCachePolicy) solo está disponibles para la manipulación mediante programación cuando se representa realmente.

## <a name="changes-to-caching-configuration---the-ltcachinggt-element"></a>Los cambios en la configuración de almacenamiento en caché: el &lt;almacenamiento en caché&gt; elemento

Hay varios cambios en la configuración de almacenamiento en caché en ASP.NET 2.0. El &lt;almacenamiento en caché&gt; elemento es nuevo en ASP.NET 2.0 y le permite realizar cambios de configuración de almacenamiento en caché en el archivo de configuración. Los siguientes atributos están disponibles.

| **Element** | **Descripción** |
| --- | --- |
| **cache** | Elemento opcional. Define la configuración de la caché global de la aplicación. |
| **outputCache** | Elemento opcional. Especifica la configuración de caché de resultados de toda la aplicación. |
| **outputCacheSettings** | Elemento opcional. Especifica la configuración de caché de resultados que se puede aplicar a las páginas de la aplicación. |
| **sqlCacheDependency** | Elemento opcional. Configura las dependencias de caché SQL para una aplicación ASP.NET. |

### <a name="the-ltcachegt-element"></a>El &lt;caché&gt; elemento

Los siguientes atributos están disponibles en el &lt;caché&gt; elemento:

| **Attribute** | **Descripción** |
| --- | --- |
| **disableMemoryCollection** | Opcional **booleano** atributo. Obtiene o establece un valor que indica si la colección de la memoria caché que se produce cuando el equipo está bajo presión de memoria está deshabilitada. |
| **disableExpiration** | Opcional **booleano** atributo. Obtiene o establece un valor que indica si la expiración de caché está deshabilitada. Cuando está deshabilitado, los elementos en caché no caducan y eliminación de registros obsoletos en segundo plano de elementos de caché expirados no se produce. |
| **privateBytesLimit** | Opcional **Int64** atributo. Obtiene o establece un valor que indica el tamaño máximo de bytes privados de la aplicación antes de empezar a vaciar la memoria caché ha expirado elementos e intentar reclamar la memoria. Este límite incluye tanto memoria usada por la memoria caché como memoria normal sobrecarga de la aplicación en ejecución. Un valor de cero indica que ASP.NET usará su propia heurística para determinar cuándo se debe iniciar reclamar memoria. |
| **percentagePhysicalMemoryUsedLimit** | Opcional **Int32** atributo. Obtiene o establece un valor que indica el porcentaje máximo de memoria física de un equipo que puede consumir una aplicación antes de empezar a vaciar la memoria caché ha expirado elementos e intentar reclamar la memoria que este uso de memoria incluye tanto de la memoria usada por la caché también como el uso de memoria normal de la aplicación en ejecución. Un valor de cero indica que ASP.NET usará su propia heurística para determinar cuándo se debe iniciar reclamar memoria. |
| **privateBytesPollTime** | Opcional **TimeSpan** atributo. Obtiene o establece un valor que indica el intervalo de tiempo entre el sondeo para el uso de memoria de bytes privados de la aplicación. |

### <a name="the-ltoutputcachegt-element"></a>El &lt;outputCache&gt; elemento

Los siguientes atributos están disponibles para el &lt;outputCache&gt; elemento.


|       <strong>Attribute</strong>        |                                                                                                                                                                                                                                                       <strong>Descripción</strong>                                                                                                                                                                                                                                                       |
|-----------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|   <strong>enableOutputCache</strong>    |                                                                                                                                                          Opcional <strong>booleano</strong> atributo. Habilita o deshabilita la caché de resultados de página. Si deshabilita esta opción, no hay páginas se almacenan en caché independientemente de la configuración declarativa o mediante programación. Valor predeterminado es <strong>true</strong>.                                                                                                                                                           |
|  <strong>enableFragmentCache</strong>   |                                                Opcional <strong>booleano</strong> atributo. Habilita o deshabilita la caché de fragmentos de la aplicación. Si deshabilita esta opción, no hay páginas se almacenan en caché sin tener en cuenta la [@ OutputCache](https://msdn.microsoft.com/library/hdxfb6cy.aspx) directiva o el almacenamiento en caché de perfil que se usa. Incluye un encabezado de control de caché que indica que no deben intentar servidores proxy de nivel superior, así como los clientes de explorador a la caché de resultados de página. Valor predeterminado es <strong>false</strong>.                                                 |
| <strong>sendCacheControlHeader</strong> |                                                                                                                                                      Opcional <strong>booleano</strong> atributo. Obtiene o establece un valor que indica si el <strong>cache-control: private</strong> encabezado se envía el módulo de caché de resultados de forma predeterminada. Valor predeterminado es <strong>false</strong>.                                                                                                                                                      |
|      <strong>omitVaryStar</strong>      | Opcional <strong>booleano</strong> atributo. Habilita o deshabilita el envío de Http "<strong>Vary: \</strong ><em>" encabezado de la respuesta. Con el valor predeterminado es False, un "</em>* Vary: \* <strong>" se envía el encabezado para las páginas en caché de salida. Cuando se envía el encabezado Vary, permite que diferentes versiones en la memoria caché según lo especificado en el encabezado Vary. Por ejemplo, <em>Vary: usuario-agentes</em> almacenará versiones diferentes de una página según el agente de usuario que se emite la solicitud. Valor predeterminado es ** false</strong>. |

### <a name="the-ltoutputcachesettingsgt-element"></a>El &lt;outputCacheSettings&gt; elemento

El &lt;outputCacheSettings&gt; elemento permite la creación de perfiles de memoria caché como se describió anteriormente. El elemento secundario único para el &lt;outputCacheSettings&gt; elemento es el &lt;outputCacheProfiles&gt; (elemento) para configurar los perfiles de memoria caché.

### <a name="the-ltsqlcachedependencygt-element"></a>El &lt;sqlCacheDependency&gt; elemento

Los siguientes atributos están disponibles para el &lt;sqlCacheDependency&gt; elemento.

| **Attribute** | **Descripción** |
| --- | --- |
| **enabled** | Requiere **booleano** atributo. Indica si se sondean los cambios. |
| **pollTime** | Opcional **Int32** atributo. Establece la frecuencia con que SqlCacheDependency sondea la tabla de base de datos de los cambios. Este valor corresponde al número de milisegundos que transcurren entre sondeos sucesivos. No se puede establecer en menos de 500 milisegundos. Valor predeterminado es 1 minuto. |

### <a name="more-information"></a>Más información

No hay información adicional que debe tener en cuenta sobre la configuración de la memoria caché.

- Si no se establece el límite de bytes privados del proceso de trabajo, la memoria caché usará uno de los límites siguientes: 

    - x86 2 GB: 800 MB o el 60% de memoria RAM física, lo que sea menor
    - x86 3 GB: 1800 MB o el 60% de memoria RAM física, lo que sea menor
    - x 64: 1 terabyte o 60% de memoria RAM física, lo que sea menor
- Límite de bytes si tanto el proceso de trabajo privado y &lt;caché privateBytesLimit /&gt; están configurados, se utilizará la caché el valor mínimo de los dos.
- Al igual que en la versión 1.x, se quite las entradas de caché y llamar a GC. Recopilar por dos motivos: 

    - Estamos muy cerca del límite de bytes privados
    - La memoria disponible es inferior al 10% o casi
- Puede deshabilitar el recorte y almacenar en caché para las condiciones de poca memoria disponible estableciendo eficazmente &lt;caché percentagePhysicalMemoryUseLimit /&gt; a 100.
- A diferencia de 1.x, 2.0 suspender las llamadas de recorte y recopilar si la última GC. No reducir recopilar bytes privados o el tamaño de los montones administrados en más de un 1% del límite de memoria (caché).

## <a name="lab1-custom-cache-dependencies"></a>Lab1: Dependencias de caché personalizadas

1. Cree un nuevo sitio Web.
2. Agregue un nuevo archivo XML llamado cache.xml y guárdelo en la raíz de la aplicación Web.
3. Agregue el código siguiente a la página\_Load (método) en el código subyacente de default.aspx: 

    [!code-csharp[Main](caching/samples/sample15.cs)]
4. Agregue lo siguiente a la parte superior de default.aspx en la vista de origen: 

    [!code-aspx[Main](caching/samples/sample16.aspx)]
5. Examinar Default.aspx. ¿Qué decir el tiempo?
6. Actualice el explorador. ¿Qué decir el tiempo?
7. Abra cache.xml y agregue el código siguiente: 

    [!code-xml[Main](caching/samples/sample17.xml)]
8. Guardar cache.xml.
9. Actualice el explorador. ¿Qué decir el tiempo?
10. Se explica por qué la hora se actualiza en lugar de mostrar los valores previamente almacenado en caché:

## <a name="lab-2-using-polling-based-cache-dependencies"></a>Laboratorio 2: Uso de las dependencias de caché basada en sondeo

Esta práctica utiliza el proyecto que creó en el módulo anterior que permite la edición de los datos en la base de datos Northwind mediante un control GridView y DetailsView.

1. Abra el proyecto en Visual Studio 2005.
2. Ejecute aspnet\_regsql utilidad frente a la base de datos Northwind para habilitar la base de datos y la tabla Products. Use el siguiente comando desde un símbolo del sistema de Visual Studio: 

    [!code-console[Main](caching/samples/sample18.cmd)]
3. Agregue lo siguiente al archivo web.config: 

    [!code-xml[Main](caching/samples/sample19.xml)]
4. Agregue un nuevo webform llamado showdata.aspx.
5. Agregue el siguiente @ outputcache (directiva) a la página showdata.aspx: 

    [!code-aspx[Main](caching/samples/sample20.aspx)]
6. Agregue el código siguiente a la página\_carga de showdata.aspx: 

    [!code-html[Main](caching/samples/sample21.html)]
7. Agregar un nuevo control SqlDataSource para showdata.aspx y configúrelo para utilizar la conexión de base de datos Northwind. Haga clic en siguiente.
8. Seleccione las casillas de verificación ProductName y ProductID y haga clic en siguiente.
9. Haga clic en Finalizar.
10. Agregar un control GridView nuevo a la página showdata.aspx.
11. En la lista desplegable, elija SqlDataSource1.
12. Guarde y examinar showdata.aspx. Anote la hora mostrada.
