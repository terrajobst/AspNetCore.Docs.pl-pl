---
uid: mvc/overview/older-versions/aspnet-mvc-4-mobile-features
title: Funkcje mobilne platformy ASP.NET MVC 4 | Dokumentacja firmy Microsoft
author: Rick-Anderson
description: "Teraz jest wersją MVC 5 tego samouczka z przykładów kodu w Wdróż ASP.NET MVC 5 mobilnych aplikacji sieci Web w witrynach sieci Web platformy Azure."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2012
ms.topic: article
ms.assetid: 27dc4fc8-1b51-43b0-933f-fc1b52476523
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/aspnet-mvc-4-mobile-features
msc.type: authoredcontent
ms.openlocfilehash: d47d8f61dc7af6e1dc5887338be862ea81d7bb17
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-mvc-4-mobile-features"></a>Funkcje mobilne platformy ASP.NET MVC 4
====================
Przez [Rick Anderson](https://github.com/Rick-Anderson)

> Teraz jest wersja MVC 5 tego samouczka z przykładów kodu w [wdrażania platformy ASP.NET MVC 5 mobilnych aplikacji sieci Web w witrynach sieci Web Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-mobile-app/).


W tym samouczku uczy podstaw dotyczących pracy z Funkcje mobilne w aplikacji sieci Web programu ASP.NET MVC 4. W tym samouczku, można użyć [programu Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) lub Visual Web Developer 2010 Express Service Pack 1 (&quot;Visual Web Developer lub VWD&quot;). Jeśli masz już który umożliwia professional wersji programu Visual Studio.

Przed rozpoczęciem upewnij się, że po zainstalowaniu wymagania wstępne wymienione poniżej.

- [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express) (zalecane) lub programu Visual Studio Web Developer Express z dodatkiem SP1. Visual Studio 2012 contains ASP.NET MVC 4. Jeśli używasz programu Visual Web Developer 2010, należy zainstalować [ASP.NET MVC 4](https://go.microsoft.com/fwlink/?LinkId=243392).

Należy również emulatora przeglądarkę dla telefonów. Działa jedną z następujących czynności:

- [Emulator systemu Windows 7 Phone](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). (Jest to emulator, który jest używany w większości zrzutów ekranu, w tym samouczku).
- Zmień ciąg agenta użytkownika, co pozwoliłoby na emulowanie iPhone. Zobacz [to](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) wpis w blogu.
- [Opera emulatorze przenośnym](http://www.opera.com/developer/tools/mobile/)
- [Apple Safari](http://www.apple.com/safari/download/) z ustawioną iPhone agenta użytkownika. Aby uzyskać instrukcje na temat sposobu ustawiania agenta użytkownika w programie Safari na "iPhone", zobacz [jak umożliwić Safari podawać jest IE](http://www.davidalison.com/2008/05/how-to-let-safari-pretend-its-ie.html) na blogu Dominik Alison.

Powiązany z tym tematem dostępnych projektów programu Visual Studio z kodu źródłowego C#:

- [Początkowy projektem do pobrania](https://go.microsoft.com/fwlink/?linkid=228307&amp;clcid=0x409)
- [Pobranie projektu zakończone](https://go.microsoft.com/fwlink/?linkid=228306&amp;clcid=0x409)

### <a name="what-youll-build"></a>Jakie będzie kompilacji

W tym samouczku, warto Funkcje mobilne prostą aplikację konferencji listę, która znajduje się w [projektu starter](https://go.microsoft.com/fwlink/?LinkId=228307). Poniższy zrzut ekranu przedstawia strony tagi ukończona aplikacja, jak pokazano w [Emulator systemu Windows Phone 7](https://msdn.microsoft.com/library/ff402563(VS.92).aspx). Zobacz [klawiatury mapowania dla Windows Phone Emulator](https://msdn.microsoft.com/library/ff754352(v=vs.92).aspx) uprościć wprowadzanie z klawiatury.

[![p1_Tags_CompletedProj](aspnet-mvc-4-mobile-features/_static/image2.png)](aspnet-mvc-4-mobile-features/_static/image1.png)

Można użyć programu Internet Explorer w wersji 9 lub 10, FireFox lub Chrome, do opracowywania aplikacji mobilnej, ustawiając [ciąg agenta użytkownika](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/). Na poniższej ilustracji przedstawiono ukończone samouczka przy użyciu programu Internet Explorer emulowanie iPhone. Korzystając z narzędzi deweloperskich programu Internet Explorer F-12 i [narzędzie Fiddler](http://www.fiddler2.com/fiddler2/) do debugowania aplikacji.

![](aspnet-mvc-4-mobile-features/_static/image3.png)

### <a name="skills-youll-learn"></a>Dowiesz się umiejętności

Oto dowiesz się:

- Jak szablony ASP.NET MVC 4 używać HTML5 `viewport` atrybut i renderowania adaptacyjną zwiększające wyświetlania na urządzeniach przenośnych.
- Jak tworzyć widoki specyficzne dla mobilnych.
- Jak utworzyć przełącznik Widok ten umożliwia przełączania użytkowników między widokiem dla urządzeń przenośnych i stacjonarnych widoku aplikacji.

### <a name="getting-started"></a>Wprowadzenie

Pobierz aplikację listy konferencji dla projektu starter, korzystając z następującego łącza: [Pobierz](https://go.microsoft.com/fwlink/?LinkId=228307). Następnie w Eksploratorze Windows, kliknij prawym przyciskiem myszy *MvcMobile.zip* plik i wybierz polecenie **właściwości**. W **właściwości MvcMobile.zip** oknie dialogowym wybierz **Odblokuj** przycisku. (Odblokowywania uniemożliwia ostrzeżenie występuje, gdy użytkownik próbuje użyć *.zip* pliku pobranym z sieci web.)

![p1_unBlock](aspnet-mvc-4-mobile-features/_static/image4.png)

Kliknij prawym przyciskiem myszy *MvcMobile.zip* plik i wybierz **Wyodrębnij wszystkie** aby rozpakować plik. W programie Visual Studio Otwórz *MvcMobile.sln* pliku.

Naciśnij klawisze CTRL + F5, aby uruchomić aplikację, którym będą wyświetlane w przeglądarce pulpitu. Uruchom z emulatora przeglądarkę dla telefonów, skopiuj adres URL dla aplikacji konferencji do emulatora, a następnie kliknij **Przeglądaj według znaczników** łącza. Jeśli używasz emulatora Windows Phone, kliknij na pasku adresu URL i naciśnij klawisz Wstrzymaj, aby uzyskać dostęp za pomocą klawiatury. Obraz poniżej przedstawia *alltags —* widoku (wybór **Przeglądaj według znaczników**).

[![p1_browseTag](aspnet-mvc-4-mobile-features/_static/image6.png)](aspnet-mvc-4-mobile-features/_static/image5.png)

Ekran jest bardzo do odczytu na urządzeniu przenośnym. Wybierz łącze ASP.NET.

[![p1_tagged_ASPNET](aspnet-mvc-4-mobile-features/_static/image8.png)](aspnet-mvc-4-mobile-features/_static/image7.png)

Widok znaczników ASP.NET jest bardzo dużej liczby. Na przykład **data** kolumny jest bardzo trudne do odczytu. W dalszej części samouczka utworzysz wersję *alltags —* widoku dotyczy w szczególności przeglądarki dla urządzeń przenośnych i będzie odczytywać wyświetlania.

Uwaga: Obecnie usterki istnieje w przenośnych aparat buforowania. Dla aplikacji produkcyjnych, należy zainstalować [stałym DisplayModes](http://nuget.org/packages/Microsoft.AspNet.Mvc.FixedDisplayModes) nugget pakietu. Zobacz [ASP.NET MVC 4 Mobile buforowanie usterki stałym](https://blogs.msdn.com/b/rickandy/archive/2012/09/17/asp-net-mvc-4-mobile-caching-bug-fixed.aspx) szczegółowe informacje na temat poprawki.

## <a name="css-media-queries"></a>Zapytaniami multimediów CSS

[Zapytaniami multimediów CSS](http://www.w3.org/TR/css3-mediaqueries/) są rozszerzeniem CSS do typów nośników. Umożliwiają one tworzenie reguł, które zastępują domyślne reguły CSS dla przeglądarki (agentów użytkownika). Typowe regułę CSS, przeznaczonego dla przeglądarki dla urządzeń przenośnych jest zdefiniowanie maksymalny rozmiar ekranu. *Content\Site.css* pliku, który jest tworzony podczas tworzenia nowego projektu platformy ASP.NET MVC 4 Internet zawiera następujące zapytanie o multimedia:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample1.css)]

Jeśli 850 pikseli szerokości okna przeglądarki, zostanie użyty reguły CSS wewnątrz tego bloku nośnika. Zapytaniami multimediów CSS, jak to umożliwia zapewniają lepsze wyświetlanie zawartości HTML w małych przeglądarek (na przykład przeglądarki dla urządzeń przenośnych) niż domyślne reguły CSS, które są przeznaczone dla szerszego Wyświetla przeglądarek pulpitu.

## <a name="the-viewport-meta-tag"></a>Tag Meta okienka ekranu

Szerokość okna przeglądarki wirtualnego zdefiniuj przeglądarki dla większości urządzeń przenośnych ( *okienka ekranu*) jest znacznie większą niż szerokość rzeczywistego urządzenia przenośnego. Dzięki temu przeglądarek urządzeń przenośnych pomieścić całą stronę sieci web wewnątrz wirtualnych wyświetlania. Użytkownicy mogą następnie powiększ interesujące zawartości. Jednak jeśli ustawisz szerokość okienka ekranu do szerokości rzeczywistego urządzenia powiększanie nie jest wymagane, ponieważ zawartość mieści się w przeglądarce przenośnych.

Okienko ekranu `<meta>` znacznika w pliku układu ASP.NET MVC 4 Ustawia szerokość urządzenia okienka ekranu. Następujący wiersz zawiera okienka ekranu `<meta>` znacznika w pliku układu ASP.NET MVC 4.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample2.html)]

## <a name="examining-the-effect-of-css-media-queries-and-the-viewport-meta-tag"></a>Badanie wpływu zapytaniami multimediów CSS i metatag okienka ekranu

Otwórz *Views\Shared\\_Layout.cshtml* plik w edytorze i komentarz dla okienka ekranu `<meta>` tagu. Następujący kod przedstawia wiersza poza komentarzem.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample3.cshtml)]

Otwórz *MvcMobile\Content\Site.css* plik w edytorze i zmień maksymalną szerokość w zapytanie o multimedia zero pikseli. Uniemożliwi to reguły CSS używane w przeglądarkach dla urządzeń przenośnych. Następujący wiersz zawiera zapytanie o multimedia zmodyfikowane:

[!code-css[Main](aspnet-mvc-4-mobile-features/samples/sample4.css)]

Zapisz zmiany i przejdź do aplikacji konferencji w emulatorze przenośnym przeglądarki. Niewielki rozmiar tekstu na poniższej ilustracji jest wynikiem usunięcia okienka ekranu `<meta>` tagu. Z ma okienka ekranu `<meta>` tagu przeglądarki zmniejszanie jest domyślną szerokość okienka ekranu (850 pikseli lub szerokość dla przeglądarki dla większości urządzeń przenośnych.)

[![p1_noViewPort](aspnet-mvc-4-mobile-features/_static/image10.png)](aspnet-mvc-4-mobile-features/_static/image9.png)

Cofnij zmiany — usuń znaczniki komentarza okienka ekranu `<meta>` tagów w pliku układu i przywrócić zapytanie o multimedia do 850 pikseli *Site.css* pliku. Zapisz zmiany i odświeżyć przeglądarkę przenośnych, aby sprawdzić, czy wyświetlana przyjazna została przywrócona.

Okienko ekranu `<meta>` znacznika i zapytanie o multimedia CSS nie są specyficzne dla platformy ASP.NET MVC 4, a w dowolnej aplikacji sieci web mogą korzystać z tych funkcji. Ale są one obecnie wbudowane w pliki, które są generowane podczas tworzenia nowego projektu platformy ASP.NET MVC 4.

Aby uzyskać więcej informacji na temat okienka ekranu `<meta>` tagów, zobacz [wskaźnik dwa okienka ekranu — część druga](http://www.quirksmode.org/mobile/viewports2.html).

W następnej sekcji pojawi się, jak zapewnić konkretnych widoków mobile przeglądarki.

## <a name="overriding-views-layouts-and-partial-views"></a>Zastępowanie widoki, układów i widoki częściowe

Znaczące nową funkcją w technologii ASP.NET MVC 4 jest prosty mechanizm, który pozwala na zastępowanie dowolnego widoku (w tym układy i widoki częściowe) dla przeglądarki dla urządzeń przenośnych ogólnie rzecz biorąc, poszczególne przeglądarkę dla telefonów lub dowolnej określonej przeglądarki. W celu zapewnienia przeglądu specyficzne dla mobilnych, skopiuj plik widoku i Dodaj *. Mobile* do nazwy pliku. Na przykład, aby utworzyć przenośnym *indeksu* wyświetlić, skopiować *Views\Home\Index.cshtml* do *Views\Home\Index.Mobile.cshtml*.

W tej sekcji utworzysz plik układu specyficzne dla mobilnych.

Aby rozpocząć, skopiuj *Views\Shared\\_Layout.cshtml* do *Views\Shared\\_Layout.Mobile.cshtml*. Otwórz  *\_Layout.Mobile.cshtml* i Zmień tytuł z **konferencji MVC4** do **konferencji (Mobile)**.

W każdym `Html.ActionLink` wywołać, Usuń "Przeglądaj według", w każdym odnośniku *ActionLink*. Poniższy kod przedstawia sekcji ukończone treści pliku przenośnych układu.

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample5.cshtml)]

Kopiuj *Views\Home\AllTags.cshtml* pliku *Views\Home\AllTags.Mobile.cshtml*. Otwórz nowy plik i zmień `<h2>` element na podstawie "Tagi" do "tagi (M)":

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample6.html)]

Przejdź do strony tagów za pomocą przeglądarki pulpitu i przy użyciu emulatora przeglądarkę dla telefonów. Emulator przeglądarkę dla telefonów zawiera dwa wprowadzone zmiany.

[![p2m_layoutTags.mobile](aspnet-mvc-4-mobile-features/_static/image12.png)](aspnet-mvc-4-mobile-features/_static/image11.png)

Z kolei Monitor nie zmienił się.

[![p2_layoutTagsDesktop](aspnet-mvc-4-mobile-features/_static/image14.png)](aspnet-mvc-4-mobile-features/_static/image13.png)

## <a name="browser-specific-views"></a>Widoki specyficzne dla przeglądarki

Oprócz mobile dotyczące pulpitu i widoki można tworzyć widoki dla poszczególnych przeglądarki. Na przykład można tworzyć widoki, które są przeznaczone dla przeglądarki iPhone. W tej sekcji utworzysz układu dla przeglądarki iPhone i wersji iPhone *alltags —* widoku.

Otwórz *Global.asax* i Dodaj następujący kod do `Application_Start` metody.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample7.cs)]

Ten kod definiuje nowy tryb wyświetlania o nazwie "iPhone" pasujących względem każdego żądania przychodzącego. Jeśli żądanie przychodzące odpowiada warunku, zdefiniowane przez użytkownika (jeśli agent użytkownika zawiera ciąg "iPhone"), platformy ASP.NET MVC sprawdza widoki, których nazwa zawiera sufiksu "iPhone".

W kodzie, kliknij prawym przyciskiem myszy `DefaultDisplayMode`, wybierz **rozwiązać**, a następnie wybierz pozycję `using System.Web.WebPages;`. Spowoduje to dodanie odwołania do `System.Web.WebPages` przestrzeni nazw, czyli gdzie `DisplayModes` i `DefaultDisplayMode` typy zostały zdefiniowane.

[![p2_resolve](aspnet-mvc-4-mobile-features/_static/image16.png)](aspnet-mvc-4-mobile-features/_static/image15.png)

Alternatywnie można po prostu ręcznie dodaj następujący wiersz do `using` sekcji pliku.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample8.cs)]

