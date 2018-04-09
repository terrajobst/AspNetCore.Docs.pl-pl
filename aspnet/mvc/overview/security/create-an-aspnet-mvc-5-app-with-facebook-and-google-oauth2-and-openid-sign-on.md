---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Utwórz MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Ten samouczek pokazuje, jak utworzyć aplikację sieci web platformy ASP.NET MVC 5 umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 z poświadczeń authenti zewnętrznych...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: c289c209b50f0c2c1f2d8b15a3aedeaebf671d0b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="04c81-103">Tworzenie aplikacji platformy ASP.NET MVC 5 z usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego (C#)</span><span class="sxs-lookup"><span data-stu-id="04c81-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="04c81-104">przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="04c81-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="04c81-105">W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 5, która umożliwia użytkownikom logowanie użyciu [OAuth 2.0](http://oauth.net/2/) przy użyciu poświadczeń z zewnętrznego dostawcę uwierzytelniania, takich jak Facebook, Twitter, LinkedIn, Microsoft lub Google.</span><span class="sxs-lookup"><span data-stu-id="04c81-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="04c81-106">Dla uproszczenia ten samouczek koncentruje się na temat pracy z poświadczeń z usługi Facebook i Google.</span><span class="sxs-lookup"><span data-stu-id="04c81-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="04c81-107">Włączanie te poświadczenia w witrynach sieci web zapewnia znaczących korzyści, ponieważ milionów użytkowników mają już konta z tych dostawców zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="04c81-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="04c81-108">Ci użytkownicy będą bardziej skłonni zalogowania się do witryny, jeśli nie mają do tworzenia nowego zestawu poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="04c81-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="04c81-109">Zobacz też [aplikacji ASP.NET MVC 5 z programu SMS i adres e-mail uwierzytelniania dwuskładnikowego](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="04c81-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="04c81-110">Samouczka przedstawiono również sposób dodawania danych profilu dla użytkownika oraz sposób dodawania ról za pomocą interfejsu API członkostwa.</span><span class="sxs-lookup"><span data-stu-id="04c81-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="04c81-111">W tym samouczku, została napisana przy [Rick Anderson](https://blogs.msdn.com/rickAndy) (wykonaj mnie w serwisie Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="04c81-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="04c81-112">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="04c81-112">Getting Started</span></span>

<span data-ttu-id="04c81-113">Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="04c81-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="04c81-114">Zainstaluj program Visual Studio [2013, aktualizacja 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="04c81-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="04c81-115">Aby uzyskać pomoc dotyczącą Dropbox, GitHub, Linkedin, Instagram, buforu, Salesforce, pary, Exchange stosu, Tripit, Twitch, Twitter, Yahoo! i więcej, zobacz [przykładowy projekt](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="04c81-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="04c81-116">Należy zainstalować program Visual Studio [2013, aktualizacja 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej, do korzystania z usługi Google OAuth 2 i debugowania lokalnie bez ostrzeżeń protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="04c81-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="04c81-117">Kliknij przycisk **nowy projekt** z **Start** strony, lub użyj menu i wybierz **pliku**, a następnie **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="04c81-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="04c81-118">Tworzenie pierwszej aplikacji</span><span class="sxs-lookup"><span data-stu-id="04c81-118">Creating Your First Application</span></span>

<span data-ttu-id="04c81-119">Kliknij przycisk **nowy projekt**, a następnie wybierz pozycję **Visual C#** po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="04c81-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="04c81-120">Nazwa projektu "MvcAuth", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="04c81-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="04c81-121">W **nowy projekt ASP.NET** okna dialogowego, kliknij przycisk **MVC**.</span><span class="sxs-lookup"><span data-stu-id="04c81-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="04c81-122">Jeśli uwierzytelnianie nie jest **indywidualnych kont użytkowników**, kliknij przycisk **Zmień uwierzytelnianie** i wybrać **indywidualnych kont użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="04c81-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="04c81-123">Sprawdzając **Hostuj w chmurze**, aplikacja będzie łatwo udostępniać na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="04c81-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="04c81-124">W przypadku wybrania **Hostuj w chmurze**, ukończyć konfigurowanie okna dialogowego.</span><span class="sxs-lookup"><span data-stu-id="04c81-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="04c81-125">Użycie narzędzia NuGet można zaktualizować do najnowszego oprogramowania pośredniczącego OWIN</span><span class="sxs-lookup"><span data-stu-id="04c81-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="04c81-126">Użyj Menedżera pakietów NuGet, aby zaktualizować [oprogramowanie pośredniczące OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="04c81-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="04c81-127">Wybierz **aktualizacje** w lewym menu.</span><span class="sxs-lookup"><span data-stu-id="04c81-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="04c81-128">Możesz kliknąć **Aktualizuj wszystkie** przycisk lub Wyszukaj tylko pakiety OWIN (wyświetlane w obrazie dalej):</span><span class="sxs-lookup"><span data-stu-id="04c81-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="04c81-129">Na poniższej ilustracji wyświetlane są tylko pakiety OWIN:</span><span class="sxs-lookup"><span data-stu-id="04c81-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="04c81-130">Z konsoli Menedżera pakietów (PMC), można wprowadzić `Update-Package` polecenia, które będą wszystkich pakietów aktualizacji.</span><span class="sxs-lookup"><span data-stu-id="04c81-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="04c81-131">Naciśnij klawisz **F5** lub **Ctrl + F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04c81-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="04c81-132">Na poniższej ilustracji numer portu to 1234.</span><span class="sxs-lookup"><span data-stu-id="04c81-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="04c81-133">Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="04c81-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="04c81-134">W zależności od rozmiaru okna przeglądarki, konieczne może być kliknij ikonę nawigacji, aby wyświetlić **Home**, **o**, **skontaktuj się z**, **zarejestrować**i **Zaloguj** łącza.</span><span class="sxs-lookup"><span data-stu-id="04c81-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="04c81-135">Konfigurowanie protokołu SSL w projekcie</span><span class="sxs-lookup"><span data-stu-id="04c81-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="04c81-136">Aby połączyć dostawców uwierzytelniania, takich jak Google i Facebook, należy skonfigurować usługi IIS Express do używania protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="04c81-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="04c81-137">Ważne jest, aby zachować po logowania przy użyciu protokołu SSL i nie spadku HTTP, Twoje plik cookie logowania tylko jako klucz tajny jako nazwę użytkownika i hasło, a następnie bez użycia protokołu SSL jest wysyłana w postaci zwykłego tekstu przez sieć.</span><span class="sxs-lookup"><span data-stu-id="04c81-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="04c81-138">Ponadto już wykonałeś czas wykonania uzgadniania i należy zabezpieczyć kanał (czyli większość co sprawia, że HTTPS wolniej niż HTTP) przed uruchomieniem potok MVC, więc przekierowywanie z powrotem do HTTP po zalogowano nie należy bieżącego żądania lub przyszłe żądania jest znacznie szybsze.</span><span class="sxs-lookup"><span data-stu-id="04c81-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="04c81-139">W **Eksploratora rozwiązań**, kliknij przycisk **MvcAuth** projektu.</span><span class="sxs-lookup"><span data-stu-id="04c81-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="04c81-140">Naciśnięcie klawisza F4, aby wyświetlić właściwości projektu.</span><span class="sxs-lookup"><span data-stu-id="04c81-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="04c81-141">Alternatywnie z **widoku** menu można wybrać **okna właściwości**.</span><span class="sxs-lookup"><span data-stu-id="04c81-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="04c81-142">Zmień **SSL włączone** na wartość True.</span><span class="sxs-lookup"><span data-stu-id="04c81-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="04c81-143">Skopiuj adres URL protokołu SSL (który będzie `https://localhost:44300/` chyba, że po utworzeniu innych projektów SSL).</span><span class="sxs-lookup"><span data-stu-id="04c81-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="04c81-144">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **MvcAuth** projekt i wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="04c81-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="04c81-145">Wybierz **Web** karcie, a następnie wklej adres URL protokołu SSL do **adres Url projektu** pole.</span><span class="sxs-lookup"><span data-stu-id="04c81-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="04c81-146">Zapisz plik (listy Ctl + S).</span><span class="sxs-lookup"><span data-stu-id="04c81-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="04c81-147">Należy ten adres URL do skonfigurowania aplikacji uwierzytelniania usługi Facebook i Google.</span><span class="sxs-lookup"><span data-stu-id="04c81-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="04c81-148">Dodaj [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atrybutu `Home` kontrolera, aby wymagać wszystkie żądania muszą używać protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="04c81-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="04c81-149">Bardziej bezpieczną metodą jest dodanie [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtr do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04c81-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="04c81-150">Zobacz sekcję &quot;chronić aplikacje z protokołu SSL i autoryzować atrybutu&quot; w mojej tutoral [tworzenie aplikacji ASP.NET MVC z uwierzytelniania i bazy danych SQL i wdrożyć w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="04c81-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="04c81-151">Poniżej przedstawiono część głównej kontrolera.</span><span class="sxs-lookup"><span data-stu-id="04c81-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="04c81-152">Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="04c81-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="04c81-153">Jeśli certyfikat został zainstalowany w przeszłości, możesz pominąć pozostałej części tej sekcji i przejść do [tworzenie aplikacji Google OAuth 2 i łączenie aplikacji z projektu](#goog), w przeciwnym razie wartość postępuj zgodnie z instrukcjami, aby zaufać podpisem certyfikat, który wygenerował usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="04c81-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="04c81-154">Odczyt **ostrzeżenie o zabezpieczeniach** okna dialogowego, a następnie kliknij przycisk **tak** Jeśli chcesz zainstalować certyfikat reprezentujący localhost.</span><span class="sxs-lookup"><span data-stu-id="04c81-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="04c81-155">Pokazuje IE *Home* strony i nie ma żadnych ostrzeżeń protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="04c81-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="04c81-156">Google Chrome również zaakceptuje certyfikat i wyświetli zawartości HTTPS bez ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="04c81-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="04c81-157">Firefox używa magazynu certyfikatów, więc zostanie wyświetlone ostrzeżenie o.</span><span class="sxs-lookup"><span data-stu-id="04c81-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="04c81-158">Dla aplikacji możesz kliknąć **rozumiem ryzyko**.</span><span class="sxs-lookup"><span data-stu-id="04c81-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="04c81-159">Tworzenie aplikacji Google OAuth 2 i łączenie aplikacji z projektu</span><span class="sxs-lookup"><span data-stu-id="04c81-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="04c81-160">Bieżący protokołu Google OAuth instrukcje można znaleźć [Konfigurowanie uwierzytelniania serwisu Google w ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="04c81-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="04c81-161">Przejdź do [konsoli deweloperów Google](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="04c81-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="04c81-162">Jeśli nie utworzono projekt przed, wybierz **poświadczenia** w karcie po lewej stronie, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="04c81-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="04c81-163">Na karcie po lewej stronie kliknij **poświadczenia**.</span><span class="sxs-lookup"><span data-stu-id="04c81-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="04c81-164">Kliknij przycisk **Utwórz poświadczenia** następnie **identyfikator klienta OAuth**.</span><span class="sxs-lookup"><span data-stu-id="04c81-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="04c81-165">W **utworzyć identyfikator klienta** okna dialogowego, zachowaj ustawienie domyślne **aplikacji sieci Web** typu aplikacja.</span><span class="sxs-lookup"><span data-stu-id="04c81-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="04c81-166">Ustaw **autoryzowany JavaScript** źródła do adresu URL protokołu SSL używany powyżej (`https://localhost:44300/` chyba, że po utworzeniu innych projektów SSL)</span><span class="sxs-lookup"><span data-stu-id="04c81-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="04c81-167">Ustaw **identyfikator URI przekierowania autoryzowanych** do:</span><span class="sxs-lookup"><span data-stu-id="04c81-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="04c81-168">Kliknij element menu zgoda OAuth ekranu, a następnie ustaw nazwę adres i produktu poczty e-mail.</span><span class="sxs-lookup"><span data-stu-id="04c81-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="04c81-169">Po zakończeniu kliknij formularz **zapisać**.</span><span class="sxs-lookup"><span data-stu-id="04c81-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="04c81-170">Kliknij element menu biblioteki, wyszukaj **interfejsu API Google +**, kliknij go, a następnie naciśnij klawisz Włącz.</span><span class="sxs-lookup"><span data-stu-id="04c81-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="04c81-171">Na poniższym obrazie pokazano włączone interfejsy API.</span><span class="sxs-lookup"><span data-stu-id="04c81-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="04c81-172">Odwiedź stronę z Google APIs interfejsu API menedżera, **poświadczenia** kartę, aby uzyskać **identyfikator klienta**.</span><span class="sxs-lookup"><span data-stu-id="04c81-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="04c81-173">Pobierz, aby zapisać plik JSON z haseł aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04c81-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="04c81-174">Skopiuj i Wklej **ClientId** i **ClientSecret** do `UseGoogleAuthentication` znaleziono metody w *Startup.Auth.cs* w pliku *App_Start* folderu.</span><span class="sxs-lookup"><span data-stu-id="04c81-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="04c81-175">**ClientId** i **ClientSecret** poniższe wartości są przykłady i nie działają.</span><span class="sxs-lookup"><span data-stu-id="04c81-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="04c81-176">Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="04c81-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="04c81-177">Konta i poświadczenia zostaną dodane do powyżej, aby zachować prosty przykład kodu.</span><span class="sxs-lookup"><span data-stu-id="04c81-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="04c81-178">Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="04c81-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="04c81-179">Naciśnij klawisz **CTRL + F5** Aby skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="04c81-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="04c81-180">Kliknij przycisk **Zaloguj** łącza.</span><span class="sxs-lookup"><span data-stu-id="04c81-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="04c81-181">W obszarze **Zaloguj się za pomocą innej usługi**, kliknij przycisk **Google**.</span><span class="sxs-lookup"><span data-stu-id="04c81-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="04c81-182">Jeśli pominiesz dowolne z opisanych wyżej wystąpi błąd HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="04c81-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="04c81-183">Ponownie sprawdź powyższe kroki.</span><span class="sxs-lookup"><span data-stu-id="04c81-183">Recheck your steps above.</span></span> <span data-ttu-id="04c81-184">Jeśli pominiesz wymaganego ustawienia (na przykład **nazwa produktu**), Dodaj brakujący element i zapisać; może potrwać kilka minut dla działania uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="04c81-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="04c81-185">Nastąpi przekierowanie do witryny Google, w którym należy wprowadzić poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="04c81-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="04c81-186">Po wprowadź swoje poświadczenia, pojawi się monit, aby udzielić uprawnień do nowo utworzonej aplikacji sieci web:</span><span class="sxs-lookup"><span data-stu-id="04c81-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="04c81-187">Kliknij przycisk **zaakceptować**.</span><span class="sxs-lookup"><span data-stu-id="04c81-187">Click **Accept**.</span></span> <span data-ttu-id="04c81-188">Teraz nastąpi przekierowanie do **zarejestrować** strony aplikacji MvcAuth, w którym można zarejestrować konto Google.</span><span class="sxs-lookup"><span data-stu-id="04c81-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="04c81-189">Istnieje możliwość zmiany nazwy rejestracji lokalnej poczty e-mail używany dla tego konta usługi Gmail, ale zazwyczaj chcesz pozostawić domyślny alias e-mail (to znaczy aktualnie używany do uwierzytelniania).</span><span class="sxs-lookup"><span data-stu-id="04c81-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="04c81-190">Kliknij przycisk **zarejestrować**.</span><span class="sxs-lookup"><span data-stu-id="04c81-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="04c81-191">Tworzenie aplikacji w serwisie Facebook i łączenie aplikacji z projektu</span><span class="sxs-lookup"><span data-stu-id="04c81-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="04c81-192">Bieżący Facebook OAuth2 uwierzytelniania instrukcje można znaleźć [uwierzytelniania Konfigurowanie usługi Facebook](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="04c81-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="04c81-193">Dla uwierzytelniania serwisu Facebook OAuth2 należy skopiować do projektu niektórych ustawień z aplikacji, która tworzenia w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="04c81-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="04c81-194">W przeglądarce przejdź do [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) i zaloguj ponownie wprowadzić swoje poświadczenia usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="04c81-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="04c81-195">Jeśli nie są już zarejestrowane jako deweloper usługi Facebook, kliknij przycisk **Zarejestruj się jako deweloper** i postępuj zgodnie z instrukcjami, aby zarejestrować.</span><span class="sxs-lookup"><span data-stu-id="04c81-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="04c81-196">Na **aplikacje** , kliknij pozycję **Utwórz nową aplikację**.</span><span class="sxs-lookup"><span data-stu-id="04c81-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![Utwórz nową aplikację](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="04c81-198">Wprowadź **Nazwa aplikacji** i **kategorii**, następnie kliknij przycisk **tworzenie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="04c81-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="04c81-199">To musi być unikatowa w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="04c81-199">This must be unique across Facebook.</span></span> <span data-ttu-id="04c81-200"><strong>Namespace aplikacji</strong> to część adresu URL, które Twoja aplikacja będzie używać do dostępu do aplikacji usługi Facebook dla uwierzytelniania (na przykład https://apps.facebook.com/{App Namespace}).</span><span class="sxs-lookup"><span data-stu-id="04c81-200">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="04c81-201">Jeśli nie określisz <strong>Namespace aplikacji</strong>, <strong>identyfikator aplikacji</strong> będą używane dla adresu URL.</span><span class="sxs-lookup"><span data-stu-id="04c81-201">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="04c81-202"><strong>Identyfikator aplikacji</strong> jest liczbą długo generowanych przez system, która będzie widoczna w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="04c81-202">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![Utwórz nową aplikację w oknie dialogowym](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="04c81-204">Przedstawia wyboru standardowych zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="04c81-204">Submit the standard security check.</span></span>

    ![Kontrola zabezpieczeń](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="04c81-206">Wybierz **ustawienia** na pasku menu po lewej stronie![ Pasek menu dewelopera usługi Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="04c81-206">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="04c81-207">Na **podstawowe** sekcja ustawienia na stronie wybierz **dodać platformy** do określenia, że w przypadku dodawania aplikacji witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="04c81-207">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="04c81-208">![Ustawienia podstawowe](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="04c81-208">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="04c81-209">Wybierz **witryny sieci Web** spośród dostępnych platform.</span><span class="sxs-lookup"><span data-stu-id="04c81-209">Select **Website** from the platform choices.</span></span>  
  
    ![Wybór platformy](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="04c81-211">Zanotuj Twojej **identyfikator aplikacji** i **klucz tajny aplikacji** , w którym można dodawać zarówno do aplikacji MVC w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="04c81-211">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="04c81-212">Ponadto Dodaj adres URL witryny (`https://localhost:44300/`) do testowania aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="04c81-212">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="04c81-213">Ponadto Dodaj **E-mail kontaktu**.</span><span class="sxs-lookup"><span data-stu-id="04c81-213">Also, add a **Contact Email**.</span></span> <span data-ttu-id="04c81-214">Następnie wybierz opcję **Zapisz zmiany**.</span><span class="sxs-lookup"><span data-stu-id="04c81-214">Then, select **Save Changes**.</span></span>   

    ![Strony szczegółów aplikacji w warstwie podstawowa](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="04c81-216">Należy zauważyć, że tylko będzie można przeprowadzić uwierzytelniania za pomocą aliasu poczty e-mail, który został zarejestrowany.</span><span class="sxs-lookup"><span data-stu-id="04c81-216">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="04c81-217">Inni użytkownicy i testowe konta nie będzie mógł się zarejestrować.</span><span class="sxs-lookup"><span data-stu-id="04c81-217">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="04c81-218">Można przyznać dostęp kont innych Facebook do aplikacji w usłudze Facebook **ról Developer** kartę.</span><span class="sxs-lookup"><span data-stu-id="04c81-218">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="04c81-219">W programie Visual Studio Otwórz *aplikacji\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="04c81-219">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="04c81-220">Skopiuj i Wklej **AppId** i **klucz tajny aplikacji** do `UseFacebookAuthentication` metody.</span><span class="sxs-lookup"><span data-stu-id="04c81-220">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="04c81-221">**AppId** i **klucz tajny aplikacji** poniższe wartości są przykłady i nie będzie działać.</span><span class="sxs-lookup"><span data-stu-id="04c81-221">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="04c81-222">Kliknij przycisk **zapisać zmiany**.</span><span class="sxs-lookup"><span data-stu-id="04c81-222">Click **Save Changes**.</span></span>
13. <span data-ttu-id="04c81-223">Naciśnij klawisz **CTRL + F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04c81-223">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="04c81-224">Wybierz **Zaloguj** do wyświetlenia strony logowania.</span><span class="sxs-lookup"><span data-stu-id="04c81-224">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="04c81-225">Kliknij przycisk **Facebook** w obszarze **Zaloguj się za pomocą innej usługi.**</span><span class="sxs-lookup"><span data-stu-id="04c81-225">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="04c81-226">Wprowadź swoje poświadczenia usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="04c81-226">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="04c81-227">Pojawi się monit o udzielenie uprawnień do dostępu do profilu publicznego i friend listy aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04c81-227">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Szczegóły aplikacji usługi Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="04c81-229">Użytkownik jest obecnie zalogowany.</span><span class="sxs-lookup"><span data-stu-id="04c81-229">You are now logged in.</span></span> <span data-ttu-id="04c81-230">Możesz teraz zarejestrować tego konta w aplikacji.</span><span class="sxs-lookup"><span data-stu-id="04c81-230">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="04c81-231">Podczas rejestrowania, wpis jest dodawany do *użytkowników* tabeli członkostwa bazy danych.</span><span class="sxs-lookup"><span data-stu-id="04c81-231">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="04c81-232">Sprawdzanie danych członkostwa</span><span class="sxs-lookup"><span data-stu-id="04c81-232">Examine the Membership Data</span></span>

<span data-ttu-id="04c81-233">W **widoku** menu, kliknij przycisk **Eksploratora serwera**.</span><span class="sxs-lookup"><span data-stu-id="04c81-233">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="04c81-234">Rozwiń węzeł **parametru DefaultConnection (MvcAuth)**, rozwiń węzeł **tabel**, kliknij prawym przyciskiem myszy **AspNetUsers** i kliknij przycisk **Pokaż dane tabeli**.</span><span class="sxs-lookup"><span data-stu-id="04c81-234">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dane tabeli aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="04c81-236">Dodawanie danych profilu do klasy użytkownika</span><span class="sxs-lookup"><span data-stu-id="04c81-236">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="04c81-237">W tej sekcji zostanie dodana daty urodzenia i miejscowość macierzystego do danych użytkownika podczas rejestracji, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="04c81-237">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![zwrócona z macierzystego miejscowość i Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="04c81-239">Otwórz *Models\IdentityModels.cs* plik i dodać właściwości miejscowość daty i w domu urodzenia:</span><span class="sxs-lookup"><span data-stu-id="04c81-239">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="04c81-240">Otwórz *Models\AccountViewModels.cs* plików i zestaw urodzenia daty i w domu właściwości Miejscowość w `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="04c81-240">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="04c81-241">Otwórz *Controllers\AccountController.cs* pliku i Dodaj kod dla daty i w domu mieście urodzenia w `ExternalLoginConfirmation` metody akcji, jak pokazano:</span><span class="sxs-lookup"><span data-stu-id="04c81-241">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="04c81-242">Dodaj daty urodzenia i macierzystego mieście *Views\Account\ExternalLoginConfirmation.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="04c81-242">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="04c81-243">Aby można było ponownie zarejestrować konto usługi Facebook z aplikacją i sprawdź, czy można dodać nowego Data urodzenia i informacje o profilu miejscowość macierzystego, należy usunąć bazy danych członkostwa.</span><span class="sxs-lookup"><span data-stu-id="04c81-243">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="04c81-244">Z **Eksploratora rozwiązań**, kliknij przycisk **Pokaż wszystkie pliki** ikonę, a następnie kliknij prawym przyciskiem myszy *Dodaj\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* i kliknij przycisk **usunąć**.</span><span class="sxs-lookup"><span data-stu-id="04c81-244">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="04c81-245">Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów NuGet**, następnie kliknij przycisk **Konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="04c81-245">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="04c81-246">Wpisz następujące polecenia w kryterium.</span><span class="sxs-lookup"><span data-stu-id="04c81-246">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="04c81-247">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="04c81-247">Enable-Migrations</span></span>
2. <span data-ttu-id="04c81-248">Init dodać migracji</span><span class="sxs-lookup"><span data-stu-id="04c81-248">Add-Migration Init</span></span>
3. <span data-ttu-id="04c81-249">Update-Database</span><span class="sxs-lookup"><span data-stu-id="04c81-249">Update-Database</span></span>

<span data-ttu-id="04c81-250">Uruchom aplikację i zaloguj się i rejestrować w przypadku niektórych użytkowników za pomocą usługi FaceBook i Google.</span><span class="sxs-lookup"><span data-stu-id="04c81-250">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="04c81-251">Sprawdzanie danych członkostwa</span><span class="sxs-lookup"><span data-stu-id="04c81-251">Examine the Membership Data</span></span>

<span data-ttu-id="04c81-252">W **widoku** menu, kliknij przycisk **Eksploratora serwera**.</span><span class="sxs-lookup"><span data-stu-id="04c81-252">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="04c81-253">Kliknij prawym przyciskiem myszy **AspNetUsers** i kliknij przycisk **Pokaż dane tabeli**.</span><span class="sxs-lookup"><span data-stu-id="04c81-253">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="04c81-254">`HomeTown` i `BirthDate` pola są wyświetlane poniżej.</span><span class="sxs-lookup"><span data-stu-id="04c81-254">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="04c81-255">Rejestrowanie aplikacji i logowania za pomocą innego konta</span><span class="sxs-lookup"><span data-stu-id="04c81-255">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="04c81-256">Jeśli logowania do aplikacji z usługą Facebook, a następnie wyloguj się i spróbuj zalogować się ponownie za pomocą innego konta usługi Facebook (przy użyciu tej samej przeglądarce), użytkownik będzie można natychmiast zalogowany do poprzedniego konta usługi Facebook, używanego.</span><span class="sxs-lookup"><span data-stu-id="04c81-256">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="04c81-257">Aby użyć innego konta, należy przejść do usługi Facebook i wylogowania w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="04c81-257">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="04c81-258">Tę samą zasadę stosuje się do żadnych innych 3 strona dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="04c81-258">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="04c81-259">Można też, zaloguj się za pomocą innego konta, przy użyciu innej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="04c81-259">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="04c81-260">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="04c81-260">Next Steps</span></span>

<span data-ttu-id="04c81-261">Zobacz [wprowadzenie dostawcy zabezpieczeń Yahoo i LinkedIn OAuth dla OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) przez Jerrie Pelser instrukcje Yahoo i LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="04c81-261">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="04c81-262">Zobacz na Jerrie Pretty przycisków społecznościowych logowania dla platformy ASP.NET MVC 5 uzyskanie Włączanie logowania społecznościowych przycisków.</span><span class="sxs-lookup"><span data-stu-id="04c81-262">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="04c81-263">Wykonaj czynności opisane w samouczku Mój [tworzenie aplikacji ASP.NET MVC z uwierzytelniania i bazy danych SQL i wdrożyć w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), które nadal tego samouczka i zawiera następujące:</span><span class="sxs-lookup"><span data-stu-id="04c81-263">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="04c81-264">Jak wdrożyć aplikację na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="04c81-264">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="04c81-265">Jak zabezpieczyć użytkownik aplikacji z rolami.</span><span class="sxs-lookup"><span data-stu-id="04c81-265">How to secure you app with roles.</span></span>
3. <span data-ttu-id="04c81-266">Jak zabezpieczyć aplikacji za pomocą [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) i [autoryzacji](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtrów.</span><span class="sxs-lookup"><span data-stu-id="04c81-266">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="04c81-267">Jak używać interfejs API członkostwa, aby dodać użytkowników i ról.</span><span class="sxs-lookup"><span data-stu-id="04c81-267">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="04c81-268">Wystaw opinię na jak zbędne tego samouczka i co można możemy ulepszyć.</span><span class="sxs-lookup"><span data-stu-id="04c81-268">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="04c81-269">Możesz również poprosić o nowe tematy w [Pokaż mnie jak z kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="04c81-269">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="04c81-270">Można nawet uzyskać oraz oddawać głosy na nowe funkcje do dodania do programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="04c81-270">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="04c81-271">Na przykład można głosować narzędzia do [tworzenie i zarządzanie użytkownikami i rolami.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="04c81-271">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="04c81-272">Dobry opis działania zewnętrznych usług uwierzytelniania ASP.NET, zobacz Roberta Mcmurraya [zewnętrznych usług uwierzytelniania](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="04c81-272">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="04c81-273">Artykuł Roberta również przechodzi w stan szczegółów w włączania uwierzytelniania firmy Microsoft i Twitter.</span><span class="sxs-lookup"><span data-stu-id="04c81-273">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="04c81-274">Tomasz Dykstra obiektu znakomity [samouczek EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) pokazano, jak pracować z programu Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="04c81-274">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
