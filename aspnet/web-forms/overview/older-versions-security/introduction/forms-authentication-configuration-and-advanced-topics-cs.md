---
uid: web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
title: Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku Zapoznamy zbadania różnych ustawień uwierzytelniania formularzy i dowiedzieć się, jak ich modyfikacji przez element formularzy. Wiąże się to szczegółowe...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: b9c29865-a34e-48bb-92c0-c443a72cb860
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/forms-authentication-configuration-and-advanced-topics-cs
msc.type: authoredcontent
ms.openlocfilehash: 707c47917450bad409a04b3b5b80edc560b7c756
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372777"
---
<a name="forms-authentication-configuration-and-advanced-topics-c"></a>Konfiguracja uwierzytelniania formularzy i Tematy zaawansowane (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_03_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial03_AuthAdvanced_cs.pdf)

> W tym samouczku Zapoznamy zbadania różnych ustawień uwierzytelniania formularzy i dowiedzieć się, jak ich modyfikacji przez element formularzy. Wiąże się to szczegółowe omówienie na dostosowywanie biletu uwierzytelniania formularzy, wartość limitu czasu, za pomocą strony logowania za pomocą niestandardowego adresu URL (na przykład SignIn.aspx zamiast Login.aspx) i biletów uwierzytelniania formularzy cookieless.


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczka](an-overview-of-forms-authentication-cs.md) przyjrzeliśmy się niezbędne w celu wykonania uwierzytelniania formularzy w aplikacji ASP.NET z Określanie ustawień konfiguracji w pliku Web.config, do tworzenia dziennika na stronie do wyświetlania różnych kroków zawartość dla użytkowników uwierzytelnionych i anonimowych. Pamiętaj, że skonfigurowaliśmy witryny sieci Web, aby używać uwierzytelniania formularzy, ustawiając atrybut tryb &lt;uwierzytelniania&gt; element do formularzy. &lt;Uwierzytelniania&gt; elementu może opcjonalnie obejmować &lt;formularzy&gt; elementu podrzędnego, za pomocą których można określić pewną liczbę ustawień uwierzytelniania formularzy.

W tym samouczku będziemy zbada różnych ustawień uwierzytelniania formularzy i zobacz, jak i zmodyfikuj je za pośrednictwem &lt;formularzy&gt; elementu. Wiąże się to szczegółowe omówienie na dostosowywanie biletu uwierzytelniania formularzy, wartość limitu czasu, za pomocą strony logowania za pomocą niestandardowego adresu URL (na przykład SignIn.aspx zamiast Login.aspx) i biletów uwierzytelniania formularzy cookieless. Firma Microsoft będzie również dokładniej sprawdzić korzeń biletu uwierzytelniania formularzy i zobacz środki ostrożności, potrzebnego ASP.NET, aby upewnić się, że dane biletu są chronione przed inspekcją i manipulowania. Na koniec zostanie przedstawiony sposób przechowywania danych dodatkowych użytkowników w biletu uwierzytelniania formularzy oraz modelu te dane za pomocą niestandardowego obiektu jednostki.

## <a name="step-1-examining-the-ltformsgt-configuration-settings"></a>Krok 1: Badanie &lt;formularzy&gt; ustawień konfiguracji

