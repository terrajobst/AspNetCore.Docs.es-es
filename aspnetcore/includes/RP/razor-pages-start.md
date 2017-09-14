La plantilla predeterminada crea los vínculos y las páginas **RazorPagesMovie**, **Inicio**, **Sobre** y **Contacto**. Según el tamaño de la ventana del explorador, tendrá que hacer clic en el icono de navegación para mostrar los vínculos.

![Página principal o de índice](../../tutorials/razor-pages/razor-pages-start/_static/home2.png)

Pruebe los vínculos. Los vínculos **RazorPagesMovie** e **Inicio** dirigen a la página de índice. Los vínculos **Acerca de** y **Contacto** dirigen a las páginas `About` y `Contact` respectivamente.

## <a name="project-files-and-folders"></a>Archivos y carpetas del proyecto

En la tabla siguiente se enumeran los archivos y las carpetas del proyecto. En este tutorial, el archivo *Startup.cs* es el más importante. No es necesario revisar todos los vínculos siguientes. Los vínculos se proporcionan como referencia para cuando necesite más información sobre un archivo o una carpeta del proyecto.

| Archivo o carpeta              | Propósito |
| ----------------- | ------------ | 
| wwwroot | Contiene archivos estáticos. Consulte [Working with static files](xref:fundamentals/static-files) (Trabajar con archivos estáticos). |
| Páginas | Carpeta para [páginas de Razor](xref:mvc/razor-pages/index). | 
| *appsettings.json* | [Configuración](xref:fundamentals/configuration) |
| *bower.json* | Administración de paquetes del lado cliente Consulte [Bower](xref:client-side/bower).|
| *Program.cs* | [Aloja](xref:fundamentals/hosting) la aplicación de ASP.NET Core.|
| *Startup.cs* | Configura los servicios y la canalización de solicitudes. Consulte [Inicio](xref:fundamentals/startup).|

### <a name="the-pages-folder"></a>Carpeta Páginas

El archivo *_Layout.cshtml* contiene elementos HTML comunes (scripts y hojas de estilos) y establece el diseño de la aplicación. Por ejemplo, al hacer clic en **RazorPagesMovie**, **Inicio**, **Acerca de** o **Contacto**, verá los mismos elementos. Los elementos comunes incluyen el menú de navegación de la parte superior y el encabezado de la parte inferior de la ventana. Consulte [Diseño](xref:mvc/views/layout) para obtener más información.

El archivo *_ViewStart.cshtml* establece la propiedad `Layout` de las páginas de Razor que para usar el archivo *_Layout.cshtml*. Consulte [Diseño](xref:mvc/views/layout) para obtener más información.

El archivo *_ViewImports.cshtml* contiene directivas de Razor que se importan en cada página de Razor. Consulte [Importing Shared Directives](xref:mvc/views/layout#importing-shared-directives) (Importar directivas compartidas) para obtener más información.

El archivo *_ValidationScriptsPartial.cshtml* proporciona una referencia de los scripts de validación de [jQuery](https://jquery.com/). Al agregar las páginas `Create` y `Edit` más adelante en el tutorial, se usará el archivo *_ValidationScriptsPartial.cshtml*.

Las páginas `About`, `Contact` y `Index` son páginas básicas que puede usar para iniciar una aplicación. La página `Error` se usa para mostrar información de errores.