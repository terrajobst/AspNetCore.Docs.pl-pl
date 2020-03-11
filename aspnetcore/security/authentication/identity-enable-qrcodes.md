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
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="447d9-103">Włącz generowanie kodu QR dla aplikacji TOTP Authenticator w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="447d9-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="447d9-104">Kody QR wymagają ASP.NET Core 2,0 lub nowszego.</span><span class="sxs-lookup"><span data-stu-id="447d9-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="447d9-105">ASP.NET Core jest dostarczany z obsługą aplikacji uwierzytelniania do indywidualnego uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="447d9-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="447d9-106">Dwa Authentication uwierzytelnianie (2FA) uwierzytelniania aplikacji przy użyciu opartych na czasie jednorazowe hasła algorytm (TOTP), są zalecane podejście do uwierzytelniania 2FA.</span><span class="sxs-lookup"><span data-stu-id="447d9-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="447d9-107">Funkcji 2FA przy użyciu TOTP jest preferowany dla wiadomości SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="447d9-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="447d9-108">Aplikacja Authenticator zawiera 6-8-cyfrowy kod, który użytkownicy muszą wprowadzić po potwierdzeniu nazwy użytkownika i hasła.</span><span class="sxs-lookup"><span data-stu-id="447d9-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="447d9-109">Zazwyczaj aplikacja uwierzytelniania jest instalowana na telefonie inteligentnym.</span><span class="sxs-lookup"><span data-stu-id="447d9-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="447d9-110">Szablony aplikacji sieci Web ASP.NET Core obsługują wystawców, ale nie zapewniają wsparcia dla generacji QRCode.</span><span class="sxs-lookup"><span data-stu-id="447d9-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="447d9-111">Generatory QRCode ułatwiają konfigurację funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="447d9-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="447d9-112">Ten dokument przeprowadzi Cię przez proces dodawania generowania [kodu QR](https://wikipedia.org/wiki/QR_code) do strony konfiguracji funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="447d9-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

<span data-ttu-id="447d9-113">Uwierzytelnianie dwuskładnikowe nie odbywa się przy użyciu dostawcy uwierzytelniania zewnętrznego, takiego jak [Google](xref:security/authentication/google-logins) lub [Facebook](xref:security/authentication/facebook-logins).</span><span class="sxs-lookup"><span data-stu-id="447d9-113">Two factor authentication does not happen using an external authentication provider, such as [Google](xref:security/authentication/google-logins) or [Facebook](xref:security/authentication/facebook-logins).</span></span> <span data-ttu-id="447d9-114">Logowania zewnętrzne są chronione przez każdy mechanizm, który zapewnia zewnętrzny dostawca logowania.</span><span class="sxs-lookup"><span data-stu-id="447d9-114">External logins are protected by whatever mechanism the external login provider provides.</span></span> <span data-ttu-id="447d9-115">Rozważmy na przykład, że dostawca uwierzytelniania [firmy Microsoft](xref:security/authentication/microsoft-logins) wymaga klucza sprzętowego lub innego podejścia funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="447d9-115">Consider, for example, the [Microsoft](xref:security/authentication/microsoft-logins) authentication provider requires a hardware key or another 2FA approach.</span></span> <span data-ttu-id="447d9-116">Jeśli domyślne szablony wymuszają "Local" funkcji 2FA, użytkownicy będą musieli spełnić dwa podejścia funkcji 2FA, które nie są powszechnie używanym scenariuszem.</span><span class="sxs-lookup"><span data-stu-id="447d9-116">If the default templates enforced "local" 2FA then users would be required to satisfy two 2FA approaches, which is not a commonly used scenario.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="447d9-117">Dodawanie kodów QR do strony konfiguracji funkcji 2FA</span><span class="sxs-lookup"><span data-stu-id="447d9-117">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="447d9-118">Te instrukcje używają *QRCode. js* z repozytorium https://davidshimjs.github.io/qrcodejs/.</span><span class="sxs-lookup"><span data-stu-id="447d9-118">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="447d9-119">Pobierz [bibliotekę JavaScript QRCode. js](https://davidshimjs.github.io/qrcodejs/) do folderu `wwwroot\lib` w projekcie.</span><span class="sxs-lookup"><span data-stu-id="447d9-119">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="447d9-120">Postępuj zgodnie z instrukcjami w temacie [tożsamość szkieletowa](xref:security/authentication/scaffold-identity) , aby wygenerować */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="447d9-120">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="447d9-121">W */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*znajdź sekcję `Scripts` na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="447d9-121">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="447d9-122">W obszarze *Pages/Account/Manage/EnableAuthenticator. cshtml* (Razor Pages) lub *widoków/Manage/EnableAuthenticator. cshtml* (MVC) Znajdź sekcję `Scripts` na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="447d9-122">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="447d9-123">Zaktualizuj sekcję `Scripts`, aby dodać odwołanie do dodanej biblioteki `qrcodejs` i wywołanie w celu wygenerowania kodu QR.</span><span class="sxs-lookup"><span data-stu-id="447d9-123">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="447d9-124">Powinien wyglądać następująco:</span><span class="sxs-lookup"><span data-stu-id="447d9-124">It should look as follows:</span></span>

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

