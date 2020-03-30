## <a name="debug-diagnostics"></a>Diagnóstico de depuración

Para ver la salida detallada del diagnóstico de cálculo de ruta, establezca `Logging:LogLevel:Microsoft` en `Debug`. Por ejemplo, en el entorno de desarrollo, establezca *appsettings.Development.json*:

```JSON
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft": "Debug",
      "Microsoft.Hosting.Lifetime": "Information"
    }
  }
}