System uwierzytelniania formularzy w programie ASP.NET udostępnia wiele ustawień konfiguracji, które można dostosować na podstawie aplikacji w aplikacji. Obejmuje to ustawienia, takie jak: uwierzytelnianie formularzy okres istnienia biletu; jakiego rodzaju ochrony jest stosowany do biletu; w obszarze rodzaju uwierzytelniania cookieless warunki są używane bilety; Ścieżka do strony logowania; i inne informacje. Aby zmodyfikować wartości domyślne, należy dodać [ &lt;formularzy&gt; elementu](https://msdn.microsoft.com/library/1d3t3c61.aspx) jako element podrzędny elementu [ &lt;uwierzytelniania&gt; elementu](https://msdn.microsoft.com/library/532aee0e.aspx), określając tych właściwości wartości, które chcesz dostosować jako atrybuty XML w następujący sposób:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample1.xml)]

Tabela 1 zawiera podsumowanie właściwości, które można dostosowywać za pomocą &lt;formularzy&gt; elementu. Ponieważ plik Web.config jest plikiem XML, nazwy atrybutów w lewej kolumnie jest rozróżniana wielkość liter.


| <strong>Atrybut</strong> |                                                                                                                                                                                                                                     <strong>Opis</strong>                                                                                                                                                                                                                                      |
|----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|         cookieless         |                                                                                                                Ten atrybut określa, pod jakimi warunkami biletu uwierzytelniania są przechowywane w pliku cookie i są osadzone w adresie URL. Dozwolone wartości to: UseCookies; UseUri; Autowykrywanie; i UseDeviceProfile (ustawienie domyślne). Krok 2 sprawdza, czy to ustawienie, które bardziej szczegółowo.                                                                                                                |
|         defaultUrl         |                                                                                                                                                         Wskazuje adres URL, do którego użytkownicy są przekierowywani do po zalogowaniu się na stronie logowania, jeśli nie ma RedirectUrl wartości określone w zmiennej querystring. Wartość domyślna to default.aspx.                                                                                                                                                         |
|           domena           | Korzystając z biletów uwierzytelniania na podstawie plików cookie, to ustawienie określa wartość domeny pliku cookie. Wartość domyślna to ciąg pusty, co powoduje, że przeglądarka do korzystania z domeny, z którego został wydany (na przykład www.yourdomain.com). W takim przypadku plik cookie zostanie <strong>nie</strong> wysłania w przypadku wprowadzania żądań poddomen, na przykład admin.yourdomain.com. Jeśli chcesz, aby plik cookie, które mają być przekazane do wszystkimi domenami podrzędnymi, należy dostosować atrybut domain ustawieniem dla niego yourdomain.com. |
|  enableCrossAppRedirects   |                                                                                                                                                                   Wartość logiczna wskazująca, czy użytkownicy uwierzytelnieni są zapamiętywane, gdy przekierowywane do adresów URL w innych aplikacjach sieci web na tym samym serwerze. Wartością domyślną jest false.                                                                                                                                                                   |
|          loginUrl          |                                                                                                                                                                                                                      Adres URL strony logowania. Wartość domyślna to login.aspx.                                                                                                                                                                                                                      |
|            nazwa            |                                                                                                                                                                                                   Korzystając z biletów na podstawie plików cookie uwierzytelniania, nazwa pliku cookie. Wartość domyślna to. ASPXAUTH.                                                                                                                                                                                                   |
|            ścieżka            |                                                                             Korzystając z biletów uwierzytelniania na podstawie plików cookie, to ustawienie określa atrybut ścieżki pliku cookie. Atrybut path umożliwia zatem programistą, aby ograniczyć zakres pliku cookie do hierarchii określonego katalogu. Wartością domyślną jest /, która informuje przeglądarkę, aby wysłać plik cookie biletu uwierzytelniania na każde żądanie, wprowadzone do domeny.                                                                              |
|         ochrona         |                                                                                                                                            Wskazuje, jakie metody są używane do ochrony biletu uwierzytelniania formularzy. Dozwolone wartości to: wszystkie (ustawienie domyślne); Szyfrowanie; None; i sprawdzania poprawności. Te ustawienia są szczegółowo omówione w kroku 3.                                                                                                                                            |
|         wartość parametru requireSSL         |                                                                                                                                                                                Wartość logiczna wskazująca, czy połączenia SSL jest wymagany do przesyłania pliku cookie uwierzytelniania. Wartość domyślna to false.                                                                                                                                                                                |
|     slidingExpiration      |                                                                                                 Wartość logiczna wskazująca, czy limit czasu uwierzytelniania pliku cookie jest resetowany każdorazowo użytkownik odwiedza witrynę podczas jednej sesji. Wartość domyślna to true. Omówiono bardziej szczegółowo w Określanie zasad limitu czasu biletu uwierzytelniania biletu wartość limitu czasu sekcji.                                                                                                 |
|          limit czasu           |                                                                                                                               Określa czas w minutach, po którym wygasa plik cookie biletu uwierzytelniania. Wartość domyślna to 30. Omówiono bardziej szczegółowo w Określanie zasad limitu czasu biletu uwierzytelniania biletu wartość limitu czasu sekcji.                                                                                                                               |

**Tabela 1**: Podsumowanie &lt;formularzy&gt; atrybuty elementu

W programie ASP.NET 2.0 i ponad wartość domyślną wartości uwierzytelniania formularzy są zakodowane w klasie FormsAuthenticationConfiguration w programie .NET Framework. Wszelkie modyfikacje musi zostać zastosowana na podstawie aplikacji w aplikacji w pliku Web.config. To różni się od ASP.NET 1.x, gdzie wartości domyślne uwierzytelniania formularzy były przechowywane w pliku machine.config (i w związku z tym może zostać zmodyfikowany za pomocą edycji pliku machine.config). Na temat platformy ASP.NET 1.x, warto wspomnieć, szereg ustawień systemu uwierzytelniania formularzy mieć różne domyślne wartości w programie ASP.NET 2.0 i poza nim niż w programie ASP.NET: 1.x. Jeśli migrujesz aplikacji ze środowiska ASP.NET 1.x, ważne jest pod uwagę te różnice. Zapoznaj się z [ &lt;formularzy&gt; dokumentacji technicznej element](https://msdn.microsoft.com/library/1d3t3c61.aspx) listę różnic.

> [!NOTE]
> Kilka ustawień uwierzytelniania formularzy, takie jak limit czasu, domeny i ścieżki, określ szczegóły wynikowego pliku cookie biletu uwierzytelniania formularzy. Aby uzyskać więcej informacji na temat plików cookie, jak działają i ich właściwości, przeczytaj [w tym samouczku pliki cookie](http://www.quirksmode.org/js/cookies.html).


### <a name="specifying-the-tickets-timeout-value"></a>Określając wartość limitu czasu biletu

Bilet uwierzytelniania formularzy jest token reprezentujący tożsamości. Biletów uwierzytelniania na podstawie plików cookie token ten jest przechowywany w postaci pliku cookie i wysyłane do serwera sieci web na każde żądanie. Posiadanie tokenu, w zasadzie deklaruje, jestem *username*, już logowali się i jest używany, aby tożsamości użytkowników należy pamiętać, między odwiedzin strony.

Bilet uwierzytelniania formularzy nie tylko obejmuje tożsamości użytkownika, ale zawiera również informacje, aby zapewnić integralność i bezpieczeństwo tokenu. Nie chcemy przecież szkodliwa użytkownikowi można utworzyć token fałszywe lub zmodyfikować legit token w jakiś sposób underhanded.

Jest jeden bit takich informacji znajdujących się w bilecie *wygaśnięcia*, czyli Data i godzina--ticket nie jest już prawidłowy. Każdorazowo FormsAuthenticationModule sprawdza bilet uwierzytelnienia gwarantuje, że wygaśnięcia biletu nie został jeszcze przekazany. Jeśli tak, pomija--ticket i identyfikuje użytkownika jako anonimowy. To zabezpieczenie pomaga w ochronie przed atakami powtarzania. Bez wygaśnięcia, jeśli haker był w stanie wyświetlić swoje sesje hands on użytkownika prawidłowego biletu — prawdopodobnie fizyczny dostęp do komputera i zakorzenienia za pośrednictwem ich pliki cookie — można wysyłają żądania do serwera z tego biletu uwierzytelniania kradzieży i Uzyskaj wpis. Natomiast po upływie nie zapobiec temu scenariuszowi, ale ogranicza okna, w którym będzie możliwe takiego ataku.

> [!NOTE]
> Krok 3 szczegółów innych technik używane przez system uwierzytelniania formularzy do ochrony biletu uwierzytelniania.


Podczas tworzenia biletu uwierzytelniania, systemem uwierzytelniania formularzy określa jego wygaśnięcia, sprawdzając ustawienia limitu czasu. Jak wspomniano w tabeli 1, limit czasu ustawień domyślnych do 30 minut, co oznacza, że po utworzeniu biletu uwierzytelniania formularzy jej wygaśnięcia jest ustawiona na datę i godzinę w przyszłości 30 minut.

Po upływie definiuje bezwzględny czas w przyszłości wygaśnięcia biletu uwierzytelniania formularzy. Jednak zazwyczaj deweloperów do zaimplementowania wygaśnięcia przewijania, taki, który jest resetowany za każdym razem, gdy użytkownik powraca do lokacji. To zachowanie zależy od ustawień slidingExpiration. Jeśli ma wartość true (domyślnie), każdorazowo FormsAuthenticationModule użytkownik zostanie uwierzytelniony, aktualizuje wygaśnięcia biletu. Jeśli ma wartość false, po upływie nie zostaną zaktualizowane na każde żądanie, co powoduje--ticket wygaśnie dokładnie limitu czasu liczba minut po pełnej po raz pierwszy--ticket utworzony.

> [!NOTE]
> Po upływie przechowywane w biletu uwierzytelniania jest bezwzględna wartość daty i godziny, takich jak 2 sierpień 2008 11:34:00. Ponadto daty i godziny są względem czasu lokalnego serwera sieci web. Ta decyzja projektowa może mieć kilka interesujących efekty uboczne wokół czasu letniego (DST), czyli gdy zegary w Stanach Zjednoczonych zostaną przeniesione wyprzedzeniem godzinę (przy założeniu, że serwer sieci web jest hostowana w ustawieniach regionalnych, gdy zostanie wykryty czas letni). Należy wziąć pod uwagę, co się stanie, witryny sieci Web programu ASP.NET o wygaśnięciu 30-minutowy w zbliżonym czasie, który rozpoczyna czasu letniego (czyli o 2:00). Załóżmy, że użytkownik loguje się do witryny 11 marca 2008 o 1:55. Spowoduje to wygenerowanie biletu uwierzytelniania formularzy, który upływa 11 marca 2008 o 2:25 (30 minut w przyszłości). Jednak po 2:00 AM toczy się wokół, zegar przechodzi do 3:00 z powodu czasu letniego. Po użytkownik załadowaniu nowej strony sześciu minut po zalogowaniu się (na 3:01:00), FormsAuthenticationModule zauważa, że bilet wygasł i przekierowuje użytkownika do strony logowania. Bardziej szczegółowe omówienie dotyczące tego i innych oddities limitu czasu biletu uwierzytelniania, jak również rozwiązania problemu, wybierz kopię Stefan Schackow *Professional programu ASP.NET 2.0 zabezpieczeń, członkostwo i zarządzanie rolami* (ISBN: 978-0-7645-9698-8).


Rysunek 1 przedstawia przepływ pracy, gdy slidingExpiration będzie miał ustawioną na wartość false, a limit czasu jest ustawiona na 30. Należy pamiętać, że bilet uwierzytelniania, wygenerowany przy logowaniu zawiera datę wygaśnięcia, a ta wartość nie jest aktualizowana podczas kolejnych żądań. Jeśli FormsAuthenticationModule wykryje, że bilet wygasł, odrzuci ją i traktuje żądania jako użytkownik anonimowy.


[![Graficzna reprezentacja biletu uwierzytelniania formularzy, po upływie slidingExpiration ma wartość false](forms-authentication-configuration-and-advanced-topics-cs/_static/image2.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image1.png)

**Rysunek 01**: graficzną reprezentację biletu uwierzytelniania formularzy, po upływie slidingExpiration ma wartość false ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image3.png))


Na rysunku 2 przedstawiono przepływ pracy, gdy slidingExpiration będzie miał ustawioną wartość PRAWDA, a limit czasu jest ustawiona na 30. Po odebraniu uwierzytelnionego żądania (z biletem wygasła) jej wygaśnięcia jest aktualizowany do limitu liczby minut w przyszłości.


[![Graficzna reprezentacja biletu uwierzytelniania formularzy po slidingExpiration ma wartość true](forms-authentication-configuration-and-advanced-topics-cs/_static/image5.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image4.png)

**Rysunek 02**: graficzną reprezentację biletu uwierzytelniania formularzy po slidingExpiration ma wartość true ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image6.png))


Korzystając z biletów na podstawie plików cookie uwierzytelniania (ustawienie domyślne), staje się tej dyskusji z nieco bardziej skomplikowane, ponieważ pliki cookie może też mieć własne expiries określony. Plik cookie wygaśnięcia (lub ich brak) powoduje, że przeglądarka podczas należy zniszczyć plik cookie. Jeśli plik cookie nie ma wygaśnięcia, jest niszczony, podczas zamykania przeglądarki. Jeśli występuje wygaśnięcia jednak plik cookie pozostają zapisane na komputerze użytkownika aż do daty i czas określony w wygaśnięcia został przekazany. Jeśli plik cookie zostanie zniszczony przez przeglądarkę, już nie są wysyłane do serwera sieci web. W związku ze zniszczeniem plik cookie jest analogiczne do użytkownika wylogować się w witrynie.

> [!NOTE]
> Oczywiście użytkownik aktywnie usunąć pliki cookie przechowywane na swoim komputerze. W programie Internet Explorer 7 będzie przejdź do lokalizacji narzędzia, opcje i kliknij przycisk Usuń w sekcji Historia przeglądania. W tym miejscu przycisk usuwania plików cookie.


System uwierzytelniania formularzy tworzy oparte na sesji lub ważności na podstawie plików cookie w zależności od wartości przekazanej w celu *persistCookie* parametru. Odwołania, która metod GetAuthCookie, SetAuthCookie i RedirectFromLoginPage klasy uwierzytelniania formularzy w dwóch parametrów wejściowych: *username* i *persistCookie*. Na stronie logowania, utworzonego w poprzednim samouczku uwzględnione Pamiętaj mnie pole wyboru, które ustalić, czy trwały plik cookie został utworzony. Trwałe pliki cookie są oparte na wygaśnięcia; trwałe pliki cookie są oparte na sesji.

Limit czasu i slidingExpiration omówione już takie same mają zastosowanie do obu plików cookie opartego na sesji i wygaśnięcia. Istnieje tylko jedna różnica drobne podczas wykonywania: korzystając z slidingTimeout o wartości true ważności na podstawie plików cookie, plik cookie wygaśnięcia są aktualizowane tylko po ponad połowy określonego czasu upłynął.

Zaktualizujmy zasad limitu czasu biletu uwierzytelniania naszej witryny sieci Web, więc biletów, limit czasu po godzinie (60 minut), za pomocą wygaśniecie. Wpływ tej zmiany, zaktualizuj plik Web.config, dodając &lt;formularzy&gt; elementu &lt;uwierzytelniania&gt; element następującym kodem:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample2.xml)]

