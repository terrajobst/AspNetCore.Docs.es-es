## <a name="troubleshoot"></a><span data-ttu-id="1c44e-101">Solución de problemas</span><span class="sxs-lookup"><span data-stu-id="1c44e-101">Troubleshoot</span></span>

### <a name="cookies-and-site-data"></a><span data-ttu-id="1c44e-102">Cookies y datos del sitio</span><span class="sxs-lookup"><span data-stu-id="1c44e-102">Cookies and site data</span></span>

<span data-ttu-id="1c44e-103">Las cookies y los datos del sitio pueden persistir en las actualizaciones de la aplicación e interferir con las pruebas y la solución de problemas.</span><span class="sxs-lookup"><span data-stu-id="1c44e-103">Cookies and site data can persist across app updates and interfere with testing and troubleshooting.</span></span> <span data-ttu-id="1c44e-104">Borre lo siguiente al realizar cambios en el código de la aplicación, los cambios en la cuenta de usuario con el proveedor o en la configuración de la aplicación de proveedor:</span><span class="sxs-lookup"><span data-stu-id="1c44e-104">Clear the following when making app code changes, user account changes with the provider, or provider app configuration changes:</span></span>

* <span data-ttu-id="1c44e-105">Cookies de inicio de sesión del usuario</span><span class="sxs-lookup"><span data-stu-id="1c44e-105">User sign-in cookies</span></span>
* <span data-ttu-id="1c44e-106">Cookies de aplicaciones</span><span class="sxs-lookup"><span data-stu-id="1c44e-106">App cookies</span></span>
* <span data-ttu-id="1c44e-107">Datos almacenados en caché y almacenados en el sitio</span><span class="sxs-lookup"><span data-stu-id="1c44e-107">Cached and stored site data</span></span>

<span data-ttu-id="1c44e-108">Un enfoque para evitar que las cookies persistentes y los datos del sitio interfieran con las pruebas y la solución de problemas es:</span><span class="sxs-lookup"><span data-stu-id="1c44e-108">One approach to prevent lingering cookies and site data from interfering with testing and troubleshooting is to:</span></span>

* <span data-ttu-id="1c44e-109">Utilice un navegador para las pruebas que puede configurar para eliminar todos los datos de cookies y sitios cada vez que se cierra el navegador.</span><span class="sxs-lookup"><span data-stu-id="1c44e-109">Use a browser for testing that you can configure to delete all cookie and site data each time the browser is closed.</span></span>
* <span data-ttu-id="1c44e-110">Cierre el explorador entre cualquier cambio en la configuración de la aplicación, el usuario de prueba o el proveedor.</span><span class="sxs-lookup"><span data-stu-id="1c44e-110">Close the browser between any change to the app, test user, or provider configuration.</span></span>

### <a name="run-the-server-app"></a><span data-ttu-id="1c44e-111">Ejecute la aplicación Servidor</span><span class="sxs-lookup"><span data-stu-id="1c44e-111">Run the Server app</span></span>

<span data-ttu-id="1c44e-112">Al probar y solucionar problemas de una aplicación Blazor hospedada, asegúrese de que está ejecutando la aplicación desde el proyecto **de servidor.**</span><span class="sxs-lookup"><span data-stu-id="1c44e-112">When testing and troubleshooting a hosted Blazor app, make sure that you're running the app from the **Server** project.</span></span> <span data-ttu-id="1c44e-113">Por ejemplo, en Visual Studio, confirme que el proyecto de servidor se resalta en el **Explorador** de soluciones antes de iniciar la aplicación con cualquiera de los enfoques siguientes:</span><span class="sxs-lookup"><span data-stu-id="1c44e-113">For example in Visual Studio, confirm that the Server project is highlighted in **Solution Explorer** before you start the app with any of the following approaches:</span></span>

* <span data-ttu-id="1c44e-114">Haga clic en el botón **Ejecutar**.</span><span class="sxs-lookup"><span data-stu-id="1c44e-114">Select the **Run** button.</span></span>
* <span data-ttu-id="1c44e-115">Use **Depurar** > **Iniciar depuración depuración** en el menú.</span><span class="sxs-lookup"><span data-stu-id="1c44e-115">Use **Debug** > **Start Debugging** from the menu.</span></span>
* <span data-ttu-id="1c44e-116">Presione <kbd>F5</kbd>.</span><span class="sxs-lookup"><span data-stu-id="1c44e-116">Press <kbd>F5</kbd>.</span></span>
