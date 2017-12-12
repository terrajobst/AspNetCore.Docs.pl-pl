---
title: "Uwierzytelnianie dwuskładnikowe z programem SMS"
author: rick-anderson
description: "Pokazuje, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) z platformy ASP.NET Core"
keywords: "Platformy ASP.NET Core, programu SMS, uwierzytelnianie, 2FA, uwierzytelnianie dwuskładnikowe, uwierzytelniania dwuskładnikowego"
ms.author: riande
manager: wpickett
ms.date: 08/15/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/2fa
ms.openlocfilehash: 15620d89c4db2e74dbcec4707bb2ebc6df916e03
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/29/2017
---
# <a name="two-factor-authentication-with-sms"></a><span data-ttu-id="a1d22-104">Uwierzytelnianie dwuskładnikowe z programem SMS</span><span class="sxs-lookup"><span data-stu-id="a1d22-104">Two-factor authentication with SMS</span></span>

<span data-ttu-id="a1d22-105">Przez [Rick Anderson](https://twitter.com/RickAndMSFT) i [Devs Szwajcaria](https://github.com/Swiss-Devs)</span><span class="sxs-lookup"><span data-stu-id="a1d22-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Swiss-Devs](https://github.com/Swiss-Devs)</span></span>

<span data-ttu-id="a1d22-106">Ten samouczek dotyczy platformy ASP.NET Core 1.x tylko.</span><span class="sxs-lookup"><span data-stu-id="a1d22-106">This tutorial applies to ASP.NET Core 1.x only.</span></span> <span data-ttu-id="a1d22-107">Zobacz [generowania kodu QR włączania uwierzytelniania aplikacji w ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) dla platformy ASP.NET Core 2.0 i nowszych.</span><span class="sxs-lookup"><span data-stu-id="a1d22-107">See [Enabling QR Code generation for authenticator apps in ASP.NET Core](xref:security/authentication/identity-enable-qrcodes) for ASP.NET Core 2.0 and later.</span></span>

<span data-ttu-id="a1d22-108">Ten samouczek pokazuje, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS.</span><span class="sxs-lookup"><span data-stu-id="a1d22-108">This tutorial shows how to set up two-factor authentication (2FA) using SMS.</span></span> <span data-ttu-id="a1d22-109">Instrukcje są podane dla [usługi twilio](https://www.twilio.com/) i [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), ale można użyć dowolnego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="a1d22-109">Instructions are given for [twilio](https://www.twilio.com/) and [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/), but you can use any other SMS provider.</span></span> <span data-ttu-id="a1d22-110">Firma Microsoft zaleca, należy wykonać [potwierdzenie konta i hasła odzyskiwania](accconfirm.md) przed rozpoczęciem tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="a1d22-110">We recommend you complete [Account Confirmation and Password Recovery](accconfirm.md) before starting this tutorial.</span></span>

<span data-ttu-id="a1d22-111">Widok [ukończone próbki](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span><span class="sxs-lookup"><span data-stu-id="a1d22-111">View the [completed sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/2fa/sample/Web2FA).</span></span> <span data-ttu-id="a1d22-112">[Jak pobrać](xref:tutorials/index#how-to-download-a-sample).</span><span class="sxs-lookup"><span data-stu-id="a1d22-112">[How to download](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="create-a-new-aspnet-core-project"></a><span data-ttu-id="a1d22-113">Utwórz nowy projekt platformy ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a1d22-113">Create a new ASP.NET Core project</span></span>

<span data-ttu-id="a1d22-114">Utwórz nową aplikację sieci web platformy ASP.NET Core o nazwie `Web2FA` z indywidualnych kont użytkowników.</span><span class="sxs-lookup"><span data-stu-id="a1d22-114">Create a new ASP.NET Core web app named `Web2FA` with individual user accounts.</span></span> <span data-ttu-id="a1d22-115">Postępuj zgodnie z instrukcjami [Wymuszanie protokołu SSL w aplikacji platformy ASP.NET Core](xref:security/enforcing-ssl) do ustawiania i wymagać protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="a1d22-115">Follow the instructions in [Enforcing SSL in an ASP.NET Core app](xref:security/enforcing-ssl) to set up and require SSL.</span></span>

### <a name="create-an-sms-account"></a><span data-ttu-id="a1d22-116">Tworzenie konta usługi programu SMS</span><span class="sxs-lookup"><span data-stu-id="a1d22-116">Create an SMS account</span></span>

<span data-ttu-id="a1d22-117">Tworzenie konta usługi programu SMS, na przykład z [usługi twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span><span class="sxs-lookup"><span data-stu-id="a1d22-117">Create an SMS account, for example, from [twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/core/testcredits/).</span></span> <span data-ttu-id="a1d22-118">Zapisz poświadczenia uwierzytelniania (dla usługi twilio: accountSid i parametr authToken dla ASPSMS: Userkey i hasło).</span><span class="sxs-lookup"><span data-stu-id="a1d22-118">Record the authentication credentials (for twilio: accountSid and authToken, for ASPSMS: Userkey and Password).</span></span>

#### <a name="figuring-out-sms-provider-credentials"></a><span data-ttu-id="a1d22-119">Ustaleniem poświadczenia dostawcy programu SMS</span><span class="sxs-lookup"><span data-stu-id="a1d22-119">Figuring out SMS Provider credentials</span></span>

<span data-ttu-id="a1d22-120">**Usługi Twilio:**</span><span class="sxs-lookup"><span data-stu-id="a1d22-120">**Twilio:**</span></span>  
<span data-ttu-id="a1d22-121">Z konta usługi Twilio, na karcie pulpitu nawigacyjnego, skopiuj **identyfikator SID konta** i **token uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="a1d22-121">From the Dashboard tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>

<span data-ttu-id="a1d22-122">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="a1d22-122">**ASPSMS:**</span></span>  
<span data-ttu-id="a1d22-123">W ustawieniach konta, przejdź do **Userkey** i skopiować go razem z Twojej **hasło**.</span><span class="sxs-lookup"><span data-stu-id="a1d22-123">From your account settings, navigate to **Userkey** and copy it together with your **Password**.</span></span>

<span data-ttu-id="a1d22-124">Przechowujemy później te wartości przy użyciu narzędzia menedżera klucza tajnego w kluczach `SMSAccountIdentification` i `SMSAccountPassword`.</span><span class="sxs-lookup"><span data-stu-id="a1d22-124">We will later store these values in with the secret-manager tool within the keys `SMSAccountIdentification` and `SMSAccountPassword`.</span></span>

#### <a name="specifying-senderid--originator"></a><span data-ttu-id="a1d22-125">Określanie SenderID / inicjator</span><span class="sxs-lookup"><span data-stu-id="a1d22-125">Specifying SenderID / Originator</span></span>

<span data-ttu-id="a1d22-126">**Usługi Twilio:**</span><span class="sxs-lookup"><span data-stu-id="a1d22-126">**Twilio:**</span></span>  
<span data-ttu-id="a1d22-127">Na karcie liczb, skopiować z usługi Twilio **numer telefonu**.</span><span class="sxs-lookup"><span data-stu-id="a1d22-127">From the Numbers tab, copy your Twilio **phone number**.</span></span> 

<span data-ttu-id="a1d22-128">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="a1d22-128">**ASPSMS:**</span></span>  
<span data-ttu-id="a1d22-129">W Menu nadawcy odblokować Odblokuj co najmniej jednego nadawcy, lub Wybierz zleceniodawcę alfanumeryczne (nieobsługiwane przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="a1d22-129">Within the Unlock Originators Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span> 

<span data-ttu-id="a1d22-130">Przechowujemy później tę wartość za pomocą narzędzia menedżera klucza tajnego w kluczu `SMSAccountFrom`.</span><span class="sxs-lookup"><span data-stu-id="a1d22-130">We will later store this value with the secret-manager tool within the key `SMSAccountFrom`.</span></span>


### <a name="provide-credentials-for-the-sms-service"></a><span data-ttu-id="a1d22-131">Podaj poświadczenia dla usługi programu SMS</span><span class="sxs-lookup"><span data-stu-id="a1d22-131">Provide credentials for the SMS service</span></span>

<span data-ttu-id="a1d22-132">Użyjemy [wzorzec opcje](xref:fundamentals/configuration/options) uzyskać dostęp do konta i klucz ustawień użytkownika.</span><span class="sxs-lookup"><span data-stu-id="a1d22-132">We'll use the [Options pattern](xref:fundamentals/configuration/options) to access the user account and key settings.</span></span> 

   * <span data-ttu-id="a1d22-133">Utwórz klasę, aby pobrać klucz zabezpieczeń programu SMS.</span><span class="sxs-lookup"><span data-stu-id="a1d22-133">Create a class to fetch the secure SMS key.</span></span> <span data-ttu-id="a1d22-134">Dla tego przykładu `SMSoptions` klasy jest tworzony w *Services/SMSoptions.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="a1d22-134">For this sample, the `SMSoptions` class is created in the *Services/SMSoptions.cs* file.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Services/SMSoptions.cs)]

<span data-ttu-id="a1d22-135">Ustaw `SMSAccountIdentification`, `SMSAccountPassword` i `SMSAccountFrom` z [narzędzie Menedżer klucz tajny](xref:security/app-secrets).</span><span class="sxs-lookup"><span data-stu-id="a1d22-135">Set the `SMSAccountIdentification`, `SMSAccountPassword` and `SMSAccountFrom` with the [secret-manager tool](xref:security/app-secrets).</span></span> <span data-ttu-id="a1d22-136">Na przykład:</span><span class="sxs-lookup"><span data-stu-id="a1d22-136">For example:</span></span>

```none
C:/Web2FA/src/WebApp1>dotnet user-secrets set SMSAccountIdentification 12345
info: Successfully saved SMSAccountIdentification = 12345 to the secret store.
```
* <span data-ttu-id="a1d22-137">Dodaj pakiet NuGet dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="a1d22-137">Add the NuGet package for the SMS provider.</span></span> <span data-ttu-id="a1d22-138">Z pakietu Menedżera konsoli (PMC) uruchom:</span><span class="sxs-lookup"><span data-stu-id="a1d22-138">From the Package Manager Console (PMC) run:</span></span>

<span data-ttu-id="a1d22-139">**Usługi Twilio:**</span><span class="sxs-lookup"><span data-stu-id="a1d22-139">**Twilio:**</span></span>  
`Install-Package Twilio`

<span data-ttu-id="a1d22-140">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="a1d22-140">**ASPSMS:**</span></span>  
`Install-Package ASPSMS`


* <span data-ttu-id="a1d22-141">Dodaj kod w *Services/MessageServices.cs* plik w celu włączenia programu SMS.</span><span class="sxs-lookup"><span data-stu-id="a1d22-141">Add code in the *Services/MessageServices.cs* file to enable SMS.</span></span> <span data-ttu-id="a1d22-142">Przy użyciu usługi Twilio lub sekcji ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="a1d22-142">Use either the Twilio or the ASPSMS section:</span></span>


<span data-ttu-id="a1d22-143">**Usługi Twilio:**</span><span class="sxs-lookup"><span data-stu-id="a1d22-143">**Twilio:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_twilio.cs)]

<span data-ttu-id="a1d22-144">**ASPSMS:**</span><span class="sxs-lookup"><span data-stu-id="a1d22-144">**ASPSMS:**</span></span>  
[!code-csharp[Main](2fa/sample/Web2FA/Services/MessageServices_ASPSMS.cs)]

### <a name="configure-startup-to-use-smsoptions"></a><span data-ttu-id="a1d22-145">Konfigurowanie uruchamiania do użycia`SMSoptions`</span><span class="sxs-lookup"><span data-stu-id="a1d22-145">Configure startup to use `SMSoptions`</span></span>

<span data-ttu-id="a1d22-146">Dodaj `SMSoptions` do kontenera usługi w `ConfigureServices` metody w *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="a1d22-146">Add `SMSoptions` to the service container in the `ConfigureServices` method in the *Startup.cs*:</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet1&highlight=4)]