Pełna zawartość *Global.asax* plików są wyświetlane poniżej.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample9.cs)]

Zapisz zmiany. Kopiuj *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* pliku *MvcMobile\Views\Shared\\_Layout.iPhone.cshtml*. Otwórz nowy plik, a następnie zmień `h1` nagłówek z `Conference (Mobile)` do `Conference (iPhone)`.

Kopiuj *MvcMobile\Views\Home\AllTags.Mobile.cshtml* pliku *MvcMobile\Views\Home\AllTags.iPhone.cshtml*. W nowym pliku zmienić `<h2>` element z "tagi (M)" do "Tagów (iPhone)".

Uruchom aplikację. Uruchamianie emulatora przeglądarkę dla telefonów, upewnij się, że jego agenta użytkownika ma ustawioną wartość "iPhone" i przejdź do *alltags —* widoku. Poniższy zrzut ekranu przedstawia *alltags —* widok renderowany w [Safari](http://www.apple.com/safari/download/) przeglądarki. Safari dla systemu Windows można pobrać [tutaj](https://support.apple.com/kb/DL1531).

[![p2_iphoneView](aspnet-mvc-4-mobile-features/_static/image18.png)](aspnet-mvc-4-mobile-features/_static/image17.png)

W tej sekcji możemy przedstawiono sposób tworzenia widoków i układy przenośnych oraz sposobu tworzenia widoków dla konkretnych urządzeń, takich jak telefonów iPhone i układów. W następnej sekcji pojawi się, jak korzystać z jQuery Mobile więcej atrakcyjnych widoków przenośnych.

## <a name="using-jquery-mobile"></a>Przy użyciu technologii jQuery Mobile

[JQuery Mobile](http://jquerymobile.com/demos/1.0b3/#/demos/1.0b3/docs/about/intro.html) biblioteki zapewniają strukturę interfejsu użytkownika, która działa we wszystkich popularnych przeglądarkach dla urządzeń przenośnych. jQuery Mobile stosuje *stopniowym rozszerzaniu* do przeglądarek urządzeń przenośnych, które obsługują CSS i JavaScript. Stopniowym rozszerzaniu umożliwia wszystkie przeglądarki wyświetlić podstawowe elementy strony sieci web, podczas gdy większe możliwości przeglądarki i urządzenia wyświetlone bardziej zaawansowane funkcje. Pliki JavaScript i CSS, które są dołączone do jQuery Mobile stylów wiele elementów, aby dopasować przeglądarki dla urządzeń przenośnych bez wprowadzania żadnych zmian znaczników.

W tej sekcji nastąpi instalacja *jQuery.Mobile.MVC* pakietu NuGet, który instaluje jQuery Mobile i elementu widget tym przełącznikiem widoku.

Aby rozpocząć, należy usunąć *Shared\\_Layout.Mobile.cshtml* i *Shared\\_Layout.iPhone.cshtml* pliki, które zostały utworzone wcześniej.

Zmień nazwę *Views\Home\AllTags.Mobile.cshtml* i *Views\Home\AllTags.iPhone.cshtml* plików do *Views\Home\AllTags.iPhone.cshtml.hide* i  *Views\Home\AllTags.Mobile.cshtml.Hide*. Ponieważ pliki nie muszą już *.cshtml* rozszerzenie, nie będą one używane przez środowisko uruchomieniowe programu ASP.NET MVC do renderowania *alltags —* widoku.

Zainstaluj *jQuery.Mobile.MVC* pakietu NuGet w ten sposób:

1. Z **narzędzia** menu, wybierz opcję **Menedżer pakietów biblioteki**, a następnie wybierz **Konsola Menedżera pakietów**.

    [![p3_packageMgr](aspnet-mvc-4-mobile-features/_static/image20.png)](aspnet-mvc-4-mobile-features/_static/image19.png)
2. W **Konsola Menedżera pakietów**, wprowadź`Install-Package jQuery.Mobile.MVC -version 1.0.0`

Na poniższej ilustracji przedstawiono pliki, dodawania i modyfikowania do projektu MvcMobile przez pakiet NuGet jQuery.Mobile.MVC. Pliki, które są dodawane [Dodaj] dołączane po nazwie pliku. Obraz nie jest wyświetlany plik GIF i PNG, pliki dodane do *Content\images* folderu.

![](aspnet-mvc-4-mobile-features/_static/image21.png)

Pakiet NuGet jQuery.Mobile.MVC instalowane są następujące:

- *Aplikacji\_Start\BundleMobileConfig.cs* pliku, który jest potrzebny do odwołania dodane pliki JavaScript i CSS jQuery. Należy postępuj zgodnie z instrukcjami poniżej i odwołać przenośnych pakietu zdefiniowane w tym pliku.
- pliki Mobile CSS jQuery.
- A `ViewSwitcher` widget kontrolera (*Controllers\ViewSwitcherController.cs*).
- jQuery Mobile JavaScript pliki.
- Pliku jQuery Mobile, styl układu (*Views\Shared\\_Layout.Mobile.cshtml*).
- Widok częściowy tym przełącznikiem widoku *(MvcMobile\Views\Shared\\_ViewSwitcher.cshtml*) zapewnia łącze na początku każdej strony, aby przełączyć się z widoku pulpitu widokiem dla urządzeń przenośnych i na odwrót.
- Kilka*.png* i *.gif* pliki obrazów w *Content\images* folderu.

Otwórz *Global.asax* pliku i Dodaj następujący kod, jak ostatni wiersz `Application_Start` metody.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample10.cs)]

