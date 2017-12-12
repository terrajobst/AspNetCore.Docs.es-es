---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
title: "Almacenamiento de blobs no estructurados (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft"
author: MikeWasson
description: "Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/30/2015
ms.topic: article
ms.assetid: 9f05ccb1-2004-4661-ad8b-c370e6c09c8e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/unstructured-blob-storage
msc.type: authoredcontent
ms.openlocfilehash: 6cb77e8ef301c2eeef7df3e391e14f4e2c0364e9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="unstructured-blob-storage-building-real-world-cloud-apps-with-azure"></a>Almacenamiento de blobs no estructurados (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


En el capítulo anterior se busca en esquemas de partición y se explica cómo la aplicación repararlo almacena las imágenes en el servicio de almacenamiento de Azure y otros datos de la tarea en la base de datos de SQL Azure. En este capítulo se profundizar en el servicio Blob y mostrar cómo se implementa en código del proyecto repararlo.

## <a name="what-is-blob-storage"></a>¿Qué es el almacenamiento de blobs?

El servicio de almacenamiento de Azure proporciona una manera de almacenar archivos en la nube. El servicio Blob tiene una serie de ventajas sobre el almacenamiento de archivos en un sistema de archivos de red local:

- Es muy escalable. Puede almacenar una sola cuenta de almacenamiento [centenares de terabytes](https://msdn.microsoft.com/en-us/library/windowsazure/dn249410.aspx), y puede tener varias cuentas de almacenamiento. Algunos de los principales clientes de Azure almacenan cientos de petabytes. Microsoft SkyDrive usa almacenamiento de blobs.
- Es duradera. Todos los archivos que se almacenan en el servicio Blob se hacen copias de seguridad.
- Proporciona alta disponibilidad. El [SLA de almacenamiento](https://go.microsoft.com/fwlink/p/?linkid=159705&amp;clcid=0x409) promesas 99,9% o 99,99% tiempo de actividad, según la opción de redundancia geográfica que elija.
- Es una característica de plataforma como servicio (PaaS) de Azure, lo que significa que acaba de almacena y recupera archivos, pagar solo por la cantidad real de almacenamiento que use, y Azure automáticamente se encarga de configurar y administrar todas las máquinas virtuales y las unidades de disco necesarias para el servicio.
- Puede acceder al servicio de Blob mediante el uso de una API de REST o mediante un lenguaje de programación API. Hay disponibles SDK para. NET, Java, Ruby y otros usuarios.
- Si almacena un archivo en el servicio Blob, se puede fácilmente hacer públicamente disponible a través de Internet.
- Puede proteger archivos en el Blob tiene acceso al servicio para que puedan solo por usuarios autorizados, o puede proporcionar tokens de acceso temporal que hace que estén disponibles para un usuario solo durante un período limitado de tiempo.

Cada vez que se va a compilar una aplicación de Azure y desea almacenar una gran cantidad de datos que en un entorno local saldría en archivos, como imágenes, vídeos, PDF, hojas de cálculo, etc., tenga en cuenta el servicio Blob.

## <a name="creating-a-storage-account"></a>Crear una cuenta de almacenamiento

Para empezar a trabajar con el servicio Blob, creará una cuenta de almacenamiento de Azure. En el portal, haga clic en **New** -- **Data Services** -- **almacenamiento** -- **creación rápida**, y, a continuación, escriba una dirección URL y una ubicación de centro de datos. La ubicación de centro de datos debe ser la misma que la aplicación web.

![Crear una cuenta de almacenamiento](unstructured-blob-storage/_static/image1.png)

Elija la región principal donde desea almacenar el contenido y, si elige la [georreplicación](https://blogs.msdn.com/b/windowsazurestorage/archive/2013/12/11/introducing-read-access-geo-replicated-storage-ra-grs-for-windows-azure-storage.aspx#_Geo_Redundant_Storage) opción, Azure crea réplicas de todos los datos en otro centro de datos en otra región del país. Por ejemplo, si elige el centro de datos US occidentales, cuando se almacena un archivo entra en el centro de datos de Estados Unidos occidentales, pero en segundo plano Azure también lo copia en uno de los centros de datos de Estados Unidos. Si ocurre un desastre en una región del país, sus datos están seguros.

Azure no replica datos a través de límites geopolíticos: si la ubicación principal es la zona horaria del Pacífico, los archivos sólo se replican en otra región dentro de los Estados Unidos; Si la ubicación principal es Australia, los archivos sólo se replican en otro centro de datos en Australia.

Por supuesto, también puede crear una cuenta de almacenamiento mediante la ejecución de comandos desde una secuencia de comandos, tal y como se ha explicado anteriormente. Este es un comando de Windows PowerShell para crear una cuenta de almacenamiento:

[!code-powershell[Main](unstructured-blob-storage/samples/sample1.ps1)]

Una vez que tenga una cuenta de almacenamiento, puede empezar inmediatamente a almacenar archivos en el servicio Blob.

## <a name="using-blob-storage-in-the-fix-it-app"></a>Con el almacenamiento de blobs en la aplicación repararlo

La aplicación repararlo permite cargar fotos.

![Crear una tarea repararlo](unstructured-blob-storage/_static/image2.png)

Al hacer clic en **crear la FixIt**, la aplicación carga el archivo de imagen especificado y lo almacena en el servicio Blob.

### <a name="set-up-the-blob-container"></a>Configurar el contenedor de blobs

Con el fin de almacenar un archivo en el servicio de Blob que necesite un *contenedor* se va a almacenar en. Un contenedor de servicios de Blob corresponde a una carpeta de sistema de archivos. Las secuencias de comandos de creación de entorno que se revisan en el [automatizar todo capítulo](automate-everything.md) crear la cuenta de almacenamiento, pero no crean un contenedor. Por lo que el propósito de la `CreateAndConfigure` método de la `PhotoService` clase consiste en crear un contenedor si ya no existe. Este método se llama desde el `Application_Start` método *Global.asax*.

[!code-csharp[Main](unstructured-blob-storage/samples/sample2.cs)]

La clave de acceso y nombre de la cuenta de almacenamiento se almacenan en la `appSettings` colección de la *Web.config* de archivos y el código de la `StorageUtils.StorageAccount` método usa estos valores para generar una cadena de conexión y establecer una conexión:

[!code-csharp[Main](unstructured-blob-storage/samples/sample3.cs)]

El `CreateAndConfigureAsync` método, a continuación, crea un objeto que representa el servicio Blob, y un objeto que representa un contenedor (carpeta) con el nombre "imágenes" en el servicio Blob:

[!code-csharp[Main](unstructured-blob-storage/samples/sample4.cs)]

Si un contenedor había denominado "imágenes" no existe todavía, que será true la primera vez que se ejecuta la aplicación en una nueva cuenta de almacenamiento, el código crea el contenedor y establece permisos para que sea público. (De forma predeterminada, nuevos contenedores de blob son privados y solo son accesibles a los usuarios que tienen permiso para acceder a su cuenta de almacenamiento.)

[!code-csharp[Main](unstructured-blob-storage/samples/sample5.cs)]

### <a name="store-the-uploaded-photo-in-blob-storage"></a>Almacenar la foto cargada en el almacenamiento de blobs

Para cargar y guardar el archivo de imagen, la aplicación usa un `IPhotoService` interfaz y una implementación de la interfaz en la `PhotoService` clase. El *PhotoService.cs* archivo contiene todo el código en la aplicación repararlo que se comunica con el servicio Blob.

El siguiente método de controlador MVC se llama cuando el usuario hace clic en **crear la FixIt**. En este código, `photoService` hace referencia a una instancia de la `PhotoService` (clase), y `fixittask` hace referencia a una instancia de la `FixItTask` clase de entidad que almacena los datos para una nueva tarea.

[!code-csharp[Main](unstructured-blob-storage/samples/sample6.cs?highlight=8)]

El `UploadPhotoAsync` método en la `PhotoService` clase almacena el archivo cargado en el servicio Blob y devuelve una dirección URL que señala al blob nuevo.

[!code-csharp[Main](unstructured-blob-storage/samples/sample7.cs)]

Como en el `CreateAndConfigure` método, el código se conecta a la cuenta de almacenamiento y crea un objeto que representa el contenedor de blob "imágenes", salvo que aquí se supone que ya existe el contenedor.

A continuación, crea un identificador único para la imagen que se va a cargar, al concatenar un valor GUID nueva con la extensión de archivo:

[!code-csharp[Main](unstructured-blob-storage/samples/sample8.cs)]

A continuación, el código usa el objeto de contenedor de blob y el nuevo identificador único para crear un objeto blob, establece un atributo en ese objeto que indica qué tipo de archivo lo está y, a continuación, utiliza el objeto blob para almacenar el archivo en el almacenamiento de blobs.

[!code-csharp[Main](unstructured-blob-storage/samples/sample9.cs)]

Por último, obtiene una dirección URL que hace referencia el blob. Esta dirección URL se almacenará en la base de datos y puede usarse en páginas web repararlo para mostrar la imagen cargada.

[!code-csharp[Main](unstructured-blob-storage/samples/sample10.cs)]

Esta dirección URL se almacena en la base de datos como una de las columnas de la tabla FixItTask.

[!code-csharp[Main](unstructured-blob-storage/samples/sample11.cs?highlight=10)]

Con solo la dirección URL en la base de datos e imágenes en almacenamiento de blobs, la aplicación repararlo mantiene la base de datos pequeños, escalable y asequible, mientras que las imágenes se almacenan donde almacenamiento es barato y capaz de controlar terabytes o petabytes. Una cuenta de almacenamiento puede almacenar cientos de terabytes de fotos repararlo y solo se paga por lo que usa. Para que pueda empezar pequeños centavos 9 pago para el primer gigabyte y agregar más imágenes para céntimos por gigabyte adicional.

### <a name="display-the-uploaded-file"></a>Mostrar el archivo cargado

La aplicación repararlo muestra el archivo de imagen cargada cuando se muestren los detalles de una tarea.

![Corregir detalles de la tarea con fotografías](unstructured-blob-storage/_static/image3.png)

Para mostrar la imagen, todo la vista MVC tiene que hacer es incluir el `PhotoUrl` valor en el código HTML que se envía al explorador. El servidor web y la base de datos no utiliza ciclos para mostrar la imagen, que solo trabajan de unos pocos bytes a la dirección URL de imagen. En el siguiente código Razor, `Model` hace referencia a una instancia de la `FixItTask` clase de entidad.

[!code-cshtml[Main](unstructured-blob-storage/samples/sample12.cshtml?highlight=11)]

Si observa el código HTML de la página que muestra, consulte la dirección URL que apunta directamente a la imagen en el almacenamiento de blobs, parecido a lo siguiente:

[!code-cshtml[Main](unstructured-blob-storage/samples/sample13.cshtml?highlight=11)]

## <a name="summary"></a>Resumen

Ha visto cómo la aplicación repararlo almacena las imágenes en el servicio de Blob y sólo direcciones URL de imagen en la base de datos SQL. Usar el servicio Blob mantiene la base de datos SQL mucho menor que de lo contrario sería, hace posible escalar hasta un número casi ilimitado de tareas y puede realizarse sin tener que escribir una gran cantidad de código.

Puede tener cientos de terabytes en una cuenta de almacenamiento, y el costo de almacenamiento es mucho más económico que el almacenamiento de base de datos SQL, a partir de unos 3 centavos por gigabyte al mes, más una carga de transacciones pequeño. Y tenga en cuenta que no se presta para la capacidad máxima, pero solo para la cantidad que almacena realmente; por lo que la aplicación esté lista para escalar, pero no se presta capacidad adicional para todas las que.

En el [siguiente capítulo](design-to-survive-failures.md) hablaremos sobre la importancia de hacer que una aplicación de nube capaz de controlar correctamente los errores.

## <a name="resources"></a>Recursos

Para obtener más información, vea los siguientes recursos:

- [Una introducción al almacenamiento de blobs de Azure](https://www.simple-talk.com/cloud/cloud-data/an-introduction-to-windows-azure-blob-storage-/). Blog de Mike madera.
- [Cómo usar el servicio de almacenamiento de blobs de Azure en .NET](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Documentación oficial en el sitio MicrosoftAzure.com. Una breve introducción a seguido de los ejemplos de código que muestra cómo conectarse al almacenamiento de blobs, el almacenamiento de blobs crear contenedores, cargar y descargar los blobs, etcetera.
- [FailSafe: Creación de servicios en la nube escalables y resistentes](https://channel9.msdn.com/Series/FailSafe). Serie de vídeos de nueve partes por Ulrich Homann y Marc Mercuri, Mark Simms. Presenta los conceptos y principios de la arquitectura de una manera muy interesante y accesible, con casos extraídos de la experiencia del equipo de asesoramiento al cliente (CAT) de Microsoft con clientes reales. Para obtener una descripción de servicio de almacenamiento de Azure y los blobs, vea episodio 5 a partir de 35:13.
- [Microsoft patrones y prácticas - Guía de Azure](https://msdn.microsoft.com/en-us/library/dn568099.aspx). Patrón de clave Valet vea.

>[!div class="step-by-step"]
[Anterior](data-partitioning-strategies.md)
[Siguiente](design-to-survive-failures.md)
