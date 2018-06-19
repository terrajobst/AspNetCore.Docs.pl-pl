---
uid: web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
title: Omówienie uwierzytelniania formularzy (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: Tworzenie trasy niestandardowe
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/14/2008
ms.topic: article
ms.assetid: de2d65b9-aadc-42ba-abe1-4e87e66521a0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs
msc.type: authoredcontent
ms.openlocfilehash: 1f64384d403f3cf81ffa3327a81b635bc71e2b44
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892039"
---
<a name="an-overview-of-forms-authentication-c"></a>Omówienie uwierzytelniania formularzy (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/ASPNET_Security_Tutorial_02_CS.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/2/F/7/2F705A34-F9DE-4112-BBDE-60098089645E/aspnet_tutorial02_FormsAuth_cs.pdf)

> W tym samouczku będziemy zmieni się z zaledwie dyskusji implementacji; w szczególności zajmiemy Implementowanie uwierzytelniania formularzy. Rozpoczniemy konstruowania w tym samouczku aplikacji sieci web będą nadal być wbudowane w system w kolejnych samouczkach, jak możemy przenieść z uwierzytelniania formularzy proste do członkostwa oraz ról.
> 
> Zobacz ten film, aby uzyskać więcej informacji na ten temat: [przy użyciu podstawowego uwierzytelniania formularzy w programie ASP.NET](# "using-basic-forms-authentication-in-aspnet").


## <a name="introduction"></a>Wprowadzenie

W [poprzedniego samouczek](security-basics-and-asp-net-support-cs.md) Rozmawialiśmy różnych uwierzytelniania, autoryzacji i użytkownika konta opcji dostarczane przez platformę ASP.NET. W tym samouczku będziemy zmieni się z zaledwie dyskusji implementacji; w szczególności zajmiemy Implementowanie uwierzytelniania formularzy. Rozpoczniemy konstruowania w tym samouczku aplikacji sieci web będą nadal być wbudowane w system w kolejnych samouczkach, jak możemy przenieść z uwierzytelniania formularzy proste do członkostwa oraz ról.

W tym samouczku rozpoczyna się od omówiono przepływu pracy uwierzytelniania formularzy, temat, który firma Microsoft dotknięciu po poprzednim samouczka. Następujące, utworzymy witryny sieci Web ASP.NET za pośrednictwem której do demonstracją pojęcia związane z uwierzytelniania formularzy. Firma Microsoft będzie skonfiguruj lokacji korzystają z uwierzytelniania formularzy, tworzyć strony logowania proste i zobaczyć jak ustalić, w kodzie, czy użytkownik jest uwierzytelniany, a jeśli tak, nazwa użytkownika, są rejestrowane przy użyciu.

Opis formularzy, przepływu pracy uwierzytelniania, umożliwiając w aplikacji sieci web i tworzenie stron logowania i wylogowywania są wszystkie niezbędne kroki tworzenia aplikacji programu ASP.NET, która obsługuje konta użytkowników i uwierzytelnia użytkowników za pomocą strony sieci web. W związku z tym — i ponieważ zależą od siebie nawzajem — samouczki I czy zachęca do pracy w tym samouczku w pełni przed przejściem do następnego nawet wtedy, gdy użytkownik ma już Konfigurowanie obsługi uwierzytelnianie formularzy w ciągu ostatnich projektów.

## <a name="understanding-the-forms-authentication-workflow"></a>Opis przepływu pracy uwierzytelniania formularzy

Gdy moduł wykonawczy platformy ASP.NET przetwarza żądanie dla zasobu ASP.NET, takich jak strony ASP.NET lub usługi sieci Web ASP.NET, żądanie podnosi liczbę zdarzeń podczas jego cyklu życia. Brak zdarzeń zgłaszanych na końcu bardzo początkowe i bardzo żądania, takich, które jest wywoływane, gdy żądanie jest trwa uwierzytelnieniu i autoryzacji, zdarzenie zgłaszane w przypadku nieobsługiwany wyjątek i tak dalej. Aby wyświetlić pełną listę zdarzeń, zobacz [zdarzenia obiektów obiekcie HttpApplication](https://msdn.microsoft.com/library/system.web.httpapplication_events.aspx).

*Moduły HTTP* są klasami zarządzanymi, którego kod jest wykonywany w odpowiedzi na wystąpienie określonego zdarzenia w cyklu życia żądania. Program ASP.NET jest dostarczany z modułów HTTP wykonywania podstawowych zadań w tle. Są dwa wbudowane moduły HTTP, które są szczególnie istotne w naszym dyskusji:

- **[`FormsAuthenticationModule`](https://msdn.microsoft.com/library/system.web.security.formsauthenticationmodule.aspx)** — uwierzytelnia użytkownika, sprawdzając biletu uwierzytelniania formularzy, który zazwyczaj znajduje się w kolekcji plików cookie przez użytkownika. Jeśli nie biletu uwierzytelniania formularzy, użytkownik jest anonimowy.
- **[`UrlAuthorizationModule`](https://msdn.microsoft.com/library/system.web.security.urlauthorizationmodule.aspx)** — Określa, czy bieżący użytkownik jest autoryzowany do dostępu do żądanego adresu URL. Ten moduł określa jako źródło przez zapoznanie się z reguł autoryzacji, określona w plikach konfiguracji aplikacji. Zawiera również ASP.NET [ `FileAuthorizationModule` ](https://msdn.microsoft.com/library/system.web.security.fileauthorizationmodule.aspx) określający urzędu, przeglądając pliki żądanej listy kontroli dostępu.

`FormsAuthenticationModule` Próby uwierzytelnienia użytkownika przed `UrlAuthorizationModule` (i `FileAuthorizationModule`) wykonywania. Jeśli użytkownik, zgłoszenia żądania nie ma uprawnień dostępu do żądanego zasobu, moduł autoryzacji kończy żądanie i zwraca [HTTP 401 nieautoryzowane](http://www.checkupdown.com/status/E401.html) stanu. W scenariuszach uwierzytelniania systemu Windows stan HTTP 401, jest zwracana do przeglądarki. Ten kod stanu powoduje, że przeglądarka monit o podanie swoich poświadczeń za pomocą modalne okno dialogowe. Za pomocą uwierzytelniania formularzy, stan HTTP 401 nieautoryzowane nigdy nie są wysyłane do przeglądarki ponieważ FormsAuthenticationModule wykrywa tego stanu i modyfikuje je przekierowanie użytkownika do strony logowania, zamiast tego (za pośrednictwem [HTTP 302 przekierowania](http://www.checkupdown.com/status/E302.html) stanu).

Odpowiedzialność strony logowania ma na celu określenie, czy poświadczenia użytkownika są prawidłowe, a jeśli tak, aby utworzyć biletu uwierzytelniania formularzy i przekieruje użytkownika z powrotem do strony one próba można znaleźć w. Bilet uwierzytelniania znajduje się w kolejnych żądań wysyłanych do stron w witrynie sieci Web, które `FormsAuthenticationModule` używa do identyfikacji użytkownika.


![Przepływ uwierzytelniania formularzy](an-overview-of-forms-authentication-cs/_static/image1.png)

**Rysunek 1**: przepływ uwierzytelniania formularzy


### <a name="remembering-the-authentication-ticket-across-page-visits"></a>Zapamiętywanie biletu uwierzytelniania między wizyt strony

Po zalogowaniu biletu uwierzytelniania formularzy musi zostać odesłany do serwera sieci web na każdym żądaniu, aby użytkownik pozostanie zalogowany, zgodnie z ich przeglądanie witryny. Zazwyczaj jest to osiągane przez umieszczenie biletu uwierzytelniania w kolekcji plików cookie przez użytkownika. [Pliki cookie](http://en.wikipedia.org/wiki/HTTP_cookie) to małe pliki tekstowe, które znajdują się na komputerze użytkownika i są przesyłane w nagłówkach HTTP dla każdego żądania do witryny sieci Web, która utworzyła plik cookie. Dlatego po utworzeniu biletu uwierzytelniania formularzy i przechowywane w plikach cookie w przeglądarce, każda kolejne wizyta tej lokacji wysyła biletu uwierzytelniania wraz z żądaniem, a tym samym identyfikacji użytkownika.

Jednym aspekcie plików cookie jest ich wygaśnięciem, czyli datę i godzinę, co spowoduje odrzucenie pliku cookie przeglądarki. Wygaśnięcia pliku cookie uwierzytelniania formularzy użytkownika nie może być uwierzytelniona i w związku z tym stają się anonimowe. Podczas odwiedzania z publicznego terminalu, prawdopodobnie będzie ich biletu uwierzytelniania wygaśnie przy zamykaniu przeglądarki. Podczas przeglądania w domu, jednak ten sam użytkownik może być bilet uwierzytelniania do zapamiętanych uruchomieniach przeglądarki, tak aby nie miały do logowania się ponownie w każdym odwiedzają witrynę. Ta decyzja jest często wprowadzone przez użytkownika w postaci "Zapamiętaj mnie" wyboru na stronie logowania. W kroku 3 omówione implementowania pola wyboru "Zapamiętaj mnie" na stronie logowania. Samouczek następujące adresy ustawienia limitu czasu biletu uwierzytelniania szczegółowo.

> [!NOTE]
> Istnieje możliwość, że agent użytkownika używane do logowania się do witryny sieci Web może nie obsługiwać pliki cookie. W takim przypadku platformy ASP.NET można użyć biletów uwierzytelniania formularzy bez plików cookie. W tym trybie biletu uwierzytelniania jest zakodowane w adresie URL. Zajmiemy gdy biletów bez plików cookie uwierzytelniania są używane, i jak są tworzone i zarządzane w następnym samouczku.


### <a name="the-scope-of-forms-authentication"></a>Zakres uwierzytelniania formularzy

`FormsAuthenticationModule` Jest zarządzanego kodu, który jest częścią środowiska uruchomieniowego ASP.NET. Przed firmy Microsoft w wersji 7 [Internet Information Services (IIS)](https://www.iis.net/) serwera sieci web wystąpił różne bariery między potoku HTTP przez usługi IIS i środowiskiem uruchomieniowym ASP.NET, potok. Krótko mówiąc, w usługach IIS 6 i starszych `FormsAuthenticationModule` wykonywana tylko wtedy, gdy żądanie jest delegowane za pomocą programu IIS do środowiska wykonawczego programu ASP.NET. Domyślnie usługi IIS przetwarzają zawartość statyczną, sama — podobnie jak strony HTML i CSS i pliki obrazów — i tylko ręce poza żądań do aparatu ASP.NET, czy zostanie zażądana strona z rozszerzeniem .aspx, .asmx i ashx.

IIS 7, jednak umożliwia zintegrowanych usług IIS i platformy ASP.NET potoków. Przy użyciu kilku ustawień konfiguracji należy skonfigurować usług IIS 7 do wywołania FormsAuthenticationModule dla *wszystkie* żądań. Ponadto z usług IIS 7 można zdefiniować reguł autoryzacji adresów URL dla plików dowolnego typu. Aby uzyskać więcej informacji, zobacz [zmiany między IIS6 i zabezpieczeń IIS7](https://www.iis.net/learn/get-started/whats-new-in-iis-7/changes-in-security-between-iis-60-and-iis-7-and-above), [Your zabezpieczeń platformy sieci Web](https://www.iis.net/learn/get-started/whats-new-in-iis-7/iis7-and-above-security-improvements), i [Autoryzacja adresów URL usług IIS7 opis](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/URL-Authorization/Understanding-IIS7-URL-Authorization).

Długi tekst short, w wersjach wcześniejszych niż IIS 7, tylko służy uwierzytelniania formularzy do ochrony zasobów obsługiwane przez moduł wykonawczy platformy ASP.NET. Podobnie reguł autoryzacji adresów URL są stosowane tylko do zasobów obsługiwane przez moduł wykonawczy platformy ASP.NET. Jednak z usług IIS 7 jest możliwe do integracji FormsAuthenticationModule i UrlAuthorizationModule w potoku HTTP przez usługi IIS, rozszerzając w ten sposób tę funkcję do wszystkich żądań.

## <a name="step-1-creating-an-aspnet-website-for-this-tutorial-series"></a>Krok 1: Tworzenie witryny sieci Web platformy ASP.NET dla tej serii samouczka

Aby można było uzyskać dostęp do najszerszych możliwych odbiorców, witryny sieci Web ASP.NET, firma Microsoft będzie kompilowania w tej serii zostanie utworzony z firmy Microsoft bezpłatna wersja programu Visual Studio 2008, [Visual Web Developer 2008](https://www.microsoft.com/express/vwd/). Wprowadzimy `SqlMembershipProvider` magazynu użytkowników w [Microsoft SQL Server 2005 Express Edition](https://msdn.microsoft.com/sql/Aa336346.aspx) bazy danych. Jeśli używasz programu Visual Studio 2005 lub innej wersji programu Visual Studio 2008 lub SQL Server, nie martw się — kroki są niemal identyczne i będzie wskazać nieuproszczony różnice.

> [!NOTE]
> Używane w każdym samouczek aplikacji sieci web pokaz jest dostępny do pobrania. Ta aplikacja do pobrania został utworzony z programu Visual Web Developer 2008 przeznaczony dla platformy .NET Framework w wersji 3.5. Ponieważ aplikacja jest przeznaczona dla .NET 3.5, jego plik Web.config zawiera elementy konfiguracji dodatkowych, specyficzne dla 3.5. Długie wątku, short, jeśli masz jeszcze do zainstalowania .NET 3.5 na komputerze następnie aplikacji sieci web do pobrania nie będzie działać bez pierwszego znacznika specyficzne dla 3.5 usunięcia z pliku Web.config.


Przed można skonfigurować uwierzytelnianie formularzy, należy najpierw witryny sieci Web platformy ASP.NET. Rozpocznij od utworzenia nowego pliku oparte na systemie ASP.NET witryny sieci Web. Aby to zrobić, uruchom Visual Web Developer, a następnie przejdź do menu Plik i wybierz nową witrynę sieci Web w celu wyświetlania okna dialogowego nowej witryny sieci Web. Wybierz szablon witryny sieci Web ASP.NET, wartość na liście rozwijanej lokalizacji do systemu plików, wybierz folder, który można umieścić witrynę sieci web i ustawioną języka C#. Spowoduje to utworzenie nowej witryny sieci web za pomocą strony Default.aspx ASP.NET aplikacji\_folderu danych i pliku Web.config.

> [!NOTE]
> Program Visual Studio obsługuje dwa tryby zarządzania projektem: witryny sieci Web i projektów aplikacji sieci Web. Projektów witryny sieci Web brakuje pliku projektu, podczas gdy projekty aplikacji sieci Web naśladować architektura projektu w Visual Studio .NET 2002/2003 — dołączenie pliku projektu i kompilowanie kodu źródłowego projektu w ramach jednego zestawu, który jest umieszczony w folderze/bin. Visual Studio 2005 projektów początkowo tylko obsługiwane witryny sieci Web, mimo że projektu aplikacji sieci Web modelu została przywrócona z dodatkiem Service Pack 1; Program Visual Studio 2008 oferuje oba modele projektu. Visual 2005 Developer sieci Web i wersje 2008, jednak obsługują tylko projektów witryny sieci Web. I będzie używać modelu projekt witryny sieci Web. Jeśli używasz wersji-Express i chcesz użyć [modelu projektu aplikacji sieci Web](https://msdn.microsoft.com/library/aa730880%28vs.80%29.aspx) zamiast tego możesz to zrobić, ale należy pamiętać, że może zostać pewne rozbieżności między informacje wyświetlane na ekranie i kroki należy wykonać w porównaniu z zrzuty ekranu pokazano i instrukcje podane w tych samouczkach.


[![Tworzenie nowego pliku oparte na systemie witryny sieci Web](an-overview-of-forms-authentication-cs/_static/image3.png)](an-overview-of-forms-authentication-cs/_static/image2.png)

**Rysunek 2**: tworzenie witryny sieci Web New File System-Based ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image4.png))


### <a name="adding-a-master-page"></a>Dodawanie strony wzorcowej

Następnie dodaj nową stronę wzorcową do lokacji w katalogu głównym o nazwie Site.master. [Strony wzorcowe](https://msdn.microsoft.com/library/wtxbf3hh.aspx) włączyć dewelopera strony zdefiniować szablon całej lokacji, który można zastosować do stron ASP.NET. Największą zaletą stron wzorcowych jest, że ogólny wygląd witryny można zdefiniować w jednej lokalizacji, co ułatwia aktualizacji lub dostosować układu witryny.


[![Dodaj stronę wzorcową o nazwie Site.master witryny sieci Web](an-overview-of-forms-authentication-cs/_static/image6.png)](an-overview-of-forms-authentication-cs/_static/image5.png)

**Rysunek 3**: Dodaj Site.master o nazwie wzorca strony do witryny sieci Web ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image7.png))


Zdefiniuj tutaj układ strony całej lokacji, na stronie głównej. Można użyć widoku projektu i Dodaj formanty niezależnie od układu lub sieci Web należy, lub można ręcznie dodać kod znaczników ręcznie w widoku źródła. Strukturę I układ strony wzorcowej naśladować układu używane w mojej *[Praca z danymi w programie ASP.NET 2.0](../../data-access/index.md)* samouczka serii (patrz rysunek 4). Używa strony wzorcowej [kaskadowych arkuszy stylów](http://www.w3schools.com/css/default.asp) pozycjonowanie i style z ustawieniami CSS zdefiniowanych w pliku Style.css (który jest dołączony do pobierania tego samouczka). Gdy nie można ustalić, z poziomu znacznika pokazano poniżej, reguły CSS są zdefiniowane tak, aby nawigacji &lt;div&gt;w zawartości jest bezwzględnego, który pojawia się po lewej stronie i ma stałą szerokość 200 pikseli.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample1.aspx)]

Strona wzorcowa definiuje zarówno układ strony statyczne, jak i regionów, które mogą być edytowane przez stron ASP.NET, które używają strony wzorcowej. Te zawartości edytowalnych są wskazywane przez `ContentPlaceHolder` kontroli, które są widoczne w obrębie zawartości &lt;div&gt;. Nasze strony wzorcowej ma jeden `ContentPlaceHolder` (znacznika), ale strony wzorcowej mogą mieć wiele Elementy ContentPlaceHolders.

Adiustację wprowadzony powyżej przełączając do widoku projektu zawiera układ strony wzorcowej. Żadnych stron ASP.NET, które używają tej strony wzorcowej będzie mieć tego układu jednolite, możliwość określenia kod znaczników dla `MainContent` regionu.


[![Strona wzorcowa, podczas wyświetlania za pośrednictwem widoku Projekt](an-overview-of-forms-authentication-cs/_static/image9.png)](an-overview-of-forms-authentication-cs/_static/image8.png)

**Rysunek 4**: strona wzorcowa, podczas wyświetlania za pośrednictwem widoku Projekt ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image10.png))


### <a name="creating-content-pages"></a>Tworzenie strony z zawartością

W tym momencie mamy stronę Default.aspx w naszej witrynie sieci Web, ale nie używa strony wzorcowej, którą właśnie utworzyliśmy. Mimo że jest możliwa do manipulowania deklaratywne znaczników strony sieci web do strony głównej, jeśli strona nie zawiera żadnej zawartości jeszcze łatwiej jest po prostu Usuń stronę i ponownie dodać do projektu, określając strony wzorcowej do użycia. W związku z tym uruchomienia przez usunięcie Default.aspx z projektu.

Następnie kliknij prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań i wybierz dodać nowy formularz sieci Web o nazwie Default.aspx. Teraz, zaznacz pole wyboru "Wybierz strony wzorcowej" i wybierz z listy stronę wzorcową Site.master.


[![Dodaj nową stronę Default.aspx wybranie wybierz strony wzorcowej](an-overview-of-forms-authentication-cs/_static/image12.png)](an-overview-of-forms-authentication-cs/_static/image11.png)

**Rysunek 5**: dodawanie nowych Default.aspx strony wybranie wybierz strony wzorcowej ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image13.png))


![Użyj strony wzorcowej Site.master](an-overview-of-forms-authentication-cs/_static/image14.png)

**Rysunek 6**: Użyj strony wzorcowej Site.master


> [!NOTE]
> Jeśli używasz modelu projektu aplikacji sieci Web w oknie dialogowym Dodawanie nowego elementu nie zawiera pola wyboru "Wybierz strony głównej". Zamiast tego należy dodać element typu "Formularza zawartości sieci Web." Po opcji "Formularza zawartości sieci Web", a następnie klikając przycisk Dodaj, Visual Studio spowoduje wyświetlenie tego samego wzorca wybierz okno dialogowe przedstawiono na rysunku 6.


Nowa strona Default.aspx deklaratywne znaczników obejmuje tylko @Page dyrektywy Określanie ścieżki do poziomu głównego dla elementu ContentPlaceHolder znacznika strony wzorcowej strony plików i zawartości formantu.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample2.aspx)]

Na razie Default.aspx może pozostać puste. Wrócimy do niego później w tym samouczku, aby dodać zawartość.

> [!NOTE]
> Nasze strony głównej zawiera sekcja menu lub inny interfejs nawigacji. W przyszłości samouczku utworzymy takiego interfejsu.

## <a name="step-2-enabling-forms-authentication"></a>Krok 2: Włączanie uwierzytelniania formularzy

Z witryny internetowej ASP.NET utworzona naszych następne zadanie jest aby włączyć uwierzytelnianie formularzy. Konfiguracja uwierzytelniania aplikacji jest określany za pośrednictwem [ `<authentication>` elementu](https://msdn.microsoft.com/library/532aee0e.aspx) w pliku Web.config. `<authentication>` Element zawiera jeden atrybut o nazwie tryb określający model uwierzytelniania używany przez aplikację. Ten atrybut może mieć jeden z następujące cztery wartości:

- **Windows** — zgodnie z opisem w powyższej samouczek, gdy aplikacja używa uwierzytelniania systemu Windows jest odpowiedzialny za serwer sieci web do uwierzytelniania osoby odwiedzającej jest możliwe i zwykle odbywa się za pośrednictwem podstawowe, szyfrowane lub zintegrowane z systemem Windows uwierzytelnianie.
- **Formularze**— użytkownicy są uwierzytelniani za pomocą formularza na stronie sieci web.
- **Usługa Passport**— użytkownicy są uwierzytelniani za pomocą sieci firmy Microsoft Passport.
- **Brak**— bez modelu uwierzytelniania jest używany; wszystkich odwiedzających są anonimowe.

Domyślnie aplikacje ASP.NET używa uwierzytelniania systemu Windows. Aby zmienić typ uwierzytelniania do uwierzytelniania formularzy, następnie należy zmodyfikować `<authentication>` atrybut tryb elementu formularzy.

Jeśli projekt nie zawiera jeszcze pliku Web.config, należy dodać co teraz przez kliknięcie prawym przyciskiem myszy nazwę projektu w Eksploratorze rozwiązań, wybierając pozycję Dodaj nowy element, a następnie dodanie pliku konfiguracji sieci Web.


[![Jeśli projekt nie ma jeszcze pliku Web.config, dodaj je teraz](an-overview-of-forms-authentication-cs/_static/image16.png)](an-overview-of-forms-authentication-cs/_static/image15.png)

**Rysunek 7**: Jeśli swój projekt jest nie jeszcze obejmują plik Web.config, Dodaj teraz ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image17.png))


Następnie odszukaj `<authentication>` element i zaktualizuj go do użycia uwierzytelnianie formularzy. Po tej zmianie znacznika w pliku Web.config powinien wyglądać podobnie do poniższej:

[!code-xml[Main](an-overview-of-forms-authentication-cs/samples/sample3.xml)]

> [!NOTE]
> Ponieważ plik Web.config jest plik XML, ważne jest wielkość liter. Upewnij się, ustaw atrybut tryb formularzy, z kapitału "F". Jeśli używasz innej wielkości znaków, takich jak "Form", otrzymasz błąd konfiguracji podczas odwiedzania witryn za pośrednictwem przeglądarki.


`<authentication>` Element może opcjonalnie `<forms>` elementu podrzędnego, który zawiera ustawienia specyficzne dla uwierzytelniania formularzy. Teraz załóżmy wystarczy użyć domyślne ustawienia uwierzytelniania formularzy. Przeanalizujemy `<forms>` element podrzędny bardziej szczegółowo w następnej instrukcji.

## <a name="step-3-building-the-login-page"></a>Krok 3: Tworzenie strony logowania

Aby zapewnić obsługę uwierzytelniania formularzy naszą witrynę sieci Web musi strony logowania. Zgodnie z opisem w sekcji "Opis formularze przepływ pracy uwierzytelniania" `FormsAuthenticationModule` zostanie automatycznie przekierowanie użytkownika do strony logowania przy próbie dostępu do strony, które nie mają uprawnień do wyświetlania. Dostępne są także formantów sieci Web ASP.NET, które zostanie wyświetlone łącze do strony logowania do użytkowników anonimowych. Begs to pytanie, "Co to jest adres URL strony logowania?"

Domyślnie system uwierzytelniania formularzy oczekuje strony logowania, aby nosić Login.aspx i umieszczane w katalogu głównym aplikacji sieci web. Jeśli chcesz używać adresu URL strony logowania innego, możesz to zrobić, podając go w pliku Web.config. Zostanie wyświetlone jak to zrobić, kolejne kroki samouczka.

Strona logowania ma trzy obowiązki:

1. Udostępniają interfejs, umożliwiający obiektu odwiedzającego wprowadzić swoje poświadczenia.
2. Ustal, czy przesłane poświadczenia są prawidłowe.
3. "Zaloguj" użytkownika przez utworzenie biletu uwierzytelniania formularzy.

### <a name="creating-the-login-pages-user-interface"></a>Tworzenie interfejsu użytkownika strony logowania

Rozpocznijmy pierwszego zadania. Dodaj nową stronę ASP.NET do katalogu głównego witryny o nazwie Login.aspx i powiązać ją z Site.master strony wzorcowej.


[![Dodaj nową stronę ASP.NET o nazwie Login.aspx](an-overview-of-forms-authentication-cs/_static/image19.png)](an-overview-of-forms-authentication-cs/_static/image18.png)

**Rysunek 8**: dodawanie nowych ASP.NET stronę o nazwie Login.aspx ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image20.png))


