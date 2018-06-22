---
title: What's new in ASP.NET Core 2.1
author: isaac2004
description: Dowiedz się więcej na temat nowych funkcji programu ASP.NET Core 2.1.
manager: wpickett
monikerRange: = aspnetcore-2.1
ms.custom: mvc
ms.date: 05/30/2018
uid: aspnetcore-2.1
ms.openlocfilehash: 98e867ec77a79102bc536fe5580c8796cf142feb
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291652"
---
# <a name="whats-new-in-aspnet-core-21"></a><span data-ttu-id="f45bf-103">What's new in ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="f45bf-103">What's new in ASP.NET Core 2.1</span></span>

<span data-ttu-id="f45bf-104">W tym artykule omówiono najbardziej znaczących zmian w ASP.NET Core 2.1, wraz z łączami do odpowiedniej dokumentacji.</span><span class="sxs-lookup"><span data-stu-id="f45bf-104">This article highlights the most significant changes in ASP.NET Core 2.1, with links to relevant documentation.</span></span>

## <a name="signalr"></a><span data-ttu-id="f45bf-105">SignalR</span><span class="sxs-lookup"><span data-stu-id="f45bf-105">SignalR</span></span>

<span data-ttu-id="f45bf-106">Dla platformy ASP.NET Core 2.1 została ponownie napisana SignalR.</span><span class="sxs-lookup"><span data-stu-id="f45bf-106">SignalR has been rewritten for ASP.NET Core 2.1.</span></span> <span data-ttu-id="f45bf-107">SignalR platformy ASP.NET Core zawiera wiele ulepszeń:</span><span class="sxs-lookup"><span data-stu-id="f45bf-107">ASP.NET Core SignalR includes a number of improvements:</span></span>

* <span data-ttu-id="f45bf-108">Uproszczony model skalowalnego w poziomie.</span><span class="sxs-lookup"><span data-stu-id="f45bf-108">A simplified scale-out model.</span></span>
* <span data-ttu-id="f45bf-109">Nowy klient JavaScript z zależności nie jQuery.</span><span class="sxs-lookup"><span data-stu-id="f45bf-109">A new JavaScript client with no jQuery dependency.</span></span>
* <span data-ttu-id="f45bf-110">Nowy compact binarne protokół oparte na MessagePack.</span><span class="sxs-lookup"><span data-stu-id="f45bf-110">A new compact binary protocol based on MessagePack.</span></span>
* <span data-ttu-id="f45bf-111">Obsługa protokołów niestandardowych.</span><span class="sxs-lookup"><span data-stu-id="f45bf-111">Support for custom protocols.</span></span>
* <span data-ttu-id="f45bf-112">Nowy model odpowiedzi przesyłania strumieniowego.</span><span class="sxs-lookup"><span data-stu-id="f45bf-112">A new streaming response model.</span></span>
* <span data-ttu-id="f45bf-113">Obsługa klientów zgodnie z Websocket systemu od zera.</span><span class="sxs-lookup"><span data-stu-id="f45bf-113">Support for clients based on bare WebSockets.</span></span>

<span data-ttu-id="f45bf-114">Aby uzyskać więcej informacji, zobacz [ASP.NET Core SignalR](xref:signalr/index).</span><span class="sxs-lookup"><span data-stu-id="f45bf-114">For more information, see [ASP.NET Core SignalR](xref:signalr/index).</span></span>

## <a name="razor-class-libraries"></a><span data-ttu-id="f45bf-115">Biblioteki klas razor</span><span class="sxs-lookup"><span data-stu-id="f45bf-115">Razor class libraries</span></span>

