---
title: Co nowego w programie ASP.NET Core 2.1
author: isaac2004
description: Dowiedz się więcej o nowych funkcjach w ASP.NET Core 2,1.
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- SignalR
uid: aspnetcore-2.1
ms.openlocfilehash: af5807b782d4acec8c7d40111dc508dfa6127057
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667546"
---
# <a name="whats-new-in-aspnet-core-21"></a><span data-ttu-id="83ad9-103">Co nowego w programie ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="83ad9-103">What's new in ASP.NET Core 2.1</span></span>

<span data-ttu-id="83ad9-104">W tym artykule przedstawiono najbardziej znaczące zmiany w programie ASP.NET Core 2.1 wraz z łączami do odpowiedniej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="83ad9-104">This article highlights the most significant changes in ASP.NET Core 2.1, with links to relevant documentation.</span></span>

## SignalR

SignalR<span data-ttu-id="83ad9-105"> został ponownie zapisany dla ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="83ad9-105"> has been rewritten for ASP.NET Core 2.1.</span></span> <span data-ttu-id="83ad9-106">ASP.NET Core SignalR obejmuje kilka ulepszeń:</span><span class="sxs-lookup"><span data-stu-id="83ad9-106">ASP.NET Core SignalR includes a number of improvements:</span></span>

* <span data-ttu-id="83ad9-107">Uproszczony model skalowania w poziomie.</span><span class="sxs-lookup"><span data-stu-id="83ad9-107">A simplified scale-out model.</span></span>
* <span data-ttu-id="83ad9-108">Nowy klient JavaScript bez zależności od jQuery.</span><span class="sxs-lookup"><span data-stu-id="83ad9-108">A new JavaScript client with no jQuery dependency.</span></span>
* <span data-ttu-id="83ad9-109">Nowy kompaktowy protokół binarny oparty na MessagePack.</span><span class="sxs-lookup"><span data-stu-id="83ad9-109">A new compact binary protocol based on MessagePack.</span></span>
* <span data-ttu-id="83ad9-110">Obsługa niestandardowych protokołów.</span><span class="sxs-lookup"><span data-stu-id="83ad9-110">Support for custom protocols.</span></span>
* <span data-ttu-id="83ad9-111">Nowy model odpowiedzi przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="83ad9-111">A new streaming response model.</span></span>
* <span data-ttu-id="83ad9-112">Obsługa klientów opartych bezpośrednio na WebSockets.</span><span class="sxs-lookup"><span data-stu-id="83ad9-112">Support for clients based on bare WebSockets.</span></span>

