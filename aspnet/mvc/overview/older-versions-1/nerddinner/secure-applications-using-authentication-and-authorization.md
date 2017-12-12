---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: "Proteger aplicaciones mediante la autenticación y autorización | Documentos de Microsoft"
author: microsoft
description: "Paso 9 muestra cómo agregar autenticación y autorización para proteger nuestra aplicación NerdDinner, para que los usuarios necesitan registrar e inicie sesión en el sitio para crear..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: a23b2cf4d1728624698c0db49c25ea7efd3af67d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="secure-applications-using-authentication-and-authorization"></a>Proteger aplicaciones mediante la autenticación y autorización
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 9 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 9 muestra cómo agregar autenticación y autorización para proteger nuestra aplicación NerdDinner, para que los usuarios necesitan registrar y de inicio de sesión en el sitio para crear nuevo cenas y solo el usuario que está hospedando una cena puede modificarlo más adelante.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner paso 9: Autenticación y autorización

Ahora nuestro NerdDinner aplicación concede a cualquier usuario visita el sitio la capacidad de crear y editar los detalles de cualquier cena. Vamos a cambiar esto para que los usuarios necesitan registrar e inicie sesión en el sitio para crear nuevo cenas y agregar una restricción para que solo el usuario que está hospedando una cena puede editar más adelante.

Para habilitar esta opción vamos a usar la autenticación y autorización para proteger la aplicación.

### <a name="understanding-authentication-and-authorization"></a>Descripción de la autenticación y autorización

*Autenticación* es el proceso de identificar y validar la identidad de un cliente de acceso a una aplicación. En términos más simples, se trata de identificar "quién el usuario final es cuando visita un sitio Web". ASP.NET admite varias maneras de autenticar a los usuarios del explorador. En las aplicaciones web de Internet, el enfoque más común de autenticación utilizado se denomina "Autenticación por formularios". Autenticación de formularios permite que un desarrollador crear un formulario de inicio de sesión HTML dentro de su aplicación y, a continuación, validar el nombre de usuario y la contraseña de que un usuario final se envía en una base de datos u otro almacén de credenciales de contraseña. Si la combinación de nombre de usuario/contraseña es correcta, el desarrollador, a continuación, puede pedir ASP.NET para emitir una cookie HTTP cifrada para identificar al usuario en solicitudes futuras. Te enviaremos un mensaje mediante la autenticación de formularios con nuestra aplicación NerdDinner.

*Autorización* es el proceso de determinar si un usuario autenticado tiene permiso para tener acceso a un recurso de dirección URL determinado/o realizar alguna acción. Por ejemplo, dentro de nuestra aplicación NerdDinner queremos autorizar que solo los usuarios que han iniciado sesión pueden tener acceso a la */cenas/crear* dirección URL y crear nuevos cenas. También se deseará agregar lógica de autorización para que solo el usuario que está hospedando una cena pueda editarlo – y denegar el acceso de edición a todos los demás usuarios.

### <a name="forms-authentication-and-the-accountcontroller"></a>Autenticación de formularios y la AccountController

La plantilla de proyecto de Visual Studio de forma predeterminada para ASP.NET MVC automáticamente habilita la autenticación de formularios cuando se crean nuevas aplicaciones de ASP.NET MVC. También agrega automáticamente una implementación de página de inicio de sesión de cuenta previamente generada para el proyecto, lo que hace más fácil de integrar la seguridad dentro de un sitio.

La página maestra Site.master de manera predeterminada muestra un vínculo "Iniciar sesión" en la parte superior derecha del sitio cuando no se autentica al usuario tener acceso a ella:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Haga clic en el vínculo "Iniciar sesión" transfiere a un usuario a la */Account/inicio de sesión* dirección URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Los visitantes que no se haya registrado pueden hacerlo haciendo clic en el vínculo "Register", que le llevará a la */Account/Register* dirección URL y permitir que se especifique los detalles de la cuenta:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Haga clic en el botón "Registrar" crear un nuevo usuario en el sistema de pertenencia a ASP.NET y autenticar al usuario en el sitio mediante la autenticación de formularios.

Cuando un usuario se ha iniciado la sesión, el Site.master cambia la parte superior derecha de la página para generar un "Bienvenido [username]" mensaje y representa un "cerrar" vincular en lugar de un "iniciar sesión" uno. Haga clic en el vínculo "Cerrar sesión" cierre la sesión del usuario:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

