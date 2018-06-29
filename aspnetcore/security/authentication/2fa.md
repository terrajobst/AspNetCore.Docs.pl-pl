---
title: Uwierzytelnianie dwuskładnikowe z programem SMS w ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) z aplikacją ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 08/15/2017
uid: security/authentication/2fa
ms.openlocfilehash: 0308b05ebcda1af7f6850549d7a33f1df1a912a0
ms.sourcegitcommit: 1faf2525902236428dae6a59e375519bafd5d6d7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37089987"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="3065b-103">Uwierzytelnianie dwuskładnikowe z programem SMS w ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3065b-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="3065b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Devs Szwajcaria](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="3065b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

 <span data-ttu-id="3065b-105">Dwa współczynnik uwierzytelniania (2FA) aplikacje uwierzytelniania, przy użyciu opartego na czasie jednorazowe hasła algorytmu (TOTP), są zalecane podejście do 2FA branży.</span><span class="sxs-lookup"><span data-stu-id="3065b-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="3065b-106">2FA przy użyciu TOTP jest preferowana względem 2FA programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3065b-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="3065b-107">Aby uzyskać więcej informacji, zobacz [generowania kodu QR włączyć TOTP uwierzytelniania aplikacji w ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) dla platformy ASP.NET Core 2.0 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="3065b-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="3065b-108">Ten samouczek pokazuje, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3065b-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="3065b-109">Instrukcje są podane dla [usługi twilio](https://www.twilio.com/) i [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale można użyć dowolnego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3065b-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="3065b-110">Firma Microsoft zaleca, należy wykonać [potwierdzenie konta i hasła odzyskiwania](xref:security/authentication/accconfirm) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="3065b-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="3065b-111">Widok [ukończone próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="3065b-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="3065b-112">[Jak pobrać](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="3065b-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="3065b-113">Utwórz nowy projekt platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3065b-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="3065b-114">Utwórz nową aplikację sieci web platformy ASP.NET Core o nazwie `Web2FA` z indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="3065b-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="3065b-115">Postępuj zgodnie z instrukcjami [Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core](xref:security/enforcing-ssl) do ustawiania i wymagać protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="3065b-115">Follow the instructions in [Enforce SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="3065b-116">Tworzenie konta usługi programu SMS</span><span class="sxs-lookup"><span data-stu-id="3065b-116">Create an SMS account</span></span>

<span data-ttu-id="3065b-117">Tworzenie konta usługi programu SMS, na przykład z [usługi twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="3065b-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="3065b-118">Zapisz poświadczenia uwierzytelniania (dla usługi twilio: accountSid i parametr authToken dla ASPSMS: Userkey i hasło).</span><span class="sxs-lookup"><span data-stu-id="3065b-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="3065b-119">Ustaleniem poświadczenia dostawcy programu SMS</span><span class="sxs-lookup"><span data-stu-id="3065b-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="3065b-120">**Usługi Twilio:** z karty Pulpit nawigacyjny konta usługi Twilio, skopiuj **identyfikator SID konta** i **token uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="3065b-120">**Twilio:** From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="3065b-121">**ASPSMS:** z ustawienia konta, przejdź do **Userkey** i skopiować go razem z Twojej **hasło**.</span><span class="sxs-lookup"><span data-stu-id="3065b-121">**ASPSMS:** From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="3065b-122">Przechowujemy później te wartości przy użyciu narzędzia menedżera klucza tajnego w kluczach `SMSAccountIdentification` i `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="3065b-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="3065b-123">Określanie SenderID / inicjator</span><span class="sxs-lookup"><span data-stu-id="3065b-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="3065b-124">**Usługi Twilio:** z karty liczb, skopiować z usługi Twilio **numer telefonu**.</span><span class="sxs-lookup"><span data-stu-id="3065b-124">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="3065b-125">**ASPSMS:** w Menu nadawcy odblokować Odblokuj co najmniej jednego nadawcy, lub Wybierz zleceniodawcę alfanumeryczne (nieobsługiwane przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="3065b-125">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="3065b-126">Przechowujemy później tę wartość za pomocą narzędzia menedżera klucza tajnego w kluczu `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="3065b-126">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="3065b-127">Podaj poświadczenia dla usługi programu SMS</span><span class="sxs-lookup"><span data-stu-id="3065b-127">Provide credentials for the SMS service</span></span>

<span data-ttu-id="3065b-128">Użyjemy [wzorzec opcje](xref:fundamentals/configuration/options) uzyskać dostęp do konta i klucz ustawień użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3065b-128">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

   * <span data-ttu-id="3065b-129">Utwórz klasę, aby pobrać klucz zabezpieczeń programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3065b-129">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="3065b-130">Dla tego przykładu `SMSoptions` klasy jest tworzony w *Services/SMSoptions.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="3065b-130">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="3065b-131">Ustaw `SMSAccountIdentification`, `SMSAccountPassword` i `SMSAccountFrom` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3065b-131">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="3065b-132">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3065b-132">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="3065b-133">Dodaj pakiet NuGet dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3065b-133">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="3065b-134">Z pakietu Menedżera konsoli (PMC) uruchom:</span><span class="sxs-lookup"><span data-stu-id="3065b-134">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="3065b-135">**Usługi Twilio:**
`Install-Package Twilio`</span><span class="sxs-lookup"><span data-stu-id="3065b-135">**Twilio:**
`Install-Package Twilio`</span></span>

<span data-ttu-id="3065b-136">**ASPSMS:**
`Install-Package ASPSMS`</span><span class="sxs-lookup"><span data-stu-id="3065b-136">**ASPSMS:**
`Install-Package ASPSMS`</span></span>


* <span data-ttu-id="3065b-137">Dodaj kod w *Services/MessageServices.cs* plik w celu włączenia programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3065b-137">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="3065b-138">Przy użyciu usługi Twilio lub sekcji ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3065b-138">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="3065b-139">**Usługi Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="3065b-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="3065b-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="3065b-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="3065b-141">Konfigurowanie uruchamiania do użycia `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="3065b-141">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="3065b-142">Dodaj `SMSoptions` do kontenera usługi w `ConfigureServices` metody w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3065b-142">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="3065b-143">Włącz uwierzytelnianie dwuskładnikowe</span><span class="sxs-lookup"><span data-stu-id="3065b-143">Enable two-factor authentication</span></span>

<span data-ttu-id="3065b-144">Otwórz *Views/Manage/Index.cshtml* pliku widoku Razor i Usuń komentarz znaków (taki sposób, nie ujęty w komentarzu).</span><span class="sxs-lookup"><span data-stu-id="3065b-144">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="3065b-145">Zaloguj się za pomocą uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3065b-145">Log in with two-factor authentication</span></span>

* <span data-ttu-id="3065b-146">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="3065b-146">Run the app and register a new user</span></span>

![Otwórz w programie Microsoft Edge widok zarejestrować aplikacji sieci Web](2fa/_static/login2fa1.png)

* <span data-ttu-id="3065b-148">Wybierz nazwę użytkownika, który uaktywnia `Index` metody akcji w kontrolerze zarządzanie.</span><span class="sxs-lookup"><span data-stu-id="3065b-148">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="3065b-149">Następnie wybierz numer telefonu **Dodaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="3065b-149">Then tap the phone number **Add** link.</span></span>

![Zarządzanie widoku](2fa/_static/login2fa2.png)

* <span data-ttu-id="3065b-151">Numer telefonu, który zostanie wyświetlony kod weryfikacyjny i naciśnij pozycję Dodaj **wysłać kod weryfikacyjny**.</span><span class="sxs-lookup"><span data-stu-id="3065b-151">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Dodaj stronę numer telefonu](2fa/_static/login2fa3.png)

* <span data-ttu-id="3065b-153">Otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="3065b-153">You will get a text message with the verification code.</span></span> <span data-ttu-id="3065b-154">Wprowadź go i wybierz **przesyłania**</span><span class="sxs-lookup"><span data-stu-id="3065b-154">Enter it and tap **Submit**</span></span>

![Sprawdź numer telefonu strony](2fa/_static/login2fa4.png)

<span data-ttu-id="3065b-156">Jeśli nie otrzymasz wiadomość SMS, zobacz stronę dziennika usługi twilio.</span><span class="sxs-lookup"><span data-stu-id="3065b-156">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="3065b-157">Zarządzaj ilustracja pokazuje numer telefonu został pomyślnie dodany.</span><span class="sxs-lookup"><span data-stu-id="3065b-157">The Manage view shows your phone number was added successfully.</span></span>

![Zarządzanie widoku](2fa/_static/login2fa5.png)

* <span data-ttu-id="3065b-159">Wybierz **włączyć** do włączenia uwierzytelniania dwuskładnikowego.</span><span class="sxs-lookup"><span data-stu-id="3065b-159">Tap **Enable** to enable two-factor authentication.</span></span>

![Zarządzanie widoku](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="3065b-161">Uwierzytelnianie dwuskładnikowe testu</span><span class="sxs-lookup"><span data-stu-id="3065b-161">Test two-factor authentication</span></span>

* <span data-ttu-id="3065b-162">Wyloguj.</span><span class="sxs-lookup"><span data-stu-id="3065b-162">Log off.</span></span>

* <span data-ttu-id="3065b-163">Zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="3065b-163">Log in.</span></span>

* <span data-ttu-id="3065b-164">Konto użytkownika ma włączony uwierzytelniania dwuskładnikowego, dlatego należy podać drugiego etapu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="3065b-164">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="3065b-165">W tym samouczku włączono weryfikację telefoniczną.</span><span class="sxs-lookup"><span data-stu-id="3065b-165">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="3065b-166">Wbudowane szablony pozwalają również na konfigurowanie poczty e-mail jako drugiego etapu.</span><span class="sxs-lookup"><span data-stu-id="3065b-166">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="3065b-167">Można skonfigurować dodatkowe czynniki drugi do uwierzytelniania, takich jak kody QR.</span><span class="sxs-lookup"><span data-stu-id="3065b-167">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="3065b-168">Wybierz **przesłać**.</span><span class="sxs-lookup"><span data-stu-id="3065b-168">Tap **Submit**.</span></span>

![Wyślij kod weryfikacyjny widoku](2fa/_static/login2fa7.png)

* <span data-ttu-id="3065b-170">Wprowadź kod w wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="3065b-170">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="3065b-171">Kliknięcie **Pamiętaj przeglądarka** pola wyboru będzie zwalnia z konieczności korzystania z 2FA się zalogować, korzystając z tego samego urządzenia i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3065b-171">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="3065b-172">Włączanie 2FA i klikając **Pamiętaj przeglądarka** zapewnia 2FA silnej ochrony przed złośliwymi użytkownikami próby dostęp do tego konta, dopóki nie mają dostępu do urządzenia.</span><span class="sxs-lookup"><span data-stu-id="3065b-172">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="3065b-173">Można to zrobić na dowolnym urządzeniu prywatne, regularnie używane.</span><span class="sxs-lookup"><span data-stu-id="3065b-173">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="3065b-174">Przez ustawienie **Pamiętaj przeglądarka**, Pobierz zwiększa bezpieczeństwo 2FA z urządzeń, które nie są regularnie używane i pobrać wygody na ominięcie za pośrednictwem 2FA na własnych urządzeniach.</span><span class="sxs-lookup"><span data-stu-id="3065b-174">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Sprawdź widok](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="3065b-176">Blokada konta do ochrony przed atakami siłowymi</span><span class="sxs-lookup"><span data-stu-id="3065b-176">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="3065b-177">Blokada konta jest zalecane z 2FA.</span><span class="sxs-lookup"><span data-stu-id="3065b-177">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="3065b-178">Po zalogowaniu się za pomocą konta lokalnego lub konta społecznościowych każdego nieudane próby 2FA jest przechowywany.</span><span class="sxs-lookup"><span data-stu-id="3065b-178">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="3065b-179">Po osiągnięciu maksymalnej nieudanych prób dostępu użytkownika jest zablokowane (domyślne: 5 minut blokady po 5 nieudanych prób dostępu).</span><span class="sxs-lookup"><span data-stu-id="3065b-179">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="3065b-180">Po pomyślnym uwierzytelnieniu Resetuje licznik nieudanych prób dostępu prób i zresetowanie zegara.</span><span class="sxs-lookup"><span data-stu-id="3065b-180">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="3065b-181">Maksymalną liczbę nieudanych prób dostępu i czasu blokady można ustawić za pomocą [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) i [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="3065b-181">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="3065b-182">Następujące konfiguruje blokady konta przez 10 minut po 10 nieudanych prób dostępu:</span><span class="sxs-lookup"><span data-stu-id="3065b-182">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="3065b-183">Upewnij się, że [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ustawia `lockoutOnFailure` do `true`:</span><span class="sxs-lookup"><span data-stu-id="3065b-183">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