Interfejs strony logowania typowe składa się z dwóch pól tekstowych — jeden dla nazwy użytkownika, jedną dla swojego hasła — oraz przycisk służący do przesyłania formularza. Witryny sieci Web często zawierają wyboru "Zapamiętaj mnie", który jeśli zaznaczone, będzie nadal występował wynikowy biletu uwierzytelniania uruchomieniach przeglądarki.

Dodaj dwa pola tekstowe Login.aspx i zestawu ich `ID` właściwości nazwy użytkownika i hasła, odpowiednio. Ustawienia hasła w `TextMode` właściwości hasła. Następnie należy dodać kontrolkę pola wyboru, ustawienie jej `ID` właściwości RememberMe i jego `Text` "Pamiętaj Me" dla właściwości. Następujące, Dodaj przycisk o nazwie LoginButton którego `Text` właściwość jest ustawiona na "Login". Na koniec Dodaj formant etykiety w sieci Web i ustawić jej `ID` właściwości InvalidCredentialsMessage, jego `Text` dla właściwości "Nazwa użytkownika lub hasło jest nieprawidłowe. Spróbuj ponownie. ", jego `ForeColor` właściwości czerwony, a jego `Visible` wartość False dla właściwości.

W tym momencie ekranie powinien wyglądać podobnie do ekranu zrzut na rysunku nr 9 i składni deklaratywnej ze strony powinny takie jak następujące:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample4.aspx)]


