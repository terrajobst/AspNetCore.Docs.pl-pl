---
uid: mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
title: "Aplikacja ASP.NET MVC 5 z programu SMS i adres e-mail uwierzytelniania dwuskładnikowego | Dokumentacja firmy Microsoft"
author: Rick-Anderson
description: "Ten samouczek przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 5 z uwierzytelniania dwuskładnikowego. Należy wykonać aplikację sieci web tworzenie bezpiecznej platformy ASP.NET MVC 5 z..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/20/2015
ms.topic: article
ms.assetid: f50a5cdb-c06a-46ed-aa14-fc5b049dc8dc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication
msc.type: authoredcontent
ms.openlocfilehash: db57b8fe44f41d65d27964f45e0884138629f92b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication"></a><span data-ttu-id="3fa4a-104">Aplikacja ASP.NET MVC 5 z programu SMS i adres e-mail uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3fa4a-104">ASP.NET MVC 5 app with SMS and email Two-Factor Authentication</span></span>
====================
<span data-ttu-id="3fa4a-105">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="3fa4a-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="3fa4a-106">Ten samouczek przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 5 z uwierzytelniania dwuskładnikowego.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with Two-Factor Authentication.</span></span> <span data-ttu-id="3fa4a-107">Należy wykonać [utworzenia bezpiecznego aplikacji sieci web platformy ASP.NET MVC 5 z dziennika w resetowania hasła i potwierdzania poczty e-mail](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-107">You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="3fa4a-108">Możesz pobrać ukończona aplikacja [tutaj](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="3fa4a-108">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="3fa4a-109">Pobieranie zawiera debugowania wątków, które umożliwiają testowanie potwierdzenie adresu e-mail i SMS bez konfiguracji poczty e-mail lub dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-109">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="3fa4a-110">W tym samouczku, została napisana przy [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="3fa4a-110">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


- [<span data-ttu-id="3fa4a-111">Tworzenie aplikacji platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3fa4a-111">Create an ASP.NET MVC app</span></span>](#createMvc)
- [<span data-ttu-id="3fa4a-112">Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3fa4a-112">Set up SMS for Two-factor authentication</span></span>](#SMS)
- [<span data-ttu-id="3fa4a-113">Włącz uwierzytelnianie dwuskładnikowe</span><span class="sxs-lookup"><span data-stu-id="3fa4a-113">Enable Two-factor authentication</span></span>](#enable2)
- [<span data-ttu-id="3fa4a-114">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3fa4a-114">Additional Resources</span></span>](#addRes)

<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="3fa4a-115">Tworzenie aplikacji platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="3fa4a-115">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="3fa4a-116">Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="3fa4a-116">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="3fa4a-117">Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-117">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="3fa4a-118">Ostrzeżenie: Należy wykonać [utworzenia bezpiecznego aplikacji sieci web platformy ASP.NET MVC 5 z dziennika w resetowania hasła i potwierdzania poczty e-mail](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) przed kontynuowaniem.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-118">Warning: You should complete [Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset.md) before proceeding.</span></span> <span data-ttu-id="3fa4a-119">Musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej do ukończenia tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-119">You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="3fa4a-120">Utwórz nowy projekt sieci Web ASP.NET i wybierz szablon MVC.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-120">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="3fa4a-121">Formularze sieci Web obsługuje również tożsamości platformy ASP.NET, można wykonać podobne kroki w aplikacji formularzy sieci web.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-121">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image1.png)
2. <span data-ttu-id="3fa4a-122">Pozostaw domyślne uwierzytelnianie jako **indywidualnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-122">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="3fa4a-123">Jeśli chcesz udostępniać aplikacji na platformie Azure, pozostaw zaznaczone pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-123">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="3fa4a-124">Później w samouczku będziemy zostanie wdrożona na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-124">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="3fa4a-125">Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="3fa4a-125">You can [open an Azure account for free](https://azure.microsoft.com/en-us/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="3fa4a-126">Ustaw [projektu do używania protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="3fa4a-126">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>

<a id="SMS"></a>
## <a name="set-up-sms-for-two-factor-authentication"></a><span data-ttu-id="3fa4a-127">Konfigurowanie programu SMS dla uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3fa4a-127">Set up SMS for Two-factor authentication</span></span>

<span data-ttu-id="3fa4a-128">Ten samouczek zawiera instrukcje dotyczące korzystania z usługi Twilio lub ASPSMS, ale można użyć dowolnego dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-128">This tutorial provides instructions for using either Twilio or ASPSMS but you can use any other SMS provider.</span></span>

1. <span data-ttu-id="3fa4a-129">**Tworzenie konta użytkownika z dostawcą programu SMS**</span><span class="sxs-lookup"><span data-stu-id="3fa4a-129">**Creating a User Account with an SMS provider**</span></span>  
  
 <span data-ttu-id="3fa4a-130">Utwórz [usługi Twilio](https://www.twilio.com/try-twilio) lub [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) konta.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-130">Create a [Twilio](https://www.twilio.com/try-twilio) or an [ASPSMS](https://www.aspsms.com/asp.net/identity/testcredits/) account.</span></span>
2. <span data-ttu-id="3fa4a-131">**Instalowanie dodatkowych pakietów lub Dodawanie odwołań do usługi**</span><span class="sxs-lookup"><span data-stu-id="3fa4a-131">**Installing additional packages or adding service references**</span></span>  
  
 <span data-ttu-id="3fa4a-132">Usługi Twilio:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-132">Twilio:</span></span>  
 <span data-ttu-id="3fa4a-133">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-133">In the Package Manager Console, enter the following command:</span></span>  
    `Install-Package Twilio`  
  
 <span data-ttu-id="3fa4a-134">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-134">ASPSMS:</span></span>  
 <span data-ttu-id="3fa4a-135">Następujące odwołanie do usługi musi zostać dodany:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-135">The following service reference needs to be added:</span></span>  
  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image2.png)  
  
 <span data-ttu-id="3fa4a-136">Adres:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-136">Address:</span></span>  
    `https://webservice.aspsms.com/aspsmsx2.asmx?WSDL`  
  
 <span data-ttu-id="3fa4a-137">Przestrzeń nazw:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-137">Namespace:</span></span>  
    `ASPSMSX2`
3. <span data-ttu-id="3fa4a-138">**Ustaleniem, poświadczenia użytkownika dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="3fa4a-138">**Figuring out SMS Provider User credentials**</span></span>  
  
 <span data-ttu-id="3fa4a-139">Usługi Twilio:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-139">Twilio:</span></span>  
 <span data-ttu-id="3fa4a-140">Z **pulpitu nawigacyjnego** kartę konta usługi Twilio kopiowania **identyfikator SID konta** i **token uwierzytelniania**.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-140">From the **Dashboard** tab of your Twilio account, copy the **Account SID** and **Auth token**.</span></span>  
  
 <span data-ttu-id="3fa4a-141">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-141">ASPSMS:</span></span>  
 <span data-ttu-id="3fa4a-142">W ustawieniach konta, przejdź do **Userkey** i skopiować go razem z własnym zdefiniowanych **hasło**.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-142">From your account settings, navigate to **Userkey** and copy it together with your self-defined **Password**.</span></span>  
  
 <span data-ttu-id="3fa4a-143">Przechowujemy później tych wartości w *web.config* pliku w kluczach `"SMSAccountIdentification"` i `"SMSAccountPassword"` .</span><span class="sxs-lookup"><span data-stu-id="3fa4a-143">We will later store these values in the *web.config* file within the keys `"SMSAccountIdentification"` and `"SMSAccountPassword"` .</span></span>
4. <span data-ttu-id="3fa4a-144">**Określanie SenderID / inicjator**</span><span class="sxs-lookup"><span data-stu-id="3fa4a-144">**Specifying SenderID / Originator**</span></span>  
  
 <span data-ttu-id="3fa4a-145">Usługi Twilio:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-145">Twilio:</span></span>  
 <span data-ttu-id="3fa4a-146">Z **numery** karcie, skopiuj numer telefonu usługi Twilio.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-146">From the **Numbers** tab, copy your Twilio phone number.</span></span>  
  
 <span data-ttu-id="3fa4a-147">ASPSMS:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-147">ASPSMS:</span></span>  
 <span data-ttu-id="3fa4a-148">W ramach **odblokować nadawcy** Menu, odblokuj co najmniej jednego nadawcy, lub Wybierz zleceniodawcę alfanumeryczne (nieobsługiwane przez wszystkie sieci).</span><span class="sxs-lookup"><span data-stu-id="3fa4a-148">Within the **Unlock Originators** Menu, unlock one or more Originators or choose an alphanumeric Originator (Not supported by all networks).</span></span>  
  
 <span data-ttu-id="3fa4a-149">Przechowujemy później tę wartość w *web.config* pliku w kluczu `"SMSAccountFrom"` .</span><span class="sxs-lookup"><span data-stu-id="3fa4a-149">We will later store this value in the *web.config* file within the key `"SMSAccountFrom"` .</span></span>
5. <span data-ttu-id="3fa4a-150">**Transferowanie poświadczenia dostawcy programu SMS w aplikacji**</span><span class="sxs-lookup"><span data-stu-id="3fa4a-150">**Transferring SMS provider credentials into app**</span></span>  
  
 <span data-ttu-id="3fa4a-151">Udostępnij poświadczenia i numer telefonu nadawcy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-151">Make the credentials and sender phone number available to the app.</span></span> <span data-ttu-id="3fa4a-152">Aby zapewnić proste przechowujemy będzie tych wartości w *web.config* pliku.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-152">To keep things simple we will store these values in the *web.config* file.</span></span> <span data-ttu-id="3fa4a-153">Gdy firma Microsoft wdrażanie na platformie Azure, firma Microsoft może przechowywać bezpiecznie w wartości **ustawień aplikacji** Karta Konfigurowanie sekcji w witrynie sieci web.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-153">When we deploy to Azure, we can store the values securely in the **app settings** section on the web site configure tab.</span></span> 

    [!code-xml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample1.xml?highlight=8-10)]

    > [!WARNING]
    > <span data-ttu-id="3fa4a-154">Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-154">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="3fa4a-155">Konta i poświadczenia zostaną dodane do powyżej, aby zachować prosty przykład kodu.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-155">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="3fa4a-156">Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="3fa4a-156">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
6. <span data-ttu-id="3fa4a-157">**Implementacja transferu danych do dostawcy programu SMS**</span><span class="sxs-lookup"><span data-stu-id="3fa4a-157">**Implementation of data transfer to SMS provider**</span></span>  
  
 <span data-ttu-id="3fa4a-158">Skonfiguruj `SmsService` klasy w *aplikacji\_Start\IdentityConfig.cs* pliku.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-158">Configure the `SmsService`  class in the *App\_Start\IdentityConfig.cs* file.</span></span>  
  
 <span data-ttu-id="3fa4a-159">W zależności od dostawcy programu SMS używanego aktywować **usługi Twilio** lub **ASPSMS** sekcji:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-159">Depending on the used SMS provider activate either the **Twilio** or the **ASPSMS** section:</span></span> 

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample2.cs)]
7. <span data-ttu-id="3fa4a-160">Aktualizacja *Views\Manage\Index.cshtml* widoku Razor: (Uwaga: nie po prostu usuń komentarze w kodzie istniejących, użyj poniższy kod.)</span><span class="sxs-lookup"><span data-stu-id="3fa4a-160">Update the *Views\Manage\Index.cshtml* Razor view: (note: don't just remove the comments in the exiting code, use the code below.)</span></span>  

    [!code-cshtml[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample3.cshtml?highlight=29-66)]
8. <span data-ttu-id="3fa4a-161">Sprawdź `EnableTwoFactorAuthentication` i `DisableTwoFactorAuthentication` metod akcji w `ManageController` ma[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) atrybutu:</span><span class="sxs-lookup"><span data-stu-id="3fa4a-161">Verify the `EnableTwoFactorAuthentication` and `DisableTwoFactorAuthentication` action methods in the `ManageController` have the[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/en-us/library/system.web.mvc.validateantiforgerytokenattribute(v=vs.118).aspx) attribute:</span></span>  

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample4.cs?highlight=3,16)]
9. <span data-ttu-id="3fa4a-162">Uruchom aplikację i zaloguj się przy użyciu konta, które wcześniej zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-162">Run the app and log in with the account you previously registered.</span></span>
10. <span data-ttu-id="3fa4a-163">Kliknij nazwę użytkownika, który uaktywnia `Index` metody akcji w `Manage` kontrolera.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-163">Click on your User ID, which activates the `Index` action method in `Manage` controller.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image3.png)
11. <span data-ttu-id="3fa4a-164">Kliknij przycisk Dodaj.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-164">Click Add.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image4.png)
12. <span data-ttu-id="3fa4a-165">`AddPhoneNumber` Metody akcji Wyświetla okno dialogowe, aby wprowadzić numer telefonu, który może odbierać wiadomości SMS.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-165">The `AddPhoneNumber` action method displays a dialog box to enter a phone number that can receive SMS messages.</span></span>

    [!code-csharp[Main](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/samples/sample5.cs)]

    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image5.png)