Poniższy kod przedstawia pełną *Global.asax* pliku.

[!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample11.cs?highlight=26)]

> [!NOTE]
> Jeśli korzystasz z programu Internet Explorer 9, a nie ma `BundleMobileConfig` wiersz powyżej w Wyróżnij żółty, kliknij przycisk [przycisk Widok zgodności](https://windows.microsoft.com/windows7/How-to-use-Compatibility-View-in-Internet-Explorer-9)![obraz przycisku widoku zgodności (wyłączone)] (http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg " Obraz przycisku widoku zgodności (wyłączone)") w programie Internet Explorer, aby ikona zmiany z konspektu ![obraz przycisku widoku zgodności (wyłączone)](http://res2.windows.microsoft.com/resbox/en/Windows 7/main/f080e77f-9b66-4ac8-9af0-803c4f8a859c_15.jpg "obraz przycisku widoku zgodności (wyłączone) ") pełny kolor ![obraz przycisku widoku zgodności (na)](http://res1.windows.microsoft.com/resbox/en/Windows 7/main/156805ff-3130-481b-a12d-4d3a96470f36_14.jpg "obraz przycisku widoku zgodności (na)"). Alternatywnie można wyświetlić w tym samouczku w przeglądarki FireFox lub Chrome.


Otwórz *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* i Dodaj następujący kod bezpośrednio po `Html.Partial` wywołania:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample12.cshtml)]