[![Strona logowania zawiera dwa pola tekstowe, pole wyboru, przycisk i etykiety](an-overview-of-forms-authentication-cs/_static/image22.png)](an-overview-of-forms-authentication-cs/_static/image21.png)

**Rysunek 9**: logowania strony zawiera dwa pola tekstowe, pole wyboru, przycisk i etykiety ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image23.png))


Na koniec Utwórz program obsługi zdarzeń dla LoginButton kliknij zdarzenie. Przy użyciu projektanta kliknij dwukrotnie ikonę formantu przycisku, aby utworzyć ten program obsługi zdarzeń.

### <a name="determining-if-the-supplied-credentials-are-valid"></a>Określanie, czy podane poświadczenia są prawidłowe

Teraz musisz zaimplementować zadanie 2 w kliknięcia przycisku program obsługi zdarzeń — Określanie, czy podane poświadczenia są prawidłowe. W tym celu musi istnieć magazynu użytkownika, który przechowuje wszystkie poświadczenia użytkowników, dzięki czemu można określić, czy podane poświadczenia zgodny z żadnych znanych poświadczeń.

Przed składnika ASP.NET 2.0 deweloperzy zostały odpowiedzialnych za wdrażanie własnych magazyny użytkowników i pisanie kodu można sprawdzić poprawności podanych poświadczeń z magazynu. Większość deweloperów czy implementuje magazynie użytkownika w bazie danych, tworzenie tabeli o nazwie użytkowników z kolumnami, takie jak nazwa użytkownika, hasło, poczty E-mail, LastLoginDate i tak dalej. Ta tabela musi następnie jeden rekord dla każdego konta użytkownika. Weryfikowanie podanych poświadczeń użytkownika wymagałoby kwerend bazy danych dla zgodnego nazwy użytkownika i sprawdzeniu, czy podane hasło odpowiadał hasła w bazie danych.