### <a name="enable-two-factor-authentication"></a><span data-ttu-id="a1d22-147">Włącz uwierzytelnianie dwuskładnikowe</span><span class="sxs-lookup"><span data-stu-id="a1d22-147">Enable two-factor authentication</span></span>

<span data-ttu-id="a1d22-148">Otwórz *Views/Manage/Index.cshtml* pliku widoku Razor i Usuń komentarz znaków (taki sposób, nie ujęty w komentarzu).</span><span class="sxs-lookup"><span data-stu-id="a1d22-148">Open the *Views/Manage/Index.cshtml* Razor view file and remove the comment characters (so no markup is commnted out).</span></span>

## <a name="log-in-with-two-factor-authentication"></a><span data-ttu-id="a1d22-149">Zaloguj się za pomocą uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="a1d22-149">Log in with two-factor authentication</span></span>

* <span data-ttu-id="a1d22-150">Uruchom aplikację i zarejestrować nowego użytkownika</span><span class="sxs-lookup"><span data-stu-id="a1d22-150">Run the app and register a new user</span></span>

![Otwórz w programie Microsoft Edge widok zarejestrować aplikacji sieci Web](2fa/_static/login2fa1.png)

* <span data-ttu-id="a1d22-152">Wybierz nazwę użytkownika, który uaktywnia `Index` metody akcji w kontrolerze zarządzanie.</span><span class="sxs-lookup"><span data-stu-id="a1d22-152">Tap on your user name, which activates the `Index` action method in Manage controller.</span></span> <span data-ttu-id="a1d22-153">Następnie wybierz numer telefonu **Dodaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="a1d22-153">Then tap the phone number **Add** link.</span></span>