Pełną *MvcMobile\Views\Shared\\_Layout.Mobile.cshtml* pliku przedstawiono poniżej:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample13.cshtml)]

Tworzenie aplikacji i w Twojej emulatorze przenośnym przeglądarki przejdź do *alltags —* widoku. Zostanie wyświetlony poniżej:

[![p3_afterNuGet](aspnet-mvc-4-mobile-features/_static/image23.png)](aspnet-mvc-4-mobile-features/_static/image22.png)

> [!NOTE]
> Można debugować dostępu mobilnego określonych przez [ustawienie ciąg agenta użytkownika](http://www.howtogeek.com/113439/how-to-change-your-browsers-user-agent-without-installing-any-extensions/) dla programu Internet Explorer lub przeglądarki Chrome iPhone i przy użyciu narzędzia Projektant F-12. Jeśli przeglądarka przenośnych nie są wyświetlane **Home**, **prelegenta**, **Tag**, i **data** łącza jako przyciski, odwołania do jQuery Mobile skrypty i pliki CSS prawdopodobnie nie są prawidłowe.


Oprócz zmian stylu, zobacz **wyświetlania widoku przenośnych** oraz łącze, które umożliwia przełączenie z widokiem dla urządzeń przenośnych do widoku pulpitu. Wybierz **widok pulpitu** łącza, a widok pulpitu jest wyświetlana.

[![p3_desktopView](aspnet-mvc-4-mobile-features/_static/image25.png)](aspnet-mvc-4-mobile-features/_static/image24.png)

Widok pulpitu nie umożliwiają bezpośrednio przejść z powrotem do widokiem dla urządzeń przenośnych. Który będzie napraw teraz. Otwórz *Views\Shared\\_Layout.cshtml* pliku. Tylko w obszarze strony `body` elementu, Dodaj następujący kod, który renderuje element widget tym przełącznikiem widoku:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample14.cshtml)]

