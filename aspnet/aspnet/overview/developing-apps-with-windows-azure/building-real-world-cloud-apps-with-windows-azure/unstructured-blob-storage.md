---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: Almacenamiento de blobs sin estructurar (crear aplicaciones de nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 03/30/2015
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 9ca599e023ac9b1cf8f00389048c8da6ef0e364f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836683"
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Almacenamiento de blobs sin estructurar (crear aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


En el capítulo anterior hemos examinado los esquemas de partición y se explica cómo la aplicación Fix It almacena las imágenes en el servicio Blob de almacenamiento de Azure y otros datos de la tarea en Azure SQL Database. En este capítulo se profundizan en Blob service y muestran cómo se implementa en código del proyecto Fix It.

## <a name="what-is-blob-storage"></a>¿Qué es Blob storage?

El servicio de Blob de Azure Storage proporciona una forma de almacenar archivos en la nube. El servicio Blob tiene una serie de ventajas con respecto a almacenar los archivos en un sistema de archivos de red local:

- Es altamente escalable. Puede almacenar una sola cuenta de almacenamiento [cientos de terabytes](https://msdn.microsoft.com/library/windowsazure/dn249410.aspx), y puede tener varias cuentas de almacenamiento. Algunos de los clientes de Azure más importantes almacenan cientos de petabytes. Microsoft SkyDrive usa blob storage.
- Es duradero. Automáticamente se copian todos los archivos que se almacenan en Blob service.
- Proporciona una alta disponibilidad. El [SLA de almacenamiento](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) tiempo de actividad de promesas del 99,9% o del 99,99%, según la opción de redundancia geográfica que elija.
- Es una característica de plataforma como servicio (PaaS) de Azure, lo que significa que acaba de almacena y recupera archivos, pagar solo por la cantidad real de almacenamiento que use, y Azure se encarga automáticamente de configurar y administrar todas las máquinas virtuales y las unidades de disco necesarias para el servicio.
- Puede acceder al servicio de Blob mediante el uso de una API de REST o mediante un lenguaje de programación API. Los SDK están disponibles para. NET, Java, Ruby y otros usuarios.
- Si almacena un archivo en Blob service, se puede fácilmente hacer públicamente disponible a través de Internet.
- Puede proteger archivos en Blob service para que puedan tener acceso a solo los usuarios autorizados, o puede proporcionar tokens de acceso temporal que pone a disposición de un usuario solo para un período de tiempo limitado.

Cada vez que se está compilando una aplicación de Azure y desea almacenar una gran cantidad de datos que en un entorno local irían en archivos, como imágenes, vídeos, documentos PDF, hojas de cálculo, etc., considere la posibilidad de Blob service.

## <a name="creating-a-storage-account"></a>Creación de una cuenta de almacenamiento

Para empezar a trabajar con Blob service cree una cuenta de almacenamiento en Azure. En el portal, haga clic en **New** -- **Data Services** -- **almacenamiento** -- **creación rápida**, y, a continuación, escriba una dirección URL y una ubicación de centro de datos. La ubicación del centro de datos debe ser la misma que la aplicación web.

![Crear una cuenta de almacenamiento](unstructured-blob-storage/_static/image1.png)

Elija la región principal donde desea almacenar el contenido y, si elige la [georreplicación](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) opción, Azure crea las réplicas de todos los datos en otro centro de datos en otra región del país. Por ejemplo, si elige el centro de datos oeste de EE.UU., al almacenar un archivo entra en el centro de datos del oeste de EE.UU., pero en segundo plano, Azure también copia a uno de los otros centros de datos de Estados Unidos. Si se produce un desastre en una región del país, los datos están seguros.

Azure no replica datos entre los límites geopolíticos: si la ubicación principal está en Estados Unidos, los archivos solo se replican en otra región de EE. Si la ubicación principal es Australia, los archivos solo se replican en otro centro de datos en Australia.

Por supuesto, también puede crear una cuenta de almacenamiento mediante la ejecución de comandos desde una secuencia de comandos, como hemos visto anteriormente. Este es un comando de Windows PowerShell para crear una cuenta de almacenamiento:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Una vez que tenga una cuenta de almacenamiento, puede comenzar inmediatamente almacenar archivos en Blob service.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Usar Blob storage en la aplicación Fix It

La aplicación Fix It le permite cargar fotos.

![Crear una tarea Fix It](unstructured-blob-storage/_static/image2.png)

Al hacer clic en **crear el FixIt**, la aplicación carga el archivo de imagen especificados y lo almacena en el servicio Blob.

### <a name="set-up-the-blob-container"></a>Configurar el contenedor de blobs

Con el fin de almacenar un archivo en el servicio de Blob que necesita un *contenedor* para almacenarla. Un contenedor de Blob service corresponde a una carpeta de sistema de archivos. Los scripts de creación del entorno que se han revisado en el [automatizar todo capítulo](automate-everything.md) crear la cuenta de almacenamiento, pero no crear un contenedor. Por lo que el propósito de la `CreateAndConfigure` método de la `PhotoService` clase consiste en crear un contenedor si aún no existe. Este método se llama desde el `Application_Start` método *Global.asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

La clave de acceso y el nombre de la cuenta de almacenamiento se almacenan en el `appSettings` colección de la *Web.config* de archivos y código en el `StorageUtils.StorageAccount` método usa esos valores para generar una cadena de conexión y establecer una conexión:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

El `CreateAndConfigureAsync` método, a continuación, crea un objeto que representa el servicio Blob y un objeto que representa un contenedor (carpeta) denominado "imágenes" en el servicio Blob:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Si un contenedor había llamado "images" que no exista, que será true, la primera vez que se ejecuta la aplicación en una nueva cuenta de almacenamiento, el código crea el contenedor y establece permisos para que sea público. (De forma predeterminada, nuevos contenedores de blob son privados y son accesibles sólo a los usuarios que tienen permiso para acceder a su cuenta de almacenamiento).

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Store la fotografía cargada en Blob storage

Para cargar y guardar el archivo de imagen, la aplicación usa un `IPhotoService` interfaz y una implementación de la interfaz en el `PhotoService` clase. El *PhotoService.cs* archivo contiene todo el código en la aplicación Fix It que se comunica con el servicio Blob.

El siguiente método de controlador MVC se llama cuando el usuario hace clic **crear el FixIt**. En este código, `photoService` hace referencia a una instancia de la `PhotoService` (clase), y `fixittask` hace referencia a una instancia de la `FixItTask` clase de entidad que almacena los datos de una nueva tarea.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

El `UploadPhotoAsync` método en el `PhotoService` clase almacena el archivo cargado en Blob service y devuelve una dirección URL que apunte al nuevo blob.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Como en el `CreateAndConfigure` método, el código se conecta a la cuenta de almacenamiento y crea un objeto que representa el contenedor de blobs "imágenes", excepto que aquí se supone el contenedor ya existe.

A continuación, crea un identificador único para la imagen está a punto de cargarse, mediante la concatenación de un nuevo valor GUID con la extensión de archivo:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

A continuación, el código usa el objeto de contenedor de blob y el nuevo identificador único para crear un objeto de blob, establece un atributo en ese objeto que indica qué tipo de archivo es y, a continuación, usa el objeto de blob para almacenar el archivo en blob storage.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Por último, obtiene una dirección URL que hace referencia al blob. Esta dirección URL se almacenarán en la base de datos y puede usarse en páginas web Fix It para mostrar la imagen cargada.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Esta dirección URL se almacena en la base de datos como una de las columnas de la tabla FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Con solo la dirección URL en la base de datos e imágenes en Blob storage, la aplicación Fix It mantiene la base de datos pequeñas, escalable y económico, mientras que las imágenes se almacenan en almacenamiento es barato y capaz de controlar terabytes o petabytes. Una cuenta de almacenamiento puede almacenar cientos de terabytes de fotos Fix It, y solo paga por lo que usa. Para que pueda comenzar en pequeño centavos 9 versiones de pago para el primer gigabyte y agregar más imágenes para céntimos por gigabyte adicional.

### <a name="display-the-uploaded-file"></a>Mostrar el archivo cargado

La aplicación Fix It muestra el archivo de imagen cargada cuando se muestran los detalles de una tarea.

![Corregir detalles de la tarea con fotografías](unstructured-blob-storage/_static/image3.png)

Para mostrar la imagen, todo la vista de MVC tiene que hacer es incluir el `PhotoUrl` valor en el código HTML que se envía al explorador. El servidor web y la base de datos no están utilizando ciclos para mostrar la imagen, sólo se ocupen de unos pocos bytes a la dirección URL de imagen. En el siguiente código Razor, `Model` hace referencia a una instancia de la `FixItTask` clase de entidad.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Si observa el código HTML de la página que muestra, consulte la dirección URL que apunta directamente a la imagen en blob storage, algo parecido a esto:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Resumen

Ha visto cómo la aplicación Fix It almacena las imágenes en Blob service y solo direcciones URL de imagen en la base de datos SQL. Para usar Blob service mantiene la base de datos SQL mucho menor de lo contrario sería, hace posible escalar verticalmente hasta un número casi ilimitado de tareas y puede realizarse sin tener que escribir mucho código.

Puede tener cientos de terabytes en una cuenta de almacenamiento y el costo de almacenamiento es mucho menos costoso que el almacenamiento de base de datos SQL, empezando en aproximadamente 3 centavos por gigabyte al mes, más un cargo de transacción pequeña. Y tenga en cuenta que no se presta para la capacidad máxima, pero solo para la cantidad que almacena realmente; por lo que la aplicación está lista para escalar, pero no está pagando una capacidad adicional por todo lo que.

En el [siguiente capítulo](design-to-survive-failures.md) hablaremos sobre la importancia de hacer que una aplicación de nube capaz de controlar correctamente los errores.

## <a name="resources"></a>Recursos

Para obtener más información, consulte los siguientes recursos:

- [Una introducción a Azure BLOB Storage](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog de Mike madera.
- [Cómo usar el servicio Azure Blob Storage en .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentación oficial sobre el sitio MicrosoftAzure.com. Una breve introducción en blob storage, seguido de los ejemplos de código que muestra cómo conectarse a blob storage, crear contenedores, cargar y descargar blobs, etcetera.
- [FailSafe: Creación de servicios de nube escalables, resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de vídeos de nueve partes, Ulrich Homann, Marc Mercuri y Mark Simms. Presenta conceptos y principios de la arquitectura de una manera muy accesible e interesante, con casos procedentes de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Para obtener una descripción del servicio de Azure Storage y blobs, vea el episodio 5 comenzando en 35:13.
- [Microsoft Patterns and Practices - Guía de Azure](https://msdn.microsoft.com/library/dn568099.aspx). Patrón Valet Key de ver.

> [!div class="step-by-step"]
> [Anterior](data-partitioning-strategies.md)
> [Siguiente](design-to-survive-failures.md)
