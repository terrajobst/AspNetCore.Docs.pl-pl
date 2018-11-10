---
title: Włączanie generowania kodu QR dla TOTP aplikacje uwierzytelniania w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak włączyć generowanie kodu QR dla aplikacji authenticator TOTP, współpracujących z uwierzytelniania dwuskładnikowego platformy ASP.NET Core.
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 437f354f71128a98bae9abdced291e04efc9f48e
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225385"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="7701d-103">Włączanie generowania kodu QR dla TOTP aplikacje uwierzytelniania w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7701d-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="7701d-104">Kody QR wymaga platformy ASP.NET Core 2.0 lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="7701d-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7701d-105">Platforma ASP.NET Core jest dostarczany z Obsługa aplikacji wystawcy uwierzytelnienia do poszczególnych uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="7701d-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="7701d-106">Dwa Authentication uwierzytelnianie (2FA) uwierzytelniania aplikacji przy użyciu opartych na czasie jednorazowe hasła algorytm (TOTP), są zalecane podejście do uwierzytelniania 2FA.</span><span class="sxs-lookup"><span data-stu-id="7701d-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="7701d-107">Funkcji 2FA przy użyciu TOTP jest preferowany dla wiadomości SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="7701d-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="7701d-108">Aplikację authenticator zawiera kod cyfrę 6 do 8, które użytkownicy muszą wprowadzić po potwierdzeniu, nazwy użytkownika i hasła.</span><span class="sxs-lookup"><span data-stu-id="7701d-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="7701d-109">Zazwyczaj aplikację wystawcy uwierzytelnienia jest instalowany na smartfonie.</span><span class="sxs-lookup"><span data-stu-id="7701d-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="7701d-110">Szablony aplikacji sieci web platformy ASP.NET Core obsługuje wystawcy uwierzytelnienia, ale nie zapewnia pomoc techniczną dla QRCode generacji.</span><span class="sxs-lookup"><span data-stu-id="7701d-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="7701d-111">Generatory QRCode jej obsługi ułatwiają realizację konfigurowania funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="7701d-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="7701d-112">Ten dokument przeprowadzi Cię przez proces dodawania [kod QR](https://wikipedia.org/wiki/QR_code) generacji do strony konfiguracji uwierzytelniania 2FA.</span><span class="sxs-lookup"><span data-stu-id="7701d-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="7701d-113">Dodawanie kody QR do strony konfiguracji funkcji 2FA</span><span class="sxs-lookup"><span data-stu-id="7701d-113">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="7701d-114">W poniższych instrukcjach użyto *qrcode.js* z https://davidshimjs.github.io/qrcodejs/ repozytorium.</span><span class="sxs-lookup"><span data-stu-id="7701d-114">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="7701d-115">Pobierz [biblioteki javascript qrcode.js](https://davidshimjs.github.io/qrcodejs/) do `wwwroot\lib` folder w projekcie.</span><span class="sxs-lookup"><span data-stu-id="7701d-115">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="7701d-116">Postępuj zgodnie z instrukcjami w [tożsamości szkieletu](xref:security/authentication/scaffold-identity) do generowania */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7701d-116">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="7701d-117">W */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, zlokalizuj `Scripts` sekcji na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="7701d-117">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="7701d-118">W *Pages/Account/Manage/EnableAuthenticator.cshtml* (stron Razor) lub *Views/Manage/EnableAuthenticator.cshtml* (MVC), Znajdź `Scripts` sekcji na końcu pliku:</span><span class="sxs-lookup"><span data-stu-id="7701d-118">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="7701d-119">Aktualizacja `Scripts` sekcji, aby dodać odwołanie do `qrcodejs` biblioteki został dodany i wywołania do generowania kodu QR.</span><span class="sxs-lookup"><span data-stu-id="7701d-119">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="7701d-120">Powinno to wyglądać w następujący sposób:</span><span class="sxs-lookup"><span data-stu-id="7701d-120">It should look as follows:</span></span>

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

* <span data-ttu-id="7701d-121">Usuń akapit, który podano łącza do niniejszych instrukcji.</span><span class="sxs-lookup"><span data-stu-id="7701d-121">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="7701d-122">Uruchom aplikację i upewnij się, aby zeskanować kod QR i sprawdzanie poprawności kodu, który okaże się wystawcy uwierzytelnienia.</span><span class="sxs-lookup"><span data-stu-id="7701d-122">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="7701d-123">Zmień nazwę lokacji w kod QR</span><span class="sxs-lookup"><span data-stu-id="7701d-123">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7701d-124">Nazwa witryny w kod QR jest pobierana z nazwę projektu, który wybierzesz podczas początkowego tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="7701d-124">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="7701d-125">Można go zmienić, wyszukując `GenerateQrCodeUri(string email, string unformattedKey)` method in Class metoda */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="7701d-125">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="7701d-126">Nazwa witryny w kod QR jest pobierana z nazwę projektu, który wybierzesz podczas początkowego tworzenia projektu.</span><span class="sxs-lookup"><span data-stu-id="7701d-126">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="7701d-127">Można go zmienić, wyszukując `GenerateQrCodeUri(string email, string unformattedKey)` method in Class metoda *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* pliku (stron Razor) lub *Controllers/ManageController.cs* pliku (MVC).</span><span class="sxs-lookup"><span data-stu-id="7701d-127">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="7701d-128">Domyślny kod z szablonu wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="7701d-128">The default code from the template looks as follows:</span></span>

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

<span data-ttu-id="7701d-129">Drugi parametr w wywołaniu `string.Format` jest nazwą witryny z nazwą rozwiązania.</span><span class="sxs-lookup"><span data-stu-id="7701d-129">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="7701d-130">Można ją zmienić na jakąkolwiek wartość, ale zawsze musi być zakodowane w adresie URL.</span><span class="sxs-lookup"><span data-stu-id="7701d-130">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="7701d-131">Przy użyciu innej biblioteki kodu QR</span><span class="sxs-lookup"><span data-stu-id="7701d-131">Using a different QR Code library</span></span>

<span data-ttu-id="7701d-132">Biblioteka kodów QR można zastąpić preferowanych biblioteki.</span><span class="sxs-lookup"><span data-stu-id="7701d-132">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="7701d-133">Zawiera kod HTML `qrCode` zawiera biblioteki elementu, w którym można umieszczać kod QR przy użyciu dowolnego mechanizmu.</span><span class="sxs-lookup"><span data-stu-id="7701d-133">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="7701d-134">Poprawnie sformatowany adres URL kod QR jest dostępna w:</span><span class="sxs-lookup"><span data-stu-id="7701d-134">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="7701d-135">`AuthenticatorUri` właściwości modelu.</span><span class="sxs-lookup"><span data-stu-id="7701d-135">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="7701d-136">`data-url` Właściwość `qrCodeData` elementu.</span><span class="sxs-lookup"><span data-stu-id="7701d-136">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="7701d-137">TOTP niesymetryczność czasu klienta i serwera</span><span class="sxs-lookup"><span data-stu-id="7701d-137">TOTP client and server time skew</span></span>

<span data-ttu-id="7701d-138">Uwierzytelnianie TOTP (oparte na czasie hasła jednorazowego) zależy od serwera i uwierzytelniania urządzeń mających dokładnego czasu.</span><span class="sxs-lookup"><span data-stu-id="7701d-138">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="7701d-139">Tokeny trwać tylko przez 30 sekund.</span><span class="sxs-lookup"><span data-stu-id="7701d-139">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="7701d-140">Logowania do uwierzytelniania 2FA TOTP kończą się niepowodzeniem, sprawdź, czy czas serwera jest dokładne i najlepiej zsynchronizowane z usługą NTP dokładne.</span><span class="sxs-lookup"><span data-stu-id="7701d-140">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
