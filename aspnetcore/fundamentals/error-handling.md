---
title: Obsługa błędów w programie ASP.NET Core
author: tdykstra
description: Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/01/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a2ae2cb25c8cc5048b189b4035abbfc32a29aaff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345508"
---
# <a name="handle-errors-in-aspnet-core"></a>Obsługa błędów w programie ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), i [Steve Smith](https://ardalis.com/)

W tym artykule opisano typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([sposobu pobierania](xref:index#how-to-download-a-sample))

## <a name="developer-exception-page"></a>Stronie wyjątków dla deweloperów

Aby skonfigurować aplikację tak, aby wyświetlić stronę, która zawiera szczegółowe informacje o wyjątkach, należy użyć *stronie wyjątków deweloperów*. Strona jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Dodaj wiersz w celu `Startup.Configure` metody:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=5)]

Umieść wywołanie <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> przed dowolnego oprogramowania pośredniczącego, którym chcesz przechwytywać wyjątki.

> [!WARNING]
> Włącz na stronie wyjątków dla deweloperów **tylko wtedy, gdy aplikacja jest uruchomiona w środowisku programistycznym**. Nie chcesz publicznie udostępnić szczegółowe informacje o wyjątku, gdy aplikacja jest uruchamiana w środowisku produkcyjnym. Aby uzyskać więcej informacji na temat konfigurowania środowiska, zobacz <xref:fundamentals/environments>.

Aby wyświetlić stronę wyjątek dla deweloperów, uruchom przykładową aplikację w środowisku równa `Development` i Dodaj `?throw=true` do podstawowego adresu URL aplikacji. Strona zawiera następujące informacje dotyczące wyjątku i żądanie:

* Ślad stosu
* Parametry ciągu zapytania (jeśli istnieje)
* Pliki cookie (jeśli istnieje)
* Nagłówki

## <a name="configure-a-custom-exception-handling-page"></a>Konfigurowanie niestandardowych wyjątków, Obsługa strony

Gdy aplikacja nie jest uruchomiona w środowisku programistycznym, wywołaj <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> metodę rozszerzenia, aby dodać obsługę wyjątków w oprogramowaniu pośredniczącym. Oprogramowanie pośredniczące:

* Przechwytuje wyjątki.
* Rejestruje wyjątki.
* Ponownie wykonuje żądanie w potoku usługi alternatywnej strony lub kontroler wskazane. Żądanie nie jest wykonany ponownie, jeśli odpowiedź została uruchomiona.

W poniższym przykładzie z przykładowej aplikacji <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> dodaje oprogramowanie pośredniczące obsługi wyjątków w środowiskach innych niż. Określa metody rozszerzenia, stronę błędu lub kontrolera w `/Error` punktu końcowego dla żądań wykonany ponownie po wyjątki są przechwytywane i rejestrowane:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=9)]

Szablon aplikacji stron Razor zawiera stronę błędu (*.cshtml*) i <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> klasy (`ErrorModel`) w folderze strony.

W aplikacji MVC następującą metodę procedury obsługi błędów jest zawarte w szablonie aplikacji MVC i pojawia się na kontrolerze głównej:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

Nie dekoracji metody akcji programu obsługi błędów z atrybutami metody HTTP, takich jak `HttpGet`. Jawne zleceń uniemożliwić metody osiągając niektórych żądań. Zezwalaj na anonimowy dostęp do metody, aby były nieuwierzytelnionym użytkownikom możliwość odbierania widoku błędów.

## <a name="configure-status-code-pages"></a>Konfigurowanie stanu strony kodowe

Domyślnie aplikacji ASP.NET Core nie zapewnia stan stronę kodową dla kodów stanu HTTP, takich jak *404 — Nie można odnaleźć*. Aplikacja zwraca kod stanu i pusta treść odpowiedzi. Aby zapewnić stan stron kodowych, użyj oprogramowanie pośredniczące strony kodu stanu.

Oprogramowanie pośredniczące jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który jest dostępny w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Dodaj wiersz w celu `Startup.Configure` metody:

