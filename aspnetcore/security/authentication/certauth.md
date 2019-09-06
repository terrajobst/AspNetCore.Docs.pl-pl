---
title: Konfigurowanie uwierzytelniania certyfikatów w ASP.NET Core
author: blowdart
description: Dowiedz się, jak skonfigurować uwierzytelnianie certyfikatów w ASP.NET Core dla usług IIS i HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 08/19/2019
uid: security/authentication/certauth
ms.openlocfilehash: ce7bcdbfb8ce0f1febf34b49786e92c917be139c
ms.sourcegitcommit: 116bfaeab72122fa7d586cdb2e5b8f456a2dc92a
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/05/2019
ms.locfileid: "70384853"
---
# <a name="overview"></a><span data-ttu-id="db2f3-103">Omówienie</span><span class="sxs-lookup"><span data-stu-id="db2f3-103">Overview</span></span>

<span data-ttu-id="db2f3-104">`Microsoft.AspNetCore.Authentication.Certificate`zawiera implementację podobną do [uwierzytelniania certyfikatu](https://tools.ietf.org/html/rfc5246#section-7.4.4) dla ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db2f3-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="db2f3-105">Uwierzytelnianie certyfikatu odbywa się na poziomie protokołu TLS, o ile nie zostanie kiedykolwiek przeASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="db2f3-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="db2f3-106">Dokładniej, jest to procedura obsługi uwierzytelniania, która sprawdza poprawność certyfikatu, a następnie przekazuje zdarzenie, w którym można rozwiązać ten certyfikat do `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="db2f3-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="db2f3-107">[Skonfiguruj hosta](#configure-your-host-to-require-certificates) na potrzeby uwierzytelniania certyfikatów, to usługi IIS, Kestrel, Azure Web Apps lub inne, z których korzystasz.</span><span class="sxs-lookup"><span data-stu-id="db2f3-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="db2f3-108">Scenariusze dotyczące serwerów proxy i równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="db2f3-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="db2f3-109">Uwierzytelnianie certyfikatu jest scenariuszem stanowym głównie używanym w przypadku, gdy serwer proxy lub moduł równoważenia obciążenia nie obsługuje ruchu między klientami i serwerami.</span><span class="sxs-lookup"><span data-stu-id="db2f3-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="db2f3-110">Jeśli jest używany serwer proxy lub moduł równoważenia obciążenia, uwierzytelnianie certyfikatu działa tylko wtedy, gdy serwer proxy lub moduł równoważenia obciążenia:</span><span class="sxs-lookup"><span data-stu-id="db2f3-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="db2f3-111">Obsługuje uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="db2f3-111">Handles the authentication.</span></span>
* <span data-ttu-id="db2f3-112">Przekazuje informacje o uwierzytelnianiu użytkownika do aplikacji (na przykład w nagłówku żądania), która działa na informacje o uwierzytelnianiu.</span><span class="sxs-lookup"><span data-stu-id="db2f3-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="db2f3-113">Alternatywą dla uwierzytelniania certyfikatu w środowiskach, w których są używane serwery proxy i moduły równoważenia obciążenia, są Active Directory usług federacyjnych (AD FS) za pomocą OpenID Connect Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="db2f3-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="db2f3-114">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="db2f3-114">Get started</span></span>

<span data-ttu-id="db2f3-115">Uzyskaj certyfikat HTTPS, zastosuj go i [skonfiguruj hosta](#configure-your-host-to-require-certificates) , aby wymagał certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="db2f3-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="db2f3-116">W aplikacji sieci Web Dodaj odwołanie do `Microsoft.AspNetCore.Authentication.Certificate` pakietu.</span><span class="sxs-lookup"><span data-stu-id="db2f3-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="db2f3-117">Następnie w `Startup.Configure` metodzie Zadzwoń `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` z własnymi opcjami `OnCertificateValidated` , podając delegata w celu wykonania dodatkowej weryfikacji dla certyfikatu klienta wysyłanego z żądaniami.</span><span class="sxs-lookup"><span data-stu-id="db2f3-117">Then in the `Startup.Configure` method, call `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="db2f3-118">Zmień te informacje `ClaimsPrincipal` na i ustaw `context.Principal` dla właściwości.</span><span class="sxs-lookup"><span data-stu-id="db2f3-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="db2f3-119">Jeśli uwierzytelnianie nie powiedzie się, ta `403 (Forbidden)` procedura obsługi zwróci `401 (Unauthorized)`odpowiedź zamiast elementu, zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="db2f3-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="db2f3-120">Powodem jest to, że uwierzytelnianie powinno nastąpić podczas początkowego połączenia TLS.</span><span class="sxs-lookup"><span data-stu-id="db2f3-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="db2f3-121">Przez czas, gdy dociera do programu obsługi, jest zbyt opóźniony.</span><span class="sxs-lookup"><span data-stu-id="db2f3-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="db2f3-122">Nie ma możliwości uaktualnienia połączenia z anonimowego połączenia z certyfikatem.</span><span class="sxs-lookup"><span data-stu-id="db2f3-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="db2f3-123">`app.UseAuthentication();` Dodaj`Startup.Configure` również metodę.</span><span class="sxs-lookup"><span data-stu-id="db2f3-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="db2f3-124">W przeciwnym razie Właściwość HttpContext. User nie zostanie ustawiona jako `ClaimsPrincipal` utworzona na podstawie certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="db2f3-124">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="db2f3-125">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="db2f3-125">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(
        CertificateAuthenticationDefaults.AuthenticationScheme)
            .AddCertificate();
    // All the other service configuration.
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseAuthentication();

    // All the other app configuration.
}
```

<span data-ttu-id="db2f3-126">W powyższym przykładzie przedstawiono domyślny sposób dodawania uwierzytelniania przy użyciu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="db2f3-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="db2f3-127">Program obsługi konstruuje jednostkę główną użytkownika przy użyciu wspólnych właściwości certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="db2f3-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="db2f3-128">Konfigurowanie weryfikacji certyfikatu</span><span class="sxs-lookup"><span data-stu-id="db2f3-128">Configure certificate validation</span></span>

<span data-ttu-id="db2f3-129">`CertificateAuthenticationOptions` Program obsługi ma pewne wbudowane walidacje, które są minimalnymi walidacjami, które należy wykonać na certyfikacie.</span><span class="sxs-lookup"><span data-stu-id="db2f3-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="db2f3-130">Każdy z tych ustawień jest domyślnie włączony.</span><span class="sxs-lookup"><span data-stu-id="db2f3-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="db2f3-131">AllowedCertificateTypes = łańcuchy, SelfSigned lub wszystkie (łańcuchowo | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="db2f3-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="db2f3-132">Ten test sprawdza, czy dozwolony jest tylko odpowiedni typ certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="db2f3-132">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="db2f3-133">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="db2f3-133">ValidateCertificateUse</span></span>

<span data-ttu-id="db2f3-134">Ten test sprawdza, czy certyfikat przedstawiony przez klienta ma rozszerzone użycie klucza uwierzytelniania klienta (EKU) lub nie rozszerzeń EKU w ogóle.</span><span class="sxs-lookup"><span data-stu-id="db2f3-134">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="db2f3-135">Zgodnie ze specyfikacją, jeśli nie określono rozszerzenia EKU, wszystkie rozszerzeń EKU są uznawane za prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="db2f3-135">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="db2f3-136">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="db2f3-136">ValidateValidityPeriod</span></span>

<span data-ttu-id="db2f3-137">Ten test sprawdza, czy certyfikat jest w jego okresie ważności.</span><span class="sxs-lookup"><span data-stu-id="db2f3-137">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="db2f3-138">W przypadku każdego żądania program obsługi zapewnia, że certyfikat, który był ważny, gdy był prezentowany, nie upłynął podczas bieżącej sesji.</span><span class="sxs-lookup"><span data-stu-id="db2f3-138">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="db2f3-139">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="db2f3-139">RevocationFlag</span></span>

<span data-ttu-id="db2f3-140">Flaga określająca, które certyfikaty w łańcuchu są sprawdzane pod kątem odwołania.</span><span class="sxs-lookup"><span data-stu-id="db2f3-140">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="db2f3-141">Sprawdzanie odwołań jest wykonywane tylko wtedy, gdy certyfikat jest powiązany z certyfikatem głównym.</span><span class="sxs-lookup"><span data-stu-id="db2f3-141">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="db2f3-142">Odwołaniemode</span><span class="sxs-lookup"><span data-stu-id="db2f3-142">RevocationMode</span></span>

<span data-ttu-id="db2f3-143">Flaga określająca sposób sprawdzania odwołania.</span><span class="sxs-lookup"><span data-stu-id="db2f3-143">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="db2f3-144">Określenie, że sprawdzanie online może skutkować długim opóźnieniem podczas kontaktowania się z urzędem certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="db2f3-144">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="db2f3-145">Sprawdzanie odwołań jest wykonywane tylko wtedy, gdy certyfikat jest powiązany z certyfikatem głównym.</span><span class="sxs-lookup"><span data-stu-id="db2f3-145">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="db2f3-146">Czy mogę skonfigurować aplikację, aby wymagać certyfikatu tylko w określonych ścieżkach?</span><span class="sxs-lookup"><span data-stu-id="db2f3-146">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="db2f3-147">Nie jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="db2f3-147">This isn't possible.</span></span> <span data-ttu-id="db2f3-148">Należy pamiętać, że wymiana certyfikatów jest wykonywana na początku konwersacji HTTPS, przez serwer przed odebraniem pierwszego żądania w ramach tego połączenia, aby nie było możliwe Określanie zakresu na podstawie pól żądania.</span><span class="sxs-lookup"><span data-stu-id="db2f3-148">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="db2f3-149">Zdarzenia programu obsługi</span><span class="sxs-lookup"><span data-stu-id="db2f3-149">Handler events</span></span>

<span data-ttu-id="db2f3-150">Program obsługi ma dwa zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="db2f3-150">The handler has two events:</span></span>

* <span data-ttu-id="db2f3-151">`OnAuthenticationFailed`&ndash; Wywołuje się, gdy wyjątek występuje podczas uwierzytelniania i pozwala na reagowanie.</span><span class="sxs-lookup"><span data-stu-id="db2f3-151">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="db2f3-152">`OnCertificateValidated`&ndash; Wywoływana po zweryfikowaniu certyfikatu, została pomyślnie utworzona Walidacja i domyślny podmiot zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="db2f3-152">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="db2f3-153">To zdarzenie umożliwia wykonywanie własnych weryfikacji i rozszerzanie lub zastępowanie podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="db2f3-153">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="db2f3-154">Przykłady obejmują:</span><span class="sxs-lookup"><span data-stu-id="db2f3-154">For examples include:</span></span>
  * <span data-ttu-id="db2f3-155">Ustalanie, czy certyfikat jest znany dla usług.</span><span class="sxs-lookup"><span data-stu-id="db2f3-155">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="db2f3-156">Konstruowanie własnego podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="db2f3-156">Constructing your own principal.</span></span> <span data-ttu-id="db2f3-157">Rozważmy następujący przykład w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="db2f3-157">Consider the following example in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var claims = new[]
                {
                    new Claim(
                        ClaimTypes.NameIdentifier, 
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer),
                    new Claim(ClaimTypes.Name,
                        context.ClientCertificate.Subject,
                        ClaimValueTypes.String, 
                        context.Options.ClaimsIssuer)
                };

                context.Principal = new ClaimsPrincipal(
                    new ClaimsIdentity(claims, context.Scheme.Name));
                context.Success();

                return Task.CompletedTask;
            }
        };
    });
```

