---
title: Konfigurowanie uwierzytelniania certyfikatów w ASP.NET Core
author: blowdart
description: Dowiedz się, jak skonfigurować uwierzytelnianie certyfikatów w ASP.NET Core dla usług IIS i HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 01/02/2020
uid: security/authentication/certauth
ms.openlocfilehash: 9c175439c0313d62c75898f1af097774b06f353a
ms.sourcegitcommit: e7d4fe6727d423f905faaeaa312f6c25ef844047
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/02/2020
ms.locfileid: "75608148"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a><span data-ttu-id="c2a84-103">Konfigurowanie uwierzytelniania certyfikatów w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c2a84-103">Configure certificate authentication in ASP.NET Core</span></span>

<span data-ttu-id="c2a84-104">`Microsoft.AspNetCore.Authentication.Certificate` zawiera implementację podobną do [uwierzytelniania certyfikatu](https://tools.ietf.org/html/rfc5246#section-7.4.4) dla ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2a84-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="c2a84-105">Uwierzytelnianie certyfikatu odbywa się na poziomie protokołu TLS, o ile nie zostanie kiedykolwiek przeASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="c2a84-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="c2a84-106">Dokładniej, jest to procedura obsługi uwierzytelniania, która sprawdza poprawność certyfikatu, a następnie przekazuje zdarzenie, w którym można rozwiązać ten certyfikat do `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="c2a84-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="c2a84-107">[Skonfiguruj hosta](#configure-your-host-to-require-certificates) na potrzeby uwierzytelniania certyfikatów, to usługi IIS, Kestrel, Azure Web Apps lub inne, z których korzystasz.</span><span class="sxs-lookup"><span data-stu-id="c2a84-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="proxy-and-load-balancer-scenarios"></a><span data-ttu-id="c2a84-108">Scenariusze dotyczące serwerów proxy i równoważenia obciążenia</span><span class="sxs-lookup"><span data-stu-id="c2a84-108">Proxy and load balancer scenarios</span></span>

<span data-ttu-id="c2a84-109">Uwierzytelnianie certyfikatu jest scenariuszem stanowym głównie używanym w przypadku, gdy serwer proxy lub moduł równoważenia obciążenia nie obsługuje ruchu między klientami i serwerami.</span><span class="sxs-lookup"><span data-stu-id="c2a84-109">Certificate authentication is a stateful scenario primarily used where a proxy or load balancer doesn't handle traffic between clients and servers.</span></span> <span data-ttu-id="c2a84-110">Jeśli jest używany serwer proxy lub moduł równoważenia obciążenia, uwierzytelnianie certyfikatu działa tylko wtedy, gdy serwer proxy lub moduł równoważenia obciążenia:</span><span class="sxs-lookup"><span data-stu-id="c2a84-110">If a proxy or load balancer is used, certificate authentication only works if the proxy or load balancer:</span></span>

* <span data-ttu-id="c2a84-111">Obsługuje uwierzytelnianie.</span><span class="sxs-lookup"><span data-stu-id="c2a84-111">Handles the authentication.</span></span>
* <span data-ttu-id="c2a84-112">Przekazuje informacje o uwierzytelnianiu użytkownika do aplikacji (na przykład w nagłówku żądania), która działa na informacje o uwierzytelnianiu.</span><span class="sxs-lookup"><span data-stu-id="c2a84-112">Passes the user authentication information to the app (for example, in a request header), which acts on the authentication information.</span></span>

<span data-ttu-id="c2a84-113">Alternatywą dla uwierzytelniania certyfikatu w środowiskach, w których są używane serwery proxy i moduły równoważenia obciążenia, są Active Directory usług federacyjnych (AD FS) za pomocą OpenID Connect Connect (OIDC).</span><span class="sxs-lookup"><span data-stu-id="c2a84-113">An alternative to certificate authentication in environments where proxies and load balancers are used is Active Directory Federated Services (ADFS) with OpenID Connect (OIDC).</span></span>

## <a name="get-started"></a><span data-ttu-id="c2a84-114">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="c2a84-114">Get started</span></span>

<span data-ttu-id="c2a84-115">Uzyskaj certyfikat HTTPS, zastosuj go i [skonfiguruj hosta](#configure-your-host-to-require-certificates) , aby wymagał certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="c2a84-115">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="c2a84-116">W aplikacji sieci Web Dodaj odwołanie do pakietu `Microsoft.AspNetCore.Authentication.Certificate`.</span><span class="sxs-lookup"><span data-stu-id="c2a84-116">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="c2a84-117">Następnie w metodzie `Startup.ConfigureServices` Wywołaj `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` z opcjami, podając delegata `OnCertificateValidated` w celu przeprowadzenia wszelkich dodatkowych weryfikacji w certyfikacie klienta wysłanym z żądaniami.</span><span class="sxs-lookup"><span data-stu-id="c2a84-117">Then in the `Startup.ConfigureServices` method, call `services.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).AddCertificate(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="c2a84-118">Przełączaj te informacje do `ClaimsPrincipal` i ustaw je na właściwości `context.Principal`.</span><span class="sxs-lookup"><span data-stu-id="c2a84-118">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="c2a84-119">Jeśli uwierzytelnianie nie powiedzie się, ta procedura obsługi zwróci odpowiedź `403 (Forbidden)`, a nie `401 (Unauthorized)`, zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="c2a84-119">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="c2a84-120">Powodem jest to, że uwierzytelnianie powinno nastąpić podczas początkowego połączenia TLS.</span><span class="sxs-lookup"><span data-stu-id="c2a84-120">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="c2a84-121">Przez czas, gdy dociera do programu obsługi, jest zbyt opóźniony.</span><span class="sxs-lookup"><span data-stu-id="c2a84-121">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="c2a84-122">Nie ma możliwości uaktualnienia połączenia z anonimowego połączenia z certyfikatem.</span><span class="sxs-lookup"><span data-stu-id="c2a84-122">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="c2a84-123">Dodaj również `app.UseAuthentication();` w metodzie `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="c2a84-123">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="c2a84-124">W przeciwnym razie `HttpContext.User` nie zostanie ustawiona na `ClaimsPrincipal` utworzone na podstawie certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="c2a84-124">Otherwise, the `HttpContext.User` will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="c2a84-125">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="c2a84-125">For example:</span></span>

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

<span data-ttu-id="c2a84-126">W powyższym przykładzie przedstawiono domyślny sposób dodawania uwierzytelniania przy użyciu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="c2a84-126">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="c2a84-127">Program obsługi konstruuje jednostkę główną użytkownika przy użyciu wspólnych właściwości certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="c2a84-127">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="c2a84-128">Konfigurowanie weryfikacji certyfikatu</span><span class="sxs-lookup"><span data-stu-id="c2a84-128">Configure certificate validation</span></span>

<span data-ttu-id="c2a84-129">Procedura obsługi `CertificateAuthenticationOptions` ma pewne wbudowane walidacje, które są minimalnymi walidacjami, które należy wykonać na certyfikacie.</span><span class="sxs-lookup"><span data-stu-id="c2a84-129">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="c2a84-130">Każdy z tych ustawień jest domyślnie włączony.</span><span class="sxs-lookup"><span data-stu-id="c2a84-130">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="c2a84-131">AllowedCertificateTypes = łańcuchy, SelfSigned lub wszystkie (łańcuchowo | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="c2a84-131">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="c2a84-132">Wartość domyślna: `CertificateTypes.Chained`</span><span class="sxs-lookup"><span data-stu-id="c2a84-132">Default value: `CertificateTypes.Chained`</span></span>

<span data-ttu-id="c2a84-133">Ten test sprawdza, czy dozwolony jest tylko odpowiedni typ certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="c2a84-133">This check validates that only the appropriate certificate type is allowed.</span></span> <span data-ttu-id="c2a84-134">Jeśli aplikacja korzysta z certyfikatów z podpisem własnym, ta opcja musi być ustawiona na `CertificateTypes.All` lub `CertificateTypes.SelfSigned`.</span><span class="sxs-lookup"><span data-stu-id="c2a84-134">If the app is using self-signed certificates, this option needs to be set to `CertificateTypes.All` or `CertificateTypes.SelfSigned`.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="c2a84-135">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="c2a84-135">ValidateCertificateUse</span></span>

<span data-ttu-id="c2a84-136">Wartość domyślna: `true`</span><span class="sxs-lookup"><span data-stu-id="c2a84-136">Default value: `true`</span></span>

<span data-ttu-id="c2a84-137">Ten test sprawdza, czy certyfikat przedstawiony przez klienta ma rozszerzone użycie klucza uwierzytelniania klienta (EKU) lub nie rozszerzeń EKU w ogóle.</span><span class="sxs-lookup"><span data-stu-id="c2a84-137">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="c2a84-138">Zgodnie ze specyfikacją, jeśli nie określono rozszerzenia EKU, wszystkie rozszerzeń EKU są uznawane za prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="c2a84-138">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="c2a84-139">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="c2a84-139">ValidateValidityPeriod</span></span>

<span data-ttu-id="c2a84-140">Wartość domyślna: `true`</span><span class="sxs-lookup"><span data-stu-id="c2a84-140">Default value: `true`</span></span>

<span data-ttu-id="c2a84-141">Ten test sprawdza, czy certyfikat jest w jego okresie ważności.</span><span class="sxs-lookup"><span data-stu-id="c2a84-141">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="c2a84-142">W przypadku każdego żądania program obsługi zapewnia, że certyfikat, który był ważny, gdy był prezentowany, nie upłynął podczas bieżącej sesji.</span><span class="sxs-lookup"><span data-stu-id="c2a84-142">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="c2a84-143">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="c2a84-143">RevocationFlag</span></span>

<span data-ttu-id="c2a84-144">Wartość domyślna: `X509RevocationFlag.ExcludeRoot`</span><span class="sxs-lookup"><span data-stu-id="c2a84-144">Default value: `X509RevocationFlag.ExcludeRoot`</span></span>

<span data-ttu-id="c2a84-145">Flaga określająca, które certyfikaty w łańcuchu są sprawdzane pod kątem odwołania.</span><span class="sxs-lookup"><span data-stu-id="c2a84-145">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="c2a84-146">Sprawdzanie odwołań jest wykonywane tylko wtedy, gdy certyfikat jest powiązany z certyfikatem głównym.</span><span class="sxs-lookup"><span data-stu-id="c2a84-146">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="c2a84-147">Odwołaniemode</span><span class="sxs-lookup"><span data-stu-id="c2a84-147">RevocationMode</span></span>

<span data-ttu-id="c2a84-148">Wartość domyślna: `X509RevocationMode.Online`</span><span class="sxs-lookup"><span data-stu-id="c2a84-148">Default value: `X509RevocationMode.Online`</span></span>

<span data-ttu-id="c2a84-149">Flaga określająca sposób sprawdzania odwołania.</span><span class="sxs-lookup"><span data-stu-id="c2a84-149">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="c2a84-150">Określenie, że sprawdzanie online może skutkować długim opóźnieniem podczas kontaktowania się z urzędem certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="c2a84-150">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="c2a84-151">Sprawdzanie odwołań jest wykonywane tylko wtedy, gdy certyfikat jest powiązany z certyfikatem głównym.</span><span class="sxs-lookup"><span data-stu-id="c2a84-151">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="c2a84-152">Czy mogę skonfigurować aplikację, aby wymagać certyfikatu tylko w określonych ścieżkach?</span><span class="sxs-lookup"><span data-stu-id="c2a84-152">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="c2a84-153">Nie jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="c2a84-153">This isn't possible.</span></span> <span data-ttu-id="c2a84-154">Należy pamiętać, że wymiana certyfikatów jest wykonywana na początku konwersacji HTTPS, przez serwer przed odebraniem pierwszego żądania w ramach tego połączenia, aby nie było możliwe Określanie zakresu na podstawie pól żądania.</span><span class="sxs-lookup"><span data-stu-id="c2a84-154">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="c2a84-155">Zdarzenia programu obsługi</span><span class="sxs-lookup"><span data-stu-id="c2a84-155">Handler events</span></span>

<span data-ttu-id="c2a84-156">Program obsługi ma dwa zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="c2a84-156">The handler has two events:</span></span>

* <span data-ttu-id="c2a84-157">`OnAuthenticationFailed` &ndash; wywoływany, jeśli wystąpi wyjątek podczas uwierzytelniania i pozwala na reagowanie.</span><span class="sxs-lookup"><span data-stu-id="c2a84-157">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="c2a84-158">`OnCertificateValidated` &ndash; wywołane po sprawdzeniu poprawności certyfikatu, pomyślnie utworzono weryfikację i domyślny podmiot zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="c2a84-158">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="c2a84-159">To zdarzenie umożliwia wykonywanie własnych weryfikacji i rozszerzanie lub zastępowanie podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="c2a84-159">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="c2a84-160">Przykłady obejmują:</span><span class="sxs-lookup"><span data-stu-id="c2a84-160">For examples include:</span></span>
  * <span data-ttu-id="c2a84-161">Ustalanie, czy certyfikat jest znany dla usług.</span><span class="sxs-lookup"><span data-stu-id="c2a84-161">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="c2a84-162">Konstruowanie własnego podmiotu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="c2a84-162">Constructing your own principal.</span></span> <span data-ttu-id="c2a84-163">Rozważmy następujący przykład w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c2a84-163">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="c2a84-164">Jeśli okaże się, że certyfikat ruchu przychodzącego nie spełnia dodatkowej weryfikacji, wywołaj `context.Fail("failure reason")` z przyczyną niepowodzenia.</span><span class="sxs-lookup"><span data-stu-id="c2a84-164">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="c2a84-165">W przypadku rzeczywistej funkcjonalności prawdopodobnie chcesz wywołać usługę zarejestrowaną w iniekcji zależności, która łączy się z bazą danych lub innym typem magazynu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="c2a84-165">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="c2a84-166">Uzyskaj dostęp do usługi przy użyciu kontekstu przesłanego do obiektu delegowanego.</span><span class="sxs-lookup"><span data-stu-id="c2a84-166">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="c2a84-167">Rozważmy następujący przykład w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c2a84-167">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="c2a84-168">Koncepcyjnie sprawdzenie poprawności certyfikatu jest problemem z autoryzacją.</span><span class="sxs-lookup"><span data-stu-id="c2a84-168">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="c2a84-169">Dodanie kontroli, na przykład wystawcy lub odcisk palca w zasadach autoryzacji, a nie w `OnCertificateValidated`, jest doskonale akceptowalne.</span><span class="sxs-lookup"><span data-stu-id="c2a84-169">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="c2a84-170">Konfigurowanie hosta tak, aby wymagał certyfikatów</span><span class="sxs-lookup"><span data-stu-id="c2a84-170">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="c2a84-171">Kestrel</span><span class="sxs-lookup"><span data-stu-id="c2a84-171">Kestrel</span></span>

<span data-ttu-id="c2a84-172">W *program.cs*Skonfiguruj Kestrel w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="c2a84-172">In *Program.cs*, configure Kestrel as follows:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateHostBuilder(args).Build().Run();
}

public static IHostBuilder CreateHostBuilder(string[] args)
{
    return Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
            webBuilder.ConfigureKestrel(o =>
            {
                o.ConfigureHttpsDefaults(o => 
            o.ClientCertificateMode = 
                ClientCertificateMode.RequireCertificate);
            });
        });
}
```

> [!NOTE]
> <span data-ttu-id="c2a84-173">Punkty końcowe utworzone przez wywołanie <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **przed** wywołaniem <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> nie będą miały zastosowania wartości domyślnych.</span><span class="sxs-lookup"><span data-stu-id="c2a84-173">Endpoints created by calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> **before** calling <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ConfigureHttpsDefaults*> won't have the defaults applied.</span></span>

### <a name="iis"></a><span data-ttu-id="c2a84-174">IIS</span><span class="sxs-lookup"><span data-stu-id="c2a84-174">IIS</span></span>

<span data-ttu-id="c2a84-175">Wykonaj następujące kroki w Menedżerze usług IIS:</span><span class="sxs-lookup"><span data-stu-id="c2a84-175">Complete the following steps in IIS Manager:</span></span>

1. <span data-ttu-id="c2a84-176">Wybierz swoją witrynę z karty **połączenia** .</span><span class="sxs-lookup"><span data-stu-id="c2a84-176">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="c2a84-177">Kliknij dwukrotnie opcję **Ustawienia protokołu SSL** w oknie **Widok funkcji** .</span><span class="sxs-lookup"><span data-stu-id="c2a84-177">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="c2a84-178">Zaznacz pole wyboru **Wymagaj protokołu SSL** , a następnie wybierz przycisk **Wymagaj** przycisku radiowego w sekcji **Certyfikaty klienta** .</span><span class="sxs-lookup"><span data-stu-id="c2a84-178">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Ustawienia certyfikatu klienta w usługach IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="c2a84-180">Azure i niestandardowe serwery proxy sieci Web</span><span class="sxs-lookup"><span data-stu-id="c2a84-180">Azure and custom web proxies</span></span>

<span data-ttu-id="c2a84-181">Zapoznaj się z [dokumentacją hosta i Wdróż](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) , jak skonfigurować oprogramowanie pośredniczące przesyłania certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="c2a84-181">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>

### <a name="use-certificate-authentication-in-azure-web-apps"></a><span data-ttu-id="c2a84-182">Używanie uwierzytelniania certyfikatów na platformie Azure Web Apps</span><span class="sxs-lookup"><span data-stu-id="c2a84-182">Use certificate authentication in Azure Web Apps</span></span>

<span data-ttu-id="c2a84-183">Metoda `AddCertificateForwarding` służy do określenia:</span><span class="sxs-lookup"><span data-stu-id="c2a84-183">The `AddCertificateForwarding` method is used to specify:</span></span>

* <span data-ttu-id="c2a84-184">Nazwa nagłówka klienta.</span><span class="sxs-lookup"><span data-stu-id="c2a84-184">The client header name.</span></span>
* <span data-ttu-id="c2a84-185">Sposób ładowania certyfikatu (przy użyciu właściwości `HeaderConverter`).</span><span class="sxs-lookup"><span data-stu-id="c2a84-185">How the certificate is to be loaded (using the `HeaderConverter` property).</span></span>

<span data-ttu-id="c2a84-186">W usłudze Azure Web Apps certyfikat jest przenoszona jako niestandardowy nagłówek żądania o nazwie `X-ARR-ClientCert`.</span><span class="sxs-lookup"><span data-stu-id="c2a84-186">In Azure Web Apps, the certificate is passed as a custom request header named `X-ARR-ClientCert`.</span></span> <span data-ttu-id="c2a84-187">W tym celu należy skonfigurować przekazywanie certyfikatów w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c2a84-187">To use it, configure certificate forwarding in `Startup.ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddCertificateForwarding(options =>
    {
        options.CertificateHeader = "X-ARR-ClientCert";
        options.HeaderConverter = (headerValue) =>
        {
            X509Certificate2 clientCertificate = null;
        
            if(!string.IsNullOrWhiteSpace(headerValue))
            {
                byte[] bytes = StringToByteArray(headerValue);
                clientCertificate = new X509Certificate2(bytes);
            }

            return clientCertificate;
        };
    });
}

