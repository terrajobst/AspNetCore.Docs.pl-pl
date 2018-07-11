---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Ten dokument opisuje wersję platformy ASP.NET MVC 4 w wersji Beta programu Visual Studio 2010.
ms.author: aspnetcontent
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: b9d50114a239b67b1adc263f6ea6d3a811bcc8f7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816694"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Ten dokument opisuje wersję platformy ASP.NET MVC 4 w wersji Beta programu Visual Studio 2010.
> 
> > [!NOTE]
> > Nie jest najnowsza wersja. Dostępne są informacje o wersji platformy ASP.NET MVC 4 RC [tutaj](mvc4-release-notes.md).


- [Uwagi dotyczące instalacji](#_Toc303253802)
- [Dokumentacja](#_Toc303253803)
- [Obsługa](#_Toc303253804)
- [Wymagania dotyczące oprogramowania](#_Toc303253805)
- [Uaktualnianie projektu ASP.NET MVC 3 do ASP.NET MVC 4](#_Toc303253806)
- [Nowe funkcje w wersji Beta programu ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [ASP.NET pojedynczej strony aplikacji](#_Toc317096198)
    - [Ulepszenia domyślne szablony projektów](#_Toc303253808)
    - [Szablon projektu przenośnych](#_Toc303253809)
    - [Tryby wyświetlania](#_Toc303253810)
    - [jQuery Mobile, przełącznikiem widoku i zastępowanie przeglądarki](#_Toc303253811)
    - [Rozwiązania na potrzeby generowania kodu w programie Visual Studio](#_Toc303253812)
    - [Zadanie obsługi asynchronicznego kontrolerów](#_Toc303253813)
    - [Zestaw Azure SDK](#_Toc303253814)
    - [Znane problemy i fundamentalne zmiany](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

ASP.NET MVC 4 w wersji Beta dla programu Visual Studio 2010 można zainstalować ze [strony głównej platformy ASP.NET MVC 4](../mvc/mvc4.md) za pomocą Instalatora platformy sieci Web.

Należy odinstalować wszelkie zainstalowanych wcześniej wersji zapoznawczych programu ASP.NET MVC 4, przed zainstalowaniem programu ASP.NET MVC 4 w wersji Beta.

Ta wersja nie jest zgodny z .NET Framework 4.5 Developer Preview. Przed zainstalowaniem programu ASP.NET MVC 4 w wersji Beta, należy odinstalować .NET 4.5 Developer Preview.

ASP.NET MVC 4 można zainstalować i uruchomić side-by-side przy użyciu platformy ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentacja

Dokumentacja dla platformy ASP.NET MVC jest dostępna w witrynie MSDN pod adresem URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

W samouczkach i innych informacji na temat platformy ASP.NET MVC są dostępne na stronie witryny sieci Web platformy ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Obsługa

To jest wersja zapoznawcza i nie jest oficjalnie obsługiwana. Jeśli masz pytania na temat pracy z tej wersji, opublikuj je na forum platformy ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), gdzie są często w stanie zapewnić obsługę nieformalne członków społeczności platformy ASP.NET.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Wymagania programowe

Składniki platformy ASP.NET MVC 4 dla programu Visual Studio wymagają programu PowerShell w wersji 2.0, a program Visual Studio 2010 z dodatkiem Service Pack 1 lub Visual Web Developer Express 2010 z dodatkiem Service Pack 1.

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Uaktualnianie projektu ASP.NET MVC 3 do ASP.NET MVC 4

ASP.NET MVC 4 można zainstalować równolegle z programem ASP.NET MVC 3 na tym samym komputerze, co daje elastyczności w wyborze, kiedy należy uaktualnić aplikację ASP.NET MVC 3 do ASP.NET MVC 4.

Najprostszym sposobem uaktualnienia jest, aby utworzyć nowy projekt ASP.NET MVC 4 i skopiuj wszystkie widoki, kontrolery, kodu i zawartość plików z istniejącego projektu MVC 3 do nowego projektu i można zaktualizować zestawu odwołuje się w nowym projekcie, aby dopasować starego projektu. Jeśli zmiany zostały wprowadzone do pliku Web.config w projekcie MVC 3, można również scalić te zmiany w pliku Web.config w projekcie MVC 4.

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 3 w wersji 4, wykonaj następujące czynności:

1. We wszystkich plikach Web.config w projekcie (istnieje w katalogu głównym projektu: jeden w folderze Widoki i jeden w folderze widoków dla każdego obszaru w projekcie) Zastąp każde wystąpienie następującego tekstu:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    za pomocą odpowiedniego następujący tekst:

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. W głównym pliku Web.config, zaktualizuj *webPages:Version* elementu "2.0.0.0" i Dodaj nowy *PreserveLoginUrl* klucza, który ma wartość "true":

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. W Eksploratorze rozwiązań, usunięcie odwołania do *System.Web.Mvc* (które wskazuje do wersji 3 biblioteki DLL). Następnie dodaj odwołanie do *System.Web.Mvc* (v4.0.0.0). W szczególności należy wprowadzić następujące zmiany można zaktualizować odwołania do zestawu. Oto szczegóły:

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
4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz pozycję Edytuj *ProjectName*csproj.
5. Znajdź *ProjectTypeGuids* i Zastąp ciąg {E53F8FEA-EAE0-44A6-8774-FFD645390401} z {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Zapisz zmiany, zamknij plik projektu (.csproj), który edytowania, kliknij prawym przyciskiem myszy projekt i następnie wybierz pozycję Załaduj ponownie projekt.
7. Jeśli projekt odwołuje się do żadnych bibliotek innych firm, które są kompilowane przy użyciu poprzedniej wersji platformy ASP.NET MVC, otwórz głównego pliku Web.config i dodaj następujące trzy *bindingRedirect* elementy w obszarze  *Konfiguracja* sekcji: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a>Nowe funkcje w wersji Beta programu ASP.NET MVC 4

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji platformy ASP.NET MVC 4 w wersji Beta.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

ASP.NET MVC 4 zawiera teraz interfejsu API sieci Web platformy ASP.NET, nowe umożliwiająca tworzenie usług HTTP, który może osiągnąć szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych. ASP.NET Web API również jest idealną platformą do tworzenia usługi RESTful.

ASP.NET Web API obsługuje następujące funkcje:

- **Nowoczesne modelu programowania protokołu HTTP:** bezpośrednio uzyskać dostęp i manipulowania nimi żądań HTTP i odpowiedzi w interfejsy API sieci Web przy użyciu nowego, silnie typizowany model obiektów HTTP. Taki sam programowania modelu i potoku HTTP jest symetrycznie dostępne na komputerze klienckim, za pomocą nowego typu HttpClient.
- **Pełną pomoc techniczną dla tras**: interfejsy API sieci Web teraz obsługuje ona pełnego zestawu możliwości trasy, które były zawsze częścią stos sieci Web, w tym parametry trasy i ograniczenia. Ponadto mapowanie do akcji ma pełną obsługę konwencje, dzięki czemu nie trzeba zastosować atrybutów, takich jak [HttpPost] do metod i klas.
- **Negocjowanie zawartości**: klient i serwer mogą pracować razem określają prawo format danych zwracanych z interfejsu API. Firma Microsoft zapewnia domyślną obsługę formatów zakodowane w adresie URL formularza, JSON i XML i można rozszerzyć tę obsługę, dodając własne elementy formatujące lub nawet jej zastąpienie strategia domyślna negocjacji zawartości.
- **Powiązania i sprawdzania poprawności modelu:** integratorów zapewniają prosty sposób wyodrębniania danych z różnych części żądania HTTP oraz przekształcać tych części komunikatu do obiektów platformy .NET, które mogą być używane przez akcje interfejsu API sieci Web.
- **Filtry:** interfejsów API sieci Web obsługuje teraz filtry, w tym filtry dobrze znanego, takich jak atrybutu [Authorize]. Można tworzyć i Dołącz filtry dla działania, autoryzacji i obsługi wyjątków.
- **Tworzenia zapytania:** powracając po prostu IQueryable&lt;T&gt;, internetowy interfejs API będzie obsługiwać zapytań za pomocą z konwencjami adresu URL OData.
- **Ulepszone testowania szczegóły HTTP:** zamiast ustawienie protokołu HTTP, szczegółowe informacje w obiektach kontekstu statycznego, akcje interfejsu API sieci Web, teraz można pracować z wystąpień HttpRequestMessage i obiektu HttpResponseMessage. Ogólny wersje tych obiektów istnieją również pozwala pracować swoje niestandardowe typy oprócz typów protokołu HTTP.
- **Ulepszone Inwersja kontroli (IoC) za pomocą klasy DependencyResolver:** interfejsu API sieci Web używa teraz wzorzec lokalizatora usług, które są implementowane przez mechanizm rozpoznawania zależności MVC do uzyskania wystąpienia do wielu różnych urządzeń.
- **Konfiguracja na podstawie kodu:** konfigurację składnika Web API odbywa się wyłącznie za pośrednictwem kodu, pozostawiając pliku config czyszczenia plików.
- **Hosta samodzielnego:** interfejsów API sieci Web może być hostowana we własnym procesie, oprócz usługi IIS podczas nadal przy użyciu pełnego zestawu funkcji tras i inne funkcje interfejsu API sieci Web.

Szczegółowe informacje na temat interfejsu API sieci Web platformy ASP.NET można znaleźć [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a>ASP.NET pojedynczej strony aplikacji

ASP.NET MVC 4 teraz obejmuje wczesną wersję zapoznawczą środowiska do tworzenia aplikacji jednostronicowej przy użyciu znaczące interakcji po stronie klienta przy użyciu języka JavaScript i interfejsów API sieci Web. Ta obsługa obejmuje:

- Zestaw bibliotek języka JavaScript dla bogatszych interakcji lokalnej przy użyciu danych z pamięci podręcznej
- Dodatkowe składniki interfejsu API sieci Web dla jednostki pracy i pomocy technicznej warstwy DAL
- Szablon projektu MVC za pomocą tworzenia szkieletu, aby szybko rozpocząć pracę

Szczegółowe informacje na temat aplikacji jednostronicowej obsługi w technologii ASP.NET MVC 4, odwiedź [ https://www.asp.net/single-page-application ](../single-page-application/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Ulepszenia domyślne szablony projektów

Szablon, który jest używany do tworzenia nowych projektów platformy ASP.NET MVC 4 została zaktualizowana w celu tworzenia witryny sieci Web bardziej nowoczesnym wyglądzie:

![](mvc4-beta-release-notes/_static/image1.png)

Oprócz kosmetycznych ulepszenia zostały udoskonalone funkcje w nowym szablonie. Szablon wykorzystuje technikę o nazwie adaptacyjne renderowania wyglądają dobrze zarówno w przypadku przeglądarek klasycznych, jak i przeglądarki dla urządzeń przenośnych bez potrzeby dostosowywania.

![](mvc4-beta-release-notes/_static/image2.png)

Aby zobaczyć adaptacyjne renderowania w akcji, możesz użyć emulatora mobilnych lub po prostu spróbuj zmienia rozmiar okna przeglądarki na komputerze, który może być mniejszy. Gdy okno przeglądarki pobiera wystarczająco mała, zmieni się układ strony.

Innym usprawnieniem domyślny szablon projektu jest korzystanie z języka JavaScript umożliwia bogatszych interfejsu użytkownika. Logowanie i rejestrowanie łącza, które są używane w szablonie są przykłady sposobów użycia jQuery okno dialogowe interfejsu użytkownika do przedstawienia ekran logowania sformatowanego:

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Szablon projektu przenośnych

Jeśli Trwa uruchamianie nowego projektu, aby utworzyć witryny specjalnie dla urządzeń przenośnych i tabletów przeglądarki można użyć szablonu projektu aplikacji mobilnej. Jest to oparty na technologii jQuery Mobile, biblioteka typu open source do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika:

![](mvc4-beta-release-notes/_static/image4.png)

Ten szablon zawiera tę samą strukturę aplikacji jako szablonu aplikacji internetowej i kodu kontrolera jest niemal identyczne, ale jest stylem, wygląd i działają prawidłowo na urządzeniach przenośnych oparte na dotyku przy użyciu jQuery Mobile. Aby dowiedzieć się więcej na temat struktury i stylem przenośnych interfejsu użytkownika, zobacz [jQuery przenośnych projektu witryny sieci Web](http://jquerymobile.com/).

Jeśli masz już zorientowane na pulpicie lokacji, czy chcesz dodać widoki zoptymalizowane pod kątem mobile, lub jeśli chcesz tworzyć jednej lokacji, który służy inaczej ze stylem widoków do klasycznych i mobilnych przeglądarek, można użyć nowej funkcji trybów wyświetlania. (Zobacz następną sekcję).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Tryby wyświetlania

Nowa funkcja trybów wyświetlania umożliwia aplikacji wybierz widoki, w zależności od przeglądarki, z której wysłano żądanie. Na przykład na stronie głównej na żądanie przeglądarki na komputerze aplikacji może być szablon Views\Home\Index.cshtml. Jeśli w przeglądarce dla urządzeń przenośnych żąda strony głównej, aplikacja może zwrócić szablonu Views\Home\Index.mobile.cshtml.

Układy i częściowych również można przesłonić dla typów w konkretnej przeglądarce. Na przykład:

- Jeśli Views\Shared folder zawiera zarówno \_Layout.cshtml i \_Layout.mobile.cshtml szablonów, domyślnie aplikacja będzie używać \_Layout.mobile.cshtml podczas żądania od przeglądarki dla urządzeń przenośnych i \_Layout.cshtml podczas innych żądań.
- Jeśli folder zawiera zarówno \_MyPartial.cshtml i \_MyPartial.mobile.cshtml, instrukcja @Html.Partial("\_MyPartial") będzie renderowany \_MyPartial.mobile.cshtml podczas żądania od mobile przeglądarki, a \_MyPartial.cshtml podczas innych żądań.

Jeśli chcesz utworzyć bardziej szczegółowych widoków układów i widoki częściowe, w przypadku innych urządzeń, możesz zarejestrować nową *DefaultDisplayMode* wystąpienie można określić, której nazwa do wyszukiwania, gdy żądanie spełnia warunki określonej. Na przykład można dodać następujący kod, aby *aplikacji\_Start* metody w pliku Global.asax zarejestrować się ciągiem "iPhone" jako tryb wyświetlania, która ma zastosowanie, gdy przeglądarka iPhone Apple wysyła żądanie:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

Po uruchomieniu tego kodu, gdy przeglądarka iPhone Apple wysyła żądanie, aplikacja będzie używać Views\Shared\\_Layout.iPhone.cshtml układ (jeśli istnieje).

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a>jQuery Mobile, przełącznikiem widoku i zastępowanie przeglądarki

jQuery Mobile to biblioteka typu open source do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika sieci web. Jeśli chcesz używać jQuery Mobile z aplikacją ASP.NET MVC 4 można pobrać i zainstalować pakiet NuGet, który pomoże Ci rozpocząć pracę. Aby go zainstalować z poziomu konsoli Menedżera pakietów Visual Studio, wpisz następujące polecenie:

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

Spowoduje to zainstalowanie jQuery Mobile, a niektóre pliki pomocnicze, takie jak następujące:

- Widoki/Shared/\_Layout.Mobile.cshtml, czyli układzie jQuery Mobile.
- Składnik przełącznikiem widoku, który składa się zwidoków/Shared/\_ViewSwitcher.cshtml widok częściowy i kontroler ViewSwitcherController.cs.

Po zainstalowaniu pakietu, uruchom aplikację za pomocą przeglądarce dla urządzeń przenośnych (lub równoważne, takich jak program Firefox [przełącznik agenta użytkownika](http://chrispederick.com/work/user-agent-switcher/) dodatku). Zostaną wyświetlone strony wyglądają zupełnie inny, ponieważ jQuery Mobile obsługuje układ i style. Aby móc korzystać z tego, czy są:

- Utwórz wartości zastąpień mobile określonego widoku zgodnie z opisem w obszarze [trybów wyświetlania](#_Toc303253810) wcześniej (na przykład można utworzyć Views\Home\Index.mobile.cshtml do zastąpienia Views\Home\Index.cshtml przeglądarki dla urządzeń przenośnych).
- Odczyt [jQuery przenośnych dokumentacji](http://jquerymobile.com/) Aby dowiedzieć się więcej o sposobie dodawania zoptymalizowane pod kątem dotykowe elementy interfejsu użytkownika w widokach dla urządzeń przenośnych.

Konwencja zoptymalizowane pod kątem mobile stron sieci web jest dodać łącze, którego tekst jest coś, takich jak widok pulpitu lub w trybie witrynę w trybie pełnym, który umożliwia użytkownikom, przełącz się do klasycznej wersji strony. Pakiet jQuery.Mobile.MVC zawiera przykładowy składnik przełącznikiem widoku w tym celu. Jest on używany w domyślnej Views\Shared\\widoku _Layout.Mobile.cshtml i Po wyrenderowaniu strony wyglądają następująco:

![](mvc4-beta-release-notes/_static/image5.png)

Jeśli osoby odwiedzające kliknie link, są przełączone klasycznej wersji tej samej stronie.

Ponieważ układ pulpitu nie będzie zawierać przełącznikiem widoku domyślnie, goście nie będzie miał sposób, aby przejść do trybu mobilnych. Aby włączyć tę opcję, należy dodać następujące odwołanie do  *\_ViewSwitcher* do układu pulpitu, po prostu wewnątrz *&lt;treści&gt;* elementu:

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

Przełącznik widoku używa nową funkcję o nazwie zastępowania przeglądarki. Ta funkcja umożliwia aplikacji taką obsługę żądań, tak jakby pochodziły one z innej przeglądarki (agenta użytkownika) niż są rzeczywiście z. Poniższa tabela zawiera listę metod, udostępnianych przez usługę zastępowania przeglądarki.

| `HttpContext.SetOverriddenBrowser(userAgentString)` | Przesłania żądania rzeczywistą wartość agenta użytkownika przy użyciu określonego agenta użytkownika. |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | Zwraca wartość zastąpienia agenta użytkownika żądania lub rzeczywisty ciąg agenta użytkownika, jeśli żadne przesłonięcie nie zostało określone. |
| `HttpContext.GetOverriddenBrowser()` | Zwraca *HttpBrowserCapabilitiesBase* wystąpienie, które odnosi się do agenta użytkownika aktualnie ustawiona dla żądania (rzeczywistych lub zastąpiona). Tej wartości można użyć do pobrania właściwości, takie jak *IsMobileDevice*. |
| `HttpContext.ClearOverriddenBrowser()` | Usuwa wszelkich przesłoniętych agentów użytkownika dla bieżącego żądania. |

Przesłanianie przeglądarki jest funkcją core ASP.NET MVC 4 i jest dostępna, nawet wtedy, gdy nie jest instalowany pakiet jQuery.Mobile.MVC. Ma to jednak wpływ widoku, układ i wybór widoku częściowego — nie ma wpływu na inne funkcje platformy ASP.NET, który zależy od *Request.Browser* obiektu.

Domyślnie zastąpienie agenta użytkownika są przechowywane przy użyciu pliku cookie. Jeśli chcesz przechowywać na przesłonięcia, gdzie indziej (na przykład w bazie danych), można zastąpić domyślny dostawca (*BrowserOverrideStores.Current*). Dokumentacja dla tego dostawcy będą dostępne dla towarzyszyć nowszej wersji platformy ASP.NET MVC.

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a>Rozwiązania na potrzeby generowania kodu w programie Visual Studio

Nowa funkcja przepisy umożliwia środowisku Visual Studio można wygenerować kodu specyficznego dla rozwiązania, oparte na pakiety, które można zainstalować za pomocą narzędzia NuGet. Framework przepisy ułatwia programistom pisanie wtyczek generowania kodu, który umożliwia także zastąpić generatorów kodu wbudowanego dodać obszaru, Dodaj kontroler i Dodaj widok. Ponieważ przepisy są wdrażane jako pakiety NuGet, można łatwo je sprawdzone w formancie źródła i automatycznie udostępniane wszystkim deweloperom rozwiązań w projekcie. Są one również dostępne na podstawie danego rozwiązania.

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Zadanie obsługi asynchronicznego kontrolerów

Teraz możesz tworzyć metody akcji asynchronicznej jako pojedynczej metody, które zwracają obiekt typu *zadań* lub *zadań&lt;ActionResult&gt;*.

Na przykład, jeśli używasz Visual C# 5 (lub za pomocą [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), możesz utworzyć asynchronicznej metody akcji, która wygląda podobnie do następującej:

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

W metodzie akcji z poprzedniego wywołania *newsService.GetHeadlinesAsync* i *sportsService.GetScoresAsync* są wywoływane asynchronicznie, a nie blokują wątek z puli wątków.

Metody asynchroniczne akcji, które zwracają *zadań* wystąpień może również obsługiwać przekroczeń limitu czasu. Aby wprowadzić swoje metody akcji można anulować, Dodaj parametr typu *CancellationToken* w podpisie metody akcji. W poniższym przykładzie pokazano asynchronicznej metody akcji ma limit czasu równy 2500 milisekund i wyświetlającą *przekroczenie limitu czasu* wyświetlić do klienta, jeśli zostanie przekroczony limit czasu.

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 w wersji Beta obsługuje wrzesień 2011 r. 1.5 wersję zestawu Windows Azure SDK.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

- **Po zainstalowaniu programu ASP.NET MVC 4 w wersji Beta, Edytor CSHTML/VBHTML w edytorze programu Visual Studio 2010 Service Pack 1 CSHTML/VBHTML mogą wstrzymać przez długi czas, po wpisaniu w plikach cshtml lub vbhtml fragment kodu lub języka JavaScript.** Dzieje się tak tylko w aplikacjach ASP.NET MVC 4, które właśnie utworzony i nie został skompilowany.

    Obejście polega na skompilować projekt, aby pobrać zestawy z folderu bin. Należy zauważyć, że jeśli możesz wyczyścić projektu, który usuwa zestawy z folderu bin, problem edytora przechodzi.

    Zostanie to poprawione w następnej wersji.
- **Szablony projektów C# dla programu Visual Studio 11 Beta zawierają nieprawidłowe parametry połączeń w Global.asax.cs.** Domyślne połączenie, określona w aplikacji\_Uruchom metodę dla projektów utworzonych w programie Visual Studio 11 Beta zawierać ciąg połączenia LocalDB, który zawiera o niezmienionym znaczeniu kreski ułamkowej odwróconej (\) znaków. Powoduje to błąd połączenia podczas próby uzyskania dostępu Entity Framework DbContext, który generuje sqlexception —.

    Aby rozwiązać ten problem, znak ucieczki ukośnika odwrotnego w aplikacji\_Start metoda Global.asax.cs, tak aby wyglądało następująco:

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- **Aplikacje platformy ASP.NET MVC 4, których przeznaczony dla platformy .NET 4.5 zgłosi fileloadexception — podczas próby dostępu do zestawu System.Net.Http.dll uruchamiania w ramach programu .NET 4.0.** Aplikacje platformy ASP.NET MVC 4, utworzone w ramach platformy .NET 4.5 zawierają powiązania przekierowania, które będą powodować fileloadexception —, która stwierdza, że "nie można załadować pliku lub zestawu"System.Net.Http"lub jednej z jego zależności." gdy aplikacji jest wykonywana w systemie przy użyciu programu .NET 4.0, zainstalowane. Aby rozwiązać ten problem, należy usunąć następujące przekierowanie powiązania z pliku web.config:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    Element powiązania zestawu w pliku web.config zmodyfikowane powinna wyglądać następująco:

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <strong>Szablon elementu "Dodaj kontroler" w projektach języka Visual Basic generuje nieprawidłowa przestrzeń nazw po wywołaniu</strong><strong>z wewnątrz obszaru.</strong> Po dodaniu kontrolera do obszaru w projekcie ASP.NET MVC, która używa języka Visual Basic, szablon elementu wstawia nieprawidłową przestrzeń nazw do kontrolera. Wynikiem jest błąd "nie można odnaleźć pliku" po przejściu do dowolnej akcji w kontrolerze.  
  
  Wygenerowany obszar nazw pomija wszystko po głównej przestrzeni nazw. Na przykład, przestrzeń nazw, generowany jest *RootNamespace* , ale powinien być *RootNamespace.Areas.AreaName.Controllers* .
- **Istotne zmiany w aparatu widoku Razor.** W ramach nadpisania analizator Razor, następujące typy zostały usunięte z *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Ponadto usunięto następujące metody: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **WebMatrix.WebData.dll znajduje się w katalogu/bin aplikacji ASP.NET MVC 4, zajmuje się za pośrednictwem adresu URL dla uwierzytelniania formularzy.** Trwa dodawanie zestawu WebMatrix.WebData.dll z aplikacją (np. przez wybranie "ASP.NET Web Pages o składni Razor", gdy za pomocą okna dialogowego Dodaj zależności do wdrożenia) spowoduje przesłonięcie przekierowania loginu uwierzytelnianie/konto/zalogować się zamiast / konta/logowania zgodnie z oczekiwaniami, domyślny kontroler konta programu ASP.NET MVC. Aby temu zapobiec i użyj adresu URL już określone w sekcji uwierzytelniania w pliku Web.config, możesz dodać element appSetting o nazwie PreserveLoginUrl i ustaw ją na wartość true: 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- **Menedżer pakietów NuGet nie zostanie zainstalowany podczas próby zainstalowania platformy ASP.NET MVC 4 dla równoległymi instalacjami programu Visual Studio 2010 i Visual Web Developer 2010.** Do uruchomienia programu Visual Studio 2010 i Visual Web Developer 2010 równolegle z programem ASP.NET MVC 4 należy zainstalować platformy ASP.NET MVC 4, gdy obie wersje programu Visual Studio ma już zainstalowany.
- **Odinstalowywanie programu ASP.NET MVC 4 kończy się niepowodzeniem, jeśli wstępnie wymagane składniki zostały już odinstalowane.** Aby prawidłowo odinstalować ASP.NET MVC 4you należy odinstalować ASP.NET MVC 4, przed rozpoczęciem odinstalowywania programu Visual Studio.
- **Uruchamianie projektu domyślnego internetowego interfejsu API zawiera instrukcje, które niepoprawnie kierować użytkownikowi na dodawanie tras przy użyciu metody RegisterApis, która nie istnieje.** Trasy powinny zostać dodane w metodzie RegisterRoutes przy użyciu tabeli trasy ASP.NET.
- **Instalowanie platformy ASP.NET MVC 4 w wersji Beta przerywa aplikacji ASP.NET MVC 3 RTM.** Aplikacje programu ASP.NET MVC 3, które zostały utworzone za pomocą wersji RTM wersji (nie w programie ASP.NET MVC 3 Tools Update) wymagają następujących zmian do działania side-by-side przy użyciu platformy ASP.NET MVC 4 w wersji Beta. Tworzenie projektu bez wprowadzania tych wyników aktualizacji błędy kompilacji. 

    **Wymagane aktualizacje**

  1. W głównym pliku Web.config, Dodaj nowy *&lt;appSettings&gt;* wpis z kluczem *webPages:Version* i wartość *1.0.0.0*.

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz pozycję Edytuj *ProjectName*csproj.
  3. Znajdź następujące odwołania do zestawów: 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      Zastąp je poniżej:

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. Zapisz zmiany, zamknij plik projektu (.csproj), edytowania, a następnie kliknij prawym przyciskiem myszy projekt i wybierz pozycję Załaduj ponownie.
