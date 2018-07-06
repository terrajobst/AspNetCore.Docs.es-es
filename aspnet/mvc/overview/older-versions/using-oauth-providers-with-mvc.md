---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: Usar proveedores OAuth con MVC 4 | Microsoft Docs
author: tfitzmac
description: Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 4 que permite a los usuarios inicien sesión con credenciales de un proveedor externo, como Facebo...
ms.author: aspnetcontent
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: 15f6b45706c0711d68b0780a7474d4c939a85fba
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823331"
---
<a name="using-oauth-providers-with-mvc-4"></a>Usar proveedores OAuth con MVC 4
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este tutorial muestra cómo crear una aplicación web de ASP.NET MVC 4 que permite a los usuarios iniciar sesión con credenciales de un proveedor externo, como Facebook, Twitter, Microsoft o Google y luego la integrará algunas de las funcionalidades de dichos proveedores en su aplicación Web. Por motivos de simplicidad, este tutorial se centra en trabajar con las credenciales de Facebook.
> 
> Para usar credenciales externas en una aplicación web ASP.NET MVC 5, consulte [crear una aplicación de ASP.NET MVC 5 con Facebook y Google OAuth2 y OpenID Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).
> 
> Habilitar estas credenciales en los sitios web proporciona una ventaja considerable porque millones de usuarios ya tienen cuentas con estos proveedores externos. Estos usuarios pueden ser más propensos a suscribirse a su sitio si no tienen que crear y recordar un nuevo conjunto de credenciales. Además, después de que un usuario haya iniciado sesión a través de uno de estos proveedores, puede incorporar operaciones sociales desde el proveedor.


## <a name="what-youll-build"></a>¿Qué va a crear

En este tutorial, hay dos objetivos principales:

1. Permitir que un usuario que inicie sesión con credenciales de un proveedor de OAuth.
2. Recuperar información de la cuenta del proveedor e integrar esa información en el registro de la cuenta para su sitio.

Aunque se centran en los ejemplos de este tutorial sobre el uso de Facebook como proveedor de autenticación, puede modificar el código para usar cualquiera de los proveedores. Los pasos para implementar cualquier proveedor son muy similares a los pasos que verá en este tutorial. Solo se puede detectar diferencias significativas al realizar llamadas directas a la API del proveedor establezca.

## <a name="prerequisites"></a>Requisitos previos

- [Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) o [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)

O bien

- Microsoft Visual Studio 2010 SP1 o [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)
- [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392)

