---
uid: mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
title: Tworzenie bezpiecznej aplikacji sieci web platformy ASP.NET MVC 5 z dziennikiem, poczty e-mail resetowania hasła i potwierdzania (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 5 z wiadomości e-mail z potwierdzeniem i resetowania przy użyciu systemu członkostwa ASP.NET Identity hasła. Możesz urzędu certyfikacji...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: d4911cb3-1afb-4805-b860-10818c4b1280
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset
msc.type: authoredcontent
ms.openlocfilehash: bfa5d52019be81374c7a544e255ab7ffb301fa7b
ms.sourcegitcommit: 50d40c83fa641d283c097f986dde5341ebe1b44c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/22/2018
---
<a name="create-a-secure-aspnet-mvc-5-web-app-with-log-in-email-confirmation-and-password-reset-c"></a><span data-ttu-id="54f21-104">Tworzenie bezpiecznej aplikacji sieci web platformy ASP.NET MVC 5 z dziennikiem, poczty e-mail resetowania hasła i potwierdzania (C#)</span><span class="sxs-lookup"><span data-stu-id="54f21-104">Create a secure ASP.NET MVC 5 web app with log in, email confirmation and password reset (C#)</span></span>
====================
<span data-ttu-id="54f21-105">przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="54f21-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="54f21-106">Ten samouczek przedstawia sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 5 z wiadomości e-mail z potwierdzeniem i resetowania przy użyciu systemu członkostwa ASP.NET Identity hasła.</span><span class="sxs-lookup"><span data-stu-id="54f21-106">This tutorial shows you how to build an ASP.NET MVC 5 web app with email confirmation and password reset using the ASP.NET Identity membership system.</span></span> <span data-ttu-id="54f21-107">Możesz pobrać ukończona aplikacja [tutaj](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="54f21-107">You can download the completed application [here](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="54f21-108">Pobieranie zawiera debugowania wątków, które umożliwiają testowanie potwierdzenie adresu e-mail i SMS bez konfiguracji poczty e-mail lub dostawcy programu SMS.</span><span class="sxs-lookup"><span data-stu-id="54f21-108">The download contains debugging helpers that let you test email confirmation and SMS without setting up an email or SMS provider.</span></span>
> 
> <span data-ttu-id="54f21-109">W tym samouczku, została napisana przy [Rick Anderson](https://blogs.msdn.com/rickAndy) (Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="54f21-109">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="createMvc"></a>
## <a name="create-an-aspnet-mvc-app"></a><span data-ttu-id="54f21-110">Tworzenie aplikacji platformy ASP.NET MVC</span><span class="sxs-lookup"><span data-stu-id="54f21-110">Create an ASP.NET MVC app</span></span>

<span data-ttu-id="54f21-111">Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="54f21-111">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="54f21-112">Zainstaluj [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="54f21-112">Install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher.</span></span>

> [!NOTE]
> <span data-ttu-id="54f21-113">Ostrzeżenie: Musisz zainstalować [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) lub nowszej do ukończenia tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="54f21-113">Warning: You must install [Visual Studio 2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390465) or higher to complete this tutorial.</span></span>


1. <span data-ttu-id="54f21-114">Utwórz nowy projekt sieci Web ASP.NET i wybierz szablon MVC.</span><span class="sxs-lookup"><span data-stu-id="54f21-114">Create a new ASP.NET Web project and select the MVC template.</span></span> <span data-ttu-id="54f21-115">Formularze sieci Web obsługuje również tożsamości platformy ASP.NET, można wykonać podobne kroki w aplikacji formularzy sieci web.</span><span class="sxs-lookup"><span data-stu-id="54f21-115">Web Forms also supports ASP.NET Identity, so you could follow similar steps in a web forms app.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image1.png)
2. <span data-ttu-id="54f21-116">Pozostaw domyślne uwierzytelnianie jako **indywidualnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="54f21-116">Leave the default authentication as **Individual User Accounts**.</span></span> <span data-ttu-id="54f21-117">Jeśli chcesz udostępniać aplikacji na platformie Azure, pozostaw zaznaczone pole wyboru.</span><span class="sxs-lookup"><span data-stu-id="54f21-117">If you'd like to host the app in Azure, leave the check box checked.</span></span> <span data-ttu-id="54f21-118">Później w samouczku będziemy zostanie wdrożona na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="54f21-118">Later in the tutorial we will deploy to Azure.</span></span> <span data-ttu-id="54f21-119">Możesz [otworzyć bezpłatne konto platformy Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="54f21-119">You can [open an Azure account for free](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).</span></span>
3. <span data-ttu-id="54f21-120">Ustaw [projektu do używania protokołu SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span><span class="sxs-lookup"><span data-stu-id="54f21-120">Set the [project to use SSL](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
4. <span data-ttu-id="54f21-121">Uruchom aplikację, kliknij przycisk **zarejestrować** i łącza do zarejestrowania użytkownika.</span><span class="sxs-lookup"><span data-stu-id="54f21-121">Run the app, click the **Register** link and register a user.</span></span> <span data-ttu-id="54f21-122">W tym momencie jest tylko sprawdzanie poprawności w wiadomości e-mail z [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) atrybutu.</span><span class="sxs-lookup"><span data-stu-id="54f21-122">At this point, the only validation on the email is with the [[EmailAddress]](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.emailaddressattribute(v=vs.110).aspx) attribute.</span></span>
5. <span data-ttu-id="54f21-123">W Eksploratorze serwera, przejdź do **Connections\DefaultConnection\Tables\AspNetUsers danych**, kliknij prawym przyciskiem myszy i wybierz **Otwórz definicję tabeli**.</span><span class="sxs-lookup"><span data-stu-id="54f21-123">In Server Explorer, navigate to **Data Connections\DefaultConnection\Tables\AspNetUsers**, right click and select **Open table definition**.</span></span>

    <span data-ttu-id="54f21-124">Poniższy obraz przedstawia `AspNetUsers` schematu:</span><span class="sxs-lookup"><span data-stu-id="54f21-124">The following image shows the `AspNetUsers` schema:</span></span>

    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image2.png)
6. <span data-ttu-id="54f21-125">Kliknij prawym przyciskiem myszy **AspNetUsers** tabeli i wybierz **Pokaż dane tabeli**.</span><span class="sxs-lookup"><span data-stu-id="54f21-125">Right click on the **AspNetUsers** table and select **Show Table Data**.</span></span>  
    ![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image3.png)  
 <span data-ttu-id="54f21-126">W tym momencie wiadomości e-mail nie został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="54f21-126">At this point the email has not been confirmed.</span></span>
7. <span data-ttu-id="54f21-127">Kliknij wiersz i wybierz polecenie Usuń.</span><span class="sxs-lookup"><span data-stu-id="54f21-127">Click on the row and select delete.</span></span> <span data-ttu-id="54f21-128">Będzie ponownie dodać ten adres e-mail w następnym kroku, a następnie wyślij wiadomość e-mail z potwierdzeniem.</span><span class="sxs-lookup"><span data-stu-id="54f21-128">You'll add this email again in the next step, and send a confirmation email.</span></span>

## <a name="email-confirmation"></a><span data-ttu-id="54f21-129">Wiadomości e-mail z potwierdzeniem</span><span class="sxs-lookup"><span data-stu-id="54f21-129">Email confirmation</span></span>

<span data-ttu-id="54f21-130">Jest najlepszym rozwiązaniem, aby potwierdzić nowej rejestracji użytkownika, aby sprawdzić ich są nie przeprowadza personifikacji ktoś inny adres e-mail (to znaczy one nie został zarejestrowany przy do kogoś innego adresu e-mail).</span><span class="sxs-lookup"><span data-stu-id="54f21-130">It's a best practice to confirm the email of a new user registration to verify they are not impersonating someone else (that is, they haven't registered with someone else's email).</span></span> <span data-ttu-id="54f21-131">Załóżmy, że masz forum dyskusyjne, czy chcesz zapobiec `"bob@example.com"` z rejestracją jako `"joe@contoso.com"`.</span><span class="sxs-lookup"><span data-stu-id="54f21-131">Suppose you had a discussion forum, you would want to prevent `"bob@example.com"` from registering as `"joe@contoso.com"`.</span></span> <span data-ttu-id="54f21-132">Bez wiadomości e-mail z potwierdzeniem `"joe@contoso.com"` można pobrać niechcianych wiadomości e-mail z aplikacji.</span><span class="sxs-lookup"><span data-stu-id="54f21-132">Without email confirmation, `"joe@contoso.com"` could get unwanted email from your app.</span></span> <span data-ttu-id="54f21-133">Załóżmy, że Bob przypadkowo zarejestrowany jako `"bib@example.com"` i nie zauważyć, ADAM nie będą mogli używać hasła odzyskiwania, ponieważ aplikacja nie ma jego prawidłowy adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="54f21-133">Suppose Bob accidently registered as `"bib@example.com"` and hadn't noticed it, he wouldn't be able to use password recover because the app doesn't have his correct email.</span></span> <span data-ttu-id="54f21-134">Wiadomości e-mail z potwierdzeniem chroni tylko ograniczone z robotów i nie zapewnia ochrony z określone nadawcy wiadomości-śmieci, ponieważ mają one wiele aliasów e-mail pracy służące do rejestrowania.</span><span class="sxs-lookup"><span data-stu-id="54f21-134">Email confirmation provides only limited protection from bots and doesn't provide protection from determined spammers, they have many working email aliases they can use to register.</span></span>

<span data-ttu-id="54f21-135">Ogólnie rzecz biorąc chcesz uniemożliwić przesyłanie danych do witryny sieci web, zanim zostały potwierdzone za pośrednictwem poczty e-mail, wiadomość SMS lub inny mechanizm nowych użytkowników.</span><span class="sxs-lookup"><span data-stu-id="54f21-135">You generally want to prevent new users from posting any data to your web site before they have been confirmed by email, a SMS text message or another mechanism.</span></span> <a id="build"></a><span data-ttu-id="54f21-136">W poniższych sekcjach możemy włączy wiadomości e-mail z potwierdzeniem i zmodyfikuj kod, aby uniemożliwić użytkownikom nowo zarejestrowanych logowanie do momentu swój adres e-mail został potwierdzony.</span><span class="sxs-lookup"><span data-stu-id="54f21-136">In the sections below, we will enable email confirmation and modify the code to prevent newly registered users from logging in until their email has been confirmed.</span></span>

<a id="SG"></a>
## <a name="hook-up-sendgrid"></a><span data-ttu-id="54f21-137">Podłączanie SendGrid</span><span class="sxs-lookup"><span data-stu-id="54f21-137">Hook up SendGrid</span></span>

<span data-ttu-id="54f21-138">Mimo że w tym samouczku tylko przedstawiono sposób dodawania powiadomienia pocztą e-mail za pomocą [SendGrid](http://sendgrid.com/), możesz wysłać wiadomości e-mail przy użyciu SMTP i innych mechanizmów (zobacz [dodatkowe zasoby](#addRes)).</span><span class="sxs-lookup"><span data-stu-id="54f21-138">Although this tutorial only shows how to add email notification through [SendGrid](http://sendgrid.com/), you can send email using SMTP and other mechanisms (see [additional resources](#addRes)).</span></span>

1. <span data-ttu-id="54f21-139">W konsoli Menedżera pakietów wprowadź następujące polecenie:</span><span class="sxs-lookup"><span data-stu-id="54f21-139">In the Package Manager Console, enter the following command:</span></span> 

    [!code-console[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample1.cmd)]
2. <span data-ttu-id="54f21-140">Przejdź do [stronę Tworzenie konta Azure SendGrid](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) i zarejestruj bezpłatne konto SendGrid.</span><span class="sxs-lookup"><span data-stu-id="54f21-140">Go to the [Azure SendGrid sign up page](https://go.microsoft.com/fwlink/?linkid=271033&clcid=0x409) and register for a free SendGrid account.</span></span> <span data-ttu-id="54f21-141">Skonfiguruj SendGrid przez dodanie kodu podobne do następującego *App_Start/IdentityConfig.cs*:</span><span class="sxs-lookup"><span data-stu-id="54f21-141">Configure SendGrid by adding code similar to the following in *App_Start/IdentityConfig.cs*:</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample2.cs?highlight=3,5)]

<span data-ttu-id="54f21-142">Musisz dodać zawiera następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="54f21-142">You'll need to add the following includes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample3.cs)]

<span data-ttu-id="54f21-143">Aby zachować ten przykład prostego, możemy przechowywania ustawień aplikacji w *web.config* pliku:</span><span class="sxs-lookup"><span data-stu-id="54f21-143">To keep this sample simple, we'll store the app settings in the *web.config* file:</span></span>

[!code-xml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample4.xml)]

> [!WARNING]
> <span data-ttu-id="54f21-144">Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="54f21-144">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="54f21-145">Konto i poświadczenia są przechowywane w appSetting.</span><span class="sxs-lookup"><span data-stu-id="54f21-145">The account and credentials are stored in the appSetting.</span></span> <span data-ttu-id="54f21-146">Na platformie Azure, można bezpiecznie przechowywać te wartości na **[Konfiguruj](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** kartę w portalu Azure.</span><span class="sxs-lookup"><span data-stu-id="54f21-146">On Azure, you can securely store these values on the **[Configure](https://blogs.msdn.com/b/webdev/archive/2014/06/04/queuebackgroundworkitem-to-reliably-schedule-and-run-long-background-process-in-asp-net.aspx)** tab in the Azure portal.</span></span> <span data-ttu-id="54f21-147">Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="54f21-147">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>


### <a name="enable-email-confirmation-in-the-account-controller"></a><span data-ttu-id="54f21-148">Włącz wiadomości e-mail z potwierdzeniem w kontrolerze konta</span><span class="sxs-lookup"><span data-stu-id="54f21-148">Enable email confirmation in the Account controller</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample5.cs?highlight=16-21)]

<span data-ttu-id="54f21-149">Sprawdź *Views\Account\ConfirmEmail.cshtml* plik ma składni razor poprawne.</span><span class="sxs-lookup"><span data-stu-id="54f21-149">Verify the *Views\Account\ConfirmEmail.cshtml* file has correct razor syntax.</span></span> <span data-ttu-id="54f21-150">(@-Znak w pierwszym wierszu może brakować.</span><span class="sxs-lookup"><span data-stu-id="54f21-150">( The @ character in the first line might be missing.</span></span> <span data-ttu-id="54f21-151">)</span><span class="sxs-lookup"><span data-stu-id="54f21-151">)</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample6.cshtml?highlight=1)]

<span data-ttu-id="54f21-152">Uruchom aplikację i kliknąć łącze rejestru.</span><span class="sxs-lookup"><span data-stu-id="54f21-152">Run the app and click the Register link.</span></span> <span data-ttu-id="54f21-153">Po przesłaniu formularza rejestracji użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="54f21-153">Once you submit the registration form, you are logged in.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image4.png)

<span data-ttu-id="54f21-154">Sprawdź swoje konto e-mail i kliknij link, aby potwierdzić swój adres e-mail.</span><span class="sxs-lookup"><span data-stu-id="54f21-154">Check your email account and click on the link to confirm your email.</span></span>

<a id="require"></a>
## <a name="require-email-confirmation-before-log-in"></a><span data-ttu-id="54f21-155">Wymagaj wiadomości e-mail z potwierdzeniem przed logowania</span><span class="sxs-lookup"><span data-stu-id="54f21-155">Require email confirmation before log in</span></span>

<span data-ttu-id="54f21-156">Obecnie, gdy użytkownik kończy formularz rejestracji, są rejestrowane.</span><span class="sxs-lookup"><span data-stu-id="54f21-156">Currently once a user completes the registration form, they are logged in.</span></span> <span data-ttu-id="54f21-157">Zazwyczaj mają Potwierdź swój adres e-mail przed ich zalogowaniem się.</span><span class="sxs-lookup"><span data-stu-id="54f21-157">You generally want to confirm their email before logging them in.</span></span> <span data-ttu-id="54f21-158">W poniższej sekcji firma Microsoft będzie zmodyfikuj kod do wymagają nowych użytkowników, aby miał potwierdzony adres e-mail, które są rejestrowane (uwierzytelniony).</span><span class="sxs-lookup"><span data-stu-id="54f21-158">In the section below, we will modify the code to require new users to have a confirmed email before they are logged in (authenticated).</span></span> <span data-ttu-id="54f21-159">Aktualizacja `HttpPost Register` metody z następującymi zmianami wyróżnione:</span><span class="sxs-lookup"><span data-stu-id="54f21-159">Update the `HttpPost Register` method with the following highlighted changes:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample7.cs?highlight=14-15,23-30)]

<span data-ttu-id="54f21-160">Przez komentowania limit `SignInAsync` metody, użytkownik nie będzie można zalogował się przy rejestracji.</span><span class="sxs-lookup"><span data-stu-id="54f21-160">By commenting out the `SignInAsync` method, the user will not be signed in by the registration.</span></span> <span data-ttu-id="54f21-161">`TempData["ViewBagLink"] = callbackUrl;` Wiersza mogą być używane do [debugowania aplikacji](#dbg) i testowanie rejestracji bez wysyłania wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="54f21-161">The `TempData["ViewBagLink"] = callbackUrl;` line can be used to [debug the app](#dbg) and test registration without sending email.</span></span> <span data-ttu-id="54f21-162">`ViewBag.Message` Służy do wyświetlania instrukcje Potwierdź.</span><span class="sxs-lookup"><span data-stu-id="54f21-162">`ViewBag.Message` is used to display the confirm instructions.</span></span> <span data-ttu-id="54f21-163">[Pobieranie próbki](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) zawiera kod, aby przetestować wiadomości e-mail z potwierdzeniem bez konfiguracji poczty e-mail i może również służyć do debugowania aplikacji.</span><span class="sxs-lookup"><span data-stu-id="54f21-163">The [download sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952) contains code to test email confirmation without setting up email, and can also be used to debug the application.</span></span>

<span data-ttu-id="54f21-164">Utwórz `Views\Shared\Info.cshtml` i Dodaj następujący kod razor:</span><span class="sxs-lookup"><span data-stu-id="54f21-164">Create a `Views\Shared\Info.cshtml` file and add the following razor markup:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample8.cshtml)]

<span data-ttu-id="54f21-165">Dodaj [atrybutu autoryzacji](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) do `Contact` metody akcji kontrolera głównej.</span><span class="sxs-lookup"><span data-stu-id="54f21-165">Add the [Authorize attribute](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.118).aspx) to the `Contact` action method of the Home controller.</span></span> <span data-ttu-id="54f21-166">Możesz kliknąć **skontaktuj się z** łącze, aby zweryfikować użytkownicy anonimowi nie mają dostępu oraz uwierzytelnieni użytkownicy mają dostęp.</span><span class="sxs-lookup"><span data-stu-id="54f21-166">You can click on the **Contact** link to verify anonymous users don't have access and authenticated users do have access.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample9.cs?highlight=1)]

