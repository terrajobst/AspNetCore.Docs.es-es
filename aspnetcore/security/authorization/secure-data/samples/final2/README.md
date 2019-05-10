# <a name="how-to-buildrun-secure-user-data-sample"></a><span data-ttu-id="d9979-101">Procedimiento para generar y ejecutar el ejemplo de datos de usuario segura</span><span class="sxs-lookup"><span data-stu-id="d9979-101">How to build/run Secure user data sample</span></span>

* <span data-ttu-id="d9979-102">Establezca la contraseña con la herramienta Secret Manager:</span><span class="sxs-lookup"><span data-stu-id="d9979-102">Set password with the Secret Manager tool:</span></span>

  `dotnet user-secrets set SeedUserPW <pw>`

* <span data-ttu-id="d9979-103">Actualización de la base de datos:</span><span class="sxs-lookup"><span data-stu-id="d9979-103">Update the database:</span></span>

  `dotnet ef database update`

* <span data-ttu-id="d9979-104">Habilitar HTTPS en el proyecto</span><span class="sxs-lookup"><span data-stu-id="d9979-104">Enable HTTPS in the project</span></span>
