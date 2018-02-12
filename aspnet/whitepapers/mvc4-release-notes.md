---
uid: whitepapers/mvc4-release-notes
title: ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Ten dokument zawiera opis wersji platformy ASP.NET MVC 4.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: f014524f-25c0-4094-b8e1-886d99536f00
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-release-notes
msc.type: content
ms.openlocfilehash: bea6f6112388290a2c6b5ed267626ba28fc36671
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-mvc-4"></a>ASP.NET MVC 4
====================
> Ten dokument zawiera opis wersji platformy ASP.NET MVC 4.


- [Informacje o instalacji](#_Toc303253802)
- [Dokumentacja](#_Toc303253803)
- [Obsługa](#_Toc303253804)
- [Wymagania dotyczące oprogramowania](#_Toc303253805)
- [Nowe funkcje w programie ASP.NET MVC 4](#_Toc303253807)

    - [ASP.NET Web API](#_Toc317096197)
    - [Ulepszenia domyślnych szablonów projektu](#_Toc303253808)
    - [Szablon projektu przenośnych](#_Toc303253809)
    - [Wyświetlanie trybów](#_Toc303253810)
    - [jQuery Mobile, tym przełącznikiem widoku i zastępowanie przeglądarki](#_Toc303253811)
    - [Obsługa zadanie asynchroniczne kontrolerów](#_Toc303253813)
    - [Zestaw Azure SDK](#_Toc303253814)
    - [Migracja bazy danych](#_Toc303253818)
    - [Pusty szablon projektu](#_Toc303253819)
    - [Dodawanie kontrolera do dowolnego folderu projektu](#_Toc303253820)
    - [Tworzenie pakietów i minifikacja](#_Toc303253821)
    - [Włączanie logowania z usługi Facebook i innych lokacji za pomocą protokołu OAuth i OpenID](#_Toc303253822)
- [Uaktualnianie projektów programu ASP.NET MVC 3 do platformy ASP.NET MVC 4](#_Toc303253806)
- [Zmiany z platformy ASP.NET MVC 4 Release Candidate](#_Toc303253817)
- [Znane problemy i fundamentalne zmiany](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a>Informacje o instalacji

ASP.NET MVC 4 dla programu Visual Studio 2010 można zainstalować z [strona główna programu ASP.NET MVC 4](../mvc/mvc4.md) za pomocą Instalatora platformy sieci Web.

Zaleca się odinstalowanie żadnych wcześniej zainstalowanych podglądy ASP.NET MVC 4, przed zainstalowaniem programu ASP.NET MVC 4. ASP.NET MVC 4 w wersji Beta i wersji Release Candidate można uaktualnić do programu ASP.NET MVC 4 bez odinstalowania.

Ta wersja nie jest zgodny z wersjami dowolnej wersji zapoznawczej programu .NET Framework 4.5. Wszystkie wersje zainstalowanych wersji zapoznawczej programu .NET Framework 4.5 należy oddzielnie uaktualnić do wersji ostatecznej wersji przed zainstalowaniem programu ASP.NET MVC 4.

ASP.NET MVC 4 może być zainstalowany i uruchom side-by-side ASP.NET MVC 3 z.

<a id="_Toc303253803"></a>
## <a name="documentation"></a>Dokumentacja

Dokumentacja dla platformy ASP.NET MVC jest dostępna w witrynie MSDN pod adresem URL:

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

Samouczki i inne informacje o platformie ASP.NET MVC są dostępne na stronie witryny sieci Web platformy ASP.NET MVC 4 ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).

<a id="_Toc303253804"></a>
## <a name="support"></a>Obsługa

ASP.NET MVC 4 jest w pełni obsługiwany. Jeśli masz pytania dotyczące pracy z tej wersji można również zadawać je na platformie ASP.NET MVC forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), gdzie są często można zapewnić obsługę nieformalne członkami społeczności programu ASP.NET.

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a>Wymagania programowe

Składniki platformy ASP.NET MVC 4 dla programu Visual Studio wymaga środowiska PowerShell 2.0, a program Visual Studio 2010 z dodatkiem Service Pack 1 lub Visual Web Developer Express 2010 z dodatkiem Service Pack 1.

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4"></a>Nowe funkcje w programie ASP.NET MVC 4

W tej sekcji opisano funkcje, które zostały wprowadzone w wersji platformy ASP.NET MVC 4.

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a>ASP.NET Web API

Platforma ASP.NET MVC 4 zawiera ASP.NET Web API, nowe framework umożliwiający tworzenie usług HTTP, które może korzystać szeroka gama klientów, w tym przeglądarki i urządzenia przenośne. ASP.NET Web API również jest idealną platformą do tworzenia usługi RESTful.

Interfejs API sieci Web platformy ASP.NET obsługuje następujące funkcje:

- **Nowoczesne model programowania protokołu HTTP:** bezpośrednio uzyskać dostęp i manipulowania żądań i odpowiedzi w interfejsów API sieci Web przy użyciu nowego, jednoznacznie modelu obiektu HTTP HTTP. Taki sam programowania modelu i potoku HTTP jest symetrycznie dostępny na kliencie za pomocą nowej *HttpClient* typu.
- **Pełna obsługa tras:** interfejsu API sieci Web platformy ASP.NET obsługuje pełen zestaw możliwości routingu platformy ASP.NET, w tym parametrów trasy i ograniczenia trasy. Ponadto umożliwia proste konwencje mapowania akcji do metod HTTP.
- **Negocjowanie zawartości:** klient i serwer mogą współdziałać, aby określić nieprawidłowy format danych zwracanych z interfejsu API sieci web. ASP.NET Web API zapewnia obsługę XML, JSON, i formaty zakodowane w adresie URL formularza i można rozszerzyć tę obsługę, dodając własne elementy formatujące, lub nawet jej zastąpienie domyślnego strategii negocjacji zawartości.
- **Model powiązania i sprawdzania poprawności:** integratorów modeli z łatwością wyodrębniania danych z różnych części żądania HTTP i konwertowania tych części wiadomości na obiekty .NET, które mogą być używane przez akcje interfejsu API sieci Web. Weryfikacja odbywa się również na parametry akcji na adnotacje danych na podstawie.
- **Filtry:** ASP.NET Web API obsługuje filtry, tym dobrze znanego filtry takie jak *[Authorize]* atrybutu. Możesz tworzyć i dołączyć filtry dla działania, autoryzacji i obsługi wyjątków.
- **Tworzenia zapytania:** użyj *[Queryable]* atrybutu filtru akcji, która zwraca *IQueryable* włączyć obsługę zapytań interfejsu API sieci web za pomocą Konwencji zapytania OData.
- **Ulepszone pola:** zamiast ustawienie szczegóły HTTP w obiektach kontekstu statycznego sieci web interfejsu API pracy akcji wystąpieniami *HttpRequestMessage* i *HttpResponseMessage*. Utwórz jednostkowy projekt testowy, wraz z projektem interfejsu API sieci Web, aby rozpocząć szybko pisania testów jednostkowych dla funkcji z interfejsu API sieci Web.
- **Konfiguracja na podstawie kodu:** Konfiguracja interfejsu API sieci Web ASP.NET jest realizowane wyłącznie przy użyciu kodu, pozostawiając konfigurację czyszczenia plików. Umożliwia skonfigurowanie punktów rozszerzalności wzorzec lokalizatora świadczonych usług.
- **Ulepszona obsługa kontenerów Inwersja kontroli (IoC):** interfejsu API sieci Web platformy ASP.NET zapewnia doskonałą pomoc techniczną dla kontenerów Inwersja kontroli za pośrednictwem abstrakcję rozpoznawania zależności ulepszone
- **Host samodzielny:** interfejsów API sieci Web może być hostowana w procesie oprócz usług IIS podczas nadal przy użyciu pełną moc tras i inne funkcje interfejsu API sieci Web.
- **Tworzenie niestandardowych Pomoc i stron w teście:** teraz łatwo kompilacji niestandardowej pomocy i stron w teście sieci Web API przy użyciu nowej *IApiExplorer* usługi, aby uzyskać opis pełnej obsługi sieci web API.
- **Monitorowania i diagnostyki:** interfejsu API sieci Web platformy ASP.NET udostępnia teraz lekkie śledzenia infrastruktury, który można łatwo zintegrować z istniejącymi rozwiązaniami rejestrowania, takich jak System.Diagnostics, ETW i platform rejestrowania innych firm. Śledzenie można włączyć, podając *ITraceWriter* wdrożenia i dodać go do konfiguracji interfejsu API sieci web.
- **Link generowania:** za pomocą interfejsu API sieci Web ASP.NET *UrlHelper* do generowania łącza do powiązane zasoby w tej samej aplikacji.
- **Szablon projektu interfejsu API sieci Web:** wybierz nowy formularz projektu interfejsu API sieci Web Kreatora nowego projektu 4 MVC, aby szybko rozpocząć pracę z interfejsu API sieci Web platformy ASP.NET.
- **Funkcja szkieletów:** użyj **Dodaj kontroler** okna dialogowego, aby szybko utworzyć szkielet kontrolera interfejsu API sieci web oparta na platformie Entity Framework, na podstawie typu modelu.

Więcej informacji na temat interfejsu API sieci Web platformy ASP.NET można znaleźć pod adresem [https://www.asp.net/web-api](../web-api/index.md).

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a>Ulepszenia domyślnych szablonów projektu

Szablon, który służy do tworzenia nowych projektów programu ASP.NET MVC 4 została zaktualizowana w celu utworzenia bardziej nowoczesny wyglądające witryny sieci Web:

![](mvc4-release-notes/_static/image1.png)

Oprócz bardzo drobny ulepszenia ulepszono funkcje w nowym szablonie. Szablon zostają technika o nazwie adaptacyjną renderowania wyglądać zarówno w przypadku przeglądarek klasycznych, jak i przeglądarki dla urządzeń przenośnych bez potrzeby dostosowywania.

![](mvc4-release-notes/_static/image2.png)

Aby wyświetlić adaptacyjną renderowania w akcji, można użyć emulatorze przenośnym, lub podaj zmiany rozmiaru okna przeglądarki pulpitu na mniejszy. Gdy duże pobiera okna przeglądarki, zmieni się układ strony.

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a>Szablon projektu przenośnych

Jeśli zaczynasz nowego projektu i chcesz utworzyć witryny specjalnie dla urządzeń przenośnych i tablet przeglądarki, można użyć nowego szablonu projektu aplikacji mobilnej. To jest oparty na technologii jQuery Mobile, biblioteka open source do tworzenia zoptymalizowanych pod kątem touch interfejsu użytkownika:

![](mvc4-release-notes/_static/image3.png)

Ten szablon zawiera tę samą strukturę aplikacji jako szablonu aplikacji internetowej (i kodu kontrolera jest praktycznie identyczny), ale jest stylem wygląd i działają prawidłowo na podstawie touch urządzeniach przenośnych przy użyciu jQuery Mobile. Aby dowiedzieć się więcej na temat struktury i stylem przenośnych interfejsu użytkownika, zobacz [jQuery przenośnych projektu witryny sieci Web](http://jquerymobile.com/).

Jeśli masz już zorientowane na pulpicie lokacji, czy chcesz dodać widoków zoptymalizowanych pod kątem mobile, lub jeśli chcesz utworzyć jedną lokację służy inaczej styl widoków do komputerów stacjonarnych i przenośnych przeglądarek, można użyć nowej funkcji trybów wyświetlania. (Zobacz następną sekcję).

<a id="_Toc303253810"></a>
### <a name="display-modes"></a>Wyświetlanie trybów

Nowa funkcja trybów wyświetlania umożliwia aplikacji wybierz widoki, w zależności od przeglądarki, do której wysłano żądanie. Na przykład jeśli przeglądarka pulpitu zażąda strony głównej, aplikacja może szablon Views\Home\Index.cshtml. Jeśli przeglądarkę dla telefonów zażąda strony głównej, aplikacja może zwrócić Views\Home\Index.mobile.cshtml szablonu.

Układy i częściowe może także być zmieniona dla typów przeglądarki. Na przykład:

- Jeśli Views\Shared folder zawiera zarówno \_Layout.cshtml i \_Layout.mobile.cshtml szablony, domyślnie aplikacja będzie używać \_Layout.mobile.cshtml podczas żądania od przeglądarki dla urządzeń przenośnych i \_Layout.cshtml podczas innych żądań.
- Jeśli folder zawiera zarówno \_MyPartial.cshtml i \_MyPartial.mobile.cshtml, instrukcja @Html.Partial("\_MyPartial") będzie renderować \_MyPartial.mobile.cshtml podczas żądania od mobile przeglądarki, a \_MyPartial.cshtml podczas innych żądań.

Jeśli chcesz utworzyć bardziej szczegółowych widoków, układów lub widoki częściowe przez inne urządzenia, możesz zarejestrować nową *DefaultDisplayMode* wystąpienia, aby określić, którego nazwa do wyszukiwania, gdy żądanie spełnia warunki określonej. Można na przykład, Dodaj następujący kod, aby *aplikacji\_Start* metody w pliku Global.asax, aby zarejestrować ciąg "iPhone" jako tryb wyświetlania, która ma zastosowanie, gdy przeglądarka Apple iPhone wysyła żądanie:

[!code-csharp[Main](mvc4-release-notes/samples/sample1.cs)]

Po uruchomieniu tego kodu, jeśli Apple iPhone przeglądarki wysyła żądanie, aplikacja będzie korzystać z Views\Shared\\układu _Layout.iPhone.cshtml (jeśli istnieje). Aby uzyskać więcej informacji dotyczących trybu wyświetlania, zobacz [ASP.NET MVC 4 Mobile funkcji](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md). Należy instalować aplikacji za pomocą DisplayModeProvider [stałym DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pakietu NuGet. [ASP.NET spadek 2012 Update](https://go.microsoft.com/fwlink/?LinkID=271322) obejmuje [stałym DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pakietu NuGet w nowych szablonów projektu. Zobacz [Fixedd buforowanie błędów platformy ASP.NET MVC 4 Mobile](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) szczegółowe informacje na temat poprawki.

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-and-mobile-features"></a>jQuery Mobile i funkcje Mobile

Aby uzyskać informacje dotyczące tworzenia aplikacji dla urządzeń przenośnych z platformy ASP.NET MVC 4 przy użyciu jQuery Mobile, zobacz samouczek [ASP.NET MVC 4 Mobile funkcji](../mvc/overview/older-versions/aspnet-mvc-4-mobile-features.md).

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a>Obsługa zadanie asynchroniczne kontrolerów

Możesz teraz zapisać metod asynchronicznych akcji w jednej metody, które zwracają obiekt typu *zadań* lub *zadań&lt;ActionResult&gt;*.

 Aby uzyskać więcej informacji, zobacz [przy użyciu metod asynchronicznych w technologii ASP.NET MVC 4](../mvc/overview/performance/using-asynchronous-methods-in-aspnet-mvc-4.md).

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a>Azure SDK

ASP.NET MVC 4 obsługuje 1,6 i w nowszych wersjach zestawu SDK platformy Windows Azure.

<a id="_Toc303253818"></a>
### <a name="database-migrations"></a>Migracja bazy danych

Projekty składnika ASP.NET MVC 4 zawierają teraz Entity Framework 5. Jednym z najważniejszych funkcji w programie Entity Framework 5 jest obsługiwane dla migracji w bazie danych. Ta funkcja pozwala łatwo rozwijać schemat bazy danych przy użyciu migracji fokus kodu przy zachowaniu danych w bazie danych. Aby uzyskać więcej informacji o migracji bazy danych, zobacz [dodanie nowego pola filmu modelu i tabeli](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-a-new-field-to-the-movie-model-and-table.md) w [wprowadzenie do platformy ASP.NET MVC 4 samouczka](../mvc/overview/older-versions/getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).

<a id="_Toc303253819"></a>
### <a name="empty-project-template"></a>Pusty szablon projektu

MVC pusty szablon projektu jest teraz naprawdę pusta, dzięki czemu można rozpocząć od pustego całkowicie. Starszą wersję pusty szablon projektu została zmieniona na podstawowe.

<a id="_Toc303253820"></a>
### <a name="add-controller-to-any-project-folder"></a>Dodawanie kontrolera do dowolnego folderu projektu

Teraz kliknij prawym przyciskiem myszy i wybierz **Dodaj kontroler** z dowolnego folderu w projekcie platformy MVC. Zapewnia większą elastyczność i możliwość organizowania kontrolerach w dowolny sposób, w tym zachowaniu kontrolerów MVC i interfejsu API sieci Web w oddzielnych folderach.

<a id="_Toc303253821"></a>
### <a name="bundling-and-minification"></a>Tworzenie pakietów i minimalizowanie

Tworzenie pakietów i minimalizowanie framework pozwala zmniejszyć liczbę żądań HTTP, które strony sieci Web musi wprowadzić przez połączenie poszczególnych plików w jednej, powiązane plików skryptów i CSS. Można następnie zmniejszyć całkowity rozmiar te żądania przez zminimalizowania liczby zawartości pakietu. Zminimalizowania liczby mogą zawierać działań, takich jak wyeliminowanie odstępy na skrócenie nazw zmiennych nawet zwijanie selektory CSS oparte na ich semantyki. Pakiety są zadeklarowane i skonfigurowane w kodu i są łatwo lub odwołuje się w widokach za pośrednictwem metody pomocnicze, które mogą generować albo jedno łącze do pakietu, podczas debugowania, wiele łączy do poszczególnych zawartości pakietu. Aby uzyskać więcej informacji, zobacz [tworzenie pakietów i minimalizowanie](../mvc/overview/performance/bundling-and-minification.md).

<a id="_Toc303253822"></a>
### <a name="enabling-logins-from-facebook-and-other-sites-using-oauth-and-openid"></a>Włączanie logowania z usługi Facebook i innych lokacji za pomocą protokołu OAuth i OpenID

Domyślne szablony ASP.NET MVC 4 Internet projektu szablonu teraz obsługuje logowania OAuth i OpenID za pomocą Biblioteka DotNetOpenAuth. Aby uzyskać informacje dotyczące konfigurowania dostawcy uwierzytelniania OAuth lub OpenID, zobacz [obsługę uwierzytelniania OAuth/OpenID formularzy sieci Web, MVC i stron sieci Web](https://blogs.msdn.com/b/webdev/archive/2012/08/15/oauth-openid-support-for-webforms-mvc-and-webpages.aspx) i [OAuth i OpenID funkcji dokumentacji stron ASP.NET Web Pages](../web-pages/overview/releases/top-features-in-web-pages-2.md#oauthsetup).

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a>Uaktualnianie projektów programu ASP.NET MVC 3 do platformy ASP.NET MVC 4

ASP.NET MVC 4 można zainstalować równolegle program ASP.NET MVC 3 na tym samym komputerze, który zapewnia elastyczność w wyborze, gdy do uaktualniania aplikacji ASP.NET MVC 3 do platformy ASP.NET MVC 4.

Najprostszym sposobem uaktualnienia jest aby utworzyć nowy projekt ASP.NET MVC 4 i skopiuj wszystkie widoki, kontrolery, kodu i zawartość plików z istniejącego projektu MVC 3 do nowego projektu i zaktualizować zestaw odwołuje się w nowym projekcie odpowiadające dowolnego szablonu MVC z systemem innym niż w assembiles dołączone, którego używasz. Jeśli wprowadzono zmiany w pliku Web.config w projekcie MVC 3, musi również scalić te zmiany w pliku Web.config w projekcie MVC 4.

Aby ręcznie uaktualnić istniejącą aplikację ASP.NET MVC 3 do wersji 4, wykonaj następujące czynności:

1. W pliku Web.config wszystkie pliki w projekcie (istnieje w folderze głównym projektu, w folderze Widoki i w folderze widoków dla każdego obszaru projektu), Zastąp każde wystąpienie następujący tekst (Uwaga: System.Web.WebPages, Version = 1.0.0.0 nie został znaleziony w projektów utworzonych za pomocą programu Visual Studio 2012): 

    [!code-console[Main](mvc4-release-notes/samples/sample2.cmd)]

    z odpowiedniego następujący tekst:

    [!code-console[Main](mvc4-release-notes/samples/sample3.cmd)]
2. W głównym pliku Web.config, zaktualizuj *webPages:Version* elementu "2.0.0.0" i Dodaj nową *PreserveLoginUrl* klucz, który ma wartość "true": 

    [!code-xml[Main](mvc4-release-notes/samples/sample4.xml)]
3. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy na odwołań i wybierz opcję Zarządzaj pakietami NuGet. W okienku po lewej stronie wybierz **źródła pakietu oficjalnego Online\NuGet**, następnie zaktualizuj następujące:

    - ASP.NET MVC 4
    - (Opcjonalnie) jQuery, jQuery sprawdzania poprawności i jQuery interfejsu użytkownika
    - (Opcjonalnie) Entity Framework
    - (Optonal) Modernizr
4. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz polecenie Edytuj *ProjectName*.csproj.
5. Zlokalizuj *ProjectTypeGuids* elementu i zastępowanie {E53F8FEA-EAE0-44A6-8774-FFD645390401} z {E3E379DF-F4C6-4180-9B81-6769533ABE47}.
6. Zapisz zmiany, zamknij plik projektu (.csproj), który edytowania, kliknij prawym przyciskiem myszy projekt, a następnie wybierz ponowne załadowanie projektu.
7. Jeśli projekt odwołuje się do żadnych bibliotek innych firm, które są kompilowane przy użyciu poprzedniej wersji platformy ASP.NET MVC, otwórz głównego pliku Web.config i dodaj następujące trzy *bindingRedirect* elementy w obszarze  *Konfiguracja* sekcji: 

    [!code-xml[Main](mvc4-release-notes/samples/sample5.xml)]

<a id="_Toc303253817"></a>
## <a name="changes-from-aspnet-mvc-4-release-candidate"></a>Zmiany z platformy ASP.NET MVC 4 Release Candidate

Informacje o wersji programu ASP.NET MVC 4 Release Candidate można znaleźć tutaj:

Poniżej przedstawiono istotne zmiany z platformy ASP.NET MVC 4 Release Candidate w tej wersji:

- **Na konfiguracji kontrolera:** można przypisać kontrolery interfejsu API sieci Web platformy ASP.NET z atrybutu niestandardowego, który implementuje *IControllerConfiguration* można skonfigurować własne elementy formatujące, selektor akcji i integratorów parametru . *HttpControllerConfigurationAttribute* został usunięty.
- **Na programy obsługi wiadomości trasy:** program obsługi komunikatów końcowe można teraz określić w łańcuchu żądanie dla danej trasy. Umożliwia to obsługę jazdy wzdłuż struktur korzystać z routingu do wysłania do ich własnych (z systemem innym niż*IHttpController*) punktów końcowych.
- **Postęp powiadomienia:** *ProgressMessageHandler* generuje powiadomienie o postępie dla przekazywanych jednostek żądania i pobieranych jednostek odpowiedzi. Przy użyciu tej procedury obsługi jest możliwe do śledzenia ile przekazywania treści żądania lub pobieranie treści odpowiedzi.
- **Wypychania zawartości:** *PushStreamContent* klasa umożliwia obsługę scenariuszy, w których producent danych chce dokonywać zapisu bezpośrednio do żądania lub odpowiedzi (synchronicznie lub asynchronicznie) przy użyciu strumienia. Gdy *PushStreamContent* jest gotowa do akceptowania danych wywołuje do delegata działania z strumienia wyjściowego. Deweloper może następnie zapisać do strumienia tak długo, jak konieczne i zamknij strumienia podczas zapisywania zostało ukończone. *PushStreamContent* wykrywa zamknięcie strumienia i zakończeniu asynchronicznego odpowiadającego *zadań* dla wypisywanie zawartości.
- **Tworzenie odpowiedzi na błędy:** użyj *HttpError* typu do reprezentowania spójnie informacje o błędzie z takich jak błędy sprawdzania poprawności i wyjątków, przy jednoczesnym zachowaniu nadal *IncludeErrorDetailPolicy* . Użyj nowych *CreateErrorResponse* metody rozszerzenia umożliwiające łatwe tworzenie odpowiedzi z *HttpError* jako zawartości. *HttpError* zawartości jest w pełni zawartości negocjowane.
- **Usunięte MediaRangeMapping:** zakresy typu nośnika są teraz obsługiwane przez domyślny moduł negocjowania zawartości.
- **Wiązanie parametru domyślne dla parametrów typu prostego jest teraz [FromUri]:** w poprzednich wersjach programu ASP.NET Web API powiązanie parametru domyślne dla parametrów typu prostego wiązania modelu. Wiązanie parametru domyślne dla parametrów typu prostego jest teraz *[FromUri]*.
- **Wybór działania honoruje wymaganych parametrów:** wybór akcji w interfejsie API sieci Web ASP.NET będzie teraz tylko wybierz akcję, jeśli znajdują się wszystkie wymagane parametry, które pochodzą z identyfikatora URI. Parametr można określić jako opcjonalną, podając wartość domyślna argumentu w podpisie metody akcji.
- **Dostosuj powiązania parametrów HTTP:** użyj *ParameterBindingAttribute* dostosować wiązanie parametru dla parametru określonej akcji lub korzystania z *ParameterBindingRules* na *HttpConfiguration* dostosować powiązania parametrów szerzej.
- **Ulepszenia MediaTypeFormatter:** elementy formatujące mają teraz dostęp do pełnego *HttpContent* wystąpienia.
- **Wybieranie zasad buforowania hosta:** wdrożenie i skonfigurowanie *IHostBufferPolicySelector* w ASP.NET Web API, aby umożliwić hostom określania zasad Jeśli buforowanie ma być używany.
- **Dostępu do certyfikatów klienta w sposób niezależny od hosta:** użyj *GetClientCertificate* — metoda rozszerzenia, aby uzyskać dostarczonego certyfikatu klienta z komunikatu żądania.
- **Rozszerzalność negocjowanie zawartości:** dostosowanie negocjowanie zawartości przez pochodny *DefaultContentNegotiator* i zastępowanie dowolnego aspektu negocjacje zawartości, którą chcesz.
- **Obsługę zwracania 406 nie do przyjęcia odpowiedzi:** można teraz wróć 406 nie do przyjęcia odpowiedzi w interfejsie API sieci Web ASP.NET, gdy nie znaleziono odpowiedniego elementu formatującego, tworząc *DefaultContentNegotiator* z *excludeMatchOnTypeOnly* ustawiona *true*.
- **Odczytanie danych formularza jako NameValueCollection lub JToken:** może odczytywać dane formularza w ciągu zapytania identyfikatora URI lub w treści żądania jako *NameValueCollection* przy użyciu *ParseQueryString* i  *ReadAsFormDataAsync* metody rozszerzenia odpowiednio. Podobnie można odczytać danych formularza w ciągu zapytania identyfikatora URI lub w treści żądania jako *JToken* przy użyciu *TryReadQueryAsJson* i *ReadAsAsync*&lt;T&gt; metody rozszerzenia odpowiednio.
- **Ulepszenia wieloczęściowego:** obecnie istnieje możliwość zapisu *MultipartStreamProvider* całkowicie dostosowane do typu danych wieloczęściowej wiadomości MIME może go odczytywać i przedstawia wynik w optymalny sposób dla użytkownika. Można również Podłącz krok przetwarzania końcowego na *MultipartStreamProvider* umożliwiająca realizacji celu post, niezależnie od jego przetworzeniem chce części treści wieloczęściowej wiadomości MIME. Na przykład *MultipartFormDataStreamProvider* implementacji odczytuje dane formularza HTML części danych i dodaje je do *NameValueCollection* tak, aby były łatwe do pobrania w wywołujący.
- **Ulepszenia generowania łącza:** *UrlHelper* zależy od już *HttpControllerContext*. Teraz uzyskiwać dostęp do *UrlHelper* z dowolnym kontekście gdzie *HttpRequestMessage* jest dostępna.
- **Zmień kolejność wykonywania obsługi komunikatów:** programy obsługi wiadomości, teraz są wykonywane w kolejności, które są skonfigurowane zamiast w odwrotnej kolejności.
- **Pomocnik dla okablowania się programy obsługi wiadomości:** nowy *HttpClientFactory* który można okablować się *DelegatingHandlers* i Utwórz *HttpClient* z żądany potoku rozpocząć pracę. Zapewnia także funkcjonalność okablowania się z alternatywnych obsługi wewnętrzny (wartość domyślna to *HttpClientHandler*) również wykonać okablowania zapasową danych, korzystając z *HttpMessageInvoker* lub innym  *DelegatingHandler* zamiast *HttpClient* jako element wywołujący top.
- **Obsługa CDN optymalizację sieci Web platformy ASP.NET:** optymalizacji sieci Web programu ASP.NET zapewnia wsparcie w zakresie alternatywnych ścieżek CDN, dzięki któremu można określić dla każdego pakietu dodatkowy adres URL wskazujący na tego samego zasobu w sieci dostarczania zawartości. Obsługa CDN pozwala uzyskać Twojego skryptu i stylu pakiety geograficznie bliżej do użytkowników końcowych w aplikacji sieci Web.
- **Kieruje Web API platformy ASP.NET i konfiguracji przeniesiona do *WebApiConfig.Register* statyczną metodę, która może być resused w kodzie testu.** ASP.NET Web API trasy zostały wcześniej dodane w *RouteConfig.RegisterRoutes* wraz ze standardowych MVC trasy. Domyślnym kieruje interfejsu API sieci Web platformy ASP.NET i konfiguracji są teraz obsługiwane w oddzielnej *WebApiConfig.Register* metodę, aby ułatwić testowanie.

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a>Znane problemy i fundamentalne zmiany

- **Wersja RC i RTM programu ASP.NET MVC 4 niepoprawnie zwracane widoki pulpitu w pamięci podręcznej widokach dla urządzeń przenośnych, które ma zostać zwrócony.**

    - Zobacz [ASP.NET MVC 4 Mobile buforowanie usterki stałym](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) szczegółowe informacje na temat poprawki. Można zainstalować poprawkę z [stałym DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) pakietu NuGet.
- **Fundamentalne zmiany w aparatu widoku Razor**. Następujące typy zostały usunięte z *System.Web.Mvc.Razor*: 

    - *ModelSpan*
    - *MvcVBRazorCodeGenerator*
    - *MvcCSharpRazorCodeGenerator*
    - *MvcVBRazorCodeParser*

 Ponadto usunięto następujących metod: 

    - *MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
    - *MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*
    - *MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*
- **Gdy WebMatrix.WebData.dll znajduje się w katalogu/bin aplikacji ASP.NET MVC 4, przejmuje adres URL uwierzytelniania formularzy.** Dodawanie zestawu WebMatrix.WebData.dll do swojej aplikacji (na przykład, wybierając "ASP.NET Web Pages o składni Razor" przy użyciu okna dialogowego Dodaj zależności do wdrożenia) zastąpią przekierowania logowania uwierzytelniania do logowania/account/zamiast / / logowanie się na koncie zgodnie z oczekiwaniami domyślnie ASP.NET MVC konta kontrolera. Aby temu zapobiec i użyć adres URL już określony w sekcji uwierzytelniania w pliku Web.config, możesz dodać appSetting o nazwie PreserveLoginUrl i ustaw ją na wartość true: 

    [!code-xml[Main](mvc4-release-notes/samples/sample6.xml)]
- **Menedżer pakietów NuGet nie zostanie zainstalowany podczas próby zainstalowania programu ASP.NET MVC 4 dla siebie instalacji programu Visual Studio 2010 oraz Visual Web Developer 2010.** Do uruchomienia programu Visual Studio 2010 i Visual Web Developer 2010 równolegle z platformy ASP.NET MVC 4 należy zainstalować program ASP.NET MVC 4 po obie wersje programu Visual Studio zostały już zainstalowane.
- **Odinstalowywanie platformy ASP.NET MVC 4 kończy się niepowodzeniem, jeśli wymagania wstępne zostały już odinstalowane.** Aby odinstalować prawidłowo ASP.NET MVC 4you odinstalować ASP.NET MVC 4 przed odinstalowaniem programu Visual Studio.
- **Instalowanie platformy ASP.NET MVC 4 dzieli RTM programu ASP.NET MVC 3 aplikacji.** Wersji aplikacji ASP.NET MVC 3, które zostały utworzone za pomocą RTM (nie z [aktualizacji narzędzi programu ASP.NET MVC 3](https://www.microsoft.com/download/details.aspx?id=1491) release) wymagają następujących zmian do side-by-side z platformy ASP.NET MVC 4. Tworzenie projektu bez wprowadzania tych wyników aktualizacje w błędy kompilacji. 

    **Wymagane aktualizacje**

    1. W głównym pliku Web.config, Dodaj nową  *&lt;appSettings&gt;*  wpisu z kluczem *webPages:Version* i wartość *1.0.0.0*. 

        [!code-xml[Main](mvc4-release-notes/samples/sample7.xml)]
    2. W Eksploratorze rozwiązań kliknij prawym przyciskiem myszy nazwę projektu, a następnie wybierz Zwolnij projekt. Następnie ponownie kliknij prawym przyciskiem myszy nazwę i wybierz polecenie Edytuj *ProjectName*.csproj.
    3. Znajdź następujące odwołania do zestawu: 

        [!code-xml[Main](mvc4-release-notes/samples/sample8.xml)]

        Zastąp je z następujących czynności:

        [!code-xml[Main](mvc4-release-notes/samples/sample9.xml)]
    4. Zapisz zmiany, zamknij plik projektu (.csproj), edytowania, a następnie kliknij prawym przyciskiem myszy projekt i wybierz pozycję Załaduj ponownie.
- **Zmiana projektu programu ASP.NET MVC 4 do docelowego 4.0, 4.5 nie zaktualizować odwołania do zestawu EntityFramework:** Jeżeli zmienisz projekt platformy ASP.NET MVC 4 do docelowego 4.0 po dotyczących 4.5 nadal będzie wskazywać odwołanie do zestawu EntityFramework w wersji 4.5. Aby rozwiązać ten problem Odinstaluj i ponownie zainstaluj pakiet EntityFramework NuGet.
- **403 Zabroniony podczas uruchamiania aplikacji ASP.NET MVC 4 na platformie Azure po zmianie do docelowego 4.0, 4.5:** Jeśli możesz zmienić projekt platformy ASP.NET MVC 4 do docelowego 4.0 po dotyczących 4.5, a następnie wdrożyć na platformie Azure może zostać wyświetlony błąd 403 Zabroniony w czasie wykonywania. Aby obejść ten problem, Dodaj następujący kod do pliku web.config:`<modules runAllManagedModulesForAllRequests="true" />`
- **Program Visual Studio 2012 ulega awarii podczas wpisywania "\' w literale ciągu w pliku Razor.** Aby pracować wokół problem wprowadź najpierw zamykającym literału ciągu.
- **Przeglądanie do &quot;konta/Zarządzaj&quot; w wynikach szablonu Internet błąd środowiska uruchomieniowego dla języków, CHS, TRK i (CHT).** Aby rozwiązać ten problem, zmodyfikuj stronę, aby rozdzielić  *@User.Identity.Name*  ustawiając dla niego jako zawartości tylko w ramach  *&lt;silne&gt;*  tagu.
- **Dostawcy Google i LinkedIn nie są obsługiwane w systemie Azure Web Sites.** Podczas wdrażania do witryn sieci Web platformy Azure, należy użyć dostawcy uwierzytelniania alternatywnych.
- **Korzystając z programu IIS 8 Express/IIS UriPathExtensionMapping, otrzyma 404 Nie znaleziono błędy podczas próby użycia rozszerzenia.** Obsługa plików statycznych zakłóca żądań sieci Web API używanego *UriPathExtensionMappings*. Ustaw *runAllManagedModulesForAllRequests = true* w pliku web.config, aby obejść ten problem.
- **Metoda Controller.Execute nie jest wywoływana.** Wszystkich kontrolerów MVC są teraz zawsze wykonywane asynchronicznie.
