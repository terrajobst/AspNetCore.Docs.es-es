# <a name="additional-claims-sample-app"></a>Aplicación de ejemplo de notificaciones adicionales

La aplicación de ejemplo muestra cómo:

* Obtener el nombre especificado y el apellido del usuario de Google y almacenar las notificaciones de nombre con los valores proporcionados por Google.
* Store el token de acceso de Google en el usuario `AuthenticationProperties`.

Para usar la aplicación de ejemplo:

1. Registrar la aplicación y obtener un identificador de cliente válido y el secreto de cliente para la autenticación de Google. Para obtener más información, consulte [el programa de instalación de inicio de sesión externo de Google](https://docs.microsoft.com/aspnet/core/security/authentication/social/google-logins).
1. Proporcione el identificador de cliente y secreto de cliente a la aplicación en el [GoogleOptions](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authentication.google.googleoptions) de `Startup.ConfigureServices`.
1. Ejecute la aplicación y solicite la página Mis notificaciones. Cuando el usuario no se ha iniciado sesión, la aplicación se redirige a Google. Inicie sesión con Google. Google redirige al usuario a la aplicación (`/MyClaims`). El usuario se autentica y se carga la página Mis notificaciones. El nombre dado y el apellido notificaciones están presentes en **notificaciones de usuario** con los valores proporcionados por Google. El token de acceso se muestra bajo **las propiedades de autenticación**.