<span data-ttu-id="db2f3-158">Jeśli okaże się, że certyfikat ruchu przychodzącego nie spełnia dodatkowej weryfikacji `context.Fail("failure reason")` , wywołaj z przyczynę niepowodzenia.</span><span class="sxs-lookup"><span data-stu-id="db2f3-158">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="db2f3-159">W przypadku rzeczywistej funkcjonalności prawdopodobnie chcesz wywołać usługę zarejestrowaną w iniekcji zależności, która łączy się z bazą danych lub innym typem magazynu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="db2f3-159">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="db2f3-160">Uzyskaj dostęp do usługi przy użyciu kontekstu przesłanego do obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="db2f3-160">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="db2f3-161">Rozważmy następujący przykład w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="db2f3-161">Consider the following example in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddAuthentication(
    CertificateAuthenticationDefaults.AuthenticationScheme)
    .AddCertificate(options =>
    {
        options.Events = new CertificateAuthenticationEvents
        {
            OnCertificateValidated = context =>
            {
                var validationService =
                    context.HttpContext.RequestServices
                        .GetService<ICertificateValidationService>();
                
                if (validationService.ValidateCertificate(
                    context.ClientCertificate))
                {
                    var claims = new[]
                    {
                        new Claim(
                            ClaimTypes.NameIdentifier, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer),
                        new Claim(
                            ClaimTypes.Name, 
                            context.ClientCertificate.Subject, 
                            ClaimValueTypes.String, 
                            context.Options.ClaimsIssuer)
                    };

                    context.Principal = new ClaimsPrincipal(
                        new ClaimsIdentity(claims, context.Scheme.Name));
                    context.Success();
                }                     

                return Task.CompletedTask;
            }
        };
    });