### <a name="using-an-login-page-url-other-than-loginaspx"></a>Przy użyciu adresu URL strony logowania inne niż Login.aspx

Ponieważ FormsAuthenticationModule automatycznie przekierowuje nieautoryzowanych użytkowników do strony logowania, trzeba znać adres URL strony logowania. Ten adres URL jest określony przez atrybut loginUrl w &lt;formularzy&gt; elementu i wartość domyślna to login.aspx. Jeśli są przenoszenie się za pośrednictwem istniejącej witryny sieci Web, może już istnieć strony logowania przy użyciu innego adresu URL, taki, który został już zakładek i indeksowany przez wyszukiwarki. Zamiast zmienianie nazw istniejących strona logowania do strony login.aspx i istotne łącza i zakładki użytkowników, zamiast tego można zmodyfikować atrybutu loginUrl, aby wskazywały do strony logowania.

Na przykład, jeśli Twoja strona logowania nosiła nazwę SignIn.aspx i znajdują się w katalogu użytkowników, możesz punkt ustawienia konfiguracji loginUrl ~/Users/SignIn.aspx w następujący sposób:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample3.xml)]

Ponieważ już naszej bieżącej aplikacji o nazwie Login.aspx strony logowania, nie ma potrzeby określić niestandardową wartość w &lt;formularzy&gt; elementu.

## <a name="step-2-using-cookieless-forms-authentication-tickets"></a>Krok 2: Za pomocą biletów uwierzytelniania formularzy Cookieless

Domyślnie system uwierzytelniania formularzy Określa, czy do przechowywania jego biletów uwierzytelniania w kolekcji plików cookie lub osadzeniu w adresie URL, w oparciu o agenta użytkownika w witrynie. Wszystkie mainstream przeglądarek komputerowych, takich jak program Internet Explorer, Firefox, Opera i Safari, obsługa plików cookie, ale nie wszystkie urządzenia przenośne nie.

Zasady plików cookie, używane przez system uwierzytelniania formularzy zależy od ustawienia cookieless w &lt;formularzy&gt; element, który można przypisać jedną z czterech wartości:

- UseCookies - Określa, że zawsze będzie służyć biletów uwierzytelniania na podstawie plików cookie.
- UseUri — wskazuje, że biletów na podstawie plików cookie uwierzytelniania nigdy nie będą używane.
- Autowykrywanie — Jeśli profil urządzenia obsługuje pliki cookie, biletów uwierzytelniania na podstawie plików cookie nie są używane; Jeśli profil urządzenia obsługuje pliki cookie, mechanizm sondowania jest używany do określenia, czy pliki cookie są włączone.
- UseDeviceProfile — domyślnie; używa biletów uwierzytelniania na podstawie plików cookie tylko wtedy, gdy jest to obsługiwane przez profil urządzenia. Jest używany żaden mechanizm sondowania.

Zależą od ustawień Autowykrywanie i UseDeviceProfile *profilu urządzenia* w upewnieniu się, czy ma być używane uwierzytelnianie na podstawie plików cookie lub cookieless biletów. Program ASP.NET obsługuje bazę danych z różnych urządzeń i ich możliwości, takie jak czy obsługują one plików cookie, jakie wersje języka JavaScript obsługują i tak dalej. Każdorazowo urządzenia żądania strony sieci web z serwera sieci web wysyła wzdłuż *agenta użytkownika* nagłówka HTTP, który identyfikuje typ urządzenia. ASP.NET automatycznie dopasowuje ciąg agenta użytkownika podane odpowiedni profil określony w swojej bazie danych.

> [!NOTE]
> Tej bazy danych z możliwości urządzenia są przechowywane w wielu plikach XML stosować się do [schematu plik definicji przeglądarki](https://msdn.microsoft.com/library/ms228122.aspx). Domyślne pliki profilu urządzenia znajdują się w lokalizacji % WINDIR%\Microsoft.Net\Framework\v2.0.50727\CONFIG\Browsers. Można również dodać niestandardowe pliki do aplikacji App\_folderu przeglądarki. Aby uzyskać więcej informacji, zobacz [How to: wykrywanie typy przeglądarek w składniku ASP.NET Web Pages](https://msdn.microsoft.com/library/3yekbd5b.aspx).


Ustawieniem domyślnym jest UseDeviceProfile, biletów uwierzytelniania formularzy cookieless będzie używany, gdy witryna jest kontrolowane przez urządzenie, którego profil zgłasza, że nie obsługuje pliki cookie.

### <a name="encoding-the-authentication-ticket-in-the-url"></a>Kodowanie biletu uwierzytelniania w adresie URL

Pliki cookie to naturalne średni, w tym informacji z przeglądarki do wszystkich żądań do określonej witryny internetowej, dlatego domyślne ustawienia uwierzytelniania formularzy używanie plików cookie, jeśli urządzenie zaproszonych obsługuje je. Jeśli pliki cookie nie są obsługiwane, należy zastosować alternatywny sposób przekazywania bilet uwierzytelnienia od klienta do serwera. Typowym obejściem używanymi w środowiskach cookieless polega na zakodowaniu dane pliku cookie w adresie URL.

Najlepszym sposobem, aby zobaczyć, jak takie informacje mogą być osadzone w adresie URL jest, aby wymusić używanie biletów uwierzytelniania cookieless lokację. Można to zrobić, ustawiając UseUri ustawienie konfiguracji cookieless:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample4.xml)]

Po wprowadzeniu tej zmiany, odwiedź witrynę za pośrednictwem przeglądarki. Gdy użytkownik odwiedzi jako użytkownik anonimowy, adresy URL będzie wyglądać dokładnie tak, jak przed. Na przykład podczas odwiedzania strony Default.aspx paska adresu w przeglądarce pokazuje następującego adresu URL:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/default.aspx`

Jednak po zalogowaniu biletu uwierzytelniania formularzy jest osadzony w adresie URL. Na przykład po przechodząc do strony logowania i logując się jako Sam, czy jestem powrót do strony Default.aspx, ale adres URL jest w tej chwili:

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/default.aspx`