13. <span data-ttu-id="3fa4a-166">Za chwilę otrzymasz wiadomość SMS z kodem weryfikacyjnym.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-166">In a few seconds you will get a text message with the verification code.</span></span> <span data-ttu-id="3fa4a-167">Wprowadź go i naciśnij klawisz **przesyłania**.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-167">Enter it and press **Submit**.</span></span>  
    ![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image6.png)
14. <span data-ttu-id="3fa4a-168">Zarządzaj ilustracja pokazuje numer telefonu został dodany.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-168">The Manage view shows your phone number was added.</span></span>

<a id="enable2"></a>
## <a name="enable-two-factor-authentication"></a><span data-ttu-id="3fa4a-169">Włącz uwierzytelnianie dwuskładnikowe</span><span class="sxs-lookup"><span data-stu-id="3fa4a-169">Enable two-factor authentication</span></span>

<span data-ttu-id="3fa4a-170">W aplikacji szablonu wygenerowanego należy użyć interfejsu użytkownika, aby włączyć uwierzytelnianie dwuskładnikowe (2FA).</span><span class="sxs-lookup"><span data-stu-id="3fa4a-170">In the template generated app, you need to use the UI to enable two-factor authentication (2FA).</span></span> <span data-ttu-id="3fa4a-171">Aby włączyć 2FA, kliknij nazwę użytkownika (alias e-mail) na pasku nawigacyjnym.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-171">To enable 2FA, click on your user ID (email alias) in the navigation bar.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image7.png)

