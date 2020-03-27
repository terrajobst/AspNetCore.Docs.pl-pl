## <a name="debug-diagnostics"></a><span data-ttu-id="3cae8-101">Diagnostyka debugowania</span><span class="sxs-lookup"><span data-stu-id="3cae8-101">Debug diagnostics</span></span>

<span data-ttu-id="3cae8-102">W celu uzyskania szczegółowych danych wyjściowych diagnostycznych routingu Ustaw `Logging:LogLevel:Microsoft` na `Debug`.</span><span class="sxs-lookup"><span data-stu-id="3cae8-102">For detailed routing diagnostic output, set `Logging:LogLevel:Microsoft` to `Debug`.</span></span> <span data-ttu-id="3cae8-103">Na przykład w środowisku programistycznym ustaw wartość *appSettings. Plik Development. JSON*:</span><span class="sxs-lookup"><span data-stu-id="3cae8-103">For example, in the development environment, set *appsettings.Development.json*:</span></span>

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