Odśwież *alltags —* Wyświetl w przeglądarce przenośnych. Teraz można przechodzić między widokami komputerów stacjonarnych i przenośnych.

[![p3_desktopViewWithMobileLink](aspnet-mvc-4-mobile-features/_static/image27.png)](aspnet-mvc-4-mobile-features/_static/image26.png)

> [!NOTE]
> Debugowanie Uwaga: możesz Dodaj następujący kod na końcu Views\Shared\\_ViewSwitcher.cshtml debugować widoków ustawianą przy użyciu przeglądarki ciąg agenta użytkownika na urządzenie przenośne.
> 
> [!code-csharp[Main](aspnet-mvc-4-mobile-features/samples/sample15.cs)]
> 
>  i dodawanie następujący nagłówek do *Views\Shared\\_Layout.cshtml* pliku.  
> 
> [!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample16.html)]


Przejdź do *alltags —* strony w przeglądarce pulpitu. Widżet tym przełącznikiem widoku nie jest wyświetlana w przeglądarce pulpitu, ponieważ jest ono dodane tylko do strony układu przenośnych. Później w samouczku zobaczysz, jak dodać element widget tym przełącznikiem widoku do widoku pulpitu.

[![p3_desktopBrowser](aspnet-mvc-4-mobile-features/_static/image29.png)](aspnet-mvc-4-mobile-features/_static/image28.png)

