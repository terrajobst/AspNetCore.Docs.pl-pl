---
title: Co nowego w programie ASP.NET Core 2.1
author: isaac2004
description: Dowiedz się więcej o nowych funkcjach w programie ASP.NET Core 2.1.
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: aspnetcore-2.1
ms.openlocfilehash: 8299af819f86d3d2371650ce3d87deb817f0feb8
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248176"
---
# <a name="whats-new-in-aspnet-core-21"></a><span data-ttu-id="dec63-103">Co nowego w programie ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="dec63-103">What's new in ASP.NET Core 2.1</span></span>

<span data-ttu-id="dec63-104">W tym artykule przedstawiono najbardziej znaczące zmiany w programie ASP.NET Core 2.1 wraz z łączami do odpowiedniej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="dec63-104">This article highlights the most significant changes in ASP.NET Core 2.1, with links to relevant documentation.</span></span>

## <a name="signalr"></a><span data-ttu-id="dec63-105">SignalR</span><span class="sxs-lookup"><span data-stu-id="dec63-105">SignalR</span></span>

<span data-ttu-id="dec63-106">SignalR został przepisany dla platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="dec63-106">SignalR has been rewritten for ASP.NET Core 2.1.</span></span> <span data-ttu-id="dec63-107">SignalR dla ASP.NET Core zawiera szereg ulepszeń:</span><span class="sxs-lookup"><span data-stu-id="dec63-107">ASP.NET Core SignalR includes a number of improvements:</span></span>

* <span data-ttu-id="dec63-108">Uproszczony model skalowania w poziomie.</span><span class="sxs-lookup"><span data-stu-id="dec63-108">A simplified scale-out model.</span></span>
* <span data-ttu-id="dec63-109">Nowy klient JavaScript bez zależności od jQuery.</span><span class="sxs-lookup"><span data-stu-id="dec63-109">A new JavaScript client with no jQuery dependency.</span></span>
* <span data-ttu-id="dec63-110">Nowy kompaktowy protokół binarny oparty na MessagePack.</span><span class="sxs-lookup"><span data-stu-id="dec63-110">A new compact binary protocol based on MessagePack.</span></span>
* <span data-ttu-id="dec63-111">Informacje dotyczące obsługi niestandardowych protokołów.</span><span class="sxs-lookup"><span data-stu-id="dec63-111">Support for custom protocols.</span></span>
* <span data-ttu-id="dec63-112">Nowy model odpowiedzi przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="dec63-112">A new streaming response model.</span></span>
* <span data-ttu-id="dec63-113">Obsługa klientów opartych bezpośrednio na WebSockets.</span><span class="sxs-lookup"><span data-stu-id="dec63-113">Support for clients based on bare WebSockets.</span></span>

<span data-ttu-id="dec63-114">Aby uzyskać więcej informacji, zobacz [biblioteki SignalR platformy ASP.NET Core](xref:signalr/index).</span><span class="sxs-lookup"><span data-stu-id="dec63-114">For more information, see [ASP.NET Core SignalR](xref:signalr/index).</span></span>

## <a name="razor-class-libraries"></a><span data-ttu-id="dec63-115">Biblioteki klas Razor</span><span class="sxs-lookup"><span data-stu-id="dec63-115">Razor class libraries</span></span>

<span data-ttu-id="dec63-116">ASP.NET Core 2.1 ułatwia tworzenie i dołączanie interfejsu użytkownika opartego na składni Razor w bibliotece i udostępnianie go w wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="dec63-116">ASP.NET Core 2.1 makes it easier to build and include Razor-based UI in a library and share it across multiple projects.</span></span> <span data-ttu-id="dec63-117">Nowy zestaw SDK Razor umożliwia kompilowanie plików Razor do projektów bibliotek klas, które mogą być umieszczone w pakietach NuGet.</span><span class="sxs-lookup"><span data-stu-id="dec63-117">The new Razor SDK enables building Razor files into a class library project that can be packaged into a NuGet package.</span></span> <span data-ttu-id="dec63-118">Widoki i strony w bibliotekach są automatycznie wykrywane i mogą być nadpisane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="dec63-118">Views and pages in libraries are automatically discovered and can be overridden by the app.</span></span> <span data-ttu-id="dec63-119">Dzięki integracji kompilacji Razor w kompilacji:</span><span class="sxs-lookup"><span data-stu-id="dec63-119">By integrating Razor compilation into the build:</span></span>