La funcionalidad de inicio de sesión, cierre de sesión y el registro anterior se implementa dentro de la clase AccountController que se ha agregado a nuestro proyecto de Visual Studio cuando crea el proyecto. La interfaz de usuario para el AccountController se implementa mediante plantillas de vista en el directorio \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La clase AccountController utiliza el sistema de autenticación de formularios de ASP.NET para emitir cookies de autenticación cifrada y la API de pertenencia de ASP.NET para almacenar y validar nombres de usuario y contraseñas. La API de pertenencia de ASP.NET es extensible y permite que cualquier almacén de credenciales de contraseña que se usará. ASP.NET se suministra con las implementaciones del proveedor de pertenencia integrado que almacenan el nombre de usuario o contraseñas en una base de datos SQL, o en Active Directory.

Podemos configurar qué proveedor de pertenencia a nuestra aplicación NerdDinner debe usar, abra el archivo "web.config" en la raíz del proyecto y buscando el &lt;pertenencia&gt; sección dentro de él. Registra el proveedor de pertenencia SQL predeterminado web.config agregado cuando se creó el proyecto y lo configura para utilizar una cadena de conexión denominada "ApplicationServices" para especificar la ubicación de la base de datos.

La cadena de conexión predeterminada "ApplicationServices" (que se especifica en el &lt;connectionStrings&gt; sección del archivo web.config) está configurado para usar SQL Express. Apunta a una base de datos de SQL Express denominada "ASPNETDB. MDF"en la aplicación" aplicación\_datos "directorio. Si esta base de datos no existe la primera vez que se utiliza la API de pertenencia dentro de la aplicación, ASP.NET creará automáticamente la base de datos y aprovisionar el esquema de base de datos de pertenencia adecuada dentro de él:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Si en lugar de usar SQL Express que deseamos utilizar una instancia completa de SQL Server (o conectarse a una base de datos remota), lo único que necesitaría tareas consiste en actualizar la cadena de conexión de "ApplicationServices" en el archivo web.config y asegúrese de que el esquema de pertenencia adecuada se ha agregado a la base de datos que apunta al. Puede ejecutar el "aspnet\_regsql.exe" utilidad dentro del directorio \Windows\Microsoft.NET\Framework\v2.0.50727\ para agregar el esquema apropiado para la suscripción y los servicios de aplicación de ASP.NET a una base de datos.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorización de la dirección URL de cenas/crear usando el filtro [Authorize]

No hemos tenido que escribir ningún código para habilitar una autenticación segura y la implementación de administración de cuenta para la aplicación NerdDinner. Los usuarios pueden registrar nuevas cuentas con nuestra aplicación y el inicio de sesión o cierre de sesión del sitio.

Ahora podemos agregar la lógica de autorización a la aplicación y utilizar el estado de autenticación y el nombre de usuario de los visitantes para controlar lo que puede y no se puede hacer en el sitio. Comencemos por agregar lógica de autorización a los métodos de acción "Crear" de nuestra clase DinnersController. En concreto, será necesario que los usuarios obtener acceso a la */cenas/crear* dirección URL debe haber iniciado sesión. Si no se han iniciado se deberá redirigir a la página de inicio de sesión para que puede iniciar sesión.

Esta lógica de la implementación es bastante fácil. Todo lo que necesitamos tarea consiste en Agregar un atributo de filtro [Authorize] a nuestros métodos de acción de creación de este modo:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC admite la capacidad de crear "filtros de acción" que pueden usarse para implementar la lógica reutilizable que puede aplicarse mediante declaración a métodos de acción. El filtro [Authorize] es uno de los filtros de acción integrados proporcionados por ASP.NET MVC, y permite al programador mediante declaración aplicar reglas de autorización a métodos de acción y las clases de controlador.

Cuando se aplica sin parámetros (como anteriormente) el filtro [Authorize] exige que el usuario que realiza la solicitud del método de acción debe haber iniciado sesión, y se le redirigirá automáticamente el explorador a la dirección URL de inicio de sesión si no lo son. ¿Al realizar esta redirección de la dirección URL solicitada originalmente se pasa como un argumento de cadena de consulta (por ejemplo: / Account/inicio de sesión? ReturnUrl = % 2fDinners % 2fCreate). El AccountController, a continuación, redirija al usuario a la dirección URL solicitada originalmente una vez que inicie sesión.

