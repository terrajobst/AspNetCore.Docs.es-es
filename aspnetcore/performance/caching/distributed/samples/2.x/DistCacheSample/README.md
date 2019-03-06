# <a name="aspnet-core-distributed-cache-sample"></a>Ejemplo de caché distribuida de ASP.NET Core

En este ejemplo se muestra el uso de una caché distribuida. Este ejemplo muestra el escenario descrito en la [trabajar con una memoria caché distribuida en ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/caching/distributed) tema.

En el entorno de producción, la aplicación de ejemplo está configurada para usar una caché distribuida de SQL Server. Para volver a configurar la aplicación para usar una caché en Redis distribuida, cambiar la directiva de preprocesador en la parte superior de la *Startup.cs* archivo para usar Redis (`#define Redis // SQLServer`). Para obtener más información, consulte [las directivas de preprocesador en el código de ejemplo](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code).