<span data-ttu-id="83ad9-113">Aby uzyskać więcej informacji, zobacz [ASP.NET Core SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="83ad9-113">For more information, see [ASP.NET Core SignalR](xref:signalr/introduction).</span></span>

## <a name="razor-class-libraries"></a><span data-ttu-id="83ad9-114">Biblioteki klas Razor</span><span class="sxs-lookup"><span data-stu-id="83ad9-114">Razor class libraries</span></span>

<span data-ttu-id="83ad9-115">ASP.NET Core 2.1 ułatwia tworzenie i dołączanie interfejsu użytkownika opartego na składni Razor w bibliotece i udostępnianie go w wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="83ad9-115">ASP.NET Core 2.1 makes it easier to build and include Razor-based UI in a library and share it across multiple projects.</span></span> <span data-ttu-id="83ad9-116">Nowy zestaw SDK Razor umożliwia kompilowanie plików Razor do projektów bibliotek klas, które mogą być umieszczone w pakietach NuGet.</span><span class="sxs-lookup"><span data-stu-id="83ad9-116">The new Razor SDK enables building Razor files into a class library project that can be packaged into a NuGet package.</span></span> <span data-ttu-id="83ad9-117">Widoki i strony w bibliotekach są automatycznie wykrywane i mogą być nadpisane w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="83ad9-117">Views and pages in libraries are automatically discovered and can be overridden by the app.</span></span> <span data-ttu-id="83ad9-118">Dzięki integracji kompilacji Razor w kompilacji:</span><span class="sxs-lookup"><span data-stu-id="83ad9-118">By integrating Razor compilation into the build:</span></span>

* <span data-ttu-id="83ad9-119">Czas uruchamiania aplikacji jest znacznie krótszy.</span><span class="sxs-lookup"><span data-stu-id="83ad9-119">The app startup time is significantly faster.</span></span>
* <span data-ttu-id="83ad9-120">Szybkie aktualizacje widoków i stron Razor w czasie wykonywania są nadal dostępne jako część przepływu pracy iteracyjnego programowania.</span><span class="sxs-lookup"><span data-stu-id="83ad9-120">Fast updates to Razor views and pages at runtime are still available as part of an iterative development workflow.</span></span>

<span data-ttu-id="83ad9-121">Aby uzyskać więcej informacji, zobacz [Tworzenie interfejsu użytkownika wielokrotnego użytku przy użyciu projektu biblioteki klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="83ad9-121">For more information, see [Create reusable UI using the Razor Class Library project](xref:razor-pages/ui-class).</span></span>

## <a name="identity-ui-library--scaffolding"></a><span data-ttu-id="83ad9-122">Tworzenie szkieletu biblioteki interfejsu użytkownika tożsamości &</span><span class="sxs-lookup"><span data-stu-id="83ad9-122">Identity UI library & scaffolding</span></span>

<span data-ttu-id="83ad9-123">ASP.NET Core 2,1 zapewnia [ASP.NET Core tożsamość](xref:security/authentication/identity) jako [Biblioteka klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="83ad9-123">ASP.NET Core 2.1 provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="83ad9-124">Aplikacje, które używają mechanizmu tożsamości, mogą zastosować nowy generator szkieletu tożsamości, aby wybiórczo dodawać kod źródłowy, znajdujący się w bibliotece klas Razor tożsamości (RCL).</span><span class="sxs-lookup"><span data-stu-id="83ad9-124">Apps that include Identity can apply the new Identity scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="83ad9-125">Przydatne może być wygenerowanie kodu źródłowego, aby można było zmodyfikować kod i zmienić zachowanie.</span><span class="sxs-lookup"><span data-stu-id="83ad9-125">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="83ad9-126">Na przykład można nakazać Generatorowi szkieletu wygenerowanie kodu używanego podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="83ad9-126">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="83ad9-127">Wygenerowany kod ma pierwszeństwo przed tym samym kodem w bibliotece RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="83ad9-127">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="83ad9-128">Aplikacje, które **nie** obejmują uwierzytelniania, mogą zastosować szkielet tożsamości w celu dodania pakietu tożsamości RCL.</span><span class="sxs-lookup"><span data-stu-id="83ad9-128">Apps that do **not** include authentication can apply the Identity scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="83ad9-129">Masz możliwość wyboru kodu tożsamości do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="83ad9-129">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="83ad9-130">Aby uzyskać więcej informacji, zobacz [tożsamość szkieletu w projektach ASP.NET Core](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="83ad9-130">For more information, see [Scaffold Identity in ASP.NET Core projects](xref:security/authentication/scaffold-identity).</span></span>

## <a name="https"></a><span data-ttu-id="83ad9-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="83ad9-131">HTTPS</span></span>

<span data-ttu-id="83ad9-132">Aby lepiej skoncentrować się na zabezpieczeniach i prywatności, ważne jest włączenie protokołu HTTPS dla aplikacji sieci Web.</span><span class="sxs-lookup"><span data-stu-id="83ad9-132">With the increased focus on security and privacy, enabling HTTPS for web apps is important.</span></span> <span data-ttu-id="83ad9-133">Wymuszanie protokołu HTTPS staje się coraz bardziej rygorystyczne w sieci Web.</span><span class="sxs-lookup"><span data-stu-id="83ad9-133">HTTPS enforcement is becoming increasingly strict on the web.</span></span> <span data-ttu-id="83ad9-134">Lokacje, które nie używają protokołu HTTPS, są uznawane za niezabezpieczone.</span><span class="sxs-lookup"><span data-stu-id="83ad9-134">Sites that don't use HTTPS are considered insecure.</span></span> <span data-ttu-id="83ad9-135">Przeglądarki (Chromium, Mozilla) coraz częściej wymuszają, aby funkcje sieci Web były używane w bezpiecznym kontekście.</span><span class="sxs-lookup"><span data-stu-id="83ad9-135">Browsers (Chromium, Mozilla) are starting to enforce that web features must be used from a secure context.</span></span> <span data-ttu-id="83ad9-136">[Rodo](xref:security/gdpr) wymaga użycia protokołu HTTPS do ochrony prywatności użytkowników.</span><span class="sxs-lookup"><span data-stu-id="83ad9-136">[GDPR](xref:security/gdpr) requires the use of HTTPS to protect user privacy.</span></span> <span data-ttu-id="83ad9-137">Użycie protokołu HTTPS w środowisku produkcyjnym jest krytyczne, natomiast w środowisku programistycznym może zapobiec problemom we wdrożeniu (takim jak niezabezpieczone linki).</span><span class="sxs-lookup"><span data-stu-id="83ad9-137">While using HTTPS in production is critical, using HTTPS in development can help prevent issues in deployment (for example, insecure links).</span></span> <span data-ttu-id="83ad9-138">Platforma ASP.NET Core 2.1 zawiera szereg ulepszeń, które ułatwiają wykorzystanie protokołu HTTPS podczas programowania i konfigurowanie protokołu HTTPS w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="83ad9-138">ASP.NET Core 2.1 includes a number of improvements that make it easier to use HTTPS in development and to configure HTTPS in production.</span></span> <span data-ttu-id="83ad9-139">Aby uzyskać więcej informacji, zobacz [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="83ad9-139">For more information, see [Enforce HTTPS](xref:security/enforcing-ssl).</span></span>

### <a name="on-by-default"></a><span data-ttu-id="83ad9-140">Włączony domyślnie</span><span class="sxs-lookup"><span data-stu-id="83ad9-140">On by default</span></span>

<span data-ttu-id="83ad9-141">W celu ułatwienia tworzenia bezpiecznej witryny sieci Web protokół HTTPS jest teraz włączony domyślnie.</span><span class="sxs-lookup"><span data-stu-id="83ad9-141">To facilitate secure website development, HTTPS  is now enabled by default.</span></span> <span data-ttu-id="83ad9-142">Począwszy od 2,1, Kestrel nasłuchuje na `https://localhost:5001`, gdy obecny jest lokalny certyfikat programistyczny.</span><span class="sxs-lookup"><span data-stu-id="83ad9-142">Starting in 2.1, Kestrel listens on `https://localhost:5001` when a local development certificate is present.</span></span> <span data-ttu-id="83ad9-143">Certyfikat programistyczny jest tworzony:</span><span class="sxs-lookup"><span data-stu-id="83ad9-143">A development certificate is created:</span></span>

* <span data-ttu-id="83ad9-144">W ramach pierwszego uruchomienia zestaw .NET Core SDK, gdy zestaw SDK jest używany po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="83ad9-144">As part of the .NET Core SDK first-run experience, when you use the SDK for the first time.</span></span>
* <span data-ttu-id="83ad9-145">Ręczne korzystanie z nowego narzędzia `dev-certs`.</span><span class="sxs-lookup"><span data-stu-id="83ad9-145">Manually using the new `dev-certs` tool.</span></span>

<span data-ttu-id="83ad9-146">Uruchom `dotnet dev-certs https --trust`, aby zaufać certyfikatowi.</span><span class="sxs-lookup"><span data-stu-id="83ad9-146">Run `dotnet dev-certs https --trust` to trust the certificate.</span></span>

### <a name="https-redirection-and-enforcement"></a><span data-ttu-id="83ad9-147">Przekierowania i wymuszenia protokołu HTTPS</span><span class="sxs-lookup"><span data-stu-id="83ad9-147">HTTPS redirection and enforcement</span></span>

<span data-ttu-id="83ad9-148">Aplikacje internetowe zazwyczaj wymagają nasłuchiwania protokołów HTTP i HTTPS, ale następnie przekierowują cały ruchu HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83ad9-148">Web apps typically need to listen on both HTTP and HTTPS, but then redirect all HTTP traffic to HTTPS.</span></span> <span data-ttu-id="83ad9-149">W wersji 2.1 zostało wprowadzone wyspecjalizowane oprogramowanie pośredniczące przekierowania protokołu HTTPS inteligentnie przekierowujące na podstawie obecności konfiguracji lub powiązanych portów serwera.</span><span class="sxs-lookup"><span data-stu-id="83ad9-149">In 2.1, specialized HTTPS redirection middleware that intelligently redirects based on the presence of configuration or bound server ports has been introduced.</span></span>

<span data-ttu-id="83ad9-150">Korzystanie z protokołu HTTPS może być realizowane przy użyciu [protokołu HTTP Strict Transport Security Protocol (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="83ad9-150">Use of HTTPS can be further enforced using [HTTP Strict Transport Security Protocol (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="83ad9-151">HSTS powoduje, że przeglądarka zawsze uzyskuje dostęp do witryny za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83ad9-151">HSTS instructs browsers to always access the site via HTTPS.</span></span> <span data-ttu-id="83ad9-152">Platforma ASP.NET Core 2.1 dodaje oprogramowanie pośredniczące HSTS, które obsługuje opcje dla maksymalnego wieku, poddomen i listy wstępnego ładowania HSTS.</span><span class="sxs-lookup"><span data-stu-id="83ad9-152">ASP.NET Core 2.1 adds HSTS middleware that supports options for max age, subdomains, and the HSTS preload list.</span></span>

### <a name="configuration-for-production"></a><span data-ttu-id="83ad9-153">Konfiguracja dla środowiska produkcyjnego</span><span class="sxs-lookup"><span data-stu-id="83ad9-153">Configuration for production</span></span>

<span data-ttu-id="83ad9-154">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="83ad9-154">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="83ad9-155">W wersji 2.1 został dodany domyślny schemat konfiguracji dotyczący konfigurowania protokołu HTTPS dla serwera Kestrel.</span><span class="sxs-lookup"><span data-stu-id="83ad9-155">In 2.1, default configuration schema for configuring HTTPS for Kestrel has been added.</span></span> <span data-ttu-id="83ad9-156">Aplikacje można skonfigurować do korzystania z:</span><span class="sxs-lookup"><span data-stu-id="83ad9-156">Apps can be configured to use:</span></span>

* <span data-ttu-id="83ad9-157">Wielu punktów końcowych, w tym adresów URL.</span><span class="sxs-lookup"><span data-stu-id="83ad9-157">Multiple endpoints including the URLs.</span></span> <span data-ttu-id="83ad9-158">Aby uzyskać więcej informacji, zobacz [Implementacja serwera sieci Web Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="83ad9-158">For more information, see [Kestrel web server implementation: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
* <span data-ttu-id="83ad9-159">Certyfikatu używanego do obsługi protokołu HTTPS z pliku na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="83ad9-159">The certificate to use for HTTPS either from a file on disk or from a certificate store.</span></span>

## <a name="gdpr"></a><span data-ttu-id="83ad9-160">RODO</span><span class="sxs-lookup"><span data-stu-id="83ad9-160">GDPR</span></span>

<span data-ttu-id="83ad9-161">ASP.NET Core udostępnia interfejsy API i szablony, które pomagają spełnić niektóre wymagania dotyczące [ogólne rozporządzenie o ochronie danych UE (Rodo)](https://www.eugdpr.org/) .</span><span class="sxs-lookup"><span data-stu-id="83ad9-161">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements.</span></span> <span data-ttu-id="83ad9-162">Aby uzyskać więcej informacji, zobacz [Obsługa Rodo w ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="83ad9-162">For more information, see [GDPR support in ASP.NET Core](xref:security/gdpr).</span></span> <span data-ttu-id="83ad9-163">[Przykładowa aplikacja](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) pokazuje, jak używać programu i umożliwia testowanie większości punktów rozszerzenia Rodo i interfejsów API dodanych do szablonów ASP.NET Core 2,1.</span><span class="sxs-lookup"><span data-stu-id="83ad9-163">A [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/gdpr/sample) shows how to use and lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span>

## <a name="integration-tests"></a><span data-ttu-id="83ad9-164">Testy integracji</span><span class="sxs-lookup"><span data-stu-id="83ad9-164">Integration tests</span></span>

<span data-ttu-id="83ad9-165">Został wprowadzony nowy pakiet upraszczający tworzenie i wykonywanie testów.</span><span class="sxs-lookup"><span data-stu-id="83ad9-165">A new package is introduced that streamlines test creation and execution.</span></span> <span data-ttu-id="83ad9-166">Pakiet [Microsoft. AspNetCore. MVC. test](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) obsługuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="83ad9-166">The [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) package handles the following tasks:</span></span>

* <span data-ttu-id="83ad9-167">Kopiuje plik zależności ( *\*. deps*) z testowanej aplikacji do folderu *bin* projektu testowego.</span><span class="sxs-lookup"><span data-stu-id="83ad9-167">Copies the dependency file (*\*.deps*) from the tested app into the test project's *bin* folder.</span></span>
* <span data-ttu-id="83ad9-168">Ustawia katalog główny zawartości na katalog główny projektu testowanej aplikacji, aby można było znaleźć pliki statyczne i strony/widoki podczas wykonywania testów.</span><span class="sxs-lookup"><span data-stu-id="83ad9-168">Sets the content root to the tested app's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="83ad9-169">Udostępnia klasę [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) do usprawnienia uruchamiania przetestowanej aplikacji z [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span><span class="sxs-lookup"><span data-stu-id="83ad9-169">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the tested app with [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span>

<span data-ttu-id="83ad9-170">Poniższy test korzysta z [xUnit](https://xunit.github.io/) , aby sprawdzić, czy strona indeksu ładuje się z kodem stanu sukcesu i z prawidłowym nagłówkiem Content-Type:</span><span class="sxs-lookup"><span data-stu-id="83ad9-170">The following test uses [xUnit](https://xunit.github.io/) to check that the Index page loads with a success status code and with the correct Content-Type header:</span></span>

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

<span data-ttu-id="83ad9-171">Aby uzyskać więcej informacji, zobacz temat [testy integracji](xref:test/integration-tests) .</span><span class="sxs-lookup"><span data-stu-id="83ad9-171">For more information, see the [Integration tests](xref:test/integration-tests) topic.</span></span>

## <a name="apicontroller-actionresultt"></a><span data-ttu-id="83ad9-172">[ApiController], ActionResult\<T ></span><span class="sxs-lookup"><span data-stu-id="83ad9-172">[ApiController], ActionResult\<T></span></span>

<span data-ttu-id="83ad9-173">Platforma ASP.NET Core 2.1 dodaje nowe konwencje programowania, które ułatwiają tworzenie czystych i bardziej opisowych interfejsów API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="83ad9-173">ASP.NET Core 2.1 adds new programming conventions that make it easier to build clean and descriptive web APIs.</span></span> <span data-ttu-id="83ad9-174">`ActionResult<T>` jest nowym typem dodawanym w celu umożliwienia aplikacji zwrócenia typu odpowiedzi lub dowolnego innego wyniku działania (podobnego do IActionResult), przy czym wskazuje typ odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="83ad9-174">`ActionResult<T>` is a new type added to allow an app to return either a response type or any other action result (similar to IActionResult), while still indicating the response type.</span></span> <span data-ttu-id="83ad9-175">Atrybut `[ApiController]` został również dodany jako sposób, aby zrezygnować z Konwencji i zachowań specyficznych dla interfejsu API sieci Web.</span><span class="sxs-lookup"><span data-stu-id="83ad9-175">The `[ApiController]` attribute has also been added as the way to opt in to Web API-specific conventions and behaviors.</span></span>

<span data-ttu-id="83ad9-176">Aby uzyskać więcej informacji, zobacz [Tworzenie internetowych interfejsów API za pomocą ASP.NET Core](xref:web-api/index).</span><span class="sxs-lookup"><span data-stu-id="83ad9-176">For more information, see [Build Web APIs with ASP.NET Core](xref:web-api/index).</span></span>

## <a name="ihttpclientfactory"></a><span data-ttu-id="83ad9-177">IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="83ad9-177">IHttpClientFactory</span></span>

<span data-ttu-id="83ad9-178">ASP.NET Core 2,1 obejmuje nową usługę `IHttpClientFactory`, która ułatwia konfigurowanie i używanie wystąpień `HttpClient` w aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="83ad9-178">ASP.NET Core 2.1 includes a new `IHttpClientFactory` service that makes it easier to configure and consume instances of `HttpClient` in apps.</span></span> <span data-ttu-id="83ad9-179">`HttpClient` już ma koncepcję delegowania programów obsługi, które mogą być połączone ze sobą w przypadku wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="83ad9-179">`HttpClient` already has the concept of delegating handlers that could be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="83ad9-180">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="83ad9-180">The factory:</span></span>

* <span data-ttu-id="83ad9-181">Sprawia, że rejestrowanie wystąpień `HttpClient` na nazwę klienta jest bardziej intuicyjne.</span><span class="sxs-lookup"><span data-stu-id="83ad9-181">Makes registering of instances of `HttpClient` per named client more intuitive.</span></span>
* <span data-ttu-id="83ad9-182">Implementuje procedurę obsługi Polly, która umożliwia używanie zasad Polly do ponawiania, CircuitBreakers itd.</span><span class="sxs-lookup"><span data-stu-id="83ad9-182">Implements a Polly handler that allows Polly policies to be used for Retry, CircuitBreakers, etc.</span></span>

<span data-ttu-id="83ad9-183">Aby uzyskać więcej informacji, zobacz [Inicjowanie żądań HTTP](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="83ad9-183">For more information, see [Initiate HTTP Requests](xref:fundamentals/http-requests).</span></span>

## <a name="kestrel-transport-configuration"></a><span data-ttu-id="83ad9-184">Konfiguracja transportu Kestrel</span><span class="sxs-lookup"><span data-stu-id="83ad9-184">Kestrel transport configuration</span></span>

<span data-ttu-id="83ad9-185">W wersji platformy ASP.NET Core 2.1 transport domyślny serwera Kestrel nie jest już oparty na bibliotece libuv, ale na zarządzanych gniazdach.</span><span class="sxs-lookup"><span data-stu-id="83ad9-185">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="83ad9-186">Aby uzyskać więcej informacji, zobacz [Kestrel Web Server implementation: transport Configuration](xref:fundamentals/servers/kestrel#transport-configuration).</span><span class="sxs-lookup"><span data-stu-id="83ad9-186">For more information, see [Kestrel web server implementation: Transport configuration](xref:fundamentals/servers/kestrel#transport-configuration).</span></span>

## <a name="generic-host-builder"></a><span data-ttu-id="83ad9-187">Ogólny konstruktor hosta</span><span class="sxs-lookup"><span data-stu-id="83ad9-187">Generic host builder</span></span>

<span data-ttu-id="83ad9-188">Wprowadzono ogólny Konstruktor hosta (`HostBuilder`).</span><span class="sxs-lookup"><span data-stu-id="83ad9-188">The Generic Host Builder (`HostBuilder`) has been introduced.</span></span> <span data-ttu-id="83ad9-189">Ten konstruktor może służyć do aplikacji, które nie przetwarzają żądań HTTP (Obsługa komunikatów, zadania w tle itp.).</span><span class="sxs-lookup"><span data-stu-id="83ad9-189">This builder can be used for apps that don't process HTTP requests (Messaging, background tasks, etc.).</span></span>

<span data-ttu-id="83ad9-190">Aby uzyskać więcej informacji, zobacz [host ogólny programu .NET](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="83ad9-190">For more information, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

## <a name="updated-spa-templates"></a><span data-ttu-id="83ad9-191">Zaktualizowane szablony SPA</span><span class="sxs-lookup"><span data-stu-id="83ad9-191">Updated SPA templates</span></span>

<span data-ttu-id="83ad9-192">Szablony aplikacji jednostronicowej dla platform Angular, React i React z Redux zostały zaktualizowane, aby używać standardowych struktur projektów i systemów budowania dla każdej z platform.</span><span class="sxs-lookup"><span data-stu-id="83ad9-192">The Single Page Application templates for Angular, React, and React with Redux are updated to use the standard project structures and build systems for each framework.</span></span>

<span data-ttu-id="83ad9-193">Szablon platformy Angular jest oparty na interfejsie wiersza polecenia Angular, a szablony platformy React — na narzędziu`create-react-app`.</span><span class="sxs-lookup"><span data-stu-id="83ad9-193">The Angular template is based on the Angular CLI, and the React templates are based on create-react-app.</span></span>

<span data-ttu-id="83ad9-194">Aby uzyskać więcej informacji, zobacz:</span><span class="sxs-lookup"><span data-stu-id="83ad9-194">For more information, see:</span></span>

* <xref:spa/angular>
* <xref:spa/react>
* <xref:spa/react-with-redux>

## <a name="razor-pages-search-for-razor-assets"></a><span data-ttu-id="83ad9-195">Wyszukiwanie zasobów Razor w stronach Razor</span><span class="sxs-lookup"><span data-stu-id="83ad9-195">Razor Pages search for Razor assets</span></span>

<span data-ttu-id="83ad9-196">W wersji 2.1 w programie Razor Pages wyszukiwanie zasobów Razor (na przykład układów i widoków częściowych) odbywa się w następujących katalogach w podanej kolejności:</span><span class="sxs-lookup"><span data-stu-id="83ad9-196">In 2.1, Razor Pages search for Razor assets (such as layouts and partials) in the following directories in the listed order:</span></span>

1. <span data-ttu-id="83ad9-197">Folder bieżące strony.</span><span class="sxs-lookup"><span data-stu-id="83ad9-197">Current Pages folder.</span></span>
1. <span data-ttu-id="83ad9-198">*/Pages/Shared/*</span><span class="sxs-lookup"><span data-stu-id="83ad9-198">*/Pages/Shared/*</span></span>
1. <span data-ttu-id="83ad9-199">*/Views/Shared/*</span><span class="sxs-lookup"><span data-stu-id="83ad9-199">*/Views/Shared/*</span></span>

## <a name="razor-pages-in-an-area"></a><span data-ttu-id="83ad9-200">Razor Pages w obszarze</span><span class="sxs-lookup"><span data-stu-id="83ad9-200">Razor Pages in an area</span></span>

<span data-ttu-id="83ad9-201">Razor Pages teraz obsługiwać [obszary](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="83ad9-201">Razor Pages now support [areas](xref:mvc/controllers/areas).</span></span> <span data-ttu-id="83ad9-202">Aby zobaczyć przykład wykorzystania obszarów, należy utworzyć nową aplikację internetową Razor Pages z indywidualnymi kontami użytkowników.</span><span class="sxs-lookup"><span data-stu-id="83ad9-202">To see an example of areas, create a new Razor Pages web app with individual user accounts.</span></span> <span data-ttu-id="83ad9-203">Aplikacja sieci Web Razor Pages z pojedynczymi kontami użytkowników zawiera */Areas/Identity/Pages*.</span><span class="sxs-lookup"><span data-stu-id="83ad9-203">A Razor Pages web app with individual user accounts includes */Areas/Identity/Pages*.</span></span>

## <a name="mvc-compatibility-version"></a><span data-ttu-id="83ad9-204">Wersja zgodności MVC</span><span class="sxs-lookup"><span data-stu-id="83ad9-204">MVC compatibility version</span></span>

<span data-ttu-id="83ad9-205">Metoda <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> pozwala aplikacji na zgodę lub rezygnację z ewentualnych zmian w zachowaniu, które wprowadzono w ASP.NET Core MVC 2,1 lub nowszych.</span><span class="sxs-lookup"><span data-stu-id="83ad9-205">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span>

<span data-ttu-id="83ad9-206">Aby uzyskać więcej informacji, zobacz <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="83ad9-206">For more information, see <xref:mvc/compatibility-version>.</span></span>

## <a name="migrate-from-20-to-21"></a><span data-ttu-id="83ad9-207">Migracja z wersji 2.0 do wersji 2.1</span><span class="sxs-lookup"><span data-stu-id="83ad9-207">Migrate from 2.0 to 2.1</span></span>

<span data-ttu-id="83ad9-208">Zobacz [Migrowanie z ASP.NET Core 2,0 do 2,1](xref:migration/20_21).</span><span class="sxs-lookup"><span data-stu-id="83ad9-208">See [Migrate from ASP.NET Core 2.0 to 2.1](xref:migration/20_21).</span></span>

## <a name="additional-information"></a><span data-ttu-id="83ad9-209">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="83ad9-209">Additional information</span></span>

<span data-ttu-id="83ad9-210">Aby uzyskać pełną listę zmian, zapoznaj się z [informacjami o wersji ASP.NET Core 2,1](https://github.com/dotnet/aspnetcore/releases/tag/2.1.0).</span><span class="sxs-lookup"><span data-stu-id="83ad9-210">For the complete list of changes, see the [ASP.NET Core 2.1 Release Notes](https://github.com/dotnet/aspnetcore/releases/tag/2.1.0).</span></span>
