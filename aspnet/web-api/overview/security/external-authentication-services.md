---
uid: web-api/overview/security/external-authentication-services
title: Zewnętrznych usług uwierzytelniania przy użyciu interfejsu API sieci Web platformy ASP.NET (C#) | Dokumentacja firmy Microsoft
author: rmcmurray
description: W tym artykule opisano korzystanie z zewnętrznych usług uwierzytelniania w Web API platformy ASP.NET.
ms.author: riande
ms.date: 06/26/2013
ms.assetid: 3bb8eb15-b518-44f5-a67d-a27e051aedc6
msc.legacyurl: /web-api/overview/security/external-authentication-services
msc.type: authoredcontent
ms.openlocfilehash: 0b23baac7eca0297e063c682a8ae199f9543d75e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/16/2018
ms.locfileid: "41754929"
---
<a name="external-authentication-services-with-aspnet-web-api-c"></a>Zewnętrznych usług uwierzytelniania przy użyciu interfejsu API sieci Web platformy ASP.NET (C#)
====================
przez [Robert McMurray](https://github.com/rmcmurray)

Visual Studio 2013 i ASP.NET 4.5.1 Rozwiń opcje zabezpieczeń [aplikacje jednostronicowe](../../../single-page-application/index.md) (SPA) i [interfejsu API sieci Web](../../index.md) usługi w celu integracji z usługami uwierzytelniania zewnętrznego, które obejmują kilka Uwierzytelniania OAuth/OpenID i uwierzytelniania mediach społecznościowych: Accounts firmy Microsoft, Twitter, Facebook i Google.

### <a name="in-this-walkthrough"></a>W tym przewodniku

- [Za pomocą zewnętrznych usług uwierzytelniania](#USING)
- [Tworzenie przykładowej aplikacji sieci Web](#SAMPLE)
- [Włączanie uwierzytelniania serwisu Facebook](#FACEBOOK)
- [Włączanie uwierzytelniania serwisu Google](#GOOGLE)
- [Włączanie uwierzytelniania firmy Microsoft](#MICROSOFT)
- [Włączanie uwierzytelniania usługi Twitter](#TWITTER)
- [Dodatkowe informacje](#MOREINFO)

    - [Łączenie zewnętrznych usług uwierzytelniania](#COMBINE)
    - [Konfiguracja usług IIS Express do użycia w pełni kwalifikowaną nazwę domeny](#FQDN)
    - [Jak uzyskać ustawienia aplikacji dla uwierzytelniania firmy Microsoft](#OBTAIN)
    - [Opcjonalnie: Wyłącz lokalną rejestrację.](#DISABLE)

### <a name="prerequisites"></a>Wymagania wstępne

Aby skorzystać z przykładów w tym przewodniku, należy dysponować następującymi elementami:

- Visual Studio 2013
- Konto usługi dla co najmniej jeden z następujących usług uwierzytelniania zewnętrznego:

    - Konto użytkownika Google
    - Konto dewelopera z identyfikatorem aplikacji i klucz tajny dla jednego z następujących usług uwierzytelniania mediów społecznościowych:

        - Konta Microsoft ([https://go.microsoft.com/fwlink/?LinkID=144070](https://go.microsoft.com/fwlink/?LinkID=144070))
        - W usłudze Twitter ([https://dev.twitter.com/](https://dev.twitter.com/))
        - Facebook ([https://developers.facebook.com/](https://developers.facebook.com/))

<a id="USING"></a>
## <a name="using-external-authentication-services"></a>Za pomocą zewnętrznych usług uwierzytelniania

Wiele usług uwierzytelniania zewnętrznego, które są obecnie dostępne dla sieci web deweloperów pomoc, aby zmniejszyć rozwoju czas podczas tworzenia nowej aplikacji sieci web. Użytkownicy sieci Web zazwyczaj mają kilka istniejących kont usług sieci web popularnych i społecznościowych witryn sieci Web, w związku z tym gdy implementuje aplikacji sieci web, usług uwierzytelniania z zewnętrznej usługi sieci web lub witryny sieci Web w mediach społecznościowych, można zaoszczędzić czas opracowywania może zostać wykorzystana tworzenia implementację uwierzytelniania. Usługa uwierzytelniania zewnętrznego zapisuje użytkownikom końcowym, trzeba utworzyć nowe konto dla aplikacji sieci web, a także od trzeba pamiętać innej nazwy użytkownika i hasła.

W przeszłości deweloperów dwie opcje: tworzenie zapewniali własną implementację uwierzytelniania lub informacje o sposobie integrowania usługi uwierzytelniania zewnętrznego w swoich aplikacjach. Na najbardziej podstawowym poziomie, na poniższym diagramie przedstawiono przepływ żądania prostego agenta użytkownika (przeglądarka sieci web), który żąda informacji z aplikacji sieci web, który jest skonfigurowany do używania usługi uwierzytelniania zewnętrznych:

[![](external-authentication-services/_static/image2.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image1.png)

Agent użytkownika (lub przeglądarki sieci web, w tym przykładzie) na powyższym diagramie, kieruje żądanie do aplikacji sieci web, który przekierowuje przeglądarki sieci web w usłudze uwierzytelniania zewnętrznego. Agent użytkownika wysyła swoje poświadczenia usługi uwierzytelniania zewnętrznego, i jeśli agent użytkownika o pomyślnym uwierzytelnieniu, usługę uwierzytelniania zewnętrznego przekieruje agenta użytkownika do oryginalnej aplikacji sieci web przy użyciu pewnego rodzaju token, który agent użytkownika będzie wysyłać do aplikacji sieci web. Aplikacja sieci web użyje token Sprawdź, czy agent użytkownika został pomyślnie uwierzytelniony przez usługę uwierzytelniania zewnętrznego i aplikacji sieci web może zebrać więcej informacji na temat agenta użytkownika przy użyciu tokenu. Po zakończeniu aplikacji przetwarzania informacji agenta użytkownika, aplikacja sieci web zwróci właściwą odpowiedź na podstawie jej ustawień autoryzacji przekazać agentowi użytkownika.

W drugim przykładzie agent użytkownika negocjuje z serwera zewnętrznego autoryzacji i aplikacji sieci web i aplikacji sieci web wykonuje dodatkowej komunikacji z serwerem autoryzacji zewnętrzne, można pobrać dodatkowe informacje o użytkowniku Agent:

[![](external-authentication-services/_static/image4.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image3.png)

Visual Studio 2013 i ASP.NET 4.5.1 ułatwienia integracji z zewnętrznych usług uwierzytelniania dla deweloperów, zapewniając wbudowane funkcje integracji dla następujących usług uwierzytelniania:

- Facebook
- Google
- Accounts firmy Microsoft (konta Windows Live ID)
- Twitter

W przykładach w tym przewodniku pokazano, jak skonfigurować każdej z obsługiwanych zewnętrznych usług uwierzytelniania przy użyciu nowego szablonu aplikacji sieci Web ASP.NET, który jest dostarczany za pomocą programu Visual Studio 2013.

> [!NOTE]
> Jeśli to konieczne, może być konieczne dodanie Twojej nazwy FQDN do ustawienia usługi uwierzytelniania zewnętrznego. Wymóg ten jest oparty na ograniczenia zabezpieczeń w przypadku niektórych usług uwierzytelniania zewnętrznego, które wymagają nazwy FQDN w ustawieniach aplikacji dopasować nazwę FQDN, który jest używany przez klientów. (Kroki opisane w tym się znacznie różnić dla poszczególnych usług uwierzytelniania zewnętrznego, należy skontaktować się z dokumentacją poszczególnych usług uwierzytelniania zewnętrznego, aby zobaczyć, jeśli jest to wymagane i sposobie konfigurowania tych ustawień.) Jeśli potrzebujesz do skonfigurowania usług IIS Express do używania nazwy FQDN, w tym środowisku testowym, zobacz [konfigurowania usług IIS Express do użycia w pełni kwalifikowaną nazwę domeny](#FQDN) sekcję w dalszej części tego przewodnika.


<a id="SAMPLE"></a>
## <a name="creating-a-sample-web-application"></a>Tworzenie przykładowej aplikacji sieci Web

Poniższe kroki prowadzi przez proces tworzenia przykładowej aplikacji przy użyciu szablonu aplikacji sieci Web platformy ASP.NET i użyjesz tej przykładowej aplikacji dla poszczególnych usług uwierzytelniania zewnętrznego w dalszej części tego przewodnika.

Uruchom program Visual Studio 2013, wybierz **nowy projekt** ze strony początkowej. Lub z **pliku** menu, wybierz opcję **New** i następnie **projektu**.

[![](external-authentication-services/_static/image6.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image5.png)

Gdy **nowy projekt** zostanie wyświetlone okno dialogowe, wybierz **zainstalowane** **szablony** i rozwiń **Visual C#**. W obszarze **Visual C#**, wybierz opcję **Web**. Na liście szablonów projektu wybierz **aplikacji sieci Web ASP.NET**. Wprowadź nazwę dla projektu, a następnie kliknij przycisk **OK**.

[![](external-authentication-services/_static/image8.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image7.png)

Gdy **nowy projekt ASP.NET** jest wyświetlany, wybierz opcję **SPA** szablon i kliknij przycisk **Tworzenie projektu**.

[![](external-authentication-services/_static/image10.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image9.png)

Poczekaj co program Visual Studio 2013 tworzy projekt.

[![](external-authentication-services/_static/image12.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image11.png)

Po zakończeniu programu Visual Studio 2013 podczas tworzenia projektu, otwórz *Startup.Auth.cs* pliku, który znajduje się w **aplikacji\_Start** folderu.

[![](external-authentication-services/_static/image14.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image13.png)

Po utworzeniu projektu Brak zewnętrznych usług uwierzytelniania są włączone w *Startup.Auth.cs* pliku; poniższy rysunek ilustruje, co może wyglądać w kodzie, z sekcjami wyróżniony, gdzie zostanie włączone Usługa uwierzytelniania zewnętrznego i wszelkie odpowiednie ustawienia, aby można było używać uwierzytelniania Accounts firmy Microsoft, Twitter, Facebook lub Google przy użyciu aplikacji ASP.NET:

[!code-csharp[Main](external-authentication-services/samples/sample1.cs)]

Po naciśnięciu klawisza F5, aby kompilować i debugować aplikację sieci web, wyświetli ekran logowania, gdzie zobaczysz, że nie zdefiniowano żadnych zewnętrznych usług uwierzytelniania.

[![](external-authentication-services/_static/image16.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image15.png)

W poniższych sekcjach dowiesz się, jak włączyć każdą z tych usług uwierzytelniania zewnętrznego, które są dostarczane za pomocą platformy ASP.NET w programie Visual Studio 2013.

<a id="FACEBOOK"></a>
## <a name="enabling-facebook-authentication"></a>Włączanie uwierzytelniania serwisu Facebook

Za pomocą usługi Facebook uwierzytelnianie wymaga utworzenia konta dewelopera usługi Facebook, a projekt będzie wymagać Identyfikatora aplikacji i klucza tajnego z usługi Facebook aby funkcjonować. Aby uzyskać informacje o tworzeniu konta dla deweloperów w usłudze Facebook i uzyskania Twojej aplikacji, identyfikator i klucz tajny, zobacz [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Po uzyskaniu swojego Identyfikatora aplikacji i klucz tajny, wykonaj następujące kroki, aby włączyć uwierzytelnianie serwisu Facebook dla aplikacji sieci web:

1. Gdy projekt jest otwarty w programie Visual Studio 2013, otwórz *Startup.Auth.cs* pliku:

    [![](external-authentication-services/_static/image18.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image17.png)
2. Znajdź wyróżnioną sekcję kodu:

    [!code-csharp[Main](external-authentication-services/samples/sample2.cs)]
3. Usuń &quot; // &quot; znaków, usuń znaczniki komentarza wyróżnione wiersze kodu, a następnie dodaj swojego Identyfikatora aplikacji i klucz tajny. Po dodaniu tych parametrów można ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample3.cs)]
4. Po naciśnięciu klawisza F5, aby otworzyć aplikację sieci web w przeglądarce sieci web, pojawią się, że usługi Facebook został zdefiniowany jako usługi uwierzytelniania zewnętrznego:

    [![](external-authentication-services/_static/image20.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image19.png)
5. Po kliknięciu **Facebook** przycisku przeglądarki nastąpi przekierowanie do strony logowania usługi Facebook:

    [![](external-authentication-services/_static/image22.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image21.png)
6. Po wprowadź swoje poświadczenia usługi Facebook i kliknij przycisk **Zaloguj się**, przeglądarki sieci web zostanie przekierowany ponownie do aplikacji sieci web, która wyświetli monit o **nazwy użytkownika** , którą chcesz skojarzyć z usługi Konta w serwisie Facebook:

    [![](external-authentication-services/_static/image24.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image23.png)
7. Po wprowadzeniu nazwy użytkownika i kliknięciu **Zarejestruj** przycisku aplikacji sieci web zostanie wyświetlona domyślna **strony głównej** dla Twojego konta serwisu Facebook:

    [![](external-authentication-services/_static/image26.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image25.png)

<a id="GOOGLE"></a>
## <a name="enabling-google-authentication"></a>Włączanie uwierzytelniania serwisu Google

Jest znacznie najłatwiejszym usług uwierzytelniania zewnętrznego, aby włączyć, ponieważ nie wymaga konta dewelopera ani nie wymaga dodatkowych informacji takimi jak identyfikator aplikacji lub klucz tajny w odróżnieniu od innych usług uwierzytelniania zewnętrznego, Google wymagają.

Aby włączyć uwierzytelnianie serwisu Google dla aplikacji sieci web, użyj następujących kroków:

1. Gdy projekt jest otwarty w programie Visual Studio 2013, otwórz *Startup.Auth.cs* pliku:

    [![](external-authentication-services/_static/image28.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image27.png)
2. Znajdź wyróżnioną sekcję kodu:

    [!code-csharp[Main](external-authentication-services/samples/sample4.cs)]
3. Usuń &quot; // &quot; znaków, usuń znaczniki komentarza wyróżniony wiersz kodu, a następnie ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample5.cs)]
4. Po naciśnięciu klawisza F5, aby otworzyć aplikację sieci web w przeglądarce sieci web, zobaczysz, że Google został zdefiniowany jako usługi uwierzytelniania zewnętrznego:

    [![](external-authentication-services/_static/image30.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image29.png)
5. Po kliknięciu **Google** przycisku przeglądarki nastąpi przekierowanie do strony logowania Google:

    [![](external-authentication-services/_static/image32.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image31.png)
6. Po wprowadzeniu poświadczeń Google i kliknięciu **Zaloguj**, Google zostanie wyświetlony monit, aby sprawdzić, czy aplikacja sieci web ma uprawnienia dostępu do konta Google:

    [![](external-authentication-services/_static/image34.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image33.png)
7. Po kliknięciu **Akceptuj**, nastąpi przekierowanie przeglądarki sieci web do aplikacji sieci web, która wyświetli monit o **nazwy użytkownika** , którą chcesz skojarzyć ze swoim kontem Google:

    [![](external-authentication-services/_static/image36.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image35.png)
8. Po wprowadzeniu nazwy użytkownika i kliknięciu **Zarejestruj** przycisku aplikacji sieci web zostanie wyświetlona domyślna **strony głównej** dla swojego konta Google:

    [![](external-authentication-services/_static/image38.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image37.png)

<a id="MICROSOFT"></a>
## <a name="enabling-microsoft-authentication"></a>Włączanie uwierzytelniania firmy Microsoft

Uwierzytelnianie firmy Microsoft wymaga utworzenia konta dewelopera i aby funkcjonować wymaga Identyfikatora klienta oraz klucz tajny klienta. Aby uzyskać informacje o tworzeniu konta dewelopera Microsoft i uzyskiwanie swojego Identyfikatora klienta i klucz tajny klienta, zobacz [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070).

Po uzyskaniu usługi konsumenta oraz klucz tajny klienta, wykonaj następujące kroki, aby włączyć uwierzytelnianie firmy Microsoft dla aplikacji sieci web:

1. Gdy projekt jest otwarty w programie Visual Studio 2013, otwórz *Startup.Auth.cs* pliku:

    [![](external-authentication-services/_static/image40.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image39.png)
2. Znajdź wyróżnioną sekcję kodu:

    [!code-csharp[Main](external-authentication-services/samples/sample6.cs)]
3. Usuń &quot; // &quot; znaków, usuń znaczniki komentarza wyróżnione wiersze kodu, a następnie dodaj swój identyfikator klienta i klucz tajny klienta. Po dodaniu tych parametrów można ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample7.cs)]
4. Po naciśnięciu klawisza F5, aby otworzyć aplikację sieci web w przeglądarce sieci web, zobaczysz, że Microsoft został zdefiniowany jako usługi uwierzytelniania zewnętrznego:

    [![](external-authentication-services/_static/image42.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image41.png)
5. Po kliknięciu **Microsoft** przycisku przeglądarki nastąpi przekierowanie do strony logowania firmy Microsoft:

    [![](external-authentication-services/_static/image44.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image43.png)
6. Po wprowadzeniu poświadczeń firmy Microsoft i kliknij przycisk **Zaloguj**, zostanie wyświetlony monit, aby sprawdzić, czy aplikacja sieci web ma uprawnienia dostępu do Twojego konta Microsoft:

    [![](external-authentication-services/_static/image46.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image45.png)
7. Po kliknięciu **tak**, nastąpi przekierowanie przeglądarki sieci web do aplikacji sieci web, która wyświetli monit o **nazwy użytkownika** , którą chcesz skojarzyć z kontem Microsoft:

    [![](external-authentication-services/_static/image48.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image47.png)
8. Po wprowadzeniu nazwy użytkownika i kliknięciu **Zarejestruj** przycisku aplikacji sieci web zostanie wyświetlona domyślna **strona główna** do swojego konta Microsoft:

    [![](external-authentication-services/_static/image50.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image49.png)

<a id="TWITTER"></a>
## <a name="enabling-twitter-authentication"></a>Włączanie uwierzytelniania usługi Twitter

W usłudze Twitter uwierzytelniania, musisz utworzyć konto dewelopera i aby funkcjonować wymaga klucza klienta i klucz tajny klienta. Aby uzyskać informacje o tworzeniu konta dla deweloperów w usłudze Twitter i uzyskiwanie Twojego klucza klienta i klucz tajny klienta, zobacz [ https://go.microsoft.com/fwlink/?LinkID=252166 ](https://go.microsoft.com/fwlink/?LinkID=252166).

Po uzyskaniu usługi konsumenta oraz klucz tajny klienta, wykonaj następujące kroki, aby włączyć uwierzytelnianie usługi Twitter dla aplikacji sieci web:

1. Gdy projekt jest otwarty w programie Visual Studio 2013, otwórz *Startup.Auth.cs* pliku:

    [![](external-authentication-services/_static/image52.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image51.png)
2. Znajdź wyróżnioną sekcję kodu:

    [!code-csharp[Main](external-authentication-services/samples/sample8.cs)]
3. Usuń &quot; // &quot; znaków usuń znaczniki komentarza wyróżnione wiersze kodu, a następnie dodaj klucz klienta i klucz tajny klienta. Po dodaniu tych parametrów można ponownie skompilować projekt:

    [!code-csharp[Main](external-authentication-services/samples/sample9.cs)]
4. Po naciśnięciu klawisza F5, aby otworzyć aplikację sieci web w przeglądarce sieci web, zobaczysz, że Twitter został zdefiniowany jako usługi uwierzytelniania zewnętrznego:

    [![](external-authentication-services/_static/image54.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image53.png)
5. Po kliknięciu **Twitter** przycisku przeglądarki nastąpi przekierowanie do strony logowania usługi Twitter:

    [![](external-authentication-services/_static/image56.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image55.png)
6. Po wprowadź swoje poświadczenia usługi Twitter i kliknij przycisk **Autoryzuj aplikację**, przeglądarki sieci web zostanie przekierowany ponownie do aplikacji sieci web, która wyświetli monit o **nazwa_użytkownika** , którą chcesz skojarzyć z Twoje konto w usłudze Twitter:

    [![](external-authentication-services/_static/image58.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image57.png)
7. Po wprowadzeniu nazwy użytkownika i kliknięciu **Zarejestruj** przycisku aplikacji sieci web zostanie wyświetlona domyślna **strony głównej** dla konta usługi Twitter:

    [![](external-authentication-services/_static/image60.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image59.png)

<a id="MOREINFO"></a>
## <a name="additional-information"></a>Dodatkowe informacje

Aby uzyskać dodatkowe informacje na temat tworzenia aplikacji, które używają protokołu OAuth i OpenID zobacz następujące adresy URL:

- [https://go.microsoft.com/fwlink/?LinkID=252166](https://go.microsoft.com/fwlink/?LinkID=252166)
- [https://go.microsoft.com/fwlink/?LinkID=243995](https://go.microsoft.com/fwlink/?LinkID=243995)

<a id="COMBINE"></a>
### <a name="combining-external-authentication-services"></a>Łączenie zewnętrznych usług uwierzytelniania

Aby uzyskać większą elastyczność można zdefiniować wiele usług uwierzytelniania zewnętrznego, w tym samym czasie — dzięki temu usługi sieci web do użytkowników aplikacji użyć konta z dowolnej z usług jest włączone uwierzytelnianie zewnętrzne:

[![](external-authentication-services/_static/image62.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image61.png)

<a id="FQDN"></a>
### <a name="configuring-iis-express-to-use-a-fully-qualified-domain-name"></a>Konfiguracja usług IIS Express do użycia w pełni kwalifikowaną nazwę domeny

Niektórzy dostawcy uwierzytelniania zewnętrznego nie obsługują testowanie aplikacji przy użyciu adresu protokołu HTTP, takich jak `http://localhost:port/`. Aby obejść ten problem, można statyczne mapowanie w pełni kwalifikowanej domeny nazwę (FQDN) należy dodać do pliku HOSTS i skonfiguruj opcje projektu w programie Visual Studio 2013 na potrzeby testowania/debugowanie nazwy FQDN. Aby to zrobić, wykonaj następujące kroki:

- Dodaj nazwę FQDN statyczne mapowania pliku hostów:

  1. Otwórz wiersz polecenia z podwyższonym w Windows.
  2. Wpisz następujące polecenie:

      <kbd>Notatnik %WinDir%\system32\drivers\etc\hosts</kbd>
  3. Dodaj następujący wpis w pliku HOSTS:

      <kbd>127.0.0.1 www.wingtiptoys.com</kbd>
  4. Zapisz i zamknij plik HOSTS.

- Konfigurowanie projektu programu Visual Studio, aby użyć nazwy FQDN:

  1. Gdy projekt jest otwarty w programie Visual Studio 2013, kliknij przycisk **projektu** menu, a następnie wybierz pozycję Właściwości projektu. Na przykład, możesz wybrać **właściwości WebApplication1**.
  2. Wybierz **Web** kartę.
  3. Wprowadź nazwę FQDN dla <strong>projektu adres Url</strong>. Na przykład wprowadzisz <kbd> <http://www.wingtiptoys.com> </kbd> jeśli było mapowania nazwy FQDN, który został dodany do Twojego pliku HOSTS.

- Konfigurowanie usług IIS Express do używania nazwy FQDN dla twojej aplikacji:

    1. Otwórz wiersz polecenia z podwyższonym w Windows.
    2. Wpisz następujące polecenie, aby zmienić do folderu usługi IIS Express:

        <kbd>CD /d &quot;%ProgramFiles%\IIS Express&quot;</kbd>
    3. Wpisz następujące polecenie, aby dodać nazwę FQDN do aplikacji:

        <kbd>appcmd.exe set config-section:system.applicationHost/sites / +&quot;[name = "WebApplication1"] .bindings. [ Protokół = "http" bindingInformation = "*:80:www.wingtiptoys.com']&quot; /commit:apphost</kbd>

  Gdzie **WebApplication1** jest nazwą projektu i **bindingInformation** zawiera numer portu i nazwy FQDN, które chcesz użyć na potrzeby testów.

<a id="OBTAIN"></a>
### <a name="how-to-obtain-your-application-settings-for-microsoft-authentication"></a>Jak uzyskać ustawienia aplikacji dla uwierzytelniania firmy Microsoft

Łączenie aplikacji w usłudze Windows Live na usługę Microsoft Authentication jest prostym procesem. Jeśli aplikacja w usłudze Windows Live nie jest już połączone, można użyć następujące czynności:

1. Przejdź do [ https://go.microsoft.com/fwlink/?LinkID=144070 ](https://go.microsoft.com/fwlink/?LinkID=144070) i wprowadź nazwę konta Microsoft i hasło po wyświetleniu monitu, a następnie kliknij przycisk **Zaloguj**:

    [![](external-authentication-services/_static/image64.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image63.png)
2. Wprowadź nazwę i języka w aplikacji po wyświetleniu monitu, a następnie kliknij przycisk **akceptuję**:

    [![](external-authentication-services/_static/image66.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image65.png)
3. Na **ustawień interfejsu API** stronie aplikacji, wprowadź domenę przekierowania dla aplikacji i skopiuj **identyfikator klienta** i **klucz tajny klienta** dla projektu, a następnie Kliknij przycisk **Zapisz**:

    [![](external-authentication-services/_static/image68.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image67.png)

<a id="DISABLE"></a>
### <a name="optional-disable-local-registration"></a>Opcjonalnie: Wyłącz lokalną rejestrację.

Bieżącą funkcjonalność lokalnego rejestracji programu ASP.NET nie uniemożliwia tworzenie elementu członkowskiego kont; automatyczne programom (robotom) na przykład za pomocą zapobiegania bot i sprawdzanie poprawności technologii takich jak [CAPTCHA](../../../web-pages/overview/security/16-adding-security-and-membership.md). W związku z tym należy usunąć łącze formularza i rejestracji lokalny identyfikator logowania na stronie logowania. Aby to zrobić, otwórz  *\_Login.cshtml* strony w projekcie, a następnie przekształcić w komentarz wiersze dla panelu logowania lokalnego i link do rejestracji. Wynikowy strony powinien wyglądać jak w następującym przykładzie kodu:

[!code-html[Main](external-authentication-services/samples/sample10.html)]

Po wyłączeniu panelu logowania lokalnego i link do rejestracji, strona logowania będą wyświetlane tylko dostawców uwierzytelniania zewnętrznego, które mają włączone:

[![](external-authentication-services/_static/image70.png "Kliknij, aby rozwinąć obrazu")](external-authentication-services/_static/image69.png)
