---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Funkcje mobilne platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: Teraz jest wersja MVC 5, po ukończeniu tego samouczka przy użyciu przykładów kodu w zasięgu Wdróż aplikację ASP.NET MVC 5 Mobile sieci Web w witrynach sieci Web platformy Azure.
ms.author: aspnetcontent
ms.date: 08/15/2012
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: c852f4a853d14badb6c9a1c2c1ddb7b069bc3441
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806590"
---
<a name="aspnet-mvc-4-mobile-features"></a>Funkcje mobilne platformy ASP.NET MVC 4
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Teraz jest dostępna wersja MVC 5 tego samouczka przy użyciu przykładów kodu w zasięgu [wdrożyć aplikację ASP.NET MVC 5 Mobile sieci Web w usłudze Azure Web Sites](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


Ta seria samouczków obejmuje podstawowe informacje dotyczące korzystania z funkcji mobilnych aplikacji sieci Web programu ASP.NET MVC 4. W tym samouczku użyjesz [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) lub Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer lub VWD&quot;). Jeśli masz już, można użyć programu Visual Studio w wersji professional.

Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagań wstępnych wymienionych poniżej.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (zalecane) lub Visual Studio Web Developer Express z dodatkiem SP1. Program Visual Studio 2012 zawiera ASP.NET MVC 4. Jeśli używasz programu Visual Web Developer 2010 należy zainstalować [platformy ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Należy również emulatora przeglądarce dla urządzeń przenośnych. Działają dowolne z następujących czynności:

- [Windows 7, Emulator telefonu](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Jest to emulator, który jest używany w większości zrzutów ekranu w ramach tego samouczka).
- Zmień ciąg agenta użytkownika w celu emulacji dla telefonu iPhone. Zobacz [to](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) wpis w blogu.
- [Opera Mobile Emulator](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) za pomocą agenta użytkownika, ustaw na telefonie iPhone. Aby uzyskać instrukcje na temat sposobu ustawiania agenta użytkownika w programie Safari na "iPhone", zobacz [jak umożliwić Safari poudawać jest IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) na blogu David Alison.

Projektów programu Visual Studio z kodem źródłowym języka C# są dostępne powiązany z tym tematem:

- [Pobieranie projektu startowego](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Ukończono projektem do pobrania](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Jakie będziesz tworzyć

W tym samouczku dodasz Funkcje mobilne na prostej aplikacji listy konferencji, który znajduje się w [projekt startowy](https://go.microsoft.com/fwlink/?LinkId=228307). Poniższy zrzut ekranu przedstawia stronę tagów w ukończonej aplikacji, jak pokazano w [Windows 7 Phone Emulator](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Zobacz [klawiatury mapowania dla Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) ułatwiają wprowadzanie z klawiatury.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Można użyć programu Internet Explorer w wersji 9 lub 10, FireFox i Chrome do tworzenia aplikacji mobilnych, ustawiając [ciąg agenta użytkownika](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). Na poniższej ilustracji przedstawiono ukończone samouczku, korzystając z programu Internet Explorer emulowanie dla telefonu iPhone. Możesz użyć narzędzi deweloperskich programu Internet Explorer F-12 i [narzędzie Fiddler](http://www.fiddler2.com/fiddler2/) aby pomóc w debugowaniu aplikacji.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Umiejętności, których dowiesz się

Oto, dowiesz się:

- Jak szablony platformy ASP.NET MVC 4 używają HTML5 `viewport` atrybut i renderowanie adaptacyjne w celu wyświetlania na urządzeniach przenośnych.
- Jak tworzyć widoki specyficzne dla mobilnych.
- Jak utworzyć ten przełącznik użytkowników umożliwia między widokiem mobilnych i klasycznych aplikacji przełącznikiem widoku.

### <a name="getting-started"></a>Wprowadzenie

Pobierz aplikację listy konferencji na projekt startowy, korzystając z następującego linku: [Pobierz](https://go.microsoft.com/fwlink/?LinkId=228307). Następnie w Eksploratorze Windows, kliknij prawym przyciskiem myszy *MvcMobile.zip* pliku, a następnie wybierz **właściwości**. W **właściwości MvcMobile.zip** okna dialogowego wybierz **odblokowanie** przycisku. (Odblokowywania zapobiega ostrzeżenie zabezpieczeń, który występuje podczas próby użycia *zip* plik pobrany z sieci web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Kliknij prawym przyciskiem myszy *MvcMobile.zip* plik i wybierz **Wyodrębnij wszystkie** aby rozpakować plik. W programie Visual Studio, otwórz *MvcMobile.sln* pliku.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, która zostanie ona wyświetlona w przeglądarce pulpitu. Uruchom emulator w swojej przeglądarce dla urządzeń przenośnych, skopiuj adres URL dla aplikacji konferencji do emulatora, a następnie kliknij **Przeglądaj według znaczników** łącza. Jeśli używasz emulatora Windows Phone, kliknij na pasku adresu URL i naciśnij klawisz Wstrzymaj, aby uzyskać dostęp za pomocą klawiatury. Obraz poniżej przedstawia *alltags —* widoku (wybór **Przeglądaj według znaczników**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Ekran jest bardzo czytelny na urządzeniu przenośnym. Wybierz łącze programu ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

Widok znaczników ASP.NET jest bardzo dużej liczby. Na przykład **data** kolumna jest bardzo trudne do odczytania. W dalszej części tego samouczka utworzysz wersję *alltags —* widoku dotyczy w szczególności przeglądarki dla urządzeń przenośnych i będzie odczytywać wyświetlania.

Uwaga: Obecnie usterkę w istnieje przenośnych buforowania. W przypadku aplikacji produkcyjnych należy zainstalować [stały DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nugget pakietu. Zobacz [platformy ASP.NET MVC 4 Mobile buforowania usterki stały](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) szczegółowe informacje dotyczące poprawki.

## <a name="css-media-queries"></a>Zapytaniami multimediów CSS

[Zapytaniami multimediów CSS](http://www.w3.org/TR/css3-mediaqueries/) stanowią rozszerzenie do arkusza CSS dla typów nośników. Umożliwiają one tworzenie reguł, które zastępują domyślne reguły CSS dla określonych przeglądarek (agentów użytkownika). Typowe reguły CSS, który jest przeznaczony dla przeglądarki dla urządzeń przenośnych jest zdefiniowanie maksymalnego rozmiaru. *Content\Site.css* plik, który jest tworzony podczas tworzenia nowego projektu ASP.NET MVC 4 Internet zawiera następujące zapytanie o multimedia:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Jeśli okno przeglądarki jest 850 pikseli szerokości, użyje reguły CSS wewnątrz bloku tego nośnika. CSS media kwerend, takich jak ta umożliwia zapewnia lepsze wyświetlanie zawartości HTML w małych przeglądarek (na przykład w przeglądarkach dla urządzeń przenośnych) niż reguły CSS domyślne, które są przeznaczone dla szerszego Wyświetla przeglądarek komputerowych.

## <a name="the-viewport-meta-tag"></a>Tag Meta okienka ekranu

Przeglądarki dla większości urządzeń przenośnych zdefiniować szerokość okna przeglądarki wirtualnej ( *okienka ekranu*) jest znacznie większa niż rzeczywista szerokość urządzenia przenośnego. Dzięki temu przeglądarki dla urządzeń przenośnych do rozmiaru całej strony sieci web wewnątrz wirtualnych wyświetlania. Użytkownicy mogą następnie powiększ interesującej zawartości. Jednak jeśli ustawisz szerokość okienka ekranu do szerokości rzeczywistego urządzenia nie powiększania jest wymagane, ponieważ zawartość mieści się w przeglądarce dla urządzeń przenośnych.

Okienko ekranu `<meta>` znacznika w pliku układu platformy ASP.NET MVC 4 Ustawia szerokość urządzenia okienka ekranu. Następujący wiersz zawiera okienka ekranu `<meta>` znacznika w pliku układu platformy ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Badanie wpływu zapytaniami multimediów CSS i metatag okienka ekranu

Otwórz *Views\Shared\\_Layout.cshtml* plik w edytorze i komentarz dla okienka ekranu `<meta>` tagu. Poniższy kod przedstawia zakomentowany wiersz.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Otwórz *MvcMobile\Content\Site.css* plik w edytorze, a następnie zmień maksymalną szerokość w zapytanie o multimedia na zero pikseli. Uniemożliwi to reguły CSS używany w przeglądarkach dla urządzeń przenośnych. Następujący wiersz zawiera zapytanie o multimedia zmodyfikowane:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Zapisz zmiany i przejdź do aplikacji konferencji w emulatorze przeglądarce dla urządzeń przenośnych. Niewielki rozmiar tekstu na poniższej ilustracji jest wynikiem usunięcia okienka ekranu `<meta>` tagu. Z nie okienka ekranu `<meta>` tagu, przeglądarka oddalając jest domyślną szerokość okienka ekranu (850 pikseli lub szerokość dla najbardziej mobilnych przeglądarek.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Cofnij zmiany — Usuń komentarz z okienka ekranu `<meta>` tagów w pliku układu i przywrócić zapytanie o multimedia do 850 pikseli *Site.css* pliku. Zapisz zmiany i odświeżyć przeglądarkę mobilnych, aby sprawdzić, czy wyświetlanie przyjaznych dla urządzeń przenośnych została przywrócona.

Okienko ekranu `<meta>` tag i zapytanie o multimedia CSS nie są specyficzne dla platformy ASP.NET MVC 4 oraz korzystać z zalet tych funkcji, w dowolnej aplikacji sieci web. Ale teraz są one wbudowane w pliki, które są generowane, gdy utworzysz nowy projekt ASP.NET MVC 4.

Aby uzyskać więcej informacji na temat okienka ekranu `<meta>` tagów, zobacz [wskaźnik dwa okienka ekranu — część druga](http://www.quirksmode.org/mobile/viewports2.html).

W następnej sekcji zobaczysz się, jak zapewnić określonych widoków w przeglądarce mobilnej.

## <a name="overriding-views-layouts-and-partial-views"></a>Zastępowanie widoki i układy, widoki częściowe

Znaczące nową funkcją w ASP.NET MVC 4 jest prosty mechanizm, który pozwala na zastępowanie dowolny widok (w tym układy i widoki częściowe) do przeglądarek dla urządzeń przenośnych ogólnie rzecz biorąc, poszczególne przeglądarce dla urządzeń przenośnych lub dla dowolnej przeglądarki. Aby przedstawić widok specyficzne dla mobilnych, skopiuj plik widoku i Dodaj *. Mobile* do nazwy pliku. Na przykład, aby utworzyć urządzeń przenośnych *indeksu* wyświetlić, skopiować *Views\Home\Index.cshtml* do *Views\Home\Index.Mobile.cshtml*.

W tej sekcji utworzysz plik układu specyficzne dla mobilnych.

Aby rozpocząć, skopiuj *Views\Shared\\_Layout.cshtml* do *Views\Shared\\_Layout.Mobile.cshtml*. Otwórz  *\_Layout.Mobile.cshtml* i Zmień tytuł z **konferencji MVC4** do **konferencji (dla urządzeń przenośnych)**.

W każdym `Html.ActionLink` wywołanie, Usuń "Przeglądaj według" w każdym odnośniku *ActionLink*. Poniższy kod przedstawia sekcję ukończone treści pliku układu dla urządzeń przenośnych.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Kopiuj *Views\Home\AllTags.cshtml* plik *Views\Home\AllTags.Mobile.cshtml*. Otwórz nowy plik i zmień `<h2>` elementu z "Tags" do "tagi (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Przejdź na stronę tagów za pomocą przeglądarki na komputerze i za pomocą emulatora w przeglądarce dla urządzeń przenośnych. Emulator przeglądarce dla urządzeń przenośnych zawiera dwa wprowadzone zmiany.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Z kolei Monitor nie zmienił się.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Widoki specyficzne dla przeglądarki

Oprócz przenośnych dotyczące pulpitu i widoki można tworzyć widoki dla poszczególnych przeglądarki. Na przykład można utworzyć widoków, które są przeznaczone dla przeglądarki dla telefonu iPhone. W tej sekcji utworzysz układ przeglądarki dla telefonu iPhone i wersję telefonu iPhone *alltags —* widoku.

Otwórz *Global.asax* pliku i Dodaj następujący kod do `Application_Start` metody.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Ten kod definiuje nowy tryb wyświetlania o nazwie "iPhone", który dopasowywane każdego żądania przychodzącego. Jeśli przychodzące żądanie dopasowuje warunek, zdefiniowane przez użytkownika (to znaczy, jeśli agent użytkownika zawiera ciąg "iPhone"), platformy ASP.NET MVC sprawdza widoki, których nazwa zawiera sufiksu "iPhone".

W kodzie, kliknij prawym przyciskiem myszy `DefaultDisplayMode`, wybierz **rozwiązać**, a następnie wybierz polecenie `using System.Web.WebPages;`. Spowoduje to dodanie odwołania do `System.Web.WebPages` przestrzeni nazw, czyli gdzie `DisplayModes` i `DefaultDisplayMode` są zdefiniowane typy.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Alternatywnie, można po prostu ręcznie dodać następujący wiersz do `using` części pliku.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Pełna zawartość *Global.asax* plików znajdują się poniżej.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Zapisz zmiany. Kopiuj *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* plik *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Otwórz nowy plik, a następnie zmień `h1` nagłówek z `Conference (Mobile)` do `Conference (iPhone)`.

Kopiuj *MvcMobile\Views\Home\AllTags.Mobile.cshtml* plik *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. W nowym pliku, należy zmienić `<h2>` elementu z "tagi (M)" do "Tagi (iPhone)".

Uruchom aplikację. Uruchamianie emulatora przeglądarce dla urządzeń przenośnych, upewnij się, że jej agent użytkownika jest ustawiona na "iPhone" i przejdź do *alltags —* widoku. Poniższy zrzut ekranu przedstawia *alltags —* widok renderowany w [Safari](http://www.apple.com/safari/download/) przeglądarki. Możesz pobrać Safari dla Windows [tutaj](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

W tej sekcji zobaczyliśmy, jak tworzyć układy mobilnych i widoków oraz sposób tworzenia układy i widoki dla określonych urządzeń, takich jak telefon iPhone. W następnej sekcji pokazano, jak korzystać z jQuery Mobile więcej atrakcyjnych mobilnych widoków.

## <a name="using-jquery-mobile"></a>Korzystanie z technologii jQuery Mobile

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) biblioteka zawiera struktura interfejsu użytkownika, który działa na wszystkich popularnych przeglądarkach dla urządzeń przenośnych. jQuery Mobile stosuje *stopniowym rozszerzaniu funkcji* do przeglądarki dla urządzeń przenośnych, które obsługują CSS i JavaScript. Stopniowym rozszerzaniu funkcji zezwala na wszystkie przeglądarki do wyświetlania podstawowa zawartość strony sieci web, zapewniając bardziej zaawansowane przeglądarek i urządzeń bardziej rozbudowane wyświetlone. Pliki JavaScript i CSS, które są dołączone do technologii jQuery Mobile stylu wiele elementów, aby dopasować przeglądarki dla urządzeń przenośnych bez wprowadzania żadnych zmian kodu znaczników.

W tej sekcji zainstalujesz *jQuery.Mobile.MVC* pakiet NuGet, który instaluje jQuery Mobile i element widget o przełącznikiem widoku.

Aby rozpocząć, należy usunąć *Shared\\_Layout.Mobile.cshtml* i *Shared\\_Layout.iPhone.cshtml* pliki, które zostały utworzone wcześniej.

Zmień nazwę *Views\Home\AllTags.Mobile.cshtml* i *Views\Home\AllTags.iPhone.cshtml* plików *Views\Home\AllTags.iPhone.cshtml.hide* i  *Views\Home\AllTags.Mobile.cshtml.Hide*. Ponieważ pliki nie będzie już *.cshtml* rozszerzenie, nie będą one używane przez środowisko uruchomieniowe programu ASP.NET MVC do renderowania *alltags —* widoku.

Zainstaluj *jQuery.Mobile.MVC* pakietu NuGet w ten sposób:

1. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz pozycję **Konsola Menedżera pakietów**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. W **Konsola Menedżera pakietów**, wprowadź `Install-Package jQuery.Mobile.MVC -version 1.0.0`

Na poniższej ilustracji przedstawiono pliki dodane i zmiany wprowadzone do projektu MvcMobile przez pakiet NuGet jQuery.Mobile.MVC. Pliki, które są dodawane [Dodaj] dołączany po nazwie pliku. Obraz, który nie jest wyświetlany plik GIF i dodawać pliki PNG *Content\images* folderu.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Pakiet NuGet jQuery.Mobile.MVC instaluje następujące czynności:

- *Aplikacji\_Start\BundleMobileConfig.cs* pliku, który jest potrzebny do odwołania dodane pliki JavaScript i CSS jQuery. Należy postępuj zgodnie z instrukcjami poniżej, a następnie odwoływać się do pakietu przenośnych zdefiniowaną w tym pliku.
- jQuery Mobile CSS pliki.
- A `ViewSwitcher` widżet kontrolera (*Controllers\ViewSwitcherController.cs*).
- jQuery Mobile JavaScript pliki.
- Plik układu różne Mobile jQuery (*Views\Shared\\_Layout.Mobile.cshtml*).
- Widok częściowy przełącznikiem widoku *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) zapewniającej link u góry każdej strony, aby przełączyć się z widok pulpitu na widok dla urządzeń przenośnych i na odwrót.
- Kilka<em>.png</em> i <em>.gif</em> pliki obrazów w <em>Content\images</em> folderu.

Otwórz *Global.asax* pliku i Dodaj następujący kod jako ostatni wiersz `Application_Start` metody.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Poniższy kod przedstawia pełny *Global.asax* pliku.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Jeśli używasz programu Internet Explorer 9, a nie widać `BundleMobileConfig` wiersz powyżej w Wyróżnij żółty, kliknij przycisk [przycisk Widok zgodności](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![obraz przycisku Widok zgodności (wyłączone)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Obraz przycisku Widok zgodności (wyłączone)") w programie Internet Explorer, aby ikona zmiany z konturem ![obraz przycisku Widok zgodności (wyłączone)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "obraz przycisku Widok zgodności (wyłączony) ") pełny kolor ![obraz przycisku Widok zgodności (funkcja włączona)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "obraz przycisku Widok zgodności (funkcja włączona)"). Alternatywnie można wyświetlić w tym samouczku w przeglądarce FireFox lub Chrome.


Otwórz *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* pliku i Dodaj następujący kod bezpośrednio po `Html.Partial` wywołania:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Pełne *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* plików znajdują się poniżej:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Skompiluj aplikację i w swojej przeglądarce dla urządzeń przenośnych emulatorze przejdź do *alltags —* widoku. Zostaną wyświetlone następujące czynności:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Można debugować mobilnego określonych przez [ustawienie ciąg agenta użytkownika](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) dla programu Internet Explorer lub Chrome do telefonu iPhone, a następnie użyć narzędzia deweloperskie F-12. Jeśli w przeglądarce dla urządzeń przenośnych nie są wyświetlane **Home**, **osoby mówiącej**, **Tag**, i **data** łącza jako przyciski, odwołania do technologii jQuery Mobile skrypty i pliki CSS prawdopodobnie nie są prawidłowe.


Oprócz zmian stylów zobaczysz **wyświetlanie widoku dla urządzeń przenośnych** i łącze, które umożliwia przechodzenie z widoku dla urządzeń przenośnych do widoku pulpitu. Wybierz **widok pulpitu** łącze i widok pulpitu są wyświetlane.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Widok pulpitu nie umożliwiają bezpośrednie przejście do widoku dla urządzeń przenośnych. Można to naprawić teraz. Otwórz *Views\Shared\\_Layout.cshtml* pliku. Tuż poniżej strony `body` elementu, Dodaj następujący kod, który renderuje element widget przełącznikiem widoku:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Odśwież *alltags —* Wyświetl w przeglądarce dla urządzeń przenośnych. Możesz teraz przechodzić między widokami pulpitu i na urządzeniach przenośnych.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Debugowanie Uwaga: można dodać następujący kod na końcu Views\Shared\\_ViewSwitcher.cshtml, aby pomóc w debugowaniu widoki, gdy za pomocą przeglądarki ciąg agenta użytkownika ustawiony na urządzeniu przenośnym.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  i dodać następujący nagłówek do *Views\Shared\\_Layout.cshtml* pliku.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Przejdź do *alltags —* strony w przeglądarce pulpitu. Widżet o przełącznikiem widoku nie jest wyświetlana w przeglądarce pulpitu, ponieważ jest ono dodane tylko do strony układu dla urządzeń przenośnych. W dalszej części tego samouczka zobaczysz, jak dodać element widget przełącznikiem widoku w widoku pulpitu.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Poprawa lista prelegentów

W przeglądarce mobilnej, wybierz **prelegentów** łącza. Ponieważ nie istnieje żadne widoku dla urządzeń przenośnych (*AllSpeakers.Mobile.cshtml*), Wyświetl prelegentów domyślne (*AllSpeakers.cshtml*) jest renderowany przy użyciu widoku układu dla urządzeń przenośnych ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Domyślny widok (innych niż urządzenia przenośne) z renderowania wewnątrz układu dla urządzeń przenośnych można globalnie wyłączyć, ustawiając `RequireConsistentDisplayMode` do `true` w *widoków\\_ViewStart.cshtml* pliku następująco:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Gdy `RequireConsistentDisplayMode` ustawiono `true`, układu dla urządzeń przenośnych (<em>\_Layout.Mobile.cshtml</em>) jest używany tylko dla mobilnych widoków. (To znaczy, Wyświetl plik ma postać <em>** Nazwa widoku</em><em>. Mobile.cshtml</em>.) Warto ustawić `RequireConsistentDisplayMode` do `true` Jeśli Twoje układu dla urządzeń przenośnych nie działa dobrze z widoków nieprzenośnych. Zrzut ekranu poniżej przedstawiono sposób, w jaki <em>prelegentów</em> renderowanie strony, gdy `RequireConsistentDisplayMode` ustawiono `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Wyłącz spójnego trybu wyświetlania w widoku, ustawiając `RequireConsistentDisplayMode` do `false` w pliku widoku. Następujące znaczniki w *Views\Home\AllSpeakers.cshtml* plików zestawów `RequireConsistentDisplayMode` do `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Tworzenie widoku przenośnych prelegentów

Ponieważ właśnie uruchomiliśmy, *prelegentów* widoku do odczytu, ale linki są małe i są trudne do naciśnij przycisk na urządzeniu przenośnym. W tej sekcji utworzysz konkretną mobile *prelegentów* widoku, który wygląda podobnie do nowoczesnych aplikacji mobilnej — Wyświetla dużych, łatwo naciśnij łączy i zawiera pole wyszukiwania, aby szybko znaleźć głośników.

Kopiuj *AllSpeakers.cshtml* do *AllSpeakers.Mobile.cshtml*. Otwórz *AllSpeakers.Mobile.cshtml* pliku i usuwania `<h2>` element nagłówka.

W `<ul>` tagów, należy dodać `data-role` atrybut i ustawić jej wartość na `listview`. Jak inne [ `data-*` atrybuty](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` sprawia, że łatwiej elementów listy dużych naciśnięcie pozycji. Jest to ukończone znaczników wygląda następująco:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Odśwież w przeglądarce mobilnej. Wyświetl zaktualizowane wygląda następująco:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Mimo, że zostały udoskonalone w widoku dla urządzeń przenośnych, jest trudny do przejść długą listę prelegentów. Aby rozwiązać ten problem, w `<ul>` tagów, należy dodać `data-filter` atrybutu i ustaw ją na `true`. Poniższy kod przedstawia `ul` znaczników.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

Na poniższej ilustracji przedstawiono okno filtr wyszukiwania w górnej części strony, która wynika z `data-filter` atrybutu.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Podczas wpisywania każdego literę w polu wyszukiwania, jQuery Mobile filtruje wyświetlonej listy, jak pokazano na poniższej ilustracji.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Poprawa lista tagów

Wartość domyślna, takich jak *prelegentów* widoku *tagi* widok jest do odczytu, ale linki są małe i trudne do naciśnij przycisk na urządzeniu przenośnym. W tej sekcji naprawisz *tagi* wyświetlać ten sam sposób Naprawiono *prelegentów* widoku.

Usuń &quot;Ukryj&quot; sufiks *Views\Home\AllTags.Mobile.cshtml.hide* pliku, dzięki czemu jest nazwa *Views\Home\AllTags.Mobile.cshtml*. Otwórz plik o zmienionej nazwie i Usuń `<h2>` elementu.

Dodaj `data-role` i `data-filter` atrybuty do `<ul>` tag, jak pokazano poniżej:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

Na poniższym obrazie przedstawiono stronę tagów i filtrowanie list `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Poprawianie listy daty

Można zwiększyć *daty* wyświetlić, takie jak zwiększenie *prelegentów* i *tagi* widoki tak, aby ułatwić korzystanie na urządzeniu przenośnym.

Kopiuj *Views\Home\AllDates.cshtml* plik *Views\Home\AllDates.Mobile.cshtml*. Otwórz nowy plik i Usuń `<h2>` elementu.

Dodaj `data-role="listview"` do `<ul>` tag następująco:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

Na poniższym obrazie przedstawiono co **data** wygląda strona przy użyciu `data-role` atrybutu w miejscu.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Zastąp zawartość *Views\Home\AllDates.Mobile.cshtml* pliku następującym kodem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Ten kod grupuje wszystkie sesje według dni. Tworzy separator listy na każdy dzień nowych i wyświetla listę wszystkich sesji dla poszczególnych dni w obszarze separator. Poniżej przedstawiono wygląda jak podczas wykonywania tego kodu:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Poprawianie widoku SessionsTable

W tej sekcji utworzysz widok mobile określonej sesji. Zmiany, które firma Microsoft będzie bardziej rozbudowany niż w innych widokach, którą utworzyliśmy.

W przeglądarce mobilnej naciśnij **osoby mówiącej** przycisk, a następnie wprowadź `Sc` w polu wyszukiwania.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Naciśnij pozycję **Scott Hanselman** łącza.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Jak widać, ekran jest trudne do odczytania w przeglądarce dla urządzeń przenośnych. Kolumna dat jest trudny do odczytania i kolumny tagów jest poza widoku. Aby rozwiązać ten problem, skopiuj *Views\Home\SessionsTable.cshtml* do *Views\Home\SessionsTable.Mobile.cshtml*, a następnie zastąp zawartość pliku następującym kodem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Kod usuwa pokoju tagów, kolumny i formatuje tytuł, osoby mówiącej i datę w pionie, tak, aby wszystkie te informacje są do odczytu w przeglądarce dla urządzeń przenośnych. Na poniższej ilustracji odzwierciedla zmian w kodzie.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Poprawianie widoku SessionByCode

Na koniec utworzymy widok specyficzne dla mobile *SessionByCode* widoku. W przeglądarce mobilnej naciśnij **osoby mówiącej** przycisk, a następnie wprowadź `Sc` w polu wyszukiwania.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Naciśnij pozycję **Scott Hanselman** łącza. Scott Hanselman sesje są wyświetlane.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Wybierz **omówienie stosu sieci Web MS Love** łącza.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Domyślny widok pulpitu jest w dobrym stanie, ale można ją ulepszyć.

Kopiuj *Views\Home\SessionByCode.cshtml* do *Views\Home\SessionByCode.Mobile.cshtml* i Zastąp zawartość *Views\Home\SessionByCode.Mobile.cshtml*pliku następującym kodem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Korzysta z nowych znaczników `data-role` atrybutu, aby poprawić układ widoku.

Odśwież w przeglądarce mobilnej. Poniższa ilustracja odzwierciedla zmian w kodzie, które zostały wykonane:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup i przeglądu

W tym samouczku wprowadził nowe funkcje mobilnych programu ASP.NET MVC 4 dla deweloperów w wersji zapoznawczej. Funkcje mobilne obejmują:

- Możliwość przesłaniania układ, widoki i widoki częściowe, globalnie i dla poszczególnych widoku.
- Kontroluje układ i za pomocą wymuszania częściowe zastąpienie `RequireConsistentDisplayMode` właściwości.
- Element widget o przełącznikiem widoku dla urządzeń przenośnych widoki nie mogą być także wyświetlane w widokach pulpitu.
- Obsługa do obsługi określonych przeglądarki, takich jak przeglądarki dla telefonu iPhone.

## <a name="see-also"></a>Zobacz też

- [jQuery Mobile](http://jquerymobile.com) lokacji.
- [jQuery Mobile — omówienie](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C zalecenie mobilne stosowanie najlepszych rozwiązań](http://www.w3.org/TR/mwabp/)
- [W3C zalecenie Release Candidate dla zapytaniami multimediów](http://www.w3.org/TR/css3-mediaqueries/)
