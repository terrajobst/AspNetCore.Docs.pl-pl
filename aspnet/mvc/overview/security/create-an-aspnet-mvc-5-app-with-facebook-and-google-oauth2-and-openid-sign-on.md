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
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 05/21/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Tworzenie aplikacji platformy ASP.NET MVC 5 z usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego (C#)
====================
przez [Rick Anderson](https://github.com/Rick-Anderson)

> W tym samouczku przedstawiono sposób tworzenia aplikacji sieci web platformy ASP.NET MVC 5, która umożliwia użytkownikom logowanie użyciu [OAuth 2.0](http://oauth.net/2/) przy użyciu poświadczeń z zewnętrznego dostawcę uwierzytelniania, takich jak Facebook, Twitter, LinkedIn, Microsoft lub Google. Dla uproszczenia ten samouczek koncentruje się na temat pracy z poświadczeń z usługi Facebook i Google.
> 
> Włączanie te poświadczenia w witrynach sieci web zapewnia znaczących korzyści, ponieważ milionów użytkowników mają już konta z tych dostawców zewnętrznych. Ci użytkownicy będą bardziej skłonni zalogowania się do witryny, jeśli nie mają do tworzenia nowego zestawu poświadczeń.
> 
> Zobacz też [aplikacji ASP.NET MVC 5 z programu SMS i adres e-mail uwierzytelniania dwuskładnikowego](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> Samouczka przedstawiono również sposób dodawania danych profilu dla użytkownika oraz sposób dodawania ról za pomocą interfejsu API członkostwa. W tym samouczku, została napisana przy [Rick Anderson](https://blogs.msdn.com/rickAndy) (wykonaj mnie w serwisie Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Wprowadzenie

Rozpocznij od instalowania i uruchamiania [programu Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj program Visual Studio [2013, aktualizacja 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej. Aby uzyskać pomoc dotyczącą Dropbox, GitHub, Linkedin, Instagram, buforu, Salesforce, pary, Exchange stosu, Tripit, Twitch, Twitter, Yahoo! i więcej, zobacz [przykładowy projekt](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Należy zainstalować program Visual Studio [2013, aktualizacja 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej, do korzystania z usługi Google OAuth 2 i debugowania lokalnie bez ostrzeżeń protokołu SSL.


Kliknij przycisk **nowy projekt** z **Start** strony, lub użyj menu i wybierz **pliku**, a następnie **nowy projekt**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Kliknij przycisk **nowy projekt**, a następnie wybierz pozycję **Visual C#** po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web ASP.NET**. Nazwa projektu "MvcAuth", a następnie kliknij przycisk **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

W **nowy projekt ASP.NET** okna dialogowego, kliknij przycisk **MVC**. Jeśli uwierzytelnianie nie jest **indywidualnych kont użytkowników**, kliknij przycisk **Zmień uwierzytelnianie** i wybrać **indywidualnych kont użytkowników**. Sprawdzając **Hostuj w chmurze**, aplikacja będzie łatwo udostępniać na platformie Azure.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

W przypadku wybrania **Hostuj w chmurze**, ukończyć konfigurowanie okna dialogowego.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Użycie narzędzia NuGet można zaktualizować do najnowszego oprogramowania pośredniczącego OWIN

Użyj Menedżera pakietów NuGet, aby zaktualizować [oprogramowanie pośredniczące OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Wybierz **aktualizacje** w lewym menu. Możesz kliknąć **Aktualizuj wszystkie** przycisk lub Wyszukaj tylko pakiety OWIN (wyświetlane w obrazie dalej):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Na poniższej ilustracji wyświetlane są tylko pakiety OWIN:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

Z konsoli Menedżera pakietów (PMC), można wprowadzić `Update-Package` polecenia, które będą wszystkich pakietów aktualizacji.

Naciśnij klawisz **F5** lub **Ctrl + F5** do uruchomienia aplikacji. Na poniższej ilustracji numer portu to 1234. Po uruchomieniu aplikacji zostanie wyświetlony inny numer portu.

W zależności od rozmiaru okna przeglądarki, konieczne może być kliknij ikonę nawigacji, aby wyświetlić **Home**, **o**, **skontaktuj się z**, **zarejestrować**i **Zaloguj** łącza.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Konfigurowanie protokołu SSL w projekcie

Aby połączyć dostawców uwierzytelniania, takich jak Google i Facebook, należy skonfigurować usługi IIS Express do używania protokołu SSL. Ważne jest, aby zachować po logowania przy użyciu protokołu SSL i nie spadku HTTP, Twoje plik cookie logowania tylko jako klucz tajny jako nazwę użytkownika i hasło, a następnie bez użycia protokołu SSL jest wysyłana w postaci zwykłego tekstu przez sieć. Ponadto już wykonałeś czas wykonania uzgadniania i należy zabezpieczyć kanał (czyli większość co sprawia, że HTTPS wolniej niż HTTP) przed uruchomieniem potok MVC, więc przekierowywanie z powrotem do HTTP po zalogowano nie należy bieżącego żądania lub przyszłe żądania jest znacznie szybsze.

1. W **Eksploratora rozwiązań**, kliknij przycisk **MvcAuth** projektu.
2. Naciśnięcie klawisza F4, aby wyświetlić właściwości projektu. Alternatywnie z **widoku** menu można wybrać **okna właściwości**.
3. Zmień **SSL włączone** na wartość True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Skopiuj adres URL protokołu SSL (który będzie `https://localhost:44300/` chyba, że po utworzeniu innych projektów SSL).
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **MvcAuth** projekt i wybierz **właściwości**.
6. Wybierz **Web** karcie, a następnie wklej adres URL protokołu SSL do **adres Url projektu** pole. Zapisz plik (listy Ctl + S). Należy ten adres URL do skonfigurowania aplikacji uwierzytelniania usługi Facebook i Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Dodaj [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atrybutu `Home` kontrolera, aby wymagać wszystkie żądania muszą używać protokołu HTTPS. Bardziej bezpieczną metodą jest dodanie [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtr do aplikacji. Zobacz sekcję &quot;chronić aplikacje z protokołu SSL i autoryzować atrybutu&quot; w mojej tutoral [tworzenie aplikacji ASP.NET MVC z uwierzytelniania i bazy danych SQL i wdrożyć w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Poniżej przedstawiono część głównej kontrolera.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Jeśli certyfikat został zainstalowany w przeszłości, możesz pominąć pozostałej części tej sekcji i przejść do [tworzenie aplikacji Google OAuth 2 i łączenie aplikacji z projektu](#goog), w przeciwnym razie wartość postępuj zgodnie z instrukcjami, aby zaufać podpisem certyfikat, który wygenerował usług IIS Express.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Odczyt **ostrzeżenie o zabezpieczeniach** okna dialogowego, a następnie kliknij przycisk **tak** Jeśli chcesz zainstalować certyfikat reprezentujący localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Pokazuje IE *Home* strony i nie ma żadnych ostrzeżeń protokołu SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome również zaakceptuje certyfikat i wyświetli zawartości HTTPS bez ostrzeżenia. Firefox używa magazynu certyfikatów, więc zostanie wyświetlone ostrzeżenie o. Dla aplikacji możesz kliknąć **rozumiem ryzyko**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Tworzenie aplikacji Google OAuth 2 i łączenie aplikacji z projektu

> [!WARNING]
> Bieżący protokołu Google OAuth instrukcje można znaleźć [Konfigurowanie uwierzytelniania serwisu Google w ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Przejdź do [konsoli deweloperów Google](https://console.developers.google.com/).
2. Jeśli nie utworzono projekt przed, wybierz **poświadczenia** w karcie po lewej stronie, a następnie wybierz **Utwórz**.
3. Na karcie po lewej stronie kliknij **poświadczenia**.
4. Kliknij przycisk **Utwórz poświadczenia** następnie **identyfikator klienta OAuth**. 

    1. W **utworzyć identyfikator klienta** okna dialogowego, zachowaj ustawienie domyślne **aplikacji sieci Web** typu aplikacja.
    2. Ustaw **autoryzowany JavaScript** źródła do adresu URL protokołu SSL używany powyżej (`https://localhost:44300/` chyba, że po utworzeniu innych projektów SSL)
    3. Ustaw **identyfikator URI przekierowania autoryzowanych** do:  
         `https://localhost:44300/signin-google`
5. Kliknij element menu zgoda OAuth ekranu, a następnie ustaw nazwę adres i produktu poczty e-mail. Po zakończeniu kliknij formularz **zapisać**.
6. Kliknij element menu biblioteki, wyszukaj **interfejsu API Google +**, kliknij go, a następnie naciśnij klawisz Włącz.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   Na poniższym obrazie pokazano włączone interfejsy API.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Odwiedź stronę z Google APIs interfejsu API menedżera, **poświadczenia** kartę, aby uzyskać **identyfikator klienta**. Pobierz, aby zapisać plik JSON z haseł aplikacji. Skopiuj i Wklej **ClientId** i **ClientSecret** do `UseGoogleAuthentication` znaleziono metody w *Startup.Auth.cs* w pliku *App_Start* folderu. **ClientId** i **ClientSecret** poniższe wartości są przykłady i nie działają.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie magazynu danych poufnych w kodzie źródłowym. Konta i poświadczenia zostaną dodane do powyżej, aby zachować prosty przykład kodu. Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych poufnych danych do platformy ASP.NET i usługi Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Naciśnij klawisz **CTRL + F5** Aby skompilować i uruchomić aplikację. Kliknij przycisk **Zaloguj** łącza.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. W obszarze **Zaloguj się za pomocą innej usługi**, kliknij przycisk **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Jeśli pominiesz dowolne z opisanych wyżej wystąpi błąd HTTP 401. Ponownie sprawdź powyższe kroki. Jeśli pominiesz wymaganego ustawienia (na przykład **nazwa produktu**), Dodaj brakujący element i zapisać; może potrwać kilka minut dla działania uwierzytelniania.
10. Nastąpi przekierowanie do witryny Google, w którym należy wprowadzić poświadczenia.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Po wprowadź swoje poświadczenia, pojawi się monit, aby udzielić uprawnień do nowo utworzonej aplikacji sieci web:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Kliknij przycisk **zaakceptować**. Teraz nastąpi przekierowanie do **zarejestrować** strony aplikacji MvcAuth, w którym można zarejestrować konto Google. Istnieje możliwość zmiany nazwy rejestracji lokalnej poczty e-mail używany dla tego konta usługi Gmail, ale zazwyczaj chcesz pozostawić domyślny alias e-mail (to znaczy aktualnie używany do uwierzytelniania). Kliknij przycisk **zarejestrować**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Tworzenie aplikacji w serwisie Facebook i łączenie aplikacji z projektu

> [!WARNING]
> Bieżący Facebook OAuth2 uwierzytelniania instrukcje można znaleźć [uwierzytelniania Konfigurowanie usługi Facebook](/aspnet/core/security/authentication/social/facebook-logins)

Dla uwierzytelniania serwisu Facebook OAuth2 należy skopiować do projektu niektórych ustawień z aplikacji, która tworzenia w serwisie Facebook.

1. W przeglądarce przejdź do [ https://developers.facebook.com/apps ](https://developers.facebook.com/apps) i zaloguj ponownie wprowadzić swoje poświadczenia usługi Facebook.
2. Jeśli nie są już zarejestrowane jako deweloper usługi Facebook, kliknij przycisk **Zarejestruj się jako deweloper** i postępuj zgodnie z instrukcjami, aby zarejestrować.
3. Na **aplikacje** , kliknij pozycję **Utwórz nową aplikację**.

    ![Utwórz nową aplikację](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. Wprowadź **Nazwa aplikacji** i **kategorii**, następnie kliknij przycisk **tworzenie aplikacji**.

    To musi być unikatowa w serwisie Facebook. <strong>Namespace aplikacji</strong> to część adresu URL, które Twoja aplikacja będzie używać do dostępu do aplikacji usługi Facebook dla uwierzytelniania (na przykład https://apps.facebook.com/{App Namespace}). Jeśli nie określisz <strong>Namespace aplikacji</strong>, <strong>identyfikator aplikacji</strong> będą używane dla adresu URL. <strong>Identyfikator aplikacji</strong> jest liczbą długo generowanych przez system, która będzie widoczna w następnym kroku.

    ![Utwórz nową aplikację w oknie dialogowym](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. Przedstawia wyboru standardowych zabezpieczeń.

    ![Kontrola zabezpieczeń](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. Wybierz **ustawienia** na pasku menu po lewej stronie![ Pasek menu dewelopera usługi Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)
7. Na **podstawowe** sekcja ustawienia na stronie wybierz **dodać platformy** do określenia, że w przypadku dodawania aplikacji witryny sieci Web. ![Ustawienia podstawowe](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)
8. Wybierz **witryny sieci Web** spośród dostępnych platform.  
  
    ![Wybór platformy](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. Zanotuj Twojej **identyfikator aplikacji** i **klucz tajny aplikacji** , w którym można dodawać zarówno do aplikacji MVC w dalszej części tego samouczka. Ponadto Dodaj adres URL witryny (`https://localhost:44300/`) do testowania aplikacji MVC. Ponadto Dodaj **E-mail kontaktu**. Następnie wybierz opcję **Zapisz zmiany**.   

    ![Strony szczegółów aplikacji w warstwie podstawowa](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > Należy zauważyć, że tylko będzie można przeprowadzić uwierzytelniania za pomocą aliasu poczty e-mail, który został zarejestrowany. Inni użytkownicy i testowe konta nie będzie mógł się zarejestrować. Można przyznać dostęp kont innych Facebook do aplikacji w usłudze Facebook **ról Developer** kartę.
10. W programie Visual Studio Otwórz *aplikacji\_Start\Startup.Auth.cs*.
11. Skopiuj i Wklej **AppId** i **klucz tajny aplikacji** do `UseFacebookAuthentication` metody. **AppId** i **klucz tajny aplikacji** poniższe wartości są przykłady i nie będzie działać.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. Kliknij przycisk **zapisać zmiany**.
13. Naciśnij klawisz **CTRL + F5** do uruchomienia aplikacji.


Wybierz **Zaloguj** do wyświetlenia strony logowania. Kliknij przycisk **Facebook** w obszarze **Zaloguj się za pomocą innej usługi.**

Wprowadź swoje poświadczenia usługi Facebook.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

Pojawi się monit o udzielenie uprawnień do dostępu do profilu publicznego i friend listy aplikacji.

![Szczegóły aplikacji usługi Facebook](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

Użytkownik jest obecnie zalogowany. Możesz teraz zarejestrować tego konta w aplikacji.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

Podczas rejestrowania, wpis jest dodawany do *użytkowników* tabeli członkostwa bazy danych.

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Sprawdzanie danych członkostwa

W **widoku** menu, kliknij przycisk **Eksploratora serwera**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Rozwiń węzeł **parametru DefaultConnection (MvcAuth)**, rozwiń węzeł **tabel**, kliknij prawym przyciskiem myszy **AspNetUsers** i kliknij przycisk **Pokaż dane tabeli**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dane tabeli aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Dodawanie danych profilu do klasy użytkownika

W tej sekcji zostanie dodana daty urodzenia i miejscowość macierzystego do danych użytkownika podczas rejestracji, jak pokazano na poniższej ilustracji.

![zwrócona z macierzystego miejscowość i Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Otwórz *Models\IdentityModels.cs* plik i dodać właściwości miejscowość daty i w domu urodzenia:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Otwórz *Models\AccountViewModels.cs* plików i zestaw urodzenia daty i w domu właściwości Miejscowość w `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Otwórz *Controllers\AccountController.cs* pliku i Dodaj kod dla daty i w domu mieście urodzenia w `ExternalLoginConfirmation` metody akcji, jak pokazano:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Dodaj daty urodzenia i macierzystego mieście *Views\Account\ExternalLoginConfirmation.cshtml* pliku:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Aby można było ponownie zarejestrować konto usługi Facebook z aplikacją i sprawdź, czy można dodać nowego Data urodzenia i informacje o profilu miejscowość macierzystego, należy usunąć bazy danych członkostwa.

Z **Eksploratora rozwiązań**, kliknij przycisk **Pokaż wszystkie pliki** ikonę, a następnie kliknij prawym przyciskiem myszy *Dodaj\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* i kliknij przycisk **usunąć**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Z **narzędzia** menu, kliknij przycisk **Menedżer pakietów NuGet**, następnie kliknij przycisk **Konsola Menedżera pakietów** (PMC). Wpisz następujące polecenia w kryterium.

1. Enable-Migrations
2. Init dodać migracji
3. Update-Database

Uruchom aplikację i zaloguj się i rejestrować w przypadku niektórych użytkowników za pomocą usługi FaceBook i Google.

## <a name="examine-the-membership-data"></a>Sprawdzanie danych członkostwa

W **widoku** menu, kliknij przycisk **Eksploratora serwera**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Kliknij prawym przyciskiem myszy **AspNetUsers** i kliknij przycisk **Pokaż dane tabeli**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown` i `BirthDate` pola są wyświetlane poniżej.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Rejestrowanie aplikacji i logowania za pomocą innego konta

Jeśli logowania do aplikacji z usługą Facebook, a następnie wyloguj się i spróbuj zalogować się ponownie za pomocą innego konta usługi Facebook (przy użyciu tej samej przeglądarce), użytkownik będzie można natychmiast zalogowany do poprzedniego konta usługi Facebook, używanego. Aby użyć innego konta, należy przejść do usługi Facebook i wylogowania w serwisie Facebook. Tę samą zasadę stosuje się do żadnych innych 3 strona dostawcy uwierzytelniania. Można też, zaloguj się za pomocą innego konta, przy użyciu innej przeglądarki.

## <a name="next-steps"></a>Następne kroki

Zobacz [wprowadzenie dostawcy zabezpieczeń Yahoo i LinkedIn OAuth dla OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) przez Jerrie Pelser instrukcje Yahoo i LinkedIn. Zobacz na Jerrie Pretty przycisków społecznościowych logowania dla platformy ASP.NET MVC 5 uzyskanie Włączanie logowania społecznościowych przycisków.

Wykonaj czynności opisane w samouczku Mój [tworzenie aplikacji ASP.NET MVC z uwierzytelniania i bazy danych SQL i wdrożyć w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), które nadal tego samouczka i zawiera następujące:

1. Jak wdrożyć aplikację na platformie Azure.
2. Jak zabezpieczyć użytkownik aplikacji z rolami.
3. Jak zabezpieczyć aplikacji za pomocą [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) i [autoryzacji](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtrów.
4. Jak używać interfejs API członkostwa, aby dodać użytkowników i ról.

Wystaw opinię na jak zbędne tego samouczka i co można możemy ulepszyć. Możesz również poprosić o nowe tematy w [Pokaż mnie jak z kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Można nawet uzyskać oraz oddawać głosy na nowe funkcje do dodania do programu ASP.NET. Na przykład można głosować narzędzia do [tworzenie i zarządzanie użytkownikami i rolami.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Dobry opis działania zewnętrznych usług uwierzytelniania ASP.NET, zobacz Roberta Mcmurraya [zewnętrznych usług uwierzytelniania](https://asp.net/web-api/overview/security/external-authentication-services). Artykuł Roberta również przechodzi w stan szczegółów w włączania uwierzytelniania firmy Microsoft i Twitter. Tomasz Dykstra obiektu znakomity [samouczek EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) pokazano, jak pracować z programu Entity Framework.