private static byte[] StringToByteArray(string hex)
{
    int NumberChars = hex.Length;
    byte[] bytes = new byte[NumberChars / 2];

    for (int i = 0; i < NumberChars; i += 2)
    {
        bytes[i / 2] = Convert.ToByte(hex.Substring(i, 2), 16);
    }

    return bytes;
}
```

<span data-ttu-id="c2a84-188">Następnie Metoda `Startup.Configure` dodaje oprogramowanie pośredniczące.</span><span class="sxs-lookup"><span data-stu-id="c2a84-188">The `Startup.Configure` method then adds the middleware.</span></span> <span data-ttu-id="c2a84-189">`UseCertificateForwarding` jest wywoływana przed wywołaniami `UseAuthentication` i `UseAuthorization`:</span><span class="sxs-lookup"><span data-stu-id="c2a84-189">`UseCertificateForwarding` is called before the calls to `UseAuthentication` and `UseAuthorization`:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    ...

    app.UseRouting();

    app.UseCertificateForwarding();
    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });
}
```

<span data-ttu-id="c2a84-190">Oddzielna Klasa może służyć do implementowania logiki walidacji.</span><span class="sxs-lookup"><span data-stu-id="c2a84-190">A separate class can be used to implement validation logic.</span></span> <span data-ttu-id="c2a84-191">Ponieważ ten sam certyfikat z podpisem własnym jest używany w tym przykładzie, należy się upewnić, że można używać tylko certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="c2a84-191">Because the same self-signed certificate is used in this example, ensure that only your certificate can be used.</span></span> <span data-ttu-id="c2a84-192">Sprawdź, czy odciski palca zarówno certyfikatu klienta, jak i certyfikatu serwera, w przeciwnym razie można użyć dowolnego certyfikatu i będzie on wystarczający do uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="c2a84-192">Validate that the thumbprints of both the client certificate and the server certificate match, otherwise any certificate can be used and will be enough to authenticate.</span></span> <span data-ttu-id="c2a84-193">Będzie on używany wewnątrz metody `AddCertificate`.</span><span class="sxs-lookup"><span data-stu-id="c2a84-193">This would be used inside the `AddCertificate` method.</span></span> <span data-ttu-id="c2a84-194">W przypadku korzystania z certyfikatów pośrednich lub podrzędnych można także zweryfikować temat lub wystawcę w tym miejscu.</span><span class="sxs-lookup"><span data-stu-id="c2a84-194">You could also validate the subject or the issuer here if you're using intermediate or child certificates.</span></span>

