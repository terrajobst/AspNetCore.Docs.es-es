---
uid: web-pages/overview/api-reference/asp-net-web-pages-api-reference
title: Referencia de API rápida (Razor) de ASP.NET Web Pages | Documentos de Microsoft
author: tfitzmac
description: Esta página contiene una lista con ejemplos breves de los objetos usados más habitualmente, propiedades y métodos de programación en ASP.NET Web Pages con sintaxis Razor.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: 4001cb9b-3bfd-4ace-8a89-1561d8421e2c
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/api-reference/asp-net-web-pages-api-reference
msc.type: authoredcontent
ms.openlocfilehash: 5f9d84f4d453583d7d4eae12e4fc510275255616
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30897589"
---
<a name="aspnet-web-pages-razor-api-quick-reference"></a>Referencia de API rápida (Razor) de ASP.NET Web Pages
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Esta página contiene una lista con ejemplos breves de los objetos usados más habitualmente, propiedades y métodos de programación en ASP.NET Web Pages con sintaxis Razor.
> 
> Descripciones marcadas con "(v2)" se introdujeron en ASP.NET Web Pages versión 2.
> 
> Para obtener documentación de referencia de API, consulte el [documentación de referencia de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=208659) en MSDN.
> 
> ## <a name="software-versions"></a>Versiones de software
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2 y páginas Web de ASP.NET 1.0 (excepto funciones marcadas v2).


Esta página contiene información de referencia para lo siguiente:

- [Clases](#Classes)
- [Data](#Data)
- [Aplicaciones auxiliares](#Helpers)
- [Validación](#Validation)

<a id="Classes"></a>
## <a name="classes"></a>Clases

### `AppState[key], AppState[index],App`

Contiene los datos que pueden ser compartidas por todas las páginas en la aplicación. Puede usar dinámico `App` propiedad para tener acceso a los mismos datos, como en el ejemplo siguiente:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample1.html)]

### `AsBool(), AsBool(true|false)`

Convierte un valor de cadena en un valor booleano (verdadero/falso). Devuelve false o el valor especificado si la cadena no representa true/false.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample2.cs)]

### `AsDateTime(), AsDateTime(value)`

Convierte un valor de cadena en fecha y hora. Devuelve `DateTime.MinValue` o el valor especificado si la cadena no representa una fecha y hora.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample3.cs)]

### `AsDecimal(), AsDecimal(value)`

Convierte un valor de cadena en un valor decimal. Devuelve 0,0 o el valor especificado si la cadena no representa un valor decimal.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample4.cs)]

### `AsFloat(), AsFloat(value)`

Convierte un valor de cadena en un valor flotante. Devuelve 0,0 o el valor especificado si la cadena no representa un valor decimal.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample5.cs)]

### `AsInt(), AsInt(value)`

Convierte un valor de cadena en un entero. Devuelve 0 o el valor especificado si la cadena no representa un número entero.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample6.cs)]

### `Href(path [, param1 [, param2]])`

Crea una dirección URL compatible con el Explorador de una ruta de acceso de archivo local, con elementos de la ruta de acceso adicional opcional.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample7.cshtml)]

### `Html.Raw(value)`

Representa *valor* como marca HTML en lugar de representar HTML codificado como salida.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample8.cshtml)]

### `IsBool(), IsDateTime(), IsDecimal(), IsFloat(), IsInt()`

Devuelve true si se puede convertir el valor de una cadena al tipo especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample9.cs)]

### `IsEmpty()`

Devuelve true si el objeto o la variable no tiene ningún valor.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample10.cs)]

### `IsPost`

Devuelve true si la solicitud es una entrada de blog. (Las solicitudes iniciales normalmente son una operación GET).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample11.cs)]

### `Layout`

Especifica la ruta de acceso de una página de diseño que se aplican a esta página.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample12.html)]

### `PageData[key], PageData[index],Page`

Contiene datos que comparten el página, las páginas de diseño y las páginas parciales en la solicitud actual. Puede usar dinámico `Page` propiedad para tener acceso a los mismos datos, como en el ejemplo siguiente:

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample13.html)]

### `RenderBody()`

(Páginas de diseño) Presenta el contenido de una página de contenido que no está en ninguna sección con nombre.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample14.cs)]

### `RenderPage(path, values)`  
`RenderPage(path[,param1 [, param2]])`

Representa una página de contenido mediante la ruta de acceso especificada y datos adicionales opcionales. Puede obtener los valores de los parámetros adicionales de `PageData` por posición (ejemplo 1) o clave (ejemplo 2).

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample15.js)]

