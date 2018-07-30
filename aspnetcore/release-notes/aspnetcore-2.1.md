---
title: What's new in platformy ASP.NET Core 2.1
author: isaac2004
description: Dowiedz się więcej o nowych funkcjach w programie ASP.NET Core 2.1.
monikerRange: = aspnetcore-2.1
ms.custom: mvc
ms.date: 05/30/2018
uid: aspnetcore-2.1
ms.openlocfilehash: f113e990d8b8f3eb80def0d18e301d930e58e596
ms.sourcegitcommit: 516d0645c35ea784a3ae807be087ae70446a46ee
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39320679"
---
# <a name="whats-new-in-aspnet-core-21"></a><span data-ttu-id="ca329-103">What's new in platformy ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="ca329-103">What's new in ASP.NET Core 2.1</span></span>

<span data-ttu-id="ca329-104">W tym artykule przedstawiono najbardziej znaczących zmian w programie ASP.NET Core 2.1 wraz z łączami do odpowiedniej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="ca329-104">This article highlights the most significant changes in ASP.NET Core 2.1, with links to relevant documentation.</span></span>

## <a name="signalr"></a><span data-ttu-id="ca329-105">SignalR</span><span class="sxs-lookup"><span data-stu-id="ca329-105">SignalR</span></span>

<span data-ttu-id="ca329-106">Ma został przepisany SignalR dla platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ca329-106">SignalR has been rewritten for ASP.NET Core 2.1.</span></span> <span data-ttu-id="ca329-107">SignalR platformy ASP.NET Core zawiera szereg ulepszeń:</span><span class="sxs-lookup"><span data-stu-id="ca329-107">ASP.NET Core SignalR includes a number of improvements:</span></span>

* <span data-ttu-id="ca329-108">Uproszczony model skalowalnego w poziomie.</span><span class="sxs-lookup"><span data-stu-id="ca329-108">A simplified scale-out model.</span></span>
* <span data-ttu-id="ca329-109">Nowy klient JavaScript za pomocą niezależne jQuery.</span><span class="sxs-lookup"><span data-stu-id="ca329-109">A new JavaScript client with no jQuery dependency.</span></span>
* <span data-ttu-id="ca329-110">Nowy compact binarne protokół oparte na MessagePack.</span><span class="sxs-lookup"><span data-stu-id="ca329-110">A new compact binary protocol based on MessagePack.</span></span>
* <span data-ttu-id="ca329-111">Informacje dotyczące obsługi niestandardowych protokołów.</span><span class="sxs-lookup"><span data-stu-id="ca329-111">Support for custom protocols.</span></span>
* <span data-ttu-id="ca329-112">Nowy model odpowiedzi przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="ca329-112">A new streaming response model.</span></span>
* <span data-ttu-id="ca329-113">Obsługa klientów zgodnie z ustawieniami systemu od zera funkcja WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ca329-113">Support for clients based on bare WebSockets.</span></span>

<span data-ttu-id="ca329-114">Aby uzyskać więcej informacji, zobacz [biblioteki SignalR platformy ASP.NET Core](xref:signalr/index).</span><span class="sxs-lookup"><span data-stu-id="ca329-114">For more information, see [ASP.NET Core SignalR](xref:signalr/index).</span></span>

## <a name="razor-class-libraries"></a><span data-ttu-id="ca329-115">Biblioteki klas razor</span><span class="sxs-lookup"><span data-stu-id="ca329-115">Razor class libraries</span></span>

<span data-ttu-id="ca329-116">ASP.NET Core 2.1 ułatwia tworzenie i obejmują interfejsu użytkownika opartego na Razor w bibliotece i udostępnić go w wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="ca329-116">ASP.NET Core 2.1 makes it easier to build and include Razor-based UI in a library and share it across multiple projects.</span></span> <span data-ttu-id="ca329-117">Nowe umożliwia Razor SDK Kompilowanie plików Razor projekt biblioteki klas, które mogą być umieszczone w pakiet NuGet.</span><span class="sxs-lookup"><span data-stu-id="ca329-117">The new Razor SDK enables building Razor files into a class library project that can be packaged into a NuGet package.</span></span> <span data-ttu-id="ca329-118">Widoki stron w bibliotekach są automatycznie wykrywane i może być zastąpiona przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="ca329-118">Views and pages in libraries are automatically discovered and can be overridden by the app.</span></span> <span data-ttu-id="ca329-119">Dzięki integracji kompilacji Razor do kompilacji:</span><span class="sxs-lookup"><span data-stu-id="ca329-119">By integrating Razor compilation into the build:</span></span>

