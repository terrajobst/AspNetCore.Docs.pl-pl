---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: Zalogował się przy użyciu zewnętrznych witryn w sieci Web ASP.NET stron witryny (Razor) | Dokumentacja firmy Microsoft
author: tfitzmac
description: W tym artykule wyjaśniono, jak zalogować się do witryny stron sieci Web platformy ASP.NET (Razor) za pomocą usługi Facebook, Google, Twitter, Yahoo i innych witryn, oznacza to, jak obsługiwać...
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
ms.locfileid: "26573356"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a>Rejestrowanie przy użyciu zewnętrznych witryn w sieci Web platformy ASP.NET (Razor) stron witryny
====================
przez [FitzMacken niestandardowy](https://github.com/tfitzmac)

> W tym artykule wyjaśniono, jak zalogować się do witryny stron sieci Web platformy ASP.NET (Razor) za pomocą usługi Facebook, Google, Twitter, Yahoo i innych witryn, oznacza to, jak obsługiwać OAuth i OpenID w witrynie.
> 
> Zawartość:
> 
> - Jak włączyć logowanie z innych lokacji przy użyciu szablonu programu WebMatrix witryny początkowej.
> 
> Jest to funkcja ASP.NET, wprowadzona w artykule:
> 
> - `OAuthWebSecurity` Pomocnika.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Używane w samouczku wersje oprogramowania
> 
> 
> - Strony sieci Web platformy ASP.NET (Razor) 2
> - Program WebMatrix 3

Strony ASP.NET Web Pages obsługuje [OAuth](http://oauth.net/) i [OpenID](http://openid.net/) dostawców. Przy użyciu tych dostawców, możesz pozwolić, aby użytkowników logowania do witryny przy użyciu swoich istniejących poświadczeń z usługi Facebook, Twitter, firma Microsoft i Google. Na przykład aby zalogować się przy użyciu konta usługi Facebook, użytkowników można po prostu ikonę usługi Facebook, który przekierowuje go do strony logowania usługi Facebook, którym oni wprowadzić swoje informacje o użytkowniku. Można następnie skojarzyć logowania serwisu Facebook do swojego konta w witrynie. Powiązane rozszerzenie do funkcji przynależności stron sieci Web jest czy użytkownicy mogą powiązać wiele logowań (w tym logowania z witrynami sieci społecznościowych) z jednego konta w witrynie sieci Web.

Ten obraz zawiera strony logowania z **witryny początkowej** szablon, w którym użytkownik może wybrać ikonę Facebook, Twitter, Google lub Microsoft, aby włączyć logowanie przy użyciu zewnętrznego konta:

![zewnętrznych dostawców](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

Można włączyć członkostwo OAuth i OpenID usunięcie komentarza kilku wierszy kodu w **witryny początkowej** szablonu. Metody i właściwości można używać do pracy z uwierzytelniania OAuth i OpenID dostawców znajdują się w `WebMatrix.Security.OAuthWebSecurity` klasy. **Witryny początkowej** szablon zawiera infrastruktury pełnego członkostwa, strony logowania, bazy danych członkostwa oraz cały kod należy powiadomić użytkowników logowania do witryny przy użyciu poświadczeń lokalnych lub z innej lokacji .

Ta sekcja zawiera przykładowy sposób umożliwić użytkownikom logowania z zewnętrznych witryn do lokacji, która jest oparta na **witryny początkowej** szablonu. Po utworzeniu witryny początkowej, możesz wykonać tego (szczegóły poniżej):

- Dla witryn, które używa dostawcy OAuth (Facebook, Twitter i firmy Microsoft) utworzyć aplikację w witrynie zewnętrznych. Udostępnia klucze aplikacji, które będą potrzebne, aby można było wywołać funkcję logowania dla tych witryn.
- Dla witryn, które korzystają z dostawcy uwierzytelniania OpenID (Google) nie trzeba tworzyć aplikacji. Wszystkie te witryny musi mieć konto, aby zalogować się i utworzyć aplikacjami dla deweloperów.

    > [!NOTE]
    > Aplikacje firmy Microsoft akceptować tylko na żywo adres URL witryny sieci Web pracy, więc nie można używać adresu URL lokalną witrynę sieci Web do testowania logowania.
- Edytuj kilka plików w witrynie sieci Web, aby określić dostawcę uwierzytelniania i przesłać dane logowania do witryny, której chcesz użyć.

Ten artykuł zawiera osobne instrukcje dotyczące następujących zadań:

- [Włączanie logowania do usługi Google](#To_enable_Google_logins)
- [Włączanie logowania do usługi Facebook](#To_enable_Facebook_logins)
- [Włączanie logowania do usługi Twitter](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a>Włączanie logowania do usługi Google

1. Utwórz lub Otwórz witrynę ASP.NET Web Pages opartego na szablonie witryny początkowej programu WebMatrix.
2. Otwórz  *\_AppStart.cshtml* strony i Usuń komentarz następujący wiersz kodu. 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a>Testowanie logowania Google

1. Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **Zaloguj** przycisku.
2. Na *logowania* strony w **Zaloguj się za pomocą innej usługi** albo wybierz **Google** lub **Yahoo** przycisk Prześlij. W tym przykładzie użyto logowania Google. 

    Strony sieci web przekierowuje żądanie do strony logowania usługi Google.

    ![Logowanie do usługi Google](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. Wprowadź poświadczenia dla istniejącego konta Google.
4. Jeśli Google zapyta, czy chcesz zezwolić *Localhost* Aby użyć informacji z konta, kliknij przycisk **Zezwalaj**.

    Kod używa tokenu Google do uwierzytelniania użytkownika, a następnie wróci do tej strony w witrynie sieci Web. Ta strona umożliwia użytkownikom skojarzyć ich Google logowania przy użyciu istniejącego konta w witrynie sieci Web lub ich zarejestrować nowe konto w witrynie do skojarzenia zewnętrznych danych logowania z.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. Wybierz **skojarzyć** przycisku. Zwraca przeglądarki do strony głównej aplikacji.

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a>Włączanie logowania do usługi Facebook

1. Przejdź do [lokacji deweloperzy Facebook](https://developers.facebook.com/apps) (dziennik, jeśli jeszcze nie jest zalogowany).
2. Wybierz **Utwórz nową aplikację** przycisk, a następnie postępuj zgodnie z monitami, aby utworzyć nową aplikację.
3. W sekcji **wybierz, jak aplikacja zostanie zintegrowana z usługą Facebook**, wybierz **witryny sieci Web** sekcji.
4. Wypełnij **adres URL witryny** pole adresu URL witryny (na przykład `http://www.example.com`). **Domeny** pole jest opcjonalne; umożliwia to zapewnia uwierzytelnianie dla całej domeny (takich jak *example.com*). 

    > [!NOTE]
    > Jeśli używasz lokacji na komputerze lokalnym przy użyciu adresu URL, takich jak `http://localhost:12345` (gdzie numer jest numerem portu lokalnego), można dodać tę wartość na **adres URL witryny** pole do testowania witryny. Jednak każdy razem numer portu lokacji lokalnej zmiany, musisz zaktualizować **adres URL witryny** pole aplikacji.
5. Wybierz **Zapisz zmiany** przycisku.
6. Wybierz **aplikacji** karcie ponownie, a następnie Wyświetl strony początkowej aplikacji.
7. Kopiuj **identyfikator aplikacji** i **klucz tajny aplikacji** wartości dla aplikacji i wklej je do pliku tekstowego tymczasowego. Te wartości zostaną spełnione dla dostawcy usługi Facebook w kodzie witryny sieci Web.
8. Zamknij stronę dewelopera usługi Facebook.

Teraz możesz zmienić dwie strony w witrynie sieci Web, dzięki czemu użytkownicy będą może logować się do witryny za pomocą ich kont usługi Facebook.

1. Utwórz lub Otwórz witrynę ASP.NET Web Pages opartego na szablonie witryny początkowej programu WebMatrix.
2. Otwórz  *\_AppStart.cshtml* strony i Usuń komentarz kodu dla dostawcy uwierzytelniania Facebook OAuth. Blok kodu uncommented wygląda następująco: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. Kopiuj **identyfikator aplikacji** wartość z zakresu od aplikacji usługi Facebook jako wartość `appId` parametr (wewnątrz cudzysłowów).
4. Kopiuj **klucz tajny aplikacji** wartość z zakresu od aplikacji usługi Facebook jako `appSecret` wartość parametru.
5. Zapisz i zamknij plik.

### <a name="testing-facebook-login"></a>Testowanie logowania usługi Facebook

1. Uruchom witrynę *default.cshtml* strony i wybierz polecenie **logowania** przycisku.
2. Na *logowania* strony w **Zaloguj się za pomocą innej usługi** wybierz **Facebook** ikony. 

    Strony sieci web przekierowuje żądanie do strony logowania usługi Facebook.

    ![OAuth-2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. Zaloguj się do konta usługi Facebook. 

    Kod używa tokenu usługi Facebook do uwierzytelniania, a następnie zwraca stronę możesz skojarzyć nazwę użytkownika usługi Facebook z logowania w witrynie. Adres nazwę lub adres e-mail użytkownika jest wprowadzany do **E-mail** pola formularza.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. Wybierz **skojarzyć** przycisku. 

    Zwraca przeglądarki do strony głównej i użytkownik jest zalogowany.

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a>Włączanie logowania do usługi Twitter

1. Przejdź do [lokacji deweloperzy Twitter](https://dev.twitter.com/).
2. Wybierz **Utwórz aplikację** łącza, a następnie zaloguj się do witryny.
3. Na **tworzenie aplikacji** tworzą, wypełnij **nazwa** i **opis** pola.
4. W **witryny sieci Web** wprowadź adres URL witryny sieci (na przykład `http://www.example.com`). 

    > [!NOTE]
    > Jeśli testujesz witryny lokalnie (przy użyciu adresu URL, takie jak `http://localhost:12345`), Twitter, adres URL nie może zaakceptować. Jednak można używać lokalnego sprzężenia zwrotnego adresu IP (na przykład `http://127.0.0.1:12345`). Upraszcza proces testowanie aplikacji lokalnie. Jednak za każdym razem, gdy zmienia numer portu witryny lokalnej, należy zaktualizować **witryny sieci Web** pole aplikacji.
5. W **wywołania zwrotnego adresu URL** wprowadź adres URL strony w witrynie sieci Web, które użytkowników, aby powrócić do po zalogowaniu w serwisie Twitter. Na przykład w celu wysłania użytkownikom do strony głównej witryny Starter (który rozpozna ich stan w zarejestrowany), wprowadź ten sam adres URL wprowadzony w **witryny sieci Web** pola.
6. Zaakceptuj postanowienia, a następnie wybierz **tworzenie aplikacji Twitter** przycisku.
7. Na **Moje aplikacje** początkowej strony, wybierz utworzoną aplikację.
8. Na **szczegóły** kartę, przewiń w dół i wybierz polecenie **Utwórz moje Token dostępu** przycisku.
9. Na **szczegóły** karcie, skopiuj **konsumenta** i **klucz tajny klienta** wartości dla aplikacji i wklej je do pliku tekstowego tymczasowego. W kodzie witryny sieci Web będzie przekazywać te wartości dostawcy usługi Twitter.
10. Zakończ witrynie Twitter.

Teraz możesz zmienić dwie strony w witrynie sieci Web, dzięki czemu użytkownicy będą mogli zalogować się do witryny za pomocą ich kont usługi Twitter.

1. Utwórz lub Otwórz witrynę ASP.NET Web Pages opartego na szablonie witryny początkowej programu WebMatrix.
2. Otwórz  *\_AppStart.cshtml* strony i Usuń komentarz kodu dla dostawcy usługi Twitter OAuth. Blok kodu uncommented wygląda następująco: 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. Kopiuj **konsumenta** wartość z zakresu od aplikacji Twitter jako wartość `consumerKey` parametr (wewnątrz cudzysłowów).
4. Kopiuj **klucz tajny klienta** wartość z zakresu od aplikacji Twitter jako wartość `consumerSecret` parametru.
5. Zapisz i zamknij plik.

### <a name="testing-twitter-login"></a>Testowanie logowania usługi Twitter

1. Uruchom *default.cshtml* strony w witrynie i wybierz polecenie **logowania** przycisku.
2. Na *logowania* strony w **Zaloguj się za pomocą innej usługi** wybierz **Twitter** ikony. 

    Strony sieci web przekierowuje żądanie do strony logowania usługi Twitter dla aplikacji, który został utworzony.

    ![OAuth 4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. Zaloguj się do konta w usłudze Twitter.
4. Kod używa tokenu usługi Twitter do uwierzytelnienia użytkownika i następnie powrót do strony możesz skojarzyć logowanie za pomocą konta witryny sieci Web. Wprowadzany do Twojej nazwy lub adresu e-mail **E-mail** pola formularza.

    ![OAuth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. Wybierz **skojarzyć** przycisku. 

    Zwraca przeglądarki do strony głównej i użytkownik jest zalogowany.

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Dodatkowe zasoby


- [Dostosowywanie zachowania całej lokacji](https://go.microsoft.com/fwlink/?LinkId=202906)
- [Dodawanie zabezpieczeń i członkostwo w witrynie stron sieci Web ASP.NET](https://go.microsoft.com/fwlink/?LinkID=202904)