## <a name="improving-the-speakers-list"></a>Poprawa listy głośniki

W przeglądarce przenośnym, wybierz **głośniki** łącza. Ponieważ nie istnieje żadne widokiem dla urządzeń przenośnych (*AllSpeakers.Mobile.cshtml*), wyświetlanie głośniki domyślne (*AllSpeakers.cshtml*) jest renderowany przy użyciu widoku układu przenośnych ( *\_ Layout.Mobile.cshtml*).

[![p3_speakersDeskTop](aspnet-mvc-4-mobile-features/_static/image31.png)](aspnet-mvc-4-mobile-features/_static/image30.png)

Widok domyślny (nieprzenośnych) z renderowania w układzie przenośnych globalnie można wyłączyć, ustawiając `RequireConsistentDisplayMode` do `true` w *widoków\\_ViewStart.cshtml* pliku następująco:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample17.cshtml)]

Gdy `RequireConsistentDisplayMode` ma ustawioną wartość `true`, przenośne układu (*\_Layout.Mobile.cshtml*) jest używany tylko dla widoków przenośnych. (Plik widoku jest w formie ***ViewName**. Mobile.cshtml*). Należy ustawić `RequireConsistentDisplayMode` do `true` Jeśli przenośnych układu nie działa prawidłowo w przypadku widoków przeznaczone dla urządzeń przenośnych. Zrzut ekranu poniżej przedstawiono sposób *głośniki* renderowania strony, gdy `RequireConsistentDisplayMode` ma ustawioną wartość `true`.

[![p3_speakersConsistent](aspnet-mvc-4-mobile-features/_static/image33.png)](aspnet-mvc-4-mobile-features/_static/image32.png)