```

<span data-ttu-id="db2f3-162">Koncepcyjnie sprawdzenie poprawności certyfikatu jest problemem z autoryzacją.</span><span class="sxs-lookup"><span data-stu-id="db2f3-162">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="db2f3-163">Dodanie kontroli, na przykład wystawcy lub odcisk palca w zasadach autoryzacji, a nie wewnątrz `OnCertificateValidated`, jest doskonale akceptowalne.</span><span class="sxs-lookup"><span data-stu-id="db2f3-163">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="db2f3-164">Konfigurowanie hosta tak, aby wymagał certyfikatów</span><span class="sxs-lookup"><span data-stu-id="db2f3-164">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="db2f3-165">Kestrel</span><span class="sxs-lookup"><span data-stu-id="db2f3-165">Kestrel</span></span>

<span data-ttu-id="db2f3-166">W *program.cs*Skonfiguruj Kestrel w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="db2f3-166">In *Program.cs*, configure Kestrel as follows:</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel(options =>
        {
            options.ConfigureHttpsDefaults(opt => 
                opt.ClientCertificateMode = 
                    ClientCertificateMode.RequireCertificate);
        })
        .Build();
```

### <a name="iis"></a><span data-ttu-id="db2f3-167">IIS</span><span class="sxs-lookup"><span data-stu-id="db2f3-167">IIS</span></span>

<span data-ttu-id="db2f3-168">Wykonaj następujące kroki w Menedżerze usług IIS:</span><span class="sxs-lookup"><span data-stu-id="db2f3-168">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="db2f3-169">Wybierz swoją witrynę z karty **połączenia** .</span><span class="sxs-lookup"><span data-stu-id="db2f3-169">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="db2f3-170">Kliknij dwukrotnie opcję **Ustawienia protokołu SSL** w oknie **Widok funkcji** .</span><span class="sxs-lookup"><span data-stu-id="db2f3-170">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="db2f3-171">Zaznacz pole wyboru **Wymagaj protokołu SSL** , a następnie wybierz przycisk **Wymagaj** przycisku radiowego w sekcji **Certyfikaty klienta** .</span><span class="sxs-lookup"><span data-stu-id="db2f3-171">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Ustawienia certyfikatu klienta w usługach IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="db2f3-173">Azure i niestandardowe serwery proxy sieci Web</span><span class="sxs-lookup"><span data-stu-id="db2f3-173">Azure and custom web proxies</span></span>

<span data-ttu-id="db2f3-174">Zapoznaj się z [dokumentacją hosta i Wdróż](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) , jak skonfigurować oprogramowanie pośredniczące przesyłania certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="db2f3-174">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
