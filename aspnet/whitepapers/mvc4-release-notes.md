---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Ten dokument opisuje wersję platformy ASP.NET MVC 4.
ms.author: aspnetcontent
ms.date: 09/09/2011
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: 9e4d73349c4c01e0717aabe7da9cb126d2c545c0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826581"
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Ten dokument opisuje wersję platformy ASP.NET MVC 4.


- [Uwagi dotyczące instalacji](#_Toc303253802)
- [Dokumentacja](#_Toc303253803)
- [Obsługa](#_Toc303253804)
- [Wymagania dotyczące oprogramowania](#_Toc303253805)
- [Nowe funkcje we wzorcu ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Ulepszenia domyślne szablony projektów](#_Toc303253808)
    - [Szablon projektu przenośnych](#_Toc303253809)
    - [Tryby wyświetlania](#_Toc303253810)
    - [jQuery Mobile, przełącznikiem widoku i zastępowanie przeglądarki](#_Toc303253811)
    - [Zadanie obsługi asynchronicznego kontrolerów](#_Toc303253813)
    - [Zestaw Azure SDK](#_Toc303253814)
    - [Migracje baz danych](#_Toc303253818)
    - [Szablonu pusty projekt](#_Toc303253819)
    - [Dodawanie kontrolera do dowolnego folderu projektu](#_Toc303253820)
    - [Tworzenie pakietów i minifikacja](#_Toc303253821)
    - [Włączanie logowania do usług Facebook i innych lokacji za pomocą protokołu OAuth i OpenID](#_Toc303253822)
- [Uaktualnianie projektu ASP.NET MVC 3 do ASP.NET MVC 4](#_Toc303253806)
- [Zmiany w porównaniu z platformy ASP.NET MVC 4 w wersji Release Candidate](#_Toc303253817)
- [Znane problemy i fundamentalne zmiany](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Uwagi dotyczące instalacji

ASP.NET MVC 4 dla programu Visual Studio 2010 można zainstalować ze [strony głównej platformy ASP.NET MVC 4](../mvc/mvc4.md) za pomocą Instalatora platformy sieci Web.

Zaleca się odinstalowanie żadnych zainstalowanych wcześniej wersji zapoznawczych programu ASP.NET MVC 4, przed zainstalowaniem programu ASP.NET MVC 4. ASP.NET MVC 4 w wersji Beta i wersji Release Candidate można uaktualnić do programu ASP.NET MVC 4 bez odinstalowania.

Ta wersja nie jest zgodny z dowolnej wersji zapoznawczych programu .NET Framework 4.5. Należy uaktualnić wszystkie zainstalowanej wersji zapoznawczych programu .NET Framework 4.5 oddzielnie do ostatecznej wersji przed zainstalowaniem programu ASP.NET MVC 4.

ASP.NET MVC 4 może być zainstalowane i działają side-by-side przy użyciu platformy ASP.NET MVC 3.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentacja

Dokumentacja dla platformy ASP.NET MVC jest dostępna w witrynie MSDN pod adresem URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

W samouczkach i innych informacji na temat platformy ASP.NET MVC są dostępne na stronie witryny sieci Web platformy ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Obsługa

ASP.NET MVC 4 jest w pełni obsługiwany. Jeśli masz pytania na temat pracy z tej wersji można również ogłaszać je na forum platformy ASP.NET MVC ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), gdzie są często w stanie zapewnić obsługę nieformalne członków społeczności platformy ASP.NET.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Wymagania programowe

Składniki platformy ASP.NET MVC 4 dla programu Visual Studio wymagają programu PowerShell w wersji 2.0, a program Visual Studio 2010 z dodatkiem Service Pack 1 lub Visual Web Developer Express 2010 z dodatkiem Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nowe funkcje we wzorcu ASP.NET MVC 4

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji platformy ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

Platforma ASP.NET MVC 4 zawiera Web API platformy ASP.NET, nowe umożliwiająca tworzenie usług HTTP, które mają dostęp do szerokiej gamy klientów, w tym przeglądarek i urządzeń przenośnych. ASP.NET Web API również jest idealną platformą do tworzenia usługi RESTful.

ASP.NET Web API obsługuje następujące funkcje:

- **Nowoczesne modelu programowania protokołu HTTP:** bezpośrednio uzyskać dostęp i manipulowania nimi żądań HTTP i odpowiedzi w interfejsy API sieci Web przy użyciu nowego, silnie typizowany model obiektów HTTP. Taki sam programowania modelu i potoku HTTP jest symetrycznie dostępny na kliencie w nowej *HttpClient* typu.
- **Pełną pomoc techniczną dla tras:** interfejsu API sieci Web platformy ASP.NET obsługuje pełny zestaw możliwości routingu platformy ASP.NET, w tym parametry trasy i ograniczenia trasy. Ponadto umożliwia proste konwencje mapowania akcji metody HTTP.
- **Negocjowanie zawartości:** klient i serwer mogą pracować razem określają prawo format danych zwracanych z internetowego interfejsu API. ASP.NET Web API zapewnia domyślną obsługę XML, JSON, dane i formatów zakodowane w adresie URL formularza i można rozszerzyć tę obsługę, dodając własne elementy formatujące lub nawet jej zastąpienie strategia domyślna negocjacji zawartości.
- **Powiązania i sprawdzania poprawności modelu:** integratorów zapewniają prosty sposób wyodrębniania danych z różnych części żądania HTTP oraz przekształcać tych części komunikatu do obiektów platformy .NET, które mogą być używane przez akcje interfejsu API sieci Web. Sprawdzanie poprawności, również odbywa się na parametry akcji na adnotacje danych na podstawie.
- **Filtry:** Web API platformy ASP.NET obsługuje filtry, w tym filtry dobrze znanego, takich jak *[Authorize]* atrybutu. Można tworzyć i Dołącz filtry dla działania, autoryzacji i obsługi wyjątków.
- **Tworzenia zapytania:** użyj *[Queryable]* atrybutu filtru akcji, która zwraca *IQueryable* umożliwiające obsługę interfejsu API sieci web za pomocą Konwencji zapytania OData.
- **Ulepszone możliwości testowania:** zamiast ustawienie protokołu HTTP, szczegółowe informacje w obiektach kontekstu statycznego, sieci web interfejsu API pracy akcji z wystąpieniami *HttpRequestMessage* i *obiektu HttpResponseMessage*. Utwórz projekt testu jednostkowego, wraz z projektem interfejsu API sieci Web, aby rozpocząć pracę, szybkie pisanie testów jednostkowych dla Twojej funkcji interfejsu API sieci Web.
- **Konfiguracja na podstawie kodu:** konfigurację składnika Web API platformy ASP.NET jest realizowane wyłącznie przy użyciu kodu, pozostawiając pliku config czyszczenia plików. Aby skonfigurować punkty rozszerzeń, należy użyć wzorzec lokalizatora świadczonych usług.
- **Ulepszona obsługa kontenerów Inwersja kontroli (IoC):** interfejsu API sieci Web platformy ASP.NET oferuje fantastyczną pomoc techniczną dla kontenerów IoC za pośrednictwem abstrakcję rozpoznawania zależności ulepszone
- **Hosta samodzielnego:** interfejsów API sieci Web może być hostowana we własnym procesie, oprócz usługi IIS podczas nadal przy użyciu pełnego zestawu funkcji tras i inne funkcje interfejsu API sieci Web.
- **Tworzenie niestandardowej pomocy i stron w teście:** teraz można łatwo tworzyć niestandardowe pomocy i stron w teście sieci Web, interfejsów API za pomocą nowego *IApiExplorer* usługi, aby uzyskać opis pełne środowisko uruchomieniowe Projektując internetowe interfejsy API.
- **Monitorowanie i Diagnostyka:** interfejsu API sieci Web platformy ASP.NET udostępnia teraz Infrastruktura śledzenia lekkie, który można łatwo zintegrować z istniejącymi rozwiązaniami rejestrowania, takich jak System.Diagnostics zdarzeń systemu Windows i innych firm, struktur rejestrowania. Możesz włączyć śledzenie, zapewniając *ITraceWriter* implementacji i dodanie go do konfiguracji interfejsu API sieci web.
- **Link generacji:** za pomocą interfejsu API sieci Web platformy ASP.NET *UrlHelper* do generowania linków do powiązanych zasobów w tej samej aplikacji.
- **Szablon projektów internetowych interfejsów API:** wybierz formularz nowego projektu internetowego interfejsu API Kreatora nowego projektu programu MVC 4, aby szybko rozpocząć pracę z interfejsem API sieci Web platformy ASP.NET.
- **Tworzenie szkieletu:** użyj **Dodaj kontroler** okna dialogowego, aby szybko utworzyć szkielet kontrolera interfejsu API sieci web, w oparciu o platformy Entity Framework na podstawie typu modelu.

Szczegółowe informacje na temat interfejsu API sieci Web platformy ASP.NET można znaleźć [ https://www.asp.net/web-api ](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Ulepszenia domyślne szablony projektów

Szablon, który jest używany do tworzenia nowych projektów platformy ASP.NET MVC 4 została zaktualizowana w celu tworzenia witryny sieci Web bardziej nowoczesnym wyglądzie:

![](mvc4-release-notes/_static/image1.png)

Oprócz kosmetycznych ulepszenia zostały udoskonalone funkcje w nowym szablonie. Szablon wykorzystuje technikę o nazwie adaptacyjne renderowania wyglądają dobrze zarówno w przypadku przeglądarek klasycznych, jak i przeglądarki dla urządzeń przenośnych bez potrzeby dostosowywania.

![](mvc4-release-notes/_static/image2.png)

Aby zobaczyć adaptacyjne renderowania w akcji, możesz użyć emulatora mobilnych lub po prostu spróbuj zmienia rozmiar okna przeglądarki na komputerze, który może być mniejszy. Gdy okno przeglądarki pobiera wystarczająco mała, zmieni się układ strony.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Szablon projektu przenośnych

Jeśli Trwa uruchamianie nowego projektu, aby utworzyć witryny specjalnie dla urządzeń przenośnych i tabletów przeglądarki można użyć szablonu projektu aplikacji mobilnej. Jest to oparty na technologii jQuery Mobile, biblioteka typu open source do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika:

![](mvc4-release-notes/_static/image3.png)

Ten szablon zawiera tę samą strukturę aplikacji jako szablonu aplikacji internetowej i kodu kontrolera jest niemal identyczne, ale jest stylem, wygląd i działają prawidłowo na urządzeniach przenośnych oparte na dotyku przy użyciu jQuery Mobile. Aby dowiedzieć się więcej na temat struktury i stylem przenośnych interfejsu użytkownika, zobacz [jQuery przenośnych projektu witryny sieci Web](http://jquerymobile.com/).

Jeśli masz już zorientowane na pulpicie lokacji, czy chcesz dodać widoki zoptymalizowane pod kątem mobile, lub jeśli chcesz tworzyć jednej lokacji, który służy inaczej ze stylem widoków do klasycznych i mobilnych przeglądarek, można użyć nowej funkcji trybów wyświetlania. (Zobacz następną sekcję).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Tryby wyświetlania

Nowa funkcja trybów wyświetlania umożliwia aplikacji wybierz widoki, w zależności od przeglądarki, z której wysłano żądanie. Na przykład na stronie głównej na żądanie przeglądarki na komputerze aplikacji może być szablon Views\Home\Index.cshtml. Jeśli w przeglądarce dla urządzeń przenośnych żąda strony głównej, aplikacja może zwrócić szablonu Views\Home\Index.mobile.cshtml.

Układy i częściowych również można przesłonić dla typów w konkretnej przeglądarce. Na przykład:

- Jeśli Views\Shared folder zawiera zarówno \_Layout.cshtml i \_Layout.mobile.cshtml szablonów, domyślnie aplikacja będzie używać \_Layout.mobile.cshtml podczas żądania od przeglądarki dla urządzeń przenośnych i \_Layout.cshtml podczas innych żądań.
- Jeśli folder zawiera zarówno \_MyPartial.cshtml i \_MyPartial.mobile.cshtml, instrukcja @Html.Partial("\_MyPartial") będzie renderowany \_MyPartial.mobile.cshtml podczas żądania od mobile przeglądarki, a \_MyPartial.cshtml podczas innych żądań.

Jeśli chcesz utworzyć bardziej szczegółowych widoków układów i widoki częściowe, w przypadku innych urządzeń, możesz zarejestrować nową *DefaultDisplayMode* wystąpienie można określić, której nazwa do wyszukiwania, gdy żądanie spełnia warunki określonej. Na przykład można dodać następujący kod, aby *aplikacji\_Start* metody w pliku Global.asax zarejestrować się ciągiem "iPhone" jako tryb wyświetlania, która ma zastosowanie, gdy przeglądarka iPhone Apple wysyła żądanie:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Po uruchomieniu tego kodu, gdy przeglądarka iPhone Apple wysyła żądanie, aplikacja będzie używać Views\Shared\\_Layout.iPhone.cshtml układ (jeśli istnieje). Aby uzyskać więcej informacji na temat trybu wyświetlania, zobacz [platformy ASP.NET MVC 4 Mobile funkcji](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Należy instalować aplikacje przy użyciu DisplayModeProvider [stały DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pakietu NuGet. [ASP.NET Fall 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) obejmuje [stały DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pakietu NuGet w nowe szablony projektów. Zobacz [Fixedd buforowania błędów platformy ASP.NET MVC 4 Mobile](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) szczegółowe informacje dotyczące poprawki.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile i funkcje Mobile

Instrukcje dotyczące tworzenia aplikacji mobilnych za pomocą platformy ASP.NET MVC 4 przy użyciu jQuery Mobile znajduje się w samouczku [platformy ASP.NET MVC 4 Mobile funkcji](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Zadanie obsługi asynchronicznego kontrolerów

Teraz możesz tworzyć metody akcji asynchronicznej jako pojedynczej metody, które zwracają obiekt typu *zadań* lub *zadań&lt;ActionResult&gt;*.

 Aby uzyskać więcej informacji, zobacz [przy użyciu metod asynchronicznych w ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 obsługuje 1.6 i nowszych wersjach systemu Windows Azure SDK.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migracje baz danych

Projekty programu ASP.NET MVC 4 obejmują teraz Entity Framework 5. Jedną z najważniejszych funkcji w programie Entity Framework 5 jest obsługiwane migracje baz danych. Ta funkcja pozwala łatwo rozwój schemat bazy danych przy użyciu migracji skoncentrowane na kodzie przy jednoczesnym zachowaniu danych w bazie danych. Aby uzyskać więcej informacji na temat migracji bazy danych, zobacz [dodanie nowego pola do modelu Movie i tabeli](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) w [wprowadzenie do platformy ASP.NET MVC 4 samouczka](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Szablonu pusty projekt

MVC pusty szablon projektu jest teraz naprawdę pusty, dzięki czemu można rozpocząć od pustego całkowicie. Starszą wersję szablonu pusty projekt została zmieniona na podstawowe.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Dodawanie kontrolera do dowolnego folderu projektu

Teraz kliknij prawym przyciskiem myszy i wybierz **Dodaj kontroler** z dowolnego folderu w projekcie MVC. Zapewnia większą elastyczność w przypadku organizowania z kontrolerami w dowolny sposób, w tym zachowaniem kontrolerach MVC i interfejs API sieci Web w oddzielnych folderów.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Tworzenie pakietów i minimalizowanie

Tworzenie pakietów i minimalizowanie framework pozwala zmniejszyć liczbę żądań HTTP, które strony sieci Web musi wprowadzić, łącząc poszczególnych plików w jednym, powiązane plik skrypty i arkusze CSS. Go następnie redukowania łącznego rozmiaru te żądania, przez minifikacja zawartości pakietu. Minifikacja mogą obejmować działań takich jak wyeliminowanie odstępu, można skrócić nawet zwijanie selektorów CSS w oparciu o ich semantyki nazw zmiennych. Pakiety są deklarowane i skonfigurowane w kod i są łatwo przywoływać w widokach za pośrednictwem metody pomocnika, które mogą generować albo jedno łącze do pakietu, lub podczas debugowania, wiele łączy do poszczególnych zawartości pakietu. Aby uzyskać więcej informacji, zobacz [tworzenie pakietów i minimalizowanie](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Włączanie logowania do usług Facebook i innych lokacji za pomocą protokołu OAuth i OpenID

Domyślne szablony w szablonie projektu programu ASP.NET MVC 4 Internet obejmuje teraz obsługę logowania OAuth i OpenID za pomocą biblioteki DotNetOpenAuth. Aby uzyskać informacje dotyczące konfigurowania dostawcy uwierzytelniania OAuth lub OpenID, zobacz [obsługi uwierzytelniania OAuth/OpenID formularzy sieci Web, MVC i stron sieci Web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) i [OAuth i OpenID są wyposażone w dokumentacji w składniku ASP.NET Web Pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Uaktualnianie projektu ASP.NET MVC 3 do ASP.NET MVC 4

ASP.NET MVC 4 można zainstalować równolegle z programem ASP.NET MVC 3 na tym samym komputerze, co daje elastyczności w wyborze, kiedy należy uaktualnić aplikację ASP.NET MVC 3 do ASP.NET MVC 4.

Najprostszym sposobem uaktualnienia jest utworzyć nowy projekt ASP.NET MVC 4 i skopiuj wszystkie widoki, kontrolery, kodu i zawartość plików z istniejącego projektu MVC 3 do nowego projektu i można zaktualizować zestawu odwołuje się w nowym projekcie, aby dopasować dowolny szablon MVC inne niż w assembiles dołączone, którego używasz. Jeśli zmiany zostały wprowadzone do pliku Web.config w projekcie MVC 3, można również scalić te zmiany w pliku Web.config w projekcie MVC 4.

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 3 w wersji 4, wykonaj następujące czynności:

1. W pliku Web.config wszystkie pliki w projekcie (istnieje w katalogu głównym projektu: jeden w folderze Widoki i jeden w folderze widoków dla każdego obszaru w projekcie), Zastąp każde wystąpienie następującego tekstu (Uwaga: System.Web.WebPages, Version = 1.0.0.0 nie zostanie znaleziony w projekty utworzone za pomocą programu Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    za pomocą odpowiedniego następujący tekst:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. W głównym pliku Web.config, zaktualizuj *webPages:Version* elementu "2.0.0.0" i Dodaj nowy *PreserveLoginUrl* klucza, który ma wartość "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy na odwołań, a następnie wybierz polecenie Zarządzaj pakietami NuGet. W okienku po lewej stronie wybierz **źródła pakietu oficjalne Online\NuGet**, zaktualizuj następujące czynności:

    - ASP.NET MVC 4
    - (Opcjonalnie) jQuery, jQuery sprawdzania poprawności i interfejs użytkownika jQuery
    - (Opcjonalnie) Entity Framework
    - (Optonal) Modernizr
4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz pozycję Edytuj *ProjectName*csproj.
5. Znajdź *ProjectTypeGuids* i Zastąp ciąg {E53F8FEA-EAE0-44A6-8774-FFD645390401} z {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Zapisz zmiany, zamknij plik projektu (.csproj), który edytowania, kliknij prawym przyciskiem myszy projekt i następnie wybierz pozycję Załaduj ponownie projekt.
7. Jeśli projekt odwołuje się do żadnych bibliotek innych firm, które są kompilowane przy użyciu poprzedniej wersji platformy ASP.NET MVC, otwórz głównego pliku Web.config i dodaj następujące trzy *bindingRedirect* elementy w obszarze  *Konfiguracja* sekcji: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Zmiany w porównaniu z platformy ASP.NET MVC 4 w wersji Release Candidate

Informacje o wersji dla platformy ASP.NET MVC 4 Release Candidate można znaleźć tutaj:

Poniżej przedstawiono istotne zmiany z wersji Release Candidate programu ASP.NET MVC 4 w tej wersji:

- **Na konfigurację kontrolera:** kontrolerów Web API platformy ASP.NET można przypisać za pomocą atrybutu niestandardowego, który implementuje *IControllerConfiguration* można skonfigurować własne elementy formatujące, selektor akcji i integratorów parametru . *HttpControllerConfigurationAttribute* został usunięty.
- **Na programy obsługi komunikatów trasy:** możesz teraz określić obsługi końcowym komunikacie w łańcuchu żądanie dla danej trasy. Umożliwia to obsługę platform jazdy wzdłuż do użycia routingu do wysłania do ich własnych (non -*IHttpController*) punktów końcowych.
- **Postęp powiadomienia:** *ProgressMessageHandler* generuje powiadomienie o postępie dla przekazywanych jednostek żądania i pobieranych jednostek odpowiedzi. Za pomocą tej procedury obsługi jest możliwe do śledzenia jak daleko przekazanie treści żądania lub pobierania treści odpowiedzi.
- **Wypchnij zawartość:** *PushStreamContent* klasy umożliwia realizację scenariuszy, w których producent danych chce dokonywać zapisu bezpośrednio do żądania lub odpowiedzi (synchronicznie lub asynchronicznie) przy użyciu strumienia. Gdy *PushStreamContent* jest gotowy do przyjęcia danych, które wywołuje się, aby delegat akcji ze strumienia wyjściowego. Deweloper może następnie zapisywać do strumienia dla tak długo, jak to konieczne i zamknij strumień, podczas zapisywania zostało zakończone. *PushStreamContent* wykrywa zamknięcia strumienia i zakończeniu podstawowych asynchronicznych *zadań* dla wypisywanie zawartości.
- **Tworzenie odpowiedzi na błędy:** użyj *HttpError* typ spójnie reprezentującego informacje o błędzie z takie jak błędy sprawdzania poprawności i wyjątki, przy jednoczesnym zachowaniu nadal *IncludeErrorDetailPolicy*. Użyj nowego *CreateErrorResponse* metody rozszerzenia umożliwiające łatwe tworzenie odpowiedzi na błędy z *HttpError* jako zawartość. *HttpError* zawartość jest w pełni zawartości negocjowane.
- **MediaRangeMapping usunięte:** zakresy typu nośnika są teraz obsługiwane przez domyślny moduł negocjowania zawartości.
- **Wiązanie parametru domyślne dla parametrów typu prostego jest teraz [FromUri]:** w poprzednich wersjach wiązanie parametrów domyślne dla parametrów typu prostego używanych wiązania modelu programu ASP.NET Web API. Wiązanie parametru domyślne dla parametrów typu prostego jest teraz *[FromUri]*.
- **Wybór akcji honoruje wymaganych parametrów:** wybór akcji w interfejsie API sieci Web platformy ASP.NET będzie teraz tylko wybierz akcję, jeśli znajdują się wszystkie wymagane parametry, które pochodzą z identyfikatora URI. Parametr można określić jako opcjonalną, podając wartości domyślnej dla argumentu w podpisie metody akcji.
- **Dostosowywanie wiązania parametru HTTP:** użyj *ParameterBindingAttribute* dostosować wiązanie parametru dla parametru określonej akcji lub użyć *ParameterBindingRules* na *HttpConfiguration* Dostosowywanie powiązania parametrów szerzej.
- **Ulepszenia element MediaTypeFormatter:** elementy formatujące mają teraz dostęp do pełnego *zawartość HttpContent* wystąpienia.
- **Wybieranie zasad buforowania hosta:** wdrożenie i skonfigurowanie *IHostBufferPolicySelector* usługi ASP.NET Web API umożliwiające hosty do określania zasad buforowania po ma być używany.
- **Uzyskiwanie dostępu do certyfikatów klienta, w sposób niezależny od hosta:** użyj *GetClientCertificate* metodę rozszerzenia, aby pobrać certyfikat klienta dostarczony poza komunikatem żądania.
- **Rozszerzalność negocjacji zawartości:** dostosować negocjowanie zawartości przez pochodząca od *DefaultContentNegotiator* i zastąpienie dowolnego aspektu negocjacje zawartości, które chcieliby.
- **Obsługę zwracania 406 nie do przyjęcia odpowiedzi:** może teraz zwrócić 406 nie do przyjęcia odpowiedzi w interfejsie API sieci Web platformy ASP.NET, gdy odpowiedni element formatujący nie zostanie znaleziony, tworząc *DefaultContentNegotiator* z *excludeMatchOnTypeOnly* parametr *true*.
- **Odczyt danych formularza jako elementu NameValueCollection lub JToken:** można odczytywać dane formularza w ciągu zapytania identyfikatora URI lub w treści żądania jako *elementu NameValueCollection* przy użyciu *ParseQueryString* i  *ReadAsFormDataAsync* metody rozszerzenia odpowiednio. Podobnie, można odczytać danych formularza, w ciągu zapytania identyfikatora URI lub w treści żądania jako *JToken* przy użyciu *TryReadQueryAsJson* i *ReadAsAsync*&lt;T&gt; metody rozszerzenia odpowiednio.
- **Ulepszenia wieloczęściowego:** jest teraz możliwość napisania *MultipartStreamProvider* całkowicie dostosowane do typu danych wieloczęściowej wiadomości MIME, że może odczytywać i przedstawi wynik w optymalny sposób do użytkownika. Można również dołączyć krok przetwarzania końcowego na *MultipartStreamProvider* umożliwiająca realizacji celu post, niezależnie od jego przetworzeniem chce na części treści wieloczęściowej wiadomości MIME. Na przykład *MultipartFormDataStreamProvider* implementacji odczytuje dane formularza HTML części danych i doda je do *elementu NameValueCollection* tak, aby były łatwe uzyskiwanie na obiekt wywołujący.
- **Link ulepszenia generowania:** *UrlHelper* już jest zależna od *HttpControllerContext*. Teraz uzyskiwać dostęp do *UrlHelper* z dowolnym kontekście gdzie *HttpRequestMessage* jest dostępna.
- **Zmienianie kolejności wykonywania procedury obsługi komunikatów:** programy obsługi komunikatów teraz są wykonywane w kolejności ich skonfigurowaniu zamiast w odwrotnej kolejności.
- **Pomocnik dla okablowania się programy obsługi komunikatów:** nowy *HttpClientFactory* , Podłączanie *DelegatingHandlers* i Utwórz *HttpClient* za pomocą żądany potok jest gotowa do użytku. Zapewnia także funkcjonalność okablowania się przy użyciu alternatywnych obsługi wewnętrznego (wartość domyślna to *HttpClientHandler*) również postępowaniu łącząc za pomocą *HttpMessageInvoker* lub inne  *DelegatingHandler* zamiast *HttpClient* jako źródło top.
- **Obsługa usługi CDN w optymalizacji sieci Web platformy ASP.NET:** optymalizacji sieci Web programu ASP.NET zapewnia wsparcie w zakresie alternatywnych ścieżek sieci CDN, dzięki któremu można określić dla każdego pakietu dodatkowy adres URL, który wskazuje ten sam zasób w sieci dostarczania zawartości. Obsługa sieci CDN umożliwia pobieranie Twojego skryptu i stylu pakiety geograficznie bliżej klientów końcowych aplikacji sieci Web. Aplikacje produkcyjne powinny implementować rezerwowe, gdy sieć CDN jest niedostępny. Testuj plan awaryjny.
- **Trasy ASP.NET Web API i konfiguracji przeniesione do *WebApiConfig.Register* statyczna metoda, która może być resused w kodzie testu.** Trasy ASP.NET Web API zostały wcześniej dodane w *RouteConfig.RegisterRoutes* wraz ze standardowego MVC przekierowuje. Domyślne trasy ASP.NET Web API i konfiguracji są teraz obsługiwane w osobnym *WebApiConfig.Register* metodę, aby ułatwić testowanie.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

- **Wersji RC i RTM programu ASP.NET MVC 4 niepoprawnie zwracane widoki pulpitu w pamięci podręcznej powinna zostać zwrócona mobilnych widoków.**

    - Zobacz [platformy ASP.NET MVC 4 Mobile buforowania usterki stały](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) szczegółowe informacje dotyczące poprawki. Można zainstalować poprawkę z [stały DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pakietu NuGet.
- **Istotne zmiany w aparatu widoku Razor**. Następujące typy zostały usunięte z *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

  Ponadto usunięto następujące metody: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **WebMatrix.WebData.dll znajduje się w katalogu/bin aplikacji ASP.NET MVC 4, zajmuje się za pośrednictwem adresu URL dla uwierzytelniania formularzy.** Trwa dodawanie zestawu WebMatrix.WebData.dll z aplikacją (np. przez wybranie "ASP.NET Web Pages o składni Razor", gdy za pomocą okna dialogowego Dodaj zależności do wdrożenia) spowoduje przesłonięcie przekierowania loginu uwierzytelnianie/konto/zalogować się zamiast / konta/logowania zgodnie z oczekiwaniami, domyślny kontroler konta programu ASP.NET MVC. Aby temu zapobiec i użyj adresu URL już określone w sekcji uwierzytelniania w pliku Web.config, możesz dodać element appSetting o nazwie PreserveLoginUrl i ustaw ją na wartość true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Menedżer pakietów NuGet nie zostanie zainstalowany podczas próby zainstalowania platformy ASP.NET MVC 4 dla równoległymi instalacjami programu Visual Studio 2010 i Visual Web Developer 2010.** Do uruchomienia programu Visual Studio 2010 i Visual Web Developer 2010 równolegle z programem ASP.NET MVC 4 należy zainstalować platformy ASP.NET MVC 4, gdy obie wersje programu Visual Studio ma już zainstalowany.
- **Odinstalowywanie programu ASP.NET MVC 4 kończy się niepowodzeniem, jeśli wstępnie wymagane składniki zostały już odinstalowane.** Aby prawidłowo odinstalować ASP.NET MVC 4you należy odinstalować ASP.NET MVC 4, przed rozpoczęciem odinstalowywania programu Visual Studio.
- **Instalowanie platformy ASP.NET MVC 4 spowoduje przerwanie aplikacji ASP.NET MVC 3 RTM.** Wydania aplikacji ASP.NET MVC 3, które zostały utworzone za pomocą wersji RTM (nie z [platformy ASP.NET MVC 3 Tools Update](https://www.microsoft.com/download/details.aspx?id=1491) release) wymagają następujących zmian do działania side-by-side przy użyciu platformy ASP.NET MVC 4. Tworzenie projektu bez wprowadzania tych wyników aktualizacji błędy kompilacji. 

    **Wymagane aktualizacje**

  1. W głównym pliku Web.config, Dodaj nowy *&lt;appSettings&gt;* wpis z kluczem *webPages:Version* i wartość *1.0.0.0*. 

      [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
  2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz pozycję Edytuj *ProjectName*csproj.
  3. Znajdź następujące odwołania do zestawów: 

      [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

      Zastąp je poniżej:

      [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
  4. Zapisz zmiany, zamknij plik projektu (.csproj), edytowania, a następnie kliknij prawym przyciskiem myszy projekt i wybierz pozycję Załaduj ponownie.

- **Zmienianie projektu programu ASP.NET MVC 4 do obiektu docelowego 4.0, 4.5 nie powoduje aktualizacji EntityFramework odwołanie do zestawu:** Jeśli zmienisz projekt programu ASP.NET MVC 4 do obiektu docelowego 4.0 po odwołujących 4.5, nadal będzie wskazywać odwołanie do zestawu platformy EntityFramework w wersji 4.5. Aby rozwiązać Odinstaluj ten problem i ponownie zainstaluj pakiet NuGet platformy EntityFramework.
- **403 Zabroniony podczas uruchamiania aplikacji ASP.NET MVC 4 na platformie Azure po przejściu do docelowego 4.0, 4.5:** Jeśli zmienisz projekt programu ASP.NET MVC 4 do obiektu docelowego 4.0 po odwołujących 4.5 a następnie wdrożyć na platformie Azure może zostać wyświetlony błąd 403 Zabroniony w czasie wykonywania. Aby obejść ten problem, dodaj następującą do Twojego pliku web.config: `<modules runAllManagedModulesForAllRequests="true" />`
- **Program Visual Studio 2012 ulega awarii podczas wpisywania "\' w literale ciągu w pliku Razor.** Aby pracować obejściu problemu cudzysłowu zamykającego literału ciągu najpierw wprowadź.
- <strong>Przechodzenie do &quot;konta/Zarządzanie&quot; w wynikach szablonu Internet w błąd w czasie wykonywania dla języków (CHS), TRK i (CHT).</strong> Aby rozwiązać ten problem, zmodyfikuj strony Aby rozdzielić <em>@User.Identity.Name</em> , umieszczając go jako jedynej zawartości w ramach <em>&lt;silne&gt;</em> tagu.
- **Google i LinkedIn nie są obsługiwani w usłudze Azure Web Sites.** Korzystanie z dostawców uwierzytelniania alternatywnych, podczas wdrażania witryny sieci Web systemu Azure.
- **Korzystając z usług IIS 8 Express/IIS UriPathExtensionMapping, otrzyma 404 Nie znaleziono błędy podczas próby użycia rozszerzenia.** Obsługa plików statycznych zakłóca żądań internetowych interfejsów API, używanego przez *UriPathExtensionMappings*. Ustaw *runAllManagedModulesForAllRequests = true* w pliku web.config, aby obejść ten problem.
- **Nie jest już wywoływana jest metoda Controller.Execute.** Wszystkich kontrolerów MVC są teraz zawsze wykonywane asynchronicznie.