<span data-ttu-id="f45bf-116">Platformy ASP.NET Core 2.1 ułatwia kompilacji i umieścić Razor na podstawie interfejsu użytkownika w bibliotece i udostępnić go wielu projektach.</span><span class="sxs-lookup"><span data-stu-id="f45bf-116">ASP.NET Core 2.1 makes it easier to build and include Razor-based UI in a library and share it across multiple projects.</span></span> <span data-ttu-id="f45bf-117">Nowe umożliwia Razor SDK tworzenia plików Razor do projektu biblioteki klas, które mogą być umieszczone w pakietu NuGet.</span><span class="sxs-lookup"><span data-stu-id="f45bf-117">The new Razor SDK enables building Razor files into a class library project that can be packaged into a NuGet package.</span></span> <span data-ttu-id="f45bf-118">Widoki i stron w bibliotekach są odnajdowane automatycznie i może zostać zastąpiona przez aplikację.</span><span class="sxs-lookup"><span data-stu-id="f45bf-118">Views and pages in libraries are automatically discovered and can be overridden by the app.</span></span> <span data-ttu-id="f45bf-119">Przez włączenie Razor kompilacji do kompilacji:</span><span class="sxs-lookup"><span data-stu-id="f45bf-119">By integrating Razor compilation into the build:</span></span>

* <span data-ttu-id="f45bf-120">Czas uruchamiania aplikacji jest znacznie szybsze.</span><span class="sxs-lookup"><span data-stu-id="f45bf-120">The app startup time is significantly faster.</span></span>
* <span data-ttu-id="f45bf-121">Szybkie aktualizacje widokami Razor i stron w czasie wykonywania są nadal dostępne w ramach przepływu pracy programowania iteracji.</span><span class="sxs-lookup"><span data-stu-id="f45bf-121">Fast updates to Razor views and pages at runtime are still available as part of an iterative development workflow.</span></span>