### `RenderSection(sectionName [, required = true|false])`

(Páginas de diseño) Representa una sección de contenido que tiene un nombre. Establecer *requiere* como false para que una sección opcional.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample16.js)]

### `Request.Cookies[key]`

Obtiene o establece el valor de una cookie HTTP.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample17.cs)]

### `Request.Files[key]`

Obtiene los archivos que se cargaron en la solicitud actual.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample18.js)]

### `Request.Form[key]`

Obtiene los datos que se ha registrado en un formulario (como cadenas). `Request[key]` comprueba tanto el `Request.Form` y `Request.QueryString` colecciones.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample19.cs)]

### `Request.QueryString[key]`

Obtiene los datos que se especificó en la cadena de consulta de dirección URL. `Request[key]` comprueba tanto el `Request.Form` y `Request.QueryString` colecciones.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample20.cs)]

### `Request.Unvalidated(key)`  
`Request.Unvalidated().QueryString|Form|Cookies|Headers[key]`

Selectivamente deshabilita la validación de solicitudes para un elemento form, el valor de cadena de consulta, la cookie o el valor del encabezado. Validación de solicitudes está habilitada de forma predeterminada y evita que los usuarios enviar marcado u otro contenido potencialmente peligroso.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample21.cs)]

### `Response.AddHeader(name, value)`

Agrega un encabezado de servidor HTTP a la respuesta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample22.cs)]

### `Response.OutputCache(seconds [, sliding] [, varyByParams])`

Almacena en caché el resultado de la página durante un tiempo especificado. Si lo desea establezca *deslizante* para restablecer el tiempo de espera en cada acceso a la página y *varyByParams* para almacenar en caché diferentes versiones de la página para cada cadena de consulta diferentes en la solicitud de página.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample23.js)]

### `Response.Redirect(path)`

Redirige la solicitud del explorador a una nueva ubicación.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample24.js)]

### `Response.SetStatus(httpStatusCode)`

Establece el código de estado HTTP enviado al explorador.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample25.cs)]

### `Response.WriteBinary(data [, mimetype])`

Escribe el contenido de *datos* a la respuesta con un tipo MIME opcional.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample26.js)]

### `Response.WriteFile(file)`

Escribe el contenido de un archivo en la respuesta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample27.cs)]

### `@section(sectionName) {content }`

(Páginas de diseño) Define una sección de contenido que tiene un nombre.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample28.cshtml)]

### `Server.HtmlDecode(htmlText)`

Descodifica una cadena que está codificado en HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample29.cs)]

### `Server.HtmlEncode(text)`

Codifica una cadena para la representación en formato HTML.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample30.cs)]

### `Server.MapPath(virtualPath)`

Devuelve la ruta de acceso física de servidor para la ruta de acceso virtual especificada.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample31.cs)]

### `Server.UrlDecode(urlText)`

Descodifica el texto desde una dirección URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample32.cs)]

### `Server.UrlEncode(text)`

Codifica el texto que se va a colocar en una dirección URL.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample33.cs)]

### `Session[key]`

Obtiene o establece un valor que existe hasta que el usuario cierra el explorador.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample34.css)]

### `ToString()`

Muestra una representación de cadena del valor de objeto.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample35.html)]

### `UrlData[index]`

Obtiene los datos adicionales desde la dirección URL (por ejemplo, */MyPage/ExtraData*).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample36.cs)]

### `WebSecurity.ChangePassword(userName,currentPassword,newPassword)`

Cambia la contraseña para el usuario especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample37.cs)]

### `WebSecurity.ConfirmAccount(accountConfirmationToken)`

Confirma una cuenta con el token de confirmación de cuenta.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample38.cs)]

### `WebSecurity.CreateAccount(userName, password`  
 `[, requireConfirmationToken = true|false])`

Crea una nueva cuenta de usuario con el nombre de usuario especificado y la contraseña. Para solicitar un token de confirmación, pasar true para *requireConfirmationToken.*

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample39.cs)]

### `WebSecurity.CurrentUserId`

Obtiene el identificador entero para el usuario ha iniciado sesión actualmente.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample40.cs)]

### `WebSecurity.CurrentUserName`

Obtiene el nombre del usuario que ha iniciado sesión actualmente.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample41.cs)]

### `WebSecurity.GeneratePasswordResetToken(username`  
 `[, tokenExpirationInMinutesFromNow])`

