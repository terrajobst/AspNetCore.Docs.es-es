> [!WARNING]
> Por motivos de seguridad, debe participar en el enlace de datos de solicitud `GET` con las propiedades del modelo de página. Compruebe las entradas de los usuarios antes de asignarlas a las propiedades. Si participa en el enlace de `GET`, le puede ser útil al trabajar con escenarios que dependan de cadenas de consultas o valores de rutas.
>
> Para enlazar una propiedad en solicitudes `GET`, establezca la propiedad `SupportsGet` del atributo [[BindProperty]](/dotnet/api/microsoft.aspnetcore.mvc.bindpropertyattribute) como `true`: `[BindProperty(SupportsGet = true)]`