```csharp
using System.IO;
using System.Security.Cryptography.X509Certificates;

namespace AspNetCoreCertificateAuthApi
{
    public class MyCertificateValidationService
    {
        public bool ValidateCertificate(X509Certificate2 clientCertificate)
        {
            // Do not hardcode passwords in production code
            // Use thumbprint or key vault
            var cert = new X509Certificate2(
                Path.Combine("sts_dev_cert.pfx"), "1234");

            if (clientCertificate.Thumbprint == cert.Thumbprint)
            {
                return true;
            }

            return false;
        }
    }
}
```

#### <a name="implement-an-httpclient-using-a-certificate"></a><span data-ttu-id="c2a84-195">Implementowanie HttpClient przy użyciu certyfikatu</span><span class="sxs-lookup"><span data-stu-id="c2a84-195">Implement an HttpClient using a certificate</span></span>

<span data-ttu-id="c2a84-196">Klient internetowego interfejsu API używa `HttpClient`, który został utworzony przy użyciu wystąpienia `IHttpClientFactory`.</span><span class="sxs-lookup"><span data-stu-id="c2a84-196">The web API client uses an `HttpClient`, which was created using an `IHttpClientFactory` instance.</span></span> <span data-ttu-id="c2a84-197">To nie pozwala na zdefiniowanie programu obsługi dla `HttpClient`, dlatego użyj `HttpRequestMessage`, aby dodać certyfikat do nagłówka żądania `X-ARR-ClientCert`.</span><span class="sxs-lookup"><span data-stu-id="c2a84-197">This doesn't provide a way to define a handler for the `HttpClient`, so use an `HttpRequestMessage` to add the certificate to the `X-ARR-ClientCert` request header.</span></span> <span data-ttu-id="c2a84-198">Certyfikat jest dodawany jako ciąg za pomocą metody `GetRawCertDataString`.</span><span class="sxs-lookup"><span data-stu-id="c2a84-198">The certificate is added as a string using the `GetRawCertDataString` method.</span></span> 

