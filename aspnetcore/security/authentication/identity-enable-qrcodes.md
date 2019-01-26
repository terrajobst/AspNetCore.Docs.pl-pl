---
title: Włączanie generowania kodu QR dla TOTP aplikacje uwierzytelniania w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak włączyć generowanie kodu QR dla aplikacji authenticator TOTP, współpracujących z uwierzytelniania dwuskładnikowego platformy ASP.NET Core.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: cf99cc21a7a1bb4d01c7cc092106d23375a1a76f
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073130"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>Włączanie generowania kodu QR dla TOTP aplikacje uwierzytelniania w programie ASP.NET Core

::: moniker range="<= aspnetcore-2.0"

Kody QR wymaga platformy ASP.NET Core 2.0 lub nowszej.

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Platforma ASP.NET Core jest dostarczany z Obsługa aplikacji wystawcy uwierzytelnienia do poszczególnych uwierzytelniania. Dwa Authentication uwierzytelnianie (2FA) uwierzytelniania aplikacji przy użyciu opartych na czasie jednorazowe hasła algorytm (TOTP), są zalecane podejście do uwierzytelniania 2FA. Funkcji 2FA przy użyciu TOTP jest preferowany dla wiadomości SMS 2FA. Aplikację authenticator zawiera kod cyfrę 6 do 8, które użytkownicy muszą wprowadzić po potwierdzeniu, nazwy użytkownika i hasła. Zazwyczaj aplikację wystawcy uwierzytelnienia jest instalowany na smartfonie.

Szablony aplikacji sieci web platformy ASP.NET Core obsługuje wystawcy uwierzytelnienia, ale nie zapewnia pomoc techniczną dla QRCode generacji. Generatory QRCode jej obsługi ułatwiają realizację konfigurowania funkcji 2FA. Ten dokument przeprowadzi Cię przez proces dodawania [kod QR](https://wikipedia.org/wiki/QR_code) generacji do strony konfiguracji uwierzytelniania 2FA.

Uwierzytelnianie dwuskładnikowe nie odbywa się przy użyciu zewnętrznego dostawcę uwierzytelniania, takich jak [Google](xref:security/authentication/google-logins) lub [Facebook](xref:security/authentication/facebook-logins). Logowania zewnętrzne są chronione przy użyciu dowolnego mechanizmu zapewnia dostawcy logowania zewnętrznego. Należy wziąć pod uwagę, na przykład [Microsoft](xref:security/authentication/microsoft-logins) dostawcy uwierzytelniania wymaga klucza sprzętowego lub innej metody uwierzytelniania 2FA. Jeśli domyślne szablony wymuszane 2FA "local" użytkownicy będą wymagane do spełnienia dwie metody uwierzytelniania 2FA, które nie jest powszechnie używany scenariusz.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Dodawanie kody QR do strony konfiguracji funkcji 2FA

W poniższych instrukcjach użyto *qrcode.js* z https://davidshimjs.github.io/qrcodejs/ repozytorium.

* Pobierz [biblioteki javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) do `wwwroot\lib` folder w projekcie.

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* Postępuj zgodnie z instrukcjami w [tożsamości szkieletu](xref:security/authentication/scaffold-identity) do generowania */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.
* W */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, zlokalizuj `Scripts` sekcji na końcu pliku:

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* W *Pages/Account/Manage/EnableAuthenticator.cshtml* (stron Razor) lub *Views/Manage/EnableAuthenticator.cshtml* (MVC), Znajdź `Scripts` sekcji na końcu pliku:

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Aktualizacja `Scripts` sekcji, aby dodać odwołanie do `qrcodejs` biblioteki został dodany i wywołania do generowania kodu QR. Powinno to wyglądać w następujący sposób:

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

* Usuń akapit, który podano łącza do niniejszych instrukcji.

Uruchom aplikację i upewnij się, aby zeskanować kod QR i sprawdzanie poprawności kodu, który okaże się wystawcy uwierzytelnienia.

## <a name="change-the-site-name-in-the-qr-code"></a>Zmień nazwę lokacji w kod QR

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

Nazwa witryny w kod QR jest pobierana z nazwę projektu, który wybierzesz podczas początkowego tworzenia projektu. Można go zmienić, wyszukując `GenerateQrCodeUri(string email, string unformattedKey)` method in Class metoda */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Nazwa witryny w kod QR jest pobierana z nazwę projektu, który wybierzesz podczas początkowego tworzenia projektu. Można go zmienić, wyszukując `GenerateQrCodeUri(string email, string unformattedKey)` method in Class metoda *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* pliku (stron Razor) lub *Controllers/ManageController.cs* pliku (MVC).

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

Domyślny kod z szablonu wygląda następująco:

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Drugi parametr w wywołaniu `string.Format` jest nazwą witryny z nazwą rozwiązania. Można ją zmienić na jakąkolwiek wartość, ale zawsze musi być zakodowane w adresie URL.

## <a name="using-a-different-qr-code-library"></a>Przy użyciu innej biblioteki kodu QR

Biblioteka kodów QR można zastąpić preferowanych biblioteki. Zawiera kod HTML `qrCode` zawiera biblioteki elementu, w którym można umieszczać kod QR przy użyciu dowolnego mechanizmu.

Poprawnie sformatowany adres URL kod QR jest dostępna w:

* `AuthenticatorUri` właściwości modelu.
* `data-url` Właściwość `qrCodeData` elementu.

## <a name="totp-client-and-server-time-skew"></a>TOTP niesymetryczność czasu klienta i serwera

Uwierzytelnianie TOTP (oparte na czasie hasła jednorazowego) zależy od serwera i uwierzytelniania urządzeń mających dokładnego czasu. Tokeny trwać tylko przez 30 sekund. Logowania do uwierzytelniania 2FA TOTP kończą się niepowodzeniem, sprawdź, czy czas serwera jest dokładne i najlepiej zsynchronizowane z usługą NTP dokładne.

::: moniker-end
