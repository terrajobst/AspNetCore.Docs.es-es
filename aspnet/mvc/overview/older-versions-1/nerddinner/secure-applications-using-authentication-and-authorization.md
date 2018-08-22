---
uid: mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
title: Proteger aplicaciones mediante la autenticación y autorización | Microsoft Docs
author: microsoft
description: Paso 9 muestra cómo agregar autenticación y autorización para proteger nuestra aplicación NerdDinner, para que los usuarios necesitan registrar e inicie sesión en el sitio para crear...
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 9e4d5cac-b071-440c-b044-20b6d0c964fb
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/secure-applications-using-authentication-and-authorization
msc.type: authoredcontent
ms.openlocfilehash: d5f1b26312f11fd6d4ab500c7f24a4d89d428e38
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837914"
---
<a name="secure-applications-using-authentication-and-authorization"></a>Proteger aplicaciones mediante la autenticación y autorización
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 9 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 9 muestra cómo agregar autenticación y autorización para proteger nuestra aplicación NerdDinner, para que los usuarios necesitan registrar y el inicio de sesión en el sitio para crear instancias dinners nuevo y solo el usuario que está hospedando una cena puede editar más adelante.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-9-authentication-and-authorization"></a>NerdDinner paso 9: Autenticación y autorización

Ahora nuestra aplicación concede a cualquier persona de NerdDinner visitar el sitio la capacidad de crear y editar los detalles de cualquier cena. Vamos a cambiar esto para que los usuarios necesitan registrar e inicie sesión en el sitio para crear instancias dinners nuevo y agregar una restricción para que solo el usuario que está hospedando una cena puede editar más adelante.

Para habilitar esta opción vamos a usar la autenticación y autorización para proteger nuestra aplicación.

### <a name="understanding-authentication-and-authorization"></a>Descripción de la autenticación y autorización

*Autenticación* es el proceso de identificar y validar la identidad de un cliente de acceso a una aplicación. En términos más simples, consiste en identificar "quién es de los usuarios finales cuando visitan un sitio Web". ASP.NET admite varias maneras de autenticar a los usuarios del explorador. Para las aplicaciones web de Internet, el enfoque más común de autenticación utilizado se denomina "Autenticación de formularios". Autenticación de formularios permite a un programador crear un formulario de inicio de sesión HTML dentro de su aplicación y, a continuación, validar el nombre de usuario y la contraseña de que un usuario final se envía en una base de datos u otro almacén de credenciales de contraseña. Si la combinación de nombre de usuario/contraseña es correcta, el desarrollador, a continuación, puede solicitar a ASP.NET para emitir una cookie HTTP cifrada para identificar al usuario a través de las solicitudes futuras. Vamos a mediante el uso de autenticación de formularios con nuestra aplicación NerdDinner.

*Autorización* es el proceso de determinar si un usuario autenticado tiene permiso para tener acceso a una dirección URL o un recursos concreto o para realizar alguna acción. Por ejemplo, dentro de nuestra aplicación NerdDinner desearemos autorizar que solo los usuarios que han iniciado sesión pueden tener acceso a la */Dinners/crear* dirección URL y crear instancias Dinners nuevo. También nos conviene agregar lógica de autorización para que pueda editarlo – y denegar el acceso de edición a todos los demás usuarios solo el usuario que está hospedando una cena.

### <a name="forms-authentication-and-the-accountcontroller"></a>Autenticación de formularios y AccountController

La plantilla de proyecto de Visual Studio de forma predeterminada para ASP.NET MVC automáticamente habilita la autenticación de formularios cuando se crean nuevas aplicaciones de ASP.NET MVC. También agrega automáticamente una implementación de página de inicio de sesión de cuenta pregenerados para el proyecto, lo que resulta muy fácil integrar la seguridad dentro de un sitio.

La página maestra Site.master de forma predeterminada muestra un vínculo "Iniciar sesión" en la parte superior derecha del sitio cuando no se autentica al usuario tener acceso a él:

![](secure-applications-using-authentication-and-authorization/_static/image1.png)

Al hacer clic en el vínculo "Iniciar sesión" tarda un usuario en el */cuenta/inicio de sesión* dirección URL:

![](secure-applications-using-authentication-and-authorization/_static/image2.png)

Los visitantes que aún no ha registrado pueden hacerlo haciendo clic en el vínculo "Register", que le llevarán a la */cuenta/Register* dirección URL y permitirles que escriba los detalles de la cuenta:

![](secure-applications-using-authentication-and-authorization/_static/image3.png)

Al hacer clic en el botón "Registrar" crear un nuevo usuario en el sistema de pertenencia a ASP.NET y autenticar al usuario en el sitio mediante la autenticación de formularios.

Cuando un usuario se ha iniciado sesión, el Site.master cambia la parte superior derecha de la página para generar un "Bienvenido [username]". mensaje y se representa un "cerrar sesión" vincular en lugar de un "iniciar sesión" uno. Al hacer clic en el vínculo "Cerrar sesión" cierra la sesión del usuario:

![](secure-applications-using-authentication-and-authorization/_static/image4.png)