El filtro [Authorize] opcionalmente es compatible con la capacidad para especificar una propiedad "Users" o "Roles" que puede utilizarse para exigir que el usuario se ambos registra en y dentro de una lista de usuarios permitidos o un miembro de un rol de seguridad permitidas. Por ejemplo, el código siguiente sólo permite dos usuarios específicos, "scottgu" y "billg" tener acceso a la dirección URL de cenas/crear:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Incrustar nombres de usuario específico dentro del código tiende a ser bastante sin que se pueda mantener aunque. Un enfoque más adecuado consiste en definir un nivel más alto "roles" que comprueba el código y, a continuación, para asignar usuarios a la función mediante una base de datos o el sistema de active directory (habilitar la lista de asignación de usuarios reales se almacenan externamente el código). ASP.NET incluye una API de administración de roles integradas, así como un conjunto integrado de proveedores de funciones (incluidas las de SQL y Active Directory) que pueden ayudar a realizar esta asignación de usuarios y roles. A continuación, podríamos actualizar el código para permitir que solo los usuarios dentro de una función de "administrador" específico tener acceso a la dirección URL de cenas/crear:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Uso de la propiedad User.Identity.Name al crear cenas

Podemos recuperar el nombre de usuario del usuario ha iniciado la sesión actual de una solicitud mediante la propiedad User.Identity.Name expuesta en la clase base del controlador.

Anteriores cuando se implementa la versión de HTTP-POST de nuestro método de acción Create() tuvimos codificado de forma rígida la propiedad "HostedBy" de la cena a una cadena estática. Le podemos actualizar ahora este código para que utilice en su lugar la propiedad User.Identity.Name, así como agregar automáticamente una RSVP para la creación de la cena de host:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Debido a que hemos agregado un atributo [Authorize] para el método Create(), ASP.NET MVC garantiza que el método de acción solo se ejecuta si el usuario visita la dirección URL de cenas/crear ha iniciado sesión en el sitio. Por lo tanto, el valor de propiedad User.Identity.Name siempre contendrá un nombre de usuario válido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Mediante la propiedad User.Identity.Name al editar cenas

Ahora agreguemos alguna lógica de autorización que se limita a los usuarios para que solo pueden modificar las propiedades de cenas que ellos mismos hospedan.

Para ello, primero vamos a agregar un método de aplicación auxiliar de "IsHostedBy(username)" a nuestro objeto cena (dentro de la clase parcial Dinner.cs que se generó anteriormente). Este método auxiliar devuelve Verdadero o falso dependiendo de si un nombre de usuario proporcionado coincide con la propiedad de la cena HostedBy y encapsula la lógica necesaria realizar una comparación de cadenas entre mayúsculas y minúsculas de ellos:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

A continuación, vamos a agregar un atributo [Authorize] a los métodos de acción Edit() dentro de nuestra clase DinnersController. Esto garantizará que los usuarios deben haber iniciado sesión solicitud un */Dinners/editar / [id]* dirección URL.

A continuación, podemos agregar código a nuestros métodos de edición que usa el método de aplicación auxiliar Dinner.IsHostedBy(username) para comprobar que el usuario ha iniciado sesión coincide con el host de la cena. Si el usuario no es el host, se le muestra una vista "InvalidOwner" y finalizar la solicitud. El código para hacer esto es similar a continuación:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Podemos, a continuación, haga doble clic en el directorio \Views\Dinners y elija Agregar -&gt;ver el comando de menú para crear una nueva vista "InvalidOwner". Podrá rellenarla con el mensaje de error siguiente:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Y ahora cuando un usuario intenta modificar una cena no poseen, obtendrá un mensaje de error:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Le podemos repetir los mismos pasos para los métodos de acción Delete() dentro de nuestro controlador para bloquear el permiso para eliminar también cenas y asegúrese de que sólo el host de una cena puede eliminarlo.

### <a name="showinghiding-edit-and-delete-links"></a>Mostrar/ocultar editar y eliminar vínculos

El método de acción de edición y eliminación de nuestra clase DinnersController se estamos vincular desde nuestra dirección de URL de detalles:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Actualmente, mostramos los vínculos de acción de editar y eliminar independientemente de si el visitante a la dirección URL de detalles es el host de la cena. Vamos a cambiar esto para que los vínculos se muestran solo si el usuario visitante es el propietario de la cena.

El método de acción Details() dentro de nuestro DinnersController recupera un objeto de la cena y, a continuación, pasa como el objeto de modelo a la plantilla de vista:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Podemos actualizar nuestro ver plantilla condicionalmente mostrar u ocultar los vínculos de editar y eliminar mediante el uso de la Dinner.IsHostedBy() método auxiliar como las siguientes:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Pasos siguientes

Ahora veamos cómo podemos habilitar a usuarios autenticados RSVP para cenas mediante AJAX.

>[!div class="step-by-step"]
[Anterior](implement-efficient-data-paging.md)
[Siguiente](use-ajax-to-deliver-dynamic-updates.md)
