---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: Configurar la aplicación Web de producción para usar la base de datos de producción (C#) | Microsoft Docs
author: rick-anderson
description: Como se describe en los tutoriales anteriores, no es raro que difieren entre los entornos de desarrollo y producción información de configuración. Esto es es...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: b6899c199ea593e74d4d423cfc3f832e38bd7e57
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374955"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>Configurar la aplicación Web de producción para usar la base de datos de producción (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip) o [descargar PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> Como se describe en los tutoriales anteriores, no es raro que difieren entre los entornos de desarrollo y producción información de configuración. Esto es especialmente cierto para aplicaciones web controladas por datos, como las cadenas de conexión de base de datos difieren entre los entornos de desarrollo y producción. Este tutorial exploran métodos para configurar el entorno de producción para incluir la cadena de conexión adecuada con más detalle.


## <a name="introduction"></a>Introducción

Aplicaciones web controladas por datos suelen utilizan otra base de datos cuando está en desarrollo que cuando en producción. Para las aplicaciones hospedadas por un proveedor de hospedaje web y desarrollado de forma local, la base de datos de desarrollo reside normalmente en el equipo de s de desarrollador mientras la base de datos de producción está hospedada en un servidor de base de datos en las instalaciones de s de empresa de hospedaje web. Implementar una aplicación web controlada por datos implica copiar la base de datos de desarrollo en el servidor de base de datos de producción. En el tutorial anterior, hemos analizado algunas maneras de realizar este paso.

La aplicación web usa la información de un *cadena de conexión* para establecer una conexión con la base de datos. La cadena de conexión, que normalmente se almacena en `Web.config`, especifica el nombre del servidor de base de datos, el nombre de la base de datos, el contexto de seguridad y otra información. Dado que la base de datos utilizado por la aplicación web depende de si la aplicación web se está ejecutando en los entornos de desarrollo o producción, las cadenas de conexión deben distinguirse entre los dos entornos.

No es raro que difieren entre los entornos de desarrollo y producción información de configuración. El *Common Configuration diferencias entre desarrollo y producción* tutorial explicado técnicas para mantener la información de configuración independientes entre estos dos entornos, así como una explicación breve en cadenas de conexión de base de datos. Este tutorial exploran métodos para configurar el entorno de producción para incluir la cadena de conexión adecuada con más detalle.

## <a name="examining-the-connection-string-information"></a>Examinar la información de la cadena de conexión

La cadena de conexión utilizada por la aplicación web de reseñas de libros se almacena en el archivo de configuración de aplicación s `Web.config`. `Web.config` incluye una sección especial para almacenar cadenas de conexión, acertadamente denominadas [ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx). El `Web.config` archivo para el sitio Web de reseñas de libros tiene una cadena de conexión definida en esta sección denominada `ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

La cadena de conexión: origen de datos =. \SQLEXPRESS; AttachDbFilename = | DataDirectory|\Reviews.mdf;Integrated Security = True; Instancia de usuario = True: se compone de una serie de opciones y valores, con pares de opción y valor delimitados por punto y coma y cada opción y valor delimitados por un signo igual. Las cuatro opciones que se usan en esta cadena de conexión son:

- `Data Source` -Especifica la ubicación del servidor de la base de datos y el nombre de instancia del servidor de base de datos (si existe). El valor, `.\SQLEXPRESS`, es un ejemplo donde hay un servidor de base de datos y un nombre de instancia. El período de especifica que el servidor de base de datos está en el mismo equipo que la aplicación. es el nombre de instancia `SQLEXPRESS`.
- `AttachDbFilename` -Especifica la ubicación del archivo de base de datos. El valor contiene el marcador de posición `|DataDirectory|`, que se resuelve en la ruta de acceso completa de la aplicación de s `App_Data` carpeta en tiempo de ejecución.
- `Integrated Security` -un valor booleano que indica si se usa una nombre de usuario/contraseña especificada al conectarse a la base de datos (false) o del actual Windows las credenciales de cuenta (true).
- `User Instance` -una opción de configuración específica de SQL Server Express Edition que indica si se debe permitir que los usuarios sin derechos administrativos en el equipo local, adjuntar y conexión a una base de datos de SQL Server Express Edition. Consulte [las instancias de SQL Server Express usuario](https://msdn.microsoft.com/library/ms254504.aspx) para obtener más información sobre esta configuración.
  

Las opciones de cadena de conexión permitidos dependen de la base de datos a que se conecta y el proveedor de base de datos ADO.NET que utiliza. Por ejemplo, la cadena de conexión para conectarse a Microsoft SQL Server difiere de la base de datos que se usa para conectarse a una base de datos de Oracle. Del mismo modo, al conectarse a una base de datos de Microsoft SQL Server con el proveedor SqlClient usa una cadena de conexión diferente que cuando se utiliza el proveedor OLE DB.

Puede compilar la cadena de conexión de base de datos manualmente mediante un sitio como [ConnectionStrings.com](http://www.connectionstrings.com/) como un recurso de qué opciones están disponibles. Sin embargo, un enfoque más sencillo consiste en agregar la base de datos en el Explorador de servidores en Visual Studio y, a continuación, para obtener la cadena de conexión desde la ventana Propiedades. Permiten s usar esta última técnica para construir la cadena de conexión al servidor de base de datos de producción.

Abra Visual Studio y, a continuación, vaya a la ventana del explorador de servidores (en Visual Web Developer, esta ventana se denomina Explorador de base de datos). Haga doble clic en la opción de conexiones de datos y elija la opción de agregar la conexión en el menú contextual. Se abrirá el Asistente para la que se muestra en la figura 1. Elija el origen de datos adecuado y haga clic en continuar.


[![Optar por agregar una nueva base de datos en el Explorador de servidores](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**Figura 1**: optar por agregar una nueva base de datos en el Explorador de servidores ([haga clic aquí para ver imagen en tamaño completo](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


A continuación, especifique los distintos información de conexión de base de datos (consulte la figura 2). Cuando se registró con la empresa de hospedaje web debería entregar información sobre cómo conectarse a la base de datos: el nombre del servidor de base de datos, el nombre de base de datos, el nombre de usuario y contraseña que se utilizará para conectarse a la base de datos y así sucesivamente. Después de escribir esta información, haga clic en Aceptar para completar a este asistente y para agregar la base de datos en el Explorador de servidores.


[![Especifique la información de conexión de base de datos](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**Figura 2**: especifique la información de conexión de base de datos ([haga clic aquí para ver imagen en tamaño completo](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


La base de datos del entorno de producción debe aparecer ahora en el Explorador de servidores. Seleccione la base de datos desde el Explorador de servidores y vaya a la ventana Propiedades. Allí encontrará una propiedad denominada cadena de conexión con la cadena de conexión de base de datos s. Suponiendo que está usando una base de datos de Microsoft SQL Server en producción y el proveedor SqlClient la cadena de conexión debe ser similar al siguiente:

<strong>Origen de datos =<em>serverName</em>; Catálogo inicial =<em>databaseName</em>; Persist Security Info = True; Id. de usuario =<em>username</em>; Contraseña =*contraseña</strong>*

Donde *serverName*, *databaseName*, *username*, y *contraseña* son con los valores para el nombre del servidor de base de datos, la base de datos nombre y el nombre de usuario y la contraseña proporcionada por su empresa de host web.

## <a name="deploying-the-book-reviews-web-application"></a>Implementar la aplicación Web de las revisiones de libro

El tutorial anterior había avanzado a través de copiar la base de datos de desarrollo en el entorno de producción, pero no exploró implementar la aplicación controlada por datos. En este momento el entorno de producción contiene la base de datos, pero usa la versión de la aplicación de revisiones de libro con las revisiones estáticas. Es necesario implementar la aplicación nueva, controlada por datos en el servidor de producción junto con la información de configuración actualizada.

Dedique un momento para implementar la aplicación controlada por datos desde el entorno de desarrollo a producción. Este proceso se describe con detalle en tutoriales anteriores. Si necesita un actualizador, consulte el *implementar su sitio Web mediante un cliente FTP* o *implementar su sitio Web con Visual Studio* tutoriales. Deberá asegurarse de que la cadena de conexión de base de datos de producción es aquella usada en el entorno de producción, lo que significa que uno alternativo `Web.config` archivo debe implementarse. En concreto, puede modificar esto `Web.config` archivo s `<connectionStrings>` elemento debe contener la cadena de conexión de base de datos de producción y debe ser similar al siguiente:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

Tenga en cuenta que la cadena de conexión en el `<connectionStrings>` elemento tiene el mismo nombre (`ReviewsConnectionString`), pero ahora contiene la cadena de conexión de base de datos de producción en lugar de la cadena de conexión de base de datos de desarrollo.

A menos que tenga un flujo de trabajo de implementación más formal, modifique manualmente la `Web.config` archivo que se usará la cadena de conexión de base de datos de producción antes de implementar (recuerde volver a utilizar la cadena de conexión de base de datos de desarrollo más adelante) o mantener un independiente `Web.config` archivo con la información de configuración del entorno de producción que se carga en el entorno de producción como parte del proceso de implementación.

> [!NOTE]
> Si implementa accidentalmente un `Web.config` archivo que contiene la cadena de conexión de base de datos de desarrollo, a continuación, habrá un error cuando intente conectarse a la base de datos de la aplicación en producción. Este error se manifiesta como un `SqlException` con un mensaje que indica que el servidor no se encontró o no estaba accesible.


Una vez que el sitio se ha implementado en producción, visite el sitio de producción a través del explorador. Debe ver y disfrutar de la misma experiencia de usuario que cuando se ejecuta localmente la aplicación controlada por datos. Por supuesto cuando visite el sitio Web en producción el sitio funciona con el servidor de base de datos de producción, mientras que visitar el sitio Web en el entorno de desarrollo usa la base de datos en el desarrollo. Figura 3 muestra el *enseñar a usted mismo ASP.NET 3.5 en 24 horas* revisar página desde el sitio Web en el entorno de producción (tenga en cuenta la dirección URL en la barra de direcciones del explorador s).


[![¡La aplicación controlada por datos está ahora disponible en producción!](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**Figura 3**: The Data-Driven aplicación está ahora disponible en producción! ([Haga clic aquí para ver imagen en tamaño completo](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>Almacenar cadenas de conexión en un archivo de configuración independiente

Una técnica común para mantener la información de configuración independientes en los entornos de desarrollo y producción consiste en tener dos versiones de la `Web.config`: uno para el entorno de desarrollo y otro para producción. En tiempo de implementación adecuado `Web.config` versión se puede copiar en el entorno de producción. Idealmente, se podría automatizar este proceso como parte del flujo de trabajo de implementación.

En lugar de mantener dos `Web.config` archivos si lo desea, puede proporcionan las diferencias más granulares. Los elementos que componen el `Web.config` archivo se puede definir en un archivos de configuración externos que, a continuación, se hace referencia en el `Web.config` archivo. En pocas palabras puede tener `Web.config` de archivos para ambos entornos que hace referencia a un archivo databaseConnectionStrings.config, que contendría las cadenas de conexión utilizado por la aplicación y podrían ser únicos para cada entorno. Creo que la separación de la información de configuración diferentes en archivos independientes, proporciona un más limpia y sencilla `Web.config` archivo y mucho más claramente explica las diferencias de configuración entre entornos de desarrollo y producción.

Para usar esta técnica, empiece por crear una nueva carpeta en la aplicación web denominada `ConfigSections`. A continuación, agregue dos archivos a esta nueva carpeta denominada databaseConnectionStrings.dev.config y databaseConnectionStrings.production.config. A continuación, copie el `<connectionStrings>` elemento desde `Web.config` en los archivos databaseConnectionStrings.dev.config y databaseConnectionStrings.production.config y, a continuación, modifique la cadena de conexión en el databaseConnectionStrings.production.config archivo para que especifica la cadena de conexión de base de datos de producción. Por ejemplo, el archivo databaseConnectionStrings.dev.config debe contener solamente el `<connectionStrings>` elemento con una cadena de conexión que hace referencia a la base de datos de desarrollo:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

De forma similar, el archivo databaseConnectionStrings.production.config debe contener sólo un `<connectionStrings>` elemento, pero que tiene la cadena de conexión de base de datos de producción.

Realizar una copia del archivo databaseConnectionStrings.dev.config y asígnele el nombre databaseConnectionStrings.config.

> [!NOTE]
> Puede asignar el archivo de configuración distinto databaseConnectionStrings.config, en caso de d, tales como `connectionStrings.config` o `dbInfo.config`. Sin embargo, asegúrese de nombrar el archivo con un `.config` extensión como `.config` archivos son, de forma predeterminada, no se han atendido por el motor ASP.NET. Si el archivo asigna otro nombre, como `connectionStrings.txt`, un usuario podría señalar su explorador para [www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt) y ver el contenido del archivo.


En este momento el `ConfigSections` carpeta debe contener tres archivos (consulte la figura 4). Los archivos databaseConnectionStrings.dev.config y databaseConnectionStrings.production.config contienen las cadenas de conexión para los entornos de desarrollo y producción, respectivamente. El archivo databaseConnectionStrings.config contiene la información de la cadena de conexión que se usará en la aplicación web en tiempo de ejecución. Por lo tanto, el archivo databaseConnectionStrings.config debe ser idéntico al archivo databaseConnectionStrings.dev.config en el entorno de desarrollo, mientras que en producción debe ser idéntico al archivo databaseConnectionStrings.config databaseConnectionStrings.production.config.


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**Figura 4**: ConfigSections ([haga clic aquí para ver imagen en tamaño completo](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


Ahora es necesario indicar a `Web.config` para usar el archivo databaseConnectionStrings.config para su almacén de cadenas de conexión. Abra `Web.config` y reemplace el elemento `<connectionStrings>` existente por lo siguiente:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

El `configSource` atributo especifica una ruta de acceso física relativa a la `Web.config` archivo. Si el externo `.config` archivo se encuentra en el mismo directorio que `Web.config` , a continuación, establezca este atributo en el nombre de archivo de la `.config` archivo. Si, en un subdirectorio, como sucede con databaseConnectionStrings.config, especifican la subcarpeta con una barra diagonal inversa para delimitar los nombres de carpeta y archivo, como ConfigSections\databaseConnectionStrings.config.

Con esta modificación, los entornos de desarrollo y producción contienen los mismos `Web.config` archivo. Ahora la única diferencia es el archivo databaseConnectionStrings.config. Copie el archivo databaseConnectionStrings.production.config en producción y cámbielo por databaseConnectionStrings.config. Si, en el futuro, hay cambios en la cadena de conexión de base de datos de producción deberá incluirlas en el archivo databaseConnectionStrings.production.config y, a continuación, cargue ese archivo en el entorno de producción, el nuevo nombre databaseConnectionStrings.config.

> [!NOTE]
> Puede especificar la información para cualquier `Web.config` elemento en un archivo independiente y usar el `configSource` atributo hacer referencia a ese archivo desde `Web.config`.


## <a name="summary"></a>Resumen

Aplicaciones controladas por datos suelen utilizan diferentes bases de datos en entornos de desarrollo y producción. Por lo tanto, las cadenas de conexión de base de datos almacenadas en la configuración de s aplicación web deben ser únicas para cada entorno. En este tutorial hemos examinado cómo determinar la cadena de conexión de base de datos de producción y maneras de mantener la información de la cadena de conexión única en los dos entornos.

Feliz programación.

#### <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Cadenas de conexión y archivos de configuración](https://msdn.microsoft.com/library/ms254494.aspx)
- [Configuración de la base de datos de cadenas de información @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [Mover la configuración fuera del archivo Web.config](http://www.asp101.com/tips/index.asp?id=154)
- [Documentación técnica de la &lt;connectionStrings&gt; elemento](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [Anterior](deploying-a-database-cs.md)
> [Siguiente](configuring-a-website-that-uses-application-services-cs.md)
