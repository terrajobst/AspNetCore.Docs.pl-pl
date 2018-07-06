---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Utwórz MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku dowiesz się, jak utworzyć aplikację sieci web ASP.NET MVC 5, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z zewnętrznego typ uwier...
ms.author: aspnetcontent
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 6af4990f726bfcd0c45eb6991418661f9b8ccbf6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824709"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="6e224-103">Tworzenie aplikacji ASP.NET MVC 5 z usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego (C#)</span><span class="sxs-lookup"><span data-stu-id="6e224-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="6e224-104">Przez [Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="6e224-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="6e224-105">W tym samouczku dowiesz się, jak utworzyć aplikację sieci web ASP.NET MVC 5, która pozwala użytkownikom na logowanie za pomocą [OAuth 2.0](http://oauth.net/2/) przy użyciu poświadczeń z zewnętrznego dostawcę uwierzytelniania, takich jak Facebook, Twitter, LinkedIn, Microsoft lub Google.</span><span class="sxs-lookup"><span data-stu-id="6e224-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="6e224-106">Dla uproszczenia ten samouczek koncentruje się na pracy przy użyciu poświadczeń usług Facebook i Google.</span><span class="sxs-lookup"><span data-stu-id="6e224-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="6e224-107">Włączanie tych poświadczeń w witrynach sieci web zapewnia znaczące korzyści, ponieważ milionom użytkowników mają już konta z tych dostawców zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="6e224-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="6e224-108">Tacy użytkownicy mogą być bardziej skłonni się zarejestrować dla danej witryny, gdy ta osoba nie ma do tworzenia i Zapamiętaj nowego zestawu poświadczeń.</span><span class="sxs-lookup"><span data-stu-id="6e224-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="6e224-109">Zobacz też [aplikacji ASP.NET MVC 5 z wiadomości SMS i wiadomości e-mail uwierzytelniania dwuskładnikowego](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="6e224-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="6e224-110">W samouczku opisano również sposób dodać dane profilowe dla użytkownika oraz Dodawanie ról za pomocą interfejsu API członkostwa.</span><span class="sxs-lookup"><span data-stu-id="6e224-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="6e224-111">Ten samouczek został napisany przez [Rick Anderson](https://blogs.msdn.com/rickAndy) (postępuj zgodnie ze mną w serwisie Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).</span><span class="sxs-lookup"><span data-stu-id="6e224-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="6e224-112">Wprowadzenie</span><span class="sxs-lookup"><span data-stu-id="6e224-112">Getting Started</span></span>

<span data-ttu-id="6e224-113">Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span><span class="sxs-lookup"><span data-stu-id="6e224-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="6e224-114">Zainstaluj program Visual Studio [2013 z aktualizacją 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej.</span><span class="sxs-lookup"><span data-stu-id="6e224-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="6e224-115">Aby uzyskać pomoc dotyczącą usługi Dropbox, GitHub, Linkedin, Instagram, buforu, Salesforce, pary, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! i więcej, zobacz ten [przykładowy projekt](https://github.com/matthewdunsdon/oauthforaspnet).</span><span class="sxs-lookup"><span data-stu-id="6e224-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="6e224-116">Należy zainstalować program Visual Studio [2013 z aktualizacją 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej, do użycia Google protokołu OAuth 2 i Debuguj lokalnie, bez ostrzeżeń protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="6e224-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="6e224-117">Kliknij przycisk **nowy projekt** z **Start** strony, lub można użyć menu i wybrać **pliku**, a następnie **nowy projekt**.</span><span class="sxs-lookup"><span data-stu-id="6e224-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="6e224-118">Tworzenie pierwszej aplikacji</span><span class="sxs-lookup"><span data-stu-id="6e224-118">Creating Your First Application</span></span>

<span data-ttu-id="6e224-119">Kliknij przycisk **nowy projekt**, a następnie wybierz **Visual C#** po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web ASP.NET**.</span><span class="sxs-lookup"><span data-stu-id="6e224-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="6e224-120">Nazwij swój projekt "MvcAuth", a następnie kliknij przycisk **OK**.</span><span class="sxs-lookup"><span data-stu-id="6e224-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="6e224-121">W **nowy projekt ASP.NET** okno dialogowe, kliknij przycisk **MVC**.</span><span class="sxs-lookup"><span data-stu-id="6e224-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="6e224-122">Jeśli uwierzytelnianie nie jest **indywidualne konta użytkowników**, kliknij przycisk **Zmień uwierzytelnianie** i wybrać **indywidualne konta użytkowników**.</span><span class="sxs-lookup"><span data-stu-id="6e224-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="6e224-123">Sprawdzając **Hostuj w chmurze**, aplikacja będzie hostować na platformie Azure jest bardzo proste.</span><span class="sxs-lookup"><span data-stu-id="6e224-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="6e224-124">W przypadku wybrania **Hostuj w chmurze**, wykonaj okna dialogowego Konfigurowanie.</span><span class="sxs-lookup"><span data-stu-id="6e224-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="6e224-125">Użyj pakietu NuGet, aby zaktualizować do najnowsze oprogramowanie pośredniczące OWIN</span><span class="sxs-lookup"><span data-stu-id="6e224-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="6e224-126">Użyj Menedżera pakietów NuGet, aby zaktualizować [oprogramowania pośredniczącego OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span><span class="sxs-lookup"><span data-stu-id="6e224-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="6e224-127">Wybierz **aktualizacje** w menu po lewej stronie.</span><span class="sxs-lookup"><span data-stu-id="6e224-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="6e224-128">Możesz kliknąć **Aktualizuj wszystkie** przycisku, lub możesz wyszukiwać tylko pakiety OWIN (pokazane na następnej ilustracji):</span><span class="sxs-lookup"><span data-stu-id="6e224-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="6e224-129">Na poniższej ilustracji przedstawiono tylko OWIN pakiety:</span><span class="sxs-lookup"><span data-stu-id="6e224-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="6e224-130">W konsoli Menedżera pakietów (PMC), możesz wprowadzić `Update-Package` polecenia, które spowoduje zaktualizowanie wszystkich pakietów.</span><span class="sxs-lookup"><span data-stu-id="6e224-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="6e224-131">Naciśnij klawisz **F5** lub **kombinację klawiszy Ctrl + F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e224-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="6e224-132">Na poniższej ilustracji numer portu to 1234.</span><span class="sxs-lookup"><span data-stu-id="6e224-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="6e224-133">Po uruchomieniu aplikacji, zobaczysz inny numer portu.</span><span class="sxs-lookup"><span data-stu-id="6e224-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="6e224-134">W zależności od rozmiaru okna przeglądarki, konieczne może być kliknij ikonę nawigacji, aby zobaczyć **Home**, **o**, **skontaktuj się z pomocą**, **zarejestrować**i **Zaloguj** łącza.</span><span class="sxs-lookup"><span data-stu-id="6e224-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="6e224-135">Konfigurowanie protokołu SSL w projekcie</span><span class="sxs-lookup"><span data-stu-id="6e224-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="6e224-136">Aby połączyć z dostawców uwierzytelniania, takich jak serwis Google czy Facebook, należy skonfigurować usługi IIS Express do używania protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="6e224-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="6e224-137">Jest ważne zachować, po zalogowaniu przy użyciu protokołu SSL i nie porzucić powrót do protokołu HTTP, Twoje plik cookie logowania jest po prostu jako klucz tajny, jako nazwę użytkownika i hasło, a także bez korzystania z protokołu SSL jest wysyłana w postaci zwykłego tekstu w sieci.</span><span class="sxs-lookup"><span data-stu-id="6e224-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="6e224-138">Poza już wykonanych podczas wykonywania uzgadnianie i zabezpieczenia kanału (jest to duża część co sprawia, że wolniej niż HTTP na HTTPS) zanim potok MVC jest uruchamiany, więc przekierowywanie do protokołu HTTP, po użytkownik zalogował się nie będzie wprowadzenia, bieżącego żądania lub przyszłość żądania znacznie szybciej.</span><span class="sxs-lookup"><span data-stu-id="6e224-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="6e224-139">W **Eksploratora rozwiązań**, kliknij przycisk **MvcAuth** projektu.</span><span class="sxs-lookup"><span data-stu-id="6e224-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="6e224-140">Naciśnij klawisz F4, aby wyświetlić właściwości projektu.</span><span class="sxs-lookup"><span data-stu-id="6e224-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="6e224-141">Alternatywnie z **widoku** menu, możesz wybrać **okno właściwości**.</span><span class="sxs-lookup"><span data-stu-id="6e224-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="6e224-142">Zmiana **włączony protokół SSL** na wartość True.</span><span class="sxs-lookup"><span data-stu-id="6e224-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="6e224-143">Skopiuj adres URL protokołu SSL (który będzie stanowić `https://localhost:44300/` chyba, że utworzono inne projekty, protokołu SSL).</span><span class="sxs-lookup"><span data-stu-id="6e224-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="6e224-144">W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **MvcAuth** projektu, a następnie wybierz **właściwości**.</span><span class="sxs-lookup"><span data-stu-id="6e224-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="6e224-145">Wybierz **Web** kartę, a następnie wklej adres URL protokołu SSL do **adres Url projektu** pole.</span><span class="sxs-lookup"><span data-stu-id="6e224-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="6e224-146">Zapisz plik (listy Ctl + S).</span><span class="sxs-lookup"><span data-stu-id="6e224-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="6e224-147">Konieczne będzie ten adres URL do skonfigurowania aplikacji uwierzytelniania serwisu Facebook i Google.</span><span class="sxs-lookup"><span data-stu-id="6e224-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="6e224-148">Dodaj [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atrybutu `Home` kontrolera, aby wymagać wszystkie żądania muszą używać protokołu HTTPS.</span><span class="sxs-lookup"><span data-stu-id="6e224-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="6e224-149">Bardziej bezpieczną metodą jest dodanie [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtru do aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e224-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="6e224-150">Zobacz sekcję &quot;ochrona aplikacji przy użyciu protokołu SSL i autoryzować atrybut&quot; w mojej tutoral [tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL i wdrażanie w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span><span class="sxs-lookup"><span data-stu-id="6e224-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="6e224-151">Poniżej przedstawiono część kontrolera głównego.</span><span class="sxs-lookup"><span data-stu-id="6e224-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="6e224-152">Naciśnij klawisze CTRL + F5, aby uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6e224-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="6e224-153">Jeśli certyfikat został zainstalowany w przeszłości, możesz pominąć pozostałą część tej sekcji i przejdź do [tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji do projektu](#goog), w przeciwnym razie wykonaj instrukcje zaufania z podpisem własnym certyfikat, który wygenerował usług IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6e224-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="6e224-154">Odczyt **ostrzeżenie o zabezpieczeniach** okna dialogowego, a następnie kliknij przycisk **tak** Jeśli chcesz zainstalować certyfikat reprezentujący localhost.</span><span class="sxs-lookup"><span data-stu-id="6e224-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="6e224-155">Pokazuje, IE *Home* strony, a Brak ostrzeżeń protokołu SSL.</span><span class="sxs-lookup"><span data-stu-id="6e224-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="6e224-156">Google Chrome również zaakceptuje certyfikat i wyświetli zawartość HTTPS bez ostrzeżenia.</span><span class="sxs-lookup"><span data-stu-id="6e224-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="6e224-157">Firefox używa magazynu certyfikatów, dzięki czemu zostanie wyświetlone ostrzeżenie.</span><span class="sxs-lookup"><span data-stu-id="6e224-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="6e224-158">W przypadku naszej aplikacji można bezpiecznie kliknąć **rozumiem ryzyko**.</span><span class="sxs-lookup"><span data-stu-id="6e224-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="6e224-159">Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji do projektu</span><span class="sxs-lookup"><span data-stu-id="6e224-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="6e224-160">Bieżący protokołu Google OAuth instrukcje można znaleźć [konfigurowania uwierzytelniania serwisu Google w programie ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span><span class="sxs-lookup"><span data-stu-id="6e224-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="6e224-161">Przejdź do [konsoli deweloperów Google](https://console.developers.google.com/).</span><span class="sxs-lookup"><span data-stu-id="6e224-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="6e224-162">Jeśli nie utworzono projektu przed wybierz **poświadczenia** w karcie po lewej stronie, a następnie wybierz **Utwórz**.</span><span class="sxs-lookup"><span data-stu-id="6e224-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="6e224-163">Na karcie po lewej stronie kliknij **poświadczenia**.</span><span class="sxs-lookup"><span data-stu-id="6e224-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="6e224-164">Kliknij przycisk **Utwórz poświadczenia** następnie **identyfikator klienta OAuth**.</span><span class="sxs-lookup"><span data-stu-id="6e224-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="6e224-165">W **Utwórz identyfikator klienta** okno dialogowe, zachowaj ustawienie domyślne **aplikacji sieci Web** dla typu aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e224-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="6e224-166">Ustaw **autoryzacji JavaScript** źródła do adresu URL protokołu SSL, które zostały użyte powyżej (`https://localhost:44300/` chyba, że utworzono inne projekty, protokołu SSL)</span><span class="sxs-lookup"><span data-stu-id="6e224-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="6e224-167">Ustaw **identyfikator URI przekierowania autoryzowanych** do:</span><span class="sxs-lookup"><span data-stu-id="6e224-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="6e224-168">Kliknij element menu ekran zgody OAuth, a następnie ustaw swoje wiadomości e-mail adres i nazwy produktu.</span><span class="sxs-lookup"><span data-stu-id="6e224-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="6e224-169">Po zakończeniu kliknij przycisk formularza **Zapisz**.</span><span class="sxs-lookup"><span data-stu-id="6e224-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="6e224-170">Kliknij element menu, biblioteki, wyszukać **interfejsu API Google +**, kliknij go, a następnie naciśnij przycisk Włącz.</span><span class="sxs-lookup"><span data-stu-id="6e224-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="6e224-171">Na poniższym obrazie przedstawiono włączonych interfejsów API.</span><span class="sxs-lookup"><span data-stu-id="6e224-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="6e224-172">Odwiedź stronę z Google APIs API Manager **poświadczenia** kartę, aby uzyskać **identyfikator klienta**.</span><span class="sxs-lookup"><span data-stu-id="6e224-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="6e224-173">Pobierz, aby zapisać plik w formacie JSON przy użyciu kluczy tajnych aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e224-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="6e224-174">Skopiuj i Wklej **ClientId** i **ClientSecret** do `UseGoogleAuthentication` znaleźć metodę w *Startup.Auth.cs* w pliku *App_Start* folderu.</span><span class="sxs-lookup"><span data-stu-id="6e224-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="6e224-175">**ClientId** i **ClientSecret** wartości podanych poniżej są przykłady i nie będą działać.</span><span class="sxs-lookup"><span data-stu-id="6e224-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="6e224-176">Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym.</span><span class="sxs-lookup"><span data-stu-id="6e224-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="6e224-177">Konto i poświadczenia są dodawane do kodu powyżej, aby uprościć przykład.</span><span class="sxs-lookup"><span data-stu-id="6e224-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="6e224-178">Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i usłudze Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span><span class="sxs-lookup"><span data-stu-id="6e224-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="6e224-179">Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby skompilować i uruchomić aplikację.</span><span class="sxs-lookup"><span data-stu-id="6e224-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="6e224-180">Kliknij przycisk **Zaloguj** łącza.</span><span class="sxs-lookup"><span data-stu-id="6e224-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="6e224-181">W obszarze **Zaloguj się za pomocą innej usługi**, kliknij przycisk **Google**.</span><span class="sxs-lookup"><span data-stu-id="6e224-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="6e224-182">Jeśli pominiesz dowolnego z powyższych czynności zostanie wyświetlony błąd HTTP 401.</span><span class="sxs-lookup"><span data-stu-id="6e224-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="6e224-183">Ponownie sprawdź powyższe kroki.</span><span class="sxs-lookup"><span data-stu-id="6e224-183">Recheck your steps above.</span></span> <span data-ttu-id="6e224-184">Jeśli pominiesz wymaganego ustawienia (na przykład **nazwa produktu**), Dodaj brakującego elementu i zapisywanie; może potrwać kilka minut, zanim prawidłowego działania uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="6e224-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="6e224-185">Nastąpi przekierowanie do witryny Google, w którym należy wprowadzić poświadczenia.</span><span class="sxs-lookup"><span data-stu-id="6e224-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="6e224-186">Po wprowadzeniu poświadczeń, pojawi się monit do nadawania uprawnień do aplikacji sieci web, który został utworzony:</span><span class="sxs-lookup"><span data-stu-id="6e224-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="6e224-187">Kliknij przycisk **zaakceptować**.</span><span class="sxs-lookup"><span data-stu-id="6e224-187">Click **Accept**.</span></span> <span data-ttu-id="6e224-188">Teraz nastąpi przekierowanie do **zarejestrować** strony aplikacji MvcAuth, gdy zarejestrujesz swoje konto Google.</span><span class="sxs-lookup"><span data-stu-id="6e224-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="6e224-189">Masz możliwość zmiany nazwy rejestracji lokalny adres e-mail używany dla tego konta usługi Gmail, ale zazwyczaj chcesz zachować aliasu adresu e-mail domyślne (czyli ten, który używany do uwierzytelniania).</span><span class="sxs-lookup"><span data-stu-id="6e224-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="6e224-190">Kliknij przycisk **zarejestrować**.</span><span class="sxs-lookup"><span data-stu-id="6e224-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="6e224-191">Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji do projektu</span><span class="sxs-lookup"><span data-stu-id="6e224-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="6e224-192">Bieżący Facebook OAuth2 uwierzytelniania instrukcje można znaleźć [uwierzytelnianie Konfigurowanie serwisu Facebook](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="6e224-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="6e224-193">Dla uwierzytelniania serwisu Facebook OAuth2 należy skopiować do projektu niektórych ustawień z aplikacji utworzonej w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="6e224-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="6e224-194">W przeglądarce przejdź do [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) i zaloguj się, wprowadzając swoje poświadczenia usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="6e224-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="6e224-195">Jeśli nie są jeszcze zarejestrowana jako deweloper usługi Facebook, kliknij przycisk **Zarejestruj się jako deweloper** i postępuj zgodnie z instrukcjami, aby zarejestrować.</span><span class="sxs-lookup"><span data-stu-id="6e224-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="6e224-196">Na **aplikacje** kliknij pozycję **Utwórz nową aplikację**.</span><span class="sxs-lookup"><span data-stu-id="6e224-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![Utwórz nową aplikację](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="6e224-198">Wprowadź **nazwy aplikacji** i **kategorii**, następnie kliknij przycisk **tworzenie aplikacji**.</span><span class="sxs-lookup"><span data-stu-id="6e224-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="6e224-199"><strong>Namespace aplikacji</strong> jest częścią adresu URL, który Twoja aplikacja będzie używać do uzyskania dostępu do aplikacji usługi Facebook do uwierzytelniania (np. https\://apps.facebook.com/{App Namespace}).</span><span class="sxs-lookup"><span data-stu-id="6e224-199">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https\://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="6e224-200">Jeśli nie określisz <strong>Namespace aplikacji</strong>, <strong>Identyfikatora aplikacji</strong> będzie służyć jako adres URL.</span><span class="sxs-lookup"><span data-stu-id="6e224-200">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="6e224-201"><strong>Identyfikatora aplikacji</strong> jest liczbą długo generowanych przez system, będą widoczne w następnym kroku.</span><span class="sxs-lookup"><span data-stu-id="6e224-201">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![Utwórz nową aplikację w oknie dialogowym](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="6e224-203">Prześlij wyboru standardowych zabezpieczeń.</span><span class="sxs-lookup"><span data-stu-id="6e224-203">Submit the standard security check.</span></span>

    ![Sprawdzanie zabezpieczeń](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="6e224-205">Wybierz **ustawienia** paska menu po lewej stronie![ Pasek menu dewelopera usługi Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="6e224-205">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="6e224-206">Na **podstawowe** ustawienia części strony wybierz **Dodaj platformy** do określenia, czy dodajesz aplikację witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="6e224-206">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="6e224-207">![Ustawienia podstawowe](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="6e224-207">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="6e224-208">Wybierz **witryny sieci Web** z możliwości platformy.</span><span class="sxs-lookup"><span data-stu-id="6e224-208">Select **Website** from the platform choices.</span></span>  
  
    ![Możliwości platformy](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="6e224-210">Zwróć uwagę na Twoje **Identyfikatora aplikacji** i **klucz tajny aplikacji** tak, aby można było dodać zarówno do aplikacji MVC w dalszej części tego samouczka.</span><span class="sxs-lookup"><span data-stu-id="6e224-210">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="6e224-211">Ponadto Dodaj adres URL witryny (`https://localhost:44300/`) do testowania aplikacji MVC.</span><span class="sxs-lookup"><span data-stu-id="6e224-211">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="6e224-212">Ponadto Dodaj **E-mail kontaktowy**.</span><span class="sxs-lookup"><span data-stu-id="6e224-212">Also, add a **Contact Email**.</span></span> <span data-ttu-id="6e224-213">Następnie wybierz **Zapisz zmiany**.</span><span class="sxs-lookup"><span data-stu-id="6e224-213">Then, select **Save Changes**.</span></span>   

    ![Strona szczegółów aplikacji w warstwie podstawowa](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="6e224-215">Należy zauważyć, że tylko można przeprowadzić uwierzytelniania za pomocą aliasu adresu e-mail, które zostały zarejestrowane.</span><span class="sxs-lookup"><span data-stu-id="6e224-215">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="6e224-216">Innym użytkownikom i testowe konta nie będzie można zarejestrować.</span><span class="sxs-lookup"><span data-stu-id="6e224-216">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="6e224-217">Można udzielić dostępu dla kont usługi Facebook innych aplikacji w usłudze Facebook **ról dla deweloperów** kartę.</span><span class="sxs-lookup"><span data-stu-id="6e224-217">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="6e224-218">W programie Visual Studio, otwórz *aplikacji\_Start\Startup.Auth.cs*.</span><span class="sxs-lookup"><span data-stu-id="6e224-218">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="6e224-219">Skopiuj i Wklej **AppId** i **klucz tajny aplikacji** do `UseFacebookAuthentication` metody.</span><span class="sxs-lookup"><span data-stu-id="6e224-219">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="6e224-220">**AppId** i **klucz tajny aplikacji** wartości podanych poniżej są przykłady i nie będzie działać.</span><span class="sxs-lookup"><span data-stu-id="6e224-220">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="6e224-221">Kliknij przycisk **Zapisz zmiany**.</span><span class="sxs-lookup"><span data-stu-id="6e224-221">Click **Save Changes**.</span></span>
13. <span data-ttu-id="6e224-222">Naciśnij klawisz **kombinację klawiszy CTRL + F5** do uruchomienia aplikacji.</span><span class="sxs-lookup"><span data-stu-id="6e224-222">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="6e224-223">Wybierz **Zaloguj** można wyświetlić strony logowania.</span><span class="sxs-lookup"><span data-stu-id="6e224-223">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="6e224-224">Kliknij przycisk **Facebook** w obszarze **Zaloguj się za pomocą innej usługi.**</span><span class="sxs-lookup"><span data-stu-id="6e224-224">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="6e224-225">Wprowadź swoje poświadczenia usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="6e224-225">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="6e224-226">Użytkownik jest monitowany o udzielenie uprawnień dla aplikacji, aby uzyskać dostęp do Twojego profilu publicznego i list znajomych.</span><span class="sxs-lookup"><span data-stu-id="6e224-226">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Szczegóły aplikacji usługi Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="6e224-228">Obecnie zalogowano Cię.</span><span class="sxs-lookup"><span data-stu-id="6e224-228">You are now logged in.</span></span> <span data-ttu-id="6e224-229">Możesz teraz zarejestrować tego konta z aplikacją.</span><span class="sxs-lookup"><span data-stu-id="6e224-229">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="6e224-230">Po zarejestrowaniu, wpis jest dodawany do *użytkowników* tabeli członkostwa bazy danych.</span><span class="sxs-lookup"><span data-stu-id="6e224-230">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="6e224-231">Sprawdzanie danych członkostwa</span><span class="sxs-lookup"><span data-stu-id="6e224-231">Examine the Membership Data</span></span>

<span data-ttu-id="6e224-232">W **widoku** menu, kliknij przycisk **Eksploratora serwera**.</span><span class="sxs-lookup"><span data-stu-id="6e224-232">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="6e224-233">Rozwiń **DefaultConnection (MvcAuth)**, rozwiń węzeł **tabel**, kliknij prawym przyciskiem myszy **AspNetUsers** i kliknij przycisk **Pokaż dane tabeli**.</span><span class="sxs-lookup"><span data-stu-id="6e224-233">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dane tabeli aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="6e224-235">Dodawanie danych profilu do klasy użytkownika</span><span class="sxs-lookup"><span data-stu-id="6e224-235">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="6e224-236">W tej sekcji dodasz datę urodzin i macierzystego miejscowość danych użytkownika podczas rejestracji, jak pokazano na poniższej ilustracji.</span><span class="sxs-lookup"><span data-stu-id="6e224-236">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![reg za pomocą macierzystego miejscowości i Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="6e224-238">Otwórz *Models\IdentityModels.cs* pliku i Dodaj właściwości miejscowość strona główna i daty urodzenia:</span><span class="sxs-lookup"><span data-stu-id="6e224-238">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="6e224-239">Otwórz *Models\AccountViewModels.cs* plików i zestaw urodzenia daty i w domu właściwości Miejscowość w `ExternalLoginConfirmationViewModel`.</span><span class="sxs-lookup"><span data-stu-id="6e224-239">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="6e224-240">Otwórz *Controllers\AccountController.cs* pliku, a następnie dodaj kod miejscowości strona główna i daty urodzenia w `ExternalLoginConfirmation` metody akcji, jak pokazano:</span><span class="sxs-lookup"><span data-stu-id="6e224-240">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="6e224-241">Dodaj datę urodzenia i macierzystego mieście *Views\Account\ExternalLoginConfirmation.cshtml* pliku:</span><span class="sxs-lookup"><span data-stu-id="6e224-241">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="6e224-242">Usuń bazę danych członkostwa, dzięki czemu możesz ponownie się zarejestrować Twojego konta serwisu Facebook ze swoją aplikacją i sprawdź, czy można dodać nową datę urodzenia i informacje o profilu macierzystego miejscowości.</span><span class="sxs-lookup"><span data-stu-id="6e224-242">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="6e224-243">Z **Eksploratora rozwiązań**, kliknij przycisk **Pokaż wszystkie pliki** ikonę, a następnie kliknij prawym przyciskiem myszy *Dodaj\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* i kliknij przycisk **Usuń**.</span><span class="sxs-lookup"><span data-stu-id="6e224-243">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="6e224-244">Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet**, następnie kliknij przycisk **Konsola Menedżera pakietów** (PMC).</span><span class="sxs-lookup"><span data-stu-id="6e224-244">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="6e224-245">Wprowadź następujące polecenia w konsoli zarządzania Pakietami.</span><span class="sxs-lookup"><span data-stu-id="6e224-245">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="6e224-246">Enable-Migrations</span><span class="sxs-lookup"><span data-stu-id="6e224-246">Enable-Migrations</span></span>
2. <span data-ttu-id="6e224-247">Dodaj migracji Init</span><span class="sxs-lookup"><span data-stu-id="6e224-247">Add-Migration Init</span></span>
3. <span data-ttu-id="6e224-248">Aktualizuj bazy danych</span><span class="sxs-lookup"><span data-stu-id="6e224-248">Update-Database</span></span>

<span data-ttu-id="6e224-249">Uruchom aplikację, a następnie zaloguj się i rejestrować niektórych użytkowników za pomocą usługi FaceBook i Google.</span><span class="sxs-lookup"><span data-stu-id="6e224-249">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="6e224-250">Sprawdzanie danych członkostwa</span><span class="sxs-lookup"><span data-stu-id="6e224-250">Examine the Membership Data</span></span>

<span data-ttu-id="6e224-251">W **widoku** menu, kliknij przycisk **Eksploratora serwera**.</span><span class="sxs-lookup"><span data-stu-id="6e224-251">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="6e224-252">Kliknij prawym przyciskiem myszy **AspNetUsers** i kliknij przycisk **Pokaż dane tabeli**.</span><span class="sxs-lookup"><span data-stu-id="6e224-252">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="6e224-253">`HomeTown` i `BirthDate` pola zostały wymienione poniżej.</span><span class="sxs-lookup"><span data-stu-id="6e224-253">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="6e224-254">Rejestrowanie aplikacji i logując się przy użyciu innego konta</span><span class="sxs-lookup"><span data-stu-id="6e224-254">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="6e224-255">Jeśli Zaloguj się do aplikacji za pomocą usługi Facebook, a następnie zaloguj i spróbuj zalogować się ponownie z innym kontem serwisu Facebook (przy użyciu tej samej przeglądarki), można będzie natychmiast zalogowanie do poprzedniego konta Facebook, którego użyto.</span><span class="sxs-lookup"><span data-stu-id="6e224-255">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="6e224-256">Aby użyć innego konta, musisz przejść do usługi Facebook i wylogowania w serwisie Facebook.</span><span class="sxs-lookup"><span data-stu-id="6e224-256">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="6e224-257">Ta sama zasada dotyczy do wszelkich innych 3 strony dostawcy uwierzytelniania.</span><span class="sxs-lookup"><span data-stu-id="6e224-257">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="6e224-258">Możesz też zalogować się przy użyciu innego konta przy użyciu innej przeglądarki.</span><span class="sxs-lookup"><span data-stu-id="6e224-258">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6e224-259">Następne kroki</span><span class="sxs-lookup"><span data-stu-id="6e224-259">Next Steps</span></span>

<span data-ttu-id="6e224-260">Zobacz [wprowadzenie do dostawców zabezpieczeń Yahoo i LinkedIn OAuth dla OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) przez Jerrie Pelser instrukcje dotyczące Yahoo i LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="6e224-260">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="6e224-261">Zobacz firmy Jerrie wygląda przyciski społecznościowych logowania dla platformy ASP.NET MVC 5 można pobrać Włącz logowanie społecznościowych przycisków.</span><span class="sxs-lookup"><span data-stu-id="6e224-261">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="6e224-262">Postępuj zgodnie z samouczkiem Moje [tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL i wdrażanie w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), co będzie się powtarzać tego samouczka i zawiera następujące czynności:</span><span class="sxs-lookup"><span data-stu-id="6e224-262">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="6e224-263">Jak wdrożyć aplikację na platformie Azure.</span><span class="sxs-lookup"><span data-stu-id="6e224-263">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="6e224-264">Jak zabezpieczyć aplikację przy użyciu ról.</span><span class="sxs-lookup"><span data-stu-id="6e224-264">How to secure you app with roles.</span></span>
3. <span data-ttu-id="6e224-265">Jak zabezpieczyć swoją aplikację przy użyciu [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) i [Autoryzuj](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtrów.</span><span class="sxs-lookup"><span data-stu-id="6e224-265">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="6e224-266">Jak użyć członkostwo interfejsu API, aby dodać użytkowników i ról.</span><span class="sxs-lookup"><span data-stu-id="6e224-266">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="6e224-267">Jak się podoba w tym samouczku, i co można było ulepszyć proces Wystaw opinię.</span><span class="sxs-lookup"><span data-stu-id="6e224-267">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="6e224-268">Możesz również poprosić o nowe tematy w [Pokaż mi jak za pomocą kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span><span class="sxs-lookup"><span data-stu-id="6e224-268">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="6e224-269">Można również poprosić o i oddawać głosy na nowe funkcje, które mają zostać dodane do programu ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="6e224-269">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="6e224-270">Na przykład, możesz głosować na narzędzie do [tworzenie i zarządzanie użytkownikami i rolami.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="6e224-270">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="6e224-271">Dobry opis działania zewnętrznych usług uwierzytelniania platformy ASP.NET, zobacz Robert McMurray [zewnętrznych usług uwierzytelniania](https://asp.net/web-api/overview/security/external-authentication-services).</span><span class="sxs-lookup"><span data-stu-id="6e224-271">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="6e224-272">Artykuł Roberta również przechodzi w stan szczegółów włączania uwierzytelniania firmy Microsoft i Twitter.</span><span class="sxs-lookup"><span data-stu-id="6e224-272">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="6e224-273">Tom Dykstra firmy doskonale [samouczek programu EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) przedstawiono sposób pracy z platformą Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="6e224-273">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
