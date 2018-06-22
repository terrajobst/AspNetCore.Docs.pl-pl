---
title: Uwierzytelnianie dwuskładnikowe z programem SMS w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) z aplikacją ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
uid: security/authentication/2fa
ms.openlocfilehash: 335edfd5cd4dfbb9d223ba0ae888a6d2386cd4a5
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272312"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="20bd2-103">Uwierzytelnianie dwuskładnikowe z programem SMS w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20bd2-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="20bd2-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Devs Szwajcaria](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="20bd2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="20bd2-105">Zobacz [generowanie kodu QR włączyć dla aplikacji uwierzytelniania w ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) dla platformy ASP.NET Core 2.0 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="20bd2-105">See [Enable QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="20bd2-106">Ten samouczek pokazuje, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS.</span><span class="sxs-lookup"><span data-stu-id="20bd2-106">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="20bd2-107">Instrukcje są podane dla [usługi twilio](https://www.twilio.com/) i [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale można użyć dowolnego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="20bd2-107">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="20bd2-108">Firma Microsoft zaleca, należy wykonać [potwierdzenie konta i hasła odzyskiwania](xref:security/authentication/accconfirm) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="20bd2-108">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="20bd2-109">Widok [ukończone próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="20bd2-109">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="20bd2-110">[Jak pobrać](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="20bd2-110">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="20bd2-111">Utwórz nowy projekt platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20bd2-111">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="20bd2-112">Utwórz nową aplikację sieci web platformy ASP.NET Core o nazwie `Web2FA` z indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="20bd2-112">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="20bd2-113">Postępuj zgodnie z instrukcjami [Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core](xref:security/enforcing-ssl) do ustawiania i wymagać protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="20bd2-113">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="20bd2-114">Tworzenie konta usługi programu SMS</span><span class="sxs-lookup"><span data-stu-id="20bd2-114">Create an SMS account</span></span>

<span data-ttu-id="20bd2-115">Tworzenie konta usługi programu SMS, na przykład z [usługi twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="20bd2-115">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="20bd2-116">Zapisz poświadczenia uwierzytelniania (dla usługi twilio: accountSid i parametr authToken dla ASPSMS: Userkey i hasło).</span><span class="sxs-lookup"><span data-stu-id="20bd2-116">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="20bd2-117">Ustaleniem poświadczenia dostawcy programu SMS</span><span class="sxs-lookup"><span data-stu-id="20bd2-117">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="20bd2-118">**Usługi Twilio:**</span><span class="sxs-lookup"><span data-stu-id="20bd2-118">**Twilio:**</span></span>  
<span data-ttu-id="20bd2-119">Z konta usługi Twilio, na karcie pulpitu nawigacyjnego, skopiuj **identyfikator SID konta** i **token uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="20bd2-119">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="20bd2-120">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="20bd2-120">**ASPSMS:**</span></span>  
<span data-ttu-id="20bd2-121">W ustawieniach konta, przejdź do **Userkey** i skopiować go razem z Twojej **hasło**.</span><span class="sxs-lookup"><span data-stu-id="20bd2-121">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="20bd2-122">Przechowujemy później te wartości przy użyciu narzędzia menedżera klucza tajnego w kluczach `SMSAccountIdentification` i `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="20bd2-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="20bd2-123">Określanie SenderID / inicjator</span><span class="sxs-lookup"><span data-stu-id="20bd2-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="20bd2-124">**Usługi Twilio:**</span><span class="sxs-lookup"><span data-stu-id="20bd2-124">**Twilio:**</span></span>  
<span data-ttu-id="20bd2-125">Na karcie liczb, skopiować z usługi Twilio **numer telefonu**.</span><span class="sxs-lookup"><span data-stu-id="20bd2-125">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="20bd2-126">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="20bd2-126">**ASPSMS:**</span></span>  
<span data-ttu-id="20bd2-127">W Menu nadawcy odblokować Odblokuj co najmniej jednego nadawcy, lub Wybierz zleceniodawcę alfanumeryczne (nieobsługiwane przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="20bd2-127">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="20bd2-128">Przechowujemy później tę wartość za pomocą narzędzia menedżera klucza tajnego w kluczu `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="20bd2-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="20bd2-129">Podaj poświadczenia dla usługi programu SMS</span><span class="sxs-lookup"><span data-stu-id="20bd2-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="20bd2-130">Użyjemy [wzorzec opcje](xref:fundamentals/configuration/options) uzyskać dostęp do konta i klucz ustawień użytkownika.</span><span class="sxs-lookup"><span data-stu-id="20bd2-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="20bd2-131">Utwórz klasę, aby pobrać klucz zabezpieczeń programu SMS.</span><span class="sxs-lookup"><span data-stu-id="20bd2-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="20bd2-132">Dla tego przykładu `SMSoptions` klasy jest tworzony w *Services/SMSoptions.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="20bd2-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="20bd2-133">Ustaw `SMSAccountIdentification`, `SMSAccountPassword` i `SMSAccountFrom` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="20bd2-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="20bd2-134">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="20bd2-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="20bd2-135">Dodaj pakiet NuGet dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="20bd2-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="20bd2-136">Z pakietu Menedżera konsoli (PMC) uruchom:</span><span class="sxs-lookup"><span data-stu-id="20bd2-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="20bd2-137">**Usługi Twilio:**</span><span class="sxs-lookup"><span data-stu-id="20bd2-137">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="20bd2-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="20bd2-138">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="20bd2-139">Dodaj kod w *Services/MessageServices.cs* plik w celu włączenia programu SMS.</span><span class="sxs-lookup"><span data-stu-id="20bd2-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="20bd2-140">Przy użyciu usługi Twilio lub sekcji ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="20bd2-140">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="20bd2-141">**Usługi Twilio:**</span><span class="sxs-lookup"><span data-stu-id="20bd2-141">**Twilio:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="20bd2-142">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="20bd2-142">**ASPSMS:**</span></span>  
[!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="20bd2-143">Konfigurowanie uruchamiania do użycia `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="20bd2-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="20bd2-144">Dodaj `SMSoptions` do kontenera usługi w `ConfigureServices` metody w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="20bd2-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="20bd2-145">Włącz uwierzytelnianie dwuskładnikowe</span><span class="sxs-lookup"><span data-stu-id="20bd2-145">Enable two-factor authentication</span></span>

<span data-ttu-id="20bd2-146">Otwórz *Views/Manage/Index.cshtml* pliku widoku Razor i Usuń komentarz znaków (taki sposób, nie ujęty w komentarzu).</span><span class="sxs-lookup"><span data-stu-id="20bd2-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="20bd2-147">Zaloguj się za pomocą uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="20bd2-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="20bd2-148">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="20bd2-148">Run the app and register a new user</span></span>

![Otwórz w programie Microsoft Edge widok zarejestrować aplikacji sieci Web](2fa/_static/login2fa1.png)

* <span data-ttu-id="20bd2-150">Wybierz nazwę użytkownika, który uaktywnia `Index` metody akcji w kontrolerze zarządzanie.</span><span class="sxs-lookup"><span data-stu-id="20bd2-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="20bd2-151">Następnie wybierz numer telefonu **Dodaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="20bd2-151">Then tap the phone number **Add** link.</span></span>

![Zarządzanie widoku](2fa/_static/login2fa2.png)

* <span data-ttu-id="20bd2-153">Numer telefonu, który zostanie wyświetlony kod weryfikacyjny i naciśnij pozycję Dodaj **wysłać kod weryfikacyjny**.</span><span class="sxs-lookup"><span data-stu-id="20bd2-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Dodaj stronę numer telefonu](2fa/_static/login2fa3.png)

* <span data-ttu-id="20bd2-155">Otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="20bd2-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="20bd2-156">Wprowadź go i wybierz **przesyłania**</span><span class="sxs-lookup"><span data-stu-id="20bd2-156">Enter it and tap **Submit**</span></span>

![Sprawdź numer telefonu strony](2fa/_static/login2fa4.png)

<span data-ttu-id="20bd2-158">Jeśli nie otrzymasz wiadomość SMS, zobacz stronę dziennika usługi twilio.</span><span class="sxs-lookup"><span data-stu-id="20bd2-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="20bd2-159">Zarządzaj ilustracja pokazuje numer telefonu został pomyślnie dodany.</span><span class="sxs-lookup"><span data-stu-id="20bd2-159">The Manage view shows your phone number was added successfully.</span></span>

![Zarządzanie widoku](2fa/_static/login2fa5.png)

* <span data-ttu-id="20bd2-161">Wybierz **włączyć** do włączenia uwierzytelniania dwuskładnikowego.</span><span class="sxs-lookup"><span data-stu-id="20bd2-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Zarządzanie widoku](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="20bd2-163">Uwierzytelnianie dwuskładnikowe testu</span><span class="sxs-lookup"><span data-stu-id="20bd2-163">Test two-factor authentication</span></span>

* <span data-ttu-id="20bd2-164">Wyloguj.</span><span class="sxs-lookup"><span data-stu-id="20bd2-164">Log off.</span></span>

* <span data-ttu-id="20bd2-165">Zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="20bd2-165">Log in.</span></span>

* <span data-ttu-id="20bd2-166">Konto użytkownika ma włączony uwierzytelniania dwuskładnikowego, dlatego należy podać drugiego etapu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="20bd2-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="20bd2-167">W tym samouczku włączono weryfikację telefoniczną.</span><span class="sxs-lookup"><span data-stu-id="20bd2-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="20bd2-168">Wbudowane szablony pozwalają również na konfigurowanie poczty e-mail jako drugiego etapu.</span><span class="sxs-lookup"><span data-stu-id="20bd2-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="20bd2-169">Można skonfigurować dodatkowe czynniki drugi do uwierzytelniania, takich jak kody QR.</span><span class="sxs-lookup"><span data-stu-id="20bd2-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="20bd2-170">Wybierz **przesłać**.</span><span class="sxs-lookup"><span data-stu-id="20bd2-170">Tap **Submit**.</span></span>

![Wyślij kod weryfikacyjny widoku](2fa/_static/login2fa7.png)

* <span data-ttu-id="20bd2-172">Wprowadź kod w wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="20bd2-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="20bd2-173">Kliknięcie **Pamiętaj przeglądarka** pola wyboru będzie zwalnia z konieczności korzystania z 2FA się zalogować, korzystając z tego samego urządzenia i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="20bd2-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="20bd2-174">Włączanie 2FA i klikając **Pamiętaj przeglądarka** zapewnia 2FA silnej ochrony przed złośliwymi użytkownikami próby dostęp do tego konta, dopóki nie mają dostępu do urządzenia.</span><span class="sxs-lookup"><span data-stu-id="20bd2-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="20bd2-175">Można to zrobić na dowolnym urządzeniu prywatne, regularnie używane.</span><span class="sxs-lookup"><span data-stu-id="20bd2-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="20bd2-176">Przez ustawienie **Pamiętaj przeglądarka**, Pobierz zwiększa bezpieczeństwo 2FA z urządzeń, które nie są regularnie używane i pobrać wygody na ominięcie za pośrednictwem 2FA na własnych urządzeniach.</span><span class="sxs-lookup"><span data-stu-id="20bd2-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Sprawdź widok](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="20bd2-178">Blokada konta do ochrony przed atakami siłowymi</span><span class="sxs-lookup"><span data-stu-id="20bd2-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="20bd2-179">Blokada konta jest zalecane z 2FA.</span><span class="sxs-lookup"><span data-stu-id="20bd2-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="20bd2-180">Po zalogowaniu się za pomocą konta lokalnego lub konta społecznościowych każdego nieudane próby 2FA jest przechowywany.</span><span class="sxs-lookup"><span data-stu-id="20bd2-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="20bd2-181">Po osiągnięciu maksymalnej nieudanych prób dostępu użytkownika jest zablokowane (domyślne: 5 minut blokady po 5 nieudanych prób dostępu).</span><span class="sxs-lookup"><span data-stu-id="20bd2-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="20bd2-182">Po pomyślnym uwierzytelnieniu Resetuje licznik nieudanych prób dostępu prób i zresetowanie zegara.</span><span class="sxs-lookup"><span data-stu-id="20bd2-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="20bd2-183">Maksymalną liczbę nieudanych prób dostępu i czasu blokady można ustawić za pomocą [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) i [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="20bd2-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="20bd2-184">Następujące konfiguruje blokady konta przez 10 minut po 10 nieudanych prób dostępu:</span><span class="sxs-lookup"><span data-stu-id="20bd2-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="20bd2-185">Upewnij się, że [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ustawia `lockoutOnFailure` do `true`:</span><span class="sxs-lookup"><span data-stu-id="20bd2-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
