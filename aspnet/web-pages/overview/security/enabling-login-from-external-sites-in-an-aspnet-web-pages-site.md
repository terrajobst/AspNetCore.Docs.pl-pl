---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Logowanie się przy użyciu zewnętrznych witryn we wzorcu ASP.NET Web Pages lokacji (Razor) | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: W tym artykule wyjaśniono, jak zalogować się do witryny ASP.NET Web Pages (Razor) przy użyciu usługi Facebook, Google, Twitter, Yahoo i innych witryn — oznacza to, jak obsługiwać...
ms.author: riande
ms.date: 02/21/2014
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 188a9203ba7b04f5a88d0f802f1a05bf35d58d8c
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021134"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Logowanie się przy użyciu zewnętrznych witryn w witrynie ASP.NET Web Pages (Razor)
====================
przez [Tom FitzMacken](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak zalogować się do witryny ASP.NET Web Pages (Razor) przy użyciu usługi Facebook, Google, Twitter, Yahoo i innych witryn — oznacza to, jak obsługiwać OAuth i OpenID w lokacji.
> 
> Zawartość:
> 
> - Jak włączyć logowania z innych witryn, w przypadku użycia szablonu witryny początkowej programu WebMatrix.
> 
> Jest to ASP.NET wprowadzonej w artykule:
> 
> - `OAuthWebSecurity` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Wersje oprogramowania używanego w tym samouczku
> 
> 
> - ASP.NET Web Pages (Razor) 2
> - Program WebMatrix 3

ASP.NET Web Pages obsługuje [OAuth](http://oauth.net/) i [OpenID](http://openid.net/) dostawców. Korzystając z tych dostawców, można pozwolić użytkownikom logowanie do witryny przy użyciu istniejących poświadczeń usług Facebook, Twitter, Microsoft i Google. Na przykład aby zalogować się przy użyciu konta w serwisie Facebook, użytkowników po prostu wybrać ikonę usługi Facebook, który przekierowuje go do strony logowania usługi Facebook, gdzie użytkownik podał ich informacji o użytkowniku. Można następnie skojarzyć logowania do usługi Facebook przy użyciu swojego konta w witrynie. Powiązane rozszerzenie do funkcji przynależności stron sieci Web jest czy użytkownicy mogą powiązać logowania (w tym logowania z witrynami sieci społecznościowych) za pomocą jednego konta w witrynie sieci Web.

Ta ilustracja przedstawia strony logowania z **witryny początkowej** szablonu, w którym użytkownik może wybrać ikonę usługi Facebook, Twitter, Google i Microsoft, aby włączyć logowanie przy użyciu zewnętrznego konta:

![dostawców zewnętrznych](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Aby umożliwić członkostwa OAuth i OpenID, uncommenting kilku wierszy kodu w **witryny początkowej** szablonu. Metody i właściwości można używać do pracy z uwierzytelniania OAuth i OpenID dostawców znajdują się w `WebMatrix.Security.OAuthWebSecurity` klasy. **Witryny początkowej** szablon zawiera następujące elementy infrastruktury pełnego członkostwa, ze strony logowania, bazy danych członkostwa i cały kod należy umożliwić użytkownikom logowanie do witryny przy użyciu poświadczeń lokalnych lub z innej lokacji .

Ta sekcja zawiera przykładowy sposób zezwolić użytkownikom na logowanie z zewnętrznych witryn, witryny, która opiera się na **witryny początkowej** szablonu. Po utworzeniu witryny początkowej, należy wykonać ten (szczegóły poniżej):

- Dla witryn, które korzystają z dostawcy uwierzytelniania OAuth (Facebook, Twitter i Microsoft) utworzyć aplikację w witrynie zewnętrznej. Dzięki temu klucze aplikacji, które należy w celu wywołania funkcji logowania dla tych witryn.
- Dla witryn, które korzystają z dostawcy openid o nazwie (Google) nie masz do tworzenia aplikacji. Dla wszystkich tych lokacji musi mieć konto, aby się zalogować i tworzenie aplikacji dla deweloperów.

    > [!NOTE]
    > Aplikacje firmy Microsoft Zaakceptuj, tylko adres URL na żywo roboczej witryny sieci Web, adres URL lokalną witrynę sieci Web nie można używać do testowania nazwy logowania.
- Edytuj kilka plików w witrynie sieci Web, aby określić dostawcę uwierzytelniania i przesłanie logowania do witryny, do której chcesz użyć.

Ten artykuł zawiera instrukcje oddzielne w celu uwzględnienia poniższych zadań:

- [Włączanie logowania Google](#To_enable_Google_logins)
- [Włączanie logowania do usługi Facebook](#To_enable_Facebook_logins)
- [Włączanie logowania do usługi Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Włączanie logowania Google

1. Utwórz lub Otwórz witrynę stron ASP.NET Web Pages, który jest oparty na szablonie witryny początkowej programu WebMatrix.
2. Otwórz  *\_AppStart.cshtml* strony, a następnie usuń komentarz następujący wiersz kodu. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Testowanie logowania usługi Google

1. Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **Zaloguj** przycisku.
2. Na *logowania* stronie **Zaloguj się za pomocą innej usługi** sekcji, wybierają **Google** lub **Yahoo** przycisk Prześlij. W tym przykładzie użyto logowania usługi Google. 

    Strony sieci web przekierowuje żądanie do strony logowania firmy Google.

    ![Konta Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Wprowadź poświadczenia dla istniejącego konta Google.
4. Jeśli Google zapyta, czy chcesz zezwolić na *Localhost* mają być używane informacje z konta, kliknij przycisk **Zezwalaj**.

    Kod używa tokenu Google, aby uwierzytelnić użytkownika, a następnie wróci do tej strony w witrynie sieci Web. Ta strona umożliwia kojarzenie ich logowania usługi Google przy użyciu istniejącego konta w witrynie sieci Web lub mogą rejestrować nowe konto w witrynie do skojarzenia zewnętrznych danych logowania za pomocą.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Wybierz **skojarzyć** przycisku. Zwraca przeglądarce do strony głównej aplikacji.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Włączanie logowania do usługi Facebook

1. Przejdź do [Facebook deweloperów witryny](https://developers.facebook.com/apps) (Zaloguj się, jeśli jeszcze nie zalogowano Cię).
2. Wybierz **Utwórz nową aplikację** przycisk, a następnie postępuj zgodnie z monitami, aby utworzyć nową aplikację.
3. W sekcji **wybierz, jak Twoja aplikacja integruje się z usługi Facebook**, wybierz **witryny sieci Web** sekcji.
4. Wypełnij **adres URL witryny** pole adres URL witryny (na przykład `http://www.example.com`). **Domeny** pole jest opcjonalne; służy to do zapewnienia uwierzytelniania dla całej domeny (takie jak *example.com*). 

    > [!NOTE]
    > Jeśli używasz witryny na komputerze lokalnym przy użyciu adresu URL, takich jak `http://localhost:12345` (których liczba jest numer portu lokalnego), można dodać tę wartość, aby **adres URL witryny** pola do testowania witryny. Jednak każdy razem numer portu zmiany lokacji lokalnej, należy zaktualizować **adres URL witryny** pole aplikacji.
5. Wybierz **Zapisz zmiany** przycisku.
6. Wybierz **aplikacje** karcie ponownie, a następnie wyświetlić stronę początkową dla aplikacji.
7. Kopiuj **Identyfikatora aplikacji** i **klucz tajny aplikacji** wartości dla swojej aplikacji i wklej je do pliku tekstowego tymczasowych. W kodzie witryny sieci Web przekazuje te wartości do dostawcy usługi Facebook.
8. Zakończ działanie witryny dewelopera usługi Facebook.

Teraz możesz dokonać zmian dwie strony w witrynie sieci Web tak, aby użytkownicy będą mogli zalogować się do witryny za pomocą swoich kont usługi Facebook.

1. Utwórz lub Otwórz witrynę stron ASP.NET Web Pages, który jest oparty na szablonie witryny początkowej programu WebMatrix.
2. Otwórz  *\_AppStart.cshtml* strony, a następnie usuń komentarz kodu dla dostawcy uwierzytelniania Facebook OAuth. Blok kodu pozbawionym znaków komentarza wierszu wygląda podobnie do poniższego: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Kopiuj **Identyfikatora aplikacji** wartości z aplikacji usługi Facebook jako wartość `appId` parametru (wewnątrz znaków cudzysłowu).
4. Kopiuj **klucz tajny aplikacji** wartość z aplikacji usługi Facebook jako `appSecret` wartość parametru.
5. Zapisz i zamknij plik.

### <a name="testing-facebook-login"></a>Testowanie logowania do usługi Facebook

1. Uruchamianie witryny *default.cshtml* strony i wybierz **logowania** przycisku.
2. Na *logowania* stronie **Zaloguj się za pomocą innej usługi** wybierz pozycję **Facebook** ikony. 

    Strony sieci web przekierowuje żądanie do strony logowania usługi Facebook.

    ![protokołu OAuth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Zaloguj się do konta w serwisie Facebook. 

    Kod używa tokenu usługi Facebook do uwierzytelniania, a następnie wróci do strony umożliwiające powiązanie logowania do usługi Facebook przy logowaniu do witryny. Adres nazwy lub adresu e-mail użytkownika jest wypełniony do **E-mail** pola w formularzu.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Wybierz **skojarzyć** przycisku. 

    Przeglądarka zwraca do strony głównej, a użytkownik jest zalogowany.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Włączanie logowania do usługi Twitter

1. Przejdź do [Twitter deweloperów witryny](https://dev.twitter.com/).
2. Wybierz **tworzenie aplikacji** łącze, a następnie zaloguj się do witryny.
3. Na **tworzenia aplikacji** formularza, wypełnij **nazwa** i **opis** pola.
4. W **witryny sieci Web** wprowadź adres URL witryny (na przykład `http://www.example.com`). 

    > [!NOTE]
    > Jeśli testujesz witryny sieci lokalnie (przy użyciu adresu URL typu `http://localhost:12345`), Twitter, może nie zaakceptować adresu URL. Jednak można używać adresu IP lokalnego sprzężenia zwrotnego (na przykład `http://127.0.0.1:12345`). Upraszcza to proces testowania aplikacji w środowisku lokalnym. Jednak za każdym razem, gdy zmieni się numer portu lokacji lokalnej, należy zaktualizować **witryny sieci Web** pole aplikacji.
5. W **adresów URL wywołania zwrotnego** wprowadź adres URL dla strony witryny sieci Web, który ma użytkowników, aby powrócić do po zalogowaniu się do usługi Twitter. Na przykład wysłać do użytkowników na stronę główną witryny Starter (który będzie także rozpoznawał ich stan zalogowany), wprowadź ten sam adres URL, które wprowadziłeś w **witryny sieci Web** pola.
6. Zaakceptuj warunki i wybierz polecenie **tworzenie aplikacji usługi Twitter** przycisku.
7. Na **Moje aplikacje** początkowej strony, wybierz utworzoną aplikację.
8. Na **szczegóły** karty, przewiń w dół i wybierz **Utwórz Mój Token dostępu** przycisku.
9. Na **szczegóły** kartę, skopiuj **konsumenta** i **klucz tajny klienta** wartości dla swojej aplikacji i wklej je do pliku tekstowego tymczasowych. W kodzie witryny sieci Web będzie przekazać te wartości do dostawcy usługi Twitter.
10. Zakończ pracę w witrynie Twitter.

Teraz możesz dokonać zmian dwie strony w witrynie sieci Web tak, aby użytkownicy będą mogli zalogować się do witryny za pomocą swoich kont usługi Twitter.

1. Utwórz lub Otwórz witrynę stron ASP.NET Web Pages, który jest oparty na szablonie witryny początkowej programu WebMatrix.
2. Otwórz  *\_AppStart.cshtml* strony, a następnie usuń komentarz kodu dla dostawcy OAuth w usłudze Twitter. Blok pozbawionym znaków komentarza wierszu kodu wygląda następująco: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Kopiuj **konsumenta** wartość z aplikacji usługi Twitter jako wartość `consumerKey` parametru (wewnątrz znaków cudzysłowu).
4. Kopiuj **klucz tajny klienta** wartość z aplikacji usługi Twitter jako wartość `consumerSecret` parametru.
5. Zapisz i zamknij plik.

### <a name="testing-twitter-login"></a>Testowanie logowania usługi Twitter

1. Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **logowania** przycisku.
2. Na *logowania* stronie **Zaloguj się za pomocą innej usługi** wybierz pozycję **Twitter** ikony. 

    Strony sieci web przekierowuje żądanie do strony logowania usługi Twitter dla aplikacji, który został utworzony.

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Zaloguj się do konta w serwisie Twitter.
4. Kod używa token usługi Twitter w celu uwierzytelnienia użytkownika, a następnie powrót do strony umożliwiające powiązanie identyfikatora logowania przy użyciu konta z witryny sieci Web. Twoja nazwa lub adres e-mail jest wypełniana w **E-mail** pola w formularzu.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Wybierz **skojarzyć** przycisku. 

    Przeglądarka zwraca do strony głównej, a użytkownik jest zalogowany.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


- [Dostosowywanie zachowania dla całej witryny](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Dodawanie zabezpieczeń i członkostwa w witrynie ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkID=202904)