<span data-ttu-id="54f21-167">Należy również zaktualizować `HttpPost Login` metody akcji:</span><span class="sxs-lookup"><span data-stu-id="54f21-167">You must also update the `HttpPost Login` action method:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample10.cs?highlight=13-22)]

<span data-ttu-id="54f21-168">Aktualizacja *Views\Shared\Error.cshtml* widok, aby wyświetlić komunikat o błędzie:</span><span class="sxs-lookup"><span data-stu-id="54f21-168">Update the *Views\Shared\Error.cshtml* view to display the error message:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample11.cshtml?highlight=8-17)]

<span data-ttu-id="54f21-169">Usuwanie wszystkich kont w **AspNetUsers** tabeli, która zawiera alias e-mail chcesz przeprowadzić test z.</span><span class="sxs-lookup"><span data-stu-id="54f21-169">Delete any accounts in the **AspNetUsers** table that contain the email alias you wish to test with.</span></span> <span data-ttu-id="54f21-170">Uruchom aplikację i sprawdzić, czy nie można zalogować się do momentu potwierdzenia adresu e-mail.</span><span class="sxs-lookup"><span data-stu-id="54f21-170">Run the app and verify you can't log in until you have confirmed your email address.</span></span> <span data-ttu-id="54f21-171">Po upewnieniu się, Twój adres e-mail, kliknij przycisk **skontaktuj się z** łącza.</span><span class="sxs-lookup"><span data-stu-id="54f21-171">Once you confirm your email address, click the **Contact** link.</span></span>