Ze składnika ASP.NET 2.0 deweloperzy należy używać jednego z dostawców członkostwa do zarządzania Sklepem użytkownika. W tym samouczku użyjemy SqlMembershipProvider, który korzysta z bazy danych programu SQL Server dla magazynu użytkowników. Korzystając z SqlMembershipProvider musimy zaimplementować schemat określonej bazy danych, który zawiera tabele, widoki i procedury składowane oczekiwany przez dostawcę. Omówione zostaną sposobu wdrażania tego schematu w ***tworzenie schematu członkostwa w programie SQL Server*** samouczka. Z dostawcy członkostwa w miejscu, sprawdzanie poprawności poświadczeń użytkownika jest równie proste co wywołanie [klasy członkostwa](https://msdn.microsoft.com/library/system.web.security.membership.aspx)w [funkcja ValidateUser (*username*, *hasło*) metody](https://msdn.microsoft.com/library/system.web.security.membership.validateuser.aspx), która zwraca wartość Boolean wskazującą czy ważności *nazwy użytkownika* i *hasło* kombinacji. Zobaczysz, jak firma Microsoft ma nie zaimplementowano jeszcze SqlMembershipProvider magazynu użytkowników, nie można użyć metody funkcja ValidateUser klasy członkostwo w tej chwili.

Zamiast Poświęć chwilę, aby utworzyć własne niestandardowe użytkowników tabeli bazy danych (która jest przestarzałe po wprowadziliśmy SqlMembershipProvider), umożliwia zamiast kodowane prawidłowe poświadczenia w ramach logowania stronie. W LoginButton kliknij program obsługi zdarzeń, Dodaj następujący kod:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample5.cs)]

Jak widać, istnieją trzy prawidłowe konta użytkownika — Scott, Jisun i Sam — i wszystkie trzy korzysta z tego samego hasła ("password"). Kod w pętli tablice użytkowników i haseł, wyszukiwanie prawidłowy dopasowanie nazwy użytkownika i hasła. Jeśli nazwa użytkownika i hasło są prawidłowe, należy zalogować użytkownika, a następnie przekierowanie do odpowiedniej strony. Jeśli poświadczenia są nieprawidłowe, możemy Wyświetl etykietę InvalidCredentialsMessage.

Gdy użytkownik wprowadzi prawidłowe poświadczenia, wymienione I następnie są przekierowywane do "odpowiedniej strony". Co to jest jednak odpowiedniej strony? Odwołaj się, że gdy użytkownik odwiedzi strony, które nie mają uprawnień do wyświetlania, FormsAuthenticationModule automatycznie przekierowany do strony logowania. W ten sposób zawiera żądanego adresu URL w zmiennej querystring za pomocą parametru ReturnUrl. Oznacza to jeśli użytkownik próbował odwiedź ProtectedPage.aspx, a nie miało uprawnień Aby to zrobić, FormsAuthenticationModule spowoduje przekierowanie do:

Login.aspx?ReturnUrl=ProtectedPage.aspx

Po pomyślnym zalogowaniu użytkownika powinien zostać przekierowany z powrotem do ProtectedPage.aspx. Alternatywnie użytkownicy mogą odwiedzać strony logowania na ich własnych volition. W takim przypadku po zalogowaniu użytkownika one powinien przesyłane do folderu głównego Default.aspx strony.

### <a name="logging-in-the-user"></a>Logowanie użytkownika

Przy założeniu, że podane poświadczenia są prawidłowe, należy utworzyć biletu uwierzytelniania formularzy, a tym samym logowania użytkownika do witryny. [Klasy FormsAuthentication](https://msdn.microsoft.com/library/system.web.security.formsauthentication.aspx) w [System.Web.Security przestrzeni nazw](https://msdn.microsoft.com/library/system.web.security.aspx) zapewnia różne metody w się i wylogowywania użytkowników za pomocą formularzy system uwierzytelniania. Istnieje kilka metod w klasie uwierzytelniania formularzy, który NAS interesuje w tym momencie trzech są:

- [GetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.getauthcookie.aspx) — tworzy biletu uwierzytelniania formularzy dla podanej nazwy *username*. Następnie ta metoda tworzy i zwraca obiekt HttpCookie, który znajduje się zawartość biletu uwierzytelniania. Jeśli *persistCookie* ma wartość true, trwały plik cookie jest tworzony.
- [SetAuthCookie (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.setauthcookie.aspx) — wywołuje GetAuthCookie (*username*, *persistCookie*) Metoda służąca do generowania pliku cookie uwierzytelniania formularzy. Ta metoda jest następnie dodaje plik cookie zwrócony przez GetAuthCookie do kolekcji plików cookie (przy założeniu, uwierzytelnianie formularzy opartych na pliki cookie są używane; w przeciwnym razie, ta metoda wywołuje Wewnętrzna klasa, obsługująca logiki biletu cookieless).
- [RedirectFromLoginPage (*username*, *persistCookie*)](https://msdn.microsoft.com/library/system.web.security.formsauthentication.redirectfromloginpage.aspx) — ta metoda wywołuje SetAuthCookie (*username*, *persistCookie*), a następnie przekierowuje użytkownika do odpowiedniej strony.

GetAuthCookie jest przydatne, gdy należy zmodyfikować biletu uwierzytelniania przed zapisaniem pliku cookie do kolekcji plików cookie. SetAuthCookie jest przydatne, jeśli chcesz utworzyć biletu uwierzytelniania formularzy i dodaj go do kolekcji plików cookie, ale nie chcesz przekierowuje użytkownika do odpowiedniej strony. Prawdopodobnie chcesz zachować je na stronie logowania lub wysyłać je do niektórych alternatywnej strony.

Ponieważ chcemy zalogować użytkownika i Przekierowanie do odpowiedniej strony, Użyjmy RedirectFromLoginPage. Zaktualizuj LoginButton kliknij program obsługi zdarzeń, zastępując dwa wiersze komentarze TODO następującego kodu:

FormsAuthentication.RedirectFromLoginPage(UserName.Text, RememberMe.Checked);

Podczas tworzenia biletu uwierzytelniania formularzy używamy właściwości Text w polu tekstowym Nazwa użytkownika dla biletu uwierzytelniania formularzy *username* parametr i stan zaznaczenia pola wyboru RememberMe dla  *persistCookie* parametru.

Aby przetestować strony logowania, odwiedź stronę w przeglądarce. Najpierw wprowadź nieprawidłowe poświadczenia, takie jak nazwa "Nope" i hasło "błąd". Po kliknięciu przycisku Zaloguj nastąpi odświeżenie strony i będzie wyświetlana etykieta InvalidCredentialsMessage.


[![Etykieta InvalidCredentialsMessage jest wyświetlany podczas wprowadzania nieprawidłowe poświadczenia](an-overview-of-forms-authentication-cs/_static/image25.png)](an-overview-of-forms-authentication-cs/_static/image24.png)

**Na rysunku nr 10**: Etykieta InvalidCredentialsMessage jest wyświetlany podczas wprowadzania nieprawidłowych poświadczeń ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image26.png))


Następnie wprowadź prawidłowe poświadczenia i kliknij przycisk Zaloguj. Teraz podczas ogłaszania zwrotnego występuje biletu uwierzytelniania formularzy jest tworzony i automatycznie przekierowany do Default.aspx. W tym momencie użytkownik zalogował się do witryny sieci Web, mimo że nie ma żadnych wizualnych wskazująca, czy użytkownik jest aktualnie zalogowany. W kroku 4, należy sprawdzić, jak programowo ustalić, czy użytkownik jest zalogowany nie oraz sposób identyfikowania użytkownika, odwiedzając stronę.

Krok 5 sprawdza techniki logowania użytkownika z poziomu witryny sieci Web.

### <a name="securing-the-login-page"></a>Zabezpieczanie strony logowania

Gdy użytkownik wprowadza swoje poświadczenia i przesłaniu formularza strony logowania, — w tym swojego hasła — poświadczenia są przesyłane za pośrednictwem Internetu do serwera sieci web w *zwykły tekst*. Oznacza to, że wszystkie haker wykrywanie ruchu sieciowego można wyświetlić nazwy użytkownika i hasła. Aby tego uniknąć, jest niezbędne do szyfrowania ruchu sieciowego za pomocą [gniazda warstwy SSL (Secure)](http://en.wikipedia.org/wiki/Secure_Sockets_Layer). Daje to pewność, są szyfrowane poświadczenia (a także kod znaczników HTML strony Cała) od momentu jego opuszczają przeglądarki, dopóki nie zostaną odebrane przez serwer sieci web.

O ile witryny sieci Web zawiera poufne informacje, wystarczy do używania protokołu SSL na stronie logowania i na innych stronach gdzie hasło użytkownika, w przeciwnym razie będą przesyłane przez sieć w postaci zwykłego tekstu. Nie trzeba martwić się o zabezpieczanie biletu uwierzytelniania formularzy, ponieważ domyślnie jest on zarówno zaszyfrowany i podpisany cyfrowo (Aby zapobiec naruszeniu). Dokładniejsze omówienie na zabezpieczenia biletu uwierzytelniania formularzy jest przedstawiona w samouczku następujące.

> [!NOTE]
> Wiele witryn sieci Web finansowych i medyczne są skonfigurowane do używania protokołu SSL na *wszystkie* stron dostępne dla użytkowników uwierzytelnionych. Jeśli tworzysz witryny sieci Web systemu uwierzytelniania formularzy można skonfigurować tak, aby biletu uwierzytelniania formularzy są jedynie przesyłane za pośrednictwem bezpiecznego połączenia. Przedstawiono różne opcje konfiguracji uwierzytelniania formularzy w następnym samouczku  *[konfiguracji uwierzytelniania formularzy i Tematy zaawansowane](forms-authentication-configuration-and-advanced-topics-cs.md)*.


## <a name="step-4-detecting-authenticated-visitors-and-determining-their-identity"></a>Krok 4: Wykrywanie odwiedzających uwierzytelniony i ich identyfikacji

Na tym etapie firma Microsoft włączono uwierzytelniania formularzy lub utworzona strona logowania proste, ale musimy jeszcze sprawdzić, jak możemy ustalić, czy użytkownik jest uwierzytelnieni lub anonimowi. W niektórych scenariuszach możemy chcesz wyświetlić informacje w zależności od tego, czy uwierzytelnieni lub anonimowi odwiedzania strony lub innych danych. Ponadto firma Microsoft często muszą znać tożsamość uwierzytelnionego użytkownika.

Ta funkcja pozwala rozszerzyć istniejącej strony Default.aspx w celu zilustrowania tych metod. W Default.aspx Dodaj dwa formanty panelu jedną o nazwie AuthenticatedMessagePanel i innym AnonymousMessagePanel nazwanego. Dodawanie formantu etykiety o nazwie WelcomeBackMessage w pierwszym panelu. W drugim panelu Dodaj formant hiperłącza, ustaw dla właściwości Text "W dzienniku" i jego właściwość NavigateUrl do "~ / Login.aspx". W tym momencie deklaratywne kod znaczników dla Default.aspx powinien wyglądać podobnie do następującego:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample6.aspx)]