Genera un token de restablecimiento de contraseña que se puede enviar por correo electrónico a un usuario para que el usuario puede restablecer la contraseña.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample42.cs)]

### `WebSecurity.GetUserId(userName)`

Devuelve el identificador de usuario del nombre de usuario.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample43.cs)]

### `WebSecurity.IsAuthenticated`

Devuelve true si el usuario actual ha iniciado sesión.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample44.cs)]

### `WebSecurity.IsConfirmed(userName)`

Devuelve true si el usuario se ha confirmado (por ejemplo, a través de un correo electrónico de confirmación).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample45.cs)]

### `WebSecurity.IsCurrentUser(userName)`

Devuelve true si el nombre del usuario actual coincide con el nombre de usuario especificado.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample46.cs)]

### `WebSecurity.Login(userName,password[, persistCookie])`

Registra el usuario en estableciendo un token de autenticación en la cookie.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample47.cs)]

### `WebSecurity.Logout()`

Registra el usuario horizontal mediante la eliminación de la cookie de token de autenticación.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample48.css)]

### `WebSecurity.RequireAuthenticatedUser()`

Si el usuario no está autenticado, Establece el estado HTTP en 401 (no autorizado).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample49.css)]

### `WebSecurity.RequireRoles(roles)`

Si el usuario actual no es un miembro de uno de los roles especificados, Establece el estado de HTTP en 401 (no autorizado).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample50.html)]

### `WebSecurity.RequireUser(userId)`  
`WebSecurity.RequireUser(userName)`

Si el usuario actual no es el usuario especificado por *nombre de usuario*, Establece el estado de HTTP en 401 (no autorizado).

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample51.css)]

### `WebSecurity.ResetPassword(passwordResetToken,newPassword)`

Si el token de restablecimiento de contraseña es válido, cambia la contraseña del usuario a la nueva contraseña.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample52.css)]

<a id="Data"></a>
## <a name="data"></a>Datos

### `Database.Execute(SQLstatement [,parameters]`

Ejecuta *SQLstatement* (con parámetros opcionales), como INSERT, DELETE o UPDATE y devuelve un recuento de los registros afectados.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample53.sql)]

### `Database.GetLastInsertId()`

Devuelve la columna de identidad de la fila insertada más recientemente.

[!code-sql[Main](asp-net-web-pages-api-reference/samples/sample54.sql)]

### `Database.Open(filename)`  
`Database.Open(connectionStringName)`

Se abre el archivo de base de datos especificada o la base de datos especificada mediante una cadena de conexión con nombre de la *Web.config* archivo.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample55.cs)]

### `Database.OpenConnectionString(connectionString)`

Se abre una base de datos mediante la cadena de conexión. (Esto contrasta con `Database.Open`, que utiliza un nombre de cadena de conexión.)

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample56.cs)]

### `Database.Query(SQLstatement[,parameters])`

Consultas de la base de datos con *SQLstatement* (opcionalmente pasa parámetros) y devuelve los resultados como una colección.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample57.html)]

### `Database.QuerySingle(SQLstatement [, parameters])`

Ejecuta *SQLstatement* (con parámetros opcionales) y devuelve un único registro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample58.cs)]

### `Database.QueryValue(SQLstatement [, parameters])`

Ejecuta *SQLstatement* (con parámetros opcionales) y devuelve un valor único.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample59.cs)]

<a id="Helpers"></a>
## <a name="helpers"></a>Aplicaciones auxiliares

### `Analytics.GetGoogleHtml(webPropertyId)`

Representa el código de JavaScript de Google Analytics para el identificador especificado.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample60.js)]

### `Analytics.GetStatCounterHtml(project,security)`

Representa el código de JavaScript de análisis de StatCounter para el proyecto especificado.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample61.css)]

### `Analytics.GetYahooHtml(account)`

Representa el código de JavaScript de análisis de Yahoo para la cuenta especificada.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample62.js)]

### `Bing.SearchBox([boxWidth])`

Pasa una búsqueda de Bing. Para especificar el sitio para buscar y un título para el cuadro de búsqueda, puede establecer la `Bing.SiteUrl` y `Bing.SiteTitle` propiedades. Normalmente, debe establecer estas propiedades el  *\_AppStart* página.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample63.html)]

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample64.cshtml)]

### `Chart(width,height [, template] [, templatePath])`

Inicializa un gráfico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample65.cshtml)]

### `Chart.AddLegend([title] [, name])`

Agrega una leyenda a un gráfico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample66.cshtml)]

