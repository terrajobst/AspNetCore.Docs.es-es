---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
title: Enrutamiento de direcciones URL | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 4f4bf092-c400-471f-a876-78fda0417890
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing
msc.type: authoredcontent
ms.openlocfilehash: d9f0779d560d6ec7796a16dc2996b959dd171c80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828352"
---
<a name="url-routing"></a>Enrutamiento de direcciones URL
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible para acompañar esta serie de tutoriales.


En este tutorial, modificará la aplicación de ejemplo Wingtip Toys para admitir el enrutamiento de direcciones URL. Enrutamiento permite que la aplicación web utilizar direcciones URL que descriptivo, más fácil de recordar sean mejor admitidas por los motores de búsqueda. Este tutorial se basa en el tutorial anterior, "Pertenencia y administración" y forma parte de la serie de tutoriales de Wingtip Toys.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo registrar las rutas de una aplicación de formularios Web Forms de ASP.NET.
- Cómo agregar rutas a una página web.
- Cómo seleccionar datos de una base de datos para admitir las rutas.

## <a name="aspnet-routing-overview"></a>Introducción al enrutamiento de ASP.NET

Enrutamiento de direcciones URL le permite configurar una aplicación para aceptar solicitudes de direcciones URL que no se asignan a los archivos físicos. Una dirección URL de solicitud es simplemente la dirección URL de un usuario escribe en su explorador para buscar una página en el sitio web. Utilizar el enrutamiento para definir direcciones URL que son semánticamente significativos para los usuarios y que puede ayudarle con la optimización de motor de búsqueda (SEO).

De forma predeterminada, la plantilla de formularios Web Forms incluye [ASP.NET Friendly URLs](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/). Gran parte del trabajo de enrutamiento básico se implementará mediante el uso de *Friendly URLs*. Sin embargo, en este tutorial agregará las funcionalidades de enrutamiento personalizadas.

Antes de personalizar el enrutamiento de direcciones URL, la aplicación de ejemplo Wingtip Toys puede vincular a un producto con la dirección URL siguiente:

`https://localhost:44300/ProductDetails.aspx?productID=2`

Al personalizar el enrutamiento de direcciones URL, la aplicación de ejemplo Wingtip Toys se vinculará al mismo producto mediante una más fácil de leer la dirección URL:

`https://localhost:44300/Product/Convertible%20Car`

### <a name="routes"></a>Rutas

Una ruta es un patrón de dirección URL que se asigna a un controlador. El controlador puede ser un archivo físico, como un archivo .aspx en una aplicación de formularios Web Forms. Un controlador también puede ser una clase que procesa la solicitud. Para definir una ruta, cree una instancia de la clase de ruta especificando el patrón de dirección URL, el controlador y, opcionalmente, un nombre para la ruta.

Agregue la ruta a la aplicación agregando el `Route` objeto estático `Routes` propiedad de la `RouteTable` clase. La propiedad de las rutas es una `RouteCollection` objeto que almacena todas las rutas para la aplicación.

### <a name="url-patterns"></a>Patrones de URL

Un patrón de dirección URL puede contener valores literales y marcadores de posición de variables (denominados parámetros de URL). Los literales y marcadores de posición se encuentran en segmentos de la dirección URL que están delimitadas por la barra diagonal (`/`) caracteres.

Cuando se realiza una solicitud a la aplicación web, se analiza la dirección URL en segmentos y marcadores de posición, y se proporcionan los valores de variable para el controlador de solicitudes. Este proceso es similar a la forma en que los datos en una cadena de consulta se analiza y se pasa al controlador de solicitudes. En ambos casos, información sobre las variable está incluido en la dirección URL y pasa al controlador en forma de pares clave-valor. Para las cadenas de consulta, las claves y los valores están en la dirección URL. Para las rutas, las claves son los nombres de marcador de posición en el patrón de dirección URL y solo los valores están en la dirección URL.

