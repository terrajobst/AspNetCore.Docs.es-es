### <a name="use-sqlite-for-development-sql-server-for-production"></a>Uso de SQLite para desarrollo y SQL Server para producción

Cuando se selecciona SQLite, el código generado por la plantilla está listo para desarrollo. En el código siguiente se muestra cómo insertar <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> en el inicio. `IWebHostEnvironment` se inserta para que `ConfigureServices` pueda usar SQLite en desarrollo y SQL Server en producción.

[!code-csharp[](~/includes/RP/code/StartupDevProd.cs?name=snippet&highlight=5,10,14)]