Bilet uwierzytelniania formularzy ma zostały osadzone w adresie URL. Ciąg (F (jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv rfezq0gKadKX0YPZCkA2) reprezentuje dane biletu uwierzytelniania zakodowany w formacie szesnastkowy, a to te same dane, które są zazwyczaj przechowywane w pliku cookie.

Aby biletów uwierzytelniania cookieless do pracy system zakodować wszystkie adresy URL na stronie obejmujący dane biletu uwierzytelniania, w przeciwnym razie biletu uwierzytelniania zostaną utracone po kliknięciu łącza przez użytkownika. Szczęście tę logikę osadzania odbywa się automatycznie. Aby zademonstrować tę funkcję, otwórz strony Default.aspx, a następnie dodaj kontrolkę Hiperlinku ustawienie jego właściwości tekstu i NavigateUrl Link przetestuj i SomePage.aspx, odpowiednio. Nie ma znaczenia, czy tak naprawdę nie ma strony w projekcie o nazwie SomePage.aspx.

Zapisać zmiany na Default.aspx i można go znaleźć za pośrednictwem przeglądarki. Zaloguj się do witryny, aby biletu uwierzytelniania formularzy jest osadzony w adresie URL. Następnie na Default.aspx, kliknij łącze Testuj łącze. Co się stało? Jeśli istnieje żadnej strony o nazwie SomePage.aspx, następnie wystąpił błąd 404, ale to nie jest ważne, w tym miejscu. Zamiast tego należy skoncentrować się na pasku adresu w przeglądarce. Należy pamiętać, że zawiera on biletu uwierzytelniania formularzy w adresie URL!

`http://localhost:2448/ASPNET\_Security\_Tutorial\_03\_CS/(F(jaIOIDTJxIr12xYS-VVgkqKCVAuIoW30Bu0diWi6flQC-FyMaLXJfow\_Vd9GZkB2Cv-rfezq0gKadKX0YPZCkA2))/SomePage.aspx`

SomePage.aspx adres URL linku został automatycznie przekonwertowany do adresu URL, który się biletu uwierzytelniania — my nie mamy do zapisania lizawki kodu! Bilet uwierzytelniania formularzy automatycznie zostanie osadzony w adresie URL hiperłącza, które nie rozpoczynają się od `http://` lub `/`. Nie ma znaczenia, jeśli hiperlink, pojawi się w wywołaniu Response.Redirect, kontroli hiperłącze lub element kotwicy HTML (czyli `<a href="...">...</a>`). Tak długo, jak adres URL jest podobny do `http://www.someserver.com/SomePage.aspx` lub `/SomePage.aspx`, biletu uwierzytelniania formularzy, zostanie osadzony w firmie Microsoft.

> [!NOTE]
> Biletów uwierzytelniania formularzy cookieless stosować się do tych samych zasad limitu czasu jako biletów uwierzytelniania na podstawie plików cookie. Jednak biletów uwierzytelniania cookieless są bardziej podatne na ataki, ponieważ biletu uwierzytelniania jest osadzony bezpośrednio w adresie URL. Wyobraź sobie użytkownik odwiedza witrynę sieci Web, loguje się i następnie wklejenie adresu URL w wiadomości e-mail do współpracownika. Jeśli współpracownik kliknie ten link przed wygaśnięciem, ich będą rejestrowane w jako użytkownik, który wysłano wiadomość e-mail!


## <a name="step-3-securing-the-authentication-ticket"></a>Krok 3: Zabezpieczanie biletu uwierzytelniania

Bilet uwierzytelniania formularzy jest przesyłane przez sieć w pliku cookie lub osadzonego bezpośrednio w adresie URL. Oprócz informacji o tożsamości biletu uwierzytelniania mogą również obejmować dane użytkownika (jak zobaczymy w kroku 4). W związku z tym, ważne jest, że biletu dane są szyfrowane z wścibskimi Ty masz dostęp oraz (nawet co ważniejsze) czy systemem uwierzytelniania formularzy, co umożliwiłoby zagwarantowanie--ticket nie został zmodyfikowany z.

Aby zapewnić prywatność danych biletu, systemem uwierzytelniania formularzy można zaszyfrować dane biletu. Nie można zaszyfrować dane biletu wysyła potencjalnie wrażliwe informacje przez sieć w postaci zwykłego tekstu.

Zagwarantowanie autentyczności biletu, muszą się systemem uwierzytelniania formularzy *zweryfikować* --ticket. Sprawdzanie poprawności jest zagwarantowanie, że do określonego elementu danych nie został zmodyfikowany i odbywa się za pośrednictwem  *[komunikat o kod uwierzytelniania (MAC)](http://en.wikipedia.org/wiki/Message_authentication_code)*. Mówiąc komputerów MAC jest mała ilość informacji, która identyfikuje dane, które musi zostać zweryfikowany (w tym przypadku--ticket). W przypadku modyfikowania danych reprezentowanego przez MAC następnie komputerów MAC i dane będą niezgodny. Ponadto jest praktyce twardych przez hakera modyfikować dane i wygenerować własną MAC z zmodyfikowanych danych.

Podczas tworzenia (lub modyfikowania) bilet, a systemem uwierzytelniania formularzy tworzy komputera MAC i dołącza je do danych biletu. Po odebraniu kolejne żądanie systemem uwierzytelniania formularzy porównuje dane systemów MAC i biletu umożliwia sprawdzenie oryginalności dane biletu. Rysunek 3 ilustruje ten przepływ pracy w formie graficznej.


[![Autentyczności biletu jest zapewniony przez komputer MAC](forms-authentication-configuration-and-advanced-topics-cs/_static/image8.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image7.png)

**Rysunek 03**: zapewniony autentyczności biletu do komputera MAC ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image9.png))


Jakie środki zabezpieczeń są stosowane do biletu uwierzytelniania zależy od ustawienia ochrony w &lt;formularzy&gt; elementu. Ustawienie ochrony może być przypisana do jednego z trzech następujących wartości:

- All - biletu jest zaszyfrowany i podpisany cyfrowo (ustawienie domyślne).
- Szyfrowanie — tylko szyfrowanie jest stosowane — MAC nie jest generowany.
- Brak — biletu nie jest szyfrowany ani podpisany cyfrowo.
- Sprawdzanie poprawności — MAC jest generowany, ale dane biletu jest przesyłane przez sieć w postaci zwykłego tekstu.

Firma Microsoft zaleca użycie wszystkie ustawienia.

### <a name="setting-the-validation-and-decryption-keys"></a>Ustawienia weryfikacji i kluczy Odszyfrowujących

Szyfrowanie i używane przez system uwierzytelniania formularzy, szyfrowanie i sprawdzanie poprawności biletu uwierzytelniania algorytmy wyznaczania wartości skrótu są dostosowywane za pośrednictwem [ &lt;machineKey&gt; elementu](https://msdn.microsoft.com/library/w8h3skw9.aspx) w pliku Web.config. Tabela 2 przedstawiono &lt;machineKey&gt; atrybuty elementu i jego możliwych wartości.

| **Atrybut** | **Opis** |
| --- | --- |
| odszyfrowywanie | Określa algorytm używany do szyfrowania. Ten atrybut może mieć jedną z następujących czterech wartości: - Auto - default; Określa algorytm, w zależności od długości atrybutu decryptionKey. -Używa AES - [Advanced Encryption Standard (AES)](http://en.wikipedia.org/wiki/Advanced_Encryption_Standard) algorytmu. -Używa DES - [Data Encryption Standard (DES)](http://en.wikipedia.org/wiki/Data_Encryption_Standard) ten algorytm jest uznawany za słabe praktyce i nie powinna być używana. Używa - 3DES - [Triple DES](http://en.wikipedia.org/wiki/Triple_DES) algorytmu, który polega na stosowaniu algorytm DES trzy razy. |
| decryptionKey | Używane przez algorytm szyfrowania klucza tajnego. Ta wartość musi być ciąg szesnastkowy odpowiednią długość (na podstawie wartości odszyfrowywanie), automatyczne generowanie lub jedną z tych wartości, dołączony, IsolateApps. Dodawanie IsolateApps powoduje, że program ASP.NET do użycia unikatową wartość dla każdej aplikacji. Wartość domyślna to automatyczne generowanie, IsolateApps. |
| walidacja | Określa algorytm używany do sprawdzania poprawności. Ten atrybut może mieć jedną z następujących czterech wartości: - AES - używa algorytmu Advanced Encryption Standard (AES). -Używa MD5 - [Message Digest 5 (MD5)](http://en.wikipedia.org/wiki/MD5) algorytmu. -Używa SHA1 - [SHA1](http://en.wikipedia.org/wiki/Sha1) algorytm (ustawienie domyślne). -3DES - używa algorytmu Triple DES. |
| validationKey | Klucz tajny, używane przez algorytm sprawdzania poprawności. Ta wartość musi być ciąg szesnastkowy odpowiednią długość (na podstawie wartości podczas sprawdzania poprawności), automatyczne generowanie lub jedną z tych wartości, dołączony, IsolateApps. Dodawanie IsolateApps powoduje, że program ASP.NET do użycia unikatową wartość dla każdej aplikacji. Wartość domyślna to automatyczne generowanie, IsolateApps. |

**Tabela 2**: &lt;machineKey&gt; atrybutów elementów

Dogłębną dyskusję te opcje szyfrowanie i sprawdzanie poprawności i profesjonalistów zalet i wad różnych algorytmów wykracza poza zakres tego samouczka. Dla szczegółowe Przyjrzyj się tych problemów oraz wskazówki dotyczące jakie algorytmy, szyfrowanie i sprawdzanie poprawności, aby użyć, jakie długości kluczy do użycia i jest to najlepszy sposób wygenerować klucze, zapoznaj się *Professional programu ASP.NET 2.0 zabezpieczeń, członkostwo i zarządzanie rolami* .

Domyślnie klucze używane do szyfrowania i odszyfrowywania są generowane automatycznie dla każdej aplikacji, a te klucze są przechowywane w urzędu zabezpieczeń lokalnych (LSA). Krótko mówiąc domyślne ustawienia gwarantuje unikatowe klucze w serwera sieci web serwera sieci web i aplikacji w aplikacji. W związku z tym to domyślne zachowanie nie będzie działać w dwóch następujących scenariuszach:

- **Kolektywów serwerów sieci Web** — w [kolektywu serwerów sieci web](http://en.wikipedia.org/wiki/Web_farm) scenariusza i aplikacji sieci web jednym znajduje się na wielu serwerach sieci web na potrzeby skalowalności i nadmiarowości. Każdego żądania przychodzącego jest wysyłane do serwera w farmie, co oznacza, że przez cały okres istnienia sesji użytkownika, różne serwery może służyć do obsługi jego różnych żądań. W związku z tym każdy serwer musi używać tych samych kluczy szyfrowania i odszyfrowywania, tak, aby utworzyć bilet uwierzytelniania formularzy, zaszyfrowane i zweryfikowane na jednym serwerze może być odszyfrowane i zweryfikowane na innym serwerze w farmie.
- **Krzyżowe udostępnianie biletu aplikacji** — jednym serwerze sieci web mogą hostować wiele aplikacji programu ASP.NET. Jeśli potrzebujesz tych różnych aplikacji do udostępniania biletu uwierzytelniania formularzy pojedynczego, konieczne jest aby dopasować swoje klucze szyfrowania i odszyfrowywania.

Podczas pracy w ramach farmy sieci web, ustawienie lub udostępnianie biletów uwierzytelniania w aplikacjach na tym samym serwerze, należy skonfigurować &lt;machineKey&gt; elementu w aplikacjach, których dotyczy problem, aby ich decryptionKey i wartości validationKey zgodny.

Gdy żaden z powyższych scenariuszy dotyczy naszą przykładową aplikacją, firma Microsoft nadal określić jawnego decryptionKey i wartości validationKey i zdefiniuj algorytmy, które ma być używany. Dodaj &lt;machineKey&gt; ustawienie do pliku Web.config:

[!code-xml[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample5.xml)]

Aby uzyskać więcej informacji, zapoznaj się z [How to: Skonfiguruj MachineKey w programie ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx).

> [!NOTE]
> Wartości decryptionKey i validationKey zostały pobrane z [Steve Gibson](http://www.grc.com/stevegibson.htm)firmy [strony sieci web doskonałe haseł](https://www.grc.com/passwords.htm), generująca 64 losowo wybranych znaków szesnastkowych na każdej wizyty strony. Aby zmniejszyć prawdopodobieństwo te klucze, dzięki czemu swoją drogę do aplikacji produkcyjnych, zachęcamy do Zastąp powyższe klucze losowo generowany z nich ze strony doskonałe hasła.


## <a name="step-4-storing-additional-user-data-in-the-ticket"></a>Krok 4: Przechowywanie dodatkowe dane użytkownika w bilecie

Wiele aplikacji sieci web wyświetlić informacje o lub utworzyć wyświetlania strony na aktualnie zalogowanego użytkownika. Na przykład strony sieci web może wyświetlać nazwę użytkownika i daty, których ona ostatniego zalogowania w prawym górnym rogu każdej strony. Bilet uwierzytelniania formularzy przechowuje nazwy użytkownika aktualnie zalogowanego użytkownika, ale w razie wszelkie inne informacje strony musi przejść w magazynie użytkownika — zwykle bazy danych — do wyszukiwania informacji nie są przechowywane w biletu uwierzytelniania.

Dodatkowe informacje dotyczące użytkownika z niewielkim fragmentem kodu można są przechowywane w biletu uwierzytelniania formularzy. Takie dane mogą być wyrażone za pomocą [klasy FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx)firmy [właściwość UserData](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.userdata.aspx). To jest użytecznym miejscem do umieszczania małe ilości informacji o użytkowniku, które zazwyczaj są potrzebne. Wartość określona w UserData właściwość wchodzi w skład bilecie pliku cookie uwierzytelniania i podobnie jak inne pola biletu, zostaje zaszyfrowany i zweryfikowane na podstawie konfiguracji systemu uwierzytelniania formularzy. Domyślnie UserData jest ciągiem pustym.

W celu przechowywania danych użytkownika w biletu uwierzytelniania, należy napisać fragmentem kodu na stronie logowania, który bierze informacje specyficzne dla użytkownika i zapisuje go w bilecie. Ponieważ UserData właściwość typu String, dane znajdujące się w niej muszą być prawidłowo serializowane jako ciąg. Na przykład Wyobraź sobie, że nasz magazyn użytkownika zawarta każdy użytkownik daty urodzenia i nazwę pracodawcy, a Chcieliśmy, aby przechowywać wartości tych dwóch właściwości w biletu uwierzytelniania. Firma Microsoft można serializować te wartości na ciąg przez złączenie użytkownika daty urodzenia na ciąg przy użyciu potoku (|), a po niej nazwę pracodawcy. Dla wybranego użytkownika urodzonych 15 sierpnia 1974 r. działa dla Northwind Traders, firma Microsoft przypisywanej właściwość UserData ciągu: 1974 r.-08-15 | Northwind Traders.

Zawsze, gdy będziemy musieli uzyskiwać dostęp do danych przechowywanych w--ticket, możemy to zrobić przez Przechwytywanie FormsAuthenticationTicket bieżącego żądania i deserializacja właściwości danych użytkownika. W przypadku daty urodzenia i pracodawca przykładowe nazwy firma Microsoft będzie podzielić ciąg UserData dwóch podciągów w oparciu o ogranicznika (|).


[![Dodatkowe informacje dotyczące użytkownika mogą być przechowywane w biletu uwierzytelniania](forms-authentication-configuration-and-advanced-topics-cs/_static/image11.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image10.png)

**Rysunek 04**: dodatkowe użytkownika informacje mogą być przechowywane w biletu uwierzytelniania ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image12.png))


### <a name="writing-information-to-userdata"></a>Wpisywania informacji do danych użytkownika

Niestety Dodawanie informacji o użytkowniku do biletu uwierzytelniania formularzy nie jest tak proste, jak można oczekiwać, że. Właściwość UserData klasy FormsAuthenticationTicket jest tylko do odczytu i może zostać określony tylko za pośrednictwem FormsAuthenticationTicket konstruktora klasy. Podczas określania właściwości UserData w konstruktorze, należy również podać--ticket przez inne wartości: nazwa, Data wystawienia, wygaśnięcia i tak dalej. Wtedy stworzyliśmy strony logowania w poprzednim samouczku to wszystkie obsłużono nam klasa uwierzytelniania formularzy. Podczas dodawania danych użytkownika do FormsAuthenticationTicket, konieczne będzie napisać kod, aby replikować większość funkcjonalności już dostarczony przez klasę uwierzytelniania formularzy.

Przyjrzyjmy się niezbędny kod do pracy z danych użytkownika, aktualizując strony Login.aspx, aby rejestrować dodatkowe informacje na temat użytkownikowi biletu uwierzytelniania. Poudawać się, że nasz magazyn użytkownika zawiera informacje dotyczące firmy użytkownika działa w przypadku i stanowiska i chcemy przechwytywać te informacje w biletu uwierzytelniania. Program obsługi zdarzeń kliknięcie LoginButton strony Login.aspx należy zaktualizować tak, aby kod wyglądał następująco:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample6.cs)]

Rozważmy kroki tego jednego wiersza kodu naraz. Metoda rozpoczyna się przez definiowanie czterech tablic ciągów: Użytkownicy "," hasła "," NazwaFirmy "i" titleAtCompany. Te macierze przechowywania nazwy użytkownika, hasła, nazwy firmy i tytuły dla kont użytkowników w systemie, dla których dostępne są trzy: Scott, Jisun i Sam. W rzeczywistej aplikacji te wartości będą badane w magazynie użytkownika, a nie zakodowane w kodzie źródłowym strony.

W poprzednim samouczku Jeśli podane poświadczenia są prawidłowe po prostu dzwoniliśmy FormsAuthentication.RedirectFromLoginPage (UserName.Text, RememberMe.Checked), które wykonano następujące procedury:

1. Utworzony bilet uwierzytelniania formularzy
2. Zapisano--ticket do odpowiedniego magazynu. W przypadku biletów uwierzytelniania na podstawie plików cookie kolekcji plików cookie w przeglądarce jest używany; bilety uwierzytelniania cookieless dane biletu jest serializowany do adresu URL
3. Przekierowanie użytkownika do odpowiedniej strony

Te kroki są replikowane w powyższym kodzie. Po pierwsze ciąg, który ostatecznie będą przechowywane we właściwości UserData został utworzony przez połączenie nazwy firmy i tytuł rozdzielający dwie wartości przy użyciu znaku kreski pionowej (|).

ciąg userDataString = ciąg. Concat (companyName [i], "|", titleAtCompany[i]);

Następnie FormsAuthentication.GetAuthCookie metoda jest wywoływana, co powoduje utworzenie biletu uwierzytelniania, są szyfrowane i zweryfikuje go zgodnie z ustawieniami konfiguracji i umieszcza je w obiekcie HttpCookie.

HttpCookie authCookie = FormsAuthentication.GetAuthCookie(UserName.Text, RememberMe.Checked);

Aby pracować z FormAuthenticationTicket osadzone w pliku cookie, należy wywołać klasy FormAuthentication [odszyfrować metoda](https://msdn.microsoft.com/library/system.web.security.formsauthentication.decrypt.aspx), przekazując wartość pliku cookie.

Bilet FormsAuthenticationTicket = FormsAuthentication.Decrypt(authCookie.Value);

Następnie tworzymy *nowe* FormsAuthenticationTicket wystąpienia na podstawie istniejących FormsAuthenticationTicket wartości. Jednak ten nowy bilet zawiera informacje specyficzne dla użytkownika (userDataString).

FormsAuthenticationTicket newTicket = nowe FormsAuthenticationTicket (biletu. Wersja, biletu. Nazwa, biletu. IssueDate, biletu. Wygaśnięcie, biletu. IsPersistent, userDataString);

Firma Microsoft następnie szyfrowania (i zweryfikować) nowe wystąpienie FormsAuthenticationTicket przez wywołanie metody [szyfrowania metoda](https://msdn.microsoft.com/library/system.web.security.formsauthentication.encrypt.aspx)i ponownie umieścić te dane zaszyfrowane (i zweryfikowane), w authCookie.

authCookie.Value = FormsAuthentication.Encrypt(newTicket);

Na koniec authCookie zostanie dodany do kolekcji Response.Cookies i GetRedirectUrl metoda jest wywoływana w celu określenia odpowiedniej strony, aby wysłać użytkownika.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample7.cs)]

Wszystkie tego kodu jest potrzebna, ponieważ właściwość UserData jest tylko do odczytu, a klasa uwierzytelniania formularzy nie zapewnia żadnych metod do określania informacji danych użytkownika za pomocą jej metod GetAuthCookie, SetAuthCookie lub RedirectFromLoginPage.

> [!NOTE]
> Kod, który właśnie sprawdzane są przechowywane informacje specyficzne dla użytkownika w biletu uwierzytelniania na podstawie plików cookie. Klasy odpowiedzialny za serializacji biletu uwierzytelniania formularzy do adresu URL są wewnętrzne programu .NET Framework. Długi tekst krótki, nie można zapisać danych użytkownika w biletu uwierzytelniania formularzy cookieless.


### <a name="accessing-the-userdata-information"></a>Uzyskiwanie informacji o danych użytkownika

W tym momencie nazwy firmy i tytuł każdego użytkownika są przechowywane w biletu uwierzytelniania formularzy UserData właściwości podczas logowania. Te informacje są dostępne z biletu uwierzytelniania, na dowolnej stronie usługi bez konieczności podróży w magazynie użytkownika. Aby zilustrować, jak te informacje można pobrać z właściwości UserData, zaktualizujmy Default.aspx, dzięki czemu jego komunikat powitalny obejmuje nie tylko nazwę użytkownika, ale również firmy, które pracują w i stanowiska.

Obecnie Default.aspx zawiera kontrolkę typu etykieta o nazwie WelcomeBackMessage AuthenticatedMessagePanel panelu. Ten Panel jest wyświetlana tylko do uwierzytelnionych użytkowników. Aktualizowanie kodu na stronie firmy Default.aspx\_obciążenia programu obsługi zdarzeń, tak że wygląda podobnie do poniższego:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample8.cs)]

Jeśli Request.IsAuthenticated ma wartość true, a następnie WelcomeBackMessage tekstu jest najpierw właściwością Witamy z powrotem *username*. Następnie właściwość User.Identity jest rzutowany na obiekt FormsIdentity tak, aby firma Microsoft mogą uzyskiwać dostęp do podstawowych FormsAuthenticationTicket. Gdy będziemy już mieć FormsAuthenticationTicket, firma Microsoft deserializuje właściwość UserData na nazwy firmy i tytuł. Jest to realizowane przy dzieleniu ciągu na znaku kreski pionowej. Nazwa firmy i tytuł są następnie wyświetlane w etykiecie WelcomeBackMessage.

Rysunek 5. pokazuje zrzut ekranu przedstawiający tego ekranu w działaniu. Logowanie trybie Scott wyświetla komunikat powitalny Wstecz, obejmującą Scotta firmy i tytuł.


[![Firmy i tytuł aktualnie zalogowany na użytkownika są wyświetlane](forms-authentication-configuration-and-advanced-topics-cs/_static/image14.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image13.png)

**Rysunek 05**: aktualnie zalogowanego użytkownika w firmie i tytułu są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image15.png))