Jak możesz mieć prawdopodobnie odgadnięty przez teraz, pomysł, w tym miejscu są wyświetlane tylko AuthenticatedMessagePanel odwiedzających uwierzytelniony i po prostu AnonymousMessagePanel gościom anonimowe. W tym celu należy ustawić te panele właściwości widoczne w zależności od tego, czy użytkownik jest zalogowany lub nie.

[Właściwości Request.IsAuthenticated](https://msdn.microsoft.com/library/system.web.httprequest.isauthenticated.aspx) zwraca wartość logiczną wskazującą, czy została ona uwierzytelniona żądania. Wprowadź następujący kod do strony\_załadować kod obsługi zdarzenia:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample7.cs)]

Z tego kodu w miejscu odwiedź stronę Default.aspx za pośrednictwem przeglądarki. Przy założeniu, że masz jeszcze się zalogować, zobaczysz link do strony logowania (patrz rysunek 11). Kliknięcie tego łącza i logowania do witryny. Jako widzieliśmy w kroku 3, po wprowadzeniu poświadczeń nastąpi powrót do Default.aspx, ale tym razem na stronie znajdują się "Zapraszamy ponownie!" komunikat (patrz rysunek 12).


![Kiedy odwiedzający anonimowo, w dzienniku łącze będzie wyświetlana.](an-overview-of-forms-authentication-cs/_static/image27.png)

**Rysunek 11**: podczas odwiedzania anonimowo, w dzienniku łącze jest wyświetlana


![Uwierzytelnieni użytkownicy są wyświetlane.](an-overview-of-forms-authentication-cs/_static/image28.png)

**Rysunek 12**: uwierzytelnieni użytkownicy otrzymują "Zapraszamy ponownie!" Komunikat


Można określić tożsamości aktualnie zalogowanego użytkownika za pośrednictwem [obiektu element HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.aspx)w [właściwości użytkownika](https://msdn.microsoft.com/library/system.web.httpcontext.user.aspx). Element HttpContext obiekt reprezentuje informacje o bieżącym żądaniu i jest stroną główną dla tych wspólnych obiektów ASP.NET jako odpowiedzi, żądanie i sesji, między innymi. Właściwości użytkownika reprezentuje kontekst zabezpieczeń bieżącego żądania HTTP i implementuje [interfejsu IPrincipal](https://msdn.microsoft.com/library/system.security.principal.iprincipal.aspx).

Właściwość użytkownika jest ustawiana przez FormsAuthenticationModule. W szczególności jeśli FormsAuthenticationModule znajdzie biletu uwierzytelniania formularzy w żądaniu przychodzącym, tworzy nowego obiektu GenericPrincipal i przypisuje go do właściwości użytkownika.

Obiekty główne (na przykład GenericPrincipal) zawierają informacje dotyczące tożsamości użytkowników i role, do których należą. Interfejsu IPrincipal definiuje dwa elementy członkowskie:

- [IsInRole (*roleName*)](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole.aspx) — metoda, która zwraca wartość logiczną wskazującą, czy podmiot zabezpieczeń należy do określonej roli.
- [Tożsamość](https://msdn.microsoft.com/library/system.security.principal.iprincipal.identity.aspx) — właściwość, która zwraca obiekt, który implementuje [interfejsu tożsamości](https://msdn.microsoft.com/library/system.security.principal.iidentity.aspx). Interfejs tożsamości definiuje trzy właściwości: [AuthenticationType](https://msdn.microsoft.com/library/system.security.principal.iidentity.authenticationtype.aspx), [IsAuthenticated](https://msdn.microsoft.com/library/system.security.principal.iidentity.isauthenticated.aspx), i [nazwa](https://msdn.microsoft.com/library/system.security.principal.iidentity.name.aspx).

Można określić nazwę bieżącego obiektu odwiedzającego, używając następującego kodu:

ciąg currentUsersName = User.Identity.Name;

Gdy uwierzytelnianie, za pomocą formularzy [FormsIdentity obiektu](https://msdn.microsoft.com/library/system.web.security.formsidentity.aspx) jest tworzona dla GenericPrincipal właściwość Identity. Klasa FormsIdentity zawsze zwraca ciąg "Form" wartość true dla właściwości IsAuthenticated i właściwości AuthenticationType. Właściwość Name zwraca podana nazwa użytkownika podczas tworzenia biletu uwierzytelniania formularzy. Oprócz tych trzech właściwości FormsIdentity obejmuje dostęp do podstawowych biletu uwierzytelniania za pomocą jego [biletu właściwości](https://msdn.microsoft.com/library/system.web.security.formsidentity.ticket.aspx). Właściwość biletu zwraca obiekt typu [FormsAuthenticationTicket](https://msdn.microsoft.com/library/system.web.security.formsauthenticationticket.aspx), który zawiera właściwości, takie jak wygaśnięcia, IsPersistent IssueDate, nazwy i tak dalej.

Ważne wskaż odebranie tutaj jest to, że *username* parametr określony w FormsAuthentication.GetAuthCookie (*username*, *persistCookie*), FormsAuthentication.SetAuthCookie (*username*, *persistCookie*) i FormsAuthentication.RedirectFromLoginPage (*username*, *persistCookie*) metod jest taka sama wartość zwrócona przez User.Identity.Name. Ponadto biletu uwierzytelniania utworzone za pomocą tych metod jest dostępna przez rzutowanie User.Identity obiektowi FormsIdentity i dostęp do właściwości Ticket:

[!code-csharp[Main](an-overview-of-forms-authentication-cs/samples/sample8.cs)]

Załóżmy zapewniają więcej spersonalizowane wiadomości w Default.aspx. Zaktualizuj strony\_załadować program obsługi zdarzeń, dzięki czemu właściwość Text etykiety WelcomeBackMessage przypisano ciąg "Witamy z powrotem, *username*!"

WelcomeBackMessage.Text = "Welcome back, " + User.Identity.Name + "!";

Rysunek 13 przedstawiono wpływ tej zmiany (jeśli jest zalogowanie się jako użytkownik Scott).


![Komunikat powitalny zawiera obecnie zarejestrowane w nazwie użytkownika](an-overview-of-forms-authentication-cs/_static/image29.png)

**Rysunek 13**: komunikat powitalny zawiera obecnie zarejestrowane w nazwie użytkownika


### <a name="using-the-loginview-and-loginname-controls"></a>Przy użyciu LoginView i kontrolki nazwy logowania

Wyświetlanie innej zawartości dla użytkowników uwierzytelnionych i anonimowych jest typowe wymagane; Dlatego jest wyświetlana nazwa aktualnie zalogowanego użytkownika. Z tego powodu program ASP.NET zawiera dwa formantów sieci Web, które mają te same funkcje, jak pokazano na rysunku 13, ale bez konieczności zapisu pojedynczy wiersz kodu.

[Formantu LoginView](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginview.aspx) jest oparty na szablonie formant sieci Web, który ułatwia wyświetlania różnych danych użytkownikom uwierzytelnionym i anonimowe. LoginView zawiera dwa wstępnie zdefiniowanych szablonów:

- AnonymousTemplate — żadnych znaczników dodane do tego szablonu jest wyświetlana tylko dla osób odwiedzających anonimowe.
- LoggedInTemplate — znaczników w szablonie znajduje się tylko do uwierzytelnionych użytkowników.

Do strony głównej w naszej witrynie, Site.master Dodajmy formantu LoginView. Zamiast dodawać tylko formantu LoginView, jednak Dodajmy zarówno nowy formant ContentPlaceHolder, a następnie umieść w ramach tego nowego elementu ContentPlaceHolder podczas odwzorowania formantu LoginView. Uzasadnienie tej decyzji staną się widoczne wkrótce.

> [!NOTE]
> Oprócz AnonymousTemplate i LoggedInTemplate formantu LoginView mogą obejmować szablony określonych ról. Szablony specyficzne dla roli Pokaż znaczników tylko do użytkowników należących do określonej roli. W samouczku przyszłych omówione zostanie funkcje oparte na rolach formantu LoginView.


Rozpocznij od dodania elementu ContentPlaceHolder na stronie głównej w nawigacji o nazwie LoginContent &lt;div&gt; elementu. Można po prostu przeciągnij formant ContentPlaceHolder z przybornika do widoku źródła umieszczenia znacznika wynikowy nad "TODO: Menu zostanie w tym miejscu..." tekst.

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample9.aspx)]

Następnie dodaj w obrębie elementu LoginContent ContentPlaceHolder formantu LoginView. Uwzględniane są umieszczane w formantach ContentPlaceHolder strony wzorcowej zawartości *domyślna zawartość* dla elementu ContentPlaceHolder. Oznacza to stron ASP.NET, które używają tej strony wzorcowej można określić własne zawartości dla każdego elementu ContentPlaceHolder lub korzystać z zawartości domyślnej strony wzorcowej.

LoginView i inne formanty związane z logowaniem znajdują się w karcie logowania przybornika.


![Kontrolki widoku logowania w przyborniku](an-overview-of-forms-authentication-cs/_static/image30.png)

**Rysunek 14**: formantu LoginView w przyborniku


Następnie dodaj dwie &lt;Brazylia /&gt; elementów natychmiast po formantu LoginView, ale nadal w ramach elementu ContentPlaceHolder. W tym momencie nawigacji &lt;div&gt; znacznika elementu powinna wyglądać następująco:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample10.aspx)]