![Zarządzanie widoku](2fa/_static/login2fa2.png)

* <span data-ttu-id="a1d22-155">Numer telefonu, który zostanie wyświetlony kod weryfikacyjny i naciśnij pozycję Dodaj **wysłać kod weryfikacyjny**.</span><span class="sxs-lookup"><span data-stu-id="a1d22-155">Add a phone number that will receive the verification code, and tap **Send verification code**.</span></span>

![Dodaj stronę numer telefonu](2fa/_static/login2fa3.png)

* <span data-ttu-id="a1d22-157">Otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="a1d22-157">You will get a text message with the verification code.</span></span> <span data-ttu-id="a1d22-158">Wprowadź go i wybierz **przesyłania**</span><span class="sxs-lookup"><span data-stu-id="a1d22-158">Enter it and tap **Submit**</span></span>

![Sprawdź numer telefonu strony](2fa/_static/login2fa4.png)

<span data-ttu-id="a1d22-160">Jeśli nie otrzymasz wiadomość SMS, zobacz stronę dziennika usługi twilio.</span><span class="sxs-lookup"><span data-stu-id="a1d22-160">If you don't get a text message, see twilio log page.</span></span>

* <span data-ttu-id="a1d22-161">Zarządzaj ilustracja pokazuje numer telefonu został pomyślnie dodany.</span><span class="sxs-lookup"><span data-stu-id="a1d22-161">The Manage view shows your phone number was added successfully.</span></span>