> [!NOTE]
> Właściwość UserData biletu uwierzytelniania służy jako pamięci podręcznej dla użytkownika. Podobnie jak wszelkie pamięci podręcznej musi ona być aktualizowane po zmodyfikowaniu danych bazowych. Na przykład jeśli strony sieci web, z którego użytkownicy mogą zaktualizować swój profil, pola pamięci podręcznej we właściwości danych użytkownika musi zostać odświeżona w celu odzwierciedlenia zmian wprowadzonych przez użytkownika.


## <a name="step-5-using-a-custom-principal"></a>Krok 5: Przy użyciu jednostki niestandardowe

Na każde żądanie przychodzące FormsAuthenticationModule podejmuje próbę uwierzytelnienia użytkownika. Jeśli bilet uwierzytelniania nie wygasł jest obecny, FormsAuthenticationModule przypisuje właściwość HttpContext.User nowy obiekt obiektów GenericPrincipal. Ten obiekt obiektów GenericPrincipal ma tożsamość typu FormsIdentity, który zawiera odwołanie do biletu uwierzytelniania formularzy. Klasa obiektów GenericPrincipal zawiera bez minimalne funkcje wymagane przez klasę, która implementuje IPrincipal — po prostu ma właściwość tożsamości i metodę IsInRole.