Można zdefiniować szablony LoginView z projektanta lub deklaratywne znaczników. W programie Visual Studio Designer rozwiń tagów inteligentnych LoginView, który wyświetla skonfigurowane szablony na liście rozwijanej. Wpisz tekst "Hello, stranger" do AnonymousTemplate; następnie dodaj formant hiperłącza i ustawienia swoich właściwości Text i NavigateUrl "W dzienniku" i "~ / Login.aspx" odpowiednio.

Po skonfigurowaniu AnonymousTemplate, przełącz się do LoggedInTemplate i wprowadzania tekstu, "Witamy z powrotem,". Przeciągnij kontrolki nazwy logowania z przybornika do LoggedInTemplate, umieszczenia go bezpośrednio po "Witaj," tekstu. [Kontrolki nazwy logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginname.aspx), jak jego nazwa oznacza, że Wyświetla nazwę aktualnie zalogowanego użytkownika. Wewnętrznie kontrolki nazwy logowania po prostu generuje właściwości User.Identity.Name

Po wprowadzeniu tych dodatków do szablonów LoginView, znaczników powinien wyglądać podobnie do poniższej:

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample11.aspx)]

Z tym dodatkiem strony wzorcowej Site.master każdej strony w naszej witrynie sieci Web zostanie wyświetlony komunikat różne w zależności od tego, czy użytkownik jest uwierzytelniony. Rysunek 15 wyświetla stronę Default.aspx po odwiedzeniu za pośrednictwem przeglądarki przez użytkownika Jisun. "Witamy z powrotem, Jisun" komunikat jest powtarzany dwa razy: raz w sekcji nawigacji strony głównej po lewej stronie (za pomocą formantu LoginView właśnie dodaliśmy) i jeden raz w Default.aspx zawartości obszaru (za pomocą kontrolki panelu i logiki).


![Wyświetla formantu LoginView](an-overview-of-forms-authentication-cs/_static/image31.png)

**Rysunek 15**: Wyświetla formantu LoginView "Witaj ponownie Jisun."


Ponieważ dodaliśmy LoginView strony wzorcowej może występować w każdej strony w naszej witrynie. Jednak może być stron sieci web gdzie nie chcemy pokazuj tego komunikatu. Takie jedną stronę jest strony logowania, ponieważ łącze do strony logowania jest prawdopodobnie poza dostępne miejsce. Ponieważ formantu LoginView możemy umieścić w elementu ContentPlaceHolder na stronie głównej, możemy zastąpić tego znacznika domyślne w naszej strony zawartość. Otwórz Login.aspx i przejdź do projektanta. Ponieważ firma Microsoft nie został jawnie określony formant zawartości w Login.aspx dla elementu LoginContent ContentPlaceHolder na stronie głównej, strony logowania wyświetli znaczników domyślnej strony wzorcowej dla tego elementu ContentPlaceHolder. Widać to poprzez projektanta — LoginContent ContentPlaceHolder przedstawia użycie znaczników do domyślnego (formantu LoginView).


[![Strona logowania przedstawiono domyślne zawartości dla elementu LoginContent ContentPlaceHolder strony wzorcowej](an-overview-of-forms-authentication-cs/_static/image33.png)](an-overview-of-forms-authentication-cs/_static/image32.png)

**Rysunek 16**: strony logowania zawiera domyślne zawartości dla strony wzorcowej LoginContent ContentPlaceHolder ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image34.png))


Aby zastąpić domyślny kod znaczników dla elementu LoginContent ContentPlaceHolder, kliknij prawym przyciskiem myszy na region w Projektancie i wybierz opcję Utwórz zawartość niestandardowa z menu kontekstowego. (Jeśli za pomocą programu Visual Studio 2008 ContentPlaceHolder obejmuje inteligentna tagów, które, po wybraniu oferuje tę samą opcję.) Spowoduje to dodanie nowej zawartości formantu znaczników strony i tym samym pozwala zdefiniować niestandardowej zawartości dla tej strony. Można dodać niestandardowy komunikat, takie jak "Zaloguj cali...", ale umożliwia po prostu pozostawić to pole puste.

> [!NOTE]
> Tworzenie niestandardowych zawartości w programie Visual Studio 2005, tworzy pustą zawartość kontrolki na stronie platformy ASP.NET. W programie Visual Studio 2008 jednak tworzenie niestandardowych zawartości kopiuje zawartości domyślnej strony wzorcowej do nowo utworzonej formantu zawartości. Jeśli używasz programu Visual Studio 2008, następnie, po utworzeniu nowego formantu zawartości upewnij się wyczyścić zawartość kopiowane ze strony wzorcowej.


Rysunek 17 zawiera strony Login.aspx po odwiedzeniu w przeglądarce po wprowadzeniu tej zmiany. Należy pamiętać, że nie nie tekst "Hello, stranger" lub "Witamy z powrotem, *username*" wiadomości w lewym panelu nawigacyjnym &lt;div&gt; jak w przypadku odwiedzania Default.aspx.


[![Strona logowania ukrywa znaczników LoginContent ContentPlaceHolder domyślne](an-overview-of-forms-authentication-cs/_static/image36.png)](an-overview-of-forms-authentication-cs/_static/image35.png)

**Rysunek 17**: strony logowania ukrywa znaczników domyślnych LoginContent elementu ContentPlaceHolder na ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image37.png))


## <a name="step-5-logging-out"></a>Krok 5: Rejestrowanie

