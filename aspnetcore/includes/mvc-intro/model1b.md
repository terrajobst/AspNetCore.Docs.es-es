Agregue las propiedades siguientes a la clase `Movie`:

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/Movie.cs?name=snippet1)]

la clase `Movie` contiene:

* El campo `Id`, que requiere la base de datos para la clave principal.
* `[DataType(DataType.Date)]`:  el atributo [DataType](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.internal.datatypeattributeadapter) especifica el tipo de datos (`Date`). Con este atributo:

  * El usuario no tiene que especificar información horaria en el campo de fecha.
  * Solo se muestra la fecha, no información horaria.

Los elementos [DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) se tratan en un tutorial posterior.