### `Chart.AddSeries([name] [, chartType] [, chartArea]`  
 `[, axisLabel] [, legend] [, markerStep] [, xValue]`  
 `[, xField] [, yValues] [, yFields] [, options])`

Agrega una serie de valores al gráfico.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample67.cshtml)]

### `Crypto.Hash(string [, algorithm])`  
`Crypto.Hash(bytes [, algorithm])`

Devuelve un valor hash de los datos especificados. El algoritmo predeterminado es `sha256`.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample68.html)]

### `Facebook.LikeButton(href [, buttonLayout] [, showFaces] [, width] [, height]`   
 `[, action] [, font] [, colorScheme] [, refLabel])`

Permite a los usuarios de Facebook establecer una conexión a las páginas.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample69.js)]

### `FileUpload.GetHtml([initialNumberOfFiles] [, allowMoreFilesToBeAdded]`  
 `[, includeFormTag] [, addText] [, uploadText])`

Representa la interfaz de usuario para cargar archivos.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample70.html)]

### `GamerCard.GetHtml(gamerTag)`

Presenta la etiqueta de juegos de Xbox especificada.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample71.js)]

### `Gravatar.GetHtml(email [, imageSize] [, defaultImage] [, rating]`  
 `[, imageExtension] [, attributes])`

Presenta la imagen Gravatar para la dirección de correo electrónico especificada.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample72.css)]

### `Json.Encode(object)`

Convierte un objeto de datos en una cadena con el formato JavaScript Object Notation (JSON).

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample73.cs)]

### `Json.Decode(string)`

Convierte una cadena de entrada codificada en JSON en un objeto de datos que puede recorrer en iteración o insertar en una base de datos.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample74.cs)]

### `LinkShare.GetHtml(pageTitle[, pageLinkBack] [, twitterUserName]`  
 `[, additionalTweetText] [, linkSites])`

Representa los vínculos de red sociales mediante la dirección URL opcional y el título especificados.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample75.xml)]

### `ModelStateDictionary.AddError(key, errorMessage)`

Asocia un mensaje de error con un campo de formulario. Use la `ModelState` auxiliar para tener acceso a este miembro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample76.cs)]

### `ModelStateDictionary.AddFormError(errorMessage)`

Asocia un mensaje de error a un formulario. Use la `ModelState` auxiliar para tener acceso a este miembro.

[!code-powershell[Main](asp-net-web-pages-api-reference/samples/sample77.ps1)]

### `ModelStateDictionary.IsValid`

Devuelve true si no hay ningún error de validación. Use la `ModelState` auxiliar para tener acceso a este miembro.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample78.cs)]

### `ObjectInfo.Print(value [, depth] [, enumerationLength])`

Representa las propiedades y valores de un objeto y todos los objetos.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample79.css)]

### `Recaptcha.GetHtml([, publicKey] [, theme] [, language] [, tabIndex])`

Representa la prueba de comprobación de reCAPTCHA.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample80.css)]

### `ReCaptcha.PublicKey`  
 `ReCaptcha.PrivateKey`

Establece las claves públicas y privadas para el servicio de reCAPTCHA. Normalmente, debe establecer estas propiedades el  *\_AppStart* página.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample81.css)]

### `ReCaptcha.Validate([, privateKey])`

Devuelve el resultado de la prueba de reCAPTCHA.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample82.cs)]

### `ServerInfo.GetHtml()`

Representa información de estado sobre ASP.NET Web Pages.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample83.cshtml)]

### `Twitter.Profile(twitterUserName)`

Representa un flujo de Twitter para el usuario especificado.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample84.js)]

### `Twitter.Search(searchQuery)`

Representa un flujo de Twitter para el texto de búsqueda especificado.

[!code-xml[Main](asp-net-web-pages-api-reference/samples/sample85.xml)]

### `Video.Flash(filename [, width, height])`

Representa un Reproductor de vídeo Flash para el archivo especificado con opcional width y height.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample86.cshtml)]

### `Video.MediaPlayer(filename [, width, height])`

Representa un reproductor multimedia de Windows para el archivo especificado con opcional width y height.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample87.cshtml)]

### `Video.Silverlight(filename, width, height)`

Representa un Reproductor de Silverlight para el elemento especificado *.xap* archivo con el ancho y alto necesarios.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample88.cshtml)]

### `WebCache.Get(key)`

Devuelve el objeto especificado por *clave*, o null si no se encuentra el objeto.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample89.cs)]

### `WebCache.Remove(key)`

