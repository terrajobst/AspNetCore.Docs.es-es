## <a name="debug-diagnostics"></a><span data-ttu-id="2abcd-101">Diagnóstico de depuración</span><span class="sxs-lookup"><span data-stu-id="2abcd-101">Debug diagnostics</span></span>

<span data-ttu-id="2abcd-102">Para ver la salida detallada del diagnóstico de cálculo de ruta, establezca `Logging:LogLevel:Microsoft` en `Debug`.</span><span class="sxs-lookup"><span data-stu-id="2abcd-102">For detailed routing diagnostic output, set `Logging:LogLevel:Microsoft` to `Debug`.</span></span> <span data-ttu-id="2abcd-103">Por ejemplo, en el entorno de desarrollo, establezca *appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="2abcd-103">For example, in the development environment, set *appsettings.Development.json*:</span></span>

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