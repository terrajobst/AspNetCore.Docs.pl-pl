---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: "Zalogował się przy użyciu zewnętrznych witryn w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft"
author: tfitzmac
description: "W tym artykule wyjaśniono, jak zalogować się do witryny stron sieci Web platformy ASP.NET (Razor) za pomocą usługi Facebook, Google, Twitter, Yahoo i innych witryn, oznacza to, jak obsługiwać..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="bae27-103">Rejestrowanie przy użyciu zewnętrznych witryn w sieci Web platformy ASP.NET (Razor) stron witryny</span><span class="sxs-lookup"><span data-stu-id="bae27-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="bae27-104">przez [FitzMacken niestandardowy](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="bae27-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="bae27-105">W tym artykule wyjaśniono, jak zalogować się do witryny stron sieci Web platformy ASP.NET (Razor) za pomocą usługi Facebook, Google, Twitter, Yahoo i innych witryn, oznacza to, jak obsługiwać OAuth i OpenID w witrynie.</span><span class="sxs-lookup"><span data-stu-id="bae27-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="bae27-106">Zawartość:</span><span class="sxs-lookup"><span data-stu-id="bae27-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="bae27-107">Jak włączyć logowanie z innych lokacji przy użyciu szablonu programu WebMatrix witryny początkowej.</span><span class="sxs-lookup"><span data-stu-id="bae27-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="bae27-108">Jest to funkcja ASP.NET, wprowadzona w artykule:</span><span class="sxs-lookup"><span data-stu-id="bae27-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="bae27-109">`OAuthWebSecurity` Pomocnika.</span><span class="sxs-lookup"><span data-stu-id="bae27-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bae27-110">Używane w samouczku wersje oprogramowania</span><span class="sxs-lookup"><span data-stu-id="bae27-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bae27-111">Strony sieci Web platformy ASP.NET (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="bae27-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="bae27-112">Program WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="bae27-112">WebMatrix 3</span></span>

<span data-ttu-id="bae27-113">Strony ASP.NET Web Pages obsługuje [OAuth](http://oauth.net/) i [OpenID](http://openid.net/) dostawców.</span><span class="sxs-lookup"><span data-stu-id="bae27-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="bae27-114">Przy użyciu tych dostawców, możesz pozwolić, aby użytkowników logowania do witryny przy użyciu swoich istniejących poświadczeń z usługi Facebook, Twitter, firma Microsoft i Google.</span><span class="sxs-lookup"><span data-stu-id="bae27-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="bae27-115">Na przykład aby zalogować się przy użyciu konta usługi Facebook, użytkowników można po prostu ikonę usługi Facebook, który przekierowuje go do strony logowania usługi Facebook, którym oni wprowadzić swoje informacje o użytkowniku.</span><span class="sxs-lookup"><span data-stu-id="bae27-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="bae27-116">Można następnie skojarzyć logowania serwisu Facebook do swojego konta w witrynie.</span><span class="sxs-lookup"><span data-stu-id="bae27-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="bae27-117">Powiązane rozszerzenie do funkcji przynależności stron sieci Web jest czy użytkownicy mogą powiązać wiele logowań (w tym logowania z witrynami sieci społecznościowych) z jednego konta w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bae27-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="bae27-118">Ten obraz zawiera strony logowania z **witryny początkowej** szablon, w którym użytkownik może wybrać ikonę Facebook, Twitter, Google lub Microsoft, aby włączyć logowanie przy użyciu zewnętrznego konta:</span><span class="sxs-lookup"><span data-stu-id="bae27-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![zewnętrznych dostawców](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="bae27-120">Można włączyć członkostwo OAuth i OpenID usunięcie komentarza kilku wierszy kodu w **witryny początkowej** szablonu.</span><span class="sxs-lookup"><span data-stu-id="bae27-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="bae27-121">Metody i właściwości można używać do pracy z uwierzytelniania OAuth i OpenID dostawców znajdują się w `WebMatrix.Security.OAuthWebSecurity` klasy.</span><span class="sxs-lookup"><span data-stu-id="bae27-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="bae27-122">**Witryny początkowej** szablon zawiera infrastruktury pełnego członkostwa, strony logowania, bazy danych członkostwa oraz cały kod należy powiadomić użytkowników logowania do witryny przy użyciu poświadczeń lokalnych lub z innej lokacji .</span><span class="sxs-lookup"><span data-stu-id="bae27-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="bae27-123">Ta sekcja zawiera przykładowy sposób umożliwić użytkownikom logowania z zewnętrznych witryn do lokacji, która jest oparta na **witryny początkowej** szablonu.</span><span class="sxs-lookup"><span data-stu-id="bae27-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="bae27-124">Po utworzeniu witryny początkowej, możesz wykonać tego (szczegóły poniżej):</span><span class="sxs-lookup"><span data-stu-id="bae27-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="bae27-125">Dla witryn, które używa dostawcy OAuth (Facebook, Twitter i firmy Microsoft) utworzyć aplikację w witrynie zewnętrznych.</span><span class="sxs-lookup"><span data-stu-id="bae27-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="bae27-126">Udostępnia klucze aplikacji, które będą potrzebne, aby można było wywołać funkcję logowania dla tych witryn.</span><span class="sxs-lookup"><span data-stu-id="bae27-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="bae27-127">Dla witryn, które korzystają z dostawcy uwierzytelniania OpenID (Google) nie trzeba tworzyć aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bae27-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="bae27-128">Wszystkie te witryny musi mieć konto, aby zalogować się i utworzyć aplikacjami dla deweloperów.</span><span class="sxs-lookup"><span data-stu-id="bae27-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="bae27-129">Aplikacje firmy Microsoft akceptować tylko na żywo adres URL witryny sieci Web pracy, więc nie można używać adresu URL lokalną witrynę sieci Web do testowania logowania.</span><span class="sxs-lookup"><span data-stu-id="bae27-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="bae27-130">Edytuj kilka plików w witrynie sieci Web, aby określić dostawcę uwierzytelniania i przesłać dane logowania do witryny, której chcesz użyć.</span><span class="sxs-lookup"><span data-stu-id="bae27-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="bae27-131">Ten artykuł zawiera osobne instrukcje dotyczące następujących zadań:</span><span class="sxs-lookup"><span data-stu-id="bae27-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="bae27-132">Włączanie logowania do usługi Google</span><span class="sxs-lookup"><span data-stu-id="bae27-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="bae27-133">Włączanie logowania do usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="bae27-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="bae27-134">Włączanie logowania do usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="bae27-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="bae27-135">Włączanie logowania do usługi Google</span><span class="sxs-lookup"><span data-stu-id="bae27-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="bae27-136">Utwórz lub Otwórz witrynę ASP.NET Web Pages opartego na szablonie witryny początkowej programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bae27-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="bae27-137">Otwórz  *\_AppStart.cshtml* strony i Usuń komentarz następujący wiersz kodu.</span><span class="sxs-lookup"><span data-stu-id="bae27-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="bae27-138">Testowanie logowania Google</span><span class="sxs-lookup"><span data-stu-id="bae27-138">Testing Google login</span></span>

1. <span data-ttu-id="bae27-139">Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **Zaloguj** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bae27-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="bae27-140">Na *logowania* strony w **Zaloguj się za pomocą innej usługi** albo wybierz **Google** lub **Yahoo** przycisk Prześlij.</span><span class="sxs-lookup"><span data-stu-id="bae27-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="bae27-141">W tym przykładzie użyto logowania Google.</span><span class="sxs-lookup"><span data-stu-id="bae27-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="bae27-142">Strony sieci web przekierowuje żądanie do strony logowania usługi Google.</span><span class="sxs-lookup"><span data-stu-id="bae27-142">The web page redirects the request to the Google login page.</span></span>

    ![Logowanie do usługi Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="bae27-144">Wprowadź poświadczenia dla istniejącego konta Google.</span><span class="sxs-lookup"><span data-stu-id="bae27-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="bae27-145">Jeśli Google zapyta, czy chcesz zezwolić *Localhost* Aby użyć informacji z konta, kliknij przycisk **Zezwalaj**.</span><span class="sxs-lookup"><span data-stu-id="bae27-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="bae27-146">Kod używa tokenu Google do uwierzytelniania użytkownika, a następnie wróci do tej strony w witrynie sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bae27-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="bae27-147">Ta strona umożliwia użytkownikom skojarzyć ich Google logowania przy użyciu istniejącego konta w witrynie sieci Web lub ich zarejestrować nowe konto w witrynie do skojarzenia zewnętrznych danych logowania z.</span><span class="sxs-lookup"><span data-stu-id="bae27-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="bae27-149">Wybierz **skojarzyć** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bae27-149">Choose the **Associate** button.</span></span> <span data-ttu-id="bae27-150">Zwraca przeglądarki do strony głównej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bae27-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="bae27-151">Włączanie logowania do usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="bae27-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="bae27-152">Przejdź do [lokacji deweloperzy Facebook](https://developers.facebook.com/apps) (dziennik, jeśli jeszcze nie jest zalogowany).</span><span class="sxs-lookup"><span data-stu-id="bae27-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="bae27-153">Wybierz **Utwórz nową aplikację** przycisk, a następnie postępuj zgodnie z monitami, aby utworzyć nową aplikację.</span><span class="sxs-lookup"><span data-stu-id="bae27-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="bae27-154">W sekcji **wybierz, jak aplikacja zostanie zintegrowana z usługą Facebook**, wybierz **witryny sieci Web** sekcji.</span><span class="sxs-lookup"><span data-stu-id="bae27-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="bae27-155">Wypełnij **adres URL witryny** pole adresu URL witryny (na przykład `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="bae27-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="bae27-156">**Domeny** pole jest opcjonalne; umożliwia to zapewnia uwierzytelnianie dla całej domeny (takich jak *example.com*).</span><span class="sxs-lookup"><span data-stu-id="bae27-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="bae27-157">Jeśli używasz lokacji na komputerze lokalnym przy użyciu adresu URL, takich jak `http://localhost:12345` (gdzie numer jest numerem portu lokalnego), można dodać tę wartość na **adres URL witryny** pole do testowania witryny.</span><span class="sxs-lookup"><span data-stu-id="bae27-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="bae27-158">Jednak każdy razem numer portu lokacji lokalnej zmiany, musisz zaktualizować **adres URL witryny** pole aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bae27-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="bae27-159">Wybierz **Zapisz zmiany** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bae27-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="bae27-160">Wybierz **aplikacji** karcie ponownie, a następnie Wyświetl strony początkowej aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bae27-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="bae27-161">Kopiuj **identyfikator aplikacji** i **klucz tajny aplikacji** wartości dla aplikacji i wklej je do pliku tekstowego tymczasowego.</span><span class="sxs-lookup"><span data-stu-id="bae27-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="bae27-162">Te wartości zostaną spełnione dla dostawcy usługi Facebook w kodzie witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bae27-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="bae27-163">Zamknij stronę dewelopera usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="bae27-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="bae27-164">Teraz możesz zmienić dwie strony w witrynie sieci Web, dzięki czemu użytkownicy będą może logować się do witryny za pomocą ich kont usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="bae27-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="bae27-165">Utwórz lub Otwórz witrynę ASP.NET Web Pages opartego na szablonie witryny początkowej programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bae27-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="bae27-166">Otwórz  *\_AppStart.cshtml* strony i Usuń komentarz kodu dla dostawcy uwierzytelniania Facebook OAuth.</span><span class="sxs-lookup"><span data-stu-id="bae27-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="bae27-167">Blok kodu uncommented wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="bae27-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="bae27-168">Kopiuj **identyfikator aplikacji** wartość z zakresu od aplikacji usługi Facebook jako wartość `appId` parametr (wewnątrz cudzysłowów).</span><span class="sxs-lookup"><span data-stu-id="bae27-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="bae27-169">Kopiuj **klucz tajny aplikacji** wartość z zakresu od aplikacji usługi Facebook jako `appSecret` wartość parametru.</span><span class="sxs-lookup"><span data-stu-id="bae27-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="bae27-170">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="bae27-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="bae27-171">Testowanie logowania usługi Facebook</span><span class="sxs-lookup"><span data-stu-id="bae27-171">Testing Facebook login</span></span>

1. <span data-ttu-id="bae27-172">Uruchom witrynę *default.cshtml* strony i wybierz polecenie **logowania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bae27-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="bae27-173">Na *logowania* strony w **Zaloguj się za pomocą innej usługi** wybierz **Facebook** ikony.</span><span class="sxs-lookup"><span data-stu-id="bae27-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="bae27-174">Strony sieci web przekierowuje żądanie do strony logowania usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="bae27-174">The web page redirects the request to the Facebook login page.</span></span>

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="bae27-176">Zaloguj się do konta usługi Facebook.</span><span class="sxs-lookup"><span data-stu-id="bae27-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="bae27-177">Kod używa tokenu usługi Facebook do uwierzytelniania, a następnie zwraca stronę możesz skojarzyć nazwę użytkownika usługi Facebook z logowania w witrynie.</span><span class="sxs-lookup"><span data-stu-id="bae27-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="bae27-178">Adres nazwę lub adres e-mail użytkownika jest wprowadzany do **E-mail** pola formularza.</span><span class="sxs-lookup"><span data-stu-id="bae27-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="bae27-180">Wybierz **skojarzyć** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bae27-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="bae27-181">Zwraca przeglądarki do strony głównej i użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="bae27-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="bae27-182">Włączanie logowania do usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="bae27-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="bae27-183">Przejdź do [lokacji deweloperzy Twitter](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="bae27-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="bae27-184">Wybierz **Utwórz aplikację** łącza, a następnie zaloguj się do witryny.</span><span class="sxs-lookup"><span data-stu-id="bae27-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="bae27-185">Na **tworzenie aplikacji** tworzą, wypełnij **nazwa** i **opis** pola.</span><span class="sxs-lookup"><span data-stu-id="bae27-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="bae27-186">W **witryny sieci Web** wprowadź adres URL witryny sieci (na przykład `http://www.example.com`).</span><span class="sxs-lookup"><span data-stu-id="bae27-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="bae27-187">Jeśli testujesz witryny lokalnie (przy użyciu adresu URL, takie jak `http://localhost:12345`), Twitter, adres URL nie może zaakceptować.</span><span class="sxs-lookup"><span data-stu-id="bae27-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="bae27-188">Jednak można używać lokalnego sprzężenia zwrotnego adresu IP (na przykład `http://127.0.0.1:12345`).</span><span class="sxs-lookup"><span data-stu-id="bae27-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="bae27-189">Upraszcza proces testowanie aplikacji lokalnie.</span><span class="sxs-lookup"><span data-stu-id="bae27-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="bae27-190">Jednak za każdym razem, gdy zmienia numer portu witryny lokalnej, należy zaktualizować **witryny sieci Web** pole aplikacji.</span><span class="sxs-lookup"><span data-stu-id="bae27-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="bae27-191">W **wywołania zwrotnego adresu URL** wprowadź adres URL strony w witrynie sieci Web, które użytkowników, aby powrócić do po zalogowaniu w serwisie Twitter.</span><span class="sxs-lookup"><span data-stu-id="bae27-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="bae27-192">Na przykład w celu wysłania użytkownikom do strony głównej witryny Starter (który rozpozna ich stan w zarejestrowany), wprowadź ten sam adres URL wprowadzony w **witryny sieci Web** pola.</span><span class="sxs-lookup"><span data-stu-id="bae27-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="bae27-193">Zaakceptuj postanowienia, a następnie wybierz **tworzenie aplikacji Twitter** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bae27-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="bae27-194">Na **Moje aplikacje** początkowej strony, wybierz utworzoną aplikację.</span><span class="sxs-lookup"><span data-stu-id="bae27-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="bae27-195">Na **szczegóły** kartę, przewiń w dół i wybierz polecenie **Utwórz moje Token dostępu** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bae27-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="bae27-196">Na **szczegóły** karcie, skopiuj **konsumenta** i **klucz tajny klienta** wartości dla aplikacji i wklej je do pliku tekstowego tymczasowego.</span><span class="sxs-lookup"><span data-stu-id="bae27-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="bae27-197">W kodzie witryny sieci Web będzie przekazywać te wartości dostawcy usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="bae27-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="bae27-198">Zakończ witrynie Twitter.</span><span class="sxs-lookup"><span data-stu-id="bae27-198">Exit the Twitter site.</span></span>

<span data-ttu-id="bae27-199">Teraz możesz zmienić dwie strony w witrynie sieci Web, dzięki czemu użytkownicy będą mogli zalogować się do witryny za pomocą ich kont usługi Twitter.</span><span class="sxs-lookup"><span data-stu-id="bae27-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="bae27-200">Utwórz lub Otwórz witrynę ASP.NET Web Pages opartego na szablonie witryny początkowej programu WebMatrix.</span><span class="sxs-lookup"><span data-stu-id="bae27-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="bae27-201">Otwórz  *\_AppStart.cshtml* strony i Usuń komentarz kodu dla dostawcy usługi Twitter OAuth.</span><span class="sxs-lookup"><span data-stu-id="bae27-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="bae27-202">Blok kodu uncommented wygląda następująco:</span><span class="sxs-lookup"><span data-stu-id="bae27-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="bae27-203">Kopiuj **konsumenta** wartość z zakresu od aplikacji Twitter jako wartość `consumerKey` parametr (wewnątrz cudzysłowów).</span><span class="sxs-lookup"><span data-stu-id="bae27-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="bae27-204">Kopiuj **klucz tajny klienta** wartość z zakresu od aplikacji Twitter jako wartość `consumerSecret` parametru.</span><span class="sxs-lookup"><span data-stu-id="bae27-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="bae27-205">Zapisz i zamknij plik.</span><span class="sxs-lookup"><span data-stu-id="bae27-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="bae27-206">Testowanie logowania usługi Twitter</span><span class="sxs-lookup"><span data-stu-id="bae27-206">Testing Twitter login</span></span>

1. <span data-ttu-id="bae27-207">Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **logowania** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bae27-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="bae27-208">Na *logowania* strony w **Zaloguj się za pomocą innej usługi** wybierz **Twitter** ikony.</span><span class="sxs-lookup"><span data-stu-id="bae27-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="bae27-209">Strony sieci web przekierowuje żądanie do strony logowania usługi Twitter dla aplikacji, który został utworzony.</span><span class="sxs-lookup"><span data-stu-id="bae27-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="bae27-211">Zaloguj się do konta w usłudze Twitter.</span><span class="sxs-lookup"><span data-stu-id="bae27-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="bae27-212">Kod używa tokenu usługi Twitter do uwierzytelnienia użytkownika i następnie powrót do strony możesz skojarzyć logowanie za pomocą konta witryny sieci Web.</span><span class="sxs-lookup"><span data-stu-id="bae27-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="bae27-213">Wprowadzany do Twojej nazwy lub adresu e-mail **E-mail** pola formularza.</span><span class="sxs-lookup"><span data-stu-id="bae27-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="bae27-215">Wybierz **skojarzyć** przycisku.</span><span class="sxs-lookup"><span data-stu-id="bae27-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="bae27-216">Zwraca przeglądarki do strony głównej i użytkownik jest zalogowany.</span><span class="sxs-lookup"><span data-stu-id="bae27-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="bae27-217">Dodatkowe zasoby</span><span class="sxs-lookup"><span data-stu-id="bae27-217">Additional Resources</span></span>


- [<span data-ttu-id="bae27-218">Dostosowywanie zachowania całej lokacji</span><span class="sxs-lookup"><span data-stu-id="bae27-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="bae27-219">Dodawanie zabezpieczeń i członkostwo w witrynie stron sieci Web ASP.NET</span><span class="sxs-lookup"><span data-stu-id="bae27-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