W kroku 3 analizujemy Tworzenie strony logowania do logowania użytkownika do witryny, ale jeszcze mamy na temat sposobu wylogować użytkownika. Oprócz metod logowania użytkownika w klasie uwierzytelniania formularzy dostępne są także [metody wylogowanie](https://msdn.microsoft.com/library/system.web.security.formsauthentication.signout.aspx). Metoda Wyloguj się po prostu niszczy biletu uwierzytelniania formularzy, w tym samym logowania użytkownika z lokacji.

Oferty wylogowania łącze jest typową funkcją ten program ASP.NET zawiera formant specjalnie zaprojektowane do wylogowania użytkownika. [Kontrolki stanu logowania](https://msdn.microsoft.com/library/system.web.ui.webcontrols.loginstatus.aspx) Wyświetla LinkButton "Logowanie" lub element LinkButton "Wylogowania", w zależności od stanu uwierzytelnienia użytkownika. LinkButton "Login" jest renderowana dla użytkowników anonimowych, natomiast element LinkButton "Wylogowania" jest wyświetlany użytkownikom uwierzytelnionym. Tekst dla "Logowanie" i "Wylogowania" LinkButtons mogą być konfigurowane przez stanu logowania LoginText i LogoutText właściwości.

Klikając przycisk "Login" łącza powoduje odświeżenie strony, z którego wystawiony przekierowanie do strony logowania. Kliknięcie przycisku LinkButton "Wylogowania" powoduje, że kontrolki stanu logowania można wywołać metody FormsAuthentication.SignOff i następnie przekierowuje użytkownika do strony. Strona zalogowanego wyłączony użytkownik zostaje przekierowany do zależy od właściwości LogoutAction, które mogą być przypisane do jednej z trzech następujących wartości:

- Odśwież — domyślnie; przekierowuje użytkownika do strony, którą właśnie zostały one odwiedzający. Jeśli strona, do której właśnie zostały one odwiedzający nie zezwalają na użytkowników anonimowych, następnie FormsAuthenticationModule automatycznie przekierowuje użytkownika do strony logowania.

Być może zastanawiasz się, aby Dlaczego przekierowanie odbywa się w tym miejscu. Jeśli użytkownik chce pozostają na tej samej stronie, dlaczego konieczności jawnego przekierowania? Dzieje się tak po kliknięciu LinkButton "Wylogowania" użytkownik nadal ma biletu uwierzytelniania formularzy w ich kolekcji plików cookie. W rezultacie odświeżania strony żądanie jest żądaniem uwierzytelniony. Kontrolki stanu logowania wywołuje metodę Wyloguj się, ale tak się stanie po FormsAuthenticationModule uwierzytelnieniu użytkownika. W związku z tym jawne przekierowania powoduje, że przeglądarkę, aby ponownie zażądać strony. W czasie przeglądarka ponownie żąda strony biletu uwierzytelniania formularzy został usunięty i w związku z tym żądania przychodzącego, jeśli jest anonimowy.

- Przekierowanie — użytkownik zostanie przekierowany adres URL określony przez właściwość LogoutPageUrl stanu logowania.
- RedirectToLoginPage — użytkownik zostanie przekierowany do strony logowania.

Umożliwia dodawanie kontrolki stanu logowania do strony głównej i skonfigurować go do użyj opcji przekierowania, aby wysłać do użytkownika do strony, która zostanie wyświetlony komunikat potwierdzenia, że ich został wyrejestrowany. Rozpocznij od utworzenia strony w katalogu głównym o nazwie Logout.aspx. Należy pamiętać skojarzyć tę stronę z Site.master strony wzorcowej. Następnie wprowadź komunikat w znaczniku strony wyjaśniający dla użytkownika, które zostały zarejestrowane wychodzących.

Następnie wróć do strony głównej Site.master i dodać kontrolki stanu logowania pod LoginView w LoginContent ContentPlaceHolder. Ustaw właściwość LogoutAction kontrolki stanu logowania przekierowania i jego właściwości LogoutPageUrl "~ / Logout.aspx".

[!code-aspx[Main](an-overview-of-forms-authentication-cs/samples/sample12.aspx)]

Ponieważ stanu logowania jest poza formantu LoginView, pojawi się on zarówno anonimowych, jak i uwierzytelnionych użytkowników, ale ponieważ stanu logowania będą poprawnie wyświetlać "Logowanie" lub "Wylogowania" LinkButton to normalne. Dodając kontrolki stanu logowania HyperLink "Dziennika w" w AnonymousTemplate jest zbędny, więc można go usunąć.

18 rysunek przedstawia Default.aspx podczas odwiedza Jisun. Należy pamiętać, że lewej kolumnie wyświetli komunikat "Witamy z powrotem, Jisun" wraz z linkiem do logowania. Klikając przycisk wylogowania LinkButton powoduje odświeżenie strony, wyloguje Jisun systemu i przekierowuje jej Logout.aspx. Jak pokazano na rysunku 19, do czasu, który Jisun osiągnie Logout.aspx ona już został wyrejestrowany i dlatego jest anonimowy. W rezultacie, kolumnie po lewej stronie zawiera tekst "Witaj, stranger" oraz link do strony logowania.


[![Pokazuje default.aspx](an-overview-of-forms-authentication-cs/_static/image39.png)](an-overview-of-forms-authentication-cs/_static/image38.png)

**Rysunek 18**: Pokazuje Default.aspx "Witamy z powrotem, Jisun" oraz LinkButton "Wylogowania" ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image40.png))


[![Pokazuje Logout.aspx](an-overview-of-forms-authentication-cs/_static/image42.png)](an-overview-of-forms-authentication-cs/_static/image41.png)

**Rysunek 19**: Pokazuje Logout.aspx "Witaj, stranger" oraz LinkButton "Login" ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](an-overview-of-forms-authentication-cs/_static/image43.png))


> [!NOTE]
> I zachęca do dostosowania strony Logout.aspx, aby ukryć element LoginContent ContentPlaceHolder strony wzorcowej (takich jak robiliśmy dla Login.aspx w kroku 4). Przyczyną jest ponieważ LinkButton "Login" renderowana przez kontrolki stanu logowania (jeden poniżej "tekst Hello, stranger") wysyła użytkownika do strony logowania, przekazując bieżący adres URL ReturnUrl parametr querystring. Krótko mówiąc Jeśli użytkownik zalogował się kliknie tego stanu logowania LinkButton "Login", a następnie dzienniki w, zostanie przekierowany do Logout.aspx, który można łatwo mylić użytkownika.


## <a name="summary"></a>Podsumowanie

W tym samouczku będziemy wprowadzenie do badania przepływów pracy uwierzytelniania formularzy i następnie włączona do wykonania uwierzytelniania formularzy w aplikacji ASP.NET. Uwierzytelnianie formularzy jest obsługiwany przez FormsAuthenticationModule, która ma dwa obowiązki: identyfikowania użytkowników w oparciu o ich biletu uwierzytelniania formularzy i Przekierowanie nieautoryzowanych użytkowników do strony logowania.

.NET Framework FormsAuthentication klasa zawiera metody tworzenia, kontrolę i usuwanie biletów uwierzytelniania formularzy. Właściwość Request.IsAuthenticated i obiektu użytkownika Podaj dodatkowego wsparcia programowego do określenia, czy żądanie jest uwierzytelniana, a także informacje o tożsamości użytkownika. Dostępne są także formanty LoginView stanu logowania i nazwy logowania w sieci Web, dzięki czemu deweloperzy sposób szybki i niekorzystające z kodu do wykonywania wielu typowe zadania związane z logowaniem. Zostaną omówione tych i innych formantów sieci Web związane z logowaniem bardziej szczegółowo w przyszłości samouczki.

W tym samouczku podać pobieżną omówienie uwierzytelniania formularzy. Firma Microsoft nie Sprawdź opcje konfiguracji wybrane, przyjrzeć się jak cookieless działania biletów uwierzytelniania formularzy lub Eksploruj jak ASP.NET chroni zawartość biletu uwierzytelniania formularzy. Omówimy tych tematów i więcej informacji, zobacz [następny samouczek](forms-authentication-configuration-and-advanced-topics-cs.md).

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Zmiany między IIS6 i zabezpieczeń IIS7](https://www.iis.net/articles/view.aspx/IIS7/Managing-IIS7/Configuring-Security/Changes-between-IIS6-and-IIS7-Security)
- [Kontrolek logowania programu ASP.NET](https://msdn.microsoft.com/library/d51ttbhx.aspx)
- [Professional ASP.NET 2.0 zabezpieczeń, członkostwo i zarządzanie rolami](http://www.wrox.com/WileyCDA/WroxTitle/productCd-0764596985.html) (ISBN: 978-0-7645-9698-8)
- [`<authentication>` — Element](https://msdn.microsoft.com/library/532aee0e.aspx)
- [`<forms>` Elementu `<authentication>`](https://msdn.microsoft.com/library/1d3t3c61.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenie wideo na tematy zawarte w tym samouczku

- [Korzystanie z uwierzytelniania podstawowego formularzy na platformie ASP.NET](../../../videos/authentication/using-basic-forms-authentication-in-aspnet.md)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla...

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Recenzenta realizacji w tym samouczku został w tym samouczku, który serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku obejmują Alicja Maziarz, Jan Suru i Teresa Murphy. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Poprzednie](security-basics-and-asp-net-support-cs.md)
> [dalej](forms-authentication-configuration-and-advanced-topics-cs.md)