![Zarządzanie widoku](2fa/_static/login2fa5.png)

* <span data-ttu-id="a1d22-163">Wybierz **włączyć** do włączenia uwierzytelniania dwuskładnikowego.</span><span class="sxs-lookup"><span data-stu-id="a1d22-163">Tap **Enable** to enable two-factor authentication.</span></span>

![Zarządzanie widoku](2fa/_static/login2fa6.png)

### <a name="test-two-factor-authentication"></a><span data-ttu-id="a1d22-165">Uwierzytelnianie dwuskładnikowe testu</span><span class="sxs-lookup"><span data-stu-id="a1d22-165">Test two-factor authentication</span></span>

* <span data-ttu-id="a1d22-166">Wyloguj.</span><span class="sxs-lookup"><span data-stu-id="a1d22-166">Log off.</span></span>

* <span data-ttu-id="a1d22-167">Zaloguj się.</span><span class="sxs-lookup"><span data-stu-id="a1d22-167">Log in.</span></span>

* <span data-ttu-id="a1d22-168">Konto użytkownika ma włączony uwierzytelniania dwuskładnikowego, dlatego należy podać drugiego etapu uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="a1d22-168">The user account has enabled two-factor authentication, so you have to provide the second factor of authentication .</span></span> <span data-ttu-id="a1d22-169">W tym samouczku włączono weryfikację telefoniczną.</span><span class="sxs-lookup"><span data-stu-id="a1d22-169">In this tutorial you have enabled phone verification.</span></span> <span data-ttu-id="a1d22-170">Wbudowane szablony pozwalają również na konfigurowanie poczty e-mail jako drugiego etapu.</span><span class="sxs-lookup"><span data-stu-id="a1d22-170">The built in templates also allow you to set up email as the second factor.</span></span> <span data-ttu-id="a1d22-171">Można skonfigurować dodatkowe czynniki drugi do uwierzytelniania, takich jak kody QR.</span><span class="sxs-lookup"><span data-stu-id="a1d22-171">You can set up additional second factors for authentication such as QR codes.</span></span> <span data-ttu-id="a1d22-172">Wybierz **przesłać**.</span><span class="sxs-lookup"><span data-stu-id="a1d22-172">Tap **Submit**.</span></span>

