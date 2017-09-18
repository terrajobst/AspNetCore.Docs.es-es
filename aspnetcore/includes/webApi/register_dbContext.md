## <a name="register-the-database-context"></a>Registrar el contexto de base de datos

Para insertar el contexto de base de datos en el controlador, es necesario registrarlo con el contenedor de [inserción de dependencias](xref:fundamentals/dependency-injection). Registre el contexto de base de datos con el contenedor de servicio mediante la compatibilidad integrada para [inserción de dependencias](xref:fundamentals/dependency-injection). Reemplace el contenido del archivo *Startup.cs* con lo siguiente:

[!code-csharp[Main](../../tutorials/first-web-api/sample/TodoApi/Startup.cs?highlight=2,4,12)]

El código anterior:

* Quita el código que no estamos usando.
* Especifica que se inserte una base de datos en memoria en el contenedor de servicios.
