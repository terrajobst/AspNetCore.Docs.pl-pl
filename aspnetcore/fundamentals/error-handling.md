---
title: Obsługa błędów w programie ASP.NET Core
author: tdykstra
description: Dowiedz się, jak do obsługi błędów w aplikacji platformy ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/03/2019
uid: fundamentals/error-handling
ms.openlocfilehash: 6b92cb6b68b1c70d67f42284d548729e598f9a8b
ms.sourcegitcommit: c5339594101d30b189f61761275b7d310e80d18a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/02/2019
ms.locfileid: "66458432"
---
# <a name="handle-errors-in-aspnet-core"></a>Obsługa błędów w programie ASP.NET Core

Przez [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), i [Steve Smith](https://ardalis.com/)

W tym artykule opisano typowe metody obsługi błędów w aplikacji platformy ASP.NET Core.

[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples). ([Sposobu pobierania](xref:index#how-to-download-a-sample).) Artykuł zawiera instrukcje dotyczące sposobu ustawiania dyrektywy preprocesora (`#if`, `#endif`, `#define`) Przykładowa aplikacja do obsługi różnych scenariuszy.

## <a name="developer-exception-page"></a>Stronie wyjątków dla deweloperów

*Stronie wyjątków deweloperów* Wyświetla szczegółowe informacje o wyjątkach żądania. Strona jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app). Dodaj kod, aby `Startup.Configure` metodę, aby włączyć na stronie, gdy aplikacja jest uruchomiona w trakcie opracowywania [środowiska](xref:fundamentals/environments):

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

Umieść wywołanie <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> przed dowolnego oprogramowania pośredniczącego, które chcesz przechwytywać wyjątki.

> [!WARNING]
> Włącz na stronie wyjątków dla deweloperów **tylko wtedy, gdy aplikacja jest uruchomiona w środowisku programistycznym**. Nie chcesz publicznie udostępnić szczegółowe informacje o wyjątku, gdy aplikacja jest uruchamiana w środowisku produkcyjnym. Aby uzyskać więcej informacji na temat konfigurowania środowiska, zobacz <xref:fundamentals/environments>.

Strona zawiera następujące informacje dotyczące wyjątku i żądanie:

* Ślad stosu
* Parametry ciągu zapytania (jeśli istnieje)
* Pliki cookie (jeśli istnieje)
* Nagłówki

Aby wyświetlić stronę wyjątek dla deweloperów w [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj `DevEnvironment` preprocesora dyrektywy i wybierz pozycję **wywołanie wyjątku** na stronie głównej.

## <a name="exception-handler-page"></a>Strona obsługi wyjątków

Aby skonfigurować niestandardową obsługę błędów na stronie w środowisku produkcyjnym, należy użyć oprogramowania pośredniczącego obsługi wyjątków. Oprogramowanie pośredniczące:

* Przechwytuje i dzienników wyjątków.
* Ponownie wykonuje żądanie w potoku usługi alternatywnej strony lub kontroler wskazane. Żądanie nie jest wykonany ponownie, jeśli odpowiedź została uruchomiona.

W poniższym przykładzie <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> dodaje oprogramowanie pośredniczące obsługi wyjątków w środowiskach innych niż:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

Szablon aplikacji stron Razor zawiera stronę błędu ( *.cshtml*) i <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> klasy (`ErrorModel`) w *stron* folderu. Dla aplikacji MVC szablon projektu obejmuje metodę akcji błędu i widoku błędów. Poniżej przedstawiono metody akcji:

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

Nie dekoracji metody akcji programu obsługi błędów z atrybutami metody HTTP, takich jak `HttpGet`. Jawne zleceń uniemożliwić metody osiągając niektórych żądań. Zezwalaj na anonimowy dostęp do metody, aby były nieuwierzytelnionym użytkownikom możliwość odbierania widoku błędów.

### <a name="access-the-exception"></a>Dostęp do wyjątku

Użyj <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> dostęp do wyjątku i Oryginalna ścieżka żądania w kontrolerze obsługi błędów lub na stronie:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> Czy **nie** udostępniają informacje o błędzie poufnych klientom. Obsługuje błędy stanowi zagrożenie bezpieczeństwa.

Aby wyświetlić stronę obsługi wyjątków w [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj `ProdEnvironment` i `ErrorHandlerPage` dyrektywy preprocesora, a następnie wybierz **wywołanie wyjątku** na stronie głównej.

## <a name="exception-handler-lambda"></a>Lambda obsługi wyjątków

Alternatywa [strony programu obsługi wyjątków niestandardowych](#exception-handler-page) ma na celu dostarczenie wyrażenia lambda <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>. Używanie wyrażenia lambda zezwala na dostęp do błędu przed zwróceniem odpowiedzi.

Poniżej przedstawiono przykład użycia wyrażenia lambda do obsługi wyjątku:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> Czy **nie** obsługiwać błąd poufnych informacji z <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> lub <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> do klientów. Obsługuje błędy stanowi zagrożenie bezpieczeństwa.

Aby wyświetlić wynik wyrażenia lambda obsługi wyjątków w [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj `ProdEnvironment` i `ErrorHandlerLambda` dyrektywy preprocesora, a następnie wybierz **wywołanie wyjątku** na stronie głównej.

## <a name="usestatuscodepages"></a>UseStatusCodePages

Domyślnie aplikacji ASP.NET Core nie zapewnia stan stronę kodową dla kodów stanu HTTP, takich jak *404 — Nie można odnaleźć*. Aplikacja zwraca kod stanu i pusta treść odpowiedzi. Aby zapewnić stan stron kodowych, użyj oprogramowanie pośredniczące strony kodowe stanu.

Oprogramowanie pośredniczące jest udostępniana przez [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) pakiet, który znajduje się w [meta Microsoft.aspnetcore.all Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).

Aby włączyć domyślne tekstowy programy obsługi dla typowe kody stanu błędu, należy wywołać <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> w `Startup.Configure` metody:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

Wywołaj `UseStatusCodePages` przed żądaniem obsługi oprogramowania pośredniczącego (na przykład oprogramowanie pośredniczące plików statycznych i oprogramowanie pośredniczące MVC).

Oto przykład tekstu wyświetlanego przez programy obsługi domyślne:

```
Status Code: 404; Not Found
```

Aby znajduje się w różnych formatach kod stanu strony w [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), użyj jednej z dyrektywy preprocesora, które zaczynają się od `StatusCodePages`i wybierz **wyzwalacza a 404** na stronie głównej.

## <a name="usestatuscodepages-with-format-string"></a>UseStatusCodePages za pomocą ciągu formatu

Aby dostosować typ zawartości odpowiedzi i tekst, użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> przyjmującej zawartości typu i formatu ciągu:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a>UseStatusCodePages przy użyciu lambda

Aby określić niestandardową obsługę błędów i zapisywanie odpowiedzi kodu, użyj przeciążenia <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> przyjmującej wyrażenia lambda:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirect"></a>UseStatusCodePagesWithRedirect

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> — Metoda rozszerzenia:

* Wysyła *302 — znaleziono* kod stanu do klienta.
* Przekierowuje klienta do lokalizacji, udostępnionego w szablonie adresu URL.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

Szablon adresu URL może zawierać `{0}` symbol zastępczy dla kodu stanu, jak pokazano w przykładzie. Jeśli szablon adresu URL zaczyna się tyldą (~), za pomocą aplikacji zastępuje tylda `PathBase`. Po wskazaniu punktu końcowego w aplikacji, Utwórz widoku składnika MVC lub strona Razor dla punktu końcowego. Na przykład stron Razor, zobacz *Pages/StatusCode.cshtml* w [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).

Ta metoda jest często używana, gdy aplikacja:

* Należy przekierowuje klienta do innego punktu końcowego, zazwyczaj w przypadkach, gdzie przetwarza błąd przez inną aplikację. W przypadku aplikacji sieci web paska adresu przeglądarki klienta odzwierciedla przekierowanego punktu końcowego.
* Nie należy zachować i zwróć oryginalny kod stanu odpowiedzi przekierowania początkowej.

## <a name="usestatuscodepageswithreexecute"></a>UseStatusCodePagesWithReExecute

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> — Metoda rozszerzenia:

* Zwraca oryginalny kod stanu do klienta.
* Generuje treści odpowiedzi przy ponownym wykonaniem Potok żądań przy użyciu ścieżki alternatywnej.

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

Po wskazaniu punktu końcowego w aplikacji, Utwórz widoku składnika MVC lub strona Razor dla punktu końcowego. Na przykład stron Razor, zobacz *Pages/StatusCode.cshtml* w [przykładową aplikację](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).

Ta metoda jest często używana aplikacja powinna:

* Proces żądania bez przekierowania folderu do innego punktu końcowego. W przypadku aplikacji sieci web paska adresu przeglądarki klienta odzwierciedla pierwotnie żądanego punktu końcowego.
* Zachowanie i powrócić do oryginalnego kodu stanu z odpowiedzią.

Adres URL i zapytania szablony ciągów może zawierać symbolu zastępczego (`{0}`) dla kodu stanu. Szablon adresu URL musi rozpoczynać się od ukośnika (`/`). Korzystając z symbolem zastępczym w ścieżce, upewnij się, że punkt końcowy (strona lub kontroler) może przetwarzać segmentu ścieżki. Na przykład strona Razor dla błędów należy zaakceptować wartość segmentu ścieżki opcjonalnie za pomocą `@page` dyrektywy:

```cshtml
@page "{code?}"
```

Punkt końcowy, który przetwarza błędu można uzyskać oryginalny adres URL, który wygenerował błąd, jak pokazano w poniższym przykładzie:

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a>Wyłączanie kod stanu

Aby wyłączyć stan stron kodowych MVC kontrolera lub metody akcji, należy użyć [[SkipStatusCodePages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) atrybutu.

Aby wyłączyć stanu strony kodowe dla określonych żądań w metodzie obsługi stron Razor lub kontroler MVC, użyj <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>Kod obsługi wyjątków

Kod obsługi stron wyjątków może zgłaszać wyjątki. Często jest dobrym rozwiązaniem dla stron błędów produkcyjnych, które ma zawierać zawartość statyczną wyłącznie.

### <a name="response-headers"></a>Nagłówki odpowiedzi

Gdy nagłówki odpowiedzi są wysyłane:

* Aplikacja nie można zmienić kod stanu odpowiedzi.
* Nie można uruchomić wszystkie strony wyjątków lub programów obsługi. Odpowiedzi muszą być wypełnione albo połączenie zostało przerwane.

## <a name="server-exception-handling"></a>Obsługa wyjątków serwera

Oprócz logiki aplikacji, obsługi wyjątków [implementację serwera HTTP](xref:fundamentals/servers/index) może obsługiwać niektóre wyjątki. Jeśli serwer wyłapuje wyjątek, przed wysłaniem nagłówki odpowiedzi, serwer wysyła *500 — Wewnętrzny błąd serwera* odpowiedzi bez treści odpowiedzi. Jeśli serwer wyłapuje wyjątek po wysłaniu nagłówki odpowiedzi, serwer zamyka połączenie. Żądania, które nie są obsługiwane przez aplikację są obsługiwane przez serwer. Każdy wyjątek, który występuje, gdy ten serwer obsługuje żądania odbywa się przez wyjątek serwera obsługi. Strony błędów niestandardowych aplikacji, obsługi oprogramowania pośredniczącego i filtry wyjątków nie wpływają na to zachowanie.

## <a name="startup-exception-handling"></a>Obsługa wyjątków uruchamiania

Tylko warstwa hostingu może obsługiwać wyjątki, które mają miejsce podczas uruchamiania aplikacji. Hosta można skonfigurować w celu [przechwytywania błędów uruchamiania](xref:fundamentals/host/web-host#capture-startup-errors) i [przechwytywać szczegółowe informacje o błędach](xref:fundamentals/host/web-host#detailed-errors).

Warstwa obsługi można wyświetlić stronę błędu dla błędów uruchamiania przechwycone tylko wtedy, gdy wystąpi błąd, po adresem/port hosta powiązania. W przypadku niepowodzenia powiązania:

* Warstwa hostingu rejestruje wyjątek krytyczny.
* Proces dotnet kończy się niepowodzeniem.
* Strona błędu, nie jest wyświetlane, gdy serwer HTTP jest [Kestrel](xref:fundamentals/servers/kestrel).

Podczas uruchamiania na [IIS](/iis) lub [usług IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), *502.5 - niepowodzenia procesu* jest zwracany przez [modułu ASP.NET Core](xref:host-and-deploy/aspnet-core-module) Jeśli nie można uruchomić procesu . Aby uzyskać więcej informacji, zobacz <xref:host-and-deploy/iis/troubleshoot>. Aby uzyskać informacje na temat rozwiązywania problemów z uruchamianiem przy użyciu usługi Azure App Service, zobacz <xref:host-and-deploy/azure-apps/troubleshoot>.

## <a name="database-error-page"></a>Strona błędu bazy danych

[Stronę błędu bazy danych](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>) oprogramowania pośredniczącego przechwytuje wyjątki związane z bazy danych można rozwiązać za pomocą migracje platformy Entity Framework. Gdy występują te wyjątki, jest generowany odpowiedzi HTML szczegółowe informacje o możliwych działań w celu rozwiązania problemu. Ta strona powinna być włączona tylko w środowisku programistycznym. Włącz stronie przez dodanie kodu do `Startup.Configure`:

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

## <a name="exception-filters"></a>Filtry wyjątków

W aplikacji MVC filtry wyjątków można skonfigurować globalnie lub na zasadzie na kontroler lub akcję. W aplikacjach stron Razor można je skonfigurować globalnie lub na modelu strony. Te filtry obsługi nieobsługiwanego wyjątku, który występuje podczas wykonywania akcji kontrolera lub inny filtr. Aby uzyskać więcej informacji, zobacz <xref:mvc/controllers/filters#exception-filters>.

> [!TIP]
> Filtry wyjątków są przydatne zalewania wyjątków występujących w ramach akcji MVC, ale nie jest tak elastyczna jak oprogramowanie pośredniczące obsługi wyjątków. Zalecamy używanie oprogramowania pośredniczącego. Filtry, tylko gdy potrzebujesz należy używać do wykonywania obsługi błędów, inaczej w zależności od Akcja kontrolera MVC, który jest wybierany.

## <a name="model-state-errors"></a>Błędy stanu modelu

Aby dowiedzieć się, jak obsługiwać błędy stanu modelu, zobacz [wiązanie modelu](xref:mvc/models/model-binding) i [Walidacja modelu](xref:mvc/models/validation).

## <a name="additional-resources"></a>Dodatkowe zasoby

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