<a id="reset"></a>
## <a name="password-recoveryreset"></a><span data-ttu-id="54f21-172">Resetowanie odzyskiwania hasła</span><span class="sxs-lookup"><span data-stu-id="54f21-172">Password recovery/reset</span></span>

<span data-ttu-id="54f21-173">Usuń znaki komentarza z `HttpPost ForgotPassword` metody akcji w kontrolerze konta:</span><span class="sxs-lookup"><span data-stu-id="54f21-173">Remove the comment characters from the `HttpPost ForgotPassword` action method in the account controller:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample12.cs?highlight=17-20)]

<span data-ttu-id="54f21-174">Usuń znaki komentarza z `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) w *Views\Account\Login.cshtml* pliku widoku razor:</span><span class="sxs-lookup"><span data-stu-id="54f21-174">Remove the comment characters from the `ForgotPassword` [ActionLink](https://msdn.microsoft.com/library/system.web.mvc.html.linkextensions.actionlink(v=vs.118).aspx) in the *Views\Account\Login.cshtml* razor view file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample13.cshtml?highlight=47-50)]

<span data-ttu-id="54f21-175">Strony logowania będzie zawierać teraz link do resetowania hasła.</span><span class="sxs-lookup"><span data-stu-id="54f21-175">The Log in page will now show a link to reset the password.</span></span>

<a id="rsend"></a>
## <a name="resend-email-confirmation-link"></a><span data-ttu-id="54f21-176">Ponowne wysyłanie wiadomości e-mail potwierdzenie łącza</span><span class="sxs-lookup"><span data-stu-id="54f21-176">Resend email confirmation link</span></span>

<span data-ttu-id="54f21-177">Gdy użytkownik tworzy konto lokalne, są pocztą e-mail łącze potwierdzenia, które są wymagane do używania przed ich zalogowaniem się.</span><span class="sxs-lookup"><span data-stu-id="54f21-177">Once a user creates a new local account, they are emailed a confirmation link they are required to use before they can log on.</span></span> <span data-ttu-id="54f21-178">Jeśli użytkownik przypadkowo usuwa wiadomości e-mail z potwierdzeniem lub nigdy nie odebraniu wiadomości e-mail, muszą łącze potwierdzenie ponownego wysłania.</span><span class="sxs-lookup"><span data-stu-id="54f21-178">If the user accidently deletes the confirmation email, or the email never arrives, they will need the confirmation link sent again.</span></span> <span data-ttu-id="54f21-179">Poniższy kod zmienia pokazują, jak włączyć tę opcję.</span><span class="sxs-lookup"><span data-stu-id="54f21-179">The following code changes show how to enable this.</span></span>

<span data-ttu-id="54f21-180">Dodaj następującą metodę pomocnika do dołu *Controllers\AccountController.cs* pliku:</span><span class="sxs-lookup"><span data-stu-id="54f21-180">Add the following helper method to the bottom of the *Controllers\AccountController.cs* file:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample14.cs)]

<span data-ttu-id="54f21-181">Zaktualizuj metodę rejestru nowy Pomocnik:</span><span class="sxs-lookup"><span data-stu-id="54f21-181">Update the Register method to use the new helper:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample15.cs?highlight=17)]

<span data-ttu-id="54f21-182">Metoda logowania ponownie hasło, jeśli konto użytkownika nie został potwierdzony aktualizacji:</span><span class="sxs-lookup"><span data-stu-id="54f21-182">Update the Login method to resend the password if the user account has not been confirmed:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/samples/sample16.cs?highlight=20)]

<a id="combine"></a>
## <a name="combine-social-and-local-login-accounts"></a><span data-ttu-id="54f21-183">Łączenie kont społecznościowych i lokalne logowanie</span><span class="sxs-lookup"><span data-stu-id="54f21-183">Combine social and local login accounts</span></span>

<span data-ttu-id="54f21-184">Konta lokalne i społecznościowych można łączyć, klikając link do wiadomości e-mail.</span><span class="sxs-lookup"><span data-stu-id="54f21-184">You can combine local and social accounts by clicking on your email link.</span></span> <span data-ttu-id="54f21-185">W następującej kolejności **RickAndMSFT@gmail.com** najpierw zostanie utworzona jako logowania lokalnego, ale można utworzyć konta społecznościowych logowania w pierwszym, a następnie dodaj lokalny identyfikator logowania.</span><span class="sxs-lookup"><span data-stu-id="54f21-185">In the following sequence **RickAndMSFT@gmail.com** is first created as a local login, but you can create the account as a social log in first, then add a local login.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image5.png)

<span data-ttu-id="54f21-186">Polecenie **Zarządzaj** łącza.</span><span class="sxs-lookup"><span data-stu-id="54f21-186">Click on the **Manage** link.</span></span> <span data-ttu-id="54f21-187">Uwaga **logowań zewnętrznych: 0** skojarzone z tym kontem.</span><span class="sxs-lookup"><span data-stu-id="54f21-187">Note the **External Logins: 0** associated with this account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image6.png)

<span data-ttu-id="54f21-188">Kliknij łącze do innego dziennika w usłudze i akceptowania żądań aplikacji.</span><span class="sxs-lookup"><span data-stu-id="54f21-188">Click the link to another log in service and accept the app requests.</span></span> <span data-ttu-id="54f21-189">Dwa konta zostały połączone, będzie można logować się na każdym koncie.</span><span class="sxs-lookup"><span data-stu-id="54f21-189">The two accounts have been combined, you will be able to log on with either account.</span></span> <span data-ttu-id="54f21-190">Możesz użytkownikom dodawanie kont lokalnych, w przypadku ich społecznościowych dziennika w usłudze uwierzytelniania jest wyłączony lub najprawdopodobniej po utracie dostępu do swojego konta społecznościowych.</span><span class="sxs-lookup"><span data-stu-id="54f21-190">You might want your users to add local accounts in case their social log in authentication service is down, or more likely they have lost access to their social account.</span></span>

<span data-ttu-id="54f21-191">Na poniższej ilustracji, Tomasz jest społecznościowych logowania (które można wyświetlić z **logowań zewnętrznych: 1** wyświetlany na stronie).</span><span class="sxs-lookup"><span data-stu-id="54f21-191">In the following image, Tom is a social log in (which you can see from the **External Logins: 1** shown on the page).</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image7.png)

<span data-ttu-id="54f21-192">Kliknięcie **Wybierz hasło** umożliwia dodanie w lokalnym dzienniku na skojarzone z kontem.</span><span class="sxs-lookup"><span data-stu-id="54f21-192">Clicking on **Pick a password** allows you to add a local log on associated with the same account.</span></span>

![](create-an-aspnet-mvc-5-web-app-with-email-confirmation-and-password-reset/_static/image8.png)

## <a name="email-confirmation-in-more-depth"></a><span data-ttu-id="54f21-193">Wiadomości e-mail z potwierdzeniem bardziej szczegółowo</span><span class="sxs-lookup"><span data-stu-id="54f21-193">Email confirmation in more depth</span></span>

<span data-ttu-id="54f21-194">Moje samouczek [potwierdzania konta i hasło odzyskiwania za pomocą ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) przechodzi w tym temacie bardziej szczegółowo.</span><span class="sxs-lookup"><span data-stu-id="54f21-194">My tutorial [Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) goes into this topic with more details.</span></span>

<a id="dbg"></a>
## <a name="debugging-the-app"></a><span data-ttu-id="54f21-195">Debugowanie aplikacji</span><span class="sxs-lookup"><span data-stu-id="54f21-195">Debugging the app</span></span>

<span data-ttu-id="54f21-196">Jeśli nie otrzymasz wiadomość e-mail zawierającą łącze:</span><span class="sxs-lookup"><span data-stu-id="54f21-196">If you don't get an email containing the link:</span></span>

- <span data-ttu-id="54f21-197">Sprawdź folder wiadomości-śmieci lub spamu.</span><span class="sxs-lookup"><span data-stu-id="54f21-197">Check your junk or spam folder.</span></span>
- <span data-ttu-id="54f21-198">Zaloguj się na koncie SendGrid i kliknij na [działania pocztą E-mail łączy](https://sendgrid.com/logs/index).</span><span class="sxs-lookup"><span data-stu-id="54f21-198">Log into your SendGrid account and click on the [Email Activity link](https://sendgrid.com/logs/index).</span></span>

<span data-ttu-id="54f21-199">Aby przetestować łącze weryfikacji bez poczty e-mail, Pobierz [ukończone próbki](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span><span class="sxs-lookup"><span data-stu-id="54f21-199">To test the verification link without email, download the [completed sample](https://code.msdn.microsoft.com/MVC-5-with-2FA-email-8f26d952).</span></span> <span data-ttu-id="54f21-200">Na stronie pojawi się potwierdzenie link i kody potwierdzenia.</span><span class="sxs-lookup"><span data-stu-id="54f21-200">The confirmation link and confirmation codes will be displayed on the page.</span></span>

<a id="addRes"></a>
## <a name="additional-resources"></a><span data-ttu-id="54f21-201">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="54f21-201">Additional Resources</span></span>

- [<span data-ttu-id="54f21-202">Łącza do tożsamości ASP.NET zalecane zasobów</span><span class="sxs-lookup"><span data-stu-id="54f21-202">Links to ASP.NET Identity Recommended Resources</span></span>](../../../identity/overview/getting-started/aspnet-identity-recommended-resources.md)
- <span data-ttu-id="54f21-203">[Konta potwierdzenie i hasło odzyskiwania za pomocą ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) przechodzi w bardziej szczegółowo na potwierdzenie hasła odzyskiwania i konta.</span><span class="sxs-lookup"><span data-stu-id="54f21-203">[Account Confirmation and Password Recovery with ASP.NET Identity](../../../identity/overview/features-api/account-confirmation-and-password-recovery-with-aspnet-identity.md) Goes into more detail on password recovery and account confirmation.</span></span>
- <span data-ttu-id="54f21-204">[Aplikacji MVC 5 za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) w tym samouczku przedstawiono sposób pisania aplikacji platformy ASP.NET MVC 5 z usługi Facebook i Google OAuth 2 autoryzacji.</span><span class="sxs-lookup"><span data-stu-id="54f21-204">[MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md) This tutorial shows you how to write an ASP.NET MVC 5 app with Facebook and Google OAuth 2 authorization.</span></span> <span data-ttu-id="54f21-205">Ponadto jak dodać dodatkowe dane do bazy danych tożsamości.</span><span class="sxs-lookup"><span data-stu-id="54f21-205">It also shows how to add additional data to the Identity database.</span></span>
- <span data-ttu-id="54f21-206">[Wdrażanie aplikacji bezpiecznego platformy ASP.NET MVC z członkostwa, OAuth i bazy danych SQL Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="54f21-206">[Deploy a Secure ASP.NET MVC app with Membership, OAuth, and SQL Database to Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="54f21-207">W tym samouczku dodaje wdrożenia usługi Azure zabezpieczania aplikacji za pomocą ról, jak używać interfejs API członkostwa można dodać użytkowników i role oraz dodatkowe funkcje zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="54f21-207">This tutorial adds Azure deployment, how to secure your app with roles, how to use the membership API to add users and roles, and additional security features.</span></span>
- [<span data-ttu-id="54f21-208">Tworzenie aplikacji Google OAuth 2 i łączenie aplikacji z projektu</span><span class="sxs-lookup"><span data-stu-id="54f21-208">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#goog)
- [<span data-ttu-id="54f21-209">Tworzenie aplikacji w serwisie Facebook i łączenie aplikacji z projektu</span><span class="sxs-lookup"><span data-stu-id="54f21-209">Creating the app in Facebook and connecting the app to the project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#fb)
- [<span data-ttu-id="54f21-210">Konfigurowanie protokołu SSL w projekcie</span><span class="sxs-lookup"><span data-stu-id="54f21-210">Setting up SSL in the Project</span></span>](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md#ssl)
