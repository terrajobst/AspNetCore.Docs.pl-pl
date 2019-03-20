---
title: Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS w programie ASP.NET Core
author: rick-anderson
description: Dowiedz się, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) za pomocą aplikacji platformy ASP.NET Core.
monikerRange: < aspnetcore-2.0
ms.author: riande
ms.date: 09/22/2018
ms.custom: seodec18
uid: security/authentication/2fa
ms.openlocfilehash: 116249a7cd4faebd0c899e383d86f5c5c3c7146a
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58265239"
---
# <a name="two-factor-authentication-with-sms-in-aspnet-core"></a><span data-ttu-id="3baef-103">Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS w programie ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3baef-103">Two-factor authentication with SMS in ASP.NET Core</span></span>

<span data-ttu-id="3baef-104">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Swiss deweloperów](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="3baef-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

>[!WARNING]
> <span data-ttu-id="3baef-105">Dwa Authentication uwierzytelnianie (2FA) uwierzytelniania aplikacji przy użyciu opartych na czasie jednorazowe hasła algorytm (TOTP), są zalecane podejście do uwierzytelniania 2FA.</span><span class="sxs-lookup"><span data-stu-id="3baef-105">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="3baef-106">Funkcji 2FA przy użyciu TOTP jest preferowany dla wiadomości SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="3baef-106">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="3baef-107">Aby uzyskać więcej informacji, zobacz [generowanie kodu QR Włącz dla TOTP aplikacje uwierzytelniania w programie ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) dla platformy ASP.NET Core 2.0 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="3baef-107">For more information, see [Enable QR Code generation for TOTP authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="3baef-108">W tym samouczku pokazano, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3baef-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="3baef-109">Instrukcje są podane dla [twilio](https://www.twilio.com/) i [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale można użyć dowolnego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3baef-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="3baef-110">Firma Microsoft zaleca, po ukończeniu [potwierdzenie konta i odzyskiwanie hasła](xref:security/authentication/accconfirm) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="3baef-110">We recommend you complete [Account Confirmation and Password Recovery](xref:security/authentication/accconfirm) before starting this tutorial.</span></span>

<span data-ttu-id="3baef-111">[Wyświetlanie lub pobieranie przykładowego kodu](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="3baef-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="3baef-112">[Jak pobrać](xref:index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="3baef-112">[How to download](xref:index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="3baef-113">Utwórz nowy projekt ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3baef-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="3baef-114">Utwórz nową aplikację sieci web platformy ASP.NET Core o nazwie `Web2FA` za pomocą indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="3baef-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="3baef-115">Postępuj zgodnie z instrukcjami <xref:security/enforcing-ssl> skonfigurować i włączyć wymaganie protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3baef-115">Follow the instructions in <xref:security/enforcing-ssl> to set up and require HTTPS.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="3baef-116">Tworzenie konta usługi programu SMS</span><span class="sxs-lookup"><span data-stu-id="3baef-116">Create an SMS account</span></span>

<span data-ttu-id="3baef-117">Tworzenie konta usługi programu SMS, na przykład z [twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="3baef-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="3baef-118">Zapisz poświadczenia uwierzytelniania (usłudze twilio: accountSid i authToken dla ASPSMS: Userkey i hasło).</span><span class="sxs-lookup"><span data-stu-id="3baef-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="3baef-119">Ustalenie, poświadczenia dostawcy programu SMS</span><span class="sxs-lookup"><span data-stu-id="3baef-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="3baef-120">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="3baef-120">**Twilio:**</span></span>

<span data-ttu-id="3baef-121">Karta pulpitu nawigacyjnego konta usługi Twilio, skopiuj **identyfikator SID konta** i **tokenu uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="3baef-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="3baef-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="3baef-122">**ASPSMS:**</span></span>

<span data-ttu-id="3baef-123">W ustawieniach konta, przejdź do **Userkey** i skopiować go razem z Twojej **hasło**.</span><span class="sxs-lookup"><span data-stu-id="3baef-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="3baef-124">Później będą przechowywane te wartości przy użyciu narzędzia menedżera klucz tajny w kluczach `SMSAccountIdentification` i `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="3baef-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="3baef-125">Określanie SenderID / inicjatora</span><span class="sxs-lookup"><span data-stu-id="3baef-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="3baef-126">**Twilio:** Na karcie liczby skopiuj usługi Twilio **numer telefonu**.</span><span class="sxs-lookup"><span data-stu-id="3baef-126">**Twilio:** From the Numbers tab, copy your Twilio **phone number**.</span></span>

<span data-ttu-id="3baef-127">**ASPSMS:** W Menu nadawcy odblokować odblokować przynajmniej jednego nadawcy, lub wybierz inicjatora alfanumeryczne (nie obsługiwany przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="3baef-127">**ASPSMS:** Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>

<span data-ttu-id="3baef-128">Później będą przechowywane w tej wartości za pomocą narzędzia menedżera klucz tajny w kluczu `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="3baef-128">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>

### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="3baef-129">Podaj poświadczenia dla usługi programu SMS</span><span class="sxs-lookup"><span data-stu-id="3baef-129">Provide credentials for the SMS service</span></span>

<span data-ttu-id="3baef-130">Użyjemy [wzorzec opcje](xref:fundamentals/configuration/options) uzyskać dostępu do ustawień konta i klucza użytkownika.</span><span class="sxs-lookup"><span data-stu-id="3baef-130">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span>

* <span data-ttu-id="3baef-131">Utwórz klasę, można pobrać bezpieczny klucz wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="3baef-131">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="3baef-132">W tym przykładzie `SMSoptions` klasa jest tworzona w *Services/SMSoptions.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="3baef-132">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="3baef-133">Ustaw `SMSAccountIdentification`, `SMSAccountPassword` i `SMSAccountFrom` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="3baef-133">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="3baef-134">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="3baef-134">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```

* <span data-ttu-id="3baef-135">Dodaj pakiet NuGet dla dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3baef-135">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="3baef-136">Z pakietu Menedżera konsoli (PMC) uruchom:</span><span class="sxs-lookup"><span data-stu-id="3baef-136">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="3baef-137">**Twilio:**</span><span class="sxs-lookup"><span data-stu-id="3baef-137">**Twilio:**</span></span>

`Install-Package Twilio`

<span data-ttu-id="3baef-138">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="3baef-138">**ASPSMS:**</span></span>

`Install-Package ASPSMS`

* <span data-ttu-id="3baef-139">Dodaj kod w *Services/MessageServices.cs* plik w celu włączenia programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3baef-139">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="3baef-140">Przy użyciu usługi Twilio lub sekcji ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3baef-140">Use either the Twilio or the ASPSMS section:</span></span>

<span data-ttu-id="3baef-141">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span><span class="sxs-lookup"><span data-stu-id="3baef-141">**Twilio:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]</span></span>

<span data-ttu-id="3baef-142">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span><span class="sxs-lookup"><span data-stu-id="3baef-142">**ASPSMS:** [!code-csharp[](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]</span></span>

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="3baef-143">Konfigurowanie uruchamiania do użycia `SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="3baef-143">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="3baef-144">Dodaj `SMSoptions` do kontenera usługi w `ConfigureServices` method in Class metoda *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="3baef-144">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="3baef-145">Włączanie uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3baef-145">Enable two-factor authentication</span></span>

<span data-ttu-id="3baef-146">Otwórz *Views/Manage/Index.cshtml* plik widoku Razor i Usuń komentarz znaków (dzięki czemu żadnych znaczników jest zakomentowany).</span><span class="sxs-lookup"><span data-stu-id="3baef-146">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commented out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="3baef-147">Zaloguj się przy użyciu uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3baef-147">Log in with two-factor authentication</span></span>

* <span data-ttu-id="3baef-148">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="3baef-148">Run the app and register a new user</span></span>

![Widok zarejestrować aplikacji sieci Web otwórz w programie Microsoft Edge](2fa/_static/login2fa1.png)

* <span data-ttu-id="3baef-150">Wybierz swoją nazwę użytkownika i aktywuje `Index` metody akcji kontrolera zarządzania.</span><span class="sxs-lookup"><span data-stu-id="3baef-150">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="3baef-151">Następnie wybierz numer telefonu **Dodaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="3baef-151">Then tap the phone number **Add** link.</span></span>

![Zarządzanie widok — nacisnąć link "Dodaj"](2fa/_static/login2fa2.png)

* <span data-ttu-id="3baef-153">Dodaj numer telefonu, który uzyskać kod weryfikacyjny i naciśnij pozycję **Wyślij kod weryfikacyjny**.</span><span class="sxs-lookup"><span data-stu-id="3baef-153">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Dodaj stronę numer telefonu](2fa/_static/login2fa3.png)

* <span data-ttu-id="3baef-155">Otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="3baef-155">You will get a text message with the verification code.</span></span> <span data-ttu-id="3baef-156">Wprowadź go, a następnie naciśnij przycisk **przesyłania**</span><span class="sxs-lookup"><span data-stu-id="3baef-156">Enter it and tap **Submit**</span></span>

![Sprawdź numer telefonu strony](2fa/_static/login2fa4.png)

<span data-ttu-id="3baef-158">Jeśli nie otrzymasz wiadomość SMS, zobacz stronę dziennika usługi twilio.</span><span class="sxs-lookup"><span data-stu-id="3baef-158">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="3baef-159">W widoku Zarządzanie pokazuje numer telefonu został pomyślnie dodany.</span><span class="sxs-lookup"><span data-stu-id="3baef-159">The Manage view shows your phone number was added successfully.</span></span>

![Zarządzanie widok — pomyślnie dodano numer telefonu](2fa/_static/login2fa5.png)

* <span data-ttu-id="3baef-161">Naciśnij pozycję **Włącz** do włączenia uwierzytelniania dwuskładnikowego.</span><span class="sxs-lookup"><span data-stu-id="3baef-161">Tap **Enable** to enable two-factor authentication.</span></span>

![Zarządzanie widok — Włączanie uwierzytelniania dwuskładnikowego](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="3baef-163">Uwierzytelnianie dwuskładnikowe testu</span><span class="sxs-lookup"><span data-stu-id="3baef-163">Test two-factor authentication</span></span>

* <span data-ttu-id="3baef-164">Wyloguj się.</span><span class="sxs-lookup"><span data-stu-id="3baef-164">Log off.</span></span>

* <span data-ttu-id="3baef-165">Zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="3baef-165">Log in.</span></span>

* <span data-ttu-id="3baef-166">Konto użytkownika ma włączone uwierzytelnianie dwuskładnikowe, więc należy podać drugi składnik uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="3baef-166">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="3baef-167">W tym samouczku włączono weryfikację telefoniczną.</span><span class="sxs-lookup"><span data-stu-id="3baef-167">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="3baef-168">Wbudowane szablony pozwalają również na konfigurowanie poczty e-mail jako drugiego składnika.</span><span class="sxs-lookup"><span data-stu-id="3baef-168">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="3baef-169">Możesz skonfigurować dodatkowych czynników drugi uwierzytelniania, takiego jak kody QR.</span><span class="sxs-lookup"><span data-stu-id="3baef-169">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="3baef-170">Naciśnij pozycję **przesłać**.</span><span class="sxs-lookup"><span data-stu-id="3baef-170">Tap **Submit**.</span></span>

![Wyślij kod weryfikacyjny widoku](2fa/_static/login2fa7.png)

* <span data-ttu-id="3baef-172">Wprowadź kod, który jest pobierany w wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="3baef-172">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="3baef-173">Kliknięcie **pamiętasz tę przeglądarkę** pole wyboru zostaną wykluczone z konieczności korzystania z funkcji 2FA do logowania się w przypadku korzystania z tego samego urządzenia i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="3baef-173">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="3baef-174">Włączanie funkcji 2FA, a następnie klikając **pamiętasz tę przeglądarkę** udostępni silnego uwierzytelniania 2FA, ochronę przed złośliwymi użytkownikami podjęcie próby dostępu do Twojego konta, tak długo, jak nie mają dostępu do Twojego urządzenia.</span><span class="sxs-lookup"><span data-stu-id="3baef-174">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="3baef-175">Można to zrobić, na dowolnym urządzeniu prywatnych, regularnie używane.</span><span class="sxs-lookup"><span data-stu-id="3baef-175">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="3baef-176">Ustawiając **pamiętasz tę przeglądarkę**, uzyskaj dodatkowe zabezpieczenie w postaci funkcji 2FA z urządzeń, które nie są regularnie używane i Uzyskaj wygody na ominięcie za pomocą funkcji 2FA na własnych urządzeniach.</span><span class="sxs-lookup"><span data-stu-id="3baef-176">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Sprawdź widok](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="3baef-178">Blokada konta na potrzeby ochrony przed atakami siłowymi</span><span class="sxs-lookup"><span data-stu-id="3baef-178">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="3baef-179">Blokada konta zaleca się za pomocą funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="3baef-179">Account lockout is recommended with 2FA.</span></span> <span data-ttu-id="3baef-180">Po użytkownik loguje się przy użyciu konta lokalnego lub konta w sieci społecznościowej, znajduje się każda nieudanej próby w funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="3baef-180">Once a user signs in through a local account or social account, each failed attempt at 2FA is stored.</span></span> <span data-ttu-id="3baef-181">W przypadku osiągnięcia maksymalnej nieudanych prób dostępu użytkownika jest zablokowane (domyślne: 5-minutowego blokady po 5 nieudanych prób dostępu).</span><span class="sxs-lookup"><span data-stu-id="3baef-181">If the maximum failed access attempts is reached, the user is locked out (default: 5 minute lockout after 5 failed access attempts).</span></span> <span data-ttu-id="3baef-182">Po pomyślnym uwierzytelnieniu resetuje liczbę prób dostępu nie powiodło się i zresetowanie zegara.</span><span class="sxs-lookup"><span data-stu-id="3baef-182">A successful authentication resets the failed access attempts count and resets the clock.</span></span> <span data-ttu-id="3baef-183">Maksymalną liczbę nieudanych prób dostępu i okres blokady, można ustawić za pomocą [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) i [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span><span class="sxs-lookup"><span data-stu-id="3baef-183">The maximum failed access attempts and lockout time can be set with [MaxFailedAccessAttempts](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.maxfailedaccessattempts) and [DefaultLockoutTimeSpan](/dotnet/api/microsoft.aspnetcore.identity.lockoutoptions.defaultlockouttimespan).</span></span> <span data-ttu-id="3baef-184">Następujące konfiguruje blokady konta po upływie 10 minut po 10 nieudanych prób dostępu:</span><span class="sxs-lookup"><span data-stu-id="3baef-184">The following configures account lockout for 10 minutes after 10 failed access attempts:</span></span>

[!code-csharp[](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)]

<span data-ttu-id="3baef-185">Upewnij się, że [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) ustawia `lockoutOnFailure` do `true`:</span><span class="sxs-lookup"><span data-stu-id="3baef-185">Confirm that [PasswordSignInAsync](/dotnet/api/microsoft.aspnetcore.identity.signinmanager-1.passwordsigninasync) sets `lockoutOnFailure` to `true`:</span></span>

```csharp
var result = await _signInManager.PasswordSignInAsync(
                 Input.Email, Input.Password, Input.RememberMe, lockoutOnFailure: true);
```