La funcionalidad de inicio de sesión, cierre de sesión y registro anterior se implementa dentro de la clase AccountController que se ha agregado a nuestro proyecto de Visual Studio cuando crea el proyecto. Se implementa la interfaz de usuario para AccountController usar plantillas de vista en el directorio \Views\Account:

![](secure-applications-using-authentication-and-authorization/_static/image5.png)

La clase AccountController utiliza el sistema de autenticación de formularios de ASP.NET para emitir cookies de autenticación cifrada y la API de pertenencia de ASP.NET para almacenar y validar los nombres de usuario y contraseña. La API de pertenencia de ASP.NET es extensible y permite a cualquier almacén de credenciales de contraseña que se usará. ASP.NET incluye las implementaciones del proveedor de pertenencia integrado que almacenan el nombre de usuario/contraseña dentro de una base de datos SQL o dentro de Active Directory.

Podemos configurar qué proveedor de pertenencia que se debe usar nuestra aplicación NerdDinner, abra el archivo "web.config" en la raíz del proyecto y buscando el &lt;pertenencia&gt; sección dentro de él. Registra el proveedor de pertenencia SQL predeterminado web.config agregado cuando se creó el proyecto y lo configura para utilizar una cadena de conexión denominada "ApplicationServices" para especificar la ubicación de la base de datos.

La cadena de conexión predeterminada "ApplicationServices" (que se especifican dentro la &lt;connectionStrings&gt; sección del archivo web.config) está configurado para usar SQL Express. Apunta a una base de datos de SQL Express denominado "ASPNETDB. MDF"en la aplicación" aplicación\_datos "directorio. Si esta base de datos no existe en la primera vez que se usa la API de pertenencia dentro de la aplicación, ASP.NET creará automáticamente la base de datos y aprovisionar el esquema de base de datos de pertenencia apropiada dentro de él:

![](secure-applications-using-authentication-and-authorization/_static/image6.png)

Si en lugar de usar SQL Express quisiera usar una instancia completa de SQL Server (o conectarse a una base de datos remota), todos los necesitaríamos tarea consiste en actualizar la cadena de conexión "ApplicationServices" dentro del archivo web.config y asegúrese de que el esquema de pertenencia adecuados se agregó a la apunta a la base de datos. Puede ejecutar el "aspnet\_regsql.exe" utilidad dentro del directorio \Windows\Microsoft.NET\Framework\v2.0.50727\ para agregar el esquema apropiado para la suscripción y los servicios de aplicaciones ASP.NET a una base de datos.

### <a name="authorizing-the-dinnerscreate-url-using-the-authorize-filter"></a>Autorización de la dirección URL de instancias Dinners/Create con el filtro [Authorize]

No hemos tenido que escribir ningún código para habilitar una autenticación segura y la implementación de administración de cuenta para la aplicación NerdDinner. Los usuarios pueden registrar las cuentas nuevas con nuestra aplicación y el inicio de sesión o cierre de sesión del sitio.

Ahora podemos agregar lógica de autorización a la aplicación y usar el estado de la autenticación y el nombre de usuario de visitantes para controlar lo que puede y no pueden hacer desde el sitio. Comencemos por agregar lógica de autorización a los métodos de acción "Crear" de nuestra clase DinnersController. En concreto, será necesario que los usuarios accedan a la */Dinners/crear* dirección URL debe haber iniciado sesión. Si no ha iniciado sesión se le redirija a la página de inicio de sesión para que puede iniciar sesión.

Implementar esta lógica es bastante fácil. Todo lo que necesitamos tarea consiste en Agregar un atributo de filtro [Authorize] a nuestros métodos de acción Create de este modo:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample1.cs)]

ASP.NET MVC admite la capacidad para crear "filtros de acción" que se pueden usar para implementar la lógica reutilizable que puede aplicarse mediante declaración a métodos de acción. El filtro [Authorize] es uno de los filtros de acción integrados proporcionados por ASP.NET MVC, y permite al programador mediante declaración aplicar reglas de autorización a los métodos de acción y las clases de controlador.

Cuando se aplica sin parámetros (como anteriormente) se aplica el filtro [Authorize] que el usuario que realiza la solicitud del método de acción debe haber iniciado sesión, y le redireccionará automáticamente el explorador a la dirección URL de inicio de sesión si no lo son. ¿Al realizar esta redirección de la dirección URL originalmente solicitada se pasa como un argumento de cadena de consulta (por ejemplo: / cuenta/inicio de sesión? ReturnUrl = % 2fDinners % 2fCreate). AccountController, a continuación, redirigir al usuario a la dirección URL solicitada originalmente una vez que inicien sesión.

El filtro [Authorize] admite opcionalmente la capacidad para especificar una propiedad "Users" o "Roles" que puede utilizarse para requerir que el usuario es ambos iniciado en y dentro de una lista de usuarios permitidos o un miembro de un rol de seguridad permitidas. Por ejemplo, el código siguiente solo permite que dos usuarios específicos, "scottgu" y "billg" tener acceso a la dirección URL de instancias Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample2.cs)]

