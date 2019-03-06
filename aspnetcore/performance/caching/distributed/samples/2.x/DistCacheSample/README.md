# <a name="aspnet-core-distributed-cache-sample"></a>Przykładowe rozproszonej pamięci podręcznej platformy ASP.NET Core

Ten przykład ilustruje użycie rozproszonej pamięci podręcznej. W tym przykładzie przedstawiono scenariusz opisany w [Praca z rozproszoną pamięcią podręczną w programie ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/caching/distributed) tematu.

W środowisku produkcyjnym Przykładowa aplikacja jest skonfigurowane do korzystania z rozproszoną pamięcią podręczną programu SQL Server. Aby ponownie skonfigurować aplikację do używania rozproszonej pamięci podręcznej redis cache, należy zmienić dyrektywy preprocesora na początku *Startup.cs* plik do użycia pamięci podręcznej Redis (`#define Redis // SQLServer`). Aby uzyskać więcej informacji, zobacz [dyrektywy preprocesora w przykładowym kodzie](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code).
