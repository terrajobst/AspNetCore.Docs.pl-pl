---
title: Konfigurowanie uwierzytelniania certyfikatów w ASP.NET Core
author: blowdart
description: Dowiedz się, jak skonfigurować uwierzytelnianie certyfikatów w ASP.NET Core dla usług IIS i HTTP. sys.
monikerRange: '>= aspnetcore-3.0'
ms.author: bdorrans
ms.date: 08/19/2019
uid: security/authentication/certauth
ms.openlocfilehash: bb375cf380175daf2399f3b56f543819ee5692b8
ms.sourcegitcommit: 07cd66e367d080acb201c7296809541599c947d1
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 09/17/2019
ms.locfileid: "71039239"
---
# <a name="configure-certificate-authentication-in-aspnet-core"></a>Konfigurowanie uwierzytelniania certyfikatów w ASP.NET Core

`Microsoft.AspNetCore.Authentication.Certificate`zawiera implementację podobną do [uwierzytelniania certyfikatu](https://tools.ietf.org/html/rfc5246#section-7.4.4) dla ASP.NET Core. Uwierzytelnianie certyfikatu odbywa się na poziomie protokołu TLS, o ile nie zostanie kiedykolwiek przeASP.NET Core. Dokładniej, jest to procedura obsługi uwierzytelniania, która sprawdza poprawność certyfikatu, a następnie przekazuje zdarzenie, w którym można rozwiązać ten certyfikat do `ClaimsPrincipal`. 

[Skonfiguruj hosta](#configure-your-host-to-require-certificates) na potrzeby uwierzytelniania certyfikatów, to usługi IIS, Kestrel, Azure Web Apps lub inne, z których korzystasz.

## <a name="proxy-and-load-balancer-scenarios"></a>Scenariusze dotyczące serwerów proxy i równoważenia obciążenia

Uwierzytelnianie certyfikatu jest scenariuszem stanowym głównie używanym w przypadku, gdy serwer proxy lub moduł równoważenia obciążenia nie obsługuje ruchu między klientami i serwerami. Jeśli jest używany serwer proxy lub moduł równoważenia obciążenia, uwierzytelnianie certyfikatu działa tylko wtedy, gdy serwer proxy lub moduł równoważenia obciążenia:

* Obsługuje uwierzytelnianie.
* Przekazuje informacje o uwierzytelnianiu użytkownika do aplikacji (na przykład w nagłówku żądania), która działa na informacje o uwierzytelnianiu.

Alternatywą dla uwierzytelniania certyfikatu w środowiskach, w których są używane serwery proxy i moduły równoważenia obciążenia, są Active Directory usług federacyjnych (AD FS) za pomocą OpenID Connect Connect (OIDC).

## <a name="get-started"></a>Wprowadzenie

Uzyskaj certyfikat HTTPS, zastosuj go i [skonfiguruj hosta](#configure-your-host-to-require-certificates) , aby wymagał certyfikatów.

W aplikacji sieci Web Dodaj odwołanie do `Microsoft.AspNetCore.Authentication.Certificate` pakietu. Następnie w `Startup.Configure` metodzie Zadzwoń `app.AddAuthentication(CertificateAuthenticationDefaults.AuthenticationScheme).UseCertificateAuthentication(...);` z własnymi opcjami `OnCertificateValidated` , podając delegata w celu wykonania dodatkowej weryfikacji dla certyfikatu klienta wysyłanego z żądaniami. Zmień te informacje `ClaimsPrincipal` na i ustaw `context.Principal` dla właściwości.

Jeśli uwierzytelnianie nie powiedzie się, ta `403 (Forbidden)` procedura obsługi zwróci `401 (Unauthorized)`odpowiedź zamiast elementu, zgodnie z oczekiwaniami. Powodem jest to, że uwierzytelnianie powinno nastąpić podczas początkowego połączenia TLS. Przez czas, gdy dociera do programu obsługi, jest zbyt opóźniony. Nie ma możliwości uaktualnienia połączenia z anonimowego połączenia z certyfikatem.

`app.UseAuthentication();` Dodaj`Startup.Configure` również metodę. W przeciwnym razie Właściwość HttpContext. User nie zostanie ustawiona jako `ClaimsPrincipal` utworzona na podstawie certyfikatu. Na przykład:

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

W powyższym przykładzie przedstawiono domyślny sposób dodawania uwierzytelniania przy użyciu certyfikatu. Program obsługi konstruuje jednostkę główną użytkownika przy użyciu wspólnych właściwości certyfikatu.

## <a name="configure-certificate-validation"></a>Konfigurowanie weryfikacji certyfikatu

`CertificateAuthenticationOptions` Program obsługi ma pewne wbudowane walidacje, które są minimalnymi walidacjami, które należy wykonać na certyfikacie. Każdy z tych ustawień jest domyślnie włączony.

### <a name="allowedcertificatetypes--chained-selfsigned-or-all-chained--selfsigned"></a>AllowedCertificateTypes = łańcuchy, SelfSigned lub wszystkie (łańcuchowo | SelfSigned)

Ten test sprawdza, czy dozwolony jest tylko odpowiedni typ certyfikatu.

### <a name="validatecertificateuse"></a>ValidateCertificateUse

Ten test sprawdza, czy certyfikat przedstawiony przez klienta ma rozszerzone użycie klucza uwierzytelniania klienta (EKU) lub nie rozszerzeń EKU w ogóle. Zgodnie ze specyfikacją, jeśli nie określono rozszerzenia EKU, wszystkie rozszerzeń EKU są uznawane za prawidłowe.

### <a name="validatevalidityperiod"></a>ValidateValidityPeriod

Ten test sprawdza, czy certyfikat jest w jego okresie ważności. W przypadku każdego żądania program obsługi zapewnia, że certyfikat, który był ważny, gdy był prezentowany, nie upłynął podczas bieżącej sesji.

### <a name="revocationflag"></a>RevocationFlag

Flaga określająca, które certyfikaty w łańcuchu są sprawdzane pod kątem odwołania.

Sprawdzanie odwołań jest wykonywane tylko wtedy, gdy certyfikat jest powiązany z certyfikatem głównym.

### <a name="revocationmode"></a>Odwołaniemode

Flaga określająca sposób sprawdzania odwołania.

Określenie, że sprawdzanie online może skutkować długim opóźnieniem podczas kontaktowania się z urzędem certyfikacji.

Sprawdzanie odwołań jest wykonywane tylko wtedy, gdy certyfikat jest powiązany z certyfikatem głównym.

### <a name="can-i-configure-my-app-to-require-a-certificate-only-on-certain-paths"></a>Czy mogę skonfigurować aplikację, aby wymagać certyfikatu tylko w określonych ścieżkach?

Nie jest to możliwe. Należy pamiętać, że wymiana certyfikatów jest wykonywana na początku konwersacji HTTPS, przez serwer przed odebraniem pierwszego żądania w ramach tego połączenia, aby nie było możliwe Określanie zakresu na podstawie pól żądania.

## <a name="handler-events"></a>Zdarzenia programu obsługi

Program obsługi ma dwa zdarzenia:

* `OnAuthenticationFailed`&ndash; Wywołuje się, gdy wyjątek występuje podczas uwierzytelniania i pozwala na reagowanie.
* `OnCertificateValidated`&ndash; Wywoływana po zweryfikowaniu certyfikatu, została pomyślnie utworzona Walidacja i domyślny podmiot zabezpieczeń. To zdarzenie umożliwia wykonywanie własnych weryfikacji i rozszerzanie lub zastępowanie podmiotu zabezpieczeń. Przykłady obejmują:
  * Ustalanie, czy certyfikat jest znany dla usług.
  * Konstruowanie własnego podmiotu zabezpieczeń. Rozważmy następujący przykład w `Startup.ConfigureServices`:

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

Jeśli okaże się, że certyfikat ruchu przychodzącego nie spełnia dodatkowej weryfikacji `context.Fail("failure reason")` , wywołaj z przyczynę niepowodzenia.

W przypadku rzeczywistej funkcjonalności prawdopodobnie chcesz wywołać usługę zarejestrowaną w iniekcji zależności, która łączy się z bazą danych lub innym typem magazynu użytkownika. Uzyskaj dostęp do usługi przy użyciu kontekstu przesłanego do obiektu delegowanego. Rozważmy następujący przykład w `Startup.ConfigureServices`:

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

Koncepcyjnie sprawdzenie poprawności certyfikatu jest problemem z autoryzacją. Dodanie kontroli, na przykład wystawcy lub odcisk palca w zasadach autoryzacji, a nie wewnątrz `OnCertificateValidated`, jest doskonale akceptowalne.

## <a name="configure-your-host-to-require-certificates"></a>Konfigurowanie hosta tak, aby wymagał certyfikatów

### <a name="kestrel"></a>Kestrel

W *program.cs*Skonfiguruj Kestrel w następujący sposób:

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

Wykonaj następujące kroki w Menedżerze usług IIS:

1. Wybierz swoją witrynę z karty **połączenia** .
1. Kliknij dwukrotnie opcję **Ustawienia protokołu SSL** w oknie **Widok funkcji** .
1. Zaznacz pole wyboru **Wymagaj protokołu SSL** , a następnie wybierz przycisk **Wymagaj** przycisku radiowego w sekcji **Certyfikaty klienta** .

![Ustawienia certyfikatu klienta w usługach IIS](README-IISConfig.png)

### <a name="azure-and-custom-web-proxies"></a>Azure i niestandardowe serwery proxy sieci Web

Zapoznaj się z [dokumentacją hosta i Wdróż](xref:host-and-deploy/proxy-load-balancer#certificate-forwarding) , jak skonfigurować oprogramowanie pośredniczące przesyłania certyfikatów.
