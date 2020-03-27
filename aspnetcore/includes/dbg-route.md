## <a name="debug-diagnostics"></a>Diagnostyka debugowania

W celu uzyskania szczegółowych danych wyjściowych diagnostycznych routingu Ustaw `Logging:LogLevel:Microsoft` na `Debug`. Na przykład w środowisku programistycznym ustaw wartość *appSettings. Plik Development. JSON*:

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