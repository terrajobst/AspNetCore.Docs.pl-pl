---
uid: identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
title: Uwierzytelnianie dwuskładnikowe przy użyciu programu SMS i wiadomości e-mail z tożsamości ASP.NET | Dokumentacja firmy Microsoft
author: HaoK
description: W tym samouczku opisano, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS i wiadomości e-mail. Ten artykuł dotyczy autorstwa Ricka Andersona ( @RickAndMSFT ), wzór...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/15/2015
ms.topic: article
ms.assetid: 053e23c4-13c9-40fa-87cb-3e9b0823b31e
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: c8f628d177004a8569dde2651469ed591e48591e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="two-factor-authentication-using-sms-and-email-with-aspnet-identity"></a><span data-ttu-id="8b020-104">Uwierzytelnianie dwuskładnikowe przy użyciu programu SMS i wiadomości e-mail z tożsamości platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b020-104">Two-factor authentication using SMS and email with ASP.NET Identity</span></span>
====================
<span data-ttu-id="8b020-105">przez [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span><span class="sxs-lookup"><span data-stu-id="8b020-105">by [Hao Kung](https://github.com/HaoK), [Pranav Rastogi](https://github.com/rustd), [Rick Anderson](https://github.com/Rick-Anderson), [Suhas Joshi](https://github.com/suhasj)</span></span>

> <span data-ttu-id="8b020-106">W tym samouczku opisano, jak skonfigurować uwierzytelnianie dwuskładnikowe (2FA) przy użyciu programu SMS i wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="8b020-106">This tutorial will show you how to set up Two-factor authentication (2FA) using SMS and email.</span></span>
> 
> <span data-ttu-id="8b020-107">Ten artykuł dotyczy autorstwa Ricka Andersona ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung i Suhas Joshi.</span><span class="sxs-lookup"><span data-stu-id="8b020-107">This article was written by Rick Anderson ([@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)), Pranav Rastogi ([@rustd](https://twitter.com/rustd)), Hao Kung, and Suhas Joshi.</span></span> <span data-ttu-id="8b020-108">Przykładowe NuGet zapisał głównie Hao Kung.</span><span class="sxs-lookup"><span data-stu-id="8b020-108">The NuGet sample was written primarily by Hao Kung.</span></span>


<span data-ttu-id="8b020-109">W tym temacie omówiono następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="8b020-109">This topic covers the following:</span></span>

- [<span data-ttu-id="8b020-110">Tworzenie przykładowej tożsamości</span><span class="sxs-lookup"><span data-stu-id="8b020-110">Building the Identity sample</span></span>](#build)
- [<span data-ttu-id="8b020-111">Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="8b020-111">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="8b020-112">Włącz uwierzytelnianie dwuskładnikowe</span><span class="sxs-lookup"><span data-stu-id="8b020-112">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="8b020-113">Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="8b020-113">How to register a Two-factor authentication provider</span></span>](#reg)
- [<span data-ttu-id="8b020-114">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="8b020-114">Combine social and local login accounts</span></span>](#combine)
- [<span data-ttu-id="8b020-115">Blokada konta przed atakami siłowymi</span><span class="sxs-lookup"><span data-stu-id="8b020-115">Account lockout from brute force attacks</span></span>](#lock)

<a id="build"></a>

## <a name="building-the-identity-sample"></a><span data-ttu-id="8b020-116">Tworzenie przykładowej tożsamości</span><span class="sxs-lookup"><span data-stu-id="8b020-116">Building the Identity sample</span></span>

<span data-ttu-id="8b020-117">W tej sekcji użyjesz NuGet można pobrać przykładowy zajmiemy się.</span><span class="sxs-lookup"><span data-stu-id="8b020-117">In this section, you'll use NuGet to download a sample we will work with.</span></span> <span data-ttu-id="8b020-118">Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="8b020-118">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="8b020-119">Zainstaluj program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="8b020-119">Install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="8b020-120">Ostrzeżenie: Należy zainstalować program Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) do ukończenia tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="8b020-120">Warning: You must install Visual Studio [2013 Update 2](https://go.microsoft.com/fwlink/?LinkId=390521) to complete this tutorial.</span></span>


1. <span data-ttu-id="8b020-121">Utwórz nową ***pusty*** projektu sieci Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8b020-121">Create a new ***empty*** ASP.NET Web project.</span></span>
2. <span data-ttu-id="8b020-122">W konsoli Menedżera pakietów, wprowadź następujące polecenia:</span><span class="sxs-lookup"><span data-stu-id="8b020-122">In the Package Manager Console, enter the following the following commands:</span></span>  
  
    `Install-Package SendGrid`  
    `Install-Package -Prerelease Microsoft.AspNet.Identity.Samples`  
  
   <span data-ttu-id="8b020-123">W tym samouczku użyjemy [SendGrid](http://sendgrid.com/) do wysyłania wiadomości e-mail i [usługi Twilio](https://www.twilio.com/) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) dla tekstowe programu sms.</span><span class="sxs-lookup"><span data-stu-id="8b020-123">In this tutorial, we'll use [SendGrid](http://sendgrid.com/) to send email and [Twilio](https://www.twilio.com/) or [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) for sms texting.</span></span> <span data-ttu-id="8b020-124">`Identity.Samples` Pakiet instaluje firma Microsoft będzie działać z kodu.</span><span class="sxs-lookup"><span data-stu-id="8b020-124">The `Identity.Samples` package installs the code we will be working with.</span></span>
3. <span data-ttu-id="8b020-125">Ustaw [projektu do używania protokołu SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="8b020-125">Set the [project to use SSL](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="8b020-126">*Opcjonalne*: postępuj zgodnie z instrukcjami w mojej [samouczek potwierdzenie poczty E-mail](account-confirmation-and-password-recovery-with-aspnet-identity.md) do podpinanie SendGrid, a następnie uruchom aplikację i zarejestrować konto e-mail.</span><span class="sxs-lookup"><span data-stu-id="8b020-126">*Optional*: Follow the instructions in my [Email confirmation tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md) to hook up SendGrid and then run the app and register an email account.</span></span>
5. <span data-ttu-id="8b020-127">* Opcjonalnie: * usunąć kod potwierdzenia pokaz e-mail łączy z próbki ( `ViewBag.Link` kodu w kontrolerze konta.</span><span class="sxs-lookup"><span data-stu-id="8b020-127">*Optional: *Remove the demo email link confirmation code from the sample (The `ViewBag.Link` code in the account controller.</span></span> <span data-ttu-id="8b020-128">Zobacz `DisplayEmail` i `ForgotPasswordConfirmation` metody akcji i widokami razor).</span><span class="sxs-lookup"><span data-stu-id="8b020-128">See the `DisplayEmail` and `ForgotPasswordConfirmation` action methods and razor views ).</span></span>
6. <span data-ttu-id="8b020-129"><em>Opcjonalnie: * Usuń `ViewBag.Status` kodu z kontrolerami zarządzania i konta i *Views\Account\VerifyCode.cshtml</em> i <em>Views\Manage\VerifyPhoneNumber.cshtml</em> widokami razor.</span><span class="sxs-lookup"><span data-stu-id="8b020-129"><em>Optional: *Remove the `ViewBag.Status` code from the Manage and Account controllers and from the *Views\Account\VerifyCode.cshtml</em> and <em>Views\Manage\VerifyPhoneNumber.cshtml</em> razor views.</span></span> <span data-ttu-id="8b020-130">Alternatywnie można zachować `ViewBag.Status` wyświetlania, aby sprawdzić, jak ta aplikacja działa lokalnie, bez konieczności Podłączanie i wysyłanie wiadomości e-mail i wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="8b020-130">Alternatively, you can keep the `ViewBag.Status` display to test how this app works locally without having to hook up and send email and SMS messages.</span></span>

> [!NOTE]
> <span data-ttu-id="8b020-131">Ostrzeżenie: Jeśli zmienisz jakiekolwiek ustawienia zabezpieczeń w tym przykładzie produkcji aplikacje będą wymagały zastosowane inspekcji zabezpieczeń, która jawnie uwidacznia zmiany wprowadzone do.</span><span class="sxs-lookup"><span data-stu-id="8b020-131">Warning: If you change any of the security settings in this sample, productions apps will need to undergo a security audit that explicitly calls out the changes made.</span></span>


<a id="SMS"></a>

## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="8b020-132">Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="8b020-132">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="8b020-133">Ten samouczek zawiera instrukcje dotyczące korzystania z usługi Twilio lub ASPSMS, ale można użyć dowolnego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="8b020-133">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="8b020-134">**Tworzenie konta użytkownika z dostawcą programu SMS**</span><span class="sxs-lookup"><span data-stu-id="8b020-134">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="8b020-135">Utwórz [usługi Twilio](https://www.twilio.com/try-twilio) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) konta.</span><span class="sxs-lookup"><span data-stu-id="8b020-135">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="8b020-136">**Instalowanie dodatkowych pakietów lub Dodawanie odwołań do usługi**</span><span class="sxs-lookup"><span data-stu-id="8b020-136">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="8b020-137">Usługi Twilio:</span><span class="sxs-lookup"><span data-stu-id="8b020-137">Twilio:</span></span>  
   <span data-ttu-id="8b020-138">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="8b020-138">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="8b020-139">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="8b020-139">ASPSMS:</span></span>  
   <span data-ttu-id="8b020-140">Następujące odwołanie do usługi musi zostać dodany:</span><span class="sxs-lookup"><span data-stu-id="8b020-140">The following service reference needs to be added:</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image1.png)  
  
   <span data-ttu-id="8b020-141">Adres:</span><span class="sxs-lookup"><span data-stu-id="8b020-141">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="8b020-142">Przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="8b020-142">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="8b020-143">**Ustaleniem, poświadczenia użytkownika dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="8b020-143">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="8b020-144">Usługi Twilio:</span><span class="sxs-lookup"><span data-stu-id="8b020-144">Twilio:</span></span>  
   <span data-ttu-id="8b020-145">Z **pulpitu nawigacyjnego** kartę konta usługi Twilio kopiowania **identyfikator SID konta** i **token uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="8b020-145">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="8b020-146">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="8b020-146">ASPSMS:</span></span>  
   <span data-ttu-id="8b020-147">W ustawieniach konta, przejdź do **Userkey** i skopiować go razem z własnym zdefiniowanych **hasło**.</span><span class="sxs-lookup"><span data-stu-id="8b020-147">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="8b020-148">Te wartości będą przechowywane później w zmiennych `SMSAccountIdentification` i `SMSAccountPassword` .</span><span class="sxs-lookup"><span data-stu-id="8b020-148">We will later store these values in the variables `SMSAccountIdentification` and `SMSAccountPassword` .</span></span>
4. <span data-ttu-id="8b020-149">**Określanie SenderID / inicjator**</span><span class="sxs-lookup"><span data-stu-id="8b020-149">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="8b020-150">Usługi Twilio:</span><span class="sxs-lookup"><span data-stu-id="8b020-150">Twilio:</span></span>  
   <span data-ttu-id="8b020-151">Z **numery** karcie, skopiuj numer telefonu usługi Twilio.</span><span class="sxs-lookup"><span data-stu-id="8b020-151">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="8b020-152">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="8b020-152">ASPSMS:</span></span>  
   <span data-ttu-id="8b020-153">W ramach **odblokować nadawcy** Menu, odblokuj co najmniej jednego nadawcy, lub Wybierz zleceniodawcę alfanumeryczne (nieobsługiwane przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="8b020-153">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="8b020-154">Ta wartość zostanie później przechowywane w zmiennej `SMSAccountFrom` .</span><span class="sxs-lookup"><span data-stu-id="8b020-154">We will later store this value in the variable `SMSAccountFrom` .</span></span>
5. <span data-ttu-id="8b020-155">**Transferowanie poświadczenia dostawcy programu SMS w aplikacji**</span><span class="sxs-lookup"><span data-stu-id="8b020-155">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="8b020-156">Udostępnij poświadczenia i numer telefonu nadawcy aplikacji:</span><span class="sxs-lookup"><span data-stu-id="8b020-156">Make the credentials and sender phone number available to the app:</span></span>

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample1.cs)]

    > [!WARNING]
    > <span data-ttu-id="8b020-157">Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="8b020-157">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="8b020-158">Konta i poświadczenia zostaną dodane do powyżej, aby zachować prosty przykład kodu.</span><span class="sxs-lookup"><span data-stu-id="8b020-158">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="8b020-159">Zobacz Jan Atten [platformy ASP.NET MVC: Zachowaj prywatnego poza ustawień kontroli źródła](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b020-159">See Jon Atten's [ASP.NET MVC: Keep Private Settings Out of Source Control](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).</span></span>
6. <span data-ttu-id="8b020-160">**Implementacja transferu danych do dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="8b020-160">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="8b020-161">Skonfiguruj `SmsService` klasy w *aplikacji\_Start\IdentityConfig.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="8b020-161">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="8b020-162">W zależności od dostawcy programu SMS używanego aktywować **usługi Twilio** lub **ASPSMS** sekcji:</span><span class="sxs-lookup"><span data-stu-id="8b020-162">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample2.cs)]
7. <span data-ttu-id="8b020-163">Uruchom aplikację i zaloguj się przy użyciu konta, które wcześniej zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="8b020-163">Run the app and log in with the account you previously registered.</span></span>
8. <span data-ttu-id="8b020-164">Kliknij nazwę użytkownika, który uaktywnia `Index` metody akcji w `Manage` kontrolera.</span><span class="sxs-lookup"><span data-stu-id="8b020-164">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image2.png)
9. <span data-ttu-id="8b020-165">Kliknij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="8b020-165">Click Add.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image3.png)
10. <span data-ttu-id="8b020-166">Za chwilę otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="8b020-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="8b020-167">Wprowadź go i naciśnij klawisz **przesyłania**.</span><span class="sxs-lookup"><span data-stu-id="8b020-167">Enter it and press **Submit**.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image4.png)
11. <span data-ttu-id="8b020-168">Zarządzaj ilustracja pokazuje numer telefonu został dodany.</span><span class="sxs-lookup"><span data-stu-id="8b020-168">The Manage view shows your phone number was added.</span></span>  
  
    ![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image5.png)

### <a name="examine-the-code"></a><span data-ttu-id="8b020-169">Sprawdź kod</span><span class="sxs-lookup"><span data-stu-id="8b020-169">Examine the code</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample3.cs?highlight=2)]

<span data-ttu-id="8b020-170">`Index` Metody akcji w `Manage` kontroler Ustawia komunikat o stanie oparte na sieci poprzedniej akcji i zawiera łącza, aby zmienić hasło lokalne lub dodać konta lokalnego.</span><span class="sxs-lookup"><span data-stu-id="8b020-170">The `Index` action method in `Manage` controller sets the status message based on your previous action and provides links to change your local password or add a local account.</span></span> <span data-ttu-id="8b020-171">`Index` Metoda wyświetla również stan lub 2FA Twojego telefonu numer, logowań zewnętrznych, 2FA włączone i zapamiętać 2FA metoda ta przeglądarka (co omówiono później).</span><span class="sxs-lookup"><span data-stu-id="8b020-171">The `Index` method also displays the state or your 2FA phone number, external logins, 2FA enabled, and remember 2FA method for this browser(explained later).</span></span> <span data-ttu-id="8b020-172">Klikając nazwę użytkownika (poczta e-mail) na pasku tytułu nie przeszło wiadomości.</span><span class="sxs-lookup"><span data-stu-id="8b020-172">Clicking on your user ID (email) in the title bar doesn't pass a message.</span></span> <span data-ttu-id="8b020-173">Kliknięcie **numer telefonu: Usuń** link przekazuje `Message=RemovePhoneSuccess` jako ciąg zapytania.</span><span class="sxs-lookup"><span data-stu-id="8b020-173">Clicking on the **Phone Number : remove** link passes `Message=RemovePhoneSuccess` as a query string.</span></span>  
  
`https://localhost:44300/Manage?Message=RemovePhoneSuccess`

<span data-ttu-id="8b020-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span><span class="sxs-lookup"><span data-stu-id="8b020-174">[![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image6.png)]</span></span>

<span data-ttu-id="8b020-175">`AddPhoneNumber` Metody akcji Wyświetla okno dialogowe, aby wprowadzić numer telefonu, który może odbierać wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="8b020-175">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample4.cs)]

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image7.png)

<span data-ttu-id="8b020-176">Kliknięcie **wysłać kod weryfikacyjny** przycisk wpisów numer telefonu HTTP POST `AddPhoneNumber` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="8b020-176">Clicking on the **Send verification code** button posts the phone number to the HTTP POST `AddPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample5.cs?highlight=12)]

<span data-ttu-id="8b020-177">`GenerateChangePhoneNumberTokenAsync` Metoda generuje tokenu zabezpieczającego, które zostaną ustawione w wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="8b020-177">The `GenerateChangePhoneNumberTokenAsync` method generates the security token which will be set in the SMS message.</span></span> <span data-ttu-id="8b020-178">Jeśli usługa programu SMS został skonfigurowany, token jest wysyłany jako ciąg &quot;Twój kod zabezpieczający &lt;tokenu&gt;&quot;.</span><span class="sxs-lookup"><span data-stu-id="8b020-178">If the SMS service has been configured, the token is sent as the string &quot;Your security code is &lt;token&gt;&quot;.</span></span> <span data-ttu-id="8b020-179">`SmsService.SendAsync` Metoda jest wywoływana asynchronicznie, a następnie aplikacja jest przekierowywany do `VerifyPhoneNumber` metody akcji (która wyświetla to okno dialogowe), gdzie można wprowadzić kod weryfikacyjny.</span><span class="sxs-lookup"><span data-stu-id="8b020-179">The `SmsService.SendAsync` method to is called asynchronously, then the app is redirected to the `VerifyPhoneNumber` action method (which displays the following dialog), where you can enter the verification code.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image8.png)

<span data-ttu-id="8b020-180">Po wprowadź kod i kliknij przycisk Prześlij kod jest zamieszczana HTTP POST `VerifyPhoneNumber` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="8b020-180">Once you enter the code and click submit, the code is posted to the HTTP POST `VerifyPhoneNumber` action method.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample6.cs)]

<span data-ttu-id="8b020-181">`ChangePhoneNumberAsync` Metoda sprawdza kod oczekujących na opublikowanie zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="8b020-181">The `ChangePhoneNumberAsync` method checks the posted security code.</span></span> <span data-ttu-id="8b020-182">Jeśli kod jest poprawny, numer telefonu jest dodawany do `PhoneNumber` pole `AspNetUsers` tabeli.</span><span class="sxs-lookup"><span data-stu-id="8b020-182">If the code is correct, the phone number is added to the `PhoneNumber` field of the `AspNetUsers` table.</span></span> <span data-ttu-id="8b020-183">Jeśli tego wywołania zakończy się pomyślnie, `SignInAsync` metoda jest wywoływana:</span><span class="sxs-lookup"><span data-stu-id="8b020-183">If that call is successful, the `SignInAsync` method is called:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample7.cs)]

<span data-ttu-id="8b020-184">`isPersistent` Parametr określa, czy sesja uwierzytelniania jest trwała dla wielu żądań.</span><span class="sxs-lookup"><span data-stu-id="8b020-184">The `isPersistent` parameter sets whether the authentication session is persisted across multiple requests.</span></span>

<span data-ttu-id="8b020-185">Jeśli zmienisz profil zabezpieczeń nową sygnaturę bezpieczeństwa są generowane i przechowywane w `SecurityStamp` pole *AspNetUsers* tabeli.</span><span class="sxs-lookup"><span data-stu-id="8b020-185">When you change your security profile, a new security stamp is generated and stored in the `SecurityStamp` field of the *AspNetUsers* table.</span></span> <span data-ttu-id="8b020-186">Uwaga: `SecurityStamp` pola różni się od pliku cookie zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="8b020-186">Note, the `SecurityStamp` field is different from the security cookie.</span></span> <span data-ttu-id="8b020-187">Plik cookie zabezpieczeń nie są przechowywane w `AspNetUsers` tabeli (lub dowolnego miejsca w bazie danych tożsamości).</span><span class="sxs-lookup"><span data-stu-id="8b020-187">The security cookie is not stored in the `AspNetUsers` table (or anywhere else in the Identity DB).</span></span> <span data-ttu-id="8b020-188">Token pliku cookie zabezpieczeń jest podpisany przy użyciu [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) i zostanie utworzony z `UserId, SecurityStamp` i informacje dotyczące czasu wygaśnięcia.</span><span class="sxs-lookup"><span data-stu-id="8b020-188">The security cookie token is self-signed using [DPAPI](https://msdn.microsoft.com/library/system.security.cryptography.protecteddata.aspx) and is created with the `UserId, SecurityStamp` and expiration time information.</span></span>

<span data-ttu-id="8b020-189">Oprogramowanie pośredniczące plików cookie sprawdza, czy plik cookie na każde żądanie.</span><span class="sxs-lookup"><span data-stu-id="8b020-189">The cookie middleware checks the cookie on each request.</span></span> <span data-ttu-id="8b020-190">`SecurityStampValidator` Metody w `Startup` klasy trafienia bazę danych i okresowo sprawdza sygnaturę bezpieczeństwa z określonej `validateInterval`.</span><span class="sxs-lookup"><span data-stu-id="8b020-190">The `SecurityStampValidator` method in the `Startup` class hits the DB and checks security stamp periodically, as specified with the `validateInterval`.</span></span> <span data-ttu-id="8b020-191">Tylko dzieje co 30 minut (w naszym przykładzie), chyba że zostanie zmienione profil zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="8b020-191">This only happens every 30 minutes (in our sample) unless you change your security profile.</span></span> <span data-ttu-id="8b020-192">Aby zminimalizować rund do bazy danych wybrano 30 minut.</span><span class="sxs-lookup"><span data-stu-id="8b020-192">The 30 minute interval was chosen to minimize trips to the database.</span></span>

<span data-ttu-id="8b020-193">`SignInAsync` Metoda musi być wywoływany w przypadku wszelkich zmian do profilu zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="8b020-193">The `SignInAsync` method needs to be called when any change is made to the security profile.</span></span> <span data-ttu-id="8b020-194">Po zmianie profil zabezpieczeń bazy danych jest aktualizacje `SecurityStamp` pól i bez wywoływania elementu `SignInAsync` metody, czy użytkownik pozostanie zalogowany w *tylko* czasu kolejnego potoku OWIN trafienia bazy danych ( `validateInterval`).</span><span class="sxs-lookup"><span data-stu-id="8b020-194">When the security profile changes, the database is updates the `SecurityStamp` field, and without calling the `SignInAsync` method you would stay logged in *only* until the next time the OWIN pipeline hits the database (the `validateInterval`).</span></span> <span data-ttu-id="8b020-195">Można to sprawdzić, zmieniając `SignInAsync` metodę, aby zwrócić natychmiast i ustawienie pliku cookie `validateInterval` właściwości od 30 minut na 5 sekund:</span><span class="sxs-lookup"><span data-stu-id="8b020-195">You can test this by changing the `SignInAsync` method to return immediately, and setting the cookie `validateInterval` property from 30 minutes to 5 seconds:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample8.cs?highlight=3)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample9.cs?highlight=20-21)]

<span data-ttu-id="8b020-196">Z powyższych zmiany kodu, można zmienić Twój profil zabezpieczeń (na przykład, zmieniając stan **dwie włączone współczynnik**) i będziesz się logować w ciągu 5 sekund po `SecurityStampValidator.OnValidateIdentity` metoda kończy się niepowodzeniem.</span><span class="sxs-lookup"><span data-stu-id="8b020-196">With the above code changes, you can change your security profile (for example, by changing the state of **Two Factor Enabled**) and you will be logged out in 5 seconds when the `SecurityStampValidator.OnValidateIdentity` method fails.</span></span> <span data-ttu-id="8b020-197">Usuń wiersz zwracany w `SignInAsync` metody, ustawić inny profil zabezpieczeń zmiany i nie będziesz się logować. `SignInAsync` Metoda generuje nowy plik cookie zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="8b020-197">Remove the return line in the `SignInAsync` method, make another security profile change and you will not be logged out. The `SignInAsync` method generates a new security cookie.</span></span>

<a id="enable2"></a>

## <a name="enable-two-factor-authentication"></a><span data-ttu-id="8b020-198">Włącz uwierzytelnianie dwuskładnikowe</span><span class="sxs-lookup"><span data-stu-id="8b020-198">Enable two-factor authentication</span></span>

<span data-ttu-id="8b020-199">W przykładowej aplikacji należy użyć interfejsu użytkownika, aby włączyć uwierzytelnianie dwuskładnikowe (2FA).</span><span class="sxs-lookup"><span data-stu-id="8b020-199">In the sample app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="8b020-200">Aby włączyć 2FA, kliknij nazwę użytkownika (alias e-mail) na pasku nawigacyjnym.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="8b020-200">To enable 2FA, click on your user ID (email alias) in the navigation bar.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image9.png)</span></span>  
<span data-ttu-id="8b020-201">Kliknij pozycję Włącz 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span><span class="sxs-lookup"><span data-stu-id="8b020-201">Click on enable 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image10.png)</span></span> <span data-ttu-id="8b020-202">Wyloguj się, a następnie ponowne zalogowanie.</span><span class="sxs-lookup"><span data-stu-id="8b020-202">Log out, then log back in.</span></span> <span data-ttu-id="8b020-203">Jeśli włączono poczty e-mail (Zobacz Moje [poprzedniego samouczek](account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać SMS lub wiadomości e-mail w przypadku 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="8b020-203">If you've enabled email (see my [previous tutorial](account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image11.png)</span></span> <span data-ttu-id="8b020-204">Zostanie wyświetlona strona Sprawdź kod, gdzie można wprowadzić kod (z programu SMS lub e-mail).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="8b020-204">The Verify Code page is displayed where you can enter the code (from SMS or email).![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image12.png)</span></span> <span data-ttu-id="8b020-205">Kliknięcie **Pamiętaj przeglądarka** pola wyboru będzie zwalnia z konieczności korzystania z 2FA logowania się do tego komputera i przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="8b020-205">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log on with that computer and browser.</span></span> <span data-ttu-id="8b020-206">Włączanie 2FA i klikając **Pamiętaj przeglądarka** zapewnia 2FA silnej ochrony przed złośliwymi użytkownikami próby dostęp do tego konta, dopóki nie mają dostępu do tego komputera.</span><span class="sxs-lookup"><span data-stu-id="8b020-206">Enabling 2FA and clicking on the **Remember this browser** will provide you with strong 2FA protection from malicious users trying to access your account, as long as they don't have access to your computer.</span></span> <span data-ttu-id="8b020-207">Można to zrobić na dowolnym komputerze prywatne, regularnie używane.</span><span class="sxs-lookup"><span data-stu-id="8b020-207">You can do this on any private machine you regularly use.</span></span> <span data-ttu-id="8b020-208">Przez ustawienie **Pamiętaj przeglądarka**, Pobierz zwiększa bezpieczeństwo 2FA z komputerów, które nie są regularnie używane i pobrać wygody na ominięcie za pośrednictwem 2FA własnych komputerów.</span><span class="sxs-lookup"><span data-stu-id="8b020-208">By setting **Remember this browser**, you get the added security of 2FA from computers you don't regularly use, and you get the convenience on not having to go through 2FA on your own computers.</span></span> 

<a id="reg"></a>
## <a name="how-to-register-a-two-factor-authentication-provider"></a><span data-ttu-id="8b020-209">Jak zarejestrować dostawcę uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="8b020-209">How to register a Two-factor authentication provider</span></span>

<span data-ttu-id="8b020-210">Podczas tworzenia nowego projektu MVC *IdentityConfig.cs* plik zawiera następujący kod, aby zarejestrować dostawcę uwierzytelniania dwuetapowego:</span><span class="sxs-lookup"><span data-stu-id="8b020-210">When you create a new MVC project, the *IdentityConfig.cs* file contains the following code to register a Two-factor authentication provider:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample10.cs?highlight=22-35)]

## <a name="add-a-phone-number-for-2fa"></a><span data-ttu-id="8b020-211">Dodaj numer telefonu dla 2FA</span><span class="sxs-lookup"><span data-stu-id="8b020-211">Add a phone number for 2FA</span></span>

<span data-ttu-id="8b020-212">`AddPhoneNumber` Metody akcji w `Manage` kontrolera generuje token zabezpieczający i wysyła go z numerem telefonu numeru, jaki podano.</span><span class="sxs-lookup"><span data-stu-id="8b020-212">The `AddPhoneNumber` action method in the `Manage` controller generates a security token and sends it to the phone number you have provided.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample11.cs)]

<span data-ttu-id="8b020-213">Po wysłaniu token, przekierowuje do `VerifyPhoneNumber` metody akcji, w którym można wprowadzić kod, aby zarejestrować SMS 2FA.</span><span class="sxs-lookup"><span data-stu-id="8b020-213">After sending the token, it redirects to the `VerifyPhoneNumber` action method, where you can enter the code to register SMS for 2FA.</span></span> <span data-ttu-id="8b020-214">2FA programu SMS nie jest używany, dopóki nie upewnisz się numer telefonu.</span><span class="sxs-lookup"><span data-stu-id="8b020-214">SMS 2FA is not used until you have verified the phone number.</span></span>

## <a name="enabling-2fa"></a><span data-ttu-id="8b020-215">Włączanie 2FA</span><span class="sxs-lookup"><span data-stu-id="8b020-215">Enabling 2FA</span></span>

<span data-ttu-id="8b020-216">`EnableTFA` 2FA umożliwia metody akcji:</span><span class="sxs-lookup"><span data-stu-id="8b020-216">The `EnableTFA` action method enables 2FA:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample12.cs)]

<span data-ttu-id="8b020-217">Uwaga `SignInAsync` musi zostać wywołana, ponieważ 2FA Włącz zmianę profil zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="8b020-217">Note the `SignInAsync` must be called because enable 2FA is a change to the security profile.</span></span> <span data-ttu-id="8b020-218">Po włączeniu 2FA użytkownik będzie musiał używać do logowania, 2FA przy użyciu podejścia 2FA zarejestrowanych (SMS i poczty e-mail w próbce).</span><span class="sxs-lookup"><span data-stu-id="8b020-218">When 2FA is enabled, the user will need to use 2FA to log in, using the 2FA approaches they have registered (SMS and email in the sample).</span></span>

<span data-ttu-id="8b020-219">Można dodać więcej dostawców 2FA, takie jak generatory kodu QR lub napisania własnej (zobacz [za pomocą tokenu Google Authenticator z ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span><span class="sxs-lookup"><span data-stu-id="8b020-219">You can add more 2FA providers such as QR code generators or you can write you own (See [Using Google Authenticator with ASP.NET Identity](http://www.beabigrockstar.com/blog/using-google-authenticator-asp-net-identity/)).</span></span>

> [!NOTE]
> <span data-ttu-id="8b020-220">Kody 2FA są generowane przy użyciu [oparte na czasie algorytm hasła jednorazowego](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) i kody są ważne przez sześć minut.</span><span class="sxs-lookup"><span data-stu-id="8b020-220">The 2FA codes are generated using [Time-based One-time Password Algorithm](http://en.wikipedia.org/wiki/Time-based_One-time_Password_Algorithm) and codes are valid for six minutes.</span></span> <span data-ttu-id="8b020-221">Jeśli skorzystasz z więcej niż sześciu minut wprowadź kod, zostanie wyświetlony komunikat o błędzie nieprawidłowy kod.</span><span class="sxs-lookup"><span data-stu-id="8b020-221">If you take more than six minutes to enter the code, you'll get an Invalid code error message.</span></span>


<a id="combine"></a>

## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="8b020-222">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="8b020-222">Combine social and local login accounts</span></span>

<span data-ttu-id="8b020-223">Konta lokalne i społecznościowych można łączyć, klikając link do wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="8b020-223">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="8b020-224">W następującej kolejności &quot; RickAndMSFT@gmail.com &quot; najpierw zostanie utworzona jako logowania lokalnego, ale można utworzyć konta społecznościowych logowania w pierwszym, a następnie dodaj lokalny identyfikator logowania.</span><span class="sxs-lookup"><span data-stu-id="8b020-224">In the following sequence &quot;RickAndMSFT@gmail.com&quot; is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image13.png)

<span data-ttu-id="8b020-225">Polecenie **Zarządzaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="8b020-225">Click on the **Manage** link.</span></span> <span data-ttu-id="8b020-226">Uwaga zewnętrznych 0 (społecznościowych logowania) skojarzone z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="8b020-226">Note the 0 external (social logins) associated with this account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image14.png)

<span data-ttu-id="8b020-227">Kliknij łącze do innego dziennika w usłudze i akceptowania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="8b020-227">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="8b020-228">Dwa konta zostały połączone, będzie można logować się na każdym koncie.</span><span class="sxs-lookup"><span data-stu-id="8b020-228">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="8b020-229">Możesz użytkownikom dodawanie kont lokalnych, w przypadku ich społecznościowych dziennika w usłudze uwierzytelniania jest wyłączony lub najprawdopodobniej po utracie dostępu do swojego konta społecznościowych.</span><span class="sxs-lookup"><span data-stu-id="8b020-229">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="8b020-230">Na poniższej ilustracji, Tomasz jest społecznościowych logowania (które można wyświetlić z **logowań zewnętrznych: 1** wyświetlany na stronie).</span><span class="sxs-lookup"><span data-stu-id="8b020-230">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image15.png)

<span data-ttu-id="8b020-231">Kliknięcie **Wybierz hasło** umożliwia dodanie w lokalnym dzienniku na skojarzone z kontem.</span><span class="sxs-lookup"><span data-stu-id="8b020-231">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](two-factor-authentication-using-sms-and-email-with-aspnet-identity/_static/image16.png)

<a id="lock"></a>

## <a name="account-lockout-from-brute-force-attacks"></a><span data-ttu-id="8b020-232">Blokada konta przed atakami siłowymi</span><span class="sxs-lookup"><span data-stu-id="8b020-232">Account lockout from brute force attacks</span></span>

<span data-ttu-id="8b020-233">Konta w aplikacji przed atakami słownikowymi chronić przez włączenie blokady użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8b020-233">You can protect the accounts on your app from dictionary attacks by enabling user lockout.</span></span> <span data-ttu-id="8b020-234">Poniższy kod w `ApplicationUserManager Create` metody konfiguruje blokady:</span><span class="sxs-lookup"><span data-stu-id="8b020-234">The following code in the `ApplicationUserManager Create` method configures lock out:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample13.cs)]

<span data-ttu-id="8b020-235">Powyższy kod umożliwia blokady dla tylko uwierzytelniania dwuskładnikowego.</span><span class="sxs-lookup"><span data-stu-id="8b020-235">The code above enables lockout for two factor authentication only.</span></span> <span data-ttu-id="8b020-236">Podczas blokady dla nazwy logowania można włączyć, zmieniając `shouldLockout` na wartość true w `Login` metody kontrolera konta, zalecamy nie włączyć blokady dla nazwy logowania ponieważ sprawia, że konto podatne na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) logowania ataki.</span><span class="sxs-lookup"><span data-stu-id="8b020-236">While you can enable lock out for logins by changing `shouldLockout` to true in the `Login` method of the account controller, we recommend you not enable lock out for logins because it makes the account susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) login attacks.</span></span> <span data-ttu-id="8b020-237">W przykładowym kodzie, blokada jest wyłączona dla konta administratora utworzonego w `ApplicationDbInitializer Seed` metody:</span><span class="sxs-lookup"><span data-stu-id="8b020-237">In the sample code, lockout is disabled for the admin account created in the `ApplicationDbInitializer Seed` method:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample14.cs?highlight=19)]

## <a name="requiring-a-user-to-have-a-validated-email-account"></a><span data-ttu-id="8b020-238">Wymaganie użytkownika konta e-mail zweryfikowanych</span><span class="sxs-lookup"><span data-stu-id="8b020-238">Requiring a user to have a validated email account</span></span>

<span data-ttu-id="8b020-239">Poniższy kod wymaga od użytkownika posiadania konta e-mail zweryfikowanych przed można zalogować:</span><span class="sxs-lookup"><span data-stu-id="8b020-239">The following code requires a user to have a validated email account before they can log in:</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample15.cs?highlight=8-17)]

## <a name="how-signinmanager-checks-for-2fa-requirement"></a><span data-ttu-id="8b020-240">Jak SignInManager wyszukuje 2FA wymaganie</span><span class="sxs-lookup"><span data-stu-id="8b020-240">How SignInManager checks for 2FA requirement</span></span>

<span data-ttu-id="8b020-241">Logowanie lokalne i społecznościowych logowania Sprawdź, czy włączono 2FA.</span><span class="sxs-lookup"><span data-stu-id="8b020-241">Both the local log in and social log in check to see if 2FA is enabled.</span></span> <span data-ttu-id="8b020-242">Po włączeniu 2FA `SignInManager` logowania, metoda zwraca `SignInStatus.RequiresVerification`, a użytkownik zostanie przekierowany do `SendCode` metody akcji, w którym będzie musiał wprowadzić kod do wykonania w dzienniku w sekwencji.</span><span class="sxs-lookup"><span data-stu-id="8b020-242">If 2FA is enabled, the `SignInManager` logon method returns `SignInStatus.RequiresVerification`, and the user will be redirected to the `SendCode` action method, where they will have to enter the code to complete the log in sequence.</span></span> <span data-ttu-id="8b020-243">Jeśli użytkownik ma RememberMe jest ustawiony na użytkowników lokalnych plików cookie, `SignInManager` zwróci `SignInStatus.Success` i nie będzie musiał przejść przez 2FA.</span><span class="sxs-lookup"><span data-stu-id="8b020-243">If the user has RememberMe is set on the users local cookie, the `SignInManager` will return `SignInStatus.Success` and they will not have to go through 2FA.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample16.cs?highlight=20-22)]

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample17.cs?highlight=10-11,17-18)]

<span data-ttu-id="8b020-244">Poniższy kod przedstawia `SendCode` metody akcji.</span><span class="sxs-lookup"><span data-stu-id="8b020-244">The following code shows the `SendCode` action method.</span></span> <span data-ttu-id="8b020-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) jest tworzony za pomocą metod 2FA włączone dla danego użytkownika.</span><span class="sxs-lookup"><span data-stu-id="8b020-245">A [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is created with all the 2FA methods enabled for the user.</span></span> <span data-ttu-id="8b020-246">[SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) jest przekazywana do [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) pomocnika, który umożliwia użytkownikowi wybranie metody 2FA (zazwyczaj poczty e-mail i SMS).</span><span class="sxs-lookup"><span data-stu-id="8b020-246">The [SelectListItem](https://msdn.microsoft.com/library/system.web.mvc.selectlistitem.aspx) is passed to the [DropDownListFor](https://msdn.microsoft.com/library/system.web.ui.webcontrols.dropdownlist.aspx) helper, which allows the user to select the 2FA approach (typically email and SMS).</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample18.cs)]

<span data-ttu-id="8b020-247">Gdy użytkownik zapisuje podejście 2FA `HTTP POST SendCode` po wywołaniu metody akcji `SignInManager` wysyła kod 2FA, a użytkownik zostanie przekierowany do `VerifyCode` metody akcji, w którym mogą oni wprowadzić kod w celu zakończenia logowania.</span><span class="sxs-lookup"><span data-stu-id="8b020-247">Once the user posts the 2FA approach, the `HTTP POST SendCode` action method is called, the `SignInManager` sends the 2FA code, and the user is redirected to the `VerifyCode` action method where they can enter the code to complete the log in.</span></span>

[!code-csharp[Main](two-factor-authentication-using-sms-and-email-with-aspnet-identity/samples/sample19.cs?highlight=3,13-14,18)]

## <a name="2fa-lockout"></a><span data-ttu-id="8b020-248">2FA blokady</span><span class="sxs-lookup"><span data-stu-id="8b020-248">2FA Lockout</span></span>

<span data-ttu-id="8b020-249">Mimo że można ustawić blokady konta na niepowodzenia próba hasło logowania, takiego podejścia sprawia, że logowanie podatne na [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) blokady.</span><span class="sxs-lookup"><span data-stu-id="8b020-249">Although you can set account lockout on login password attempt failures, that approach makes your login susceptible to [DOS](http://en.wikipedia.org/wiki/Denial-of-service_attack) lockouts.</span></span> <span data-ttu-id="8b020-250">Firma Microsoft zaleca się, że używasz blokady konta tylko z 2FA.</span><span class="sxs-lookup"><span data-stu-id="8b020-250">We recommend you use account lockout only with 2FA.</span></span> <span data-ttu-id="8b020-251">Gdy `ApplicationUserManager` jest utworzony, przykładowy kod ustawia blokady 2FA i `MaxFailedAccessAttemptsBeforeLockout` pięć.</span><span class="sxs-lookup"><span data-stu-id="8b020-251">When the `ApplicationUserManager` is created, the sample code sets 2FA lockout and `MaxFailedAccessAttemptsBeforeLockout` to five.</span></span> <span data-ttu-id="8b020-252">Po zalogowaniu się użytkownika (za pomocą konta lokalnego lub konta społecznościowych), każdy nieudane próby 2FA są przechowywane i po osiągnięciu maksymalnej liczby prób użytkownik jest zablokowany przez pięć minut (można ustawić blokady czas z `DefaultAccountLockoutTimeSpan`).</span><span class="sxs-lookup"><span data-stu-id="8b020-252">Once a user logs in (through local account or social account), each failed attempt at 2FA is stored, and if the maximum attempts is reached, the user is locked out for five minutes (you can set the lock out time with `DefaultAccountLockoutTimeSpan`).</span></span>

<a id="addRes"></a>

## <a name="additional-resources"></a><span data-ttu-id="8b020-253">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="8b020-253">Additional Resources</span></span>

- <span data-ttu-id="8b020-254">[ASP.NET Identity zalecane zasobów](../getting-started/aspnet-identity-recommended-resources.md) pełną listę tożsamości blogi, wideo, samouczki i doskonałym tak łączy.</span><span class="sxs-lookup"><span data-stu-id="8b020-254">[ASP.NET Identity Recommended Resources](../getting-started/aspnet-identity-recommended-resources.md) Complete list of Identity blogs, videos, tutorials and great SO links.</span></span>
- <span data-ttu-id="8b020-255">[Aplikacji MVC 5 za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) także przedstawiono sposób dodawania informacji o profilu do tabeli użytkowników.</span><span class="sxs-lookup"><span data-stu-id="8b020-255">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) also shows how to add profile information to the users table.</span></span>
- <span data-ttu-id="8b020-256">[ASP.NET MVC i tożsamość 2.0: opis podstawowych funkcji](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) przez Atten Jan.</span><span class="sxs-lookup"><span data-stu-id="8b020-256">[ASP.NET MVC and Identity 2.0: Understanding the Basics](http://typecastexception.com/post/2014/04/20/ASPNET-MVC-and-Identity-20-Understanding-the-Basics.aspx) by John Atten.</span></span>
- [<span data-ttu-id="8b020-257">Potwierdzenie konta i hasło odzyskiwania za pomocą tożsamości platformy ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b020-257">Account Confirmation and Password Recovery with ASP.NET Identity</span></span>](account-confirmation-and-password-recovery-with-aspnet-identity.md)
- [<span data-ttu-id="8b020-258">Wprowadzenie do produktu ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="8b020-258">Introduction to ASP.NET Identity</span></span>](../getting-started/introduction-to-aspnet-identity.md)
- <span data-ttu-id="8b020-259">[Anonsowanie RTM programu ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) przez Pranav Rastogi.</span><span class="sxs-lookup"><span data-stu-id="8b020-259">[Announcing RTM of ASP.NET Identity 2.0.0](https://blogs.msdn.com/b/webdev/archive/2014/03/20/test-announcing-rtm-of-asp-net-identity-2-0-0.aspx) by Pranav Rastogi.</span></span>
- <span data-ttu-id="8b020-260">[Tożsamości ASP.NET 2.0: Sprawdzanie poprawności konta i dwuskładnikowego autoryzacji konfigurowania](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) przez Atten Jan.</span><span class="sxs-lookup"><span data-stu-id="8b020-260">[ASP.NET Identity 2.0: Setting Up Account Validation and Two-Factor Authorization](http://typecastexception.com/post/2014/04/20/ASPNET-Identity-20-Setting-Up-Account-Validation-and-Two-Factor-Authorization.aspx) by John Atten.</span></span>
