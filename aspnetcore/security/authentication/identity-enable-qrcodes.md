---
title: Włącz generowanie kodu QR dla uwierzytelniania w aplikacjach w ASP.NET Core
author: rick-anderson
description: Wykryj jak włączyć generowanie kodu QR dla aplikacji uwierzytelniania, które współpracują z uwierzytelniania dwuskładnikowego platformy ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: c61918d42b407b01484b67d740edc7a682c3a4b0
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/22/2018
---
# <a name="enable-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>Włącz generowanie kodu QR dla uwierzytelniania w aplikacjach w ASP.NET Core

Uwaga: W tym temacie dotyczą platformy ASP.NET Core 2.x

Platformy ASP.NET Core jest dostarczany z obsługą uwierzytelniania aplikacji dla indywidualnych uwierzytelniania. Dwa współczynnik uwierzytelniania (2FA) aplikacje uwierzytelniania, przy użyciu opartego na czasie jednorazowe hasła algorytmu (TOTP), są zalecane podejście do 2FA branży. 2FA przy użyciu TOTP jest preferowana względem 2FA programu SMS. Aplikacji uwierzytelniania zawiera kod 6 do 8 cyfr, które użytkownicy muszą wprowadzić po potwierdzeniu swoją nazwę użytkownika i hasło. Zwykle na smartfona zainstalowano aplikację uwierzytelniania.

Szablony aplikacji sieci web platformy ASP.NET Core obsługuje wystawców uwierzytelnienia, ale nie zapewniają obsługę QRCode generacji. Generatory QRCode do jej obsługi ułatwiają konfigurowanie 2FA. Ten dokument przeprowadzi Cię przez procedurę dodawania [kod QR](https://wikipedia.org/wiki/QR_code) generacji do strony Konfiguracja 2FA.

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>Dodawanie kodów QR do strony Konfiguracja 2FA

Użyj tych instrukcji *qrcode.js* z https://davidshimjs.github.io/qrcodejs/ repozytorium.

* Pobierz [biblioteka języka javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) do `wwwroot\lib` folder w projekcie.

* W *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor strony) lub *Views\Manage\EnableAuthenticator.cshtml* (MVC), Znajdź `Scripts` sekcji na końcu pliku:

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* Aktualizacja `Scripts` sekcji można dodać odwołania do `qrcodejs` biblioteki zostały dodane i wywołanie, aby wygenerować kod QR. Powinna wyglądać następująco:

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

Uruchom aplikację i upewnij się, że można Zeskanuj kod QR i sprawdzanie poprawności kodu, który potwierdza wystawcy uwierzytelnienia.

## <a name="change-the-site-name-in-the-qr-code"></a>Zmień nazwę lokacji w kod QR

Nazwa witryny w kod QR jest pobierana z wybranej podczas tworzenia wstępnie projektu nazwy projektu. Można go zmienić, wyszukując `GenerateQrCodeUri(string email, string unformattedKey)` metody w *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* pliku (Razor strony) lub *Controllers\ManageController.cs* pliku (MVC). 

Domyślny kod z szablonu wygląda następująco:

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

Drugi parametr w wywołaniu `string.Format` jest nazwą witryny z nazwą Twojego rozwiązania. Można zmienić wartości, ale muszą być zakodowane w adresie URL.

## <a name="using-a-different-qr-code-library"></a>Przy użyciu różnych biblioteki kod QR

Biblioteka kodów QR można zastąpić preferowane biblioteki. Zawiera kod HTML `qrCode` udostępnia biblioteki element, do którego można umieścić kod QR, przy użyciu dowolnego mechanizmu.

Adres URL poprawnie sformatowany kod QR jest dostępna w:

* `AuthenticatorUri` właściwości modelu.
* `data-url` Właściwość `qrCodeData` elementu. 

## <a name="totp-client-and-server-time-skew"></a>TOTP pochylenia czasu klienta i serwera

Uwierzytelnianie TOTP zależy od zarówno serwera, jak i Wystawca uwierzytelnienia urządzenia o dokładnego czasu. Tokeny tylko ostatnie 30 sekund. Jeśli TOTP 2FA logowania kończą się niepowodzeniem, sprawdź, czy czas serwera jest dokładne i najlepiej synchronizowane z usługi dokładne NTP.