Spójnego trybu wyświetlania w widoku można wyłączyć, ustawiając `RequireConsistentDisplayMode` do `false` w pliku widoku. Następujący kod w *Views\Home\AllSpeakers.cshtml* plików zestawów `RequireConsistentDisplayMode` do `false`:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample18.cshtml)]

## <a name="creating-a-mobile-speakers-view"></a>Tworzenie widoku głośniki przenośnych

Jak został wyświetlony, *głośniki* widoku jest możliwy do odczytu, ale łącza są małe i są trudne do naciśnij na urządzeniu przenośnym. W tej sekcji utworzysz konkretnego mobile *głośniki* widoku prawdopodobnie nowoczesnych aplikacji mobilnej — Wyświetla dużych, łatwe wybranie łączy i zawiera pole wyszukiwania, aby szybko znaleźć głośniki.

Kopiuj *AllSpeakers.cshtml* do *AllSpeakers.Mobile.cshtml*. Otwórz *AllSpeakers.Mobile.cshtml* pliku i usuwania `<h2>` element nagłówka.

W `<ul>` tagów, Dodaj `data-role` atrybutu i ustaw dla niego wartość `listview`. Takich jak inne [ `data-*` atrybuty](http://html5doctor.com/html5-custom-data-attributes/), `data-role="listview"` sprawia, że elementy listy dużych łatwiej naciśnięcie przycisku. To jest ukończone znaczników wygląda następująco:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample19.cshtml)]

Odśwież przeglądarkę dla telefonów. Widok zaktualizowanej wygląda następująco:

[![p3_updatedSpeakerView1](aspnet-mvc-4-mobile-features/_static/image35.png)](aspnet-mvc-4-mobile-features/_static/image34.png)

Chociaż ulepszono widokiem dla urządzeń przenośnych, jest trudno długą listę głośniki. Aby rozwiązać ten problem, w `<ul>` tagów, Dodaj `data-filter` atrybutu i ustaw ją na `true`. Kod poniżej przedstawia `ul` znaczników.

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample20.html)]

Na poniższej ilustracji przedstawiono pola filtru wyszukiwania w górnej części strony, który jest wynikiem `data-filter` atrybutu.

[![ps_Data_Filter](aspnet-mvc-4-mobile-features/_static/image37.png)](aspnet-mvc-4-mobile-features/_static/image36.png)

Podczas wpisywania każdego litera w polu wyszukiwania, jQuery Mobile filtruje wyświetlonej listy, jak pokazano na poniższej ilustracji.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image39.png)](aspnet-mvc-4-mobile-features/_static/image38.png)

## <a name="improving-the-tags-list"></a>Poprawa listy tagów

Wartość domyślna, takich jak *głośniki* widoku *tagi* widoku jest możliwy do odczytu, ale łącza są małe i trudne do naciśnij na urządzeniu przenośnym. W tej sekcji, będzie można rozwiązać *tagi* wyświetlać ten sam sposób, został rozwiązany *głośniki* widoku.

Usuń &quot;Ukryj&quot; sufiks *Views\Home\AllTags.Mobile.cshtml.hide* pliku, tak aby nazwa *Views\Home\AllTags.Mobile.cshtml*. Otwórz plik o zmienionej nazwie i Usuń `<h2>` elementu.

Dodaj `data-role` i `data-filter` atrybuty do `<ul>` tagów, jak pokazano poniżej:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample21.html)]

Na poniższym obrazie pokazano stronie znaczników filtrowanie list `J`.

[![p3_tags_J](aspnet-mvc-4-mobile-features/_static/image41.png)](aspnet-mvc-4-mobile-features/_static/image40.png)

## <a name="improving-the-dates-list"></a>Poprawa listy dat

Można zwiększyć *dat* wyświetlania, takich jak zwiększona możesz *głośniki* i *tagi* widoków, dzięki czemu możliwe jest łatwiejsze do użycia na urządzeniu przenośnym.

Kopiuj *Views\Home\AllDates.cshtml* pliku *Views\Home\AllDates.Mobile.cshtml*. Otwórz nowy plik i Usuń `<h2>` elementu.

Dodaj `data-role="listview"` do `<ul>` tag następująco:

[!code-html[Main](aspnet-mvc-4-mobile-features/samples/sample22.html)]

Na poniższym obrazie pokazano, co **data** prawdopodobnie strona z `data-role` atrybutu w miejscu.

[![p3_dates1](aspnet-mvc-4-mobile-features/_static/image43.png)](aspnet-mvc-4-mobile-features/_static/image42.png) Zastąp zawartość *Views\Home\AllDates.Mobile.cshtml* pliku następującym kodem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample23.cshtml)]