<span data-ttu-id="f45bf-122">Aby uzyskać więcej informacji, zobacz [tworzyć wielokrotnego użytku interfejsu użytkownika przy użyciu projektu biblioteki klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="f45bf-122">For more information, see [Create reusable UI using the Razor Class Library project](xref:razor-pages/ui-class).</span></span>

## <a name="identity-ui-library--scaffolding"></a><span data-ttu-id="f45bf-123">Biblioteka interfejsów użytkownika tożsamości & szkieletów</span><span class="sxs-lookup"><span data-stu-id="f45bf-123">Identity UI library & scaffolding</span></span>

<span data-ttu-id="f45bf-124">Platformy ASP.NET Core 2.1 udostępnia [ASP.NET Core Identity](xref:security/authentication/identity) jako [biblioteki klas Razor](xref:razor-pages/ui-class).</span><span class="sxs-lookup"><span data-stu-id="f45bf-124">ASP.NET Core 2.1 provides [ASP.NET Core Identity](xref:security/authentication/identity) as a [Razor Class Library](xref:razor-pages/ui-class).</span></span> <span data-ttu-id="f45bf-125">Aplikacje, które obejmują tożsamości można stosować nowe tworzenia szkieletu tożsamości można selektywnie dodać kod źródłowy zawartych w bibliotece klasy Razor tożsamości (RCL).</span><span class="sxs-lookup"><span data-stu-id="f45bf-125">Apps that include Identity can apply the new Identity scaffolder to selectively add the source code contained in the Identity Razor Class Library (RCL).</span></span> <span data-ttu-id="f45bf-126">Można wygenerować kodu źródłowego, aby można było zmodyfikować kod i zmiany zachowania.</span><span class="sxs-lookup"><span data-stu-id="f45bf-126">You might want to generate source code so you can modify the code and change the behavior.</span></span> <span data-ttu-id="f45bf-127">Na przykład można nakazać tworzenia szkieletu, aby wygenerować kod używany w rejestracji.</span><span class="sxs-lookup"><span data-stu-id="f45bf-127">For example, you could instruct the scaffolder to generate the code used in registration.</span></span> <span data-ttu-id="f45bf-128">Wygenerowany kod mają pierwszeństwo przed ten sam kod w RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="f45bf-128">Generated code takes precedence over the same code in the Identity RCL.</span></span>

<span data-ttu-id="f45bf-129">Aplikacje, które wykonują **nie** obejmują uwierzytelniania można zastosować tworzenia szkieletu tożsamości, aby dodać pakiet RCL tożsamości.</span><span class="sxs-lookup"><span data-stu-id="f45bf-129">Apps that do **not** include authentication can apply the Identity scaffolder to add the RCL Identity package.</span></span> <span data-ttu-id="f45bf-130">Istnieje możliwość wybrania tożsamości kod zostanie wygenerowany.</span><span class="sxs-lookup"><span data-stu-id="f45bf-130">You have the option of selecting Identity code to be generated.</span></span>

<span data-ttu-id="f45bf-131">Aby uzyskać więcej informacji, zobacz [szkieletu tożsamości w projektach platformy ASP.NET Core](xref:security/authentication/scaffold-identity).</span><span class="sxs-lookup"><span data-stu-id="f45bf-131">For more information, see [Scaffold Identity in ASP.NET Core projects](xref:security/authentication/scaffold-identity).</span></span>

## <a name="https"></a><span data-ttu-id="f45bf-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="f45bf-132">HTTPS</span></span>

<span data-ttu-id="f45bf-133">Zwiększona fokus na bezpieczeństwo i ochrona prywatności ważne jest włączenie protokołu HTTPS dla aplikacji sieci web.</span><span class="sxs-lookup"><span data-stu-id="f45bf-133">With the increased focus on security and privacy, enabling HTTPS for web apps is important.</span></span> <span data-ttu-id="f45bf-134">Wymuszanie protokołu HTTPS staje się coraz bardziej ściśle w sieci web.</span><span class="sxs-lookup"><span data-stu-id="f45bf-134">HTTPS enforcement is becoming increasingly strict on the web.</span></span> <span data-ttu-id="f45bf-135">Lokacje, które nie używają protokołu HTTPS są uważane za niebezpieczne.</span><span class="sxs-lookup"><span data-stu-id="f45bf-135">Sites that don’t use HTTPS are considered insecure.</span></span> <span data-ttu-id="f45bf-136">Przeglądarki (chromu, Mozilla) coraz wymuszania, że funkcje sieci web musi być używany z bezpiecznym kontekście.</span><span class="sxs-lookup"><span data-stu-id="f45bf-136">Browsers (Chromium, Mozilla) are starting to enforce that web features must be used from a secure context.</span></span> <span data-ttu-id="f45bf-137">[GDPR](xref:security/gdpr) wymaga użycia protokołu HTTPS, aby chronić prywatność użytkowników.</span><span class="sxs-lookup"><span data-stu-id="f45bf-137">[GDPR](xref:security/gdpr) requires the use of HTTPS to protect user privacy.</span></span> <span data-ttu-id="f45bf-138">Gdy w środowisku produkcyjnym przy użyciu protokołu HTTPS jest szczególnie ważne, przy użyciu protokołu HTTPS do rozwoju mogą pomagać w zapobieganiu problemy we wdrożeniu (na przykład niezabezpieczonych łącza).</span><span class="sxs-lookup"><span data-stu-id="f45bf-138">While using HTTPS in production is critical, using HTTPS in development can help prevent issues in deployment (for example, insecure links).</span></span> <span data-ttu-id="f45bf-139">Platformy ASP.NET Core 2.1 zawiera wiele ulepszeń, które ułatwiają do używania protokołu HTTPS do rozwoju i konfigurowania protokołu HTTPS w środowisku produkcyjnym.</span><span class="sxs-lookup"><span data-stu-id="f45bf-139">ASP.NET Core 2.1 includes a number of improvements that make it easier to use HTTPS in development and to configure HTTPS in production.</span></span> <span data-ttu-id="f45bf-140">Aby uzyskać więcej informacji, zobacz [wymusić HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="f45bf-140">For more information, see [Enforce HTTPS](xref:security/enforcing-ssl).</span></span>

### <a name="on-by-default"></a><span data-ttu-id="f45bf-141">Na domyślnie</span><span class="sxs-lookup"><span data-stu-id="f45bf-141">On by default</span></span>

<span data-ttu-id="f45bf-142">Aby ułatwić projektowanie bezpiecznej witryny internetowej, HTTPS teraz jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="f45bf-142">To facilitate secure website development, HTTPS  is now enabled by default.</span></span> <span data-ttu-id="f45bf-143">Począwszy od 2.1 Kestrel nasłuchuje na `https://localhost:5001` podczas rozwoju lokalnego certyfikat znajduje się.</span><span class="sxs-lookup"><span data-stu-id="f45bf-143">Starting in 2.1, Kestrel listens on `https://localhost:5001` when a local development certificate is present.</span></span> <span data-ttu-id="f45bf-144">Utworzono certyfikatu deweloperskiego:</span><span class="sxs-lookup"><span data-stu-id="f45bf-144">A development certificate is created:</span></span>

* <span data-ttu-id="f45bf-145">Jako część zestawu SDK programu .NET Core pierwszego uruchomienia komputera, gdy używasz zestawu SDK po raz pierwszy.</span><span class="sxs-lookup"><span data-stu-id="f45bf-145">As part of the .NET Core SDK first-run experience, when you use the SDK for the first time.</span></span>
* <span data-ttu-id="f45bf-146">Ręcznie przy użyciu nowego `dev-certs` narzędzia.</span><span class="sxs-lookup"><span data-stu-id="f45bf-146">Manually using the new `dev-certs` tool.</span></span>

<span data-ttu-id="f45bf-147">Uruchom `dotnet dev-certs https --trust` ufać certyfikatowi.</span><span class="sxs-lookup"><span data-stu-id="f45bf-147">Run `dotnet dev-certs https --trust` to trust the certificate.</span></span>

### <a name="https-redirection-and-enforcement"></a><span data-ttu-id="f45bf-148">Przekierowania protokołu HTTPS i wymuszania</span><span class="sxs-lookup"><span data-stu-id="f45bf-148">HTTPS redirection and enforcement</span></span>

<span data-ttu-id="f45bf-149">Aplikacje sieci Web muszą zwykle nasłuchiwania protokołów HTTP i HTTPS, ale następnie przekierować cały ruch HTTP, HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f45bf-149">Web apps typically need to listen on both HTTP and HTTPS, but then redirect all HTTP traffic to HTTPS.</span></span> <span data-ttu-id="f45bf-150">W 2.1 specjalne pośredniczącym przekierowania HTTPS inteligentnie przekierowuje na podstawie obecności konfiguracji lub powiązane portów zostały wprowadzone.</span><span class="sxs-lookup"><span data-stu-id="f45bf-150">In 2.1, specialized HTTPS redirection middleware that intelligently redirects based on the presence of configuration or bound server ports has been introduced.</span></span>

<span data-ttu-id="f45bf-151">Korzystanie z protokołu HTTPS można dodatkowo wymuszane, za pomocą [interfejsów HTTP Strict transportu zabezpieczeń protokołu (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="f45bf-151">Use of HTTPS can be further enforced using [HTTP Strict Transport Security Protocol (HSTS)](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="f45bf-152">HSTS nakazuje przeglądarki zawsze dostęp do witryny za pośrednictwem protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f45bf-152">HSTS instructs browsers to always access the site via HTTPS.</span></span> <span data-ttu-id="f45bf-153">Platformy ASP.NET Core 2.1 dodaje oprogramowanie pośredniczące HSTS, obsługująca opcje maksymalny wiek, podrzędne, a HSTS wstępnego ładowania listy.</span><span class="sxs-lookup"><span data-stu-id="f45bf-153">ASP.NET Core 2.1 adds HSTS middleware that supports options for max age, subdomains, and the HSTS preload list.</span></span>

### <a name="configuration-for-production"></a><span data-ttu-id="f45bf-154">Konfiguracja do produkcji</span><span class="sxs-lookup"><span data-stu-id="f45bf-154">Configuration for production</span></span>

<span data-ttu-id="f45bf-155">W środowisku produkcyjnym należy jawnie skonfigurować HTTPS.</span><span class="sxs-lookup"><span data-stu-id="f45bf-155">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="f45bf-156">2.1 domyślny schemat konfiguracji związane z konfigurowaniem protokołu HTTPS dla Kestrel został dodany.</span><span class="sxs-lookup"><span data-stu-id="f45bf-156">In 2.1, default configuration schema for configuring HTTPS for Kestrel has been added.</span></span> <span data-ttu-id="f45bf-157">Aplikacje mogą być skonfigurowana do używania:</span><span class="sxs-lookup"><span data-stu-id="f45bf-157">Apps can be configured to use:</span></span>

* <span data-ttu-id="f45bf-158">Wiele punktów końcowych, w tym adresy URL.</span><span class="sxs-lookup"><span data-stu-id="f45bf-158">Multiple endpoints including the URLs.</span></span> <span data-ttu-id="f45bf-159">Aby uzyskać więcej informacji, zobacz [implementacją serwera sieci web Kestrel: Konfiguracja punktu końcowego](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="f45bf-159">For more information, see [Kestrel web server implementation: Endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>
* <span data-ttu-id="f45bf-160">Certyfikat do użycia na potrzeby protokołu HTTPS z pliku na dysku lub z magazynu certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="f45bf-160">The certificate to use for HTTPS either from a file on disk or from a certificate store.</span></span>

## <a name="gdpr"></a><span data-ttu-id="f45bf-161">GDPR</span><span class="sxs-lookup"><span data-stu-id="f45bf-161">GDPR</span></span>

<span data-ttu-id="f45bf-162">Platformy ASP.NET Core udostępnia interfejsy API i szablony, które ułatwiają korzystanie z niektórych [interfejsów UE ogólne dane ochrony rozporządzenia (GDPR)](https://www.eugdpr.org/) wymagania.</span><span class="sxs-lookup"><span data-stu-id="f45bf-162">ASP.NET Core provides APIs and templates to help meet some of the [EU General Data Protection Regulation (GDPR)](https://www.eugdpr.org/) requirements.</span></span> <span data-ttu-id="f45bf-163">Aby uzyskać więcej informacji, zobacz [GDPR obsługuje w ASP.NET Core](xref:security/gdpr).</span><span class="sxs-lookup"><span data-stu-id="f45bf-163">For more information, see [GDPR support in ASP.NET Core](xref:security/gdpr).</span></span> <span data-ttu-id="f45bf-164">A [Przykładowa aplikacja](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) przedstawia sposób użycia i umożliwia testowanie większość punktów rozszerzenia GDPR i dodane do szablonów platformy ASP.NET Core 2.1 interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="f45bf-164">A [sample app](https://github.com/aspnet/Docs/tree/live/aspnetcore/security/gdpr/sample) shows how to use and lets you test most of the GDPR extension points and APIs added to the ASP.NET Core 2.1 templates.</span></span>

## <a name="integration-tests"></a><span data-ttu-id="f45bf-165">Testy integracji</span><span class="sxs-lookup"><span data-stu-id="f45bf-165">Integration tests</span></span>

<span data-ttu-id="f45bf-166">Wprowadzono nowy pakiet czy usprawnia przetestować tworzenie i wykonywanie.</span><span class="sxs-lookup"><span data-stu-id="f45bf-166">A new package is introduced that streamlines test creation and execution.</span></span> <span data-ttu-id="f45bf-167">[Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) pakietu obsługuje następujące zadania:</span><span class="sxs-lookup"><span data-stu-id="f45bf-167">The [Microsoft.AspNetCore.Mvc.Testing](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Testing/) package handles the following tasks:</span></span>

* <span data-ttu-id="f45bf-168">Kopiuje plik zależności (*\*.deps*) z przetestowanej aplikacji do projektu testowego *bin* folderu.</span><span class="sxs-lookup"><span data-stu-id="f45bf-168">Copies the dependency file (*\*.deps*) from the tested app into the test project's *bin* folder.</span></span>
* <span data-ttu-id="f45bf-169">Ustawia zawartości katalogu głównym projektu przetestowanej aplikacji tak, aby pliki statyczne i stron/widoki są dostępne, gdy testy są wykonywane.</span><span class="sxs-lookup"><span data-stu-id="f45bf-169">Sets the content root to the tested app's project root so that static files and pages/views are found when the tests are executed.</span></span>
* <span data-ttu-id="f45bf-170">Udostępnia [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) klasę, aby usprawnić uruchamianie przetestowanej aplikacji z [elementu TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span><span class="sxs-lookup"><span data-stu-id="f45bf-170">Provides the [WebApplicationFactory](/dotnet/api/microsoft.aspnetcore.mvc.testing.webapplicationfactory-1) class to streamline bootstrapping the tested app with [TestServer](/dotnet/api/microsoft.aspnetcore.testhost.testserver).</span></span>

<span data-ttu-id="f45bf-171">Następującego testu używa [xUnit](https://xunit.github.io/) do sprawdzenia, czy strona indeksu ładuje z kodem stanu powodzenia i poprawne nagłówka Content-Type:</span><span class="sxs-lookup"><span data-stu-id="f45bf-171">The following test uses [xUnit](https://xunit.github.io/) to check that the Index page loads with a success status code and with the correct Content-Type header:</span></span>

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

<span data-ttu-id="f45bf-172">Aby uzyskać więcej informacji, zobacz [testy integracji](xref:test/integration-tests) tematu.</span><span class="sxs-lookup"><span data-stu-id="f45bf-172">For more information, see the [Integration tests](xref:test/integration-tests) topic.</span></span>

## <a name="apicontroller-actionresult"></a><span data-ttu-id="f45bf-173">[Klasy ApiController], ActionResult</span><span class="sxs-lookup"><span data-stu-id="f45bf-173">[ApiController], ActionResult</span></span>

<span data-ttu-id="f45bf-174">Platformy ASP.NET Core 2.1 dodaje nowe konwencje programowania, które ułatwiają tworzenie czystą i opisowy interfejsów API sieci web.</span><span class="sxs-lookup"><span data-stu-id="f45bf-174">ASP.NET Core 2.1 adds new programming conventions that make it easier to build clean and descriptive web APIs.</span></span> <span data-ttu-id="f45bf-175">`ActionResult<T>` Nowy typ jest dodane do Zezwalaj aplikacji na zwracany typ odpowiedzi lub innych wynik akcji, (podobnie jak IActionResult), nadal wskazujące typ odpowiedzi.</span><span class="sxs-lookup"><span data-stu-id="f45bf-175">`ActionResult<T>` is a new type added to allow an app to return either a response type or any other action result (similar to IActionResult), while still indicating the response type.</span></span> <span data-ttu-id="f45bf-176">`[ApiController]` Atrybut dodano także sposób korzystania z Konwencji danego interfejsu API sieci Web i zachowania.</span><span class="sxs-lookup"><span data-stu-id="f45bf-176">The `[ApiController]` attribute has also been added as the way to opt in to Web API-specific conventions and behaviors.</span></span>

<span data-ttu-id="f45bf-177">Aby uzyskać więcej informacji, zobacz [Tworzenie interfejsów API sieci Web z platformy ASP.NET Core](xref:web-api/index).</span><span class="sxs-lookup"><span data-stu-id="f45bf-177">For more information, see [Build Web APIs with ASP.NET Core](xref:web-api/index).</span></span>

## <a name="ihttpclientfactory"></a><span data-ttu-id="f45bf-178">IHttpClientFactory</span><span class="sxs-lookup"><span data-stu-id="f45bf-178">IHttpClientFactory</span></span>

<span data-ttu-id="f45bf-179">Platformy ASP.NET Core 2.1 zawiera nową `IHttpClientFactory` usługi, który ułatwia konfigurowanie i używanie wystąpienia `HttpClient` w aplikacjach.</span><span class="sxs-lookup"><span data-stu-id="f45bf-179">ASP.NET Core 2.1 includes a new `IHttpClientFactory` service that makes it easier to configure and consume instances of `HttpClient` in apps.</span></span> <span data-ttu-id="f45bf-180">`HttpClient` jest już pojęcie Delegowanie obsługi, które można połączyć ze sobą dla wychodzących żądań HTTP.</span><span class="sxs-lookup"><span data-stu-id="f45bf-180">`HttpClient` already has the concept of delegating handlers that could be linked together for outgoing HTTP requests.</span></span> <span data-ttu-id="f45bf-181">Fabryka:</span><span class="sxs-lookup"><span data-stu-id="f45bf-181">The factory:</span></span>

* <span data-ttu-id="f45bf-182">Umożliwia rejestrowanie wystąpień `HttpClient` na bardziej intuicyjne nazwanego klienta.</span><span class="sxs-lookup"><span data-stu-id="f45bf-182">Makes registering of instances of `HttpClient` per named client more intuitive.</span></span>
* <span data-ttu-id="f45bf-183">Implementuje obsługi Polly, która umożliwia Polly zasady do zastosowania w przypadku ponownych prób, CircuitBreakers itp.</span><span class="sxs-lookup"><span data-stu-id="f45bf-183">Implements a Polly handler that allows Polly policies to be used for Retry, CircuitBreakers, etc.</span></span>

<span data-ttu-id="f45bf-184">Aby uzyskać więcej informacji, zobacz [inicjowania żądań HTTP](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="f45bf-184">For more information, see [Initiate HTTP Requests](xref:fundamentals/http-requests).</span></span>

## <a name="kestrel-transport-configuration"></a><span data-ttu-id="f45bf-185">Konfiguracja transportu kestrel</span><span class="sxs-lookup"><span data-stu-id="f45bf-185">Kestrel transport configuration</span></span>

<span data-ttu-id="f45bf-186">Od wersji platformy ASP.NET Core 2.1 Kestrel przez domyślny transport jest już oparta na Libuv, ale zamiast tego oparty na zarządzanych gniazda.</span><span class="sxs-lookup"><span data-stu-id="f45bf-186">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="f45bf-187">Aby uzyskać więcej informacji, zobacz [implementacją serwera sieci web Kestrel: konfiguracja transportu](xref:fundamentals/servers/kestrel#transport-configuration).</span><span class="sxs-lookup"><span data-stu-id="f45bf-187">For more information, see [Kestrel web server implementation: Transport configuration](xref:fundamentals/servers/kestrel#transport-configuration).</span></span>

## <a name="generic-host-builder"></a><span data-ttu-id="f45bf-188">Konstruktor rodzajowego hosta</span><span class="sxs-lookup"><span data-stu-id="f45bf-188">Generic host builder</span></span>

<span data-ttu-id="f45bf-189">Ogólny konstruktora hosta (`HostBuilder`) została wprowadzona.</span><span class="sxs-lookup"><span data-stu-id="f45bf-189">The Generic Host Builder (`HostBuilder`) has been introduced.</span></span> <span data-ttu-id="f45bf-190">Ten konstruktor może służyć do aplikacji, które nie przetwarzają żądań HTTP (wiadomości, zadania w tle itp.).</span><span class="sxs-lookup"><span data-stu-id="f45bf-190">This builder can be used for apps that don't process HTTP requests (Messaging, background tasks, etc.).</span></span>

<span data-ttu-id="f45bf-191">Aby uzyskać więcej informacji, zobacz [.NET rodzajowego hosta](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="f45bf-191">For more information, see [.NET Generic Host](xref:fundamentals/host/generic-host).</span></span>

## <a name="updated-spa-templates"></a><span data-ttu-id="f45bf-192">Zaktualizowane szablony SPA</span><span class="sxs-lookup"><span data-stu-id="f45bf-192">Updated SPA templates</span></span>

<span data-ttu-id="f45bf-193">Szablony jednej strony aplikacji dla kątową, platformy React, i platformy React z Redux zostaną zaktualizowane, aby używać struktur standardowe projektu i stworzyć systemy dla każdej platformy.</span><span class="sxs-lookup"><span data-stu-id="f45bf-193">The Single Page Application templates for Angular, React, and React with Redux are updated to use the standard project structures and build systems for each framework.</span></span>

<span data-ttu-id="f45bf-194">Dyrektywy Angular szablon jest oparty na kątowego interfejsu wiersza polecenia i szablonów platformy React są oparte na tworzenie platformy react aplikacji.</span><span class="sxs-lookup"><span data-stu-id="f45bf-194">The Angular template is based on the Angular CLI, and the React templates are based on create-react-app.</span></span>
<span data-ttu-id="f45bf-195">Aby uzyskać więcej informacji, zobacz [szablony jednej strony aplikacji za pomocą platformy ASP.NET Core](xref:spa/index).</span><span class="sxs-lookup"><span data-stu-id="f45bf-195">For more information, see [Use the Single Page Application templates with ASP.NET Core](xref:spa/index).</span></span>

## <a name="razor-pages-search-for-razor-assets"></a><span data-ttu-id="f45bf-196">Wyszukiwanie stron razor zasoby Razor</span><span class="sxs-lookup"><span data-stu-id="f45bf-196">Razor Pages search for Razor assets</span></span>

<span data-ttu-id="f45bf-197">2.1 stron Razor Wyszukaj Razor zasoby (na przykład układy i częściowe) w następujących katalogów w określonej kolejności:</span><span class="sxs-lookup"><span data-stu-id="f45bf-197">In 2.1, Razor Pages search for Razor assets (such as layouts and partials) in the following directories in the listed order:</span></span>

1. <span data-ttu-id="f45bf-198">Bieżący folder strony.</span><span class="sxs-lookup"><span data-stu-id="f45bf-198">Current Pages folder.</span></span>
1. <span data-ttu-id="f45bf-199">*/Pages/udostępnione /*</span><span class="sxs-lookup"><span data-stu-id="f45bf-199">*/Pages/Shared/*</span></span>
1. <span data-ttu-id="f45bf-200">*/Views/udostępnione /*</span><span class="sxs-lookup"><span data-stu-id="f45bf-200">*/Views/Shared/*</span></span>

## <a name="razor-pages-in-an-area"></a><span data-ttu-id="f45bf-201">Stron razor w obszarze</span><span class="sxs-lookup"><span data-stu-id="f45bf-201">Razor Pages in an area</span></span>

<span data-ttu-id="f45bf-202">Obsługa teraz stron razor [obszarów](xref:mvc/controllers/areas).</span><span class="sxs-lookup"><span data-stu-id="f45bf-202">Razor Pages now support [areas](xref:mvc/controllers/areas).</span></span> <span data-ttu-id="f45bf-203">Aby zapoznać się z przykładem obszarów, Utwórz nową aplikację sieci web Razor strony z indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="f45bf-203">To see an example of areas, create a new Razor Pages web app with individual user accounts.</span></span> <span data-ttu-id="f45bf-204">Obejmuje stron Razor aplikacji sieci web z indywidualne konta użytkowników */Areas/Identity/Pages*.</span><span class="sxs-lookup"><span data-stu-id="f45bf-204">A Razor Pages web app with individual user accounts includes */Areas/Identity/Pages*.</span></span>

## <a name="migrate-from-20-to-21"></a><span data-ttu-id="f45bf-205">Migracja z 2.0 do 2.1</span><span class="sxs-lookup"><span data-stu-id="f45bf-205">Migrate from 2.0 to 2.1</span></span>

<span data-ttu-id="f45bf-206">Zobacz [migracji z platformy ASP.NET Core 2.0 lub 2.1](xref:migration/20_21).</span><span class="sxs-lookup"><span data-stu-id="f45bf-206">See [Migrate from ASP.NET Core 2.0 to 2.1](xref:migration/20_21).</span></span>

## <a name="additional-information"></a><span data-ttu-id="f45bf-207">Dodatkowe informacje</span><span class="sxs-lookup"><span data-stu-id="f45bf-207">Additional information</span></span>

<span data-ttu-id="f45bf-208">Aby uzyskać pełną listę zmian, zobacz [2.1 informacje o wersji platformy ASP.NET Core](https://github.com/aspnet/Home/releases/tag/2.1.0).</span><span class="sxs-lookup"><span data-stu-id="f45bf-208">For the complete list of changes, see the [ASP.NET Core 2.1 Release Notes](https://github.com/aspnet/Home/releases/tag/2.1.0).</span></span>