* <span data-ttu-id="dec63-120">Czas uruchamiania aplikacji jest znacznie krótszy.</span><span class="sxs-lookup"><span data-stu-id="dec63-120">The app startup time is significantly faster.</span></span>
* <span data-ttu-id="dec63-121">Szybkie aktualizacje widoków i stron Razor w czasie wykonywania są nadal dostępne jako część przepływu pracy iteracyjnego programowania.</span><span class="sxs-lookup"><span data-stu-id="dec63-121">Fast updates to Razor views and pages at runtime are still available as part of an iterative development workflow.</span></span>

<span data-ttu-id="dec63-122">Aby uzyskać więcej informacji, zobacz [tworzenia interfejsu użytkownika do wielokrotnego użytku, używając projektu biblioteki klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="dec63-122">For more information, see [Create reusable UI using the Razor Class Library project](xref:razor-pages/ui-class).</span></span>

## <a name="identity-ui-library--scaffolding"></a><span data-ttu-id="dec63-123">Biblioteka interfejsów użytkownika tożsamości i tworzenia szkieletów</span><span class="sxs-lookup"><span data-stu-id="dec63-123">Identity UI library & scaffolding</span></span>

<span data-ttu-id="dec63-124">Platforma ASP.NET Core 2.1 udostępnia mechanizm [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jako [bibliotekę klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="dec63-124">ASP.NET Core 2.1 provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="dec63-125">Aplikacje, które używają mechanizmu tożsamości, mogą zastosować nowy generator szkieletu tożsamości, aby wybiórczo dodawać kod źródłowy, znajdujący się w bibliotece klas Razor tożsamości (RCL).</span><span class="sxs-lookup"><span data-stu-id="dec63-125">Apps that include Identity can apply the new Identity scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="dec63-126">Przydatne może być wygenerowanie kodu źródłowego, aby można było zmodyfikować kod i zmienić zachowanie.</span><span class="sxs-lookup"><span data-stu-id="dec63-126">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="dec63-127">Na przykład można nakazać Generatorowi szkieletu wygenerowanie kodu używanego podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="dec63-127">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="dec63-128">Wygenerowany kod ma pierwszeństwo przed tym samym kodem w bibliotece RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="dec63-128">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="dec63-129">Aplikacje, które **nie** obejmują uwierzytelniania, mogą zastosować Generator szkieletu tożsamości, aby dodać pakiet RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="dec63-129">Apps that do **not** include authentication can apply the Identity scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="dec63-130">Masz możliwość wyboru kodu tożsamości do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="dec63-130">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="dec63-131">Aby uzyskać więcej informacji, zobacz [szkieletu tożsamość w projektach programu ASP.NET Core](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="dec63-131">For more information, see [Scaffold Identity in ASP.NET Core projects](xref:security/authentication/scaffold-identity).</span></span>

## <a name="https"></a><span data-ttu-id="dec63-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="dec63-132">HTTPS</span></span>

<span data-ttu-id="dec63-133">Aby lepiej skoncentrować się na zabezpieczeniach i prywatności, ważne jest włączenie protokołu HTTPS dla aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dec63-133">With the increased focus on security and privacy, enabling HTTPS for web apps is important.</span></span> <span data-ttu-id="dec63-134">Wymuszanie protokołu HTTPS staje się coraz bardziej rygorystyczne w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dec63-134">HTTPS enforcement is becoming increasingly strict on the web.</span></span> <span data-ttu-id="dec63-135">Lokacje, które nie używają protokołu HTTPS, są uważane za niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="dec63-135">Sites that don’t use HTTPS are considered insecure.</span></span> <span data-ttu-id="dec63-136">Przeglądarki (Chromium, Mozilla) coraz częściej wymuszają, aby funkcje sieci Web były używane w bezpiecznym kontekście.</span><span class="sxs-lookup"><span data-stu-id="dec63-136">Browsers (Chromium, Mozilla) are starting to enforce that web features must be used from a secure context.</span></span> <span data-ttu-id="dec63-137">[RODO](xref:security/gdpr) wymaga użycia protokołu HTTPS, aby chronić prywatność użytkowników.</span><span class="sxs-lookup"><span data-stu-id="dec63-137">[GDPR](xref:security/gdpr) requires the use of HTTPS to protect user privacy.</span></span> <span data-ttu-id="dec63-138">Użycie protokołu HTTPS w środowisku produkcyjnym jest krytyczne, natomiast w środowisku programistycznym może zapobiec problemom we wdrożeniu (takim jak niezabezpieczone linki).</span><span class="sxs-lookup"><span data-stu-id="dec63-138">While using HTTPS in production is critical, using HTTPS in development can help prevent issues in deployment (for example, insecure links).</span></span> <span data-ttu-id="dec63-139">Platforma ASP.NET Core 2.1 zawiera szereg ulepszeń, które ułatwiają wykorzystanie protokołu HTTPS podczas programowania i konfigurowanie protokołu HTTPS w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="dec63-139">ASP.NET Core 2.1 includes a number of improvements that make it easier to use HTTPS in development and to configure HTTPS in production.</span></span> <span data-ttu-id="dec63-140">Aby uzyskać więcej informacji, zobacz [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="dec63-140">For more information, see [Enforce HTTPS](xref:security/enforcing-ssl).</span></span>

### <a name="on-by-default"></a><span data-ttu-id="dec63-141">Włączony domyślnie</span><span class="sxs-lookup"><span data-stu-id="dec63-141">On by default</span></span>

<span data-ttu-id="dec63-142">W celu ułatwienia tworzenia bezpiecznej witryny sieci Web protokół HTTPS jest teraz włączony domyślnie.</span><span class="sxs-lookup"><span data-stu-id="dec63-142">To facilitate secure website development, HTTPS  is now enabled by default.</span></span> <span data-ttu-id="dec63-143">Począwszy od wersji 2.1 serwer Kestrel nasłuchuje na porcie `https://localhost:5001`, kiedy lokalny certyfikat programistyczny jest obecny.</span><span class="sxs-lookup"><span data-stu-id="dec63-143">Starting in 2.1, Kestrel listens on `https://localhost:5001` when a local development certificate is present.</span></span> <span data-ttu-id="dec63-144">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="dec63-144">A development certificate is created:</span></span>

* <span data-ttu-id="dec63-145">Jako część zestawu .NET Core SDK pierwszego uruchomienia komputera, gdy używasz zestawu SDK po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="dec63-145">As part of the .NET Core SDK first-run experience, when you use the SDK for the first time.</span></span>
* <span data-ttu-id="dec63-146">Ręcznie przy użyciu nowego narzędzia `dev-certs`.</span><span class="sxs-lookup"><span data-stu-id="dec63-146">Manually using the new `dev-certs` tool.</span></span>

<span data-ttu-id="dec63-147">Uruchom polecenie `dotnet dev-certs https --trust`, aby zaufać certyfikatowi.</span><span class="sxs-lookup"><span data-stu-id="dec63-147">Run `dotnet dev-certs https --trust` to trust the certificate.</span></span>

### <a name="https-redirection-and-enforcement"></a><span data-ttu-id="dec63-148">Przekierowania i wymuszenia protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="dec63-148">HTTPS redirection and enforcement</span></span>

<span data-ttu-id="dec63-149">Aplikacje internetowe zazwyczaj wymagają nasłuchiwania protokołów HTTP i HTTPS, ale następnie przekierowują cały ruchu HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dec63-149">Web apps typically need to listen on both HTTP and HTTPS, but then redirect all HTTP traffic to HTTPS.</span></span> <span data-ttu-id="dec63-150">W wersji 2.1 zostało wprowadzone wyspecjalizowane oprogramowanie pośredniczące przekierowania protokołu HTTPS inteligentnie przekierowujące na podstawie obecności konfiguracji lub powiązanych portów serwera.</span><span class="sxs-lookup"><span data-stu-id="dec63-150">In 2.1, specialized HTTPS redirection middleware that intelligently redirects based on the presence of configuration or bound server ports has been introduced.</span></span>

<span data-ttu-id="dec63-151">Korzystanie z protokołu HTTPS może być dodatkowo wymuszane za pomocą [protokołu HTTP Strict Transport Security (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="dec63-151">Use of HTTPS can be further enforced using [HTTP Strict Transport Security Protocol (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="dec63-152">HSTS powoduje, że przeglądarka zawsze uzyskuje dostęp do witryny za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dec63-152">HSTS instructs browsers to always access the site via HTTPS.</span></span> <span data-ttu-id="dec63-153">Platforma ASP.NET Core 2.1 dodaje oprogramowanie pośredniczące HSTS, które obsługuje opcje dla maksymalnego wieku, poddomen i listy wstępnego ładowania HSTS.</span><span class="sxs-lookup"><span data-stu-id="dec63-153">ASP.NET Core 2.1 adds HSTS middleware that supports options for max age, subdomains, and the HSTS preload list.</span></span>

### <a name="configuration-for-production"></a><span data-ttu-id="dec63-154">Konfiguracja dla trybu produkcyjnego</span><span class="sxs-lookup"><span data-stu-id="dec63-154">Configuration for production</span></span>

<span data-ttu-id="dec63-155">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="dec63-155">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="dec63-156">W wersji 2.1 został dodany domyślny schemat konfiguracji dotyczący konfigurowania protokołu HTTPS dla serwera Kestrel.</span><span class="sxs-lookup"><span data-stu-id="dec63-156">In 2.1, default configuration schema for configuring HTTPS for Kestrel has been added.</span></span> <span data-ttu-id="dec63-157">Aplikacje można skonfigurować do użycia:</span><span class="sxs-lookup"><span data-stu-id="dec63-157">Apps can be configured to use:</span></span>

* <span data-ttu-id="dec63-158">Wielu punktów końcowych, w tym adresów URL.</span><span class="sxs-lookup"><span data-stu-id="dec63-158">Multiple endpoints including the URLs.</span></span> <span data-ttu-id="dec63-159">Aby uzyskać więcej informacji, zobacz [implementacji serwera sieci web Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="dec63-159">For more information, see [Kestrel web server implementation: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
* <span data-ttu-id="dec63-160">Certyfikatu używanego do obsługi protokołu HTTPS z pliku na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="dec63-160">The certificate to use for HTTPS either from a file on disk or from a certificate store.</span></span>

## <a name="gdpr"></a><span data-ttu-id="dec63-161">GDPR</span><span class="sxs-lookup"><span data-stu-id="dec63-161">GDPR</span></span>

<span data-ttu-id="dec63-162">Platforma ASP.NET Core udostępnia interfejsy API i szablony, które ułatwiają spełnienie niektórych wymagań [Ogólnego rozporządzenia o ochronie danych (RODO)](https://www.eugdpr.org/).</span><span class="sxs-lookup"><span data-stu-id="dec63-162">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements.</span></span> <span data-ttu-id="dec63-163">Aby uzyskać więcej informacji, zobacz [Obsługa RODO w programie ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="dec63-163">For more information, see [GDPR support in ASP.NET Core](xref:security/gdpr).</span></span> <span data-ttu-id="dec63-164">W [przykładowej aplikacji](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) pokazano sposób użycia umożliwiono przetestowanie większości punktów rozszerzenia i interfejsów API związanych z RODO, które dodano w szablonach ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="dec63-164">A [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) shows how to use and lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span>

## <a name="integration-tests"></a><span data-ttu-id="dec63-165">Testy integracyjne</span><span class="sxs-lookup"><span data-stu-id="dec63-165">Integration tests</span></span>

<span data-ttu-id="dec63-166">Został wprowadzony nowy pakiet upraszczający tworzenie i wykonywanie testów.</span><span class="sxs-lookup"><span data-stu-id="dec63-166">A new package is introduced that streamlines test creation and execution.</span></span> <span data-ttu-id="dec63-167">Pakiet [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) obsługuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="dec63-167">The [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) package handles the following tasks:</span></span>

* <span data-ttu-id="dec63-168">Kopiuje plik zależności (*\*.deps*) z przetestowanej aplikacji w projekcie testowym *bin* folderu.</span><span class="sxs-lookup"><span data-stu-id="dec63-168">Copies the dependency file (*\*.deps*) from the tested app into the test project's *bin* folder.</span></span>
* <span data-ttu-id="dec63-169">Ustawia katalog główny zawartości na katalog główny projektu testowanej aplikacji, aby można było znaleźć pliki statyczne i strony/widoki podczas wykonywania testów.</span><span class="sxs-lookup"><span data-stu-id="dec63-169">Sets the content root to the tested app's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="dec63-170">Udostępnia klasę [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1), aby usprawnić uruchamianie testowanej aplikacji za pomocą [elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span><span class="sxs-lookup"><span data-stu-id="dec63-170">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the tested app with [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span>

<span data-ttu-id="dec63-171">Następujący test używa narzędzia [xUnit](https://xunit.github.io/) do sprawdzenia, czy strona indeksu ładuje się z pomyślnym kodem statusu i poprawnym nagłówkiem Content-Type:</span><span class="sxs-lookup"><span data-stu-id="dec63-171">The following test uses [xUnit](https://xunit.github.io/) to check that the Index page loads with a success status code and with the correct Content-Type header:</span></span>

```csharp
public class BasicTests
    : IClassFixture<WebApplicationFactory<RazorPagesProject.Startup>>
{
    private readonly HttpClient _client;

    public BasicTests(WebApplicationFactory<RazorPagesProject.Startup> factory)
    {
        _client = factory.CreateClient();
    }

    [Fact]
    public async Task GetHomePage()
    {
        // Act
        var response = await _client.GetAsync("/");

        // Assert
        response.EnsureSuccessStatusCode(); // Status Code 200-299
        Assert.Equal("text/html; charset=utf-8",
            response.Content.Headers.ContentType.ToString());
    }
}
```

<span data-ttu-id="dec63-172">Aby uzyskać więcej informacji, zobacz [testy integracji](xref:test/integration-tests) tematu.</span><span class="sxs-lookup"><span data-stu-id="dec63-172">For more information, see the [Integration tests](xref:test/integration-tests) topic.</span></span>

## <a name="apicontroller-actionresultt"></a><span data-ttu-id="dec63-173">[ApiController], ActionResult\<T ></span><span class="sxs-lookup"><span data-stu-id="dec63-173">[ApiController], ActionResult\<T></span></span>

<span data-ttu-id="dec63-174">Platforma ASP.NET Core 2.1 dodaje nowe konwencje programowania, które ułatwiają tworzenie czystych i bardziej opisowych interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dec63-174">ASP.NET Core 2.1 adds new programming conventions that make it easier to build clean and descriptive web APIs.</span></span> <span data-ttu-id="dec63-175">Dodano nowy typ `ActionResult<T>`, aby zwracać albo typ odpowiedzi albo inny wynik akcji (podobnie jak w przypadku IActionResult), ale nadal wskazywać typ odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="dec63-175">`ActionResult<T>` is a new type added to allow an app to return either a response type or any other action result (similar to IActionResult), while still indicating the response type.</span></span> <span data-ttu-id="dec63-176">Atrybut `[ApiController]` został również dodany jako sposób korzystania z konwencji i zachowań specyficznych dla interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="dec63-176">The `[ApiController]` attribute has also been added as the way to opt in to Web API-specific conventions and behaviors.</span></span>

<span data-ttu-id="dec63-177">Aby uzyskać więcej informacji, zobacz [Tworzenie interfejsów API sieci Web za pomocą programu ASP.NET Core](xref:web-api/index).</span><span class="sxs-lookup"><span data-stu-id="dec63-177">For more information, see [Build Web APIs with ASP.NET Core](xref:web-api/index).</span></span>

## <a name="ihttpclientfactory"></a><span data-ttu-id="dec63-178">IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="dec63-178">IHttpClientFactory</span></span>

<span data-ttu-id="dec63-179">Platforma ASP.NET Core 2.1 zawiera nową usługę `IHttpClientFactory`, która ułatwia konfigurowanie i używanie wystąpień `HttpClient` w aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="dec63-179">ASP.NET Core 2.1 includes a new `IHttpClientFactory` service that makes it easier to configure and consume instances of `HttpClient` in apps.</span></span> <span data-ttu-id="dec63-180">Klasa `HttpClient` ma już pojęcie delegowania programów obsługi, które można połączyć ze sobą dla wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="dec63-180">`HttpClient` already has the concept of delegating handlers that could be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="dec63-181">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="dec63-181">The factory:</span></span>

* <span data-ttu-id="dec63-182">Sprawia, że rejestrowanie wystąpień `HttpClient` na klienta o nazwie bardziej intuicyjne.</span><span class="sxs-lookup"><span data-stu-id="dec63-182">Makes registering of instances of `HttpClient` per named client more intuitive.</span></span>
* <span data-ttu-id="dec63-183">Implementuje obsługi Polly, umożliwiająca Polly zasady służący do ponawiania, CircuitBreakers itp.</span><span class="sxs-lookup"><span data-stu-id="dec63-183">Implements a Polly handler that allows Polly policies to be used for Retry, CircuitBreakers, etc.</span></span>

<span data-ttu-id="dec63-184">Aby uzyskać więcej informacji, zobacz [inicjowanie żądań HTTP](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="dec63-184">For more information, see [Initiate HTTP Requests](xref:fundamentals/http-requests).</span></span>

## <a name="kestrel-transport-configuration"></a><span data-ttu-id="dec63-185">Konfiguracja transportu Kestrel</span><span class="sxs-lookup"><span data-stu-id="dec63-185">Kestrel transport configuration</span></span>

<span data-ttu-id="dec63-186">W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach.</span><span class="sxs-lookup"><span data-stu-id="dec63-186">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="dec63-187">Aby uzyskać więcej informacji, zobacz [implementacji serwera sieci web Kestrel: Konfiguracja transportu](xref:fundamentals/servers/kestrel#transport-configuration).</span><span class="sxs-lookup"><span data-stu-id="dec63-187">For more information, see [Kestrel web server implementation: Transport configuration](xref:fundamentals/servers/kestrel#transport-configuration).</span></span>

## <a name="generic-host-builder"></a><span data-ttu-id="dec63-188">Ogólny konstruktor hosta</span><span class="sxs-lookup"><span data-stu-id="dec63-188">Generic host builder</span></span>

<span data-ttu-id="dec63-189">Został wprowadzony ogólny konstruktor hosta (`HostBuilder`).</span><span class="sxs-lookup"><span data-stu-id="dec63-189">The Generic Host Builder (`HostBuilder`) has been introduced.</span></span> <span data-ttu-id="dec63-190">Ten konstruktor może służyć do aplikacji, które nie przetwarzają żądań HTTP (Obsługa komunikatów, zadania w tle itp.).</span><span class="sxs-lookup"><span data-stu-id="dec63-190">This builder can be used for apps that don't process HTTP requests (Messaging, background tasks, etc.).</span></span>

<span data-ttu-id="dec63-191">Aby uzyskać więcej informacji, zobacz [Host ogólny .NET](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="dec63-191">For more information, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

## <a name="updated-spa-templates"></a><span data-ttu-id="dec63-192">Zaktualizowane szablony SPA</span><span class="sxs-lookup"><span data-stu-id="dec63-192">Updated SPA templates</span></span>

<span data-ttu-id="dec63-193">Szablony aplikacji jednostronicowej dla platform Angular, React i React z Redux zostały zaktualizowane, aby używać standardowych struktur projektów i systemów budowania dla każdej z platform.</span><span class="sxs-lookup"><span data-stu-id="dec63-193">The Single Page Application templates for Angular, React, and React with Redux are updated to use the standard project structures and build systems for each framework.</span></span>

<span data-ttu-id="dec63-194">Szablon platformy Angular jest oparty na interfejsie wiersza polecenia Angular, a szablony platformy React — na narzędziu`create-react-app`.</span><span class="sxs-lookup"><span data-stu-id="dec63-194">The Angular template is based on the Angular CLI, and the React templates are based on create-react-app.</span></span>

<span data-ttu-id="dec63-195">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="dec63-195">For more information, see:</span></span>

* <xref:spa/angular>
* <xref:spa/react>
* <xref:spa/react-with-redux>

## <a name="razor-pages-search-for-razor-assets"></a><span data-ttu-id="dec63-196">Wyszukiwanie zasobów Razor w stronach Razor</span><span class="sxs-lookup"><span data-stu-id="dec63-196">Razor Pages search for Razor assets</span></span>

<span data-ttu-id="dec63-197">W wersji 2.1 w programie Razor Pages wyszukiwanie zasobów Razor (na przykład układów i widoków częściowych) odbywa się w następujących katalogach w podanej kolejności:</span><span class="sxs-lookup"><span data-stu-id="dec63-197">In 2.1, Razor Pages search for Razor assets (such as layouts and partials) in the following directories in the listed order:</span></span>

1. <span data-ttu-id="dec63-198">Bieżący folder stron.</span><span class="sxs-lookup"><span data-stu-id="dec63-198">Current Pages folder.</span></span>
1. <span data-ttu-id="dec63-199">*/Pages/Shared/*</span><span class="sxs-lookup"><span data-stu-id="dec63-199">*/Pages/Shared/*</span></span>
1. <span data-ttu-id="dec63-200">*/Views/Shared /*</span><span class="sxs-lookup"><span data-stu-id="dec63-200">*/Views/Shared/*</span></span>

## <a name="razor-pages-in-an-area"></a><span data-ttu-id="dec63-201">Strony razor, w obszarze</span><span class="sxs-lookup"><span data-stu-id="dec63-201">Razor Pages in an area</span></span>

<span data-ttu-id="dec63-202">Narzędzie Razor Pages obsługuje teraz [obszary](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="dec63-202">Razor Pages now support [areas](xref:mvc/controllers/areas).</span></span> <span data-ttu-id="dec63-203">Aby zobaczyć przykład wykorzystania obszarów, należy utworzyć nową aplikację internetową Razor Pages z indywidualnymi kontami użytkowników.</span><span class="sxs-lookup"><span data-stu-id="dec63-203">To see an example of areas, create a new Razor Pages web app with individual user accounts.</span></span> <span data-ttu-id="dec63-204">Aplikacja internetowa Razor Pages z indywidualnymi kontami użytkowników zawiera element */Areas/Identity/Pages*.</span><span class="sxs-lookup"><span data-stu-id="dec63-204">A Razor Pages web app with individual user accounts includes */Areas/Identity/Pages*.</span></span>

## <a name="mvc-compatibility-version"></a><span data-ttu-id="dec63-205">Wersja zgodności MVC</span><span class="sxs-lookup"><span data-stu-id="dec63-205">MVC compatibility version</span></span>

<span data-ttu-id="dec63-206">Metoda <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> umożliwia aplikacji włączenie wykorzystania lub rezygnację ze zmian zachowania wprowadzanych w programie ASP.NET Core MVC w wersji 2.1 lub nowszej, które potencjalnie mogą prowadzić do awarii.</span><span class="sxs-lookup"><span data-stu-id="dec63-206">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span>

<span data-ttu-id="dec63-207">Aby uzyskać więcej informacji, zobacz <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="dec63-207">For more information, see <xref:mvc/compatibility-version>.</span></span>

## <a name="migrate-from-20-to-21"></a><span data-ttu-id="dec63-208">Migracja z wersji 2.0 do wersji 2.1</span><span class="sxs-lookup"><span data-stu-id="dec63-208">Migrate from 2.0 to 2.1</span></span>

<span data-ttu-id="dec63-209">Zobacz [migracji z programu ASP.NET Core 2.0 i 2.1](xref:migration/20_21).</span><span class="sxs-lookup"><span data-stu-id="dec63-209">See [Migrate from ASP.NET Core 2.0 to 2.1](xref:migration/20_21).</span></span>

## <a name="additional-information"></a><span data-ttu-id="dec63-210">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="dec63-210">Additional information</span></span>

<span data-ttu-id="dec63-211">Aby uzyskać pełną listę zmian, zobacz [platformy ASP.NET Core 2.1 — informacje o wersji](https://github.com/aspnet/Home/releases/tag/2.1.0).</span><span class="sxs-lookup"><span data-stu-id="dec63-211">For the complete list of changes, see the [ASP.NET Core 2.1 Release Notes](https://github.com/aspnet/Home/releases/tag/2.1.0).</span></span>