Quita el objeto especificado por *clave* de la memoria caché.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample90.cs)]

### `WebCache.Set(key, value [, minutesToCache] [, slidingExpiration])`

Coloca *valor* en la memoria caché en el nombre especificado por *clave*.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample91.html)]

### `WebGrid(data)`

Crea un nuevo `WebGrid` utilizando los datos de una consulta de objeto.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample92.cs)]

### `WebGrid.GetHtml()`

Representa el marcado para mostrar datos en una tabla HTML.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample93.html)]

### `WebGrid.Pager()`

Representa un elemento de paginación para la `WebGrid` objeto.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample94.html)]

### `WebImage(path)`

Carga una imagen desde la ruta de acceso especificada.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample95.cs)]

### `WebImage.AddImagesWatermark(image)`

Agrega la imagen especificada como una marca de agua.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample96.cs)]

### `WebImage.AddTextWatermark(text)`

Agrega el texto especificado a la imagen.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample97.cs)]

### `WebImage.FlipHorizontal()`  
`WebImage.FlipVertical()`

Voltea la imagen horizontal o verticalmente.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample98.css)]

### `WebImage.GetImageFromRequest()`

Carga una imagen cuando se envía una imagen a una página durante una carga de archivo.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample99.cs)]

### `WebImage.Resize(width,height)`

Cambia de tamaño un la imagen.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample100.css)]

### `WebImage.RotateLeft()`  
`WebImage.RotateRight()`

Gira la imagen a la izquierda o la derecha.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample101.css)]

### `WebImage.Save(path [, imageFormat])`

Guarda la imagen en la ruta de acceso especificada.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample102.js)]

### `WebMail.Password`

Establece la contraseña para el servidor SMTP. Normalmente, debe establecer esta propiedad el  *\_AppStart* página.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample103.cs)]

### `WebMail.Send(to, subject, body [, from] [, cc] [, filesToAttach] [, isBodyHtml]`  
 `[, additionalHeaders])`

Envía un mensaje de correo electrónico.

[!code-css[Main](asp-net-web-pages-api-reference/samples/sample104.css)]

### `WebMail.SmtpServer`

Establece el nombre del servidor SMTP. Normalmente, debe establecer esta propiedad el<em>\_AppStart</em> página.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample105.html)]

### `WebMail.UserName`

Establece el nombre de usuario para el servidor SMTP. Normalmente debería establecer esta propiedad el  *\_AppStart* página.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample106.html)]

<a id="Validation"></a>
## <a name="validation"></a>Validación

### `Html.ValidationMessage(field)`

(v2) Representa un mensaje de error de validación para el campo especificado.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample107.cshtml)]

### `Html.ValidationSummary([message])`

(v2) Muestra una lista de todos los errores de validación.

[!code-cshtml[Main](asp-net-web-pages-api-reference/samples/sample108.cshtml)]

### `Validation.Add(field, validationType)`

(v2) Registra un elemento de entrada de usuario para el tipo de validación especificado.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample109.js)]

### `Validation.ClassFor(field)`

(v2) Dinámicamente presenta los atributos de clase CSS para la validación del lado cliente para que se pueden dar formato a los mensajes de error de validación. (Requiere que hace referencia a las bibliotecas de scripts de cliente adecuado y que se definen las clases CSS).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample110.html)]

### `Validation.For(field)`

(v2) Habilita la validación del lado cliente para el campo de entrada de usuario. (Requiere que se hacen referencia a las bibliotecas de scripts de cliente adecuado).

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample111.html)]

### `Validation.IsValid()`

(v2) Devuelve true si todos los elementos de entrada de usuario que están registrado para la validación contienen valores válidos.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample112.cs)]

### `Validation.RequireField(field[, errorMessage])`

(v2) Especifica que los usuarios deben proporcionar un valor para el elemento de entrada de usuario.

[!code-csharp[Main](asp-net-web-pages-api-reference/samples/sample113.cs)]

### `Validation.RequireFields(field1[, field12, field3, ...])`

(v2) Especifica que los usuarios deben proporcionar valores para cada uno de los elementos de entrada de usuario. Este método no permite especificar un mensaje de error personalizado.

[!code-html[Main](asp-net-web-pages-api-reference/samples/sample114.html)]

### `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField,[error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min,max [, error message])`  
`Validator.RegEx(pattern[, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`

(v2) Especifica una prueba de validación cuando se usa el `Validation.Add` método.

[!code-javascript[Main](asp-net-web-pages-api-reference/samples/sample115.js)]