En un modelo de dirección URL, definir los marcadores de posición encerrándolos entre llaves ( `{` y `}` ). Puede definir más de un marcador de posición en un segmento, pero los marcadores de posición deben estar separados por un valor literal. Por ejemplo, `{language}-{country}/{action}` es un patrón de ruta válido. Sin embargo, `{language}{country}/{action}` no es un patrón válido, porque no hay ningún valor literal o un delimitador entre los marcadores de posición. Por lo tanto, el enrutamiento no puede determinar dónde debe separar el valor del marcador de posición de idioma desde el valor del marcador de posición de país.

### <a name="mapping-and-registering-routes"></a>Asignación y registrar las rutas

Antes de poder incluir rutas a las páginas de la aplicación de ejemplo Wingtip Toys, debe registrar las rutas cuando se inicia la aplicación. Para registrar las rutas, modificará el `Application_Start` controlador de eventos.

1. En **el Explorador de soluciones**de Visual Studio, busque y abra el *Global.asax.cs* archivo.
2. Agregue el código resaltado en amarillo en la *Global.asax.cs* archivo como sigue:   

    [!code-csharp[Main](url-routing/samples/sample1.cs?highlight=30-31,34-46)]

Cuando se inicia la aplicación de ejemplo la Wingtip Toys, llama a la `Application_Start` controlador de eventos. Al final de este controlador de eventos, el `RegisterCustomRoutes` se llama al método. El `RegisterCustomRoutes` método agrega cada ruta mediante una llamada a la `MapPageRoute` método de la `RouteCollection` objeto. Las rutas se definen mediante un nombre de ruta, una dirección URL de ruta y una dirección URL física.

El primer parámetro ("`ProductsByCategoryRoute`") es el nombre de ruta. Sirve para llamar a la ruta cuando sea necesario. El segundo parámetro ("`Category/{categoryName}`") define la dirección URL que puede ser dinámico según el código de sustitución descriptiva. Usar esta ruta cuando va a llenar un control de datos con vínculos que se generan según los datos. Una ruta se muestra como sigue:

[!code-csharp[Main](url-routing/samples/sample2.cs)]

El segundo parámetro de la ruta incluye un valor dinámico especificado por llaves (`{ }`). En este caso, el `categoryName` es una variable que se usará para determinar la ruta de acceso de enrutamiento adecuada.

> [!NOTE] 
> 
> **Opcional**
> 
> Le resultará más fácil de administrar el código al mover el `RegisterCustomRoutes` método en una clase independiente. En el *lógica* carpeta, cree otro `RouteActions` clase. Mover el anterior `RegisterCustomRoutes` método desde el *Global.asax.cs* archivo en el nuevo `RoutesActions` clase. Use la `RoleActions` clase y el `createAdmin` método como un ejemplo de cómo llamar a la `RegisterCustomRoutes` método desde el *Global.asax.cs* archivo.


Es posible que también haya observado el `RegisterRoutes` llamada de método con el `RouteConfig` objeto situado al principio de la `Application_Start` controlador de eventos. Esta llamada se realiza para implementar el enrutamiento predeterminado. Se ha incluido como código de forma predeterminada al crear la aplicación mediante la plantilla de formularios Web Forms de Visual Studio.

## <a name="retrieving-and-using-route-data"></a>Recuperar y utilizar los datos de ruta

Como se mencionó anteriormente, se pueden definir las rutas. El código que agregó a la `Application_Start` controlador de eventos en el *Global.asax.cs* las rutas definibles por el archivo carga.

### <a name="setting-routes"></a>Rutas de configuración

Rutas requieren que se va a agregar código adicional. En este tutorial, utilizará el enlace de modelos para recuperar un `RouteValueDictionary` objeto que se usa al generar las rutas con los datos de un control de datos. La `RouteValueDictionary` objeto contendrá una lista de nombres de productos que pertenecen a una categoría de productos específica. Se crea un vínculo para cada producto en función de los datos y la ruta.

#### <a name="enable-routes-for-categories-and-products"></a>Habilitar las rutas para las categorías y productos

A continuación, actualizará la aplicación para usar el `ProductsByCategoryRoute` para determinar la ruta correcta para incluir para cada vínculo de categoría de producto. También actualizará el *ProductList.aspx* página para incluir un vínculo enrutado para cada producto. Los vínculos se mostrará como eran antes del cambio, sin embargo, los vínculos, ahora se usarán enrutamiento de direcciones URL.

