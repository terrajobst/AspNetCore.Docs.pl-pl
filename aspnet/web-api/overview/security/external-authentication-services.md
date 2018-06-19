---
uid: web-api/overview/security/external-authentication-services
title: Zewnętrznych usług uwierzytelniania z interfejsu API sieci Web platformy ASP.NET (C#) | Dokumentacja firmy Microsoft
author: rmcmurray
description: W tym artykule opisano, w interfejsie API sieci Web ASP.NET przy użyciu zewnętrznych usług uwierzytelniania.
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2013
ms.topic: article
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 406a85db7055910cb7a4e15fec8ef68dff5a19dc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876270"
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>Zewnętrznych usług uwierzytelniania z interfejsu API sieci Web platformy ASP.NET (C#)
====================
przez [Roberta Mcmurraya](https://github.com/rmcmurray)

Visual Studio 2013 i programu ASP.NET 4.5.1 Rozwiń opcje zabezpieczeń [aplikacje jednostronicowe](../../../single-page-application/index.md) (SPA) i [interfejsu API sieci Web](../../index.md) usług do integracji z usługami uwierzytelniania zewnętrznego, które obejmują kilka Usługi uwierzytelniania mediów społecznościowych i uwierzytelniania OAuth/OpenID: Accounts firmy Microsoft, Twitter, Facebook i Google.

### <a name="in-this-walkthrough"></a>W tym przewodniku

- [Za pomocą zewnętrznych usług uwierzytelniania](#USING)
- [Tworzenie przykładowej aplikacji sieci Web](#SAMPLE)
- [Włączenie uwierzytelniania serwisu Facebook](#FACEBOOK)
- [Włączanie uwierzytelniania w usłudze Google](#GOOGLE)
- [Włączanie uwierzytelniania firmy Microsoft](#MICROSOFT)
- [Włączanie uwierzytelniania usługi Twitter](#TWITTER)
- [Dodatkowe informacje](#MOREINFO)

    - [Łączenie zewnętrznych usług uwierzytelniania](#COMBINE)
    - [Konfigurowanie usług IIS Express do używania w pełni kwalifikowaną nazwę domeny](#FQDN)
    - [Jak uzyskać ustawienia aplikacji dla uwierzytelniania firmy Microsoft](#OBTAIN)
    - [Opcjonalnie: Wyłącz lokalnej rejestracji](#DISABLE)

### <a name="prerequisites"></a>Wymagania wstępne

Aby użyć przykłady w tym przewodniku, należy dysponować następującymi elementami:

- Visual Studio 2013
- Konto dla co najmniej jednego z następujących usług uwierzytelniania zewnętrznego:

    - Konto użytkownika usługi Google
    - Konto dewelopera z aplikacji identyfikator i klucz tajny dla jednej z następujących usług uwierzytelniania mediów społecznościowych:

        - Konta Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - W usłudze Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Za pomocą zewnętrznych usług uwierzytelniania

Wiele usług uwierzytelniania zewnętrznego, które są obecnie dostępne dla pomoc deweloperów sieci web, aby zmniejszyć programowanie czas podczas tworzenia nowej aplikacji sieci web. Użytkownicy sieci Web zwykle mają kilka istniejących kont dla usług sieci web popularnych i społecznościowych witryn sieci Web, w związku z tym podczas implementuje aplikacji sieci web, uwierzytelnianie usług z zewnętrznej usługi sieci web lub witryny społecznościowe, zapisuje czasie opracowywania który może zostać wykorzystana tworzenie implementację uwierzytelniania. Przy użyciu usługi uwierzytelniania zewnętrznego zapisuje użytkowników końcowych, trzeba utworzyć nowe konto dla aplikacji sieci web, a także od trzeba pamiętać innej nazwy użytkownika i hasła.

W przeszłości deweloperów dwie opcje: tworzenie realizacji uwierzytelniania lub Dowiedz się, jak zintegrować usługę uwierzytelniania zewnętrznego w swoich aplikacjach. Na najbardziej podstawowym poziomie na poniższym diagramie przedstawiono przepływ żądania prostego agenta użytkownika (przeglądarki sieci web), który żąda informacji z aplikacji sieci web, która jest skonfigurowana do używania usługi uwierzytelniania zewnętrznego:

[![](external-authentication-services/_static/image2.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image1.png)

Agent użytkownika (lub przeglądarki sieci web, w tym przykładzie) na powyższym diagramie, wysyła żądanie do aplikacji sieci web, który przekierowuje przeglądarki sieci web do usługi uwierzytelniania zewnętrznego. Agent użytkownika wysyła swoje poświadczenia do usługi uwierzytelniania zewnętrznego, a jeśli agent użytkownika został pomyślnie uwierzytelniony, Usługa uwierzytelniania zewnętrznego przekieruje agenta użytkownika do oryginalnej aplikacji sieci web z jakiegoś typu tokenu, który agent użytkownika będzie wysyłać do aplikacji sieci web. Aplikacja sieci web użyje token w celu sprawdź, czy agent użytkownika został pomyślnie uwierzytelniony przez usługę uwierzytelniania zewnętrznego i aplikacji sieci web może używać tokenu można zebrać więcej informacji na temat agenta użytkownika. Po zakończeniu aplikacji przetwarzanie informacji dotyczących agenta użytkownika, aplikacji sieci web zwróci właściwą odpowiedź agenta użytkownika na podstawie swoich ustawień autoryzacji.

W drugim przykładzie negocjuje agenta użytkownika z serwera zewnętrznego autoryzacji i aplikacji sieci web i aplikacji sieci web wykonuje dodatkowe komunikacji z serwerem autoryzację zewnętrzne można pobrać dodatkowe informacje o użytkowniku Agent:

[![](external-authentication-services/_static/image4.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image3.png)

Visual Studio 2013 i programu ASP.NET 4.5.1 ułatwienia integracji z zewnętrznych usług uwierzytelniania dla deweloperów zapewniając wbudowanej integracji dla następujących usług uwierzytelniania:

- Facebook
- Google
- Accounts Microsoft (konta usługi Windows Live ID)
- Twitter

Przykłady w tym przewodniku przedstawiono sposób konfigurowania każdej z obsługiwanych zewnętrznych usług uwierzytelniania przy użyciu nowego szablonu aplikacji sieci Web ASP.NET, który jest dostarczany z programu Visual Studio 2013.

> [!NOTE]
> Jeśli to konieczne, może być konieczne Dodaj swoją nazwę FQDN w ustawieniach usługi uwierzytelniania zewnętrznego. To wymaganie jest oparta na ograniczeń dotyczących zabezpieczeń w przypadku niektórych usług uwierzytelniania zewnętrznego, które wymagają FQDN w ustawieniach aplikacji do dopasowania nazwy FQDN, która jest używana przez klientów. (Kroki opisane w tym się znacznie różnić dla poszczególnych usług uwierzytelniania zewnętrznego; należy zapoznać się z dokumentacją dla każdej usługi uwierzytelniania zewnętrznego, aby zobaczyć, jeśli jest to wymagane oraz sposobu konfigurowania tych ustawień). Jeśli konieczne będzie skonfigurowanie usług IIS Express do używania nazwy FQDN w tym środowisku testowym, zobacz [Konfigurowanie usług IIS Express do używania w pełni kwalifikowaną nazwę domeny](#FQDN) dalszej części tego przewodnika.


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>Tworzenie przykładowej aplikacji sieci Web

Poniższe kroki przeprowadzi Cię przez proces tworzenia przykładowej aplikacji przy użyciu szablonu aplikacji sieci Web platformy ASP.NET, i użyje tej przykładowej aplikacji dla poszczególnych usług uwierzytelniania zewnętrznego w dalszej części tego przewodnika.

Uruchom program Visual Studio 2013, wybierz **nowy projekt** ze strony początkowej. Lub z **pliku** menu, wybierz opcję **nowy** , a następnie **projektu**.

[![](external-authentication-services/_static/image6.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image5.png)

Gdy **nowy projekt** zostanie wyświetlone okno dialogowe, wybierz **zainstalowana** **szablony** i rozwiń **Visual C#**. W obszarze **Visual C#**, wybierz pozycję **Web**. Na liście szablony projektów, wybierz **aplikacji sieci Web ASP.NET**. Wprowadź nazwę projektu, a następnie kliknij przycisk **OK**.

[![](external-authentication-services/_static/image8.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image7.png)

Gdy **nowy projekt ASP.NET** jest wyświetlane, wybierz pozycję **SPA** szablon i kliknij przycisk **tworzenia projektu**.

[![](external-authentication-services/_static/image10.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image9.png)

Poczekaj co program Visual Studio 2013 tworzy projektu.

[![](external-authentication-services/_static/image12.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image11.png)

Po zakończeniu programu Visual Studio 2013 Tworzenie projektu otwórz *Startup.Auth.cs* pliku, który znajduje się w **aplikacji\_Start** folderu.

[![](external-authentication-services/_static/image14.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image13.png)

Po utworzeniu projektu Brak zewnętrznych usług uwierzytelniania są włączone w *Startup.Auth.cs* pliku; następujące pokazano, co może wyglądać kodu, z sekcji wyróżnione, gdzie można włączyć Usługa uwierzytelniania zewnętrznego i wszystkie odpowiednie ustawienia, aby korzystać z uwierzytelniania Accounts firmy Microsoft, Twitter, Facebook i Google w aplikacji ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Kiedy naciśniesz klawisz F5, aby skompilować i debugowanie aplikacji sieci web, wyświetli ekran logowania, w którym pojawi się, że nie zdefiniowano żadnych zewnętrznych usług uwierzytelniania.

[![](external-authentication-services/_static/image16.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image15.png)

W poniższych sekcjach dowiesz się, jak włączyć poszczególnych usług uwierzytelniania zewnętrznego, które są dostarczane z programem ASP.NET w programie Visual Studio 2013.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Włączenie uwierzytelniania serwisu Facebook

Za pomocą usługi Facebook uwierzytelniania wymaga utworzenia konta dewelopera usługi Facebook, a projekt będzie wymagać prawidłowego działania aplikacji identyfikator i klucz tajny z usługi Facebook. Aby uzyskać informacje o tworzeniu konta dewelopera usługi Facebook i uzyskiwanie Identyfikatora aplikacji, a klucz tajny, zobacz [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Po uzyskaniu Twojej aplikacji identyfikator i klucz tajny, wykonaj następujące kroki, aby włączyć uwierzytelnianie serwisu Facebook dla aplikacji sieci web:

1. Gdy projekt jest otwarty w programie Visual Studio 2013, otwórz *Startup.Auth.cs* pliku:

    [![](external-authentication-services/_static/image18.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image17.png)
2. Znajdź sekcję wyróżniony kod:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Usuń &quot; // &quot; znaków, usuń znaczniki komentarza wyróżnione wiersze kodu, a następnie dodaj swoją nazwę aplikacji i klucz tajny. Po dodaniu tych parametrów można ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Kiedy naciśniesz klawisz F5, aby otworzyć aplikację sieci web w przeglądarce sieci web, zobaczysz, że Facebook została zdefiniowana jako usługi uwierzytelniania zewnętrznego:

    [![](external-authentication-services/_static/image20.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image19.png)
5. Po kliknięciu **Facebook** przycisku przeglądarki, zostanie przekierowany do strony logowania serwisu Facebook:

    [![](external-authentication-services/_static/image22.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image21.png)
6. Po wprowadź swoje poświadczenia usługi Facebook i kliknij przycisk **Zaloguj się za**, przeglądarki sieci web zostanie przekierowany do aplikacji sieci web, która wyświetli monit o **nazwy użytkownika** , które chcesz skojarzyć z z Konto w serwisie Facebook:

    [![](external-authentication-services/_static/image24.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image23.png)
7. Po wprowadzeniu nazwy użytkownika i kliknięciu **Zarejestruj** przycisku, aplikacji sieci web zostanie wyświetlona domyślna **strony głównej** dla Twojego konta w serwisie Facebook:

    [![](external-authentication-services/_static/image26.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Włączanie uwierzytelniania w usłudze Google

Google jest znacznie najprostsza usług uwierzytelniania zewnętrznego, można włączyć, ponieważ nie wymaga konta dewelopera, ani nie wymaga dodatkowych informacji, takimi jak identyfikator aplikacji lub klucz tajny jako innych usług uwierzytelniania zewnętrznego wymagają.

Aby włączyć uwierzytelnianie Google dla aplikacji sieci web, użyj następujących kroków:

1. Gdy projekt jest otwarty w programie Visual Studio 2013, otwórz *Startup.Auth.cs* pliku:

    [![](external-authentication-services/_static/image28.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image27.png)
2. Znajdź sekcję wyróżniony kod:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Usuń &quot; // &quot; znaków, usuń znaczniki komentarza wyróżniony wiersz kodu, a następnie ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Kiedy naciśniesz klawisz F5, aby otworzyć aplikację sieci web w przeglądarce sieci web, zobaczysz, że Google została zdefiniowana jako usługi uwierzytelniania zewnętrznego:

    [![](external-authentication-services/_static/image30.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image29.png)
5. Po kliknięciu **Google** przycisku przeglądarki, zostanie przekierowany do strony logowania usługi Google:

    [![](external-authentication-services/_static/image32.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image31.png)
6. Po wprowadzeniu poświadczeń Google i kliknij przycisk **Zaloguj**, Google spowoduje wyświetlenie monitu o Sprawdź, czy aplikacja sieci web ma uprawnienia do uzyskania dostępu do konta Google:

    [![](external-authentication-services/_static/image34.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image33.png)
7. Po kliknięciu **Akceptuj**, przeglądarki sieci web zostanie przekierowany do aplikacji sieci web, która wyświetli monit o **nazwy użytkownika** , które chcesz skojarzyć z kontem Google:

    [![](external-authentication-services/_static/image36.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image35.png)
8. Po wprowadzeniu nazwy użytkownika i kliknięciu **Zarejestruj** przycisku, aplikacji sieci web zostanie wyświetlona domyślna **strony głównej** dla Twojego konta Google:

    [![](external-authentication-services/_static/image38.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Włączanie uwierzytelniania firmy Microsoft

Uwierzytelnianie firmy Microsoft wymaga utworzenia konta dewelopera i prawidłowego działania wymaga Identyfikatora klienta i klucz tajny klienta. Aby uzyskać informacje o tworzeniu konta dewelopera Microsoft i uzyskiwanie z Identyfikatorem klienta i klucz tajny klienta, zobacz [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070).

Po uzyskaniu Twoje klucz klienta i klucz tajny klienta, wykonaj następujące kroki, aby włączyć uwierzytelnianie platformy Microsoft dla aplikacji sieci web:

1. Gdy projekt jest otwarty w programie Visual Studio 2013, otwórz *Startup.Auth.cs* pliku:

    [![](external-authentication-services/_static/image40.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image39.png)
2. Znajdź sekcję wyróżniony kod:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Usuń &quot; // &quot; znaków, usuń znaczniki komentarza wyróżnione wiersze kodu, a następnie dodaj swój identyfikator klienta i klucz tajny klienta. Po dodaniu tych parametrów można ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Kiedy naciśniesz klawisz F5, aby otworzyć aplikację sieci web w przeglądarce sieci web, zobaczysz, że Microsoft została zdefiniowana jako usługi uwierzytelniania zewnętrznego:

    [![](external-authentication-services/_static/image42.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image41.png)
5. Po kliknięciu **Microsoft** przycisku przeglądarki, zostanie przekierowany do strony logowania firmy Microsoft:

    [![](external-authentication-services/_static/image44.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image43.png)
6. Po wprowadzeniu poświadczeń Microsoft i kliknij przycisk **Zaloguj**, pojawi się monit, aby sprawdzić, czy aplikacja sieci web ma uprawnienia do uzyskania dostępu do konta Microsoft:

    [![](external-authentication-services/_static/image46.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image45.png)
7. Po kliknięciu **tak**, przeglądarki sieci web zostanie przekierowany do aplikacji sieci web, która wyświetli monit o **nazwy użytkownika** , które chcesz skojarzyć z Twoim kontem Microsoft:

    [![](external-authentication-services/_static/image48.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image47.png)
8. Po wprowadzeniu nazwy użytkownika i kliknięciu **Zarejestruj** przycisku, aplikacji sieci web zostanie wyświetlona domyślna **strony głównej** konta Microsoft:

    [![](external-authentication-services/_static/image50.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Włączanie uwierzytelniania usługi Twitter

W usłudze Twitter uwierzytelniania wymaga utworzenia konta dewelopera i klucz klienta i klucz tajny klienta wymaga prawidłowego działania. Aby uzyskać informacje o tworzeniu konta dewelopera usługi Twitter i uzyskiwania z klucza klienta i klucz tajny klienta, zobacz [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Po uzyskaniu Twoje klucz klienta i klucz tajny klienta, wykonaj następujące kroki, aby włączyć uwierzytelnianie usługi Twitter dla aplikacji sieci web:

1. Gdy projekt jest otwarty w programie Visual Studio 2013, otwórz *Startup.Auth.cs* pliku:

    [![](external-authentication-services/_static/image52.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image51.png)
2. Znajdź sekcję wyróżniony kod:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Usuń &quot; // &quot; znaków usuń znaczniki komentarza wyróżnione wiersze kodu, a następnie dodaj klucz klienta i klucz tajny klienta. Po dodaniu tych parametrów można ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Kiedy naciśniesz klawisz F5, aby otworzyć aplikację sieci web w przeglądarce sieci web, zobaczysz, że Twitter została zdefiniowana jako usługi uwierzytelniania zewnętrznego:

    [![](external-authentication-services/_static/image54.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image53.png)
5. Po kliknięciu **Twitter** przycisku przeglądarki, zostanie przekierowany do strony logowania usługi Twitter:

    [![](external-authentication-services/_static/image56.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image55.png)
6. Po wprowadzeniu poświadczeń usługi Twitter i kliknij przycisk **Autoryzuj aplikację**, przeglądarki sieci web zostanie przekierowany do aplikacji sieci web, która wyświetli monit o **nazwy użytkownika** , które chcesz skojarzyć z Konto usługi Twitter:

    [![](external-authentication-services/_static/image58.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image57.png)
7. Po wprowadzeniu nazwy użytkownika i kliknięciu **Zarejestruj** przycisku, aplikacji sieci web zostanie wyświetlona domyślna **strony głównej** dla Twojego konta w serwisie Twitter:

    [![](external-authentication-services/_static/image60.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Dodatkowe informacje

Aby uzyskać dodatkowe informacje na temat tworzenia aplikacji, które używają protokołu OAuth i OpenID zobacz następujące adresy URL:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Łączenie zewnętrznych usług uwierzytelniania

Aby uzyskać większą elastyczność można zdefiniować wiele usług uwierzytelniania zewnętrznego, w tym samym czasie — dzięki temu sieci web aplikacji użytkownicy mogą używać konta z dowolnej usługi włączone uwierzytelnianie zewnętrzne:

[![](external-authentication-services/_static/image62.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>Konfigurowanie usług IIS Express do używania w pełni kwalifikowaną nazwę domeny

Niektórzy dostawcy uwierzytelniania zewnętrznego nie obsługują testowania aplikacji przy użyciu adresu HTTP, takie jak `http://localhost:port/`. Aby obejść ten problem, można dodać mapowanie statyczne pełni kwalifikowanej domeny nazwę (FQDN) do pliku hostów i skonfigurować opcje projektu programu Visual Studio 2013 do używania nazwy FQDN do testowania debugowania. Aby to zrobić, wykonaj następujące kroki:

- Dodaj nazwę FQDN statyczne mapowania pliku HOSTS:

  1. Otwórz wiersz polecenia z podwyższonym poziomem uprawnień w systemie Windows.
  2. Wpisz następujące polecenie:

      <kbd>%WinDir%\system32\drivers\etc\hosts Notatnik</kbd>
  3. Dodaj wpis podobnie do następującej w pliku HOSTS:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Zapisz i zamknij plik HOSTS.

- Konfigurowanie projektu programu Visual Studio do nazwy FQDN:

  1. Gdy projekt jest otwarty w programie Visual Studio 2013, kliknij przycisk **projektu** menu, a następnie wybierz pozycję Właściwości projektu. Na przykład możesz wybrać pozycję **WebApplication1 właściwości**.
  2. Wybierz **Web** kartę.
  3. Wprowadź nazwę FQDN dla <strong>projektu adres Url</strong>. Na przykład możesz wpisać <kbd> <http://www.wingtiptoys.com> </kbd> Jeśli, która jest dodane do pliku HOSTS mapowanie nazwy FQDN.

- Konfigurowanie usług IIS Express do używania nazwy FQDN dla aplikacji:

    1. Otwórz wiersz polecenia z podwyższonym poziomem uprawnień w systemie Windows.
    2. Wpisz następujące polecenie, aby zmienić do folderu usług IIS Express:

        <kbd>cd /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Wpisz następujące polecenie, aby dodać nazwę FQDN do aplikacji:

        <kbd>appcmd.exe set config-section:system.applicationHost/sites / +&quot;[name = "WebApplication1"] .bindings. [ Protokół = 'http', bindingInformation = "*:80:www.wingtiptoys.com"]&quot; /commit:apphost</kbd>

  Gdzie **WebApplication1** jest nazwą projektu i **bindingInformation** zawiera numer portu i nazwy FQDN, które ma być używany na potrzeby testów.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Jak uzyskać ustawienia aplikacji dla uwierzytelniania firmy Microsoft

Łączenie aplikacji do usługi Windows Live dla Microsoft Authentication jest prosty proces. Jeśli aplikacji do usługi Windows Live nie jest już połączony, można użyć następujących czynności:

1. Przejdź do [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) i wprowadź nazwę konta Microsoft i hasło po wyświetleniu monitu, a następnie kliknij przycisk **Zaloguj**:

    [![](external-authentication-services/_static/image64.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image63.png)
2. Wprowadź nazwę i język aplikacji po wyświetleniu monitu, a następnie kliknij przycisk **akceptuję**:

    [![](external-authentication-services/_static/image66.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image65.png)
3. Na **ustawień interfejsu API** strony dla aplikacji, wprowadź domenę przekierowania aplikacji i skopiuj **identyfikator klienta** i **klucz tajny klienta** dla projektu, a następnie Kliknij przycisk **zapisać**:

    [![](external-authentication-services/_static/image68.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Opcjonalnie: Wyłącz lokalnej rejestracji

Bieżącą funkcjonalność lokalnego rejestracji programu ASP.NET nie uniemożliwia tworzenie kont; element członkowski automatycznych programów (robotów) na przykład, korzystając z technologii zapobiegania bot i sprawdzania poprawności, takich jak [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). W związku z tym należy usunąć link do logowania lokalnego formularza i rejestracji na stronie logowania. Aby to zrobić, otwórz  *\_Login.cshtml* w projekcie, a następnie komentarz wierszy panel logowania lokalnego i łącze. Wynikowej strony powinien wyglądać podobnie jak w poniższym przykładzie kodu:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Po wyłączeniu panel logowania lokalnego i połącz rejestracji, strona logowania będą wyświetlane tylko dostawcy uwierzytelniania zewnętrznego, które włączono:

[![](external-authentication-services/_static/image70.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image69.png)
