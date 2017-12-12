---
uid: whitepapers/mvc4-beta-release-notes
title: PLATFORMA ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: rick-anderson
description: Ten dokument zawiera opis wersji platformy ASP.NET MVC 4 w wersji Beta programu Visual Studio 2010.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 4af2df61ab4507b1f100d6bb75777da1168c5a75
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Ten dokument zawiera opis wersji platformy ASP.NET MVC 4 w wersji Beta programu Visual Studio 2010.
> 
> > [!NOTE]
> > To nie jest najnowsza wersja. Dostępne są informacje o wersji platformy ASP.NET MVC 4 RC [tutaj](mvc4-release-notes.md).


- [Informacje o instalacji](#_Toc303253802)
- [Dokumentacja](#_Toc303253803)
- [Obsługa](#_Toc303253804)
- [Wymagania dotyczące oprogramowania](#_Toc303253805)
- [Uaktualnianie projektów programu ASP.NET MVC 3 do platformy ASP.NET MVC 4](#_Toc303253806)
- [Nowe funkcje w wersji Beta programu ASP.NET MVC 4](#_Toc303253807)

    - [Interfejs API sieci Web ASP.NET](#_Toc317096197)
    - [ASP.NET pojedynczej strony aplikacji](#_Toc317096198)
    - [Ulepszenia domyślnych szablonów projektu](#_Toc303253808)
    - [Szablon projektu przenośnych](#_Toc303253809)
    - [Wyświetlanie trybów](#_Toc303253810)
    - [jQuery Mobile, tym przełącznikiem widoku i zastępowanie przeglądarki](#_Toc303253811)
    - [Przepisy dotyczące generowania kodu w programie Visual Studio](#_Toc303253812)
    - [Obsługa zadanie asynchroniczne kontrolerów](#_Toc303253813)
    - [Zestaw Azure SDK](#_Toc303253814)
    - [Znane problemy i fundamentalne zmiany](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Informacje o instalacji

ASP.NET MVC 4 w wersji Beta programu Visual Studio 2010 można zainstalować z [strona główna programu ASP.NET MVC 4](../mvc/mvc4.md) za pomocą Instalatora platformy sieci Web.

Należy odinstalować wszelkie zainstalowane wcześniej podglądy ASP.NET MVC 4, przed zainstalowaniem programu ASP.NET MVC 4 w wersji Beta.

Ta wersja nie jest zgodny z .NET Framework 4.5 Developer Preview. Należy odinstalować .NET 4.5 Developer Preview, przed zainstalowaniem wersji Beta programu ASP.NET MVC 4.

ASP.NET MVC 4 można zainstalować i uruchomić side-by-side z platformy ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentacja

Dokumentacja dla platformy ASP.NET MVC jest dostępna w witrynie MSDN pod adresem URL:

[https://go.microsoft.com/fwlink/?LinkId=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Samouczki i inne informacje o platformie ASP.NET MVC są dostępne na stronie witryny sieci Web platformy ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Obsługa

To jest wersja preview i nie jest oficjalnie obsługiwana. Jeśli masz pytania dotyczące pracy z tej wersji zamieścić je na platformie ASP.NET MVC forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), gdzie są często można zapewnić obsługę nieformalne członkami społeczności programu ASP.NET.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Wymagania programowe

Składniki platformy ASP.NET MVC 4 dla programu Visual Studio wymaga środowiska PowerShell 2.0, a program Visual Studio 2010 z dodatkiem Service Pack 1 lub Visual Web Developer Express 2010 z dodatkiem Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Uaktualnianie projektów programu ASP.NET MVC 3 do platformy ASP.NET MVC 4

ASP.NET MVC 4 można zainstalować równolegle program ASP.NET MVC 3 na tym samym komputerze, który zapewnia elastyczność w wyborze, gdy do uaktualniania aplikacji ASP.NET MVC 3 do platformy ASP.NET MVC 4.

Najprostszym sposobem uaktualnienia jest aby utworzyć nowy projekt ASP.NET MVC 4 i skopiuj wszystkie widoki, kontrolery, kodu i zawartość plików z istniejącego projektu MVC 3 do nowego projektu i zaktualizować zestaw odwołuje się w nowym projekcie odpowiadające stary projekt. Jeśli wprowadzono zmiany w pliku Web.config w projekcie MVC 3, musi również scalić te zmiany w pliku Web.config w projekcie MVC 4.

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 3 do wersji 4, wykonaj następujące czynności:

1. We wszystkich plikach Web.config w projekcie (istnieje w folderze głównym projektu, w folderze Widoki i w folderze widoków dla każdego obszaru projektu) Zastąp każde wystąpienie następującego tekstu:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    z odpowiedniego następujący tekst:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. W głównym pliku Web.config, zaktualizuj *webPages:Version* elementu "2.0.0.0" i Dodaj nową *PreserveLoginUrl* klucz, który ma wartość "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. W Eksploratorze rozwiązań, Usuń odwołanie do *System.Web.Mvc* (wskazujących wersja 3 biblioteki DLL). Następnie dodaj odwołanie do *System.Web.Mvc* (v4.0.0.0). W szczególności należy wprowadzić następujące zmiany można zaktualizować odwołania do zestawów. Oto szczegóły:

    1. W Eksploratorze rozwiązań Usuń odwołania do następujących zestawów: 

        - *System.Web.Mvc*(v3.0.0.0)
        - *System.Web.WebPages*(v1.0.0.0)
        - *System.Web.Razor*(v1.0.0.0)
        - *System.Web.WebPages.Deployment*(v1.0.0.0)
        - *System.Web.WebPages.Razor*(v1.0.0.0)
    2. Dodaj odwołania do następujących zestawów: 

        - *System.Web.Mvc*(v4.0.0.0)
        - *System.Web.WebPages*(v2.0.0.0)
        - *System.Web.Razor*(v2.0.0.0)
        - *System.Web.WebPages.Deployment*(v2.0.0.0)
        - *System.Web.WebPages.Razor*(v2.0.0.0)
4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz polecenie Edytuj *ProjectName*.csproj.
5. Zlokalizuj *ProjectTypeGuids* elementu i zastępowanie {E53F8FEA-EAE0-44A6-8774-FFD645390401} z {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Zapisz zmiany, zamknij plik projektu (.csproj), który edytowania, kliknij prawym przyciskiem myszy projekt, a następnie wybierz ponowne załadowanie projektu.
7. Jeśli projekt odwołuje się do żadnych bibliotek innych firm, które są kompilowane przy użyciu poprzedniej wersji platformy ASP.NET MVC, otwórz głównego pliku Web.config i dodaj następujące trzy *bindingRedirect* elementy w obszarze  *Konfiguracja* sekcji: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nowe funkcje w wersji Beta programu ASP.NET MVC 4

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji Beta programu ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 zawiera teraz interfejsu API sieci Web platformy ASP.NET, nowe framework umożliwiający tworzenie usług HTTP, która może korzystać szeroka gama klientów, w tym przeglądarki i urządzenia przenośne. ASP.NET Web API również jest idealną platformą do tworzenia usługi RESTful.

Interfejs API sieci Web platformy ASP.NET obsługuje następujące funkcje:

- **Nowoczesne model programowania protokołu HTTP:** bezpośrednio uzyskać dostęp i manipulowania żądań i odpowiedzi w interfejsów API sieci Web przy użyciu nowego, jednoznacznie modelu obiektu HTTP HTTP. Taki sam programowania modelu i potoku HTTP jest symetrycznie dostępny na kliencie przy użyciu nowego typu HttpClient.
- **Pełna obsługa tras**: interfejsów API sieci Web obsługuje teraz pełnego zestawu możliwości trasy, które były zawsze część stos sieci Web, w tym parametrów trasy i ograniczenia. Ponadto mapowanie do akcji ma pełną obsługę konwencje, więc nie trzeba zastosować atrybutów, takich jak [HttpPost] do klasy i metody.
- **Negocjowanie zawartości**: klient i serwer mogą współdziałać, aby ustalić nieprawidłowy format danych zwracanych z interfejsu API. Firma Microsoft udostępnia domyślną obsługę zakodowane w adresie URL formularza formatów XML, JSON i i można rozszerzyć tę obsługę, dodając własne elementy formatujące lub nawet jej zastąpienie domyślnego strategii negocjacji zawartości.
- **Model powiązania i sprawdzania poprawności:** integratorów modeli z łatwością wyodrębniania danych z różnych części żądania HTTP i konwertowania tych części wiadomości na obiekty .NET, które mogą być używane przez akcje interfejsu API sieci Web.
- **Filtry:** interfejsów API sieci Web obsługuje teraz filtry, w tym dobrze znanego filtrów, takich jak atrybutu [Authorize]. Możesz tworzyć i dołączyć filtry dla działania, autoryzacji i obsługi wyjątków.
- **Tworzenia zapytania:** po prostu zwracając IQueryable&lt;T&gt;, interfejs API sieci Web będzie obsługiwać zapytań za pośrednictwem z konwencjami adresu URL OData.
- **Ulepszone pola szczegóły HTTP:** zamiast ustawienie szczegóły HTTP w obiektach kontekstu statycznego, akcji interfejsu API sieci Web teraz pracować z wystąpień HttpRequestMessage i HttpResponseMessage. Ogólny wersje tych obiektów istnieje umożliwić pracę z Twojej niestandardowe typy oprócz typów protokołu HTTP.
- **Ulepszone Inwersja kontroli (IoC) za pomocą klasy DependencyResolver:** interfejsu API sieci Web teraz korzysta ze wzorca lokalizatora usługi implementowane przez mechanizm rozpoznawania zależności MVC do uzyskania wystąpienia dla wielu różnych urządzeń.
- **Konfiguracja na podstawie kodu:** konfigurację składnika Web API odbywa się wyłącznie przy użyciu kodu, pozostawiając konfigurację czyszczenia plików.
- **Host samodzielny:** interfejsów API sieci Web może być hostowana w procesie oprócz usług IIS podczas nadal przy użyciu pełną moc tras i inne funkcje interfejsu API sieci Web.

Więcej informacji na temat interfejsu API sieci Web platformy ASP.NET można znaleźć pod adresem [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET pojedynczej strony aplikacji

ASP.NET MVC 4 zawiera teraz wcześniejszy przegląd środowiska do tworzenia aplikacji jednej strony z znaczących interakcji po stronie klienta przy użyciu języka JavaScript i interfejsów API sieci Web. Ta obsługa obejmuje:

- Zestaw bibliotek JavaScript bardziej zaawansowane funkcje interakcji lokalnej pamięci podręcznej danych
- Dodatkowe składniki interfejsu API sieci Web dla jednostki pracy i obsługa warstwy DAL
- Szablon projektu MVC z rusztowania, aby szybko rozpocząć pracę

Więcej informacji na temat aplikacji jednej strony obsługuje platformie ASP.NET MVC 4, odwiedź [https://www.asp.net/single-page-application](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Ulepszenia domyślnych szablonów projektu

Szablon, który służy do tworzenia nowych projektów programu ASP.NET MVC 4 została zaktualizowana w celu utworzenia bardziej nowoczesny wyglądające witryny sieci Web:

![](mvc4-beta-release-notes/_static/image1.png)

Oprócz bardzo drobny ulepszenia ulepszono funkcje w nowym szablonie. Szablon zostają technika o nazwie adaptacyjną renderowania wyglądać zarówno w przypadku przeglądarek klasycznych, jak i przeglądarki dla urządzeń przenośnych bez potrzeby dostosowywania.

![](mvc4-beta-release-notes/_static/image2.png)

Aby wyświetlić adaptacyjną renderowania w akcji, można użyć emulatorze przenośnym, lub podaj zmiany rozmiaru okna przeglądarki pulpitu na mniejszy. Gdy duże pobiera okna przeglądarki, zmieni się układ strony.

Rozszerzenie innego domyślny szablon projektu jest użycie JavaScript, aby zapewnić bardziej zaawansowane funkcje interfejsu użytkownika. Łącza logowania i rejestrowanie, które są używane w szablonie przedstawiono sposób użycia jQuery okno dialogowe interfejsu użytkownika do przedstawienia ekran logowania sformatowanego:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Szablon projektu przenośnych

Jeśli zaczynasz nowego projektu i chcesz utworzyć witryny specjalnie dla urządzeń przenośnych i tablet przeglądarki, można użyć nowego szablonu projektu aplikacji mobilnej. To jest oparty na technologii jQuery Mobile, biblioteka open source do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika:

![](mvc4-beta-release-notes/_static/image4.png)

Ten szablon zawiera tę samą strukturę aplikacji jako szablonu aplikacji internetowej (i kodu kontrolera jest praktycznie identyczny), ale jest stylem wygląd i działają prawidłowo na podstawie touch urządzeniach przenośnych przy użyciu jQuery Mobile. Aby dowiedzieć się więcej na temat struktury i stylem przenośnych interfejsu użytkownika, zobacz [jQuery przenośnych projektu witryny sieci Web](http://jquerymobile.com/).

Jeśli masz już zorientowane na pulpicie lokacji, czy chcesz dodać widoków zoptymalizowanych pod kątem mobile, lub jeśli chcesz utworzyć jedną lokację służy inaczej styl widoków do komputerów stacjonarnych i przenośnych przeglądarek, można użyć nowej funkcji trybów wyświetlania. (Zobacz następną sekcję).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Wyświetlanie trybów

Nowa funkcja trybów wyświetlania umożliwia aplikacji wybierz widoki, w zależności od przeglądarki, do której wysłano żądanie. Na przykład jeśli przeglądarka pulpitu zażąda strony głównej, aplikacja może szablon Views\Home\Index.cshtml. Jeśli przeglądarkę dla telefonów zażąda strony głównej, aplikacja może zwrócić Views\Home\Index.mobile.cshtml szablonu.

Układy i częściowe może także być zmieniona dla typów przeglądarki. Na przykład:

- Jeśli Views\Shared folder zawiera zarówno \_Layout.cshtml i \_Layout.mobile.cshtml szablony, domyślnie aplikacja będzie używać \_Layout.mobile.cshtml podczas żądania od przeglądarki dla urządzeń przenośnych i \_Layout.cshtml podczas innych żądań.
- Jeśli folder zawiera zarówno \_MyPartial.cshtml i \_MyPartial.mobile.cshtml, instrukcja @Html.Partial("\_MyPartial") będzie renderować \_MyPartial.mobile.cshtml podczas żądania od mobile przeglądarki, a \_MyPartial.cshtml podczas innych żądań.

Jeśli chcesz utworzyć bardziej szczegółowych widoków, układów lub widoki częściowe przez inne urządzenia, możesz zarejestrować nową *DefaultDisplayMode* wystąpienia, aby określić, którego nazwa do wyszukiwania, gdy żądanie spełnia warunki określonej. Można na przykład, Dodaj następujący kod, aby *aplikacji\_Start* metody w pliku Global.asax, aby zarejestrować ciąg "iPhone" jako tryb wyświetlania, która ma zastosowanie, gdy przeglądarka Apple iPhone wysyła żądanie:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Po uruchomieniu tego kodu, jeśli Apple iPhone przeglądarki wysyła żądanie, aplikacja będzie korzystać z Views\Shared\\układu _Layout.iPhone.cshtml (jeśli istnieje).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, tym przełącznikiem widoku i zastępowanie przeglądarki

jQuery Mobile to biblioteki typu open source do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web. Jeśli chcesz użyć jQuery Mobile za pomocą aplikacji ASP.NET MVC 4, można pobrać i zainstalować pakietu NuGet, który ułatwia rozpoczęcie pracy. Aby zainstalować go z konsoli Menedżera pakietów programu Visual Studio, wpisz następujące polecenie:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Spowoduje to zainstalowanie jQuery Mobile, a niektóre pliki pomocnicze, takie jak następujące:

- Widoki/Shared/\_Layout.Mobile.cshtml, który jest układ jQuery Mobile.
- Składnik tym przełącznikiem widoku, który składa się z widoków i/lubudostępnionych/\_ViewSwitcher.cshtml widok częściowy i ViewSwitcherController.cs kontrolera.

Po zainstalowaniu pakietu, uruchom aplikację za pomocą przeglądarki przenośne (lub równoważnego, takich jak program Firefox [przełącznik agenta użytkownika](http://chrispederick.com/work/user-agent-switcher/) dodatkami). Wyświetlenie strony wyszukiwania zupełnie różne, ponieważ jQuery Mobile obsługuje układu i stylów. Aby móc korzystać z tego, czy są:

- Utwórz wartości zastąpień mobile określonego widoku zgodnie z opisem w [trybów wyświetlania](#_Toc303253810) wcześniej (na przykład utworzyć Views\Home\Index.mobile.cshtml do zastąpienia Views\Home\Index.cshtml przeglądarki dla urządzeń przenośnych).
- Odczyt [jQuery przenośnych dokumentacji](http://jquerymobile.com/) Aby dowiedzieć się więcej o sposobie dodawania zoptymalizowanych pod kątem touch elementy interfejsu użytkownika w widokach dla urządzeń przenośnych.

Konwencja dla stron sieci web zoptymalizowanych pod kątem mobile jest dodać łącze, którego tekst jest coś jak widok pulpitu lub tryb witrynę w trybie pełnym, który umożliwia użytkownikom pulpitu wersję strony. Pakiet jQuery.Mobile.MVC zawiera przykładowy składnik tym przełącznikiem widoku w tym celu. Jest on używany w domyślnym Views\Shared\\widoku _Layout.Mobile.cshtml i podczas renderowania strony wygląda następująco:

![](mvc4-beta-release-notes/_static/image5.png)

Jeśli odwiedzający kliknij łącze, jest zostały przełączone do wersji dla komputerów z tej samej stronie.

Ponieważ układ pulpitu nie będzie zawierać przełącznik widoku domyślnie, gości nie będziesz mieć sposobem uzyskania przenośnych tryb. Aby włączyć tę opcję, należy dodać następujące odwołanie do  *\_ViewSwitcher* do układu pulpitu, po prostu wewnątrz  *&lt;treści&gt;*  elementu:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Nową funkcję o nazwie zastępowania przeglądarki korzysta z tym przełącznikiem widoku. Ta funkcja umożliwia aplikacji taką obsługę żądań, jak gdyby pochodziły one z innej przeglądarki (agenta użytkownika) niż są rzeczywiście z. W poniższej tabeli wymieniono metody udostępniające zastępowania przeglądarki.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Przesłania żądania rzeczywistą wartość agenta użytkownika przy użyciu określonego agenta użytkownika. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Zwraca wartość zastąpienia agenta użytkownika żądania lub rzeczywisty ciąg agenta użytkownika, jeśli żadne przesłonięcie nie zostało określone. |
| `HttpContext.GetOverriddenBrowser()` | Zwraca *HttpBrowserCapabilitiesBase* wystąpienie, które odpowiada agenta użytkownika aktualnie ustawione na żądanie (rzeczywista lub przesłonięte). Możesz użyć tej wartości można pobrać właściwości, takie jak *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Usuwa wszelkich przesłoniętych agentów użytkownika dla bieżącego żądania. |

Przesłanianie przeglądarki podstawowych funkcji programu ASP.NET MVC 4 i jest dostępny, nawet jeśli nie jest instalowany pakiet jQuery.Mobile.MVC. Jednak wpływa na widok, układu i wybór widoku częściowego — nie ma wpływu na inne funkcję ASP.NET, która jest zależna od *Request.Browser* obiektu.

Domyślnie zastąpienie agenta użytkownika jest przechowywany przy użyciu pliku cookie. Jeśli chcesz przechowywać zastąpienia w innym miejscu (na przykład w bazie danych), możesz zastąpić domyślny dostawca (*BrowserOverrideStores.Current*). Dokumentację dla tego dostawcy można będzie towarzyszyć nowszej wersji platformy ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Przepisy dotyczące generowania kodu w programie Visual Studio

Nowa funkcja przepisami umożliwia Visual Studio do generowania kodu określonego rozwiązania opartego na pakiety, które można zainstalować przy użyciu narzędzia NuGet. Framework przepisami ułatwia deweloperom pisanie dodatków plug-in generowania kodu, który umożliwia także zastąpić generatory kodu wbudowanego Dodaj obszar i Dodaj kontroler, Dodaj widok. Ponieważ przepisami są wdrażane jako pakietów NuGet, można łatwo je wyewidencjonowany do kontroli źródła i automatycznie udostępniane wszystkich deweloperów w projekcie. Są one również dostępne w zasadach na rozwiązanie.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Obsługa zadanie asynchroniczne kontrolerów

Możesz teraz zapisać metod asynchronicznych akcji w jednej metody, które zwracają obiekt typu *zadań* lub *zadań&lt;ActionResult&gt;*.

Na przykład, jeśli używasz programu Visual C# 5 (lub przy użyciu [Async CTP](https://msdn.microsoft.com/en-us/vstudio/async.aspx)), można utworzyć asynchronicznej metody akcji, która wygląda podobnie do następującej:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

W metodzie akcji z poprzedniego wywołania metody *newsService.GetHeadlinesAsync* i *sportsService.GetScoresAsync* są wywoływane asynchronicznie, a nie blokują wątków z puli wątków.

Metody asynchroniczne akcji, które zwracają *zadań* wystąpień może również obsługiwać przekroczeń limitu czasu. Aby metodzie akcji, możliwe, należy dodać parametr typu *CancellationToken* w podpisie metody akcji. W poniższym przykładzie przedstawiono asynchronicznej metody akcji z limitu czasu w milisekundach czas oczekiwania 2500, który wyświetla *upłynął limit czasu* wyświetlić do klienta, jeśli zostanie przekroczony limit czasu.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Zestaw Azure SDK

ASP.NET MVC 4 w wersji Beta obsługuje wrzesień 2011 1.5 wersji zestawu SDK platformy Windows Azure.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

- **Po zainstalowaniu programu ASP.NET MVC 4 w wersji Beta, Edytor CSHTML/VBHTML w edytorze programu Visual Studio 2010 Service Pack 1 CSHTML/VBHTML może wstrzymać przez długi czas, po wpisaniu fragment lub JavaScript wewnątrz cshtml lub vbhtml plików.** Dzieje się tak tylko w aplikacjach ASP.NET MVC 4, które właśnie utworzony i nie został skompilowany.

    Obejście jest skompilować projekt, aby uzyskać zestawów w folderze bin. Uwaga: Jeśli wyczyścić projektu, co spowoduje usunięcie zestawy z folderu bin, problem Edytor będzie wrócić.

    Spowoduje to naprawić, w następnej wersji.
- **Szablony projektów C# dla programu Visual Studio 11 Beta zawiera niepoprawne parametry w Global.asax.cs.** Domyślne połączenie, określona w aplikacji\_Start — metoda dla projektów utworzonych w programie Visual Studio 11 Beta zawierać ciąg połączenia bazy danych LocalDB, który zawiera niezmienionym znaczeniu ukośnik odwrotny (\) znaków. Powoduje to błąd połączenia podczas próby uzyskania dostępu Entity Framework DbContext, który generuje SqlException.

    Aby rozwiązać ten problem, należy wprowadzić ukośnika w aplikacji\_uruchomienia metody Global.asax.cs, tak aby o następującej treści:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Aplikacji ASP.NET MVC 4, które docelowy .NET 4.5 zgłosi fileloadexception — podczas próby dostępu zestawu System.Net.Http.dll, gdy uruchamiana .NET 4.0.** Aplikacje programu ASP.NET MVC 4 utworzone na podstawie .NET 4.5 zawierają powiązanie przekierowania, który spowoduje fileloadexception —, które stwierdza, że "nie można załadować pliku lub zestawu 'System.Net.Http' lub jednej z jego zależności." gdy aplikacja jest wykonywany w systemie z zainstalowaną .NET 4.0. Aby rozwiązać ten problem, należy usunąć następujące przekierowanie powiązania z pliku web.config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    Element powiązania zestawu w pliku web.config zmodyfikowane powinna wyglądać następująco:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- **Szablon elementu "Dodaj kontroler" w projektach Visual Basic generuje niepoprawne przestrzeni nazw, gdy została wywołana *** z wewnątrz obszaru.** Po dodaniu kontrolera do obszaru w projekcie platformy ASP.NET MVC, który używa języka Visual Basic szablon elementu wstawia nieprawidłowy obszar nazw do kontrolera. Wynikiem jest błąd "nie można odnaleźć pliku", po przejściu do dowolnych akcji w kontrolerze.  
  
 Wygenerowany obszar nazw pomija wszystkie elementy po głównej przestrzeni nazw. Na przykład, przestrzeń nazw, generowane jest *RootNamespace* , ale powinien być *RootNamespace.Areas.AreaName.Controllers* .
- **Fundamentalne zmiany w aparatu widoku Razor.** W ramach poprawione analizator Razor, następujące typy zostały usunięte z *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 Ponadto usunięto następujących metod: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Gdy WebMatrix.WebData.dll jest uwzględniony w w katalogu/bin aplikacji ASP.NET MVC 4, przejmuje adres URL uwierzytelniania formularzy.** Dodawanie zestawu WebMatrix.WebData.dll do swojej aplikacji (na przykład, wybierając "ASP.NET Web Pages o składni Razor" przy użyciu okna dialogowego Dodaj zależności do wdrożenia) zastąpią przekierowania logowania uwierzytelniania do logowania/account/zamiast / / logowanie się na koncie zgodnie z oczekiwaniami domyślnie ASP.NET MVC konta kontrolera. Aby temu zapobiec i użyć adres URL już określony w sekcji uwierzytelniania w pliku Web.config, możesz dodać appSetting o nazwie PreserveLoginUrl i ustaw ją na wartość true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Menedżer pakietów NuGet nie zostanie zainstalowany podczas próby zainstalowania programu ASP.NET MVC 4 dla siebie instalacji programu Visual Studio 2010 oraz Visual Web Developer 2010.** Do uruchomienia programu Visual Studio 2010 i Visual Web Developer 2010 równolegle z platformy ASP.NET MVC 4 należy zainstalować program ASP.NET MVC 4 po obie wersje programu Visual Studio zostały już zainstalowane.
- **Odinstalowywanie platformy ASP.NET MVC 4 kończy się niepowodzeniem, jeśli wymagania wstępne zostały już odinstalowane.** Aby odinstalować prawidłowo ASP.NET MVC 4you odinstalować ASP.NET MVC 4 przed odinstalowaniem programu Visual Studio.
- **Uruchamianie domyślny projekt interfejsu API sieci Web zawiera instrukcje dotyczące niepoprawnie przekierować użytkownika można dodać tras za pomocą metody RegisterApis, który nie istnieje.** W metodzie RegisterRoutes przy użyciu tabeli tras platformy ASP.NET, należy dodać trasy.
- **Instalowanie platformy ASP.NET MVC 4 w wersji Beta dzieli RTM programu ASP.NET MVC 3 aplikacji.** Aplikacje programu ASP.NET MVC 3, które zostały utworzone z RTM zlecenia (nie w wersji aktualizacji narzędzi programu ASP.NET MVC 3) wymagają następujących zmian do side-by-side z platformy ASP.NET MVC 4 w wersji Beta. Tworzenie projektu bez wprowadzania tych wyników aktualizacje w błędy kompilacji. 

    **Wymagane aktualizacje**

    1. W głównym pliku Web.config, Dodaj nową  *&lt;appSettings&gt;*  wpisu z kluczem *webPages:Version* i wartość *1.0.0.0*.

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
    2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz polecenie Edytuj *ProjectName*.csproj.
    3. Znajdź następujące odwołania do zestawu: 

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

        Zastąp je z następujących czynności:

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
    4. Zapisz zmiany, zamknij plik projektu (.csproj), edytowania, a następnie kliknij prawym przyciskiem myszy projekt i wybierz pozycję Załaduj ponownie.