```csharp
private async Task<JsonDocument> GetApiDataAsync()
{
    try
    {
        // Do not hardcode passwords in production code
        // Use thumbprint or key vault
        var cert = new X509Certificate2(
            Path.Combine(_environment.ContentRootPath, 
                "sts_dev_cert.pfx"), "1234");
        var client = _clientFactory.CreateClient();
        var request = new HttpRequestMessage()
        {
            RequestUri = new Uri("https://localhost:44379/api/values"),
            Method = HttpMethod.Get,
        };

        request.Headers.Add("X-ARR-ClientCert", cert.GetRawCertDataString());
        var response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            var responseContent = await response.Content.ReadAsStringAsync();
            var data = JsonDocument.Parse(responseContent);

            return data;
        }

        throw new ApplicationException(
            $"Status code: {response.StatusCode}, " +
            $"Error: {response.ReasonPhrase}");
    }
    catch (Exception e)
    {
        throw new ApplicationException($"Exception {e}");
    }
}
```

<span data-ttu-id="c2a84-199">Jeśli do serwera zostanie wysłany prawidłowy certyfikat, dane są zwracane.</span><span class="sxs-lookup"><span data-stu-id="c2a84-199">If the correct certificate is sent to the server, the data is returned.</span></span> <span data-ttu-id="c2a84-200">Jeśli certyfikat lub nieprawidłowy certyfikat nie zostanie wysłany, zwracany jest kod stanu HTTP 403.</span><span class="sxs-lookup"><span data-stu-id="c2a84-200">If no certificate or the wrong certificate is sent, an HTTP 403 status code is returned.</span></span>

