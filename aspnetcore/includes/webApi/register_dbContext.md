## <a name="register-the-database-context"></a>Registrar el contexto de base de datos

En este paso, el contexto de base de datos se registra con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection). Los servicios (por ejemplo, el contexto de la base de datos) que se registran con el contenedor de inserción de dependencias (DI) están disponibles para los controladores.

Registre el contexto de la base de datos con el contenedor de servicio mediante la compatibilidad integrada para [inserción de dependencias](xref:fundamentals/dependency-injection). Reemplace el contenido del archivo *Startup.cs* con el código siguiente:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

El código anterior:

* Quita el código que no se utiliza.
* Especifica que se inserte una base de datos en memoria en el contenedor de servicios.