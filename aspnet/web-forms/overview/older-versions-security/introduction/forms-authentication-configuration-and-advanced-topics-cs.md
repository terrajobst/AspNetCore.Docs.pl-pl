---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: Formularze konfiguracji uwierzytelniania oraz Tematy zaawansowane (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku będziemy zbadania różnych ustawień uwierzytelniania formularzy i sposobu ich modyfikacji przez element Form w temacie. Wiąże się to szczegółowe...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: d6578737478fb86f64be261925becc3adec33247
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="forms-authentication-configuration-and-advanced-topics-c"></a>Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> W tym samouczku będziemy zbadania różnych ustawień uwierzytelniania formularzy i sposobu ich modyfikacji przez element Form w temacie. Wiąże się to szczegółowe przyjrzeć się Dostosowywanie wartość limitu czasu biletu uwierzytelniania formularzy, za pomocą strony logowania z niestandardowy adres URL (np. SignIn.aspx zamiast Login.aspx) i biletów uwierzytelniania formularzy bez plików cookie.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczek](an-overview-of-forms-authentication-cs.md) analizujemy kroki niezbędne do wdrożenia w aplikacji ASP.NET z Określanie ustawień konfiguracji w pliku Web.config na tworzeniu dziennika na stronie do wyświetlania różnych uwierzytelniania formularzy zawartość dla użytkowników uwierzytelnionych i anonimowe. Odwołaj, że został skonfigurowany witryny sieci Web do używania uwierzytelniania formularzy, ustawiając atrybut tryb &lt;uwierzytelniania&gt; element formularzy. &lt;Uwierzytelniania&gt; element może opcjonalnie &lt;formularze&gt; element podrzędny, za pomocą których można określić pewną liczbę ustawień uwierzytelniania formularzy.

W tym samouczku będziemy zbada różnych ustawień uwierzytelniania formularzy i zapoznaj się z ich za pośrednictwem &lt;formularze&gt; elementu. Wiąże się to szczegółowe przyjrzeć się Dostosowywanie wartość limitu czasu biletu uwierzytelniania formularzy, za pomocą strony logowania z niestandardowy adres URL (np. SignIn.aspx zamiast Login.aspx) i biletów uwierzytelniania formularzy bez plików cookie. Możemy również dokładniejsze zbada organizację biletu uwierzytelniania formularzy i zobacz środki ostrożności, które przyjmuje ASP.NET w celu zapewnienia bezpiecznego z inspekcji i modyfikowaniu danych biletu. Ponadto przedstawiono sposób przechowywania danych użytkownika dodatkowe w biletu uwierzytelniania formularzy i model tych danych za pośrednictwem niestandardowy obiekt główny.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Krok 1: Sprawdzenie &lt;formularze&gt; ustawienia konfiguracji

