---
title: Obsługa błędów w programie ASP.NET Core
author: tdykstra
description: Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/01/2018
uid: fundamentals/error-handling
ms.openlocfilehash: fbc86d36f66e71e6ebd84f536148fba2e3c452d8
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/12/2018
ms.locfileid: "51570064"
---
# <a name="handle-errors-in-aspnet-core"></a>Obsługa błędów w programie ASP.NET Core

Przez [Steve Smith](https://ardalis.com/) i [Tom Dykstra](https://github.com/tdykstra/)

W tym artykule opisano typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>Na stronie wyjątków dla deweloperów

::: moniker range=">= aspnetcore-2.1"

Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*. Strona jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Dodaj wiersz w celu `Startup.Configure` metody:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*. Strona jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage). Dodaj wiersz w celu `Startup.Configure` metody:

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*. Strona jest udostępniana przez dodanie odwołania do pakietu dla [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakietu w pliku projektu. Dodaj wiersz w celu `Startup.Configure` metody:

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Umieść wywołanie [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) przed dowolnego oprogramowania pośredniczącego, którym chcesz przechwytywać wyjątków, takie jak `app.UseMvc`.

> [!WARNING]
> Włącz na stronie wyjątków dla deweloperów **tylko wtedy, gdy aplikacja jest uruchomiona w środowisku programistycznym**. Nie chcesz publicznie udostępnić szczegółowe informacje o wyjątku, gdy aplikacja jest uruchamiana w środowisku produkcyjnym. [Dowiedz się więcej na temat konfigurowania środowisk](xref:fundamentals/environments).

Aby wyświetlić stronę wyjątek dla deweloperów, uruchom przykładową aplikację w środowisku równa `Development` i Dodaj `?throw=true` do podstawowego adresu URL aplikacji. Strona zawiera kilka kart zawierających informacje o wyjątku i żądanie. Na pierwszej karcie znajdują się ślad stosu:

![Ślad stosu](error-handling/_static/developer-exception-page.png)

Następna karta przedstawia zapytanie parametry ciągu ewentualne:

![Parametry ciągu zapytania](error-handling/_static/developer-exception-page-query.png)

Jeśli żądanie ma pliki cookie, są wyświetlane na **plików cookie** kartę. Nagłówki są widoczne na karcie ostatnich:

![Nagłówki](error-handling/_static/developer-exception-page-headers.png)

## <a name="configure-a-custom-exception-handling-page"></a>Konfigurowanie niestandardowych wyjątków, Obsługa strony

Strona obsługi wyjątków do użycia, gdy aplikacja nie jest uruchomiona konfiguracji `Development` środowiska:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

W aplikacji stron Razor [dotnet nowe](/dotnet/core/tools/dotnet-new) stron Razor szablon zawiera stronę błędu i błąd `PageModel` klasy w *stron* folderu.

W aplikacji MVC nie dekoracji metody akcji programu obsługi błędów z atrybutami metody HTTP, takich jak `HttpGet`. Jawne zleceń uniemożliwić metody osiągając niektórych żądań. Zezwalaj na anonimowy dostęp do metody, aby były nieuwierzytelnionym użytkownikom możliwość odbierania widoku błędów.

Na przykład, następujące metody obsługi błędów są dostarczane przez [dotnet nowe](/dotnet/core/tools/dotnet-new) szablon MVC i pojawia się na kontrolerze głównej:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configure-status-code-pages"></a>Konfigurowanie stanu strony kodowe

Domyślnie aplikacji nie zapewnia stronę kodową sformatowanego stanu dla kodów stanu HTTP, takich jak *404 Nie znaleziono*. Aby zapewnić stan stron kodowych, użyj oprogramowanie pośredniczące strony kodu stanu.

::: moniker range=">= aspnetcore-2.1"

Oprogramowanie pośredniczące jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Oprogramowanie pośredniczące jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [pakiet meta Microsoft.aspnetcore.all](xref:fundamentals/metapackage).

::: moniker-end

::: moniker range="< aspnetcore-2.0"

Oprogramowanie pośredniczące jest udostępniana przez dodanie odwołania do pakietu dla [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakietu w pliku projektu.

::: moniker-end

Dodaj wiersz w celu `Startup.Configure` metody:

```csharp
app.UseStatusCodePages();
```

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> powinna być wywoływana przed żądaniem obsługi middlewares w potoku (na przykład oprogramowanie pośredniczące plików statycznych i oprogramowanie pośredniczące MVC).

Domyślnie oprogramowanie pośredniczące strony kod stanu dodaje tekstowy programy obsługi dla typowych kodów stanu, takie jak 404:

![strona 404](error-handling/_static/default-404-status-code.png)

Oprogramowanie pośredniczące obsługuje kilka metod rozszerzenia. Jedna metoda przyjmuje wyrażenia lambda:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Przeciążenia `UseStatusCodePages` przyjmuje zawartości typu i formatu ciągu:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```
### <a name="redirect-re-execute-extension-methods"></a>Przekieruj wykonaj ponownie metody rozszerzenia

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:

* Wysyła *302 — znaleziono* kod stanu do klienta.
* Przekierowuje klienta do lokalizacji, udostępnionego w szablonie adresu URL. 

Szablon może obejmować `{0}` symbol zastępczy dla kodu stanu. Szablon musi rozpoczynać się od ukośnika (`/`).

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:

* Zwraca oryginalny kod stanu do klienta.
* Określa, czy treść odpowiedzi powinny być generowane ponownie, wykonując Potok żądań przy użyciu ścieżki alternatywnej. 

Szablon może obejmować `{0}` symbol zastępczy dla kodu stanu. Szablon musi rozpoczynać się od ukośnika (`/`).

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

Strony kodowe stanu można wyłączyć dla określonych żądań w metodzie obsługi stron Razor lub kontroler MVC. Aby wyłączyć stron kodowych stanu, próba pobrania [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) w żądaniu [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) kolekcji i wyłączyć tę funkcję, jeśli jest dostępna:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Aby użyć `UseStatusCodePages*` przeciążenia, że wskazuje punkt końcowy w ramach aplikacji, Utwórz widoku MVC lub strona Razor dla punktu końcowego. Na przykład [dotnet nowe](/dotnet/core/tools/dotnet-new) szablonu dla aplikacji stron Razor tworzy następującą stronę i klasy modelu strony:

*Error.cshtml*:

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*:

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a>Kod obsługi wyjątków

Kod obsługi stron wyjątków może zgłaszać wyjątki. Często jest dobrym rozwiązaniem dla stron błędów produkcyjnych, które ma zawierać zawartość statyczną wyłącznie.

Ponadto należy pamiętać, że nie można zmienić kod stanu odpowiedzi po wysłaniu nagłówki odpowiedzi, ponieważ wszystkie strony wyjątków lub obsługi uruchomić. Odpowiedzi muszą być wypełnione albo połączenie zostało przerwane.

## <a name="server-exception-handling"></a>Obsługa wyjątków serwera

Oprócz logiki aplikacji, obsługi wyjątków [serwera](xref:fundamentals/servers/index) hostującego twoją aplikację wykonuje niektóre obsługi wyjątków. Jeśli serwer wyłapuje wyjątek, zanim nagłówki są wysyłane, serwer wysyła *500 Wewnętrzny błąd serwera* odpowiedzi z bez treści. Jeśli serwer wyłapuje wyjątek po wysłaniu nagłówków, serwer zamyka połączenie. Żądania, które nie są obsługiwane przez aplikację są obsługiwane przez serwer. Każdy wyjątek, który występuje odbywa się przez wyjątek serwera obsługi. Oprogramowanie pośredniczące obsługi wyjątków lub filtry nie wpływają na to zachowanie lub dowolne skonfigurowane strony błędów niestandardowych.

## <a name="startup-exception-handling"></a>Obsługa wyjątków uruchamiania

Tylko warstwa hostingu może obsługiwać wyjątki, które mają miejsce podczas uruchamiania aplikacji. Za pomocą [hosta sieci Web](xref:fundamentals/host/web-host), możesz [Konfigurowanie zachowania hosta w odpowiedzi na błędy podczas uruchamiania](xref:fundamentals/host/web-host#detailed-errors) z `captureStartupErrors` i `detailedErrors` kluczy.

Jeśli wystąpi błąd, po adresem/port hosta powiązania hostingu można wyświetlić tylko stronę błędu dla błędów uruchamiania przechwycone. Jeśli wszystkie powiązania nie powiedzie się z jakiegokolwiek powodu, hostingu warstwy rejestruje wyjątek krytyczny dotnet awarii procesów, i stronę błędu, nie jest wyświetlane, gdy aplikacja jest uruchomiona na [Kestrel](xref:fundamentals/servers/kestrel) serwera.

Podczas uruchamiania na [IIS](/iis) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 niepowodzenia procesu* jest zwracany przez [modułu ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) , jeśli proces nie może być pracę. Aby uzyskać informacje na temat rozwiązywania problemów problemy z uruchamianiem w przypadku hostowania za pomocą programu IIS, zobacz <xref:host-and-deploy/iis/troubleshoot>. Aby uzyskać informacje na temat rozwiązywania problemów z uruchamianiem przy użyciu usługi Azure App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="aspnet-core-mvc-error-handling"></a>Obsługa błędów programu ASP.NET Core MVC

[MVC](xref:mvc/overview) aplikacje mają pewne dodatkowe opcje obsługi błędów, takich jak konfigurowanie filtry wyjątków i sprawdzaniu poprawności modelu.

### <a name="exception-filters"></a>Filtry wyjątków

Filtry wyjątków można skonfigurować globalnie lub na zasadzie na kontroler lub akcję w aplikacji MVC. Te filtry obsługi nieobsługiwanego wyjątku, który występuje podczas wykonywania akcji kontrolera lub inny filtr. Te filtry nie są wywoływane w przeciwnym razie. Aby dowiedzieć się więcej, zobacz [filtry](xref:mvc/controllers/filters).

> [!TIP]
> Filtry wyjątków są dobre zalewania wyjątków występujących w ramach akcji MVC, ale nie jest tak elastyczna jak błąd obsługi oprogramowania pośredniczącego. Zazwyczaj preferują użycie oprogramowania pośredniczącego i używania filtrów, tylko gdy potrzebujesz do obsługi błędów *inaczej* oparte na akcję MVC, która jest wybierany.

### <a name="handling-model-state-errors"></a>Obsługa błędy stanu modelu

[Walidacja modelu](xref:mvc/models/validation) występuje przed wywołaniem akcji każdego kontrolera i odpowiada metoda akcji sprawdzić `ModelState.IsValid` i odpowiednio reagują.

Niektóre aplikacje chce wykonać standardowej Konwencji za zajmowanie się błędy sprawdzania poprawności modelu, w którym to przypadku [filtru](xref:mvc/controllers/filters) może być w odpowiednim miejscu, aby zaimplementować takie zasady. Należy sprawdzić, jak Twoje działania zachowują się ze Stanami nieprawidłowy model. Dowiedz się więcej w [logikę kontrolera testu](xref:mvc/controllers/testing).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
