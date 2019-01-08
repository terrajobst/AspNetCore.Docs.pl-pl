---
title: Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) za pomocą aplikacji platformy ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 48bfc50378fc0ec212f5b9d4e7ce05bb4fc97b9d
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098899"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="ee61b-103">Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee61b-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="ee61b-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Swiss deweloperów](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="ee61b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="ee61b-105">Dwa Authentication uwierzytelnianie (2FA) uwierzytelniania aplikacji przy użyciu opartych na czasie jednorazowe hasła algorytm (TOTP), są zalecane podejście do uwierzytelniania 2FA.</span><span class="sxs-lookup"><span data-stu-id="ee61b-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="ee61b-106">Funkcji 2FA przy użyciu TOTP jest preferowany dla wiadomości SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="ee61b-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="ee61b-107">Aby uzyskać więcej informacji, zobacz [generowanie kodu QR Włącz dla TOTP aplikacje uwierzytelniania w programie ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) dla platformy ASP.NET Core 2.0 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="ee61b-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="ee61b-108">W tym samouczku pokazano, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS.</span><span class="sxs-lookup"><span data-stu-id="ee61b-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="ee61b-109">Instrukcje są podane dla [twilio](https://www.twilio.com/) i [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale można użyć dowolnego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="ee61b-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="ee61b-110">Firma Microsoft zaleca, po ukończeniu [potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="ee61b-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="ee61b-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="ee61b-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="ee61b-112">[Jak pobrać](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="ee61b-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="ee61b-113">Utwórz nowy projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ee61b-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="ee61b-114">Utwórz nową aplikację sieci web platformy ASP.NET Core o nazwie `Web2FA` za pomocą indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="ee61b-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="ee61b-115">Postępuj zgodnie z instrukcjami <xref:security/enforcing-ssl> skonfigurować i włączyć wymaganie protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="ee61b-115">Follow the instructions in <xref:security/enforcing-ssl> to set up and require HTTPS.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="ee61b-116">Tworzenie konta usługi programu SMS</span><span class="sxs-lookup"><span data-stu-id="ee61b-116">Create an SMS account</span></span>

<span data-ttu-id="ee61b-117">Tworzenie konta usługi programu SMS, na przykład z [twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="ee61b-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="ee61b-118">Zapisz poświadczenia uwierzytelniania (usłudze twilio: accountSid i authToken dla ASPSMS: Userkey i hasło).</span><span class="sxs-lookup"><span data-stu-id="ee61b-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="ee61b-119">Ustalenie, poświadczenia dostawcy programu SMS</span><span class="sxs-lookup"><span data-stu-id="ee61b-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="ee61b-120">**Twilio:** Karta pulpitu nawigacyjnego konta usługi Twilio, skopiuj **identyfikator SID konta** i **tokenu uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="ee61b-120">**Twilio:** From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="ee61b-121">**ASPSMS:** W ustawieniach konta, przejdź do **Userkey** i skopiować go razem z Twojej **hasło**.</span><span class="sxs-lookup"><span data-stu-id="ee61b-121">**ASPSMS:** From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="ee61b-122">Później będą przechowywane te wartości przy użyciu narzędzia menedżera klucz tajny w kluczach `SMSAccountIdentification` i `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="ee61b-122">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="ee61b-123">Określanie SenderID / inicjatora</span><span class="sxs-lookup"><span data-stu-id="ee61b-123">Specifying SenderID / Originator</span></span>

<span data-ttu-id="ee61b-124">**Twilio:** Na karcie liczby skopiuj usługi Twilio **numer telefonu**.</span><span class="sxs-lookup"><span data-stu-id="ee61b-124">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="ee61b-125">**ASPSMS:** W Menu nadawcy odblokować odblokować przynajmniej jednego nadawcy, lub wybierz inicjatora alfanumeryczne (nie obsługiwany przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="ee61b-125">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="ee61b-126">Później będą przechowywane w tej wartości za pomocą narzędzia menedżera klucz tajny w kluczu `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="ee61b-126">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="ee61b-127">Podaj poświadczenia dla usługi programu SMS</span><span class="sxs-lookup"><span data-stu-id="ee61b-127">Provide credentials for the SMS service</span></span>

<span data-ttu-id="ee61b-128">Użyjemy [wzorzec opcje](xref:fundamentals/configuration/options) uzyskać dostępu do ustawień konta i klucza użytkownika.</span><span class="sxs-lookup"><span data-stu-id="ee61b-128">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

   * <span data-ttu-id="ee61b-129">Utwórz klasę, można pobrać bezpieczny klucz wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="ee61b-129">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="ee61b-130">W tym przykładzie `SMSoptions` klasa jest tworzona w *Services/SMSoptions.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="ee61b-130">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="ee61b-131">Ustaw `SMSAccountIdentification`, `SMSAccountPassword` i `SMSAccountFrom` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="ee61b-131">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="ee61b-132">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="ee61b-132">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="ee61b-133">Dodaj pakiet NuGet dla dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="ee61b-133">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="ee61b-134">Z pakietu Menedżera konsoli (PMC) uruchom:</span><span class="sxs-lookup"><span data-stu-id="ee61b-134">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="ee61b-135">**Twilio:**
`Install-Package Twilio`</span><span class="sxs-lookup"><span data-stu-id="ee61b-135">**Twilio:**
`Install-Package Twilio`</span></span>

<span data-ttu-id="ee61b-136">**ASPSMS:**
`Install-Package ASPSMS`</span><span class="sxs-lookup"><span data-stu-id="ee61b-136">**ASPSMS:**
`Install-Package ASPSMS`</span></span>


* <span data-ttu-id="ee61b-137">Dodaj kod w *Services/MessageServices.cs* plik w celu włączenia programu SMS.</span><span class="sxs-lookup"><span data-stu-id="ee61b-137">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="ee61b-138">Przy użyciu usługi Twilio lub sekcji ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="ee61b-138">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="ee61b-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="ee61b-139">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="ee61b-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="ee61b-140">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="ee61b-141">Konfigurowanie uruchamiania do użycia `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="ee61b-141">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="ee61b-142">Dodaj `SMSoptions` do kontenera usługi w `ConfigureServices` method in Class metoda *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="ee61b-142">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="ee61b-143">Włączanie uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="ee61b-143">Enable two-factor authentication</span></span>

<span data-ttu-id="ee61b-144">Otwórz *Views/Manage/Index.cshtml* plik widoku Razor i Usuń komentarz znaków (taki sposób, nie ujęty w poziomie).</span><span class="sxs-lookup"><span data-stu-id="ee61b-144">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="ee61b-145">Zaloguj się przy użyciu uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="ee61b-145">Log in with two-factor authentication</span></span>

* <span data-ttu-id="ee61b-146">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="ee61b-146">Run the app and register a new user</span></span>

![Widok zarejestrować aplikacji sieci Web otwórz w programie Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="ee61b-148">Wybierz swoją nazwę użytkownika i aktywuje `Index` metody akcji kontrolera zarządzania.</span><span class="sxs-lookup"><span data-stu-id="ee61b-148">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="ee61b-149">Następnie wybierz numer telefonu **Dodaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="ee61b-149">Then tap the phone number **Add** link.</span></span>

![Zarządzanie widok — nacisnąć link "Dodaj"](2fa/_static/login2fa2.png)

* <span data-ttu-id="ee61b-151">Dodaj numer telefonu, który uzyskać kod weryfikacyjny i naciśnij pozycję **Wyślij kod weryfikacyjny**.</span><span class="sxs-lookup"><span data-stu-id="ee61b-151">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Dodaj stronę numer telefonu](2fa/_static/login2fa3.png)

* <span data-ttu-id="ee61b-153">Otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="ee61b-153">You will get a text message with the verification code.</span></span> <span data-ttu-id="ee61b-154">Wprowadź go, a następnie naciśnij przycisk **przesyłania**</span><span class="sxs-lookup"><span data-stu-id="ee61b-154">Enter it and tap **Submit**</span></span>

![Sprawdź numer telefonu strony](2fa/_static/login2fa4.png)

<span data-ttu-id="ee61b-156">Jeśli nie otrzymasz wiadomość SMS, zobacz stronę dziennika usługi twilio.</span><span class="sxs-lookup"><span data-stu-id="ee61b-156">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="ee61b-157">W widoku Zarządzanie pokazuje numer telefonu został pomyślnie dodany.</span><span class="sxs-lookup"><span data-stu-id="ee61b-157">The Manage view shows your phone number was added successfully.</span></span>

![Zarządzanie widok — pomyślnie dodano numer telefonu](2fa/_static/login2fa5.png)

* <span data-ttu-id="ee61b-159">Naciśnij pozycję **Włącz** do włączenia uwierzytelniania dwuskładnikowego.</span><span class="sxs-lookup"><span data-stu-id="ee61b-159">Tap **Enable** to enable two-factor authentication.</span></span>

![Zarządzanie widok — Włączanie uwierzytelniania dwuskładnikowego](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="ee61b-161">Uwierzytelnianie dwuskładnikowe testu</span><span class="sxs-lookup"><span data-stu-id="ee61b-161">Test two-factor authentication</span></span>

* <span data-ttu-id="ee61b-162">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="ee61b-162">Log off.</span></span>

* <span data-ttu-id="ee61b-163">Zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="ee61b-163">Log in.</span></span>

* <span data-ttu-id="ee61b-164">Konto użytkownika ma włączone uwierzytelnianie dwuskładnikowe, więc należy podać drugi składnik uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="ee61b-164">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="ee61b-165">W tym samouczku włączono weryfikację telefoniczną.</span><span class="sxs-lookup"><span data-stu-id="ee61b-165">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="ee61b-166">Wbudowane szablony pozwalają również na konfigurowanie poczty e-mail jako drugiego składnika.</span><span class="sxs-lookup"><span data-stu-id="ee61b-166">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="ee61b-167">Możesz skonfigurować dodatkowych czynników drugi uwierzytelniania, takiego jak kody QR.</span><span class="sxs-lookup"><span data-stu-id="ee61b-167">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="ee61b-168">Naciśnij pozycję **przesłać**.</span><span class="sxs-lookup"><span data-stu-id="ee61b-168">Tap **Submit**.</span></span>

![Wyślij kod weryfikacyjny widoku](2fa/_static/login2fa7.png)

* <span data-ttu-id="ee61b-170">Wprowadź kod, który jest pobierany w wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="ee61b-170">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="ee61b-171">Kliknięcie **pamiętasz tę przeglądarkę** pole wyboru zostaną wykluczone z konieczności korzystania z funkcji 2FA do logowania się w przypadku korzystania z tego samego urządzenia i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="ee61b-171">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="ee61b-172">Włączanie funkcji 2FA, a następnie klikając **pamiętasz tę przeglądarkę** udostępni silnego uwierzytelniania 2FA, ochronę przed złośliwymi użytkownikami podjęcie próby dostępu do Twojego konta, tak długo, jak nie mają dostępu do Twojego urządzenia.</span><span class="sxs-lookup"><span data-stu-id="ee61b-172">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="ee61b-173">Można to zrobić, na dowolnym urządzeniu prywatnych, regularnie używane.</span><span class="sxs-lookup"><span data-stu-id="ee61b-173">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="ee61b-174">Ustawiając **pamiętasz tę przeglądarkę**, uzyskaj dodatkowe zabezpieczenie w postaci funkcji 2FA z urządzeń, które nie są regularnie używane i Uzyskaj wygody na ominięcie za pomocą funkcji 2FA na własnych urządzeniach.</span><span class="sxs-lookup"><span data-stu-id="ee61b-174">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Sprawdź widok](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="ee61b-176">Blokada konta na potrzeby ochrony przed atakami siłowymi</span><span class="sxs-lookup"><span data-stu-id="ee61b-176">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="ee61b-177">Blokada konta zaleca się za pomocą funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="ee61b-177">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="ee61b-178">Po użytkownik loguje się przy użyciu konta lokalnego lub konta w sieci społecznościowej, znajduje się każda nieudanej próby w funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="ee61b-178">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="ee61b-179">W przypadku osiągnięcia maksymalnej nieudanych prób dostępu użytkownika jest zablokowane (domyślne: 5-minutowego blokady po 5 nieudanych prób dostępu).</span><span class="sxs-lookup"><span data-stu-id="ee61b-179">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="ee61b-180">Po pomyślnym uwierzytelnieniu resetuje liczbę prób dostępu nie powiodło się i zresetowanie zegara.</span><span class="sxs-lookup"><span data-stu-id="ee61b-180">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="ee61b-181">Maksymalną liczbę nieudanych prób dostępu i okres blokady, można ustawić za pomocą [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) i [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="ee61b-181">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="ee61b-182">Następujące konfiguruje blokady konta po upływie 10 minut po 10 nieudanych prób dostępu:</span><span class="sxs-lookup"><span data-stu-id="ee61b-182">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="ee61b-183">Upewnij się, że [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ustawia `lockoutOnFailure` do `true`:</span><span class="sxs-lookup"><span data-stu-id="ee61b-183">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