1. En **el Explorador de soluciones**, abra el *Site.Master* página si no está ya abierto.
2. Actualización de la **ListView** control denominado "`categoryList`" con los cambios que se resaltan en amarillo, por lo que el marcado aparece como sigue:   

    [!code-aspx[Main](url-routing/samples/sample3.aspx?highlight=7-9)]
3. En **el Explorador de soluciones**, abra el *ProductList.aspx* página.
4. Actualización de la `ItemTemplate` elemento de la *ProductList.aspx* página con las actualizaciones que se resaltan en amarillo, por lo que el marcado aparece como sigue:   

    [!code-aspx[Main](url-routing/samples/sample4.aspx?highlight=6-9,14-16)]
5. Abra el código subyacente de *ProductList.aspx.cs* y agregue el siguiente espacio de nombres como señalado en amarillo:  

    [!code-csharp[Main](url-routing/samples/sample5.cs?highlight=9)]
6. Reemplace el `GetProducts` método de código subyacente (*ProductList.aspx.cs*) con el código siguiente:   

    [!code-csharp[Main](url-routing/samples/sample6.cs)]

#### <a name="add-code-for-product-details"></a>Agregue código para obtener más información de producto

Ahora, actualice el código subyacente (*ProductDetails.aspx.cs*) para el *ProductDetails.aspx* página va a usar los datos de ruta. Tenga en cuenta que el nuevo `GetProduct` método también acepta un valor de cadena de consulta para el caso donde el usuario tenga un vínculo de un marcador que se usa la dirección de URL no preparadas, no enrutadas anterior.

1. Reemplace el `GetProduct` método de código subyacente (*ProductDetails.aspx.cs*) con el código siguiente:   

    [!code-csharp[Main](url-routing/samples/sample7.cs)]

## <a name="running-the-application"></a>Ejecutar la aplicación

Puede ejecutar la aplicación ahora para ver las rutas actualizadas.

1. Presione **F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
 El explorador se abre y muestra el *Default.aspx* página.
2. Haga clic en el **productos** vínculo en la parte superior de la página.  
 Todos los productos se muestran en el *ProductList.aspx* página. Para el explorador se muestra la siguiente dirección URL (con el número de puerto):  
    `https://localhost:44300/ProductList`
3. A continuación, haga clic en el **automóviles** vínculo de categoría en la parte superior de la página.  
 Solo los automóviles se muestran en el *ProductList.aspx* página. Para el explorador se muestra la siguiente dirección URL (con el número de puerto):  
    `https://localhost:44300/Category/Cars`
4. Haga clic en el vínculo que contiene el nombre del primer automóvil aparece en la página ("**automóvil Convertible**") para mostrar los detalles del producto.  
 Para el explorador se muestra la siguiente dirección URL (con el número de puerto):  
    `https://localhost:44300/Product/Convertible%20Car`
5. A continuación, escriba la siguiente URL no enrutadas (con el número de puerto) en el explorador:  
    `https://localhost:44300/ProductDetails.aspx?productID=2`  
 El código aún reconoce una dirección URL que incluya una cadena de consulta para el caso de que un usuario tiene un vínculo de un marcador.

## <a name="summary"></a>Resumen

En este tutorial, ha agregado las rutas para las categorías y productos. Ha aprendido cómo las rutas se pueden integrar con controles de datos que utilizan el enlace de modelos. En el siguiente tutorial, implementará el control de errores global.

## <a name="additional-resources"></a>Recursos adicionales

[ASP.NET Friendly URLs](http://www.nuget.org/packages/Microsoft.AspNet.FriendlyUrls/)  
[Implementar una aplicación de formularios Web ASP.NET segura con pertenencia, OAuth y SQL Database en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)  
[Microsoft Azure - evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/)

> [!div class="step-by-step"]
> [Anterior](membership-and-administration.md)
> [Siguiente](aspnet-error-handling.md)
