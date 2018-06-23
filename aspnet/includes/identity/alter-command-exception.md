> Algunos comandos no se admiten si la aplicación usa SQLite como su almacén de datos de identidad. Debido a las limitaciones en el motor de base de datos, `Alter` comandos producen la excepción siguiente:
>
> "System.NotSupportedException: SQLite no admite esta operación de migración." 
>
> Como una solución alternativa, ejecutar migraciones de Code First en la base de datos para cambiar las tablas.