![Wyślij kod weryfikacyjny widoku](2fa/_static/login2fa7.png)

* <span data-ttu-id="a1d22-174">Wprowadź kod w wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="a1d22-174">Enter the code you get in the SMS message.</span></span>

* <span data-ttu-id="a1d22-175">Kliknięcie **Pamiętaj przeglądarka** pola wyboru będzie zwalnia z konieczności korzystania z 2FA się zalogować, korzystając z tego samego urządzenia i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="a1d22-175">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on when using the same device and browser.</span></span> <span data-ttu-id="a1d22-176">Włączanie 2FA i klikając **Pamiętaj przeglądarka** zapewnia 2FA silnej ochrony przed złośliwymi użytkownikami próby dostęp do tego konta, dopóki nie mają dostępu do urządzenia.</span><span class="sxs-lookup"><span data-stu-id="a1d22-176">Enabling 2FA and clicking on **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your device.</span></span> <span data-ttu-id="a1d22-177">Można to zrobić na dowolnym urządzeniu prywatne, regularnie używane.</span><span class="sxs-lookup"><span data-stu-id="a1d22-177">You can do this on any private device you regularly use.</span></span> <span data-ttu-id="a1d22-178">Przez ustawienie **Pamiętaj przeglądarka**, Pobierz zwiększa bezpieczeństwo 2FA z urządzeń, które nie są regularnie używane i pobrać wygody na ominięcie za pośrednictwem 2FA na własnych urządzeniach.</span><span class="sxs-lookup"><span data-stu-id="a1d22-178">By setting  **Remember this browser**, you get the added security of 2FA from devices you don't regularly use, and you get the convenience on not having to go through 2FA on your own devices.</span></span>

![Sprawdź widok](2fa/_static/login2fa8.png)

## <a name="account-lockout-for-protecting-against-brute-force-attacks"></a><span data-ttu-id="a1d22-180">Blokada konta do ochrony przed atakami siłowymi</span><span class="sxs-lookup"><span data-stu-id="a1d22-180">Account lockout for protecting against brute force attacks</span></span>

<span data-ttu-id="a1d22-181">Firma Microsoft zaleca się, że używasz blokady konta z 2FA.</span><span class="sxs-lookup"><span data-stu-id="a1d22-181">We recommend you use account lockout with 2FA.</span></span> <span data-ttu-id="a1d22-182">Po zalogowaniu się użytkownika (za pomocą konta lokalnego lub konta społecznościowych), każdy nieudane próby 2FA są przechowywane i po osiągnięciu maksymalnej liczby prób (wartość domyślna to 5), użytkownik jest zablokowany przez pięć minut (można ustawić blokady czas z `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="a1d22-182">Once a user logs in (through a local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts (default is 5) is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span> <span data-ttu-id="a1d22-183">Następujące konfiguruje konto zostało zablokowane na 10 minut po 10 nieudanych prób.</span><span class="sxs-lookup"><span data-stu-id="a1d22-183">The following configures Account to be locked out for 10 minutes after 10 failed attempts.</span></span>

[!code-csharp[Main](2fa/sample/Web2FA/Startup.cs?name=snippet2&highlight=13-17)] 
