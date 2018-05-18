# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="6f9e2-101">Cómo generar y ejecutar el ejemplo de datos de usuario seguras</span><span class="sxs-lookup"><span data-stu-id="6f9e2-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="6f9e2-102">Establecer la contraseña con la herramienta Administrador de secreto:</span><span class="sxs-lookup"><span data-stu-id="6f9e2-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="6f9e2-103">Actualizar la base de datos:</span><span class="sxs-lookup"><span data-stu-id="6f9e2-103">Update the database:</span></span>

    `dotnet ef database update`

* <span data-ttu-id="6f9e2-104">Habilitar SSL en el proyecto</span><span class="sxs-lookup"><span data-stu-id="6f9e2-104">Enable SSL in the project</span></span>