Obiekt podmiotu zabezpieczeń ma dwa obowiązki: role, jakie użytkownik należy do wskazania i udostępniają informacje o tożsamości. Jest to realizowane za pośrednictwem interfejsu IPrincipal IsInRole (*roleName*) metod i właściwości tożsamości, odpowiednio. Klasy obiektów GenericPrincipal umożliwia tablica ciągów nazw ról można określić za pośrednictwem jej konstruktora; jego IsInRole (*roleName*) metoda sprawdza tylko, czy aby sprawdzić, czy przekazany w *roleName* istnieje w tablicy ciągów. Gdy FormsAuthenticationModule tworzy obiektów GenericPrincipal, przekazuje on w pustą tablicę ciągów do obiektów GenericPrincipal konstruktora. W związku z tym każde wywołanie IsInRole zawsze zwraca wartość false.

Klasy obiektów GenericPrincipal spełnia potrzeby w przypadku większości scenariuszy uwierzytelniania na podstawie formularzy, gdzie role nie są używane. W tych sytuacjach, gdy domyślna obsługa roli są niewystarczające lub gdy trzeba skojarzyć niestandardowy obiekt IIdentity z użytkownikiem, można utworzyć niestandardowy obiekt IPrincipal podczas przepływu pracy uwierzytelniania i przypisać ją do właściwości HttpContext.User.

