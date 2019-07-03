---
title: Konfigurowanie uwierzytelniania certyfikatów w programie ASP.NET Core
author: blowdart
description: Dowiedz się, jak skonfigurować uwierzytelnianie certyfikatu w programie ASP.NET Core dla usług IIS i sterownik HTTP.sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 06/11/2019
uid: security/authentication/certauth
ms.openlocfilehash: 8609c58265340da1d618135795915d6c49e750a3
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538721"
---
# <a name="overview"></a><span data-ttu-id="75f64-103">Omówienie</span><span class="sxs-lookup"><span data-stu-id="75f64-103">Overview</span></span>

<span data-ttu-id="75f64-104">`Microsoft.AspNetCore.Authentication.Certificate` Podobnie jak implementację [uwierzytelnianie certyfikatu](https://tools.ietf.org/html/rfc5246#section-7.4.4) dla platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75f64-104">`Microsoft.AspNetCore.Authentication.Certificate` contains an implementation similar to [Certificate Authentication](https://tools.ietf.org/html/rfc5246#section-7.4.4) for ASP.NET Core.</span></span> <span data-ttu-id="75f64-105">Uwierzytelnianie certyfikatu odbywa się na poziomie protokołu TLS, long, zanim stanie się kiedykolwiek do platformy ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="75f64-105">Certificate authentication happens at the TLS level, long before it ever gets to ASP.NET Core.</span></span> <span data-ttu-id="75f64-106">Dokładniej mówiąc, jest to program obsługi uwierzytelniania, która sprawdza poprawność certyfikatu i proponuje następnie zdarzenie, w którym można rozwiązać tego certyfikatu do `ClaimsPrincipal`.</span><span class="sxs-lookup"><span data-stu-id="75f64-106">More accurately, this is an authentication handler that validates the certificate and then gives you an event where you can resolve that certificate to a `ClaimsPrincipal`.</span></span> 

<span data-ttu-id="75f64-107">[Konfigurowanie hosta](#configure-your-host-to-require-certificates) uwierzytelniania certyfikatu, są to usługi IIS, Kestrel, Azure Web Apps lub cokolwiek innego używasz.</span><span class="sxs-lookup"><span data-stu-id="75f64-107">[Configure your host](#configure-your-host-to-require-certificates) for certificate authentication, be it IIS, Kestrel, Azure Web Apps, or whatever else you're using.</span></span>

## <a name="get-started"></a><span data-ttu-id="75f64-108">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="75f64-108">Get started</span></span>

<span data-ttu-id="75f64-109">Uzyskać certyfikat HTTPS, zastosuj go, a [skonfigurować hosta](#configure-your-host-to-require-certificates) wymagające certyfikatów.</span><span class="sxs-lookup"><span data-stu-id="75f64-109">Acquire an HTTPS certificate, apply it, and [configure your host](#configure-your-host-to-require-certificates) to require certificates.</span></span>

<span data-ttu-id="75f64-110">W aplikacji sieci web Dodaj odwołanie do `Microsoft.AspNetCore.Authentication.Certificate` pakietu.</span><span class="sxs-lookup"><span data-stu-id="75f64-110">In your web app, add a reference to the `Microsoft.AspNetCore.Authentication.Certificate` package.</span></span> <span data-ttu-id="75f64-111">Następnie w `Startup.Configure` metody, wywołanie `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` przy użyciu opcji, dostarczenie delegata dla `OnCertificateValidated` celu wszelkie dodatkowe sprawdzanie poprawności certyfikatu klienta wysłanego z żądaniami.</span><span class="sxs-lookup"><span data-stu-id="75f64-111">Then in the `Startup.Configure` method, call `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` with your options, providing a delegate for `OnCertificateValidated` to do any supplementary validation on the client certificate sent with requests.</span></span> <span data-ttu-id="75f64-112">Włącz te informacje do `ClaimsPrincipal` i ustaw ją na `context.Principal` właściwości.</span><span class="sxs-lookup"><span data-stu-id="75f64-112">Turn that information into a `ClaimsPrincipal` and set it on the `context.Principal` property.</span></span>

<span data-ttu-id="75f64-113">Jeśli uwierzytelnianie nie powiedzie się, zwraca ten program obsługi `403 (Forbidden)` odpowiedzi zamiast `401 (Unauthorized)`, zgodnie z oczekiwaniami.</span><span class="sxs-lookup"><span data-stu-id="75f64-113">If authentication fails, this handler returns a `403 (Forbidden)` response rather a `401 (Unauthorized)`, as you might expect.</span></span> <span data-ttu-id="75f64-114">Przyczyny, dla których jest to, czy uwierzytelnienie ma się zdarzyć podczas pierwszego połączenia TLS.</span><span class="sxs-lookup"><span data-stu-id="75f64-114">The reasoning is that the authentication should happen during the initial TLS connection.</span></span> <span data-ttu-id="75f64-115">Przez razem, gdy zostanie osiągnięty, program obsługi jest za późno.</span><span class="sxs-lookup"><span data-stu-id="75f64-115">By the time it reaches the handler, it's too late.</span></span> <span data-ttu-id="75f64-116">Nie ma możliwości do połączenia z połączeniem anonimowym odpowiednią aktualizację przy użyciu certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="75f64-116">There's no way to upgrade the connection from an anonymous connection to one with a certificate.</span></span>

<span data-ttu-id="75f64-117">Również dodać `app.UseAuthentication();` w `Startup.Configure` metody.</span><span class="sxs-lookup"><span data-stu-id="75f64-117">Also add `app.UseAuthentication();` in the `Startup.Configure` method.</span></span> <span data-ttu-id="75f64-118">W przeciwnym razie HttpContext.User nie zostanie ustawiona, `ClaimsPrincipal` utworzone na podstawie certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="75f64-118">Otherwise, the HttpContext.User will not be set to `ClaimsPrincipal` created from the certificate.</span></span> <span data-ttu-id="75f64-119">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="75f64-119">For example:</span></span>

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

<span data-ttu-id="75f64-120">Poprzedni przykład pokazuje sposób domyślne, aby dodać certyfikat uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="75f64-120">The preceding example demonstrates the default way to add certificate authentication.</span></span> <span data-ttu-id="75f64-121">Program obsługi tworzy jednostki użytkownika, przy użyciu wspólnej właściwości certyfikatu.</span><span class="sxs-lookup"><span data-stu-id="75f64-121">The handler constructs a user principal using the common certificate properties.</span></span>

## <a name="configure-certificate-validation"></a><span data-ttu-id="75f64-122">Skonfiguruj weryfikację certyfikatu</span><span class="sxs-lookup"><span data-stu-id="75f64-122">Configure certificate validation</span></span>

<span data-ttu-id="75f64-123">`CertificateAuthenticationOptions` Program obsługi ma kilka wbudowanych sprawdzanie poprawności, które są minimalne operacji sprawdzania poprawności, należy wykonać na certyfikacie.</span><span class="sxs-lookup"><span data-stu-id="75f64-123">The `CertificateAuthenticationOptions` handler has some built-in validations that are the minimum validations you should perform on a certificate.</span></span> <span data-ttu-id="75f64-124">Każde z tych ustawień jest domyślnie włączona.</span><span class="sxs-lookup"><span data-stu-id="75f64-124">Each of these settings is enabled by default.</span></span>

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a><span data-ttu-id="75f64-125">AllowedCertificateTypes = łańcuchowa SelfSigned lub wszystkich (łańcuchowych | SelfSigned)</span><span class="sxs-lookup"><span data-stu-id="75f64-125">AllowedCertificateTypes = Chained, SelfSigned, or All (Chained | SelfSigned)</span></span>

<span data-ttu-id="75f64-126">Ten test weryfikuje, czy jest dozwolony tylko typ odpowiedni certyfikat.</span><span class="sxs-lookup"><span data-stu-id="75f64-126">This check validates that only the appropriate certificate type is allowed.</span></span>

### <a name="validatecertificateuse"></a><span data-ttu-id="75f64-127">ValidateCertificateUse</span><span class="sxs-lookup"><span data-stu-id="75f64-127">ValidateCertificateUse</span></span>

<span data-ttu-id="75f64-128">Ten test weryfikuje, czy certyfikat przedstawiony przez klienta ma uwierzytelniania klienta na wszystkich rozszerzone użycia klucza (EKU) lub brak rozszerzeń EKU.</span><span class="sxs-lookup"><span data-stu-id="75f64-128">This check validates that the certificate presented by the client has the Client Authentication extended key use (EKU), or no EKUs at all.</span></span> <span data-ttu-id="75f64-129">Ponieważ specyfikacji powiedzieć, jeśli nie określono żadnych rozszerzenia EKU, następnie wszystkich wartości EKU uznaje za prawidłowe.</span><span class="sxs-lookup"><span data-stu-id="75f64-129">As the specifications say, if no EKU is specified, then all EKUs are deemed valid.</span></span>

### <a name="validatevalidityperiod"></a><span data-ttu-id="75f64-130">ValidateValidityPeriod</span><span class="sxs-lookup"><span data-stu-id="75f64-130">ValidateValidityPeriod</span></span>

<span data-ttu-id="75f64-131">Ten test weryfikuje, czy certyfikat jest okresem ważności.</span><span class="sxs-lookup"><span data-stu-id="75f64-131">This check validates that the certificate is within its validity period.</span></span> <span data-ttu-id="75f64-132">Na każde żądanie obsługi gwarantuje, że certyfikat, który była prawidłowa, gdy go został przedstawiony nie upłynął podczas bieżącej sesji.</span><span class="sxs-lookup"><span data-stu-id="75f64-132">On each request, the handler ensures that a certificate that was valid when it was presented hasn't expired during its current session.</span></span>

### <a name="revocationflag"></a><span data-ttu-id="75f64-133">RevocationFlag</span><span class="sxs-lookup"><span data-stu-id="75f64-133">RevocationFlag</span></span>

<span data-ttu-id="75f64-134">Flaga określająca o certyfikatach w łańcuchu są sprawdzane pod kątem odwołań.</span><span class="sxs-lookup"><span data-stu-id="75f64-134">A flag that specifies which certificates in the chain are checked for revocation.</span></span>

<span data-ttu-id="75f64-135">Sprawdzanie odwołań są wykonywane tylko w sytuacji, gdy certyfikat jest powiązany z certyfikatem głównym.</span><span class="sxs-lookup"><span data-stu-id="75f64-135">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="revocationmode"></a><span data-ttu-id="75f64-136">revocationMode</span><span class="sxs-lookup"><span data-stu-id="75f64-136">RevocationMode</span></span>

<span data-ttu-id="75f64-137">Flaga określająca sposób odwołania są sprawdzane.</span><span class="sxs-lookup"><span data-stu-id="75f64-137">A flag that specifies how revocation checks are performed.</span></span>

<span data-ttu-id="75f64-138">Określanie kontroli w trybie online może spowodować duże opóźnienie podczas skontaktować się z urzędu certyfikacji.</span><span class="sxs-lookup"><span data-stu-id="75f64-138">Specifying an online check can result in a long delay while the certificate authority is contacted.</span></span>

<span data-ttu-id="75f64-139">Sprawdzanie odwołań są wykonywane tylko w sytuacji, gdy certyfikat jest powiązany z certyfikatem głównym.</span><span class="sxs-lookup"><span data-stu-id="75f64-139">Revocation checks are only performed when the certificate is chained to a root certificate.</span></span>

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a><span data-ttu-id="75f64-140">Można skonfigurować aplikację, aby wymagają certyfikatu tylko na określone ścieżki?</span><span class="sxs-lookup"><span data-stu-id="75f64-140">Can I configure my app to require a certificate only on certain paths?</span></span>

<span data-ttu-id="75f64-141">Nie jest to możliwe.</span><span class="sxs-lookup"><span data-stu-id="75f64-141">This isn't possible.</span></span> <span data-ttu-id="75f64-142">Pamiętaj, że wymiany certyfikatu odbywa się czy początek komunikacji HTTPS to się robi przez serwer przed otrzymaniem pierwsze żądanie dla tego połączenia, więc nie jest możliwe do zakresu na podstawie pól wszelkie żądania.</span><span class="sxs-lookup"><span data-stu-id="75f64-142">Remember the certificate exchange is done that the start of the HTTPS conversation, it's done by the server before the first request is received on that connection so it's not possible to scope based on any request fields.</span></span>

## <a name="handler-events"></a><span data-ttu-id="75f64-143">Program obsługi zdarzeń</span><span class="sxs-lookup"><span data-stu-id="75f64-143">Handler events</span></span>

<span data-ttu-id="75f64-144">Program obsługi ma dwa zdarzenia:</span><span class="sxs-lookup"><span data-stu-id="75f64-144">The handler has two events:</span></span>

* <span data-ttu-id="75f64-145">`OnAuthenticationFailed` &ndash; Wywoływane, jeśli wyjątek będzie się działo podczas uwierzytelniania i pozwala reagować.</span><span class="sxs-lookup"><span data-stu-id="75f64-145">`OnAuthenticationFailed` &ndash; Called if an exception happens during authentication and allows you to react.</span></span>
* <span data-ttu-id="75f64-146">`OnCertificateValidated` &ndash; Wywołuje się, gdy certyfikat został zweryfikowany przeszły sprawdzanie poprawności i domyślne podmiot zabezpieczeń został utworzony.</span><span class="sxs-lookup"><span data-stu-id="75f64-146">`OnCertificateValidated` &ndash; Called after the certificate has been validated, passed validation and a default principal has been created.</span></span> <span data-ttu-id="75f64-147">To zdarzenie umożliwia należy przeprowadzić własne poprawności i rozszerzyć lub Zastąp podmiot zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="75f64-147">This event allows you to perform your own validation and augment or replace the principal.</span></span> <span data-ttu-id="75f64-148">Przykłady obejmują:</span><span class="sxs-lookup"><span data-stu-id="75f64-148">For examples include:</span></span>
  * <span data-ttu-id="75f64-149">Określanie, jeśli certyfikat jest znane usługi.</span><span class="sxs-lookup"><span data-stu-id="75f64-149">Determining if the certificate is known to your services.</span></span>
  * <span data-ttu-id="75f64-150">Konstruowanie własne jednostki.</span><span class="sxs-lookup"><span data-stu-id="75f64-150">Constructing your own principal.</span></span> <span data-ttu-id="75f64-151">Rozważmy następujący przykład w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="75f64-151">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="75f64-152">Jeśli okaże się ruchu przychodzącego certyfikat nie spełnia wymagań Twojej dodatkową walidację, wywołaj `context.Fail("failure reason")` wraz z przyczyną niepowodzenia.</span><span class="sxs-lookup"><span data-stu-id="75f64-152">If you find the inbound certificate doesn't meet your extra validation, call `context.Fail("failure reason")` with a failure reason.</span></span>

<span data-ttu-id="75f64-153">Rzeczywiste funkcjonalności, prawdopodobnie należy wywołać usługę sieci jest zarejestrowany w wstrzykiwanie zależności, który nawiązuje połączenie z bazą danych lub inny rodzaj magazynu użytkowników.</span><span class="sxs-lookup"><span data-stu-id="75f64-153">For real functionality, you'll probably want to call a service registered in dependency injection that connects to a database or other type of user store.</span></span> <span data-ttu-id="75f64-154">Dostęp do usługi przy użyciu kontekstu przekazywany do pełnomocnika.</span><span class="sxs-lookup"><span data-stu-id="75f64-154">Access your service by using the context passed into your delegate.</span></span> <span data-ttu-id="75f64-155">Rozważmy następujący przykład w `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="75f64-155">Consider the following example in `Startup.ConfigureServices`:</span></span>

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

<span data-ttu-id="75f64-156">Koncepcyjnie weryfikację certyfikatu jest kwestią autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="75f64-156">Conceptually, the validation of the certificate is an authorization concern.</span></span> <span data-ttu-id="75f64-157">Zaznaczenie, na przykład, wystawca lub odcisku palca w zasadach autoryzacji, a nie wewnątrz `OnCertificateValidated`, jest idealnie zgodna.</span><span class="sxs-lookup"><span data-stu-id="75f64-157">Adding a check on, for example, an issuer or thumbprint in an authorization policy, rather than inside `OnCertificateValidated`, is perfectly acceptable.</span></span>

## <a name="configure-your-host-to-require-certificates"></a><span data-ttu-id="75f64-158">Konfigurowanie hosta o wymagają certyfikatów</span><span class="sxs-lookup"><span data-stu-id="75f64-158">Configure your host to require certificates</span></span>

### <a name="kestrel"></a><span data-ttu-id="75f64-159">Kestrel</span><span class="sxs-lookup"><span data-stu-id="75f64-159">Kestrel</span></span>

<span data-ttu-id="75f64-160">W *Program.cs*, konfigurowanie usługi Kestrel w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="75f64-160">In *Program.cs*, configure Kestrel as follows:</span></span>

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

### <a name="iis"></a><span data-ttu-id="75f64-161">IIS</span><span class="sxs-lookup"><span data-stu-id="75f64-161">IIS</span></span>

<span data-ttu-id="75f64-162">Wykonaj następujące czynności w Menedżerze usług IIS:</span><span class="sxs-lookup"><span data-stu-id="75f64-162">Complete the following steps In IIS Manager:</span></span>

1. <span data-ttu-id="75f64-163">Wybierz lokację z **połączeń** kartę.</span><span class="sxs-lookup"><span data-stu-id="75f64-163">Select your site from the **Connections** tab.</span></span>
1. <span data-ttu-id="75f64-164">Kliknij dwukrotnie **ustawienia protokołu SSL** opcji **widoku funkcji** okna.</span><span class="sxs-lookup"><span data-stu-id="75f64-164">Double-click the **SSL Settings** option in the **Features View** window.</span></span>
1. <span data-ttu-id="75f64-165">Sprawdź **Wymagaj protokołu SSL** zaznacz pole wyboru i wybrać **wymagają** przycisku radiowego w **certyfikaty klienta** sekcji.</span><span class="sxs-lookup"><span data-stu-id="75f64-165">Check the **Require SSL** checkbox, and select the **Require** radio button in the **Client certificates** section.</span></span>

![Ustawienia certyfikatu klienta w usługach IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a><span data-ttu-id="75f64-167">Platforma Azure i niestandardowego internetowego serwera proxy</span><span class="sxs-lookup"><span data-stu-id="75f64-167">Azure and custom web proxies</span></span>

<span data-ttu-id="75f64-168">Zobacz [hostowanie i wdrażanie dokumentacji](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) dotyczące sposobu konfigurowania certyfikatów przekazywania oprogramowania pośredniczącego.</span><span class="sxs-lookup"><span data-stu-id="75f64-168">See the [host and deploy documentation](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) for how to configure the certificate forwarding middleware.</span></span>