### <a name="create-certificates-in-powershell"></a><span data-ttu-id="c2a84-201">Tworzenie certyfikatów w programie PowerShell</span><span class="sxs-lookup"><span data-stu-id="c2a84-201">Create certificates in PowerShell</span></span>

<span data-ttu-id="c2a84-202">Tworzenie certyfikatów jest najtrudniejszą częścią w konfigurowaniu tego przepływu.</span><span class="sxs-lookup"><span data-stu-id="c2a84-202">Creating the certificates is the hardest part in setting up this flow.</span></span> <span data-ttu-id="c2a84-203">Certyfikat główny można utworzyć za pomocą polecenia cmdlet programu `New-SelfSignedCertificate` PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c2a84-203">A root certificate can be created using the `New-SelfSignedCertificate` PowerShell cmdlet.</span></span> <span data-ttu-id="c2a84-204">Podczas tworzenia certyfikatu należy użyć silnego hasła.</span><span class="sxs-lookup"><span data-stu-id="c2a84-204">When creating the certificate, use a strong password.</span></span> <span data-ttu-id="c2a84-205">Ważne jest, aby dodać parametr `KeyUsageProperty` i parametr `KeyUsage`, jak pokazano.</span><span class="sxs-lookup"><span data-stu-id="c2a84-205">It's important to add the `KeyUsageProperty` parameter and the `KeyUsage` parameter as shown.</span></span>

