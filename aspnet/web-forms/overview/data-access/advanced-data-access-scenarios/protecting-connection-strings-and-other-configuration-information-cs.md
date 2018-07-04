---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
title: Proteger las cadenas de conexión y otra información de configuración (C#) | Microsoft Docs
author: rick-anderson
description: Normalmente, una aplicación ASP.NET almacena información de configuración en un archivo Web.config. Parte de esta información es confidencial y garantiza la protección. Por def...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: ad8dd396-30f7-4abe-ac02-a0b84422e5be
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/protecting-connection-strings-and-other-configuration-information-cs
msc.type: authoredcontent
ms.openlocfilehash: e5ce4459a1b62fe851ce8a4cae50aed798f0d508
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378980"
---
<a name="protecting-connection-strings-and-other-configuration-information-c"></a>Proteger las cadenas de conexión y otra información de configuración (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_73_CS.zip) o [descargar PDF](protecting-connection-strings-and-other-configuration-information-cs/_static/datatutorial73cs1.pdf)

> Normalmente, una aplicación ASP.NET almacena información de configuración en un archivo Web.config. Parte de esta información es confidencial y garantiza la protección. De forma predeterminada este archivo no se enviará a un visitante del sitio Web, pero un administrador o un hacker puede tener acceso al sistema de archivos del servidor Web y ver el contenido del archivo. En este tutorial, aprenderá que ASP.NET 2.0 nos permite proteger la información confidencial mediante el cifrado de secciones del archivo Web.config.


## <a name="introduction"></a>Introducción

Información de configuración para las aplicaciones ASP.NET normalmente se almacena en un archivo XML denominado `Web.config`. En el transcurso de estos tutoriales hemos actualizado la `Web.config` unas cuantas veces. Al crear el `Northwind` DataSet con tipo en el [primer tutorial](../introduction/creating-a-data-access-layer-cs.md), por ejemplo, la información de la cadena de conexión se agregó automáticamente a `Web.config` en la `<connectionStrings>` sección. Más adelante, en la [páginas maestras y navegación del sitio](../introduction/master-pages-and-site-navigation-cs.md) tutorial, hemos actualizado manualmente `Web.config`, agregar un `<pages>` elemento que indica que todas las páginas ASP.NET en el proyecto deben utilizar el `DataWebControls` tema.

Puesto que `Web.config` puede contener información confidencial como cadenas de conexión, es importante que el contenido de `Web.config` se mantiene segura y oculta de personas no autorizadas. De forma predeterminada, cualquier HTTP de solicitud en un archivo con el `.config` extensión se controla mediante el motor ASP.NET, que devuelve el *este tipo de página no sea atendido* mensaje que se muestra en la figura 1. Esto significa que los visitantes no se pueden ver su `Web.config` escribiendo simplemente el contenido de s del archivo http://www.YourServer.com/Web.config en su barra de direcciones del explorador s.


[![Visitar Web.config a través de un explorador devuelve este tipo de página no sea atendido mensaje](protecting-connection-strings-and-other-configuration-information-cs/_static/image2.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image1.png)

**Figura 1**: visite `Web.config` a través de devoluciones de explorador a que este tipo de página no sea atendido mensaje ([haga clic aquí para ver imagen en tamaño completo](protecting-connection-strings-and-other-configuration-information-cs/_static/image3.png))


Pero ¿qué ocurre si un atacante es capaz de encontrar alguna otra vulnerabilidad de seguridad que le permite ver su `Web.config` s contenido del archivo? ¿Qué puede un atacante hacer con esta información y qué pasos se pueden tomar para proteger aún más la información confidencial dentro de `Web.config`? Afortunadamente, mayoría de las secciones en `Web.config` no contienen información confidencial. ¿Qué daño puede perpetrar un atacante si saben que el nombre del tema usado por las páginas ASP.NET predeterminado?

Ciertos `Web.config` secciones, sin embargo, contienen información confidencial que puede incluir cadenas de conexión, los nombres de usuario, contraseñas, los nombres de servidor, las claves de cifrado y así sucesivamente. Esta información normalmente se encuentra en la siguiente `Web.config` secciones:

- `<appSettings>`
- `<connectionStrings>`
- `<identity>`
- `<sessionState>`

En este tutorial veremos las técnicas para proteger información confidencial de configuración. Como veremos, la versión 2.0 de .NET Framework incluye un sistema protegido configuraciones que hace el cifrado y descifrado mediante programación las secciones de configuración seleccionada sea sencillo.

> [!NOTE]
> En este tutorial concluye con un vistazo a las recomendaciones de s de Microsoft para conectarse a una base de datos desde una aplicación ASP.NET. Además de cifrar las cadenas de conexión, puede ayudar a proteger su sistema asegurándose de que se conecta a la base de datos de forma segura.


## <a name="step-1-exploring-aspnet-20-s-protected-configuration-options"></a>Paso 1: Exploración de ASP.NET 2.0 s protegida de las opciones de configuración

ASP.NET 2.0 incluye un sistema de configuración protegida para cifrar y descifrar la información de configuración. Esto incluye los métodos de .NET Framework que puede utilizarse para cifrar o descifrar la información de configuración mediante programación. Usa el sistema de configuración protegida la [modelo de proveedor](http://aspnet.4guysfromrolla.com/articles/101905-1.aspx), lo que permite a los desarrolladores elegir qué implementación cifrado se utiliza.

.NET Framework incluye dos proveedores de configuración protegida:

- [`RSAProtectedConfigurationProvider`](https://msdn.microsoft.com/library/system.configuration.rsaprotectedconfigurationprovider.aspx) -utiliza el asimétrico [algoritmo RSA](http://en.wikipedia.org/wiki/Rsa) para cifrado y descifrado.
- [`DPAPIProtectedConfigurationProvider`](https://msdn.microsoft.com/system.configuration.dpapiprotectedconfigurationprovider.aspx) -usa el Windows [API de protección de datos (DPAPI)](https://msdn.microsoft.com/library/ms995355.aspx) para cifrado y descifrado.

Puesto que el sistema de configuración protegida implementa el patrón de diseño de proveedor, es posible crear su propio proveedor de configuración protegida y conéctelo a la aplicación. Consulte [implementar un proveedor de configuración protegida](https://msdn.microsoft.com/library/wfc2t3az(VS.80).aspx) para obtener más información sobre este proceso.

Los proveedores de DPAPI y RSA usan claves para sus rutinas de cifrado y descifrado, y se pueden almacenar estas claves en el nivel máquina o usuario. Las claves de nivel de equipo son ideales para escenarios donde se ejecuta la aplicación web en su propio servidor dedicado o si hay varias aplicaciones en un servidor que necesite compartir información cifrada. Las claves de nivel de usuario son una opción más segura en entornos de hospedaje compartidos donde otras aplicaciones en el mismo servidor no deben ser capaces de descifrar sus secciones de configuración de aplicación s protegido.

En este tutorial nuestros ejemplos utilizará el proveedor DPAPI y las claves de nivel de equipo. En concreto, se examinará cifrar el `<connectionStrings>` sección `Web.config`, aunque se puede usar el sistema de configuración protegida para cifrar cualquier mayoría `Web.config` sección. Para obtener información sobre el uso de claves de nivel de usuario o mediante el proveedor RSA, consulte los recursos en la sección Lecturas adicionales al final de este tutorial.

> [!NOTE]
> El `RSAProtectedConfigurationProvider` y `DPAPIProtectedConfigurationProvider` proveedores se registran en el `machine.config` archivo con los nombres de proveedor `RsaProtectedConfigurationProvider` y `DataProtectionConfigurationProvider`, respectivamente. Al cifrar o descifrar la información de configuración se debe proporcionar el nombre de proveedor adecuado (`RsaProtectedConfigurationProvider` o `DataProtectionConfigurationProvider`) en lugar del nombre de tipo real (`RSAProtectedConfigurationProvider` y `DPAPIProtectedConfigurationProvider`). Puede encontrar el `machine.config` de archivos en el `$WINDOWS$\Microsoft.NET\Framework\version\CONFIG` carpeta.


## <a name="step-2-programmatically-encrypting-and-decrypting-configuration-sections"></a>Paso 2: Mediante programación cifrar y descifrar las secciones de configuración

Con unas pocas líneas de código podemos cifrar o descifrar una sección de configuración determinado mediante un proveedor especificado. El código, como veremos en breve, simplemente debe hacer referencia mediante programación a la sección de configuración adecuado, llame a su `ProtectSection` o `UnprotectSection` método y, a continuación, llame el `Save` método para conservar los cambios. Además, .NET Framework incluye una utilidad de línea de comandos útiles que puede cifrar y descifrar la información de configuración. Exploramos esta utilidad de línea de comandos en el paso 3.

Para ilustrar la información de configuración de protección mediante programación, s permiten crear una página ASP.NET que incluye botones para cifrar y descifrar el `<connectionStrings>` sección `Web.config`.

Comience abriendo la `EncryptingConfigSections.aspx` página en el `AdvancedDAL` carpeta. Arrastre un control de cuadro de texto del cuadro de herramientas hasta el diseñador, establecer su `ID` propiedad `WebConfigContents`, sus `TextMode` propiedad a `MultiLine`y su `Width` y `Rows` propiedades al 95% y 15, respectivamente. Este control de cuadro de texto mostrará el contenido de `Web.config` lo que nos permite ver rápidamente si el contenido está cifrado o no. Por supuesto, en una aplicación real podría no desee mostrar el contenido de `Web.config`.

Bajo el cuadro de texto, agregue dos controles de botón denominados `EncryptConnStrings` y `DecryptConnStrings`. Establezca sus propiedades de texto en cadenas de conexión de cifrar y descifrar las cadenas de conexión.

En este momento, la pantalla debe ser similar a la figura 2.


[![Agregue un cuadro de texto y dos controles Button de Web a la página](protecting-connection-strings-and-other-configuration-information-cs/_static/image5.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image4.png)

**Figura 2**: agregar un cuadro de texto y dos controles Button de Web a la página ([haga clic aquí para ver imagen en tamaño completo](protecting-connection-strings-and-other-configuration-information-cs/_static/image6.png))


A continuación, se debe escribir código que carga y muestra el contenido de `Web.config` en la `WebConfigContents` cuadro de texto cuando la página es la primera carga. Agregue el código siguiente a la clase de código subyacente de s de página. Este código agrega un método denominado `DisplayWebConfig` y llamarla desde el `Page_Load` controlador de eventos cuando `Page.IsPostBack` es `false`:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample1.cs)]

El `DisplayWebConfig` método usa la [ `File` clase](https://msdn.microsoft.com/library/system.io.file.aspx) para abrir la aplicación s `Web.config` archivo, el [ `StreamReader` clase](https://msdn.microsoft.com/library/system.io.streamreader.aspx) para leer su contenido en una cadena y el [ `Path` clase](https://msdn.microsoft.com/library/system.io.path.aspx) para generar la ruta de acceso física del `Web.config` archivo. Estas clases se encuentran en el [ `System.IO` espacio de nombres](https://msdn.microsoft.com/library/system.io.aspx). Por lo tanto, deberá agregar una `using` `System.IO` instrucción a la parte superior de la clase de código subyacente, o bien, prefijo de los nombres con estas clases `System.IO.` .

A continuación, necesitamos agregar controladores de eventos para los dos controles de botón `Click` eventos y agregue el código necesario para cifrar y descifrar el `<connectionStrings>` sección mediante una clave de nivel de equipo con el proveedor DPAPI. En el diseñador, haga doble clic en cada uno de los botones para agregar un `Click` controlador de eventos en el código subyacente de clase y, a continuación, agregue el código siguiente:


[!code-csharp[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample2.cs)]

El código usado en los dos controladores de eventos es casi idéntico. Comiencen por obtener información acerca de la actual s aplicación `Web.config` de archivos a través de la [ `WebConfigurationManager` clase](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.aspx) s [ `OpenWebConfiguration` método](https://msdn.microsoft.com/library/system.web.configuration.webconfigurationmanager.openwebconfiguration.aspx). Este método devuelve el archivo de configuración web para la ruta de acceso virtual especificada. A continuación, el `Web.config` archivo s `<connectionStrings>` sección se accede a través de la [ `Configuration` clase](https://msdn.microsoft.com/library/system.configuration.configuration.aspx) s [ `GetSection(sectionName)` método](https://msdn.microsoft.com/library/system.configuration.configuration.getsection.aspx), que devuelve un [ `ConfigurationSection` ](https://msdn.microsoft.com/library/system.configuration.configurationsection.aspx) objeto.

El `ConfigurationSection` objeto incluye un [ `SectionInformation` propiedad](https://msdn.microsoft.com/library/system.configuration.configurationsection.sectioninformation.aspx) que proporciona información adicional y funcionalidad con respecto a la sección de configuración. Como muestra el código, podemos determinar si la sección de configuración se cifra mediante la comprobación de la `SectionInformation` propiedad s `IsProtected` propiedad. Además, puede cifrar o descifrar a través de la sección la `SectionInformation` propiedad s `ProtectSection(provider)` y `UnprotectSection` métodos.

El `ProtectSection(provider)` método acepta como entrada una cadena que especifica el nombre del proveedor de configuración protegida para usar al cifrar. En el `EncryptConnString` controlador de eventos de botón s pasamos DataProtectionConfigurationProvider en el `ProtectSection(provider)` método para que se utiliza el proveedor DPAPI. El `UnprotectSection` método puede determinar el proveedor que se usó para cifrar la sección de configuración y, por tanto, no requiere que cualquier parámetro de entrada.

Después de llamar a la `ProtectSection(provider)` o `UnprotectSection` método, debe llamar a la `Configuration` objeto s [ `Save` método](https://msdn.microsoft.com/library/system.configuration.configuration.save.aspx) para conservar los cambios. Una vez que se han cifrado o descifrar la información de configuración y los cambios guardados, llamamos `DisplayWebConfig` cargar actualizadas `Web.config` contenido en el control de cuadro de texto.

Una vez que ha escrito el código anterior, puede probarlo visitando el `EncryptingConfigSections.aspx` página a través de un explorador. Inicialmente verá una página que enumera el contenido de `Web.config` con el `<connectionStrings>` sección que se muestran en texto sin formato (consulte la figura 3).


[![Agregue un cuadro de texto y dos controles Button de Web a la página](protecting-connection-strings-and-other-configuration-information-cs/_static/image8.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image7.png)

**Figura 3**: agregar un cuadro de texto y dos controles Button de Web a la página ([haga clic aquí para ver imagen en tamaño completo](protecting-connection-strings-and-other-configuration-information-cs/_static/image9.png))


Ahora haga clic en el botón cifrar cadenas de conexión. Si la validación de solicitudes está habilitada, el marcado se devuelva desde el `WebConfigContents` TextBox generará un `HttpRequestValidationException`, que muestra el mensaje, potencialmente peligroso `Request.Form` ha detectado el valor desde el cliente. Validación de solicitudes, que está habilitada de forma predeterminada en ASP.NET 2.0, prohíbe las devoluciones de datos que incluyen HTML sin codificar y está diseñado para ayudar a evitar ataques de inyección de script. Esta comprobación se puede deshabilitar en el nivel página o aplicación. Para desactivar esta función para esta página, establezca el `ValidateRequest` si se establece en `false` en el `@Page` directiva. El `@Page` directiva se encuentra en la parte superior del marcado declarativo s de la página.


[!code-aspx[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample3.aspx)]

Para obtener más información sobre la validación de solicitud, su propósito, deshabilítelo en la página - y nivel de aplicación, como así como el modo HTML codificar marcado, consulte [validación de solicitud: evitar los ataques de secuencias de comandos](../../../../whitepapers/request-validation.md).

Después de deshabilitar la validación de solicitudes de la página, intente hacer clic en el botón cifrar cadenas de conexión de nuevo. En el postback, se tendrá acceso el archivo de configuración y su `<connectionStrings>` sección cifrada mediante el proveedor DPAPI. El cuadro de texto, a continuación, se actualiza para mostrar el nuevo `Web.config` contenido. Como se muestra en la figura 4, el `<connectionStrings>` información ahora está cifrada.


[![Al hacer clic en el cifrar conexión cadenas botón cifra el &lt;connectionString&gt; sección](protecting-connection-strings-and-other-configuration-information-cs/_static/image11.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image10.png)

**Figura 4**: al hacer clic en el cifrar conexión cadenas botón cifra el `<connectionString>` sección ([haga clic aquí para ver imagen en tamaño completo](protecting-connection-strings-and-other-configuration-information-cs/_static/image12.png))


El cifrado `<connectionStrings>` siguiente sección generado en mi equipo, aunque parte del contenido en el `<CipherData>` elemento se ha quitado para mayor brevedad:


[!code-xml[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample4.xml)]

> [!NOTE]
> El `<connectionStrings>` elemento especifica el proveedor utilizado para realizar el cifrado (`DataProtectionConfigurationProvider`). Esta información se usa por la `UnprotectSection` método cuando se hace clic en el botón de descifrar las cadenas de conexión.


Cuando se tiene acceso a la información de la cadena de conexión de `Web.config` , ya sea por el código que escribimos, desde un control SqlDataSource, o el código generado automáticamente desde los TableAdapters en nuestros conjuntos de datos con tipo - se descifran automáticamente. En pocas palabras, no tenemos que agregar ningún código adicional o la lógica para descifrar el cifrado `<connectionString>` sección. Para demostrarlo, visite uno de los tutoriales anteriores en este momento, por ejemplo, el tutorial sencillo para mostrar de la sección informes básicos (`~/BasicReporting/SimpleDisplay.aspx`). Como se muestra en la figura 5, el tutorial funciona exactamente como se esperaría, que indica que la página ASP.NET que se descifra automáticamente la información de la cadena de conexión cifrada.


[![La capa de acceso a datos descifra automáticamente la información de la cadena de conexión](protecting-connection-strings-and-other-configuration-information-cs/_static/image14.png)](protecting-connection-strings-and-other-configuration-information-cs/_static/image13.png)

**Figura 5**: la capa de acceso a datos descifra automáticamente la información de la cadena de conexión ([haga clic aquí para ver imagen en tamaño completo](protecting-connection-strings-and-other-configuration-information-cs/_static/image15.png))


Para revertir el `<connectionStrings>` sección a su representación de texto sin formato, haga clic en el botón de descifrar las cadenas de conexión. Debería ver las cadenas de conexión en el postback `Web.config` en texto sin formato. En este momento, la pantalla debe ser similar a cuando primero visitar esta página (consulte la figura 3).

## <a name="step-3-encrypting-configuration-sections-usingaspnetregiisexe"></a>Paso 3: Cifrar secciones de configuración mediante`aspnet_regiis.exe`

.NET Framework incluye una variedad de herramientas de línea de comandos en el `$WINDOWS$\Microsoft.NET\Framework\version\` carpeta. En el [utilizando las dependencias de caché de SQL](../caching-data/using-sql-cache-dependencies-cs.md) tutorial, por ejemplo, analizamos utilizando el `aspnet_regsql.exe` herramienta de línea de comandos para agregar la infraestructura necesaria para las dependencias de caché SQL. Otra herramienta de línea de comandos útil en esta carpeta es la [herramienta de registro de IIS de ASP.NET (`aspnet_regiis.exe`)](https://msdn.microsoft.com/library/k6h9cz8h(VS.80).aspx). Como su nombre implica, la herramienta de registro de IIS de ASP.NET se utiliza principalmente para registrar una aplicación de ASP.NET 2.0 con el servidor Web de Microsoft s profesional, IIS. Además de sus características relacionadas con IIS, también se puede usar la herramienta de registro de IIS de ASP.NET para cifrar o descifrar las secciones de configuración especificado en `Web.config`.

La instrucción siguiente muestra la sintaxis general que se utiliza para cifrar una sección de configuración con el `aspnet_regiis.exe` herramienta de línea de comandos:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample5.cmd)]

*sección* es la sección de configuración para cifrar (por ejemplo, connectionStrings), el *físico\_directory* es la ruta de acceso física completa al directorio raíz s aplicación web, y *proveedor*  es el nombre del proveedor de configuración protegida para usar (por ejemplo, DataProtectionConfigurationProvider). Como alternativa, si está registrada la aplicación web en IIS puede escribir la ruta de acceso virtual en lugar de la ruta de acceso física mediante la sintaxis siguiente:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample6.cmd)]

La siguiente `aspnet_regiis.exe` ejemplo cifra el `<connectionStrings>` sección mediante el proveedor DPAPI con una clave de nivel de equipo:


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample7.cmd)]

De forma similar, la `aspnet_regiis.exe` herramienta de línea de comandos puede usarse para descifrar las secciones de configuración. En lugar de usar el `-pef` switch, utilice `-pdf` (o en lugar de `-pe`, utilice `-pd`). Además, tenga en cuenta que el nombre del proveedor no es necesario al descifrar.


[!code-console[Main](protecting-connection-strings-and-other-configuration-information-cs/samples/sample8.cmd)]

> [!NOTE]
> Puesto que estamos usando el proveedor DPAPI, que usa claves específicas del equipo, debe ejecutar `aspnet_regiis.exe` desde el mismo equipo desde el que se atienden las páginas web. Por ejemplo, si ejecuta este programa de línea de comandos desde el equipo de desarrollo local y, a continuación, cargue el archivo Web.config cifrado en el servidor de producción, el servidor de producción no será capaz de descifrar la información de la cadena de conexión desde que se cifró uso de claves específicas de la máquina de desarrollo. El proveedor RSA no tiene esta limitación cuando sea posible exportar las claves RSA en otra máquina.


## <a name="understanding-database-authentication-options"></a>Opciones de autenticación de base de datos de descripción

Antes de cualquier aplicación puede emitir `SELECT`, `INSERT`, `UPDATE`, o `DELETE` consultas a una base de datos de Microsoft SQL Server, la base de datos en primer lugar deben identificar el solicitante. Este proceso se conoce como *autenticación* y SQL Server proporciona dos métodos de autenticación:

- **Autenticación de Windows** -el proceso en que se ejecuta la aplicación se utiliza para comunicarse con la base de datos. Cuando se ejecuta una aplicación ASP.NET a través de Visual Studio 2005 s servidor de desarrollo de ASP.NET, la aplicación ASP.NET adopta la identidad del usuario que ha iniciado sesión actualmente. Para las aplicaciones de ASP.NET en Microsoft Internet Information Server (IIS), las aplicaciones ASP.NET normalmente asumen la identidad de `domainName``\MachineName` o `domainName``\NETWORK SERVICE`, aunque se puede personalizar.
- **Autenticación de SQL** -suministran un valores de identificador y la contraseña de usuario como credenciales para la autenticación. Con la autenticación de SQL, el Id. de usuario y la contraseña se proporcionan en la cadena de conexión.

Windows se prefiere la autenticación a través de la autenticación de SQL porque es más seguro. Con la autenticación de Windows, la cadena de conexión es libre de un nombre de usuario y una contraseña y si el servidor web y el servidor de base de datos residen en dos equipos diferentes, las credenciales no se envían a través de la red en texto sin formato. Con la autenticación de SQL, sin embargo, las credenciales de autenticación están codificados en la cadena de conexión y se transmiten desde el servidor web en el servidor de base de datos en texto sin formato.

Estos tutoriales usan autenticación de Windows. Puede indicar que está usando el modo de autenticación mediante la inspección de la cadena de conexión. La cadena de conexión en `Web.config` ha sido para los tutoriales:

`Data Source=.\SQLEXPRESS; AttachDbFilename=|DataDirectory|\NORTHWND.MDF; Integrated Security=True; User Instance=True`

La seguridad integrada = True y la falta de un nombre de usuario y una contraseña indican que se utiliza la autenticación de Windows. En algunas conexiones cadenas el término de la conexión de confianza = Yes o Integrated Security = SSPI se utiliza en lugar de la seguridad integrada = True, pero las tres indican el uso de autenticación de Windows.

El ejemplo siguiente muestra una cadena de conexión que usa la autenticación de SQL. Tenga en cuenta las credenciales incrusten dentro de la cadena de conexión:

`Server=serverName; Database=Northwind; uid=userID; pwd=password`

Imagine que un atacante es capaz de ver la aplicación s `Web.config` archivo. Si utiliza la autenticación de SQL para conectarse a una base de datos que sea accesible a través de Internet, el atacante puede usar esta cadena de conexión para conectarse a la base de datos mediante SQL Management Studio o desde las páginas ASP.NET en su propio sitio Web. Para mitigar esta amenaza, cifrar la información de la cadena de conexión en `Web.config` con el sistema de configuración protegida.

> [!NOTE]
> Para obtener más información sobre los distintos tipos de autenticación disponibles en SQL Server, vea [crear aplicaciones ASP.NET seguras: autenticación, autorización y comunicación segura](https://msdn.microsoft.com/library/aa302392.aspx). Para obtener más conexión cadena ejemplos que ilustran las diferencias entre la sintaxis de la autenticación de Windows y SQL, consulte [ConnectionStrings.com](http://www.connectionstrings.com/).


## <a name="summary"></a>Resumen

De forma predeterminada, los archivos con un `.config` extensión en una aplicación ASP.NET no se puede acceder a través de un explorador. Estos tipos de archivos no se devuelven porque es posible que contienen información confidencial, como cadenas de conexión de base de datos, los nombres de usuario y contraseñas y así sucesivamente. El sistema de configuración protegida en .NET 2.0 ayuda a proteger aún más la información confidencial al permitir que las secciones de configuración especificado a cifrarse. Existen dos proveedores de configuración protegida integrados: uno que usa el algoritmo de RSA y otra que usa la API de protección de datos (DPAPI) de Windows.

En este tutorial hemos examinado cómo cifrar y descifrar los valores de configuración mediante el proveedor DPAPI. Esto puede realizarse tanto mediante programación, como vimos en el paso 2, así como a través del `aspnet_regiis.exe` herramienta de línea de comandos, que se ha explicado en el paso 3. Para obtener más información sobre el uso de claves de nivel de usuario o mediante el proveedor RSA en su lugar, consulte los recursos en la sección Lecturas adicionales.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Generando la aplicación de ASP.NET seguras: Autenticación, autorización y comunicación segura](https://msdn.microsoft.com/library/aa302392.aspx)
- [Cifrar información de configuración en ASP.NET 2.0 aplicaciones](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Cifrar `Web.config` valores en ASP.NET 2.0](https://weblogs.asp.net/scottgu/archive/2006/01/09/434893.aspx)
- [Cómo: Cifrar secciones de configuración en ASP.NET 2.0 mediante DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)
- [Cómo: Cifrar secciones de configuración en ASP.NET 2.0 mediante RSA](https://msdn.microsoft.com/library/ms998283.aspx)
- [La API de configuración en .NET 2.0](http://www.odetocode.com/Articles/418.aspx)
- [Protección de datos de Windows](https://msdn.microsoft.com/library/ms995355.aspx)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Los revisores para este tutorial fueron Teresa Murphy y Randy Schmidt. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Anterior](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs.md)
> [Siguiente](debugging-stored-procedures-cs.md)
