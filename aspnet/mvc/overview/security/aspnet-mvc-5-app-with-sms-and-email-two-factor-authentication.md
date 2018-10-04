---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: Aplikacja ASP.NET MVC 5 z wiadomości SMS i wiadomości e-mail uwierzytelniania dwuskładnikowego | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku dowiesz się, jak utworzyć aplikację internetową platformy ASP.NET MVC 5 przy użyciu uwierzytelniania dwuskładnikowego. Należy wykonać tworzenie bezpiecznej platformy ASP.NET MVC 5 aplikacji internetowej za pomocą...
ms.author: riande
ms.date: 08/20/2015
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: 97b73c5ae18a528d33b44a4de4afc434ac46af48
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577603"
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="9475f-104">Aplikacja ASP.NET MVC 5 z wiadomości SMS i wiadomości e-mail uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="9475f-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="9475f-105">Przez [Rick Anderson]((https://twitter.com/RickAndMSFT))</span><span class="sxs-lookup"><span data-stu-id="9475f-105">by [Rick Anderson]((https://twitter.com/RickAndMSFT))</span></span>

> <span data-ttu-id="9475f-106">W tym samouczku dowiesz się, jak utworzyć aplikację internetową platformy ASP.NET MVC 5 przy użyciu uwierzytelniania dwuskładnikowego.</span><span class="sxs-lookup"><span data-stu-id="9475f-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="9475f-107">Należy wykonać [tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, adres e-mail potwierdzenia i resetowaniem hasła](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="9475f-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="9475f-108">Możesz pobrać gotową aplikację [tutaj](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="9475f-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="9475f-109">Pobierany zasób zawiera debugowania wątków, które umożliwiają testowanie potwierdzenie adresu e-mail i SMS bez konfigurowania wiadomości e-mail lub dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="9475f-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="9475f-110">Ten samouczek został napisany przez [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="9475f-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="9475f-111">Tworzenie aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="9475f-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="9475f-112">Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="9475f-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="9475f-113">Włączanie uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="9475f-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="9475f-114">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9475f-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="9475f-115">Tworzenie aplikacji ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="9475f-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="9475f-116">Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="9475f-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="9475f-117">Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="9475f-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="9475f-118">Ostrzeżenie: Należy wykonać [tworzenie bezpiecznej aplikacji sieci web ASP.NET MVC 5 z logowaniem, adres e-mail potwierdzenia i resetowaniem hasła](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="9475f-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="9475f-119">Należy zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej, aby ukończyć ten samouczek.</span><span class="sxs-lookup"><span data-stu-id="9475f-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="9475f-120">Utwórz nowy projekt sieci Web platformy ASP.NET, a następnie wybierz szablon MVC.</span><span class="sxs-lookup"><span data-stu-id="9475f-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="9475f-121">Formularze sieci Web obsługuje również produktu ASP.NET Identity, dzięki czemu można wykonać podobne kroki w aplikacji formularzy sieci web.</span><span class="sxs-lookup"><span data-stu-id="9475f-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="9475f-122">Pozostaw domyślne uwierzytelnianie jako **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="9475f-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="9475f-123">Jeśli chcesz hostować aplikację na platformie Azure, pozostaw zaznaczone pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="9475f-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="9475f-124">W dalszej części tego samouczka wdrożymy na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="9475f-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="9475f-125">Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="9475f-125">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="9475f-126">Ustaw [projektu do używania protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="9475f-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="9475f-127">Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="9475f-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="9475f-128">Ten samouczek zawiera instrukcje dotyczące korzystania z usługi Twilio lub ASPSMS, ale można użyć dowolnego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="9475f-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="9475f-129">**Tworzenie konta użytkownika z dostawcą programu SMS**</span><span class="sxs-lookup"><span data-stu-id="9475f-129">**Creating a User Account with an SMS provider**</span></span>  
  
   <span data-ttu-id="9475f-130">Tworzenie [Twilio](https://www.twilio.com/try-twilio) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) konta.</span><span class="sxs-lookup"><span data-stu-id="9475f-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="9475f-131">**Instalowanie dodatkowych pakietów lub dodawania odwołania do usług**</span><span class="sxs-lookup"><span data-stu-id="9475f-131">**Installing additional packages or adding service references**</span></span>  
  
   <span data-ttu-id="9475f-132">Twilio:</span><span class="sxs-lookup"><span data-stu-id="9475f-132">Twilio:</span></span>  
   <span data-ttu-id="9475f-133">W konsoli Menedżera pakietów wpisz następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="9475f-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
   <span data-ttu-id="9475f-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="9475f-134">ASPSMS:</span></span>  
   <span data-ttu-id="9475f-135">Należy dodać następujące odwołanie do usługi:</span><span class="sxs-lookup"><span data-stu-id="9475f-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
   <span data-ttu-id="9475f-136">Adres:</span><span class="sxs-lookup"><span data-stu-id="9475f-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
   <span data-ttu-id="9475f-137">Przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="9475f-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="9475f-138">**Ustalenie, poświadczenia użytkownika dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="9475f-138">**Figuring out SMS Provider User credentials**</span></span>  
  
   <span data-ttu-id="9475f-139">Twilio:</span><span class="sxs-lookup"><span data-stu-id="9475f-139">Twilio:</span></span>  
   <span data-ttu-id="9475f-140">Z **pulpit nawigacyjny** karty konta usługi Twilio, kopiowania **identyfikator SID konta** i **tokenu uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="9475f-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
   <span data-ttu-id="9475f-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="9475f-141">ASPSMS:</span></span>  
   <span data-ttu-id="9475f-142">W ustawieniach konta, przejdź do **Userkey** i skopiować go wraz z własnym zdefiniowanych **hasło**.</span><span class="sxs-lookup"><span data-stu-id="9475f-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
   <span data-ttu-id="9475f-143">Te wartości będą przechowywane w dalszej części *web.config* pliku w kluczach `"SMSAccountIdentification"` i `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="9475f-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="9475f-144">**Określanie SenderID / inicjatora**</span><span class="sxs-lookup"><span data-stu-id="9475f-144">**Specifying SenderID / Originator**</span></span>  
  
   <span data-ttu-id="9475f-145">Twilio:</span><span class="sxs-lookup"><span data-stu-id="9475f-145">Twilio:</span></span>  
   <span data-ttu-id="9475f-146">Z **numery** kartę, skopiuj numer telefonu usługi Twilio.</span><span class="sxs-lookup"><span data-stu-id="9475f-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
   <span data-ttu-id="9475f-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="9475f-147">ASPSMS:</span></span>  
   <span data-ttu-id="9475f-148">W ramach **odblokować nadawcy** Menu odblokować przynajmniej jednego nadawcy, lub wybierz inicjatora alfanumeryczne (nie obsługiwany przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="9475f-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
   <span data-ttu-id="9475f-149">Później będą przechowywane w tej wartości w *web.config* pliku w kluczu `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="9475f-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="9475f-150">**Transferowanie poświadczenia dostawcy programu SMS w aplikacji**</span><span class="sxs-lookup"><span data-stu-id="9475f-150">**Transferring SMS provider credentials into app**</span></span>  
  
   <span data-ttu-id="9475f-151">Udostępnij poświadczenia i numer telefonu nadawcy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="9475f-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="9475f-152">Aby zachować ich prostotę będą przechowywane te wartości w *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="9475f-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="9475f-153">Gdy firma Microsoft wdrażanie na platformie Azure, firma Microsoft może przechowywać bezpiecznie w wartości **ustawienia aplikacji** Karta Konfigurowanie sekcji w witrynie sieci web.</span><span class="sxs-lookup"><span data-stu-id="9475f-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="9475f-154">Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="9475f-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="9475f-155">Konto i poświadczenia są dodawane do kodu powyżej, aby uprościć przykład.</span><span class="sxs-lookup"><span data-stu-id="9475f-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="9475f-156">Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="9475f-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="9475f-157">**Implementacja transfer danych do dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="9475f-157">**Implementation of data transfer to SMS provider**</span></span>  
  
   <span data-ttu-id="9475f-158">Konfigurowanie `SmsService` klasy w *aplikacji\_Start\IdentityConfig.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="9475f-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
   <span data-ttu-id="9475f-159">W zależności od dostawcy programu SMS używanej aktywować **Twilio** lub **ASPSMS** sekcji:</span><span class="sxs-lookup"><span data-stu-id="9475f-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="9475f-160">Aktualizacja *Views\Manage\Index.cshtml* widoku Razor: (Uwaga: nie po prostu usuń komentarze w kodzie istniejącej, użyj poniższego kodu.)</span><span class="sxs-lookup"><span data-stu-id="9475f-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="9475f-161">Sprawdź `EnableTwoFactorAuthentication` i `DisableTwoFactorAuthentication` metod akcji w `ManageController` mają[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="9475f-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="9475f-162">Uruchom aplikację i zaloguj się przy użyciu konta które wcześniej zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="9475f-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="9475f-163">Kliknij swój identyfikator użytkownika, które uaktywnia się `Index` metody akcji w `Manage` kontrolera.</span><span class="sxs-lookup"><span data-stu-id="9475f-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="9475f-164">Kliknij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="9475f-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="9475f-165">`AddPhoneNumber` Metody akcji, wyświetlane jest okno dialogowe, aby wprowadzić numer telefonu, który może odbierać wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="9475f-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="9475f-166">W ciągu kilku sekund otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="9475f-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="9475f-167">Wprowadź go, a następnie naciśnij klawisz **przesyłania**.</span><span class="sxs-lookup"><span data-stu-id="9475f-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="9475f-168">W widoku Zarządzanie pokazuje, że numer telefonu został dodany.</span><span class="sxs-lookup"><span data-stu-id="9475f-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="9475f-169">Włączanie uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="9475f-169">Enable two-factor authentication</span></span>

<span data-ttu-id="9475f-170">W aplikacji wygenerowanego szablonu należy włączyć uwierzytelnianie dwuskładnikowe (2FA) za pomocą interfejsu użytkownika.</span><span class="sxs-lookup"><span data-stu-id="9475f-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="9475f-171">Aby włączyć funkcję 2FA, kliknij swoją nazwę użytkownika (alias adresu e-mail) na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="9475f-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="9475f-172">Kliknij pozycję Włącz 2FA.</span><span class="sxs-lookup"><span data-stu-id="9475f-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="9475f-173">Wylogowania, następnie zalogowania.</span><span class="sxs-lookup"><span data-stu-id="9475f-173">Logout, then log back in.</span></span> <span data-ttu-id="9475f-174">Po włączeniu poczty e-mail (zobacz mój [poprzedniego samouczka](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać wiadomości SMS lub wiadomości e-mail dla funkcji 2FA.</span><span class="sxs-lookup"><span data-stu-id="9475f-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="9475f-175">Zostanie wyświetlona strona Sprawdź kodu, gdzie można wprowadzić kod (z wiadomości SMS lub wiadomości e-mail).</span><span class="sxs-lookup"><span data-stu-id="9475f-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="9475f-176">Kliknięcie **pamiętasz tę przeglądarkę** pole wyboru zostaną wykluczone z konieczności korzystania z funkcji 2FA, zaloguj się w przypadku korzystania z przeglądarki i urządzenia gdy zaznaczono pole.</span><span class="sxs-lookup"><span data-stu-id="9475f-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="9475f-177">Tak długo, jak złośliwi użytkownicy nie mogą uzyskać dostęp do urządzenia, włączanie funkcji 2FA i klikając **pamiętasz tę przeglądarkę** zapewnia wygodny dostęp hasło jeden krok, przy jednoczesnym zachowaniu silnego uwierzytelniania 2FA ochrony dostępu do całej z innych zaufanych urządzeń.</span><span class="sxs-lookup"><span data-stu-id="9475f-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="9475f-178">Można to zrobić, na dowolnym urządzeniu prywatnych, regularnie używane.</span><span class="sxs-lookup"><span data-stu-id="9475f-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="9475f-179">Ten samouczek zawiera szybkie wprowadzenie do włączania funkcji 2FA w nowej aplikacji platformy ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="9475f-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="9475f-180">Moje samouczek [uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) określa szczegółowo kod związany z przykładu.</span><span class="sxs-lookup"><span data-stu-id="9475f-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="9475f-181">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="9475f-181">Additional Resources</span></span>

- <span data-ttu-id="9475f-182">[Uwierzytelnianie dwuskładnikowe za pomocą wiadomości SMS i wiadomości e-mail w produkcie ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) przechodzi do szczegółowych informacji dotyczących uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="9475f-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="9475f-183">Łącza do produktu ASP.NET Identity zalecane zasoby</span><span class="sxs-lookup"><span data-stu-id="9475f-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="9475f-184">[Potwierdzenie konta i odzyskiwanie hasła w produkcie ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) zawiera bardziej szczegółowe potwierdzenie hasła odzyskiwania i konta.</span><span class="sxs-lookup"><span data-stu-id="9475f-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="9475f-185">[MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowanie jednokrotne](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) w tym samouczku dowiesz się, jak napisać aplikację ASP.NET MVC 5 z autoryzacją usługi Facebook i Google OAuth 2.</span><span class="sxs-lookup"><span data-stu-id="9475f-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="9475f-186">Pokazano również, jak dodać dodatkowe dane do bazy danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="9475f-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="9475f-187">[Wdrażanie bezpiecznej aplikacji ASP.NET MVC z członkostwa, uwierzytelnianiem OAuth i bazą danych SQL na sieci Web platformy Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="9475f-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="9475f-188">W tym samouczku dodaje wdrażania platformy Azure, jak zabezpieczyć swoją aplikację przy użyciu ról, jak dodać użytkowników i ról i funkcji zabezpieczeń za pomocą członkostwo interfejsu API.</span><span class="sxs-lookup"><span data-stu-id="9475f-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="9475f-189">Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji do projektu</span><span class="sxs-lookup"><span data-stu-id="9475f-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="9475f-190">Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji do projektu</span><span class="sxs-lookup"><span data-stu-id="9475f-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="9475f-191">Konfigurowanie protokołu SSL w projekcie</span><span class="sxs-lookup"><span data-stu-id="9475f-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