<span data-ttu-id="3fa4a-172">Kliknij pozycję Włącz 2FA.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-172">Click on enable 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image8.png)

<span data-ttu-id="3fa4a-173">Wyloguj, następnie zalogować ponownie w.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-173">Logout, then log back in.</span></span> <span data-ttu-id="3fa4a-174">Jeśli włączono poczty e-mail (Zobacz Moje [poprzedniego samouczek](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), możesz wybrać SMS lub wiadomości e-mail w przypadku 2FA.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-174">If you've enabled email (see my [previous tutorial](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md)), you can select the SMS or email for 2FA.</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image9.png)

<span data-ttu-id="3fa4a-175">Zostanie wyświetlona strona Sprawdź kod, gdzie można wprowadzić kod (z programu SMS lub e-mail).</span><span class="sxs-lookup"><span data-stu-id="3fa4a-175">The Verify Code page is displayed where you can enter the code (from SMS or email).</span></span>

![](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication/_static/image10.png)

<span data-ttu-id="3fa4a-176">Kliknięcie **Pamiętaj przeglądarka** pola wyboru będzie zwalnia z konieczności służy do logowania podczas korzystania z przeglądarki i urządzenia, gdy zaznaczono pole 2FA.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-176">Clicking on the **Remember this browser** check box will exempt you from needing to use 2FA to log in when using the browser and device where you checked the box.</span></span> <span data-ttu-id="3fa4a-177">Tak długo, jak złośliwych użytkowników nie może uzyskać dostępu do urządzenia, włączanie 2FA i klikając **Pamiętaj przeglądarka** zapewnia najwygodniejszy dostęp hasła jeden krok, przy jednoczesnym zachowaniu 2FA silnej ochrony dostępu do całej z urządzeń z systemem innym niż zaufane.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-177">As long as malicious users can't gain access to your device, enabling 2FA and clicking on the **Remember this browser** will provide you with convenient one step password access, while still retaining strong 2FA protection for all access from non-trusted devices.</span></span> <span data-ttu-id="3fa4a-178">Można to zrobić na dowolnym urządzeniu prywatne, regularnie używane.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-178">You can do this on any private device you regularly use.</span></span>