Además, en este tema se supone que tiene conocimientos básicos sobre ASP.NET MVC y Visual Studio. Si necesita una introducción a ASP.NET MVC 4, consulte [Introducción a ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

## <a name="create-the-project"></a>Crear el proyecto

En Visual Studio, cree una nueva aplicación Web de ASP.NET MVC 4 y asígnele el nombre &quot;OAuthMVC&quot;. Puede tener como destino .NET Framework 4.5 o 4.

![Crear proyecto](using-oauth-providers-with-mvc/_static/image1.png)

En la ventana nuevo proyecto de ASP.NET MVC 4, seleccione **aplicación de Internet** y dejar **Razor** como el motor de vista.

![Seleccione la aplicación de Internet](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a>Habilitación de un proveedor

Cuando se crea una aplicación web MVC 4 con la plantilla de aplicación de Internet, se crea el proyecto con un archivo denominado AuthConfig.cs en la aplicación\_carpeta de inicio.

![Archivo AuthConfig](using-oauth-providers-with-mvc/_static/image3.png)

El archivo AuthConfig contiene código para registrar a los clientes para proveedores de autenticación externos. De forma predeterminada, este código está comentado, por lo que ninguno de los proveedores externos están habilitado.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

Debe quite este código para usar al cliente de autenticación externo. Quite la marca de comentario solo los proveedores que desea incluir en el sitio. Para este tutorial, solo permitirá las credenciales de Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

Tenga en cuenta en el ejemplo anterior que el método incluye cadenas vacías para los parámetros de registro. Si intenta ejecutar la aplicación ahora, la aplicación inicia una excepción de argumento porque no se permiten cadenas vacías para los parámetros. Para proporcionar los valores válidos, debe registrar su sitio web con los proveedores externos, como se muestra en la sección siguiente.

## <a name="registering-with-an-external-provider"></a>Registrar con un proveedor externo

Para autenticar usuarios con credenciales de un proveedor externo, debe registrar su sitio web con el proveedor. Al registrar su sitio, recibirá los parámetros (por ejemplo, clave o identificador y secreto) para incluir al registrar al cliente. Debe tener una cuenta con los proveedores que desea usar.

En este tutorial no muestra todos los pasos que debe realizar para registrar con estos proveedores. Los pasos no suelen ser difíciles. Para registrar correctamente el sitio, siga las instrucciones proporcionadas en esos sitios. Para empezar a trabajar con el registro de su sitio, consulte el sitio para desarrolladores para:

- [Facebook](https://developers.facebook.com/)
- [Google](https://developers.google.com/)
- [Microsoft](http://manage.dev.live.com/)
- [Twitter](https://dev.twitter.com/)

Al registrar su sitio con Facebook, puede proporcionar &quot;localhost&quot; para el dominio del sitio y `&quot;http://localhost/&quot;` para la dirección URL, tal como se muestra en la imagen siguiente. El uso de localhost funciona con la mayoría de los proveedores, pero actualmente no funciona con el proveedor de Microsoft. Para el proveedor de Microsoft, debe incluir una dirección URL del sitio web válido.

![registrar el sitio](using-oauth-providers-with-mvc/_static/image4.png)

En la imagen anterior, se han quitado los valores para el Id. de aplicación, el secreto de la aplicación y el correo electrónico de contacto. Al registrar su sitio realmente, esos valores estará presentes. Debe tener en cuenta los valores de Id. de aplicación y secreto de la aplicación, ya que lo agregará a la aplicación.

## <a name="creating-test-users"></a>Creación de usuarios de prueba

Si no cuenta con una cuenta existente de Facebook para probar su sitio, puede omitir esta sección.

Puede crear fácilmente usuarios de prueba para la aplicación dentro de la página de administración de aplicaciones de Facebook. Puede usar estas cuentas para iniciar sesión en el sitio de prueba. Crear usuarios de prueba, haga clic en el **Roles** vínculo en el panel de navegación izquierdo y hacer clic en el **crear** vínculo.

![crear usuarios de prueba](using-oauth-providers-with-mvc/_static/image5.png)

El sitio de Facebook crea automáticamente el número de cuentas de prueba que solicitan.

## <a name="adding-application-id-and-secret-from-the-provider"></a>Adición de Id. de aplicación y el secreto desde el proveedor

Ahora que ha recibido el identificador y el secreto de Facebook, vuelva al archivo AuthConfig y agregarlas como los valores de parámetro. Los valores que se muestra a continuación no son valores reales.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a>Inicie sesión con credenciales externas

Eso es todo lo que debe hacer para habilitar las credenciales externas en el sitio. Ejecute la aplicación y haga clic en el vínculo de inicio de sesión en la esquina superior derecha. La plantilla automáticamente reconoce que ha registrado Facebook como proveedor e incluye un botón para el proveedor. Si registra varios proveedores, un botón para cada uno de ellos se incluye automáticamente.

![inicio de sesión externo](using-oauth-providers-with-mvc/_static/image6.png)

Este tutorial no trata cómo personalizar el registro en los botones para los proveedores externos. Para obtener esa información, consulte [personalizar la interfaz de usuario de inicio de sesión al utilizar OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).

Haga clic en el botón de Facebook para iniciar sesión con las credenciales de Facebook. Cuando selecciona uno de los proveedores externos, se le redirigirá a ese sitio y se lo solicite que el servicio para iniciar sesión.

La siguiente imagen muestra la pantalla de inicio de sesión de Facebook. Observa que está utilizando su cuenta de Facebook para iniciar sesión en un sitio denominado oauthmvcexample.

![autenticación de Facebook](using-oauth-providers-with-mvc/_static/image7.png)

Después de iniciar sesión con las credenciales de Facebook, una página informa al usuario que el sitio tendrá acceso a información básica.

![permiso de solicitud](using-oauth-providers-with-mvc/_static/image8.png)

Después de seleccionar **ir a la aplicación**, el usuario debe registrar para el sitio. La siguiente imagen muestra la página de registro después de que un usuario ha iniciado sesión con las credenciales de Facebook. El nombre de usuario es normalmente se rellenan previamente con un nombre del proveedor.

![register](using-oauth-providers-with-mvc/_static/image9.png)

Haga clic en **registrar** para completar el registro. Cierre el explorador.

Puede ver que la nueva cuenta se ha agregado a la base de datos. En el Explorador de servidores, abra el **DefaultConnection** de base de datos y abra la **tablas** carpeta.

![tablas de bases de datos](using-oauth-providers-with-mvc/_static/image10.png)

Haga clic en el **UserProfile** de tabla y seleccione **mostrar datos de tabla**.

![Mostrar datos](using-oauth-providers-with-mvc/_static/image11.png)

Verá la nueva cuenta agregada. Examine los datos de **página Web\_OAuthMembership** tabla. Verá más datos relacionados con el proveedor externo para la cuenta que acaba de agregar.

Si solo desea habilitar la autenticación externa, está listo. Sin embargo, puede integrar más información del proveedor en el nuevo proceso de registro de usuario, tal como se muestra en las secciones siguientes.

## <a name="create-models-for-additional-user-information"></a>Crear modelos para obtener información adicional del usuario

Como ha observado en las secciones anteriores, no es necesario recuperar cualquier información adicional para que funcione el registro de la cuenta integrada. Sin embargo, los proveedores externos más pasan otros información sobre el usuario. Las secciones siguientes muestran cómo conservar esta información y guárdelo en una base de datos. En concreto, conservará los valores para el nombre completo del usuario, el URI de la página de web personal del usuario y un valor que indica si Facebook ha comprobado la cuenta.

Va a usar [migraciones de Code First](https://msdn.microsoft.com/data/jj591621) para agregar una tabla para almacenar información adicional del usuario. Va a agregar la tabla a una base de datos existente, por lo que primero deberá crear una instantánea de la base de datos actual. Al crear una instantánea de la base de datos actual, más adelante puede crear una migración que contiene solo la nueva tabla. Para crear una instantánea de la base de datos actual:

1. Abra el **consola de administrador de paquetes**
2. Ejecute el comando **enable-migrations**
3. Ejecute el comando **agregar a la migración inicial: IgnoreChanges**
4. Ejecute el comando **Actualizar base de datos**

Ahora, agregará las nuevas propiedades. En la carpeta Models, abra el archivo AccountModels.cs y busque la clase RegisterExternalLoginModel. La clase RegisterExternalLoginModel contiene valores que se devuelven desde el proveedor de autenticación. Agregar propiedades llamadas FullName y vínculo, tal como se muestra a continuación.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

También en AccountModels.cs, agregue una nueva clase denominada ExtraUserInformation. Esta clase representa la nueva tabla que se creará en la base de datos.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

En la clase UsersContext, agregue el código resaltado a continuación para crear una propiedad DbSet para la nueva clase.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

Ahora está listo para crear la nueva tabla. Abra la consola de administrador de paquetes de nuevo y esta vez:

1. Ejecute el comando **AddExtraUserInformation agregar a la migración**
2. Ejecute el comando **Actualizar base de datos**

La nueva tabla ya existe en la base de datos.

## <a name="retrieve-the-additional-data"></a>Recuperar los datos adicionales

Hay dos maneras de recuperar datos de usuario adicionales. La primera consiste en conservar los datos de usuario que se pasan, de forma predeterminada, durante la solicitud de autenticación. La segunda consiste en concreto, llame a la API del proveedor y solicitar más información. Automáticamente se pasan valores FullName y el vínculo volver por Facebook. Un valor que indica si Facebook ha comprobado la cuenta se recupera mediante una llamada a la API de Facebook. En primer lugar, se rellenará los valores FullName y el vínculo y, a continuación, más adelante, obtendrá el valor comprobado.

Para recuperar los datos de usuario adicionales, abra el **AccountController.cs** de archivos en el **controladores** carpeta.

Este archivo contiene la lógica para el registro, registrar y administrar cuentas. En concreto, tenga en cuenta los métodos llamados **ExternalLoginCallback** y **ExternalLoginConfirmation**. Dentro de estos métodos, puede agregar código para personalizar las operaciones de inicio de sesión externo para la aplicación. La primera línea de la **ExternalLoginCallback** contiene el método:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

Datos de usuario adicionales que se pasan en el **ExtraData** propiedad de la **AuthenticationResult** objeto que se devuelve desde el **VerifyAuthentication** método. El cliente de Facebook contiene los siguientes valores en el **ExtraData** propiedad:

- id
- name
- vínculo
- sexo
- accesstoken

Otros proveedores tendrá datos similar pero ligeramente distintos en la propiedad ExtraData.

Si el usuario es nuevo en su sitio, recuperar algunos de los datos adicionales y pasar datos a la vista de confirmación. El último bloque de código en el método se ejecuta sólo si el usuario es nuevo en su sitio. Reemplace la línea siguiente:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

Con esta línea:

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

Este cambio simplemente incluye los valores de las propiedades FullName y vínculo.

En el **ExternalLoginConfirmation** método, modifique el código tal como se muestra a continuación para guardar la información adicional del usuario.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a>Ajuste de la vista

Los datos de usuario adicionales que recuperan desde el proveedor se mostrará en la página de registro.

En el **vistas**/**cuenta** carpeta abierta **ExternalLoginConfirmation.cshtml**. Debajo del campo existente para el nombre de usuario, agregar campos para FullName, vínculo y PictureLink.

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

Ya está casi listo para ejecutar la aplicación y registrar un nuevo usuario con la información adicional que se guardó. Debe tener una cuenta que ya no está registrada con el sitio. Puede usar una cuenta de prueba diferente, o eliminar las filas de la **UserProfile** y **páginas Web\_OAuthMembership** tablas para la cuenta que desea volver a usar. Eliminando las filas, se asegurará de que la cuenta se vuelve a registrar.

Ejecute la aplicación y registrar el nuevo usuario. Tenga en cuenta que esta vez la página de confirmación contiene más valores.

![register](using-oauth-providers-with-mvc/_static/image12.png)

Después de completar el registro, cierre el explorador. Buscar en la base de datos tenga en cuenta los nuevos valores en el **ExtraUserInformation** tabla.

## <a name="install-nuget-package-for-facebook-api"></a>Instale el paquete NuGet para la API de Facebook

Facebook proporciona un [API](https://developers.facebook.com/docs/reference/apis/) que puede llamar para realizar operaciones. Puede llamar a la API de Facebook dirigiendo el envío de solicitudes HTTP, o mediante el uso de instalación de un paquete de NuGet que facilita el envío de dichas solicitudes. Mediante un paquete de NuGet se muestra en este tutorial, pero la instalación de NuGet paquete no es esencial. Este tutorial muestra cómo usar el paquete del SDK de Facebook C#. Hay otros paquetes de NuGet que ayudan con una llamada a la API de Facebook.

Desde el **administrar paquetes de NuGet** windows, seleccione el paquete de C# SDK de Facebook.

![Instalar paquete](using-oauth-providers-with-mvc/_static/image13.png)

Usará el SDK de Facebook C# para llamar a una operación que requiere el token de acceso para el usuario. La sección siguiente muestra cómo obtener el token de acceso.

## <a name="retrieve-access-token"></a>Recuperar el token de acceso

Proveedores externos más transmitir un token de acceso una vez comprobadas las credenciales del usuario. Este token de acceso es muy importante porque permite llamar a las operaciones que solo están disponibles para los usuarios autenticados. Por lo tanto, recuperar y almacenar el token de acceso es esencial cuando desea proporcionar más funcionalidad.

Dependiendo del proveedor externo, el token de acceso puede ser válido para solamente una cantidad limitada de tiempo. Para asegurarse de que tiene un token de acceso válida, se lo recuperará cada vez que el usuario inicia sesión y almacenarla como un valor de sesión en lugar de guardarlo en una base de datos.

En el **ExternalLoginCallback** método, también se pasa el token de acceso en el **ExtraData** propiedad de la **AuthenticationResult** objeto. Agregue el código resaltado a **ExternalLoginCallback** para guardar el token de acceso en el **sesión** objeto. Este código se ejecuta cada vez que el usuario inicia sesión con una cuenta de Facebook.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

Aunque en este ejemplo se recupera un token de acceso de Facebook, puede recuperar el token de acceso desde cualquier proveedor externo a través de la misma clave denominada &quot;accesstoken&quot;.

## <a name="logging-off"></a>Cerrar sesión

El valor predeterminado **LogOff** método registra el usuario de su aplicación, pero no se sesión del usuario en el proveedor externo. Para la sesión del usuario en Facebook e impedir que el token de persistencia después de que el usuario ha cerrado sesión, puede agregar el código resaltado siguiente a la **LogOff** método AccountController.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

El valor que proporcione en el `next` parámetro es la dirección URL para usar después de que el usuario ha cerrado la sesión Facebook. Al realizar pruebas en el equipo local, proporcionaría el número de puerto correcto para el sitio local. En producción, proporcionaría una página predeterminada, por ejemplo, contoso.com.

## <a name="retrieve-user-information-that-requires-the-access-token"></a>Recuperar información de usuario que requiere el token de acceso

Ahora que ha almacenado el token de acceso e instalado el paquete del SDK de Facebook C#, puede utilizarlos juntos para solicitar información adicional del usuario de Facebook. En el **ExternalLoginConfirmation** método, cree una instancia de la **FacebookClient** clase pasando el valor del token de acceso. Solicitar el valor de la **comprobado** propiedad para el usuario autenticado actual. El **comprobado** propiedad indica si Facebook ha validado la cuenta a través de otros medios, como enviar un mensaje a un teléfono móvil. Guarde este valor en la base de datos.

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

Nuevamente, deberá eliminar los registros de la base de datos para el usuario, o usar otra cuenta de Facebook.

Ejecute la aplicación y registrar el nuevo usuario. Examine el **ExtraUserInformation** tabla para ver el valor de la propiedad comprobado.

## <a name="conclusion"></a>Conclusión

En este tutorial, ha creado un sitio que se integra con Facebook para la autenticación de usuario y datos de registro. Ha aprendido sobre el comportamiento predeterminado que está configurado para que la aplicación web MVC 4 y cómo personalizar el comportamiento predeterminado.

## <a name="related-topics"></a>Temas relacionados

- [Crear una aplicación ASP.NET MVC con la autenticación y base de datos SQL e implementar en Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