* <span data-ttu-id="ca329-120">Czas uruchamiania aplikacji jest znacznie szybsze.</span><span class="sxs-lookup"><span data-stu-id="ca329-120">The app startup time is significantly faster.</span></span>
* <span data-ttu-id="ca329-121">Szybkie aktualizacje widokami Razor i stron w czasie wykonywania są nadal dostępne jako część przepływu pracy iteracyjne projektowanie.</span><span class="sxs-lookup"><span data-stu-id="ca329-121">Fast updates to Razor views and pages at runtime are still available as part of an iterative development workflow.</span></span>

<span data-ttu-id="ca329-122">Aby uzyskać więcej informacji, zobacz [tworzenia interfejsu użytkownika do wielokrotnego użytku, używając projektu biblioteki klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="ca329-122">For more information, see [Create reusable UI using the Razor Class Library project](xref:razor-pages/ui-class).</span></span>

## <a name="identity-ui-library--scaffolding"></a><span data-ttu-id="ca329-123">Biblioteka interfejsów użytkownika tożsamości i tworzenia szkieletów</span><span class="sxs-lookup"><span data-stu-id="ca329-123">Identity UI library & scaffolding</span></span>

<span data-ttu-id="ca329-124">Platforma ASP.NET Core 2.1 udostępnia [tożsamości platformy ASP.NET Core](xref:security/authentication/identity) jako [biblioteki klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="ca329-124">ASP.NET Core 2.1 provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="ca329-125">Aplikacje, które zawierają tożsamości można zastosować nowy generator szkieletu tożsamości, aby selektywnie Dodawanie kodu źródłowego, znajdujących się w bibliotece klas Razor tożsamości (RCL).</span><span class="sxs-lookup"><span data-stu-id="ca329-125">Apps that include Identity can apply the new Identity scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="ca329-126">Można wygenerować kod źródłowy, aby można było zmodyfikować kod i zmienić zachowanie.</span><span class="sxs-lookup"><span data-stu-id="ca329-126">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="ca329-127">Na przykład można nakazać Generator szkieletu do generowania kodu, używane podczas rejestracji.</span><span class="sxs-lookup"><span data-stu-id="ca329-127">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="ca329-128">Wygenerowany kod mają pierwszeństwo przed ten sam kod w RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="ca329-128">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="ca329-129">Aplikacje, które wykonują **nie** obejmują uwierzytelniania można zastosować Generator szkieletu tożsamości, aby dodać pakiet RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="ca329-129">Apps that do **not** include authentication can apply the Identity scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="ca329-130">Masz możliwość wyboru tożsamości kodu do wygenerowania.</span><span class="sxs-lookup"><span data-stu-id="ca329-130">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="ca329-131">Aby uzyskać więcej informacji, zobacz [szkieletu tożsamość w projektach programu ASP.NET Core](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="ca329-131">For more information, see [Scaffold Identity in ASP.NET Core projects](xref:security/authentication/scaffold-identity).</span></span>

## <a name="https"></a><span data-ttu-id="ca329-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="ca329-132">HTTPS</span></span>

<span data-ttu-id="ca329-133">Za pomocą lepiej skoncentrować się na zabezpieczeniach i prywatności ważne jest włączenie protokołu HTTPS dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="ca329-133">With the increased focus on security and privacy, enabling HTTPS for web apps is important.</span></span> <span data-ttu-id="ca329-134">Wymuszanie protokołu HTTPS staje się coraz bardziej rygorystyczne w sieci web.</span><span class="sxs-lookup"><span data-stu-id="ca329-134">HTTPS enforcement is becoming increasingly strict on the web.</span></span> <span data-ttu-id="ca329-135">Lokacje, które nie używają protokołu HTTPS, są uważane za niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="ca329-135">Sites that don’t use HTTPS are considered insecure.</span></span> <span data-ttu-id="ca329-136">Przeglądarki (przeglądarki Chromium, Mozilla) coraz częściej wymuszania, że funkcje sieci web musi być używany z bezpiecznym kontekście.</span><span class="sxs-lookup"><span data-stu-id="ca329-136">Browsers (Chromium, Mozilla) are starting to enforce that web features must be used from a secure context.</span></span> <span data-ttu-id="ca329-137">[RODO](xref:security/gdpr) wymaga użycia protokołu HTTPS, aby chronić prywatność użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ca329-137">[GDPR](xref:security/gdpr) requires the use of HTTPS to protect user privacy.</span></span> <span data-ttu-id="ca329-138">Przy użyciu protokołu HTTPS w środowisku produkcyjnym jest krytyczna, przy użyciu protokołu HTTPS w rozwoju może zapobiec problemom we wdrożeniu (na przykład niezabezpieczone łącza).</span><span class="sxs-lookup"><span data-stu-id="ca329-138">While using HTTPS in production is critical, using HTTPS in development can help prevent issues in deployment (for example, insecure links).</span></span> <span data-ttu-id="ca329-139">Platforma ASP.NET Core 2.1 zawiera szereg ulepszeń, które ułatwiają do używania protokołu HTTPS podczas tworzenia i konfigurowania protokołu HTTPS w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="ca329-139">ASP.NET Core 2.1 includes a number of improvements that make it easier to use HTTPS in development and to configure HTTPS in production.</span></span> <span data-ttu-id="ca329-140">Aby uzyskać więcej informacji, zobacz [Wymuszanie protokołu HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="ca329-140">For more information, see [Enforce HTTPS](xref:security/enforcing-ssl).</span></span>

### <a name="on-by-default"></a><span data-ttu-id="ca329-141">Na domyślnie</span><span class="sxs-lookup"><span data-stu-id="ca329-141">On by default</span></span>

<span data-ttu-id="ca329-142">W celu ułatwienia tworzenia bezpiecznej witryny sieci Web, HTTPS jest teraz włączony domyślnie.</span><span class="sxs-lookup"><span data-stu-id="ca329-142">To facilitate secure website development, HTTPS  is now enabled by default.</span></span> <span data-ttu-id="ca329-143">Począwszy od 2.1 Kestrel nasłuchuje na `https://localhost:5001` kiedy certyfikat rozwoju lokalnego znajduje się.</span><span class="sxs-lookup"><span data-stu-id="ca329-143">Starting in 2.1, Kestrel listens on `https://localhost:5001` when a local development certificate is present.</span></span> <span data-ttu-id="ca329-144">Utworzono certyfikatu deweloperskiego:</span><span class="sxs-lookup"><span data-stu-id="ca329-144">A development certificate is created:</span></span>

* <span data-ttu-id="ca329-145">Jako część zestawu .NET Core SDK pierwszego uruchomienia komputera, gdy używasz zestawu SDK po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="ca329-145">As part of the .NET Core SDK first-run experience, when you use the SDK for the first time.</span></span>
* <span data-ttu-id="ca329-146">Ręcznie przy użyciu nowego `dev-certs` narzędzia.</span><span class="sxs-lookup"><span data-stu-id="ca329-146">Manually using the new `dev-certs` tool.</span></span>

<span data-ttu-id="ca329-147">Uruchom `dotnet dev-certs https --trust` ufać certyfikatowi.</span><span class="sxs-lookup"><span data-stu-id="ca329-147">Run `dotnet dev-certs https --trust` to trust the certificate.</span></span>

### <a name="https-redirection-and-enforcement"></a><span data-ttu-id="ca329-148">Przekierowania protokołu HTTPS i wymuszania</span><span class="sxs-lookup"><span data-stu-id="ca329-148">HTTPS redirection and enforcement</span></span>

<span data-ttu-id="ca329-149">Aplikacje internetowe zazwyczaj konieczne nasłuchiwania protokołów HTTP i HTTPS, ale następnie przekierowania całego ruchu HTTP na HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ca329-149">Web apps typically need to listen on both HTTP and HTTPS, but then redirect all HTTP traffic to HTTPS.</span></span> <span data-ttu-id="ca329-150">2.1 wyspecjalizowanych pośredniczącym przekierowania protokołu HTTPS, inteligentnie przekierowującego na podstawie obecności konfiguracji lub powiązanych portów zostały wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="ca329-150">In 2.1, specialized HTTPS redirection middleware that intelligently redirects based on the presence of configuration or bound server ports has been introduced.</span></span>

<span data-ttu-id="ca329-151">Korzystanie z protokołu HTTPS można dodatkowo wymuszane, za pomocą [interfejsów HTTP Strict transportu zabezpieczeń protokołu (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="ca329-151">Use of HTTPS can be further enforced using [HTTP Strict Transport Security Protocol (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="ca329-152">HSTS powoduje, że przeglądarek zawsze dostęp do witryny za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ca329-152">HSTS instructs browsers to always access the site via HTTPS.</span></span> <span data-ttu-id="ca329-153">Platforma ASP.NET Core 2.1 dodaje oprogramowanie pośredniczące HSTS, który obsługuje opcje dla maksymalnego wieku, poddomen, i HSTS wstępnego ładowania listy.</span><span class="sxs-lookup"><span data-stu-id="ca329-153">ASP.NET Core 2.1 adds HSTS middleware that supports options for max age, subdomains, and the HSTS preload list.</span></span>

### <a name="configuration-for-production"></a><span data-ttu-id="ca329-154">Konfiguracja dla trybu produkcyjnego</span><span class="sxs-lookup"><span data-stu-id="ca329-154">Configuration for production</span></span>

<span data-ttu-id="ca329-155">W środowisku produkcyjnym należy jawnie skonfigurować protokół HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ca329-155">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="ca329-156">2.1 domyślny schemat konfiguracji dotyczące konfigurowania protokołu HTTPS dla Kestrel został dodany.</span><span class="sxs-lookup"><span data-stu-id="ca329-156">In 2.1, default configuration schema for configuring HTTPS for Kestrel has been added.</span></span> <span data-ttu-id="ca329-157">Aplikacje można skonfigurować do użycia:</span><span class="sxs-lookup"><span data-stu-id="ca329-157">Apps can be configured to use:</span></span>

* <span data-ttu-id="ca329-158">Wiele punktów końcowych, w tym adresy URL.</span><span class="sxs-lookup"><span data-stu-id="ca329-158">Multiple endpoints including the URLs.</span></span> <span data-ttu-id="ca329-159">Aby uzyskać więcej informacji, zobacz [implementacji serwera sieci web Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="ca329-159">For more information, see [Kestrel web server implementation: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
* <span data-ttu-id="ca329-160">Certyfikat używany do obsługi protokołu HTTPS z plikiem na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="ca329-160">The certificate to use for HTTPS either from a file on disk or from a certificate store.</span></span>

## <a name="gdpr"></a><span data-ttu-id="ca329-161">RODO</span><span class="sxs-lookup"><span data-stu-id="ca329-161">GDPR</span></span>

<span data-ttu-id="ca329-162">Platformy ASP.NET Core udostępnia interfejsów API i szablony, które ułatwiają korzystanie z niektórych [Unii Europejskiej ogólnego danych (GDPR Protection Regulation)](https://www.eugdpr.org/) wymagania.</span><span class="sxs-lookup"><span data-stu-id="ca329-162">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements.</span></span> <span data-ttu-id="ca329-163">Aby uzyskać więcej informacji, zobacz [RODO obsługi w programie ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="ca329-163">For more information, see [GDPR support in ASP.NET Core](xref:security/gdpr).</span></span> <span data-ttu-id="ca329-164">A [przykładową aplikację](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) pokazano, jak i umożliwia testowanie większość punkty rozszerzenia RODO i interfejsów API dodano szablony platformy ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="ca329-164">A [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) shows how to use and lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span>

## <a name="integration-tests"></a><span data-ttu-id="ca329-165">Testy integracji</span><span class="sxs-lookup"><span data-stu-id="ca329-165">Integration tests</span></span>

<span data-ttu-id="ca329-166">Nowy pakiet został wprowadzony przetestować upraszczają tworzenie i wykonywanie.</span><span class="sxs-lookup"><span data-stu-id="ca329-166">A new package is introduced that streamlines test creation and execution.</span></span> <span data-ttu-id="ca329-167">[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) pakietu obsługuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="ca329-167">The [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) package handles the following tasks:</span></span>

* <span data-ttu-id="ca329-168">Kopiuje plik zależności (*\*.deps*) z przetestowanej aplikacji w projekcie testowym *bin* folderu.</span><span class="sxs-lookup"><span data-stu-id="ca329-168">Copies the dependency file (*\*.deps*) from the tested app into the test project's *bin* folder.</span></span>
* <span data-ttu-id="ca329-169">Ustawia zawartości głównego katalogu głównego projektu przetestowanej aplikacji tak, aby pliki statyczne i stron/widoki są dostępne, gdy testy są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="ca329-169">Sets the content root to the tested app's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="ca329-170">Udostępnia [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) klasy, aby usprawnić uruchamianie przetestowanej aplikacji za pomocą [elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span><span class="sxs-lookup"><span data-stu-id="ca329-170">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the tested app with [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span>

<span data-ttu-id="ca329-171">Następującego testu używa [xUnit](https://xunit.github.io/) do sprawdzenia, czy strony indeksu ładuje z pomyślny kod statusu i poprawny nagłówek Content-Type:</span><span class="sxs-lookup"><span data-stu-id="ca329-171">The following test uses [xUnit](https://xunit.github.io/) to check that the Index page loads with a success status code and with the correct Content-Type header:</span></span>

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

<span data-ttu-id="ca329-172">Aby uzyskać więcej informacji, zobacz [testy integracji](xref:test/integration-tests) tematu.</span><span class="sxs-lookup"><span data-stu-id="ca329-172">For more information, see the [Integration tests](xref:test/integration-tests) topic.</span></span>

## <a name="apicontroller-actionresultt"></a><span data-ttu-id="ca329-173">[Klasy ApiController], ActionResult\<T ></span><span class="sxs-lookup"><span data-stu-id="ca329-173">[ApiController], ActionResult\<T></span></span>

<span data-ttu-id="ca329-174">Platforma ASP.NET Core 2.1 dodaje nowe konwencje programowania, które ułatwiają tworzenie czyste i opisu interfejsów API sieci web.</span><span class="sxs-lookup"><span data-stu-id="ca329-174">ASP.NET Core 2.1 adds new programming conventions that make it easier to build clean and descriptive web APIs.</span></span> <span data-ttu-id="ca329-175">`ActionResult<T>` Nowy typ dodano na aplikację, aby zwracać typ odpowiedzi lub inny wynik akcji, (podobnie jak IActionResult), nadal wskazujące typ odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="ca329-175">`ActionResult<T>` is a new type added to allow an app to return either a response type or any other action result (similar to IActionResult), while still indicating the response type.</span></span> <span data-ttu-id="ca329-176">`[ApiController]` Atrybut został również dodany jako sposób korzystania z Konwencji specyficzne dla interfejsu API sieci Web i zachowania.</span><span class="sxs-lookup"><span data-stu-id="ca329-176">The `[ApiController]` attribute has also been added as the way to opt in to Web API-specific conventions and behaviors.</span></span>

<span data-ttu-id="ca329-177">Aby uzyskać więcej informacji, zobacz [Tworzenie interfejsów API sieci Web za pomocą programu ASP.NET Core](xref:web-api/index).</span><span class="sxs-lookup"><span data-stu-id="ca329-177">For more information, see [Build Web APIs with ASP.NET Core](xref:web-api/index).</span></span>

## <a name="ihttpclientfactory"></a><span data-ttu-id="ca329-178">IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="ca329-178">IHttpClientFactory</span></span>

<span data-ttu-id="ca329-179">Platforma ASP.NET Core 2.1 zawiera nową `IHttpClientFactory` usługa, która ułatwia konfigurowanie i używanie wystąpienia `HttpClient` w aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="ca329-179">ASP.NET Core 2.1 includes a new `IHttpClientFactory` service that makes it easier to configure and consume instances of `HttpClient` in apps.</span></span> <span data-ttu-id="ca329-180">`HttpClient` ma już pojęcie Delegowanie obsługi, które można połączyć ze sobą dla wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="ca329-180">`HttpClient` already has the concept of delegating handlers that could be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="ca329-181">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="ca329-181">The factory:</span></span>

* <span data-ttu-id="ca329-182">Sprawia, że rejestrowanie wystąpień `HttpClient` na klienta o nazwie bardziej intuicyjne.</span><span class="sxs-lookup"><span data-stu-id="ca329-182">Makes registering of instances of `HttpClient` per named client more intuitive.</span></span>
* <span data-ttu-id="ca329-183">Implementuje obsługi Polly, umożliwiająca Polly zasady służący do ponawiania, CircuitBreakers itp.</span><span class="sxs-lookup"><span data-stu-id="ca329-183">Implements a Polly handler that allows Polly policies to be used for Retry, CircuitBreakers, etc.</span></span>

<span data-ttu-id="ca329-184">Aby uzyskać więcej informacji, zobacz [inicjowanie żądań HTTP](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="ca329-184">For more information, see [Initiate HTTP Requests](xref:fundamentals/http-requests).</span></span>

## <a name="kestrel-transport-configuration"></a><span data-ttu-id="ca329-185">Konfiguracja transportu kestrel</span><span class="sxs-lookup"><span data-stu-id="ca329-185">Kestrel transport configuration</span></span>

<span data-ttu-id="ca329-186">W wersji platformy ASP.NET Core 2.1 firmy Kestrel transportu domyślnego jest już oparte na Libuv, ale oparta na zarządzanych gniazda.</span><span class="sxs-lookup"><span data-stu-id="ca329-186">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="ca329-187">Aby uzyskać więcej informacji, zobacz [implementacji serwera sieci web Kestrel: konfiguracja transportu](xref:fundamentals/servers/kestrel#transport-configuration).</span><span class="sxs-lookup"><span data-stu-id="ca329-187">For more information, see [Kestrel web server implementation: Transport configuration](xref:fundamentals/servers/kestrel#transport-configuration).</span></span>

## <a name="generic-host-builder"></a><span data-ttu-id="ca329-188">Konstruktor ogólnego hosta</span><span class="sxs-lookup"><span data-stu-id="ca329-188">Generic host builder</span></span>

<span data-ttu-id="ca329-189">Ogólny konstruktora hosta (`HostBuilder`) została wprowadzona.</span><span class="sxs-lookup"><span data-stu-id="ca329-189">The Generic Host Builder (`HostBuilder`) has been introduced.</span></span> <span data-ttu-id="ca329-190">Ten konstruktor może służyć do aplikacji, które nie przetwarzają żądania HTTP (Obsługa komunikatów, zadania w tle itp.).</span><span class="sxs-lookup"><span data-stu-id="ca329-190">This builder can be used for apps that don't process HTTP requests (Messaging, background tasks, etc.).</span></span>

<span data-ttu-id="ca329-191">Aby uzyskać więcej informacji, zobacz [hosta ogólnego .NET](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="ca329-191">For more information, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

## <a name="updated-spa-templates"></a><span data-ttu-id="ca329-192">Zaktualizowane szablony SPA</span><span class="sxs-lookup"><span data-stu-id="ca329-192">Updated SPA templates</span></span>

<span data-ttu-id="ca329-193">Szablony aplikacji jednostronicowej dla szablonów Angular, React i platformy React z kontenera Redux są aktualizowane, aby używać struktur standardowy projekt i tworzenie systemów dla każdej struktury.</span><span class="sxs-lookup"><span data-stu-id="ca329-193">The Single Page Application templates for Angular, React, and React with Redux are updated to use the standard project structures and build systems for each framework.</span></span>

<span data-ttu-id="ca329-194">Platformy Angular szablon jest oparty na interfejs wiersza polecenia usługi Angular i szablony platformy React są oparte na tworzenie platformy react aplikacji.</span><span class="sxs-lookup"><span data-stu-id="ca329-194">The Angular template is based on the Angular CLI, and the React templates are based on create-react-app.</span></span>
<span data-ttu-id="ca329-195">Aby uzyskać więcej informacji, zobacz [używania szablonów aplikacji jednostronicowej platformy ASP.NET Core](xref:spa/index).</span><span class="sxs-lookup"><span data-stu-id="ca329-195">For more information, see [Use the Single Page Application templates with ASP.NET Core](xref:spa/index).</span></span>

## <a name="razor-pages-search-for-razor-assets"></a><span data-ttu-id="ca329-196">Strony razor wyszukiwania zasobów Razor</span><span class="sxs-lookup"><span data-stu-id="ca329-196">Razor Pages search for Razor assets</span></span>

<span data-ttu-id="ca329-197">2.1 stron Razor Wyszukaj Razor zasoby (na przykład układy i częściowych), w następujących katalogach w określonej kolejności:</span><span class="sxs-lookup"><span data-stu-id="ca329-197">In 2.1, Razor Pages search for Razor assets (such as layouts and partials) in the following directories in the listed order:</span></span>

1. <span data-ttu-id="ca329-198">Bieżący folder stron.</span><span class="sxs-lookup"><span data-stu-id="ca329-198">Current Pages folder.</span></span>
1. <span data-ttu-id="ca329-199">*/Pages/udostępnione /*</span><span class="sxs-lookup"><span data-stu-id="ca329-199">*/Pages/Shared/*</span></span>
1. <span data-ttu-id="ca329-200">*/Views/udostępnione /*</span><span class="sxs-lookup"><span data-stu-id="ca329-200">*/Views/Shared/*</span></span>

## <a name="razor-pages-in-an-area"></a><span data-ttu-id="ca329-201">Strony razor, w obszarze</span><span class="sxs-lookup"><span data-stu-id="ca329-201">Razor Pages in an area</span></span>

<span data-ttu-id="ca329-202">Obsługują teraz stron razor [obszarów](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="ca329-202">Razor Pages now support [areas](xref:mvc/controllers/areas).</span></span> <span data-ttu-id="ca329-203">Aby zobaczyć przykład obszarów, należy utworzyć nową aplikację sieci web dla stron Razor za pomocą indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ca329-203">To see an example of areas, create a new Razor Pages web app with individual user accounts.</span></span> <span data-ttu-id="ca329-204">Aplikacja internetowa ze stronami Razor za pomocą indywidualnych kont użytkowników zawiera */Areas/Identity/Pages*.</span><span class="sxs-lookup"><span data-stu-id="ca329-204">A Razor Pages web app with individual user accounts includes */Areas/Identity/Pages*.</span></span>

## <a name="migrate-from-20-to-21"></a><span data-ttu-id="ca329-205">Migrowanie z wersji 2.0 do wersji 2.1</span><span class="sxs-lookup"><span data-stu-id="ca329-205">Migrate from 2.0 to 2.1</span></span>

<span data-ttu-id="ca329-206">Zobacz [migracji z programu ASP.NET Core 2.0 i 2.1](xref:migration/20_21).</span><span class="sxs-lookup"><span data-stu-id="ca329-206">See [Migrate from ASP.NET Core 2.0 to 2.1](xref:migration/20_21).</span></span>

## <a name="additional-information"></a><span data-ttu-id="ca329-207">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="ca329-207">Additional information</span></span>

<span data-ttu-id="ca329-208">Aby uzyskać pełną listę zmian, zobacz [platformy ASP.NET Core 2.1 — informacje o wersji](https://github.com/aspnet/Home/releases/tag/2.1.0).</span><span class="sxs-lookup"><span data-stu-id="ca329-208">For the complete list of changes, see the [ASP.NET Core 2.1 Release Notes](https://github.com/aspnet/Home/releases/tag/2.1.0).</span></span>
