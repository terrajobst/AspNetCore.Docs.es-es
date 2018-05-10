# <a name="how-to-buildrun-secure-user-data-sample"></a>Cómo generar y ejecutar el ejemplo de datos de usuario seguras

* Establecer la contraseña con la herramienta Administrador de secreto:

  `dotnet user-secrets set SeedUserPW <pw>`

* Actualizar la base de datos:

    `dotnet ef database update`

* Habilitar SSL en el proyecto