```csharp
app.UseStatusCodePages();
```

Wywołaj <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> metoda przed żądaniem obsługi oprogramowania pośredniczącego (na przykład oprogramowanie pośredniczące plików statycznych i oprogramowanie pośredniczące MVC).

Domyślnie, oprogramowanie pośredniczące strony kod stanu dodaje tekstowy programy obsługi dla typowych kodów stanu, takie jak *404 — Nie można odnaleźć*:

```
Status Code: 404; Not Found
```

Oprogramowanie pośredniczące obsługuje kilka metod rozszerzenia, które pozwalają dostosować jego zachowanie.

Przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> pobiera Wyrażenie lambda, które służy do przetwarzania logiki niestandardowej obsługi błędów i ręcznie napisać odpowiedzi:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> przyjmuje zawartości typu i formatu ciągu, który służy do dostosowywania zawartości tekstu typu i odpowiedzi:

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a>Przekierowanie, a następnie wykonaj ponownie metody rozszerzenia

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:

* Wysyła *302 — znaleziono* kod stanu do klienta.
* Przekierowuje klienta do lokalizacji, udostępnionego w szablonie adresu URL.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> często używane podczas aplikacji:

* Należy przekierowuje klienta do innego punktu końcowego, zazwyczaj w przypadkach, gdzie przetwarza błąd przez inną aplikację. W przypadku aplikacji sieci web paska adresu przeglądarki klienta odzwierciedla przekierowanego punktu końcowego.
* Nie należy zachować i zwróć oryginalny kod stanu odpowiedzi przekierowania początkowej.

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:

* Zwraca oryginalny kod stanu do klienta.
* Generuje treści odpowiedzi przy ponownym wykonaniem Potok żądań przy użyciu ścieżki alternatywnej.

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> jest często używana aplikacja powinna:

* Proces żądania bez przekierowania folderu do innego punktu końcowego. W przypadku aplikacji sieci web paska adresu przeglądarki klienta odzwierciedla pierwotnie żądanego punktu końcowego.
* Zachowanie i powrócić do oryginalnego kodu stanu z odpowiedzią.

Szablony mogą obejmować symbol zastępczy (`{0}`) dla kodu stanu. Szablon musi rozpoczynać się od ukośnika (`/`). Korzystając z symbolem zastępczym, upewnij się, że punkt końcowy (strona lub kontroler) może przetwarzać segmentu ścieżki. Na przykład strona Razor dla błędów należy zaakceptować wartość segmentu ścieżki opcjonalnie za pomocą `@page` dyrektywy:

```cshtml
@page "{code?}"
```

Strony kodowe stanu można wyłączyć dla określonych żądań w metodzie obsługi stron Razor lub kontroler MVC. Aby wyłączyć stron kodowych stanu, próba pobrania <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> w żądaniu [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) kolekcji i wyłączyć tę funkcję, jeśli jest dostępna:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

Aby użyć <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> przeciążenia, że wskazuje punkt końcowy w ramach aplikacji, Utwórz widoku MVC lub strona Razor dla punktu końcowego. Na przykład szablon aplikacji stron Razor tworzy następującą stronę i klasy modelu strony:

*Error.cshtml*:

::: moniker range=">= aspnetcore-2.2"

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
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

*Error.cshtml.cs*:

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

::: moniker-end

## <a name="exception-handling-code"></a>Kod obsługi wyjątków

Kod obsługi stron wyjątków może zgłaszać wyjątki. Często jest dobrym rozwiązaniem dla stron błędów produkcyjnych, które ma zawierać zawartość statyczną wyłącznie.

Ponadto należy pamiętać, że wysłany nagłówki odpowiedzi:

* Aplikacja nie można zmienić kod stanu odpowiedzi.
* Nie można uruchomić wszystkie strony wyjątków lub programów obsługi. Odpowiedzi muszą być wypełnione albo połączenie zostało przerwane.

## <a name="server-exception-handling"></a>Obsługa wyjątków serwera