#### <a name="create-root-ca"></a><span data-ttu-id="c2a84-206">Utwórz główny urząd certyfikacji</span><span class="sxs-lookup"><span data-stu-id="c2a84-206">Create root CA</span></span>

```powershell
New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath root_ca_dev_damienbod.crt
```

> [!NOTE]
> <span data-ttu-id="c2a84-207">Wartość parametru `-DnsName` musi być zgodna z elementem docelowym wdrożenia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="c2a84-207">The `-DnsName` parameter value must match the deployment target of the app.</span></span> <span data-ttu-id="c2a84-208">Na przykład "localhost" do programowania.</span><span class="sxs-lookup"><span data-stu-id="c2a84-208">For example, "localhost" for development.</span></span>

#### <a name="install-in-the-trusted-root"></a><span data-ttu-id="c2a84-209">Zainstaluj w zaufanym katalogu głównym</span><span class="sxs-lookup"><span data-stu-id="c2a84-209">Install in the trusted root</span></span>

<span data-ttu-id="c2a84-210">Certyfikat główny musi być zaufany w systemie hosta.</span><span class="sxs-lookup"><span data-stu-id="c2a84-210">The root certificate needs to be trusted on your host system.</span></span> <span data-ttu-id="c2a84-211">Certyfikat główny, który nie został utworzony przez urząd certyfikacji, nie będzie domyślnie zaufany.</span><span class="sxs-lookup"><span data-stu-id="c2a84-211">A root certificate which was not created by a certificate authority won't be trusted by default.</span></span> <span data-ttu-id="c2a84-212">Poniższy link wyjaśnia, jak można to zrobić w systemie Windows:</span><span class="sxs-lookup"><span data-stu-id="c2a84-212">The following link explains how this can be accomplished on Windows:</span></span>

