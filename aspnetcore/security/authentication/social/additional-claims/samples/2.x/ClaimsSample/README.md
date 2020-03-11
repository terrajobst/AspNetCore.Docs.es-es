# <a name="additional-claims-sample-app"></a>Aplicación de ejemplo de notificaciones adicionales

En la aplicación de ejemplo se muestra cómo:

* Obtenga el nombre y el apellido del usuario dado de Google y almacene las notificaciones de nombre con los valores proporcionados por Google.
* Almacene el token de acceso de Google en el `AuthenticationProperties`del usuario.

Para usar la aplicación de ejemplo:

1. Registre la aplicación y obtenga un identificador de cliente y un secreto de cliente válidos para la autenticación de Google. Para obtener más información, consulte [configuración de inicio de sesión externo de Google](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).
1. Proporcione el identificador de cliente y el secreto de cliente a la aplicación en el [GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) de `Startup.ConfigureServices`.
1. Ejecute la aplicación y solicite la página mis notificaciones. Cuando el usuario no ha iniciado sesión, la aplicación redirige a Google. Inicie sesión con Google. Google redirige al usuario a la aplicación (`/MyClaims`). El usuario se autentica y se carga la página mis notificaciones. Las notificaciones de nombre y apellidos especificadas están presentes en **notificaciones de usuario** con los valores proporcionados por Google. El token de acceso se muestra en **propiedades de autenticación**.
