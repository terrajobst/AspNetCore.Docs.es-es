## <a name="register-the-database-context"></a>Registrar el contexto de base de datos

En este paso, el contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection). Los servicios (por ejemplo, el contexto de la base de datos) que se registran con el contenedor de inserción de dependencias (DI) están disponibles para los controladores.

Registre el contexto de la base de datos con el contenedor de servicio mediante la compatibilidad integrada para [inserción de dependencias](xref:fundamentals/dependency-injection). Reemplace el contenido del archivo *Startup.cs* con el código siguiente:

::: moniker range=">= aspnetcore-2.1"

[!code-csharp[](../../tutorials/first-web-api/samples/2.1/TodoApi/Startup.cs?highlight=3,5,13-14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

[!code-csharp[](../../tutorials/first-web-api/samples/2.0/TodoApi/Startup.cs?highlight=2,4,12-13)]

::: moniker-end  

El código anterior:

* Quita el código no usado.
* Especifica que se inserte una base de datos en memoria en el contenedor de servicios.
