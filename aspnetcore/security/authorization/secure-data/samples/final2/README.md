# <a name="how-to-buildrun-secure-user-data-sample"></a>Procedimiento para generar y ejecutar el ejemplo de datos de usuario segura

* Establezca la contraseña con la herramienta Secret Manager:

  `dotnet user-secrets set SeedUserPW <pw>`

* Actualización de la base de datos:

    `dotnet ef database update`

* Habilitar SSL en el proyecto