<span data-ttu-id="3fa4a-179">W tym samouczku przedstawiono krótkie wprowadzenie do włączania 2FA w nowej aplikacji ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-179">This tutorial provides a quick introduction to enabling 2FA on a new ASP.NET MVC app.</span></span> <span data-ttu-id="3fa4a-180">Moje samouczek [uwierzytelniania dwuskładnikowego przy użyciu programu SMS i wiadomości e-mail z tożsamości ASP.NET](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) przejdzie do szczegółów w kodzie próbki.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-180">My tutorial [Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) goes into detail on the code behind the sample.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="3fa4a-181">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="3fa4a-181">Additional Resources</span></span>

- <span data-ttu-id="3fa4a-182">[Uwierzytelnianie dwuskładnikowe przy użyciu programu SMS i wiadomości e-mail z tożsamości ASP.NET](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) przechodzi do szczegółów dotyczących uwierzytelniania dwuskładnikowego</span><span class="sxs-lookup"><span data-stu-id="3fa4a-182">[Two-factor authentication using SMS and email with ASP.NET Identity](../../../identity/overview/features-api/two-factor-authentication-using-sms-and-email-with-aspnet-identity.md) Goes into detail on Two-factor authentication</span></span>
- [<span data-ttu-id="3fa4a-183">Łącza do tożsamości ASP.NET zalecane zasobów</span><span class="sxs-lookup"><span data-stu-id="3fa4a-183">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="3fa4a-184">[Konta potwierdzenie i hasło odzyskiwania za pomocą ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) przechodzi w bardziej szczegółowo na potwierdzenie hasła odzyskiwania i konta.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-184">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="3fa4a-185">[Aplikacji MVC 5 za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) w tym samouczku przedstawiono sposób pisania aplikacji platformy ASP.NET MVC 5 z usługi Facebook i Google OAuth 2 autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-185">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="3fa4a-186">Ponadto jak dodać dodatkowe dane do bazy danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-186">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="3fa4a-187">[Wdrażanie aplikacji bezpiecznego platformy ASP.NET MVC z członkostwa, OAuth i bazy danych SQL Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="3fa4a-187">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure Web](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="3fa4a-188">W tym samouczku dodaje wdrożenia usługi Azure zabezpieczania aplikacji za pomocą ról, jak używać interfejs API członkostwa można dodać użytkowników i role oraz dodatkowe funkcje zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="3fa4a-188">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="3fa4a-189">Tworzenie aplikacji Google OAuth 2 i łączenie aplikacji z projektu</span><span class="sxs-lookup"><span data-stu-id="3fa4a-189">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="3fa4a-190">Tworzenie aplikacji w serwisie Facebook i łączenie aplikacji z projektu</span><span class="sxs-lookup"><span data-stu-id="3fa4a-190">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="3fa4a-191">Konfigurowanie protokołu SSL w projekcie</span><span class="sxs-lookup"><span data-stu-id="3fa4a-191">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
