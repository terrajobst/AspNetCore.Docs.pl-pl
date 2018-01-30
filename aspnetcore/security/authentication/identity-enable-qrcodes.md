---
title: "Włączenie generowania kodu QR uwierzytelniania aplikacji w ASP.NET Core"
author: rick-anderson
description: "Włączenie generowania kodu QR uwierzytelniania aplikacji w ASP.NET Core"
manager: wpickett
ms.author: riande
ms.date: 09/24/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: ae2d8eb938c00a26cf7ffb5f2fff0b9e0d22148b
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="05d1f-103">Włączenie generowania kodu QR uwierzytelniania aplikacji w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="05d1f-103">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="05d1f-104">Uwaga: W tym temacie dotyczą platformy ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="05d1f-104">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="05d1f-105">Platformy ASP.NET Core jest dostarczany z obsługą uwierzytelniania aplikacji dla indywidualnych uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="05d1f-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="05d1f-106">Dwa współczynnik uwierzytelniania (2FA) aplikacje uwierzytelniania, przy użyciu opartego na czasie jednorazowe hasła algorytmu (TOTP), są zalecane podejście do 2FA branży.</span><span class="sxs-lookup"><span data-stu-id="05d1f-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="05d1f-107">2FA przy użyciu TOTP jest preferowana względem 2FA programu SMS.</span><span class="sxs-lookup"><span data-stu-id="05d1f-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="05d1f-108">Aplikacji uwierzytelniania zawiera kod 6 do 8 cyfr, które użytkownicy muszą wprowadzić po potwierdzeniu swoją nazwę użytkownika i hasło.</span><span class="sxs-lookup"><span data-stu-id="05d1f-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="05d1f-109">Zwykle na smartfona zainstalowano aplikację uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="05d1f-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="05d1f-110">Szablony aplikacji sieci web platformy ASP.NET Core obsługuje wystawców uwierzytelnienia, ale nie zapewniają obsługę QRCode generacji.</span><span class="sxs-lookup"><span data-stu-id="05d1f-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="05d1f-111">Generatory QRCode do jej obsługi ułatwiają konfigurowanie 2FA.</span><span class="sxs-lookup"><span data-stu-id="05d1f-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="05d1f-112">Ten dokument przeprowadzi Cię przez procedurę dodawania [kod QR](https://wikipedia.org/wiki/QR_code) generacji do strony Konfiguracja 2FA.</span><span class="sxs-lookup"><span data-stu-id="05d1f-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="05d1f-113">Dodawanie kodów QR do strony Konfiguracja 2FA</span><span class="sxs-lookup"><span data-stu-id="05d1f-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="05d1f-114">Użyj tych instrukcji *qrcode.js* z repozytorium https://davidshimjs.github.io/qrcodejs/.</span><span class="sxs-lookup"><span data-stu-id="05d1f-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="05d1f-115">Pobierz [biblioteka języka javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) do `wwwroot\lib` folder w projekcie.</span><span class="sxs-lookup"><span data-stu-id="05d1f-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="05d1f-116">W *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor strony) lub *Views\Manage\EnableAuthenticator.cshtml* (MVC), Znajdź `Scripts` sekcji na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="05d1f-116">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="05d1f-117">Aktualizacja `Scripts` sekcji można dodać odwołania do `qrcodejs` biblioteki zostały dodane i wywołanie, aby wygenerować kod QR.</span><span class="sxs-lookup"><span data-stu-id="05d1f-117">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="05d1f-118">Powinna wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="05d1f-118">It should look as follows:</span></span>

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

* <span data-ttu-id="05d1f-119">Usuń akapit, który podano łącza do niniejszych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="05d1f-119">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="05d1f-120">Uruchom aplikację i upewnij się, że można Zeskanuj kod QR i sprawdzanie poprawności kodu, który potwierdza wystawcy uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="05d1f-120">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="05d1f-121">Zmień nazwę lokacji w kod QR</span><span class="sxs-lookup"><span data-stu-id="05d1f-121">Change the site name in the QR Code</span></span>

<span data-ttu-id="05d1f-122">Nazwa witryny w kod QR jest pobierana z wybranej podczas tworzenia wstępnie projektu nazwy projektu.</span><span class="sxs-lookup"><span data-stu-id="05d1f-122">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="05d1f-123">Można go zmienić, wyszukując `GenerateQrCodeUri(string email, string unformattedKey)` metody w *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* pliku (Razor strony) lub *Controllers\ManageController.cs* pliku (MVC).</span><span class="sxs-lookup"><span data-stu-id="05d1f-123">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\ManageController.cs* (MVC) file.</span></span> 

<span data-ttu-id="05d1f-124">Domyślny kod z szablonu wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="05d1f-124">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="05d1f-125">Drugi parametr w wywołaniu `string.Format` jest nazwą witryny z nazwą Twojego rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="05d1f-125">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="05d1f-126">Można zmienić wartości, ale muszą być zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="05d1f-126">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="05d1f-127">Przy użyciu różnych biblioteki kod QR</span><span class="sxs-lookup"><span data-stu-id="05d1f-127">Using a different QR Code library</span></span>

<span data-ttu-id="05d1f-128">Biblioteka kodów QR można zastąpić preferowane biblioteki.</span><span class="sxs-lookup"><span data-stu-id="05d1f-128">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="05d1f-129">Zawiera kod HTML `qrCode` udostępnia biblioteki element, do którego można umieścić kod QR, przy użyciu dowolnego mechanizmu.</span><span class="sxs-lookup"><span data-stu-id="05d1f-129">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="05d1f-130">Adres URL poprawnie sformatowany kod QR jest dostępna w:</span><span class="sxs-lookup"><span data-stu-id="05d1f-130">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="05d1f-131">`AuthenticatorUri`właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="05d1f-131">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="05d1f-132">`data-url`Właściwość `qrCodeData` elementu.</span><span class="sxs-lookup"><span data-stu-id="05d1f-132">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="05d1f-133">Użyj `@Html.Raw` do dostępu do właściwości modelu w widoku (w przeciwnym razie takie znaki w adresie url będzie podwójnie zakodowane i parametr etykiety kod QR zostanie zignorowana).</span><span class="sxs-lookup"><span data-stu-id="05d1f-133">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="05d1f-134">TOTP pochylenia czasu klienta i serwera</span><span class="sxs-lookup"><span data-stu-id="05d1f-134">TOTP client and server time skew</span></span>

<span data-ttu-id="05d1f-135">Uwierzytelnianie TOTP zależy od zarówno serwera, jak i Wystawca uwierzytelnienia urządzenia o dokładnego czasu.</span><span class="sxs-lookup"><span data-stu-id="05d1f-135">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="05d1f-136">Tokeny tylko ostatnie 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="05d1f-136">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="05d1f-137">Jeśli TOTP 2FA logowania kończą się niepowodzeniem, sprawdź, czy czas serwera jest dokładne i najlepiej synchronizowane z usługi dokładne NTP.</span><span class="sxs-lookup"><span data-stu-id="05d1f-137">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
