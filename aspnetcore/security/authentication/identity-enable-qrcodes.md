---
title: Włącz generowanie kodu QR dla aplikacji TOTP Authenticator w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak włączyć generowanie kodu QR dla aplikacji TOTP Authenticator, które działają przy użyciu uwierzytelniania dwuskładnikowego ASP.NET Core.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a7fdc86b3fe94e714e5147c89a32fce13757d1c1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665313"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>Włącz generowanie kodu QR dla aplikacji TOTP Authenticator w ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

Kody QR wymagają ASP.NET Core 2,0 lub nowszego.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

ASP.NET Core jest dostarczany z obsługą aplikacji uwierzytelniania do indywidualnego uwierzytelniania. Dwa Authentication uwierzytelnianie (2FA) uwierzytelniania aplikacji przy użyciu opartych na czasie jednorazowe hasła algorytm (TOTP), są zalecane podejście do uwierzytelniania 2FA. Funkcji 2FA przy użyciu TOTP jest preferowany dla wiadomości SMS 2FA. Aplikacja Authenticator zawiera 6-8-cyfrowy kod, który użytkownicy muszą wprowadzić po potwierdzeniu nazwy użytkownika i hasła. Zazwyczaj aplikacja uwierzytelniania jest instalowana na telefonie inteligentnym.

Szablony aplikacji sieci Web ASP.NET Core obsługują wystawców, ale nie zapewniają wsparcia dla generacji QRCode. Generatory QRCode ułatwiają konfigurację funkcji 2FA. Ten dokument przeprowadzi Cię przez proces dodawania generowania [kodu QR](https://wikipedia.org/wiki/QR_code) do strony konfiguracji funkcji 2FA.

Uwierzytelnianie dwuskładnikowe nie odbywa się przy użyciu dostawcy uwierzytelniania zewnętrznego, takiego jak [Google](xref:security/authentication/google-logins) lub [Facebook](xref:security/authentication/facebook-logins). Logowania zewnętrzne są chronione przez każdy mechanizm, który zapewnia zewnętrzny dostawca logowania. Rozważmy na przykład, że dostawca uwierzytelniania [firmy Microsoft](xref:security/authentication/microsoft-logins) wymaga klucza sprzętowego lub innego podejścia funkcji 2FA. Jeśli domyślne szablony wymuszają "Local" funkcji 2FA, użytkownicy będą musieli spełnić dwa podejścia funkcji 2FA, które nie są powszechnie używanym scenariuszem.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Dodawanie kodów QR do strony konfiguracji funkcji 2FA

Te instrukcje używają *QRCode. js* z repozytorium https://davidshimjs.github.io/qrcodejs/.

* Pobierz [bibliotekę JavaScript QRCode. js](https://davidshimjs.github.io/qrcodejs/) do folderu `wwwroot\lib` w projekcie.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Postępuj zgodnie z instrukcjami w temacie [tożsamość szkieletowa](xref:security/authentication/scaffold-identity) , aby wygenerować */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.
* W */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*znajdź sekcję `Scripts` na końcu pliku:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* W obszarze *Pages/Account/Manage/EnableAuthenticator. cshtml* (Razor Pages) lub *widoków/Manage/EnableAuthenticator. cshtml* (MVC) Znajdź sekcję `Scripts` na końcu pliku:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Zaktualizuj sekcję `Scripts`, aby dodać odwołanie do dodanej biblioteki `qrcodejs` i wywołanie w celu wygenerowania kodu QR. Powinien wyglądać następująco:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* Usuń akapit, który łączy się z tymi instrukcjami.

Uruchom aplikację i upewnij się, że możesz skanować kod QR i sprawdzać kod, który potwierdzi wystawca uwierzytelnienia.

## <a name="change-the-site-name-in-the-qr-code"></a>Zmień nazwę lokacji w kodzie QR

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Nazwa witryny w kodzie QR jest pobierana z nazwy projektu wybranej podczas pierwszego tworzenia projektu. Można to zmienić, wyszukując metodę `GenerateQrCodeUri(string email, string unformattedKey)` w */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Nazwa witryny w kodzie QR jest pobierana z nazwy projektu wybranej podczas pierwszego tworzenia projektu. Można to zmienić, wyszukując metodę `GenerateQrCodeUri(string email, string unformattedKey)` w pliku *Pages/Account/Manage/EnableAuthenticator. cshtml. cs* (Razor Pages) lub w pliku *controllers/ManageController. cs* (MVC).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Kod domyślny z szablonu wygląda następująco:

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenticatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Drugim parametrem w wywołaniu `string.Format` jest nazwa witryny, pobrana z nazwy rozwiązania. Można go zmienić na dowolną wartość, ale zawsze musi być zakodowany w adresie URL.

## <a name="using-a-different-qr-code-library"></a>Korzystanie z innej biblioteki kodu QR

Bibliotekę kodu QR można zastąpić za pomocą preferowanej biblioteki. Kod HTML zawiera `qrCode` elementu, w którym można umieścić numer QR przez dowolny mechanizm udostępniany przez bibliotekę.

Poprawnie sformatowany adres URL dla kodu QR jest dostępny w:

* Właściwość `AuthenticatorUri` modelu.
* Właściwość `data-url` w elemencie `qrCodeData`.

## <a name="totp-client-and-server-time-skew"></a>Przechylenie czasu klienta i serwera TOTP

TOTP (oparte na czasie hasło jednorazowe) uwierzytelnianie zależy od zarówno urządzenia serwera, jak i uwierzytelniania. Tokeny są ostatnie przez 30 sekund. Jeśli logowanie TOTP funkcji 2FA kończy się niepowodzeniem, sprawdź, czy czas serwera jest dokładny i najlepiej zsynchronizowany z dokładną usługą NTP.

::: moniker-end
