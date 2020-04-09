## <a name="troubleshoot"></a>Solución de problemas

### <a name="cookies-and-site-data"></a>Cookies y datos del sitio

Las cookies y los datos del sitio pueden persistir en las actualizaciones de la aplicación e interferir con las pruebas y la solución de problemas. Borre lo siguiente al realizar cambios en el código de la aplicación, los cambios en la cuenta de usuario con el proveedor o en la configuración de la aplicación de proveedor:

* Cookies de inicio de sesión del usuario
* Cookies de aplicaciones
* Datos almacenados en caché y almacenados en el sitio

Un enfoque para evitar que las cookies persistentes y los datos del sitio interfieran con las pruebas y la solución de problemas es:

* Utilice un navegador para las pruebas que puede configurar para eliminar todos los datos de cookies y sitios cada vez que se cierra el navegador.
* Cierre el explorador entre cualquier cambio en la configuración de la aplicación, el usuario de prueba o el proveedor.

### <a name="run-the-server-app"></a>Ejecute la aplicación Servidor

Al probar y solucionar problemas de una aplicación Blazor hospedada, asegúrese de que está ejecutando la aplicación desde el proyecto **de servidor.** Por ejemplo, en Visual Studio, confirme que el proyecto de servidor se resalta en el **Explorador** de soluciones antes de iniciar la aplicación con cualquiera de los enfoques siguientes:

* Haga clic en el botón **Ejecutar**.
* Use **Depurar** > **Iniciar depuración depuración** en el menú.
* Presione <kbd>F5</kbd>.