System uwierzytelniania formularzy w programie ASP.NET oferuje wiele ustawień konfiguracji, które mogą być dostosowywane na podstawie aplikacji przez aplikację. Obejmuje to ustawienia, takie jak: uwierzytelnianie formularzy okres istnienia biletu; jakiego rodzaju ochrony zostały zastosowane do biletu; w obszarze rodzaju warunki bez plików cookie uwierzytelniania są używane bilety; Ścieżka do strony logowania; i inne informacje. Aby zmodyfikować wartości domyślne, należy dodać [ &lt;formularze&gt; elementu](https://msdn.microsoft.com/library/1d3t3c61.aspx) jako element podrzędny [ &lt;uwierzytelniania&gt; elementu](https://msdn.microsoft.com/library/532aee0e.aspx), określenie tych właściwości wartości, które chcesz dostosować jako atrybuty XML w następujący sposób:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

Tabela 1 zawiera podsumowanie właściwości, które można dostosować za pomocą &lt;formularze&gt; elementu. Ponieważ plik Web.config jest plik XML, nazw atrybutów w kolumnie po lewej stronie jest rozróżniana wielkość liter.


| <strong>Atrybut</strong> |                                                                                                                                                                                                                                     <strong>Opis</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         cookieless         |                                                                                                                Ten atrybut określa pod jakimi warunkami biletu uwierzytelniania są przechowywane w pliku cookie i osadzona w adresie URL. Dopuszczalne wartości to: UseCookies; UseUri; Autowykrywanie; i UseDeviceProfile (ustawienie domyślne). Krok 2 sprawdza, czy to ustawienie bardziej szczegółowo.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Określa adres URL, który użytkownicy są przekierowywani do po zalogowaniu się na stronie logowania, jeśli nie ma żadnej wartości RedirectUrl określony w zmiennej querystring. Wartość domyślna to plik default.aspx.                                                                                                                                                         |
|           domena           | Korzystając z bilety uwierzytelniania opartego na pliku cookie, to ustawienie określa wartość domeny pliku cookie. Wartość domyślna to pusty ciąg, który powoduje, że przeglądarka Użyj domeny, z którego zostało wydane (na przykład www.yourdomain.com). W takim przypadku plik cookie będzie <strong>nie</strong> wysłania w przypadku wprowadzania żądań poddomen, takich jak admin.yourdomain.com. Jeśli chcesz, aby plik cookie do przekazania do wszystkich domen podrzędnych, musisz dostosować atrybut domain ustawieniem dla niego twoja_domena.com. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Wartość logiczna wskazująca, czy użytkownicy uwierzytelnieni są zapamiętywane, gdy przekierowywane do adresów URL w innych aplikacjach sieci web na tym samym serwerze. Wartością domyślną jest false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      Adres URL strony logowania. Wartość domyślna to login.aspx.                                                                                                                                                                                                                      |
|            nazwa            |                                                                                                                                                                                                   Po użyciu uwierzytelniania opartego na pliku cookie biletów, nazwa pliku cookie. Wartość domyślna to. ASPXAUTH.                                                                                                                                                                                                   |
|            ścieżka            |                                                                             Korzystając z bilety uwierzytelniania opartego na pliku cookie, to ustawienie określa atrybut ścieżki pliku cookie. Atrybut ścieżki umożliwia dewelopera ograniczyć zakres pliku cookie do hierarchii określonego katalogu. Wartością domyślną jest /, który informuje przeglądarkę przesyłają żądanie do domeny pliku cookie biletu uwierzytelniania.                                                                              |
|         ochrona         |                                                                                                                                            Wskazuje, jaki techniki służą do ochrony biletu uwierzytelniania formularzy. Dopuszczalne wartości to: wszystkie (ustawienie domyślne); Szyfrowanie; Brak; i sprawdzania poprawności. Ustawienia te omówiono szczegółowo w kroku 3.                                                                                                                                            |
|         parametru requireSSL         |                                                                                                                                                                                Wartość logiczna wskazująca, czy do przesyłania pliku cookie uwierzytelniania jest wymagane połączenie SSL. Wartość domyślna to false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Wartość logiczna wskazująca, czy limit czasu plików cookie uwierzytelniania jest resetowany zawsze użytkownik odwiedza witrynę podczas jednej sesji. Wartość domyślna to true. Omówiono bardziej szczegółowo w Określanie zasad limitu czasu biletu uwierzytelniania biletu sekcji wartość limitu czasu.                                                                                                 |
|          Limit czasu           |                                                                                                                               Określa czas w minutach, po którym wygasa plik cookie biletu uwierzytelniania. Wartość domyślna to 30. Omówiono bardziej szczegółowo w Określanie zasad limitu czasu biletu uwierzytelniania biletu sekcji wartość limitu czasu.                                                                                                                               |

**Tabela 1**: Podsumowanie A &lt;formularze&gt; atrybuty elementu

W programie ASP.NET 2.0 i ponad wartość domyślną wartości uwierzytelniania formularzy są zakodowane na stałe w klasie FormsAuthenticationConfiguration w programie .NET Framework. Wszelkie zmiany, muszą być stosowane na podstawie aplikacji przez aplikacji w pliku Web.config. Ta różni się od ASP.NET 1.x, których wartości domyślne uwierzytelniania formularzy były przechowywane w pliku machine.config (i w związku z tym może zostać zmodyfikowany za pomocą edycji pliku machine.config). Czas na temat programu ASP.NET 1.x, warto wspomnieć o że wiele ustawień systemu uwierzytelniania formularzy mają przypisane wartości domyślne różnych w programie ASP.NET 2.0 i poza niż w programie ASP.NET 1.x. W przypadku migracji aplikacji ze środowiska 1.x ASP.NET, ważne jest pod uwagę następujące różnice. Zapoznaj się [ &lt;formularze&gt; dokumentacji technicznej element](https://msdn.microsoft.com/library/1d3t3c61.aspx) listę różnic.

> [!NOTE]
> Kilka ustawień uwierzytelniania formularzy, takie jak limit czasu, domeny i ścieżkę, określ szczegóły wynikowego pliku cookie biletu uwierzytelniania formularzy. Aby uzyskać więcej informacji na pliki cookie, jak działają i ich różnych właściwości odczytu [w tym samouczku plików cookie](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Określając wartość limitu czasu biletu

Biletu uwierzytelniania formularzy jest token reprezentujący tożsamości. Bilety uwierzytelniania opartego na pliku cookie ten token jest przechowywana w postaci pliku cookie i wysłane do serwera sieci web dla każdego żądania. Posiadanie tokenu, w zasadzie deklaruje, jestem *username*, już zalogowali się i jest używana co tożsamości użytkownika można zapamiętanych między wizyt strony.

Biletu uwierzytelniania formularzy nie tylko obejmuje tożsamości użytkownika, ale zawiera także informacje w celu zapewnienia integralności i bezpieczeństwa tokenu. Nie chcemy po wszystkich nefarious użytkownika, aby mieć możliwość utworzenia tokenu fałszywym lub zmodyfikować legit token w jakiś sposób underhanded.

Jest jeden bit takich informacji znajdujących się w bilecie *wygaśnięcia*, która jest data i godzina bilet nie jest już prawidłowy. Zawsze FormsAuthenticationModule sprawdza bilet uwierzytelnienia gwarantuje, że wygaśnięcia biletu jeszcze nie minęła. Jeśli ma ona pomija biletu i identyfikuje użytkownika jako anonimowy. To zabezpieczenie pomaga w ochronie przed atakami metodą powtórzeń. Bez wygaśnięcia, jeśli haker był w stanie uzyskać jej ręce na użytkownika prawidłowego biletu - prawdopodobnie fizyczny dostęp do komputera i umieszczanie w katalogu głównym, za pośrednictwem ich pliki cookie — one może wysłać żądania do serwera z tego biletu uwierzytelniania kradzieży i Uzyskaj wpisu. Gdy okres ważności nie uniemożliwia tego scenariusza, ogranicza okna, w którym będzie możliwe takiego ataku.

> [!NOTE]
> Krok 3 szczegóły dodatkowe techniki stosowane przez system uwierzytelniania formularzy do ochrony biletu uwierzytelniania.


Podczas tworzenia biletu uwierzytelniania, system uwierzytelniania formularzy określa jego wygaśnięcia, sprawdzając ustawienia limitu czasu. Zgodnie z opisem w tabeli 1, limit czasu ustawień domyślnych do 30 minut, co oznacza, że po utworzeniu biletu uwierzytelniania formularzy jego wygaśnięcia ma ustawioną datę i godzinę w przyszłości 30 minut.

Okres ważności definiuje bezwzględnego czasu w przyszłości wygaśnięcia biletu uwierzytelniania formularzy. Jednak zwykle deweloperzy zaimplementować przesuwanego wygaśnięcia, który jest resetowany za każdym razem, gdy użytkownik revisits lokacji. To zachowanie jest określana przez ustawienia slidingExpiration. Jeśli ma wartość true (ustawienie domyślne), zawsze FormsAuthenticationModule uwierzytelnia użytkownika, aktualizuje wygaśnięcia biletu. Jeśli ma wartość false, okres ważności nie zostanie zaktualizowana na każdym żądaniu, co powoduje biletu wygaśnie dokładnie limitu czasu liczba minut po pełnej po raz pierwszy bilet utworzony.

> [!NOTE]
> Po upływie przechowywane w biletu uwierzytelniania jest bezwzględną wartość daty i godziny, takich jak 2 sierpień 2008 11:34:00. Ponadto Data i godzina są względem czasu lokalnego serwera sieci web. Tej decyzji projektowej, może mieć niektóre ciekawe efekty uboczne wokół czasu (DST), czyli gdy zegary w Stanach Zjednoczonych zostaną przeniesione wyprzedzeniem godzinę (przy założeniu, że serwer sieci web znajduje się w ustawieniach regionalnych, w którym zaobserwowano czas letni). Należy rozważyć, co się stanie dla witryny sieci Web platformy ASP.NET z upływie 30 minut, w pobliżu godzinę rozpoczęcia czasu letniego (czyli o 2:00). Załóżmy, że użytkownik loguje się przy do lokacji 11 marca 2008 na 1:55:00. Spowoduje to wygenerowanie biletu uwierzytelniania formularzy, które wygasa w 11 marca 2008 o 2: AM 25 (w przyszłości 30 minut). Jednak po 2:00 AM przedstawia wokół, zegar przechodzi do 3:00 AM z powodu czasu letniego. Gdy użytkownik załaduje nową stronę sześciu minut po zalogowaniu się (na 3:01 AM), FormsAuthenticationModule uwagi, że bilet wygasł i przekierowuje użytkownika do strony logowania. Aby uzyskać dokładniejsze omówienie na to i inne oddities limitu czasu biletu uwierzytelniania, a także rozwiązania, wybierz kopię Stefan Schackow *Professional ASP.NET 2.0 zabezpieczeń, członkostwo i zarządzanie rolami* (ISBN: 978-0-7645-9698-8).


Rysunek 1 przedstawiono przepływ pracy po slidingExpiration jest ustawiona na false i limit czasu wynosi 30. Należy pamiętać, że bilet uwierzytelnienia generowane podczas logowania zawiera datę wygaśnięcia, a ta wartość nie jest aktualizowane na kolejnych żądań. Jeśli FormsAuthenticationModule stwierdzi, że bilet wygasł, odrzuca je i traktuje żądania jako użytkownik anonimowy.


[![Graficzna reprezentacja biletu uwierzytelniania formularzy, po upływie slidingExpiration ma wartość false](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Rysunek 01**: A graficzna reprezentacja biletu uwierzytelniania formularzy, po upływie slidingExpiration ma wartość false ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))


Na rysunku 2 przedstawiono przepływ pracy, gdy slidingExpiration ma ustawioną wartość PRAWDA, a limit czasu wynosi 30. Po odebraniu żądania uwierzytelnionego (z biletem — ważność) jego wygaśnięcia zostało zaktualizowane do limitu czasu liczbę minut w przyszłości.


[![Graficzna reprezentacja biletu uwierzytelniania formularzy po slidingExpiration ma wartość true](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Rysunek 02**: A graficzna reprezentacja biletu uwierzytelniania formularzy po slidingExpiration ma wartość true ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))


Podczas korzystania z uwierzytelniania opartego na pliku cookie biletów (ustawienie domyślne), staje się rozważania z nieco bardziej skomplikowane, ponieważ pliki cookie ma także własne expiries określony. Plik cookie wygaśnięcia (lub ich brak) przekazuje przeglądarce instrukcje gdy plik cookie powinien zostać zniszczone. Jeśli plik cookie nie ma wygaśnięcia, został zniszczony wyłączaniu przeglądarki. Jeśli wygaśnięcia, jednak plik cookie zostanie zachowany na komputerze użytkownika do dnia i upłynął czas określony w okres ważności. Gdy plik cookie zostanie zniszczony przez przeglądarkę, nie są wysyłane do serwera sieci web. W związku z tym zniszczenie plik cookie jest odpowiednikiem użytkownika rejestrowania poza lokacji.

> [!NOTE]
> Oczywiście użytkownika aktywnie usunąć pliki cookie przechowywanych na komputerze. W programie Internet Explorer 7 może przejść do menu Narzędzia, opcje i kliknij przycisk Usuń, w sekcji Historia przeglądania. Z tego miejsca kliknij przycisk Usuń pliki cookie.


System uwierzytelniania formularzy tworzy oparte na sesji lub ważności na podstawie plików cookie w zależności od wartości przekazanych do *persistCookie* parametru. Odwołania prowadzące metod klasy FormsAuthentication GetAuthCookie, SetAuthCookie i RedirectFromLoginPage w dwóch parametrów wejściowych: *username* i *persistCookie*. Strony logowania, utworzony w poprzednim samouczek uwzględnione Zapamiętaj moje dane pole wyboru, które ustalić, czy trwały plik cookie został utworzony. Trwałe pliki cookie są oparte na wygaśnięcia; trwałe pliki cookie są oparte na sesji.

Limit czasu i slidingExpiration omówione już dotyczą taki sam zarówno plików cookie opartego na sesji i wygaśnięcia. Istnieje tylko jeden niewielkie różnice w wykonywania: Jeśli ważności na podstawie plików cookie z slidingTimeout ma wartość true, wygaśnięcia pliku cookie jest aktualizowana tylko po ponad połowy określonego czasu upłynął.

Teraz zaktualizować zasad limitu czasu biletu uwierzytelniania naszą witrynę sieci Web, która biletów limit czasu po upływie godziny (60 minut), za pomocą wygasanie przewijania. Wpływ tej zmiany, zaktualizuj plik Web.config, dodawanie &lt;formularze&gt; elementu &lt;uwierzytelniania&gt; element zawierający następujący kod:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Przy użyciu adresu URL strony logowania innego niż Login.aspx

Ponieważ FormsAuthenticationModule automatyczne przekierowanie nieautoryzowanych użytkowników na stronę logowania, należy znać adres URL strony logowania. Ten adres URL jest określony przez atrybut loginUrl w &lt;formularze&gt; elementu, a wartość domyślna to login.aspx. Jeśli są Eksportowanie kodu za pośrednictwem istniejącej witryny sieci Web, może już istnieć z innym adresem URL, który został już zakładki i indeksowany przez wyszukiwarki strony logowania. Zamiast zmienianie nazw istniejących strona logowania do strony login.aspx i fundamentalne łącza i zakładki użytkowników, zamiast tego można zmodyfikować atrybutu loginUrl wskaż stronę logowania.

Na przykład, jeśli strona logowania miał nazwę SignIn.aspx i znajduje się w katalogu użytkowników, możesz punktu loginUrl ustawienia konfiguracji do ~/Users/SignIn.aspx w następujący sposób:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Ponieważ już naszych bieżącej aplikacji jest strona logowania o nazwie Login.aspx, nie istnieje potrzeba do określenia wartości niestandardowych w &lt;formularze&gt; elementu.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Krok 2: Za pomocą biletów uwierzytelniania formularzy bez plików cookie

Domyślnie system uwierzytelniania formularzy Określa, czy do przechowywania jego biletów uwierzytelniania w kolekcji plików cookie lub osadzić w adresie URL oparte na tej witrynie agenta użytkownika. Wszystkie typowe przeglądarek komputerowych, takich jak program Internet Explorer, Firefox, Opera i Safari, obsługa plików cookie, ale nie wszystkie urządzenia przenośne.

Zasady pliku cookie uwierzytelniania formularzy systemu zależy od ustawienia bez plików cookie w &lt;formularze&gt; element, który można przypisać jedną cztery wartości:

- UseCookies — Określa, że bilety uwierzytelniania opartego na plik cookie będzie zawsze używana usługa.
- UseUri - wskazuje bilety uwierzytelniania opartego na pliku cookie nigdy nie będą używane.
- Autowykrywanie — Jeśli profil urządzenia obsługuje pliki cookie, bilety uwierzytelniania opartego na pliku cookie nie są używane; Jeśli profil urządzenia obsługuje pliki cookie, mechanizmu sondowania służy do określenia, czy pliki cookie są włączone.
- UseDeviceProfile — domyślnie; używa biletów uwierzytelniania na podstawie plików cookie tylko, jeśli profil urządzenia obsługuje pliki cookie. Nie mechanizmu sondowania jest używany.

Zależne od ustawienia Autowykrywanie i UseDeviceProfile *profil urządzenia* w upewnieniu się, czy ma być używany biletów uwierzytelniania pliku cookie lub bez plików cookie. Program ASP.NET używa bazy danych z różnych urządzeń i ich funkcji, takich jak czy obsługują one plików cookie, jakiej wersji JavaScript, które obsługują i tak dalej. Zawsze urządzenie zażąda strony sieci web z serwera sieci web wysyła wzdłuż *agenta użytkownika* nagłówka HTTP, który identyfikuje typ urządzenia. Program ASP.NET automatycznie dopasowuje ciąg agenta użytkownika podane odpowiedni profil określony w swojej bazie danych.

> [!NOTE]
> Ta baza danych możliwości dotyczące urządzeń są przechowywane w liczbę plików XML, która jest zgodna [schematu pliku definicji przeglądarki](https://msdn.microsoft.com/library/ms228122.aspx). Domyślne pliki profilu urządzenia znajdują się w lokalizacji % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Można również dodać niestandardowe pliki do aplikacji App\_folderu przeglądarki. Aby uzyskać więcej informacji, zobacz [jak: typy przeglądarek wykryć stron ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Ustawieniem domyślnym jest UseDeviceProfile, biletów uwierzytelniania formularzy cookieless będą używane po odwiedzeniu witryny przez urządzenie, którego profil zgłasza, że nie obsługuje pliki cookie.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Kodowanie biletu uwierzytelniania w adresie URL

Pliki cookie są fizyczną średniej dla wraz z informacjami z przeglądarki do wszystkich żądań do określonej witryny internetowej, dlatego domyślne ustawienia uwierzytelniania formularzy używać plików cookie, jeśli je obsługuje zaproszonych urządzenia. Jeśli pliki cookie nie są obsługiwane, należy zastosować alternatywne metody przekazywania bilet uwierzytelnienia od klienta do serwera. Typowe rozwiązania, używany w środowiskach bez plików cookie jest aby zakodować dane pliku cookie w adresie URL.

Najlepszym sposobem, aby zobaczyć, jak takie informacje mogą być osadzone w adresie URL jest wymuszenie lokacji do używania biletów bez plików cookie uwierzytelniania. Można to osiągnąć poprzez ustawienie ustawienia konfiguracji bez plików cookie do UseUri:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Po utworzeniu tej zmiany, odwiedź witrynę za pośrednictwem przeglądarki. Podczas odwiedzania jako użytkownik anonimowy, adresy URL będzie wyglądać dokładnie tak samo jak przed. Na przykład podczas odwiedzania strony Default.aspx paska adresu w przeglądarce zawiera następujący adres URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Jednak po zalogowaniu biletu uwierzytelniania formularzy jest osadzony w adresie URL. Na przykład po odwiedzając stronę logowania i logowanie jako Sam, m I powrót do strony Default.aspx, ale adres URL jest teraz:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Biletu uwierzytelniania formularzy zostały osadzone w adresie URL. Ciąg (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) reprezentuje dane biletu uwierzytelniania zakodowane w systemie szesnastkowym i jest tych samych danych, który zazwyczaj znajduje się w pliku cookie.

Aby biletów uwierzytelniania bez plików cookie do pracy system zakodować wszystkie adresy URL na stronie Aby dołączyć dane biletu uwierzytelniania, w przeciwnym razie biletu uwierzytelniania zostaną utracone po kliknięciu łącza. Thankfully tę logikę osadzania odbywa się automatycznie. Aby zademonstrować tę funkcję, otwórz stronę Default.aspx i Dodaj formant hiperłącza, ustawienie właściwości Text i NavigateUrl łącza testowego i SomePage.aspx, odpowiednio. Nie ma znaczenia, czy rzeczywiście nie ma strony w naszym projektu o nazwie SomePage.aspx.

Zapisz zmiany Default.aspx, a następnie odwiedź go za pośrednictwem przeglądarki. Zaloguj się do witryny, aby biletu uwierzytelniania formularzy jest osadzony w adresie URL. Następnie z Default.aspx, kliknij przycisk Test łącza. Co się stało? Jeśli istnieje żadna strona o nazwie SomePage.aspx, następnie wystąpił błąd 404, ale to nie jest ważne, w tym miejscu. Zamiast tego należy skoncentrować się na pasku adresu w przeglądarce. Należy pamiętać, że zawiera on biletu uwierzytelniania formularzy w adresie URL!

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

SomePage.aspx adres URL w łączu został automatycznie przekonwertowany do adresu URL, który się biletu uwierzytelniania — nie mamy zapisu lizawki kodu! Biletu uwierzytelniania formularzy automatycznie zostaną osadzone w adresie URL dla dowolnego hiperłącza, które nie rozpoczynają się od http:// lub /. Nie ma znaczenia, jeśli hiperłącze pojawi się w wywołaniu Response.Redirect, kontroli HyperLink lub element kotwicy HTML (tj. &lt;href = "..."&gt;... &lt;/a&gt;). Tak długo, jak adres URL nie jest podobny do http://www.someserver.com/SomePage.aspx lub /SomePage.aspx, formularze bilet uwierzytelnienia zostanie osadzony w firmie Microsoft.

> [!NOTE]
> Biletów uwierzytelniania formularzy cookieless stosować się do tych samych zasad limitu czasu jako bilety uwierzytelniania opartego na pliku cookie. Jednak biletów bez plików cookie uwierzytelniania są bardziej podatne na ataki, ponieważ biletu uwierzytelniania jest osadzony bezpośrednio w adresie URL. Wyobraź sobie użytkownik odwiedza witrynę sieci Web, loguje, a następnie wkleja adres URL w wiadomości e-mail do współpracownika. Jeśli współpracownika kliknie łącze, przed wygaśnięciem, ich w będą rejestrowane jako użytkownik, który wysłał wiadomość e-mail!


## <a name="step-3-securing-the-authentication-ticket"></a>Krok 3: Zabezpieczanie biletu uwierzytelniania

Biletu uwierzytelniania formularzy są przesyłane przez sieć w pliku cookie lub osadzonego bezpośrednio w adresie URL. Oprócz informacji o tożsamości biletu uwierzytelniania może także zawierać dane użytkownika (jak zostanie wyświetlone w kroku 4). W związku z tym, ważne jest, że są szyfrowane dane biletu z wścibskimi oczu i (nawet ważniejsze) czy system uwierzytelniania formularzy może zagwarantować, czy bilet nie została zmieniona.

Aby zapewnić poufności danych biletu, system uwierzytelniania formularzy można zaszyfrować dane biletu. Nie można zaszyfrować dane biletu wysyła potencjalnie wrażliwe informacje przez sieć jako zwykły tekst.

Zagwarantowanie autentyczności biletu uwierzytelniania formularzy systemu musi *zweryfikować* biletu. Sprawdzanie poprawności jest czynnością zapewnienie, że do określonego elementu danych nie został zmodyfikowany i odbywa się za pośrednictwem  *[komunikatu kod uwierzytelniania (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*. Mówiąc MAC jest mała ilość informacji, które identyfikuje dane, które musi być weryfikowane (w tym przypadku bilet). W przypadku modyfikacji danych reprezentowanego przez MAC następnie MAC i dane zostaną niezgodny. Ponadto jest praktyce twardych hakerom zarówno modyfikować dane i wygenerować własną MAC do modyfikacji danych.

Podczas tworzenia (lub modyfikowania) biletu uwierzytelniania formularzy systemu tworzy MAC i dołącza go do danych biletu. Nadejściu kolejne żądania uwierzytelniania formularzy systemu porównuje dane MAC i biletu umożliwia sprawdzenie oryginalności danych biletu. Rysunek 3 przedstawia graficznie ten przepływ pracy.


[![Autentyczności biletu jest zapewnione MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Rysunek 03**: autentyczności biletu jest zapewnione MAC ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))


Jakie środki zabezpieczeń są stosowane do biletu uwierzytelniania zależy od ustawienia ochrony w &lt;formularze&gt; elementu. Ustawienia ochrony mogą zostać przypisane do jednej z trzech następujących wartości:

- Wszystkie --ticket jest zaszyfrowany i podpisany cyfrowo (ustawienie domyślne).
- Stosowane jest szyfrowanie - szyfrowanie tylko — MAC nie zostanie wygenerowany.
- Brak --ticket nie jest zaszyfrowany ani podpisany cyfrowo.
- Sprawdzanie poprawności — MAC jest generowany, ale dane biletu jest przesyłane przez sieć jako zwykły tekst.

Firma Microsoft zaleca użycie wszystkie ustawienia.

### <a name="setting-the-validation-and-decryption-keys"></a>Ustawienia weryfikacji i kluczy Odszyfrowujących

Szyfrowania i tworzenia skrótów algorytmów używanych przez system uwierzytelniania formularzy, szyfrowanie i sprawdzanie poprawności biletu uwierzytelniania są można dostosować za pomocą [ &lt;machineKey&gt; elementu](https://msdn.microsoft.com/library/w8h3skw9.aspx) w pliku Web.config. Tabela 2 zawiera &lt;machineKey&gt; atrybuty elementu i ich możliwe wartości.

| **Atrybut** | **Opis** |
| --- | --- |
| odszyfrowywanie | Wskazuje algorytm używany do szyfrowania. Ten atrybut może mieć jeden z następujące cztery wartości: - Auto - domyślnie; Określa algorytm, w zależności od długości atrybutu decryptionKey. -Używa AES - [Advanced Encryption (Standard AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algorytmu. -Używa DES - [Data Encryption (Standard DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) ten algorytm jest uznawany za słabe istnieje i nie powinna być używana. Używa - 3DES - [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algorytmu, który działa przez zastosowanie algorytmu 3DES trzy razy. |
| decryptionKey | Klucz tajny, używane przez algorytm szyfrowania. Ta wartość musi być ciągiem szesnastkowym odpowiednich długości (na podstawie wartości odszyfrowywanie), automatyczne generowanie lub albo wartość, IsolateApps. Dodawanie IsolateApps program ASP.NET do użycia unikatową wartość dla każdej aplikacji. Wartość domyślna to automatyczne, IsolateApps. |
| walidacja | Wskazuje algorytm używany do sprawdzania poprawności. Ten atrybut może mieć jeden z następujące cztery wartości: - AES - używa algorytmu szyfrowania AES (Advanced Standard). -Używa MD5 - [Message Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algorytmu. -Używa SHA1 - [SHA1](http://en.wikipedia.org/wiki/Sha1) algorytm (ustawienie domyślne). -3DES - używa algorytm Triple DES. |
| validationKey | Klucz tajny, używane przez algorytm sprawdzania poprawności. Ta wartość musi być ciągiem szesnastkowym odpowiednich długości (na podstawie wartości w weryfikacji), automatyczne generowanie lub albo wartość, IsolateApps. Dodawanie IsolateApps program ASP.NET do użycia unikatową wartość dla każdej aplikacji. Wartość domyślna to automatyczne, IsolateApps. |

**Tabela 2**: &lt;machineKey&gt; atrybuty elementu

Dogłębną dyskusję tych opcji szyfrowanie i sprawdzanie poprawności i specjalistów i wad różnych algorytmów wykracza poza zakres tego samouczka. Dla szczegółowe przyjrzeć się tych problemów oraz wskazówki dotyczące jakie algorytmów szyfrowania i odszyfrowywania do użycia, jakie długości kluczy, aby użyć i jak najlepiej wygenerowania tych kluczy, zapoznaj się *Professional ASP.NET 2.0 zabezpieczeń, członkostwo i zarządzanie rolami* .

Domyślnie kluczy używanych do szyfrowania i odszyfrowywania są generowane automatycznie dla każdej aplikacji, a te klucze są przechowywane w urzędu zabezpieczeń lokalnych (LSA). Krótko mówiąc domyślne ustawienia zagwarantować unikatowy kluczy dla serwera sieci web serwera sieci web i aplikacji w aplikacji, co. W związku z tym to domyślne zachowanie nie będzie działać w dwóch następujących scenariuszach:

- **Kolektywów serwerów sieci Web** — w [kolektywu serwerów sieci web](http://en.wikipedia.org/wiki/Web_farm) scenariuszu aplikacji sieci web jednej znajduje się na wielu serwerach sieci web na potrzeby skalowalność i nadmiarowość. Każdego żądania przychodzącego jest wysyłane do serwera w farmie, oznacza, że w okresie istnienia sesji użytkownika różne serwery mogą stosowany do obsługi jego różnych żądań. W związku z tym każdy serwer musi używać tych samych kluczy szyfrowania i odszyfrowywania, aby utworzyć biletu uwierzytelniania formularzy, można odszyfrować zaszyfrowane i zweryfikowane na jednym serwerze i zweryfikowane na innym serwerze w farmie.
- **Dla wielu aplikacji do udostępniania biletu** -jednym serwerze sieci web mogą hostować wiele aplikacji programu ASP.NET. Jeśli potrzebujesz tych różnych aplikacji do udostępniania biletu uwierzytelniania formularzy pojedynczego, konieczne jest czy pasują do kluczy szyfrowania i odszyfrowywania.

Podczas pracy w farmie sieci web, ustawień lub udostępniania biletów uwierzytelniania w aplikacjach na tym samym serwerze, należy skonfigurować &lt;machineKey&gt; element dotyczy aplikacji, aby ich decryptionKey i validationKey wartości są zgodne.

Żadne z powyższych scenariuszy dotyczy naszego przykładowej aplikacji, firma Microsoft nadal określ wartości validationKey i decryptionKey jawne i zdefiniuj algorytmy, które mają być używane. Dodaj &lt;machineKey&gt; ustawienie do pliku Web.config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Aby uzyskać więcej informacji, zapoznaj się z [jak: Konfigurowanie MachineKey w programie ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Wartości decryptionKey i validationKey zostały pobrane z [Steve Gibson](http://www.grc.com/stevegibson.htm)w [strony sieci web doskonałe hasła](https://www.grc.com/passwords.htm), generująca 64 losowo wybranych znaków szesnastkowych wizyty każdej strony. Aby zmniejszyć prawdopodobieństwo te klucze wprowadzania sposób ich do aplikacji produkcyjnych, zachęca się zastąpić powyżej klucze losowo generowany widocznych na stronie doskonałe hasła.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Krok 4: Przechowywania danych użytkownika dodatkowe w bilecie

Wiele aplikacji sieci web wyświetlić informacje o lub podstawowa wyświetlania strony na aktualnie zalogowanego użytkownika. Na przykład strony sieci web mogą być wyświetlane nazwy użytkownika i datę ona ostatniego zalogowanego w górnym rogu każdej strony. Biletu uwierzytelniania formularzy przechowuje obecnie zalogowanego użytkownika, ale w razie potrzeby inne informacje strony musi przejdź do Sklepu użytkownika — zwykle bazy danych — do wyszukiwania informacji nie są przechowywane w biletu uwierzytelniania.

Dodatkowe informacje dotyczące użytkownika z niewielki kod można są przechowywane w biletu uwierzytelniania formularzy. Dane te można wyrazić za pomocą [klasy FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)w [właściwości danych użytkownika](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). Jest to przydatne miejsce do umieszczania niewielkich ilości informacji o użytkowniku, który jest zwykle potrzebne. Wartość określona w danych użytkownika właściwości wchodzi w skład pliku cookie biletu uwierzytelniania i, podobnie jak inne pola biletu, zostaje zaszyfrowany i zweryfikowane na podstawie systemu uwierzytelniania formularzy konfiguracji. Domyślnie danych użytkownika jest pustym ciągiem.

W celu przechowywania danych użytkownika w bilecie uwierzytelniania, należy zapisać fragmentem kodu na stronie logowania, która grabs informacje specyficzne dla użytkownika i zapisuje go w bilecie. Ponieważ danych użytkownika jest właściwością typu String, przechowywanych w nim danych należy prawidłowo serializacji jako ciąg. Załóżmy na przykład naszym magazynie użytkownika uwzględnione każdego użytkownika daty urodzenia i nazwę pracodawcy, czy możemy do przechowywania wartości tych dwóch właściwości w biletu uwierzytelniania. Firma Microsoft można serializować te wartości na ciąg, łącząc użytkownika daty urodzenia w ciągu symbolem kreski pionowej (|), a po niej nazwę pracodawcy. Dla użytkownika na świat w 15 sierpnia 1974, który działa w przypadku Northwind Traders, możemy przypisywanej właściwości danych użytkownika ciąg: 1974-08-15 | Northwind Traders.

Zawsze, gdy potrzebne do dostępu do danych przechowywanych w bilecie, firma Microsoft to zrobić przez Przechwytywanie FormsAuthenticationTicket bieżącego żądania i deserializacji właściwości danych użytkownika. W przypadku daty urodzenia i pracodawcy przykładowe nazwy firma Microsoft będzie podzielone ciągu danych użytkownika dwóch podciągów oparte na ogranicznika (|).


[![Dodatkowe informacje dotyczące użytkownika mogą być przechowywane w biletu uwierzytelniania](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Rysunek 04**: dodatkowe użytkownika informacje mogą być przechowywane w biletu uwierzytelniania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))


### <a name="writing-information-to-userdata"></a>Zapisywanie informacji do danych użytkownika

Niestety Dodawanie informacji o użytkowniku do biletu uwierzytelniania formularzy nie jest równie proste, co może spodziewać się. Właściwości danych użytkownika z klasy FormsAuthenticationTicket jest tylko do odczytu i może być określony tylko za pośrednictwem konstruktora klasy FormsAuthenticationTicket. Podczas określania właściwości danych użytkownika w konstruktorze, również należy podać bilet przez inne wartości: nazwa, Data wystawienia, wygaśnięcia i tak dalej. Gdy utworzyliśmy stronę logowania w poprzednim samouczek to wszystkie obsłużono firmie Microsoft przez klasę uwierzytelniania formularzy. Podczas dodawania danych użytkownika do FormsAuthenticationTicket, musimy napisać kod, aby replikować większość funkcje już udostępniane przez klasę uwierzytelniania formularzy.

Przyjrzyjmy się konieczne kodu do pracy z danych użytkownika, aktualizując strony Login.aspx, aby zarejestrować dodatkowe informacje na temat użytkownikowi biletu uwierzytelniania. Podawać czy naszym magazynie użytkownika zawiera informacje dotyczące firmy użytkownika działa w przypadku i ich tytuł i chcemy przechwycić te informacje w biletu uwierzytelniania. Aktualizacja obsługi zdarzeń kliknięcia LoginButton strony Login.aspx, dzięki czemu kod wygląda następująco:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Teraz krok ten wiersz kodu w czasie. Metoda uruchamiania, definiując czterech tablic ciągów: użytkowników, hasła NazwaFirmy i titleAtCompany. Te tablice przechowywane nazwy użytkowników, hasła, nazwy firmy i tytułów dla kont użytkowników w systemie, z których istnieją trzy: Scott Jisun i Sam. W rzeczywistej aplikacji te wartości będzie można wykonać zapytania z magazynu użytkowników nie ustalony w kodzie źródłowym strony.

W poprzednich instrukcji Jeśli podane poświadczenia są prawidłowe po prostu dzwoniliśmy FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked), które są wykonywane następujące czynności:

1. Utworzony biletu uwierzytelniania formularzy
2. Zapisano bilet do odpowiedniego magazynu. Biletu uwierzytelniania na podstawie plików cookie kolekcji plików cookie w przeglądarce jest używany; dla biletów bez plików cookie uwierzytelniania dane biletu jest serializowany do adresu URL
3. Przekierowanie użytkownika do odpowiedniej strony

Te kroki są replikowane w powyższym kodzie. Po pierwsze ciąg, który ostatecznie zostanie przechowujemy we właściwości danych użytkownika jest tworzony przez połączenie nazwy firmy i tytuł oddzielającego te dwie wartości z znaku kreski pionowej (|).

ciąg userDataString = ciąg. Concat (NazwaFirmy [i] "|", titleAtCompany[i]);

Następnie FormsAuthentication.GetAuthCookie wywoływana jest metoda, która tworzy bilet uwierzytelniania, są szyfrowane i weryfikuje on zgodnie z ustawieniami konfiguracji i umieszcza je w obiekcie HttpCookie.

HttpCookie authCookie = FormsAuthentication.GetAuthCookie(UserName.Text, RememberMe.Checked);

Aby pracować z FormAuthenticationTicket osadzone w pliku cookie, należy wywołać klasy FormAuthentication [odszyfrować metody](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), przekazując wartość pliku cookie.

Bilet FormsAuthenticationTicket = FormsAuthentication.Decrypt(authCookie.Value);

Następnie utwórz *nowe* FormsAuthenticationTicket wystąpienia na podstawie istniejących FormsAuthenticationTicket wartości. Jednak ten nowy bilet zawiera informacje specyficzne dla użytkownika (userDataString).

FormsAuthenticationTicket newTicket = nowe FormsAuthenticationTicket (biletu. Wersja, biletu. Nazwa, biletu. IssueDate, biletu. Wygaśnięcie, biletu. IsPersistent, userDataString);

Firma Microsoft następnie szyfrowania (i zweryfikować) nowe wystąpienie FormsAuthenticationTicket przez wywołanie metody [szyfrowania metody](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)i te dane zaszyfrowane (i zweryfikowane) ponownie poddane authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

Na koniec authCookie zostanie dodany do kolekcji Response.Cookies i wywoływana jest metoda GetRedirectUrl określić odpowiednią stronę, aby wysłać do użytkownika.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Wszystkie tego kodu jest potrzebna, ponieważ właściwość danych użytkownika jest tylko do odczytu i klasa uwierzytelniania formularzy nie zapewnia żadnych metod do określania informacji danych użytkownika w jego metody GetAuthCookie, SetAuthCookie lub RedirectFromLoginPage.

> [!NOTE]
> Kod, który właśnie badane są przechowywane informacje specyficzne dla użytkownika biletu uwierzytelniania na podstawie plików cookie. Klasy odpowiedzialny za serializacji biletu uwierzytelniania formularzy do adresu URL są wewnętrzne dla programu .NET Framework. Długi tekst krótkie, nie można przechowywać dane użytkownika w biletu uwierzytelniania formularzy bez plików cookie.


### <a name="accessing-the-userdata-information"></a>Uzyskiwanie dostępu do informacji danych użytkownika

W tym momencie nazwy firmy i tytuł każdego użytkownika są przechowywane w biletu uwierzytelniania formularzy właściwości danych użytkownika podczas logowania. Te informacje są dostępne z biletu uwierzytelniania na każdej stronie bez konieczności podróży w magazynie użytkownika. Aby zilustrować, jak te informacje można pobrać z właściwości danych użytkownika, możemy zaktualizować Default.aspx tak, aby jego komunikat powitalny obejmuje nie tylko nazwę użytkownika, ale również firmy, które pracują w i ich tytuł.

Obecnie Default.aspx zawiera panelu AuthenticatedMessagePanel za pomocą formantu etykiety o nazwie WelcomeBackMessage. Ten Panel jest wyświetlana tylko użytkownicy uwierzytelnieni. Zaktualizuj kod Default.aspx na stronie\_obciążenia programu obsługi zdarzeń, aby wyglądały one podobnie do następującej:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Jeśli Request.IsAuthenticated ma wartość true, właściwość Text WelcomeBackMessage najpierw ustawiono Zapraszamy ponownie *username*. Następnie właściwość User.Identity jest rzutowane na obiekt FormsIdentity tak, aby firma Microsoft mogą uzyskiwać dostęp do podstawowych FormsAuthenticationTicket. Gdy mamy FormsAuthenticationTicket możemy deserializować właściwości danych użytkownika z nazwy firmy i tytułu. Jest to osiągane przez podzielić ciąg na znaku kreski pionowej. Nazwa firmy i tytuł są następnie wyświetlane w etykiecie WelcomeBackMessage.

Rysunek 5. pokazuje zrzut ekranu tego ekranu w akcji. Logowanie jako Scott wyświetla komunikat powitalny Wstecz, zawierającą Scotta firmy i tytuł.


[![Wyświetlane są obecnie zarejestrowane na tego użytkownika firmy i tytuł](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Rysunek 05**: obecnie zalogowanego użytkownika w firmie i tytuł są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))


> [!NOTE]
> Właściwość danych użytkownika biletu uwierzytelniania służy jako pamięci podręcznej magazynu użytkowników. Podobnie jak wszelkie pamięci podręcznej należy zaktualizować, modyfikacji danych. Na przykład w przypadku strony sieci web, z której użytkownicy mogą aktualizować swój profil musi zostać odświeżona pola we właściwości danych użytkownika w pamięci podręcznej w celu odzwierciedlenia zmian wprowadzonych przez użytkownika.


## <a name="step-5-using-a-custom-principal"></a>Krok 5: Niestandardowy podmiot zabezpieczeń za pomocą

Dla każdego żądania przychodzącego FormsAuthenticationModule podejmuje próbę uwierzytelnienia użytkownika. Jeśli biletu uwierzytelniania wygasła, FormsAuthenticationModule przypisuje właściwości HttpContext.User do nowego obiektu GenericPrincipal. Ten obiekt GenericPrincipal ma tożsamość typu FormsIdentity, który zawiera odwołanie do biletu uwierzytelniania formularzy. Klasa GenericPrincipal zawiera systemu od zera minimalne funkcje wymagane przez klasę zawierającą implementację interfejsu IPrincipal — wystarczy ma właściwość tożsamości i metody IsInRole.

Obiekt główny ma dwa obowiązki: aby wskazać role, jakie użytkownik należy do oraz udostępniają informacje o tożsamości. Jest to realizowane za pośrednictwem interfejsu IPrincipal IsInRole (*roleName*) metody i właściwości Identity odpowiednio. Klasa GenericPrincipal umożliwia tablica ciągów nazw ról można określić za pomocą jego konstruktora; jego IsInRole (*roleName*) metoda jedynie sprawdza, czy przekazany w *roleName* istnieje w tablicy ciągów. Gdy FormsAuthenticationModule tworzy GenericPrincipal, przechodzą w tablicy pusty ciąg do konstruktora GenericPrincipal. W związku z tym każde wywołanie IsInRole zawsze zwraca wartość false.

Klasa GenericPrincipal spełnia potrzeby większości scenariuszy uwierzytelnianie oparte na formularzach, gdzie role nie są używane. Dla tych sytuacji, gdy domyślny obsługi roli są niewystarczające lub gdy należy skojarzyć z użytkownikiem niestandardowych obiektów tożsamości, można utworzyć niestandardowego obiektu interfejsu IPrincipal podczas przepływu pracy uwierzytelniania i przypisz je do właściwości HttpContext.User.

> [!NOTE]
> Jak zostanie wyświetlone w przyszłości samouczki, gdy ASP. Włączono jego NET framework ról tworzy niestandardowy obiekt główny typu [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) i zastępuje obiekt GenericPrincipal utworzone uwierzytelniania formularzy. Dzieje się tak, aby dostosować metody IsInRole podmiotu na potrzeby interfejsu z interfejsu API w ramach ról.


Ponieważ firma Microsoft ma nie dotyczy nad z rolami jeszcze, tylko urząd, który mamy do tworzenia niestandardowego podmiotu w tym momencie jest kojarzenie niestandardowych obiektów tożsamości do podmiotu zabezpieczeń. W kroku 4 analizujemy przechowywania dodatkowe informacje dotyczące użytkownika we właściwości danych użytkownika biletu uwierzytelniania, w szczególności nazwa firmy użytkownika i ich tytułu. Informacje o danych użytkownika jest jednak tylko dostępny za pośrednictwem biletu uwierzytelniania, a następnie tylko jako ciąg serializacji, co oznacza, że w dowolnym momencie chcemy wyświetlić dane użytkownika przechowywane w bilecie należy przeanalizować właściwości danych użytkownika.

Możemy ulepszyć środowisko dewelopera, tworząc klasę, która implementuje tożsamości i zawiera właściwości NazwaFirmy i tytuł. W ten sposób deweloperzy mogą uzyskiwać dostęp do aktualnie zalogowanego użytkownika, nazwy firmy i tytuł bezpośrednio za pomocą właściwości NazwaFirmy i tytułu, bez potrzeby wiedzieć, jak można przeanalizować właściwości danych użytkownika.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Tworzenie niestandardowej tożsamości i klasy głównej

W tym samouczku, umożliwia tworzenie niestandardowych obiektów Principal role i tożsamości w aplikacji\_katalogu z kodem. Rozpocznij od dodania aplikacji\_kodu folderu do projektu — kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wybierz opcję Dodaj Folder ASP.NET i wybierz aplikację\_kodu. Aplikacja\_kodu jest folderem specjalne ASP.NET przechowujący klasy pliki specyficzne dla witryny sieci Web.

> [!NOTE]
> Aplikacja\_katalogu z kodem należy używać tylko, gdy zarządzania projektem za pośrednictwem modelu projektu witryny sieci Web. Jeśli używasz [modelu projektu aplikacji sieci Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), Utwórz folder standardowe i dodać do tej klasy. Można na przykład dodać nowy folder o nazwie klasy, a umieść swój kod.


Następnie dodaj dwa nowe pliki klasy do aplikacji\_katalogu z kodem, jeden CustomIdentity.cs nazwane i jedną o nazwie CustomPrincipal.cs.


[![Dodawanie klasy CustomPrincipal i CustomIdentity do projektu](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Rysunek 06**: Dodaj CustomIdentity i CustomPrincipal klas do projektu Your ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))


Klasa CustomIdentity jest odpowiedzialny za implementującej interfejs tożsamości, która definiuje właściwości AuthenticationType, IsAuthenticated i nazwę. Oprócz tych wymagane właściwości Dbamy o udostępnianie podstawowej biletu uwierzytelniania formularzy oraz jak właściwości dla użytkownika nazwa firmy i tytuł. Wprowadź następujący kod do klasy CustomIdentity.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Należy pamiętać, że klasa zawiera zmienną członkowską FormsAuthenticationTicket (\_biletu) i że należy podać te informacje biletu za pośrednictwem konstruktora. Te dane biletu jest używany w zwracanie nazwy tożsamości; Aby zwrócić wartości dla właściwości NazwaFirmy i tytuł jest analizowana z jego właściwość danych użytkownika.

Następnie należy utworzyć klasa CustomPrincipal. Ponieważ firma Microsoft nie są związane z ról w tym momencie, klasa CustomPrincipal Konstruktor akceptuje tylko obiekt CustomIdentity; metody IsInRole zawsze zwraca wartość false.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Przypisywanie obiekcie CustomPrincipal kontekst zabezpieczeń żądania przychodzącego

Mamy teraz klasę, która rozszerza specyfikacji tożsamości domyślnej właściwościami NazwaFirmy i tytułu, a także niestandardowe klasy głównej, która korzysta z tożsamości niestandardowej. Firma Microsoft jest gotowe do Wkrocz do potoku platformy ASP.NET i przypisać naszych niestandardowy obiekt główny kontekst zabezpieczeń żądania przychodzącego.

Potoku ASP.NET przyjmuje przychodzącego żądania i przetwarza je za pośrednictwem kilku kroków. W każdym kroku określonego zdarzenie jest wywoływane, co umożliwia deweloperom naciśnij w potoku platformy ASP.NET i modyfikować żądania w niektórych punktach cykl życia. FormsAuthenticationModule, czeka na przykład ASP.NET podnieść [zdarzeń AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), po czym sprawdza pod żądania przychodzącego na bilet uwierzytelnienia. Jeśli bilet uwierzytelnienia zostanie znaleziony, obiektu GenericPrincipal jest tworzone i przypisywane do właściwości HttpContext.User.

Po zdarzeniu AuthenticateRequest potoku ASP.NET zgłasza [zdarzeń PostAuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), która jest, gdzie można zastąpić obiektu GenericPrincipal utworzonego przez FormsAuthenticationModule z wystąpieniem klasy Nasze Obiekcie CustomPrincipal. Na rysunku nr 7 przedstawia ten przepływ pracy.


[![GenericPrincipal zastępuje CustomPrincipal w zdarzeniu PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Rysunek 07**: GenericPrincipal zastępuje CustomPrincipal w zdarzeniu PostAuthenticationRequest ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))


Aby można było wykonać kod w odpowiedzi na zdarzenie potoku platformy ASP.NET, możemy utworzyć programu obsługi zdarzeń odpowiednie w pliku Global.asax lub Utwórz swój własny moduł HTTP. W tym samouczku Utwórzmy programu obsługi zdarzeń w pliku Global.asax. Rozpocznij od dodania pliku Global.asax do witryny sieci Web. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i Dodaj element typu globalnej klasy aplikacji o nazwie pliku Global.asax.


[![Dodaj plik Global.asax do witryny sieci Web](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Rysunek 08**: Dodawanie pliku Global.asax do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))


Domyślny szablon pliku Global.asax zawiera obsługi zdarzeń dla liczby zdarzeniami potoku platformy ASP.NET, w tym początek zakończenia i [zdarzenie błędu](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), między innymi. Możesz usunąć te programy obsługi zdarzeń, jak firma Microsoft nie potrzebne dla tej aplikacji. Zdarzenie, które Dbamy o jest PostAuthenticateRequest. Zaktualizuj plik Global.asax, więc jego znaczników podobny do następującego:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

Aplikacja\_OnPostAuthenticateRequest metoda wykonuje zawsze środowiska uruchomieniowego ASP.NET zgłasza zdarzenie PostAuthenticateRequest, które odbywa się raz dla każdego przychodzącego żądania strony. Uruchamia program obsługi zdarzeń przez sprawdzenie, czy użytkownik jest uwierzytelniony i został uwierzytelniony przy użyciu uwierzytelniania formularzy. Jeśli tak, nowy obiekt CustomIdentity jest utworzony i przekazywane bieżącego żądania biletu uwierzytelniania w jego konstruktora. Następujące, obiekcie CustomPrincipal jest utworzony i przekazano obiekt CustomIdentity po prostu utworzyć jego konstruktora. Na koniec bieżącego żądania kontekst zabezpieczeń jest przypisany do nowo utworzony obiekt CustomPrincipal.

Należy pamiętać, że ostatni krok - kojarzenie obiekcie CustomPrincipal kontekst zabezpieczeń żądania - przypisuje podmiot zabezpieczeń dwie właściwości: HttpContext.User i Thread.CurrentPrincipal. Te dwie przypisania są niezbędne ze względu na sposób kontekstów zabezpieczeń są obsługiwane w programie ASP.NET. .NET Framework kojarzy kontekstu zabezpieczeń z każdym uruchomiony wątek; Ta informacja jest dostępna jako obiekt IPrincipal za pośrednictwem [obiektu wątku](https://msdn.microsoft.com/library/system.threading.thread.aspx)w [CurrentPrincipal właściwości](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). Co to jest skomplikowana się, że program ASP.NET ma własną informacje kontekstu zabezpieczeń (HttpContext.User).

W niektórych scenariuszach właściwość Thread.CurrentPrincipal jest zbadane, określając kontekstu zabezpieczeń; w innych scenariuszach HttpContext.User jest używany. Na przykład istnieją funkcje zabezpieczeń w środowisku .NET, które umożliwiają deweloperom deklaratywnie stanu co użytkownicy lub role można utworzyć wystąpienia klasy lub wywołać określonej metody (zobacz [Dodawanie reguły autoryzacji do działalności biznesowej i warstwy danych przy użyciu PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Poniżej obejmuje te techniki deklaratywne ustalić kontekst zabezpieczeń za pomocą właściwości Thread.CurrentPrincipal.

W innych scenariuszach właściwość HttpContext.User jest używana. Na przykład w samouczku poprzedniej użyliśmy tej właściwości do wyświetlenia aktualnie zalogowanego użytkownika. Wyraźnie widać jest konieczne, że informacje kontekstu zabezpieczeń we właściwościach Thread.CurrentPrincipal i HttpContext.User dopasowanie.

Środowisko uruchomieniowe programu ASP.NET automatycznie synchronizuje wartości tych właściwości firmie Microsoft. Jednak synchronizacja występuje po zdarzeniu AuthenticateRequest, ale *przed* PostAuthenticateRequest zdarzeń. W rezultacie podczas dodawania niestandardowego podmiotu w zdarzeniu PostAuthenticateRequest musimy mieć pewność ręcznie przypisać Thread.CurrentPrincipal Thread.CurrentPrincipal albo też i HttpContext.User zostaną zsynchronizowane. Zobacz [Context.User vs. Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) bardziej szczegółowe omówienie na ten problem.

### <a name="accessing-the-companyname-and-title-properties"></a>Uzyskiwanie dostępu do właściwości Title i nazwa firmy

Zawsze, gdy żądanie dociera i jest wysyłane do aparatu ASP.NET, aplikacja\_uruchomią OnPostAuthenticateRequest obsługi zdarzeń w pliku Global.asax. Żądanie został pomyślnie uwierzytelniony przez FormsAuthenticationModule, program obsługi zdarzeń utworzy nowy obiekt CustomPrincipal z obiektem CustomIdentity oparte na biletu uwierzytelniania formularzy. Tę logikę w miejscu uzyskiwanie dostępu do informacji o nazwie firmy i tytuł aktualnie zalogowanego użytkownika jest bardzo proste.

Wróć do strony\_obciążenia programu obsługi zdarzeń w Default.aspx, gdzie w kroku 4 napisaliśmy kod, aby pobrać biletu uwierzytelniania formularzy i przeanalizować właściwości danych użytkownika, aby wyświetlić tytuł i nazwę firmy użytkownika. Z obiektami CustomPrincipal i CustomIdentity teraz używana jest niepotrzebna można przeanalizować wartości z właściwości danych użytkownika biletu. Zamiast tego po prostu pobrać odwołanie do obiektu CustomIdentity i użyj jej właściwości NazwaFirmy i tytuł:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Podsumowanie

W tym samouczku będziemy zbadać Dostosowywanie ustawień systemowych uwierzytelniania formularzy za pomocą pliku Web.config. Analizujemy obsługi wygaśnięcia biletu uwierzytelniania i jak szyfrowanie i weryfikacja zabezpieczenia są używane do ochrony biletu z inspekcji i modyfikacji. Ponadto omówiono przy użyciu właściwości danych użytkownika bilet uwierzytelniania do przechowywania dodatkowe informacje dotyczące użytkownika w bilecie sam oraz ujawnić te informacje w sposób bardziej przyjazny dla dewelopera przy użyciu niestandardowych obiektów Principal role i tożsamości.

W tym samouczku kończy się naszego badania uwierzytelniania formularzy w programie ASP.NET. Następny Samouczek uruchamia naszych przebieg w ramach członkostwa.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Pincety uwierzytelniania formularzy](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Objaśniono: Uwierzytelnianie formularzy w programie ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Porady: Ochrona uwierzytelniania formularzy w programie ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Professional ASP.NET 2.0 zabezpieczeń, członkostwo i zarządzanie rolami](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Zabezpieczanie kontrolek logowania](https://msdn.microsoft.com/library/ms178346.aspx)
- [&lt;Uwierzytelniania&gt; — Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Formularze&gt; elementu &lt;uwierzytelniania&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [&lt;MachineKey&gt; — Element](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Opis biletu uwierzytelniania formularzy i plików Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenie wideo na tematy zawarte w tym samouczku

- [Jak zmienić właściwości uwierzytelniania formularzy](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Sposób instalacji i używania uwierzytelniania plików Cookie bez w aplikacji ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Relokacja logowania formularzy stron ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Konfiguracja niestandardowa kluczy logowania formularzy](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Dodawanie niestandardowych danych do metody uwierzytelniania](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Używanie niestandardowych obiektów głównych](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Informacje o autorze

Scott Bento, Utwórz wiele książek ASP/ASP.NET i twórcę 4GuysFromRolla.com, pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki  *[Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott jest osiągalny w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blogu w [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został Alicja Maziarz. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Poprzednie](an-overview-of-forms-authentication-cs.md)
> [dalej](security-basics-and-asp-net-support-vb.md)