https://social.msdn.microsoft.com/Forums/SqlServer/5ed119ef-1704-4be4-8a4f-ef11de7c8f34/a-certificate-chain-processed-but-terminated-in-a-root-certificate-which-is-not-trusted-by-the

#### <a name="intermediate-certificate"></a><span data-ttu-id="c2a84-213">Certyfikat pośredni</span><span class="sxs-lookup"><span data-stu-id="c2a84-213">Intermediate certificate</span></span>

<span data-ttu-id="c2a84-214">Certyfikat pośredni można teraz utworzyć na podstawie certyfikatu głównego.</span><span class="sxs-lookup"><span data-stu-id="c2a84-214">An intermediate certificate can now be created from the root certificate.</span></span> <span data-ttu-id="c2a84-215">Nie jest to wymagane w przypadku wszystkich przypadków użycia, ale może być konieczne utworzenie wielu certyfikatów lub konieczność aktywowania lub wyłączenia grup certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="c2a84-215">This isn't required for all use cases, but you might need to create many certificates or need to activate or disable groups of certificates.</span></span> <span data-ttu-id="c2a84-216">Parametr `TextExtension` jest wymagany do ustawienia długości ścieżki w podstawowych ograniczeniach certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="c2a84-216">The `TextExtension` parameter is required to set the path length in the basic constraints of the certificate.</span></span>

<span data-ttu-id="c2a84-217">Certyfikat pośredni można następnie dodać do zaufanego certyfikatu pośredniego w systemie hosta systemu Windows.</span><span class="sxs-lookup"><span data-stu-id="c2a84-217">The intermediate certificate can then be added to the trusted intermediate certificate in the Windows host system.</span></span>

```powershell
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint of the root..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "intermediate_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "intermediate_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\intermediate_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath intermediate_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-intermediate-certificate"></a><span data-ttu-id="c2a84-218">Utwórz certyfikat podrzędny z certyfikatu pośredniego</span><span class="sxs-lookup"><span data-stu-id="c2a84-218">Create child certificate from intermediate certificate</span></span>

<span data-ttu-id="c2a84-219">Certyfikat podrzędny można utworzyć na podstawie certyfikatu pośredniego.</span><span class="sxs-lookup"><span data-stu-id="c2a84-219">A child certificate can be created from the intermediate certificate.</span></span> <span data-ttu-id="c2a84-220">Jest to jednostka końcowa i nie trzeba tworzyć więcej certyfikatów podrzędnych.</span><span class="sxs-lookup"><span data-stu-id="c2a84-220">This is the end entity and doesn't need to create more child certificates.</span></span>

```powershell
$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the Intermediate certificate..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="create-child-certificate-from-root-certificate"></a><span data-ttu-id="c2a84-221">Utwórz certyfikat podrzędny z certyfikatu głównego</span><span class="sxs-lookup"><span data-stu-id="c2a84-221">Create child certificate from root certificate</span></span>