Ten kod grupuje wszystkie sesje według dni. Tworzy linię podziału listy codziennie nowy i wyświetlane są wszystkie sesje dla poszczególnych dni w obszarze linii podziału. Oto, co wygląda po uruchomieniu tego kodu:

[![p3_dates2](aspnet-mvc-4-mobile-features/_static/image45.png)](aspnet-mvc-4-mobile-features/_static/image44.png)

## <a name="improving-the-sessionstable-view"></a>Poprawa widoku SessionsTable

W tej sekcji utworzysz widok mobile określonej sesji. Firma Microsoft zmiany będzie bardziej rozległych niż w innych widokach, który został utworzony.

W przeglądarce przenośnym, wybierz **prelegenta** przycisk, a następnie wprowadź `Sc` w polu wyszukiwania.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image47.png)](aspnet-mvc-4-mobile-features/_static/image46.png)

Wybierz **Scott Hanselman** łącza.

[![p3_scottHa](aspnet-mvc-4-mobile-features/_static/image49.png)](aspnet-mvc-4-mobile-features/_static/image48.png)

Jak widać, ekran jest trudne do odczytu w przeglądarce przenośnych. Data kolumny jest trudny do odczytania i kolumny znaczników leży poza widoku. Aby rozwiązać ten problem, skopiuj *Views\Home\SessionsTable.cshtml* do *Views\Home\SessionsTable.Mobile.cshtml*, a następnie zastąp zawartość pliku następującym kodem:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample24.cshtml)]

Kod usuwa pomieszczeniu znaczniki kolumn i formatuje tytuł, prelegenta i daty w pionie, tak, aby te informacje są przeznaczone do odczytu w przeglądarce przenośnych. Na poniższym obrazie odzwierciedla zmiany kodu.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image51.png)](aspnet-mvc-4-mobile-features/_static/image50.png)

## <a name="improving-the-sessionbycode-view"></a>Poprawa widoku SessionByCode

Na koniec utworzysz widok specyficzne dla mobile *SessionByCode* widoku. W przeglądarce przenośnym, wybierz **prelegenta** przycisk, a następnie wprowadź `Sc` w polu wyszukiwania.

[![ps_data_filter_SC](aspnet-mvc-4-mobile-features/_static/image53.png)](aspnet-mvc-4-mobile-features/_static/image52.png)

Wybierz **Scott Hanselman** łącza. Scott Hanselman sesji są wyświetlane.

[![ps_SessionsByScottHa](aspnet-mvc-4-mobile-features/_static/image55.png)](aspnet-mvc-4-mobile-features/_static/image54.png)

Wybierz **omówienie stosu sieci Web MS Love** łącza.

[![ps_love](aspnet-mvc-4-mobile-features/_static/image57.png)](aspnet-mvc-4-mobile-features/_static/image56.png)

Domyślny widok pulpitu działa poprawnie, ale można ją udoskonalać.

Kopiuj *Views\Home\SessionByCode.cshtml* do *Views\Home\SessionByCode.Mobile.cshtml* i Zastąp zawartość *Views\Home\SessionByCode.Mobile.cshtml*pliku z następujący kod:

[!code-cshtml[Main](aspnet-mvc-4-mobile-features/samples/sample25.cshtml)]

Używa nowego znacznika `data-role` atrybutu zwiększające układ widoku.

Odśwież przeglądarkę dla telefonów. Poniższa ilustracja odzwierciedla zmiany kodu, które zostały wykonane:

[![p3_love2](aspnet-mvc-4-mobile-features/_static/image59.png)](aspnet-mvc-4-mobile-features/_static/image58.png)

## <a name="wrapup-and-review"></a>Wrapup i przejrzyj

Ten samouczek ma wprowadzono nowe funkcje mobilne platformy ASP.NET MVC 4 Developer Preview. Funkcje mobilne obejmują:

- Możliwość zastępowania układu, widoków i widoki częściowe, globalnie i dla poszczególnych widoku.
- Sterowanie układu i częściowe zastąpienie wymuszania przy użyciu `RequireConsistentDisplayMode` właściwości.
- Element widget z tym przełącznikiem widoku dla urządzeń przenośnych widoków nie mogą być także wyświetlane w widokach pulpitu.
- Obsługa pomocniczych przeglądarki, takich jak przeglądarki iPhone.

## <a name="see-also"></a>Zobacz też

- [jQuery Mobile](http://jquerymobile.com) lokacji.
- [jQuery Mobile — omówienie](http://jquerymobile.com/demos/1.0b3/docs/about/intro.html)
- [W3C zalecenie przenośnych sieci Web aplikacji najlepsze rozwiązania](http://www.w3.org/TR/mwabp/)
- [Zalecenie Candidate W3C zapytaniami multimediów](http://www.w3.org/TR/css3-mediaqueries/)
