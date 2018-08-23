---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: Utwórz MVC 5 aplikacji za pomocą usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego (C#) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym samouczku dowiesz się, jak utworzyć aplikację sieci web ASP.NET MVC 5, która umożliwia użytkownikom zalogowanie się przy użyciu protokołu OAuth 2.0 przy użyciu poświadczeń z zewnętrznego typ uwier...
ms.author: riande
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 330cb290668ae951e822b95990ed92100b790cd5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41756912"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a>Tworzenie aplikacji ASP.NET MVC 5 z usługi Facebook, Twitter, LinkedIn i Google OAuth2 logowania jednokrotnego (C#)
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> W tym samouczku dowiesz się, jak utworzyć aplikację sieci web ASP.NET MVC 5, która pozwala użytkownikom na logowanie za pomocą [OAuth 2.0](http://oauth.net/2/) przy użyciu poświadczeń z zewnętrznego dostawcę uwierzytelniania, takich jak Facebook, Twitter, LinkedIn, Microsoft lub Google. Dla uproszczenia ten samouczek koncentruje się na pracy przy użyciu poświadczeń usług Facebook i Google.
> 
> Włączanie tych poświadczeń w witrynach sieci web zapewnia znaczące korzyści, ponieważ milionom użytkowników mają już konta z tych dostawców zewnętrznych. Tacy użytkownicy mogą być bardziej skłonni się zarejestrować dla danej witryny, gdy ta osoba nie ma do tworzenia i Zapamiętaj nowego zestawu poświadczeń.
> 
> Zobacz też [aplikacji ASP.NET MVC 5 z wiadomości SMS i wiadomości e-mail uwierzytelniania dwuskładnikowego](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).
> 
> W samouczku opisano również sposób dodać dane profilowe dla użytkownika oraz Dodawanie ról za pomocą interfejsu API członkostwa. Ten samouczek został napisany przez [Rick Anderson](https://blogs.msdn.com/rickAndy) (postępuj zgodnie ze mną w serwisie Twitter: [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) ).


<a id="start"></a>
## <a name="getting-started"></a>Wprowadzenie

Rozpocznij od instalowania i uruchamiania [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) lub [programu Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566). Zainstaluj program Visual Studio [2013 z aktualizacją 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej. Aby uzyskać pomoc dotyczącą usługi Dropbox, GitHub, Linkedin, Instagram, buforu, Salesforce, pary, Stack Exchange, Tripit, Twitch, Twitter, Yahoo! i więcej, zobacz ten [przykładowy projekt](https://github.com/matthewdunsdon/oauthforaspnet).

> [!NOTE]
> Należy zainstalować program Visual Studio [2013 z aktualizacją 3](https://go.microsoft.com/fwlink/?LinkId=390521) lub nowszej, do użycia Google protokołu OAuth 2 i Debuguj lokalnie, bez ostrzeżeń protokołu SSL.


Kliknij przycisk **nowy projekt** z **Start** strony, lub można użyć menu i wybrać **pliku**, a następnie **nowy projekt**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a>Tworzenie pierwszej aplikacji

Kliknij przycisk **nowy projekt**, a następnie wybierz **Visual C#** po lewej stronie, następnie **Web** , a następnie wybierz **aplikacji sieci Web ASP.NET**. Nazwij swój projekt "MvcAuth", a następnie kliknij przycisk **OK**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

W **nowy projekt ASP.NET** okno dialogowe, kliknij przycisk **MVC**. Jeśli uwierzytelnianie nie jest **indywidualne konta użytkowników**, kliknij przycisk **Zmień uwierzytelnianie** i wybrać **indywidualne konta użytkowników**. Sprawdzając **Hostuj w chmurze**, aplikacja będzie hostować na platformie Azure jest bardzo proste.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

W przypadku wybrania **Hostuj w chmurze**, wykonaj okna dialogowego Konfigurowanie.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a>Użyj pakietu NuGet, aby zaktualizować do najnowsze oprogramowanie pośredniczące OWIN

Użyj Menedżera pakietów NuGet, aby zaktualizować [oprogramowania pośredniczącego OWIN](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md). Wybierz **aktualizacje** w menu po lewej stronie. Możesz kliknąć **Aktualizuj wszystkie** przycisku, lub możesz wyszukiwać tylko pakiety OWIN (pokazane na następnej ilustracji):

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

Na poniższej ilustracji przedstawiono tylko OWIN pakiety:

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

W konsoli Menedżera pakietów (PMC), możesz wprowadzić `Update-Package` polecenia, które spowoduje zaktualizowanie wszystkich pakietów.

Naciśnij klawisz **F5** lub **kombinację klawiszy Ctrl + F5** do uruchomienia aplikacji. Na poniższej ilustracji numer portu to 1234. Po uruchomieniu aplikacji, zobaczysz inny numer portu.

W zależności od rozmiaru okna przeglądarki, konieczne może być kliknij ikonę nawigacji, aby zobaczyć **Home**, **o**, **skontaktuj się z pomocą**, **zarejestrować**i **Zaloguj** łącza.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a>Konfigurowanie protokołu SSL w projekcie

Aby połączyć z dostawców uwierzytelniania, takich jak serwis Google czy Facebook, należy skonfigurować usługi IIS Express do używania protokołu SSL. Jest ważne zachować, po zalogowaniu przy użyciu protokołu SSL i nie porzucić powrót do protokołu HTTP, Twoje plik cookie logowania jest po prostu jako klucz tajny, jako nazwę użytkownika i hasło, a także bez korzystania z protokołu SSL jest wysyłana w postaci zwykłego tekstu w sieci. Poza już wykonanych podczas wykonywania uzgadnianie i zabezpieczenia kanału (jest to duża część co sprawia, że wolniej niż HTTP na HTTPS) zanim potok MVC jest uruchamiany, więc przekierowywanie do protokołu HTTP, po użytkownik zalogował się nie będzie wprowadzenia, bieżącego żądania lub przyszłość żądania znacznie szybciej.

1. W **Eksploratora rozwiązań**, kliknij przycisk **MvcAuth** projektu.
2. Naciśnij klawisz F4, aby wyświetlić właściwości projektu. Alternatywnie z **widoku** menu, możesz wybrać **okno właściwości**.
3. Zmiana **włączony protokół SSL** na wartość True.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. Skopiuj adres URL protokołu SSL (który będzie stanowić `https://localhost:44300/` chyba, że utworzono inne projekty, protokołu SSL).
5. W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy **MvcAuth** projektu, a następnie wybierz **właściwości**.
6. Wybierz **Web** kartę, a następnie wklej adres URL protokołu SSL do **adres Url projektu** pole. Zapisz plik (listy Ctl + S). Konieczne będzie ten adres URL do skonfigurowania aplikacji uwierzytelniania serwisu Facebook i Google.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. Dodaj [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) atrybutu `Home` kontrolera, aby wymagać wszystkie żądania muszą używać protokołu HTTPS. Bardziej bezpieczną metodą jest dodanie [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filtru do aplikacji. Zobacz sekcję &quot;ochrona aplikacji przy użyciu protokołu SSL i autoryzować atrybut&quot; w mojej tutoral [tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL i wdrażanie w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data). Poniżej przedstawiono część kontrolera głównego.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. Naciśnij klawisze CTRL + F5, aby uruchomić aplikację. Jeśli certyfikat został zainstalowany w przeszłości, możesz pominąć pozostałą część tej sekcji i przejdź do [tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji do projektu](#goog), w przeciwnym razie wykonaj instrukcje zaufania z podpisem własnym certyfikat, który wygenerował usług IIS Express.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. Odczyt **ostrzeżenie o zabezpieczeniach** okna dialogowego, a następnie kliknij przycisk **tak** Jeśli chcesz zainstalować certyfikat reprezentujący localhost.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. Pokazuje, IE *Home* strony, a Brak ostrzeżeń protokołu SSL.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. Google Chrome również zaakceptuje certyfikat i wyświetli zawartość HTTPS bez ostrzeżenia. Firefox używa magazynu certyfikatów, dzięki czemu zostanie wyświetlone ostrzeżenie. W przypadku naszej aplikacji można bezpiecznie kliknąć **rozumiem ryzyko**.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a>Tworzenie aplikacji Google dla protokołu OAuth 2 i łączenie aplikacji do projektu

> [!WARNING]
> Bieżący protokołu Google OAuth instrukcje można znaleźć [konfigurowania uwierzytelniania serwisu Google w programie ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).

1. Przejdź do [konsoli deweloperów Google](https://console.developers.google.com/).
2. Jeśli nie utworzono projektu przed wybierz **poświadczenia** w karcie po lewej stronie, a następnie wybierz **Utwórz**.
3. Na karcie po lewej stronie kliknij **poświadczenia**.
4. Kliknij przycisk **Utwórz poświadczenia** następnie **identyfikator klienta OAuth**. 

    1. W **Utwórz identyfikator klienta** okno dialogowe, zachowaj ustawienie domyślne **aplikacji sieci Web** dla typu aplikacji.
    2. Ustaw **autoryzacji JavaScript** źródła do adresu URL protokołu SSL, które zostały użyte powyżej (`https://localhost:44300/` chyba, że utworzono inne projekty, protokołu SSL)
    3. Ustaw **identyfikator URI przekierowania autoryzowanych** do:  
         `https://localhost:44300/signin-google`
5. Kliknij element menu ekran zgody OAuth, a następnie ustaw swoje wiadomości e-mail adres i nazwy produktu. Po zakończeniu kliknij przycisk formularza **Zapisz**.
6. Kliknij element menu, biblioteki, wyszukać **interfejsu API Google +**, kliknij go, a następnie naciśnij przycisk Włącz.
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   Na poniższym obrazie przedstawiono włączonych interfejsów API.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. Odwiedź stronę z Google APIs API Manager **poświadczenia** kartę, aby uzyskać **identyfikator klienta**. Pobierz, aby zapisać plik w formacie JSON przy użyciu kluczy tajnych aplikacji. Skopiuj i Wklej **ClientId** i **ClientSecret** do `UseGoogleAuthentication` znaleźć metodę w *Startup.Auth.cs* w pliku *App_Start* folderu. **ClientId** i **ClientSecret** wartości podanych poniżej są przykłady i nie będą działać.

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > Zabezpieczenia — nigdy nie przechowywania danych poufnych w kodzie źródłowym. Konto i poświadczenia są dodawane do kodu powyżej, aby uprościć przykład. Zobacz [najlepsze rozwiązania dotyczące wdrażania haseł i innych danych poufnych na platformie ASP.NET i usłudze Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
8. Naciśnij klawisz **kombinację klawiszy CTRL + F5** Aby skompilować i uruchomić aplikację. Kliknij przycisk **Zaloguj** łącza.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. W obszarze **Zaloguj się za pomocą innej usługi**, kliknij przycisk **Google**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > Jeśli pominiesz dowolnego z powyższych czynności zostanie wyświetlony błąd HTTP 401. Ponownie sprawdź powyższe kroki. Jeśli pominiesz wymaganego ustawienia (na przykład **nazwa produktu**), Dodaj brakującego elementu i zapisywanie; może potrwać kilka minut, zanim prawidłowego działania uwierzytelniania.
10. Nastąpi przekierowanie do witryny Google, w którym należy wprowadzić poświadczenia.   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. Po wprowadzeniu poświadczeń, pojawi się monit do nadawania uprawnień do aplikacji sieci web, który został utworzony:
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. Kliknij przycisk **zaakceptować**. Teraz nastąpi przekierowanie do **zarejestrować** strony aplikacji MvcAuth, gdy zarejestrujesz swoje konto Google. Masz możliwość zmiany nazwy rejestracji lokalny adres e-mail używany dla tego konta usługi Gmail, ale zazwyczaj chcesz zachować aliasu adresu e-mail domyślne (czyli ten, który używany do uwierzytelniania). Kliknij przycisk **zarejestrować**.  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a>Tworzenie aplikacji w usłudze Facebook i łączenie aplikacji do projektu

> [!WARNING]
> Bieżący Facebook OAuth2 uwierzytelniania instrukcje można znaleźć [uwierzytelnianie Konfigurowanie serwisu Facebook](/aspnet/core/security/authentication/social/facebook-logins)


<a id="mdb"></a>
## <a name="examine-the-membership-data"></a>Sprawdzanie danych członkostwa

W **widoku** menu, kliknij przycisk **Eksploratora serwera**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

Rozwiń **DefaultConnection (MvcAuth)**, rozwiń węzeł **tabel**, kliknij prawym przyciskiem myszy **AspNetUsers** i kliknij przycisk **Pokaż dane tabeli**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![dane tabeli aspnetusers](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a>Dodawanie danych profilu do klasy użytkownika

W tej sekcji dodasz datę urodzin i macierzystego miejscowość danych użytkownika podczas rejestracji, jak pokazano na poniższej ilustracji.

![reg za pomocą macierzystego miejscowości i Bday](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

Otwórz *Models\IdentityModels.cs* pliku i Dodaj właściwości miejscowość strona główna i daty urodzenia:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

Otwórz *Models\AccountViewModels.cs* plików i zestaw urodzenia daty i w domu właściwości Miejscowość w `ExternalLoginConfirmationViewModel`.

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

Otwórz *Controllers\AccountController.cs* pliku, a następnie dodaj kod miejscowości strona główna i daty urodzenia w `ExternalLoginConfirmation` metody akcji, jak pokazano:

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

Dodaj datę urodzenia i macierzystego mieście *Views\Account\ExternalLoginConfirmation.cshtml* pliku:

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

Usuń bazę danych członkostwa, dzięki czemu możesz ponownie się zarejestrować Twojego konta serwisu Facebook ze swoją aplikacją i sprawdź, czy można dodać nową datę urodzenia i informacje o profilu macierzystego miejscowości.

Z **Eksploratora rozwiązań**, kliknij przycisk **Pokaż wszystkie pliki** ikonę, a następnie kliknij prawym przyciskiem myszy *Dodaj\_Data\aspnet-MvcAuth -&lt;dateStamp&gt;.mdf* i kliknij przycisk **Usuń**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

Z **narzędzia** menu, kliknij przycisk **Menedżera pakietów NuGet**, następnie kliknij przycisk **Konsola Menedżera pakietów** (PMC). Wprowadź następujące polecenia w konsoli zarządzania Pakietami.

1. Enable-Migrations
2. Dodaj migracji Init
3. Aktualizuj bazy danych

Uruchom aplikację, a następnie zaloguj się i rejestrować niektórych użytkowników za pomocą usługi FaceBook i Google.

## <a name="examine-the-membership-data"></a>Sprawdzanie danych członkostwa

W **widoku** menu, kliknij przycisk **Eksploratora serwera**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

Kliknij prawym przyciskiem myszy **AspNetUsers** i kliknij przycisk **Pokaż dane tabeli**.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

`HomeTown` i `BirthDate` pola zostały wymienione poniżej.

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a>Rejestrowanie aplikacji i logując się przy użyciu innego konta

Jeśli Zaloguj się do aplikacji za pomocą usługi Facebook, a następnie zaloguj i spróbuj zalogować się ponownie z innym kontem serwisu Facebook (przy użyciu tej samej przeglądarki), można będzie natychmiast zalogowanie do poprzedniego konta Facebook, którego użyto. Aby użyć innego konta, musisz przejść do usługi Facebook i wylogowania w serwisie Facebook. Ta sama zasada dotyczy do wszelkich innych 3 strony dostawcy uwierzytelniania. Możesz też zalogować się przy użyciu innego konta przy użyciu innej przeglądarki.

## <a name="next-steps"></a>Następne kroki

Zobacz [wprowadzenie do dostawców zabezpieczeń Yahoo i LinkedIn OAuth dla OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) przez Jerrie Pelser instrukcje dotyczące Yahoo i LinkedIn. Zobacz firmy Jerrie wygląda przyciski społecznościowych logowania dla platformy ASP.NET MVC 5 można pobrać Włącz logowanie społecznościowych przycisków.

Postępuj zgodnie z samouczkiem Moje [tworzenie aplikacji ASP.NET MVC z uwierzytelnianiem i bazą danych SQL i wdrażanie w usłudze Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), co będzie się powtarzać tego samouczka i zawiera następujące czynności:

1. Jak wdrożyć aplikację na platformie Azure.
2. Jak zabezpieczyć aplikację przy użyciu ról.
3. Jak zabezpieczyć swoją aplikację przy użyciu [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) i [Autoryzuj](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filtrów.
4. Jak użyć członkostwo interfejsu API, aby dodać użytkowników i ról.

Jak się podoba w tym samouczku, i co można było ulepszyć proces Wystaw opinię. Możesz również poprosić o nowe tematy w [Pokaż mi jak za pomocą kodu](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code). Można również poprosić o i oddawać głosy na nowe funkcje, które mają zostać dodane do programu ASP.NET. Na przykład, możesz głosować na narzędzie do [tworzenie i zarządzanie użytkownikami i rolami.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)

Dobry opis działania zewnętrznych usług uwierzytelniania platformy ASP.NET, zobacz Robert McMurray [zewnętrznych usług uwierzytelniania](https://asp.net/web-api/overview/security/external-authentication-services). Artykuł Roberta również przechodzi w stan szczegółów włączania uwierzytelniania firmy Microsoft i Twitter. Tom Dykstra firmy doskonale [samouczek programu EF/MVC](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) przedstawiono sposób pracy z platformą Entity Framework.