<span data-ttu-id="c2a84-222">Certyfikat podrzędny można także utworzyć bezpośrednio na podstawie certyfikatu głównego.</span><span class="sxs-lookup"><span data-stu-id="c2a84-222">A child certificate can also be created from the root certificate directly.</span></span> 

```powershell
$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\"The thumbprint from the root cert..." )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com"

$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

Get-ChildItem -Path cert:\localMachine\my\"The thumbprint..." | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\"The thumbprint..." -FilePath child_a_dev_damienbod.crt
```

#### <a name="example-root---intermediate-certificate---certificate"></a><span data-ttu-id="c2a84-223">Przykład certyfikatu pośredniego głównego — certyfikat</span><span class="sxs-lookup"><span data-stu-id="c2a84-223">Example root - intermediate certificate - certificate</span></span>

```powershell
$mypwdroot = ConvertTo-SecureString -String "1234" -Force -AsPlainText
$mypwd = ConvertTo-SecureString -String "1234" -Force -AsPlainText

New-SelfSignedCertificate -DnsName "root_ca_dev_damienbod.com", "root_ca_dev_damienbod.com" -CertStoreLocation "cert:\LocalMachine\My" -NotAfter (Get-Date).AddYears(20) -FriendlyName "root_ca_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature

Get-ChildItem -Path cert:\localMachine\my\0C89639E4E2998A93E423F919B36D4009A0F9991 | Export-PfxCertificate -FilePath C:\git\root_ca_dev_damienbod.pfx -Password $mypwdroot

Export-Certificate -Cert cert:\localMachine\my\0C89639E4E2998A93E423F919B36D4009A0F9991 -FilePath root_ca_dev_damienbod.crt

$rootcert = ( Get-ChildItem -Path cert:\LocalMachine\My\0C89639E4E2998A93E423F919B36D4009A0F9991 )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_a_dev_damienbod.com" -Signer $rootcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_a_dev_damienbod.com" -KeyUsageProperty All -KeyUsage CertSign, CRLSign, DigitalSignature -TextExtension @("2.5.29.19={text}CA=1&pathlength=1")

Get-ChildItem -Path cert:\localMachine\my\BA9BF91ED35538A01375EFC212A2F46104B33A44 | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\BA9BF91ED35538A01375EFC212A2F46104B33A44 -FilePath child_a_dev_damienbod.crt

$parentcert = ( Get-ChildItem -Path cert:\LocalMachine\My\BA9BF91ED35538A01375EFC212A2F46104B33A44 )

New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "child_b_from_a_dev_damienbod.com" -Signer $parentcert -NotAfter (Get-Date).AddYears(20) -FriendlyName "child_b_from_a_dev_damienbod.com" 

Get-ChildItem -Path cert:\localMachine\my\141594A0AE38CBBECED7AF680F7945CD51D8F28A | Export-PfxCertificate -FilePath C:\git\AspNetCoreCertificateAuth\Certs\child_b_from_a_dev_damienbod.pfx -Password $mypwd

Export-Certificate -Cert cert:\localMachine\my\141594A0AE38CBBECED7AF680F7945CD51D8F28A -FilePath child_b_from_a_dev_damienbod.crt
```

<span data-ttu-id="c2a84-224">W przypadku korzystania z certyfikatów głównych, pośrednich lub podrzędnych certyfikaty można zweryfikować przy użyciu odcisku palca lub PublicKey zgodnie z wymaganiami.</span><span class="sxs-lookup"><span data-stu-id="c2a84-224">When using the root, intermediate, or child certificates, the certificates can be validated using the Thumbprint or PublicKey as required.</span></span>

```csharp
using System.Collections.Generic;
using System.IO;
using System.Security.Cryptography.X509Certificates;

namespace AspNetCoreCertificateAuthApi
{
    public class MyCertificateValidationService 
    {
        public bool ValidateCertificate(X509Certificate2 clientCertificate)
        {
            return CheckIfThumbprintIsValid(clientCertificate);
        }

        private bool CheckIfThumbprintIsValid(X509Certificate2 clientCertificate)
        {
            var listOfValidThumbprints = new List<string>
            {
                "141594A0AE38CBBECED7AF680F7945CD51D8F28A",
                "0C89639E4E2998A93E423F919B36D4009A0F9991",
                "BA9BF91ED35538A01375EFC212A2F46104B33A44"
            };

            if (listOfValidThumbprints.Contains(clientCertificate.Thumbprint))
            {
                return true;
            }

            return false;
        }
    }
}
```
