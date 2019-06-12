---
title: Konfigurowanie uwierzytelniania certyfikatów w programie ASP.NET Core
author: blowdart
description: Dowiedz się, jak skonfigurować uwierzytelnianie certyfikatu w programie ASP.NET Core dla usług IIS i sterownik HTTP.sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: barry.dorrans
ms.date: 06/11/2019
uid: security/authentication/certauth
ms.openlocfilehash: 37567128531187bfe7dd6c9f5aa990226e70f35f
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837554"
---
# <a name="overview"></a>Omówienie

`Microsoft.AspNetCore.Authentication.Certificate` Podobnie jak implementację [uwierzytelnianie certyfikatu](https://tools.ietf.org/html/rfc5246#section-7.4.4) dla platformy ASP.NET Core. Uwierzytelnianie certyfikatu odbywa się na poziomie protokołu TLS, long, zanim stanie się kiedykolwiek do platformy ASP.NET Core. Dokładniej mówiąc, jest to program obsługi uwierzytelniania, która sprawdza poprawność certyfikatu i proponuje następnie zdarzenie, w którym można rozwiązać tego certyfikatu do `ClaimsPrincipal`. 

[Konfigurowanie hosta](#configure-your-host-to-require-certificates) uwierzytelniania certyfikatu, są to usługi IIS, Kestrel, Azure Web Apps lub cokolwiek innego używasz.

## <a name="get-started"></a>Wprowadzenie

Uzyskać certyfikat HTTPS, zastosuj go, a [skonfigurować hosta](#configure-your-host-to-require-certificates) wymagające certyfikatów.

W aplikacji sieci web Dodaj odwołanie do `Microsoft.AspNetCore.Authentication.Certificate` pakietu. Następnie w `Startup.Configure` metody, wywołanie `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` przy użyciu opcji, dostarczenie delegata dla `OnCertificateValidated` celu wszelkie dodatkowe sprawdzanie poprawności certyfikatu klienta wysłanego z żądaniami. Włącz te informacje do `ClaimsPrincipal` i ustaw ją na `context.Principal` właściwości.

Jeśli uwierzytelnianie nie powiedzie się, zwraca ten program obsługi `403 (Forbidden)` odpowiedzi zamiast `401 (Unauthorized)`, zgodnie z oczekiwaniami. Przyczyny, dla których jest to, czy uwierzytelnienie ma się zdarzyć podczas pierwszego połączenia TLS. Przez razem, gdy zostanie osiągnięty, program obsługi jest za późno. Nie ma możliwości do połączenia z połączeniem anonimowym odpowiednią aktualizację przy użyciu certyfikatu.

Również dodać `app.UseAuthentication();` w `Startup.Configure` metody. W przeciwnym razie HttpContext.User nie zostanie ustawiona, `ClaimsPrincipal` utworzone na podstawie certyfikatu. Na przykład:

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

Poprzedni przykład pokazuje sposób domyślne, aby dodać certyfikat uwierzytelniania. Program obsługi tworzy jednostki użytkownika, przy użyciu wspólnej właściwości certyfikatu.

## <a name="configure-certificate-validation"></a>Skonfiguruj weryfikację certyfikatu

`CertificateAuthenticationOptions` Program obsługi ma kilka wbudowanych sprawdzanie poprawności, które są minimalne operacji sprawdzania poprawności, należy wykonać na certyfikacie. Każde z tych ustawień jest domyślnie włączona.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = łańcuchowa SelfSigned lub wszystkich (łańcuchowych | SelfSigned)

Ten test weryfikuje, czy jest dozwolony tylko typ odpowiedni certyfikat.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Ten test weryfikuje, czy certyfikat przedstawiony przez klienta ma uwierzytelniania klienta na wszystkich rozszerzone użycia klucza (EKU) lub brak rozszerzeń EKU. Ponieważ specyfikacji powiedzieć, jeśli nie określono żadnych rozszerzenia EKU, następnie wszystkich wartości EKU uznaje za prawidłowe.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Ten test weryfikuje, czy certyfikat jest okresem ważności. Na każde żądanie obsługi gwarantuje, że certyfikat, który była prawidłowa, gdy go został przedstawiony nie upłynął podczas bieżącej sesji.

### <a name="revocationflag"></a>RevocationFlag

Flaga określająca o certyfikatach w łańcuchu są sprawdzane pod kątem odwołań.

Sprawdzanie odwołań są wykonywane tylko w sytuacji, gdy certyfikat jest powiązany z certyfikatem głównym.

### <a name="revocationmode"></a>revocationMode

Flaga określająca sposób odwołania są sprawdzane.

Określanie kontroli w trybie online może spowodować duże opóźnienie podczas skontaktować się z urzędu certyfikacji.

Sprawdzanie odwołań są wykonywane tylko w sytuacji, gdy certyfikat jest powiązany z certyfikatem głównym.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>Można skonfigurować aplikację, aby wymagają certyfikatu tylko na określone ścieżki?

Nie jest to możliwe. Pamiętaj, że wymiany certyfikatu odbywa się czy początek komunikacji HTTPS to się robi przez serwer przed otrzymaniem pierwsze żądanie dla tego połączenia, więc nie jest możliwe do zakresu na podstawie pól wszelkie żądania.

## <a name="handler-events"></a>Program obsługi zdarzeń

Program obsługi ma dwa zdarzenia:

* `OnAuthenticationFailed` &ndash; Wywoływane, jeśli wyjątek będzie się działo podczas uwierzytelniania i pozwala reagować.
* `OnCertificateValidated` &ndash; Wywołuje się, gdy certyfikat został zweryfikowany przeszły sprawdzanie poprawności i domyślne podmiot zabezpieczeń został utworzony. To zdarzenie umożliwia należy przeprowadzić własne poprawności i rozszerzyć lub Zastąp podmiot zabezpieczeń. Przykłady obejmują:
  * Określanie, jeśli certyfikat jest znane usługi.
  * Konstruowanie własne jednostki. Rozważmy następujący przykład w `Startup.ConfigureServices`:

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

Jeśli okaże się ruchu przychodzącego certyfikat nie spełnia wymagań Twojej dodatkową walidację, wywołaj `context.Fail("failure reason")` wraz z przyczyną niepowodzenia.

Rzeczywiste funkcjonalności, prawdopodobnie należy wywołać usługę sieci jest zarejestrowany w wstrzykiwanie zależności, który nawiązuje połączenie z bazą danych lub inny rodzaj magazynu użytkowników. Dostęp do usługi przy użyciu kontekstu przekazywany do pełnomocnika. Rozważmy następujący przykład w `Startup.ConfigureServices`:

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

Koncepcyjnie weryfikację certyfikatu jest kwestią autoryzacji. Zaznaczenie, na przykład, wystawca lub odcisku palca w zasadach autoryzacji, a nie wewnątrz `OnCertificateValidated`, jest idealnie zgodna.

## <a name="configure-your-host-to-require-certificates"></a>Konfigurowanie hosta o wymagają certyfikatów

### <a name="kestrel"></a>Kestrel

W *Program.cs*, konfigurowanie usługi Kestrel w następujący sposób:

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

### <a name="iis"></a>IIS

Wykonaj następujące czynności w Menedżerze usług IIS:

1. Wybierz lokację z **połączeń** kartę.
1. Kliknij dwukrotnie **ustawienia protokołu SSL** opcji **widoku funkcji** okna.
1. Sprawdź **Wymagaj protokołu SSL** zaznacz pole wyboru i wybrać **wymagają** przycisku radiowego w **certyfikaty klienta** sekcji.

![Ustawienia certyfikatu klienta w usługach IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Platforma Azure i niestandardowego internetowego serwera proxy

Zobacz [hostowanie i wdrażanie dokumentacji](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) dotyczące sposobu konfigurowania certyfikatów przekazywania oprogramowania pośredniczącego.