> [!NOTE]
> Jak widać w przyszłości samouczki, gdy ASP. Framework ról w sieci jest włączone tworzy niestandardowy obiekt jednostki typu [RolePrincipal](https://msdn.microsoft.com/library/system.web.security.roleprincipal.aspx) i zastępuje obiekt obiektów GenericPrincipal utworzone uwierzytelniania formularzy. Dzieje się tak, aby dostosować metody IsInRole podmiotu zabezpieczeń do interfejsu z interfejsem API w ramach ról.


Ponieważ firma Microsoft ma nie dotyczy osoby z rolami jeszcze, jedyny przypadek, które mamy do tworzenia jednostki niestandardowej w tym momencie mogłoby być skojarzyć niestandardowy obiekt IIdentity do jednostki. W kroku 4 przyjrzeliśmy się przechowywanie dodatkowych informacji dotyczących użytkowników we właściwości UserData biletu uwierzytelniania, w szczególności, nazwy firmy użytkownika i stanowiska. Informacje UserData jest jednak tylko za pośrednictwem biletu uwierzytelniania i następnie tylko jako ciąg serializacji, co oznacza, że w dowolnym czasie chcemy, aby wyświetlić informacje o użytkownikach, przechowywane w bilecie należy przeanalizować właściwości UserData.

Firma Microsoft może poprawić środowisko programistyczne, tworząc klasę, która implementuje IIdentity i zawiera właściwości CompanyName i tytuł. Dzięki temu deweloper mają dostęp do aktualnie zalogowanego użytkownika, nazwę firmy i tytuł bezpośrednio za pomocą właściwości CompanyName i tytuł bez potrzebne wiedzieć, jak można przeanalizować właściwości UserData.

### <a name="creating-the-custom-identity-and-principal-classes"></a>Tworzenie niestandardowej tożsamości i główne klasy

W tym samouczku utworzymy niestandardowe obiekty jednostki i tożsamości w aplikacji\_katalogu z kodem. Rozpocznij od dodania aplikacji\_folderu do projektu kodu — kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wybierz opcję Dodaj Folder programu ASP.NET i wybierz aplikację\_kodu. Aplikacja\_kodu jest folderem specjalne platformy ASP.NET, zawierający klasy pliki specyficzne dla witryny sieci Web.

> [!NOTE]
> Aplikacja\_katalogu z kodem należy używać tylko, gdy zarządzania projektem za pomocą modelu projektu witryny sieci Web. Jeśli używasz [modelu projektu aplikacji sieci Web](https://msdn.microsoft.com/asp.net/Aa336618.aspx), tworzenie folderu standardowego i Dodaj klasy do tego. Na przykład można dodać nowy folder o nazwie klasy, a istnieje umieść swój kod.


Następnie dodaj dwa nowe pliki klasy aplikacji\_katalogu z kodem, jeden o nazwie CustomIdentity.cs i jedną o nazwie CustomPrincipal.cs.


[![Dodawanie klasy CustomPrincipal i CustomIdentity do projektu](forms-authentication-configuration-and-advanced-topics-cs/_static/image17.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image16.png)

**Rysunek 06**: Dodawanie CustomIdentity i CustomPrincipal klas do projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image18.png))


Klasa CustomIdentity jest odpowiedzialny za implementującej interfejs IIdentity, który definiuje typ AuthenticationType, właściwości i nazwę właściwości. Oprócz tych wymaganych właściwości przypadku interesuje NAS ujawnienia podstawowych biletu uwierzytelniania formularzy oraz właściwości dla użytkownika nazwa firmy i tytuł. Wprowadź następujący kod do klasy CustomIdentity.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample9.cs)]

Należy pamiętać, że klasa zawiera zmienną członkowską FormsAuthenticationTicket (\_ticket) i że należy podać te informacje z biletu za pośrednictwem konstruktora. Te dane biletu jest używany w zwracanie nazwy tożsamości; jego właściwość UserData jest analizowany do zwracania wartości dla właściwości Nazwa firmy i tytuł.

Następnie należy utworzyć klasa CustomPrincipal. Ponieważ firma Microsoft nie są związani z ról w tym momencie, klasa CustomPrincipal Konstruktor akceptuje tylko obiekt CustomIdentity; jego metoda IsInRole zawsze zwraca wartość false.

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample10.cs)]

### <a name="assigning-a-customprincipal-object-to-the-incoming-requests-security-context"></a>Przypisywanie obiekcie CustomPrincipal kontekst zabezpieczeń żądania przychodzącego

W efekcie powstał klasę, która rozszerza domyślną specyfikację IIdentity właściwościami CompanyName i tytuł, a także klasę jednostki niestandardowej, która korzysta z tożsamości niestandardowej. Firma Microsoft jest gotowe, aby wejść do potoku platformy ASP.NET i przypisz naszych niestandardowy obiekt podmiotu zabezpieczeń kontekst zabezpieczeń żądania przychodzącego.

Potoku platformy ASP.NET przyjmuje żądanie przychodzące i przetwarza je za pomocą kilku kroków. W każdym kroku określonego zdarzenie jest zgłaszane, umożliwiając deweloperom akademickiej potoku platformy ASP.NET i zmodyfikować żądanie określonych momentach etapie jej cyklu życia. FormsAuthenticationModule, na przykład czeka na platformę ASP.NET w celu podniesienia [zdarzeń AuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.authenticaterequest.aspx), w tym momencie sprawdza ono przychodzące żądanie na bilet uwierzytelnienia. Jeśli bilet uwierzytelnienia zostanie znaleziony, obiektów genericprincipal — jest tworzone i przypisywane do właściwości HttpContext.User.

Po wystąpieniu zdarzenia AuthenticateRequest potoku platformy ASP.NET zgłasza [zdarzeń PostAuthenticateRequest](https://msdn.microsoft.com/library/system.web.httpapplication.postauthenticaterequest.aspx), który jest, gdzie można zastąpić obiekt obiektów GenericPrincipal utworzony przez FormsAuthenticationModule z wystąpieniem klasy Nasze Obiekcie CustomPrincipal. Rysunek 7 przedstawia ten przepływ pracy.


[![Genericprincipal — zastępuje CustomPrincipal w zdarzeniu PostAuthenticationRequest](forms-authentication-configuration-and-advanced-topics-cs/_static/image20.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image19.png)

**Rysunek 07**: genericprincipal — zastępuje CustomPrincipal w zdarzeniu PostAuthenticationRequest ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image21.png))


Aby można było wykonać kod w odpowiedzi na zdarzenie potoku platformy ASP.NET, możemy utworzyć programu obsługi zdarzeń odpowiednie w pliku Global.asax lub utworzyć własną moduł HTTP. W tym samouczku utworzymy programu obsługi zdarzeń w pliku Global.asax. Rozpocznij, dodając Global.asax do swojej witryny sieci Web. Kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i Dodaj element typu globalna klasa aplikacji o nazwie pliku Global.asax.


[![Dodaj plik Global.asax do swojej witryny sieci Web](forms-authentication-configuration-and-advanced-topics-cs/_static/image23.png)](forms-authentication-configuration-and-advanced-topics-cs/_static/image22.png)

**Rysunek 08**: Dodaj plik Global.asax do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](forms-authentication-configuration-and-advanced-topics-cs/_static/image24.png))


Domyślny szablon Global.asax zawiera procedury obsługi zdarzeń dla liczby zdarzenia potoku platformy ASP.NET, w tym rozpoczęcia, zakończenia i [zdarzenie błędu](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx), między innymi. Możesz usunąć te procedury obsługi zdarzeń, jak firma Microsoft nie są potrzebne dla tej aplikacji. Zdarzenie, które jesteśmy zainteresowani jest PostAuthenticateRequest. Zaktualizuj plik Global.asax, dzięki czemu jego znaczników wygląda podobnie do następującego:

[!code-aspx[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample11.aspx)]

Aplikacja\_OnPostAuthenticateRequest metoda jest wykonywana każdorazowo, środowisko uruchomieniowe ASP.NET zgłasza zdarzenie PostAuthenticateRequest, które odbywa się raz na każde żądanie przychodzące strony. Program obsługi zdarzeń uruchamia, sprawdzając, czy użytkownik jest uwierzytelniany i został uwierzytelniony przy użyciu uwierzytelniania formularzy. Jeśli tak, nowy obiekt CustomIdentity jest utworzony i przekazany bieżącego żądania biletu uwierzytelniania w jego konstruktorze. Poniżej obiekcie CustomPrincipal jest utworzony i przekazany obiekt CustomIdentity nowo utworzoną w jego konstruktorze. Na koniec kontekstu zabezpieczeń bieżącego żądania jest przypisany do nowo utworzonego obiektu CustomPrincipal.