Oprócz logiki aplikacji, obsługi wyjątków [implementacji serwera](xref:fundamentals/servers/index) może obsługiwać niektóre wyjątki. Jeśli serwer wyłapuje wyjątek, przed wysłaniem nagłówki odpowiedzi, serwer wysyła *500 — Wewnętrzny błąd serwera* odpowiedzi bez treści odpowiedzi. Jeśli serwer wyłapuje wyjątek po wysłaniu nagłówki odpowiedzi, serwer zamyka połączenie. Żądania, które nie są obsługiwane przez aplikację są obsługiwane przez serwer. Każdy wyjątek, który występuje odbywa się przez wyjątek serwera obsługi. Oprogramowanie pośredniczące obsługi wyjątków lub filtry nie wpływają na to zachowanie lub dowolne skonfigurowane strony błędów niestandardowych.

## <a name="startup-exception-handling"></a>Obsługa wyjątków uruchamiania

Tylko warstwa hostingu może obsługiwać wyjątki, które mają miejsce podczas uruchamiania aplikacji. Za pomocą [hosta sieci Web](xref:fundamentals/host/web-host), możesz [Konfigurowanie zachowania hosta w odpowiedzi na błędy podczas uruchamiania](xref:fundamentals/host/web-host#detailed-errors) z `captureStartupErrors` i `detailedErrors` kluczy.

Jeśli wystąpi błąd, po adresem/port hosta powiązania hostingu można wyświetlić tylko stronę błędu dla błędów uruchamiania przechwycone. Jeśli wszystkie powiązania nie powiedzie się z jakiegokolwiek powodu:

* Warstwa hostingu rejestruje wyjątek krytyczny.
* Proces dotnet kończy się niepowodzeniem.
* Strona błędu, nie jest wyświetlane, gdy aplikacja jest uruchomiona na [Kestrel](xref:fundamentals/servers/kestrel) serwera.

Podczas uruchamiania na [IIS](/iis) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 - niepowodzenia procesu* jest zwracany przez [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) Jeśli nie można uruchomić procesu . Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/iis/troubleshoot>. Aby uzyskać informacje na temat rozwiązywania problemów z uruchamianiem przy użyciu usługi Azure App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="aspnet-core-mvc-error-handling"></a>Obsługa błędów programu ASP.NET Core MVC

[MVC](xref:mvc/overview) aplikacje mają pewne dodatkowe opcje obsługi błędów, takich jak konfigurowanie filtry wyjątków i sprawdzaniu poprawności modelu.

### <a name="exception-filters"></a>Filtry wyjątków

Filtry wyjątków można skonfigurować globalnie lub na zasadzie na kontroler lub akcję w aplikacji MVC. Te filtry obsługi nieobsługiwanego wyjątku, który występuje podczas wykonywania akcji kontrolera lub inny filtr. Te filtry nie są wywoływane w przeciwnym razie. Aby dowiedzieć się więcej, zobacz <xref:mvc/controllers/filters>.

> [!TIP]
> Filtry wyjątków są przydatne zalewania wyjątków występujących w ramach akcji MVC, ale nie jest tak elastyczna jak błąd obsługi oprogramowania pośredniczącego. Firma Microsoft zaleca korzystanie z oprogramowania pośredniczącego. Użyj filtrów, tylko gdy potrzebujesz, aby wykonać obsługi błędów *inaczej* oparte na akcję MVC, która jest wybierany.

### <a name="handle-model-state-errors"></a>Obsługiwać błędy stanu modelu

[Walidacja modelu](xref:mvc/models/validation) występuje przed wywołaniem akcji każdego kontrolera i odpowiada metoda akcji sprawdzić [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) i odpowiednio reagują.

Niektóre aplikacje chce wykonać standardowej konwencji radzenia sobie z [Walidacja modelu](xref:mvc/models/validation) błędów, w którym to przypadku [filtru](xref:mvc/controllers/filters) może być w odpowiednim miejscu, aby zaimplementować takie zasady. Należy sprawdzić, jak Twoje działania zachowują się ze Stanami nieprawidłowy model. Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/testing>.

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