Incrustar nombres de usuario específicos dentro de código tiende a ser bastante fácil de mantener sin embargo. Un enfoque mejor es definir "roles" de nivel superior que comprueba el código y, a continuación, para asignar usuarios a la función mediante una base de datos o el sistema de active directory (para habilitar la lista de asignación de usuario real para almacenarse externamente desde el código). ASP.NET incluye una API de administración de roles integradas, así como un conjunto integrado de proveedores de funciones (incluidas las de SQL y Active Directory) que pueden ayudarle a realizar esta asignación de usuarios y roles. A continuación, podríamos actualizamos el código para permitir que solo los usuarios dentro de un rol específico "admin" tener acceso a la dirección URL de instancias Dinners/Create:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample3.cs)]

### <a name="using-the-useridentityname-property-when-creating-dinners"></a>Mediante la propiedad User.Identity.Name al crear instancias Dinners

Podemos recuperar el nombre de usuario del usuario ha iniciado sesión actualmente de una solicitud mediante la propiedad de User.Identity.Name expuesta en la clase base del controlador.

Anteriores cuando se implementa la versión de HTTP POST de nuestro método de acción Create() teníamos codificado de forma rígida la propiedad "HostedBy" de la cena a una cadena estática. Nos podemos actualizar ahora este código para usar en su lugar la propiedad User.Identity.Name, así como agregar automáticamente una confirmación de asistencia para el host de creación de la cena:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample4.cs)]

Dado que hemos agregado un atributo [Authorize] al método Create(), ASP.NET MVC garantiza que el método de acción solo se ejecuta si el usuario visita la dirección URL de instancias Dinners/Create ha iniciado sesión en el sitio. Por lo tanto, el valor de propiedad User.Identity.Name siempre contendrá un nombre de usuario válido.

### <a name="using-the-useridentityname-property-when-editing-dinners"></a>Mediante la propiedad User.Identity.Name al editar las cenas

Ahora agreguemos alguna lógica de autorización que se restringe a los usuarios para que solo pueden editar las propiedades de instancias dinners que hospedan ellos mismos.

Para ello, primero vamos a agregar un método de aplicación auxiliar "IsHostedBy(username)" a nuestro objeto cena (dentro de la clase parcial Dinner.cs que hemos creado anteriormente). Este método auxiliar devuelve true o false dependiendo de si un nombre de usuario proporcionado coincide con la propiedad HostedBy Dinner y encapsula la lógica necesaria para realizar una comparación de cadenas entre mayúsculas y minúsculas de ellos:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample5.cs)]

A continuación, vamos a agregar un atributo [Authorize] a los métodos de acción Edit() dentro de nuestra clase DinnersController. Así se asegurará de que los usuarios deben haber iniciado sesión solicitud un */dinners/Edit / [id]* dirección URL.

A continuación, podemos agregar código a nuestros métodos de edición que usa el método auxiliar Dinner.IsHostedBy(username) para comprobar que el usuario ha iniciado sesión coincide con el host de la cena. Si el usuario no es el host, se le muestra una vista "InvalidOwner" y finalizar la solicitud. El código para hacer esto como el siguiente:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample6.cs)]

Podemos, a continuación, haga doble clic en el directorio \Views\Dinners y elegir Add -&gt;ver el comando de menú para crear una nueva vista "InvalidOwner". Deberá rellenarla con el siguiente mensaje de error:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample7.aspx)]

Y ahora cuando un usuario intenta modificar una cena no poseen, obtendrá un mensaje de error:

![](secure-applications-using-authentication-and-authorization/_static/image7.png)

Nos podemos repetir los mismos pasos para los métodos de acción Delete() dentro de nuestro controlador para bloquear el permiso para eliminar también las cenas y asegúrese de que sólo el host de una cena puede eliminarlo.

### <a name="showinghiding-edit-and-delete-links"></a>Mostrar/ocultar editar y eliminar vínculos

Nos estamos vincula al método de acción Edit y Delete de nuestra clase DinnersController desde nuestra dirección URL de detalles:

![](secure-applications-using-authentication-and-authorization/_static/image8.png)

Actualmente, se muestra los vínculos de acción de editar y eliminar independientemente de si el visitante para la dirección URL de detalles es el host de la cena. Vamos a cambiar esto para que los vínculos se muestran solo si el usuario visitante es el propietario de la cena.

El método de acción Details() dentro de nuestro DinnersController recupera un objeto de la cena y, a continuación, pasa como el objeto de modelo a nuestra plantilla de vista:

[!code-csharp[Main](secure-applications-using-authentication-and-authorization/samples/sample8.cs)]

Podemos actualizar la plantilla de vista para condicionalmente mostrar u ocultar los vínculos de edición y eliminación mediante el uso de la Dinner.IsHostedBy() como el método auxiliar siguiente:

[!code-aspx[Main](secure-applications-using-authentication-and-authorization/samples/sample9.aspx)]

#### <a name="next-steps"></a>Pasos siguientes

Ahora veamos cómo podemos habilitar a usuarios autenticados RSVP para instancias dinners mediante AJAX.

> [!div class="step-by-step"]
> [Anterior](implement-efficient-data-paging.md)
> [Siguiente](use-ajax-to-deliver-dynamic-updates.md)