Należy pamiętać, że ostatni krok — kojarzenie obiekcie CustomPrincipal z kontekst zabezpieczeń żądania - przypisuje nazwę główną dwie właściwości: HttpContext.User i SE vlastnost Thread.CurrentPrincipal. Te dwa przypisania są niezbędne, ze względu na sposób, w których konteksty zabezpieczeń są obsługiwane w programie ASP.NET. .NET Framework kojarzy kontekstu zabezpieczeń z każdego uruchomionego wątku; Ta informacja jest dostępna jako obiekt IPrincipal za pośrednictwem [obiektu wątku](https://msdn.microsoft.com/library/system.threading.thread.aspx)firmy [właściwość CurrentPrincipal](https://msdn.microsoft.com/library/system.threading.thread.currentcontext.aspx). Co to jest skomplikowana to, że program ASP.NET ma swoje własne informacje kontekstu zabezpieczeń (HttpContext.User).

W niektórych scenariuszach właściwość SE vlastnost Thread.CurrentPrincipal jest badany, określając kontekst zabezpieczeń; w innych scenariuszach HttpContext.User jest używany. Na przykład funkcje zabezpieczeń na platformie .NET, które umożliwiają deweloperom deklaratywne stanu co użytkownicy lub role można utworzyć wystąpienia klasy lub wywoływania metod określone (zobacz [Dodawanie reguły autoryzacji w celu biznesowych i danych za pomocą warstwy PrincipalPermissionAttributes](https://weblogs.asp.net/scottgu/archive/2006/10/04/Tip_2F00_Trick_3A00_-Adding-Authorization-Rules-to-Business-and-Data-Layers-using-PrincipalPermissionAttributes.aspx)). Obiekcie nadrzędnym tych technik deklaratywne określenia kontekstu zabezpieczeń, za pomocą właściwości SE vlastnost Thread.CurrentPrincipal.

W innych scenariuszach właściwość HttpContext.User jest używana. Na przykład w poprzednim samouczku użyliśmy tej właściwości do wyświetlenia nazwy użytkownika aktualnie zalogowanego użytkownika. Wyraźnie widać następnie należy bezwzględnie czy informacje kontekstu zabezpieczeń we właściwościach SE vlastnost Thread.CurrentPrincipal i HttpContext.User dopasować.

Środowisko uruchomieniowe ASP.NET automatycznie synchronizuje te wartości właściwości dla nas. Jednak ta synchronizacja jest wykonywana po wystąpieniu zdarzenia AuthenticateRequest, ale *przed* PostAuthenticateRequest zdarzenia. W związku z tym podczas dodawania niestandardowej jednostki w zdarzeniu PostAuthenticateRequest musimy mieć pewność ręcznie przypisać SE vlastnost Thread.CurrentPrincipal — w przeciwnym razie SE vlastnost Thread.CurrentPrincipal i HttpContext.User zostaną zsynchronizowane. Zobacz [Context.User programu vs. SE vlastnost Thread.CurrentPrincipal](http://leastprivilege.com/2005/11/23/context-user-vs-thread-currentprincipal/) bardziej szczegółowe omówienie dotyczące tego problemu.

### <a name="accessing-the-companyname-and-title-properties"></a>Uzyskiwanie dostępu do CompanyName i właściwości tytułu

Zawsze, gdy żądanie dociera i jest wysyłane do aparatu programu ASP.NET, aplikację\_nastąpi OnPostAuthenticateRequest obsługi zdarzeń w pliku Global.asax. Jeśli wniosek został pomyślnie uwierzytelniony przez FormsAuthenticationModule, program obsługi zdarzeń utworzy nowy obiekt CustomPrincipal obiektu CustomIdentity, w oparciu o biletu uwierzytelniania formularzy. Tę logikę w miejscu uzyskiwanie dostępu do informacji na temat aktualnie zalogowanego użytkownika nazwy firmy i tytuł jest niezwykle proste.

Wróć do strony\_obciążenia obsługi zdarzeń w Default.aspx, gdzie w kroku 4 napisaliśmy kodu w celu pobrania biletu uwierzytelniania formularzy i przeanalizować właściwości danych użytkownika w celu wyświetlenia nazwy firmy i nazwa użytkownika. Z obiektami CustomPrincipal i CustomIdentity używany obecnie nie ma potrzeby można przeanalizować wartości z właściwości UserData biletu. Zamiast tego po prostu pobrać odwołanie do obiektu CustomIdentity i użyj jego właściwości CompanyName i tytułu:

[!code-csharp[Main](forms-authentication-configuration-and-advanced-topics-cs/samples/sample12.cs)]

## <a name="summary"></a>Podsumowanie

W tym samouczku zbadaliśmy jak dostosować ustawienia systemu uwierzytelnianie formularzy przy użyciu pliku Web.config. Zobaczyliśmy, jak obsługiwany jest wygaśnięcia biletu uwierzytelniania i jak szyfrowanie i weryfikacja zabezpieczenia są używane do ochrony--ticket z inspekcji i modyfikacji. Na koniec opisano sposób używania właściwości UserData bilet uwierzytelniania do przechowywania dodatkowych informacji dotyczących użytkowników w bilecie sam i jak za pomocą niestandardowych obiektów podmiot zabezpieczeń i tożsamości ujawnić te informacje w sposób bardziej przyjazne dla deweloperów.

W tym samouczku kończy się nasze badania uwierzytelniania formularzy w programie ASP.NET. Następny Samouczek rozpoczyna się nasza podróż w ramach członkostwa.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Analiza uwierzytelniania formularzy](http://aspnet.4guysfromrolla.com/articles/072005-1.aspx)
- [Objaśniono: Uwierzytelnianie formularzy w programie ASP.NET 2.0](https://msdn.microsoft.com/library/aa480476.aspx)
- [Instrukcje: Ochrona uwierzytelniania formularzy w programie ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)
- [Profesjonalne platformy ASP.NET 2.0 zabezpieczeń, członkostwo i zarządzanie rolami](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [Zabezpieczanie kontrolek logowania](https://msdn.microsoft.com/library/ms178346.aspx)
- [&lt;Uwierzytelniania&gt; — Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [&lt;Formularzy&gt; elementu &lt;uwierzytelniania&gt;](https://msdn.microsoft.com/library/1d3t3c61.aspx)
- [&lt;MachineKey&gt; — Element](https://msdn.microsoft.com/library/w8h3skw9.aspx)
- [Opis biletu uwierzytelniania formularzy i plików Cookie](https://support.microsoft.com/kb/910443)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenie wideo na tematy zawarte w tym samouczku

- [Jak zmienić właściwości uwierzytelniania formularzy](../../../videos/authentication/how-to-change-the-forms-authentication-properties.md)
- [Instrukcje instalacji i używania uwierzytelniania bez plików Cookie w aplikacji ASP.NET](../../../videos/authentication/how-to-setup-and-use-cookie-less-authentication-in-an-aspnet-application.md)
- [Relokacja logowania formularzy stron ASP](../../../videos/authentication/asp-forms-login-relocation.md)
- [Konfiguracja niestandardowa kluczy logowania formularzy](../../../videos/authentication/forms-login-custom-key-configuration.md)
- [Dodawanie niestandardowych danych do metody uwierzytelniania](../../../videos/authentication/add-custom-data-to-the-authentication-method.md)
- [Używanie niestandardowych obiektów głównych](../../../videos/authentication/use-custom-principal-objects.md)

### <a name="about-the-author"></a>Informacje o autorze

Pracował nad Bento Scottem, autor wiele książek ASP/ASP.NET i założyciel 4GuysFromRolla.com, przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena  *[Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)*. Scott można z Tobą skontaktować w [ mitchell@4guysfromrolla.com ](mailto:mitchell@4guysfromrolla.com) lub za pośrednictwem jego blog znajduje się na [ http://ScottOnWriting.NET ](http://scottonwriting.net/).

### <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Weryfikacja potencjalnych klientów w ramach tego samouczka został Alicja Maziarz. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4guysfromrolla.com).

> [!div class="step-by-step"]
> [Poprzednie](an-overview-of-forms-authentication-cs.md)
> [dalej](security-basics-and-asp-net-support-vb.md)
