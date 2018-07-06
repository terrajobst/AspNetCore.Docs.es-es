---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
title: Creación de un punto de conexión de OData v3 con Web API 2 | Microsoft Docs
author: MikeWasson
description: Open Data Protocol (OData) es un protocolo de acceso a datos para la web. OData proporciona una manera uniforme para estructurar datos, consultar los datos y manipular los datos...
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 262843d6-43a2-4f1c-82d9-0b90ae6df0cf
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/creating-an-odata-endpoint
msc.type: authoredcontent
ms.openlocfilehash: 67a41f37a09d089336dc96d393f929e56661c4f8
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813713"
---
<a name="creating-an-odata-v3-endpoint-with-web-api-2"></a>Creación de un punto de conexión de OData v3 con Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> El [Open Data Protocol](http://www.odata.org/) (OData) es un protocolo de acceso a datos para la web. OData proporciona una manera uniforme para estructurar datos, consultar los datos y manipular el conjunto de datos a través de las operaciones CRUD (crear, leer, actualizar y eliminar). OData admite formatos JSON y AtomPub (XML). OData también define un método para exponer metadatos sobre los datos. Los clientes pueden usar los metadatos para detectar la información de tipos y relaciones para el conjunto de datos.
> 
> ASP.NET Web API facilita la creación de un extremo de OData para un conjunto de datos. Puede controlar exactamente qué operaciones de OData en el extremo admite. Puede hospedar varios puntos de conexión de OData, junto con los puntos de conexión no OData. Tiene control total sobre su modelo de datos, la lógica de negocios de back-end y la capa de datos.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - Web API 2
> - OData versión 3
> - Entity Framework 6
> - [(Opcional) de Proxy de depuración de Web de Fiddler](http://www.fiddler2.com)
> 
> Se agregó compatibilidad con la Web API OData en [ASP.NET y Web Tools 2012.2 Update](https://go.microsoft.com/fwlink/?LinkId=282650). Sin embargo, en este tutorial se usa scaffolding que se agregó en Visual Studio 2013.


En este tutorial, creará un extremo OData simple que los clientes pueden consultar. También creará a un cliente de C# para el punto de conexión. Después de completar este tutorial, el siguiente conjunto de tutoriales se muestra cómo agregar más funcionalidad, incluidas las relaciones de entidad, acciones, y seleccione Expandir $/ $.

- [Crear el proyecto de Visual Studio](#create-project)
- [Agregar un modelo de entidades](#add-model)
- [Agregar un controlador de OData](#add-controller)
- [Agregue el EDM y ruta](#edm)
- [Valor de inicialización de la base de datos (opcional)](#seed-db)
- [Explorar el extremo de OData](#explore)
- [Formatos de serialización de OData](#formats)

<a id="create-project"></a>
## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

En este tutorial, creará un extremo de OData que admite operaciones CRUD básicas. El punto de conexión expondrá un único recurso, una lista de productos. Los tutoriales posteriores agregarán más características.

Inicie Visual Studio y seleccione **nuevo proyecto** desde la página de inicio. O bien, en el **archivo** menú, seleccione **New** y, a continuación, **proyecto**.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el nodo Visual C#. En **Visual C#**, seleccione **Web**. Seleccione **la aplicación Web ASP.NET** plantilla.

![](creating-an-odata-endpoint/_static/image1.png)

En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **vacía** plantilla. Bajo &quot;agregar carpetas y referencias centrales para... &quot;, comprobar **API Web**. Haga clic en **Aceptar**.

![](creating-an-odata-endpoint/_static/image2.png)

<a id="add-model"></a>
## <a name="add-an-entity-model"></a>Agregar un modelo de entidades

Un *modelo* es un objeto que representa los datos de la aplicación. Para este tutorial, se necesita un modelo que representa un producto. El modelo corresponde a nuestro tipo de entidad de OData.

En el Explorador de soluciones, haga clic en la carpeta Models. En el menú contextual, seleccione **agregar** , a continuación, seleccione **clase**.

![](creating-an-odata-endpoint/_static/image3.png)

En el **Agregar nuevo** elemento de cuadro de diálogo, asigne el nombre de la clase &quot;producto&quot;.

![](creating-an-odata-endpoint/_static/image4.png)

> [!NOTE]
> Por convención, las clases de modelo se colocan en la carpeta Models. No tiene que seguir esta convención en sus propios proyectos, pero vamos a usar para este tutorial.


En el archivo Product.cs, agregue la siguiente definición de clase:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample1.cs)]

La propiedad de Id. será la clave de entidad. Los clientes pueden consultar los productos por identificador. Este campo también sería la clave principal de la base de datos back-end.

Compile el proyecto ahora. En el paso siguiente, vamos a usar algunos scaffolding de Visual Studio que usa la reflexión para encontrar el tipo de producto.

<a id="add-controller"></a>
## <a name="add-an-odata-controller"></a>Agregar un controlador de OData

Un *controlador* es una clase que controla las solicitudes HTTP. Definir un controlador independiente para cada conjunto de entidades en que el servicio de OData. En este tutorial, vamos a crear un único controlador.

En el Explorador de soluciones, haga clic en la carpeta Controllers. Seleccione **agregar** y, a continuación, seleccione **controlador**.

![](creating-an-odata-endpoint/_static/image5.png)

En el **agregar Scaffold** cuadro de diálogo, seleccione &quot;controlador Web API 2 OData con acciones mediante Entity Framework&quot;.

![](creating-an-odata-endpoint/_static/image6.png)

En el **Agregar controlador** cuadro de diálogo, el nombre del controlador "ProductsController". Seleccione el &quot;usar acciones de controlador asincrónicas&quot; casilla de verificación. En el **modelo** lista desplegable, seleccione la clase de producto.

![](creating-an-odata-endpoint/_static/image7.png)

Haga clic en el **nuevo contexto de datos...**  botón. Deje el nombre predeterminado para el tipo de contexto de datos y haga clic en **agregar**.

![](creating-an-odata-endpoint/_static/image8.png)

Haga clic en Agregar en el cuadro de diálogo Agregar controlador para agregar el controlador.

![](creating-an-odata-endpoint/_static/image9.png)

Nota: Si recibe un mensaje de error que dice &quot;se produjo un error al obtener el tipo... &quot;, asegúrese de que ha generado el proyecto de Visual Studio después de agregar la clase de producto. El scaffolding usa la reflexión para encontrar la clase.

![](creating-an-odata-endpoint/_static/image10.png)

El scaffolding agrega dos archivos de código al proyecto:

- Products.cs define el controlador de API Web que implementa el extremo de OData.
- ProductServiceContext.cs proporciona métodos para consultar la base de datos subyacente mediante Entity Framework.

![](creating-an-odata-endpoint/_static/image11.png)

<a id="edm"></a>
## <a name="add-the-edm-and-route"></a>Agregue el EDM y ruta

En el Explorador de soluciones, expanda la aplicación\_carpeta Inicio y abra el archivo WebApiConfig.cs. Esta clase contiene el código de configuración de Web API. Reemplace este código con lo siguiente:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample2.cs)]

Este código hace dos cosas:

- Crea un Entity Data Model (EDM) para el extremo de OData.
- Agrega una ruta para el punto de conexión.

Un EDM es un modelo abstracto de los datos. El EDM se usa para crear el documento de metadatos y definir a los URI para el servicio. El **ODataConventionModelBuilder** crea un modelo EDM utilizando un conjunto de convenciones de nomenclatura predeterminadas EDM. Este enfoque requiere menos código. Si desea más control sobre el modelo EDM, puede usar el **ODataModelBuilder** clase para crear el EDM agregando explícitamente las propiedades, claves y las propiedades de navegación.

El **EntitySet** método agrega un conjunto de entidades al EDM:

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample3.cs)]

La cadena "Productos" define el nombre del conjunto de entidades. El nombre del controlador debe coincidir con el nombre del conjunto de entidades. En este tutorial, el conjunto de entidades se denomina "Productos" y el controlador se denomina `ProductsController`. Si el nombre de conjunto de entidades "Conjunto de productos", podría denominar el controlador `ProductSetController`. Tenga en cuenta que un punto de conexión puede tener varios conjuntos de entidades. Llame a **EntitySet&lt;T&gt;**  para cada entidad establecer y, a continuación, defina un controlador correspondiente.

El **MapODataRoute** método agrega una ruta para el extremo de OData.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample4.cs)]

El primer parámetro es un nombre descriptivo para la ruta. Los clientes del servicio no ven este nombre. El segundo parámetro es el prefijo URI para el punto de conexión. Dado que este código, el URI para el conjunto de entidades de productos es http://<em>hostname</em>  /odata o productos. La aplicación puede tener más de un extremo de OData. Para cada punto de conexión, llame a <strong>MapODataRoute</strong> y proporcione un nombre de ruta único y un prefijo de identificador URI único.

<a id="seed-db"></a>
## <a name="seed-the-database-optional"></a>Valor de inicialización de la base de datos (opcional)

En este paso, usará Entity Framework para inicializar la base de datos con algunos datos de prueba. Este paso es opcional, pero le permite probar el punto de conexión de OData de inmediato.

Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**, a continuación, seleccione **Package Manager Console**. En la ventana de consola de administrador de paquetes, escriba el siguiente comando:

[!code-console[Main](creating-an-odata-endpoint/samples/sample5.cmd)]

Esto agrega una carpeta denominada migraciones y el archivo de código Configuration.cs.

![](creating-an-odata-endpoint/_static/image12.png)

Abra este archivo y agregue el código siguiente a la `Configuration.Seed` método.

[!code-csharp[Main](creating-an-odata-endpoint/samples/sample6.cs)]

En la ventana de consola de administrador de paquetes, escriba los siguientes comandos:

[!code-console[Main](creating-an-odata-endpoint/samples/sample7.cmd)]

Estos comandos generan código que crea la base de datos y, a continuación, ejecuta ese código.

<a id="explore"></a>
## <a name="exploring-the-odata-endpoint"></a>Explorar el extremo de OData

En esta sección, vamos a usar el [Fiddler Web Debugging Proxy](http://www.fiddler2.com) para enviar solicitudes al punto de conexión y examinar los mensajes de respuesta. Esto le ayudará a comprender las capacidades de un extremo de OData.

En Visual Studio, presione F5 para iniciar la depuración. De forma predeterminada, Visual Studio abre el explorador en `http://localhost:*port*`, donde *puerto* es el número de puerto configurado en la configuración del proyecto.

Puede cambiar el número de puerto en la configuración del proyecto. En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**. En la ventana Propiedades, seleccione **Web**. Escriba el número de puerto en **dirección Url del proyecto**.

### <a name="service-document"></a>Documento de servicio

El *documento de servicio* contiene una lista de los conjuntos de entidades para el extremo de OData. Para obtener el documento de servicio, envía una solicitud GET al URI del servicio en la raíz.

Uso de Fiddler, escriba el siguiente URI en el **Composer** ficha: `http://localhost:port/odata/`, donde *puerto* es el número de puerto.

![](creating-an-odata-endpoint/_static/image13.png)

Haga clic en el **Execute** botón. Fiddler envía una solicitud HTTP GET a la aplicación. Debería ver la respuesta en la lista de sesiones de la Web. Si todo funciona, el código de estado será de 200.

![](creating-an-odata-endpoint/_static/image14.png)

Haga doble clic en la respuesta de la lista de sesiones Web para ver los detalles del mensaje de respuesta en la pestaña inspectores.

![](creating-an-odata-endpoint/_static/image15.png)

El mensaje de respuesta HTTP sin formato debe ser similar al siguiente:

[!code-console[Main](creating-an-odata-endpoint/samples/sample8.cmd)]

De forma predeterminada, API Web devuelve el documento de servicio en formato AtomPub. Para solicitar JSON, agregue el siguiente encabezado a la solicitud HTTP:

`Accept: application/json`

![](creating-an-odata-endpoint/_static/image16.png)

Ahora, la respuesta HTTP contiene una carga JSON:

[!code-console[Main](creating-an-odata-endpoint/samples/sample9.cmd)]

### <a name="service-metadata-document"></a>Documento de metadatos de servicio

El *documento de metadatos del servicio* describe el modelo de datos del servicio, mediante un lenguaje XML llamado el lenguaje de definición de esquemas conceptuales (CSDL). El documento de metadatos muestra la estructura de los datos en el servicio y puede usarse para generar código de cliente.

Para obtener el documento de metadatos, envíe una solicitud GET a `http://localhost:port/odata/$metadata`. Estos los metadatos para el extremo que se muestra en este tutorial.

[!code-console[Main](creating-an-odata-endpoint/samples/sample10.cmd)]

### <a name="entity-set"></a>Conjunto de entidades

Para obtener el conjunto de entidades de productos, envíe una solicitud GET a `http://localhost:port/odata/Products`.

[!code-console[Main](creating-an-odata-endpoint/samples/sample11.cmd)]

### <a name="entity"></a>Entity

Para obtener un producto individual, envíe una solicitud GET a `http://localhost:port/odata/Products(1)`, donde "1" es el identificador de producto.

[!code-console[Main](creating-an-odata-endpoint/samples/sample12.cmd)]

<a id="formats"></a>
## <a name="odata-serialization-formats"></a>Formatos de serialización de OData

OData admite varios formatos de serialización:

- Publicación de Atom (XML)
- JSON "light" (introducida en OData v3)
- JSON "detallado" (OData v2)

De forma predeterminada, API Web usa el formato de AtomPubJSON "light". 

Para obtener el formato AtomPub, establezca el encabezado Accept "aplicación/Atom+XML". Este es un cuerpo de respuesta de ejemplo:

[!code-console[Main](creating-an-odata-endpoint/samples/sample13.cmd)]

Puede ver una desventaja obvia del formato de Atom: es mucho más detallado que la luz JSON. Sin embargo, si tiene un cliente que entiende AtomPub, el cliente puede preferir ese formato JSON.

Esta es la versión ligera de JSON de la misma entidad:

[!code-console[Main](creating-an-odata-endpoint/samples/sample14.cmd)]

El formato JSON light se introdujo en la versión 3 del Protocolo OData. Por compatibilidad con versiones anteriores, un cliente puede solicitar el formato JSON "detallado" anterior. Para solicitar JSON detallado, establezca el encabezado Accept en `application/json;odata=verbose`. Esta es la versión detallada:

[!code-console[Main](creating-an-odata-endpoint/samples/sample15.cmd)]

Este formato transmite más metadatos en el cuerpo de respuesta, que puede agregar una sobrecarga considerable a través de una sesión completa. Además, agrega un nivel de direccionamiento indirecto ajustando el objeto en una propiedad denominada "d".

## <a name="next-steps"></a>Pasos siguientes

- [Agregar las relaciones de entidad](working-with-entity-relations.md)
- [Agregar acciones de OData](odata-actions.md)
- [Llame al servicio de OData desde un cliente .NET](calling-an-odata-service-from-a-net-client.md)