* <span data-ttu-id="447d9-125">Usuń akapit, który łączy się z tymi instrukcjami.</span><span class="sxs-lookup"><span data-stu-id="447d9-125">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="447d9-126">Uruchom aplikację i upewnij się, że możesz skanować kod QR i sprawdzać kod, który potwierdzi wystawca uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="447d9-126">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="447d9-127">Zmień nazwę lokacji w kodzie QR</span><span class="sxs-lookup"><span data-stu-id="447d9-127">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="447d9-128">Nazwa witryny w kodzie QR jest pobierana z nazwy projektu wybranej podczas pierwszego tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="447d9-128">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="447d9-129">Można to zmienić, wyszukując metodę `GenerateQrCodeUri(string email, string unformattedKey)` w */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*.</span><span class="sxs-lookup"><span data-stu-id="447d9-129">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="447d9-130">Nazwa witryny w kodzie QR jest pobierana z nazwy projektu wybranej podczas pierwszego tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="447d9-130">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="447d9-131">Można to zmienić, wyszukując metodę `GenerateQrCodeUri(string email, string unformattedKey)` w pliku *Pages/Account/Manage/EnableAuthenticator. cshtml. cs* (Razor Pages) lub w pliku *controllers/ManageController. cs* (MVC).</span><span class="sxs-lookup"><span data-stu-id="447d9-131">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="447d9-132">Kod domyślny z szablonu wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="447d9-132">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="447d9-133">Drugim parametrem w wywołaniu `string.Format` jest nazwa witryny, pobrana z nazwy rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="447d9-133">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="447d9-134">Można go zmienić na dowolną wartość, ale zawsze musi być zakodowany w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="447d9-134">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="447d9-135">Korzystanie z innej biblioteki kodu QR</span><span class="sxs-lookup"><span data-stu-id="447d9-135">Using a different QR Code library</span></span>

<span data-ttu-id="447d9-136">Bibliotekę kodu QR można zastąpić za pomocą preferowanej biblioteki.</span><span class="sxs-lookup"><span data-stu-id="447d9-136">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="447d9-137">Kod HTML zawiera `qrCode` elementu, w którym można umieścić numer QR przez dowolny mechanizm udostępniany przez bibliotekę.</span><span class="sxs-lookup"><span data-stu-id="447d9-137">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="447d9-138">Poprawnie sformatowany adres URL dla kodu QR jest dostępny w:</span><span class="sxs-lookup"><span data-stu-id="447d9-138">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="447d9-139">Właściwość `AuthenticatorUri` modelu.</span><span class="sxs-lookup"><span data-stu-id="447d9-139">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="447d9-140">Właściwość `data-url` w elemencie `qrCodeData`.</span><span class="sxs-lookup"><span data-stu-id="447d9-140">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="447d9-141">Przechylenie czasu klienta i serwera TOTP</span><span class="sxs-lookup"><span data-stu-id="447d9-141">TOTP client and server time skew</span></span>

<span data-ttu-id="447d9-142">TOTP (oparte na czasie hasło jednorazowe) uwierzytelnianie zależy od zarówno urządzenia serwera, jak i uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="447d9-142">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="447d9-143">Tokeny są ostatnie przez 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="447d9-143">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="447d9-144">Jeśli logowanie TOTP funkcji 2FA kończy się niepowodzeniem, sprawdź, czy czas serwera jest dokładny i najlepiej zsynchronizowany z dokładną usługą NTP.</span><span class="sxs-lookup"><span data-stu-id="447d9-144">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
