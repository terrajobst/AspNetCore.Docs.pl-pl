---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
title: Prekompilowanie witryny internetowej (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: 'Program Visual Studio oferuje deweloperom ASP.NET dwoma typami projektów: projekty aplikacji sieci Web (WAPs) i projektów witryny sieci Web (WSPs). Jedną z betwe podstawowych różnic...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: ecd5a4de-beb7-4d1d-bbbb-e31003633267
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-cs
msc.type: authoredcontent
ms.openlocfilehash: abe2c3329129259b9d83cb202fe730eda6abc94d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369437"
---
<a name="precompiling-your-website-c"></a>Prekompilowanie witryny internetowej (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz program Code](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_CS.zip) lub [Pobierz plik PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_cs.pdf)

> Program Visual Studio oferuje deweloperom ASP.NET dwoma typami projektów: projekty aplikacji sieci Web (WAPs) i projektów witryny sieci Web (WSPs). Jest jedną z najważniejszych różnic między dwoma typami projektów, że WAPs muszą mieć kod jawnie skompilowany przed wdrożeniem, natomiast kod w WSP może być automatycznie kompilowane na serwerze sieci web. Jednak istnieje możliwość wstępnej kompilacji WSP przed ich wdrożeniem. W tym samouczku omówiono zalety wstępnej kompilacji i pokazuje, jak przeprowadzać prekompilowanie witryny sieci Web z poziomu programu Visual Studio i w wierszu polecenia.


## <a name="introduction"></a>Wprowadzenie

Program Visual Studio oferuje deweloperom ASP.NET dwóch różnych typach projektów: projekty aplikacji sieci Web (WAP) i projektów witryny sieci Web (WSP). Jedną z podstawowych różnic między tymi typami projektów jest, że WAPs wymagają *kompilację typu explicit* zużycie WSPs *automatyczne kompilowanie*, domyślnie. Za pomocą WAPs, kompilowanie kodu aplikacji sieci web do jednego zestawu, który jest tworzony w witrynie internetowej `Bin` folderu. Wdrożenie pociąga za sobą kopiowanie zawartości kodu znaczników ( `.aspx.ascx`, i `.master` pliki) w projekcie, wraz z zestawu w `Bin` folderu; związane z kodem same pliki klasy nie powinny być wdrożony. Z drugiej strony możesz wdrożyć WSPs przez skopiowanie na stronach adiustacji i ich odpowiednich grup kodu powiązanego do środowiska produkcyjnego. Klasy związane z kodem są kompilowane na żądanie na serwerze sieci web.

> [!NOTE]
> Odwołaj się do sekcji "Kompilacja Versus automatyczne kompilację typu Explicit" [ *określająca, które pliki muszą zostać wdrożone* samouczek](determining-what-files-need-to-be-deployed-cs.md) Aby uzyskać więcej informacji na temat różnic między projektu modele, jawnego i automatycznej kompilacji i jak model kompilacja wpływa na wdrożenie.


Opcja automatycznego kompilacji jest łatwa w użyciu. Istnieje bez kroku kompilację typu explicit, a tylko te pliki, które zostały zmodyfikowane potrzebę wdrożony kompilację typu explicit wymaga wdrażania na stronach adiustacji zmienione i po prostu skompilowanego zestawu. Jednak automatycznego wdrażania ma dwie wady:

- Ponieważ stron muszą być automatycznie skompilowane, gdy najpierw odwiedzenie, może być krótki, ale zauważalnego opóźnienia zleconą strony ASP.NET po raz pierwszy po wdrożeniu.
- Automatyczne kompilowanie wymaga, aby obie deklaratywne znaczników i kod źródłowy istnieje na serwerze sieci web. Może to być nieatrakcyjnych opcję, jeśli planowane jest na sprzedaż aplikacji sieci web klientów, którzy zostanie zainstalowany na serwerach sieci web.

Jeśli albo dwóch powyżej braków wyłączniki transakcji, można albo przełącznika do modelu proxy aplikacji sieci Web lub *prekompilowanie* WSP przed ich wdrożeniem. W tym samouczku sprawdza ż opcje, które najlepiej dopasowane do witryny sieci Web hostowanych i przeprowadzi proces kompilacji wstępnej i wdrożenie wstępnie skompilowanych witryny sieci Web.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Omówienie Generowanie i kompilacja kodu ASP.NET

Zanim przyjrzymy się dostępne opcje wstępnej kompilacji, najpierw Omówmy generowanie kodu i kompilacji, która występuje kiedy strony ASP.NET jest wykonywana po raz pierwszy, ponieważ została utworzona lub ostatnia aktualizacja:. Jak już wiadomo, strony ASP.NET składają się z dwóch części: oznaczeniu deklaracyjnym w `.aspx` pliku i część kodu źródłowego, zwykle znajduje się w pliku klasy oddzielne związanym z kodem (`.aspx.cs`). Czynności wykonywanych przez środowisko uruchomieniowe zleconą strony ASP.NET, zależy od modelu kompilacji aplikacji.

WAPs kod źródłowy stron muszą być jawnie skompilowane w jednym zestawie przed wdrożeniem. Podczas wdrażania tego zestawu i różnych stronach adiustacji są kopiowane do środowiska produkcyjnego. Gdy żądanie dociera do serwera sieci web dla strony ASP.NET, środowisko uruchomieniowe tworzy wystąpienie klasy CodeBehind strony, a następnie wywołuje jego `ProcessRequest` metody, która rozpoczyna się cyklu życia strony, a ostatecznie generuje zawartość strony, który jest zwracany do Obiekt żądający. Środowisko uruchomieniowe można pracować z klasą CodeBehind strony ASP.NET, ponieważ klasy CodeBehind już został skompilowany w zestawie przed ich wdrożeniem.

WSPs oraz automatyczne kompilowanie będzie nie kroku kompilację typu explicit przed ich wdrożeniem. Zamiast tego wdrożenia obejmuje kopiowanie deklaratywne a źródłem zawartości kodu do środowiska produkcyjnego. Gdy żądanie dociera do serwera sieci web dla strony ASP.NET po raz pierwszy od strony została utworzona lub ostatnia aktualizacja, środowisko uruchomieniowe należy najpierw skompilować kodem klasę do zestawu. To skompilowany zestaw jest zapisywany w folderze `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, mimo że lokalizacji tego folderu, mogą być dostosowane za pośrednictwem `<compilation tempDirectory="" />` elementu `<system.web>`, zazwyczaj w `Web.config`. Ponieważ ten zestaw jest zapisywany na dysku, nie musi być ponownie kompilowane dla kolejnych żądań do tej samej strony.

> [!NOTE]
> Jak można oczekiwać, ma niewielkie opóźnienie podczas żądania strony po raz pierwszy (lub po raz pierwszy, ponieważ zostanie on zmieniony) w lokacji, która używa automatyczne kompilowanie, jak może potrwać chwilę dla serwera, aby skompilować kod strony i zapisać wynikowego zestawu dysk.


Krótko mówiąc za pomocą kompilację typu explicit należy skompilować kod źródłowy witryny sieci Web przed przystąpieniem do wdrożenia, zapisywanie środowiska uruchomieniowego trzeba wykonać ten krok. Automatyczne kompilowanie środowisko wykonawcze obsługuje kompilację kodu źródłowego stron, ale bez kosztów nieznaczne inicjowania dla pierwszej wizyty na stronie od czasu utworzenia lub ostatniej aktualizacji.

Ale co deklaratywne część strony ASP.NET ( `.aspx` pliku)? Jest oczywiste, że istnieje relacja między `.aspx` pliki i kod w ich klasy związane z kodem, jako określone w oznaczeniu deklaracyjnym kontrolki sieci Web są dostępne w kodzie. Jest również oczywisty, zawartość `.aspx` renderowanego kodu znaczników, generowane przez stronę znacznie wpływa na pliki. W jaki sposób działa środowisko uruchomieniowe na tekst, HTML, i składnia formantu sieci Web zdefiniowanych w `.aspx` plik do generowania żądaną stronę użytkownika renderowana zawartość?

Nie chcesz zbyt odwróciły na szczegóły niskiego poziomu implementacji, które różnią się między WAPs i WSPs, ale mówiąc środowisko wykonawcze automatycznie generuje plik klasy, który zawiera różne formanty Web jako chronione elementy członkowskie i metody. Ten wygenerowany plik jest implementowany jako *klasy częściowej* do odpowiedniej klasy związane z kodem. ([Klas częściowych](http://www.dotnet-guide.com/partialclasses.html) umożliwienia zawartość jednej klasy być rozkładane na wiele plików.) W związku z kodem klasę jest zdefiniowany w dwóch miejscach: w `.aspx.cs` plik, a w tej klasie generowanych automatycznie utworzone przez środowisko uruchomieniowe. Ta klasa generowanych automatycznie znajduje się w `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` folderu.

Ważne wynos tutaj jest to, że dla strony ASP.NET mógł być renderowany przez środowisko uruchomieniowe zarówno jego deklaracyjne i fragmenty kodu źródłowego, muszą być skompilowane do zestawu. WAPs jawnego kompilowania kodu źródłowego do zestawu przed wdrożeniem, ale nadal oznaczeniu deklaracyjnym musi być konwertowany do kodu i kompilowane w czasie wykonywania na serwerze sieci web. WSPs przy użyciu automatycznej kompilacji zarówno w kodzie źródłowym, jak i w oznaczeniu deklaracyjnym wymaga zbierane przez serwer sieci web.

Istnieje możliwość kompilację typu explicit za pomocą modelu WSP. Można jawnie kompilować części kodu źródłowego, jak za pomocą modelu WAP. Co więcej można również skompilować oznaczeniu deklaracyjnym.

## <a name="precompilation-options"></a>Opcje wstępnej kompilacji

.NET Framework, który jest dostarczany z [narzędzia kompilacji platformy ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/library/ms229863.aspx) , pozwala na kompilowanie kodu źródłowego (i nawet zawartości) aplikacja ASP.NET utworzone przy użyciu modelu WSP. To narzędzie, został wydany przy użyciu platformy .NET Framework w wersji 2.0 i znajduje się w `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` folderu; mogą być używane z poziomu wiersza polecenia lub uruchamiać z poziomu programu Visual Studio za pomocą opcji publikowania witryny sieci Web menu Kompiluj.

Narzędzie kompilacji zawiera dwa rodzaje ogólne kompilacji:- i w miejscu wstępnej kompilacji wstępnej kompilacji do wdrożenia. Dzięki wstępnej kompilacji w miejscu możesz uruchamiać `aspnet_compiler.exe` narzędzie z wiersza polecenia, a następnie określ ścieżkę do katalogu wirtualnego lub ścieżka fizyczna witryny sieci Web, która znajduje się na tym komputerze. Narzędzia kompilacji kompiluje następnie każdej strony ASP.NET w projekcie, przechowywanie wersji skompilowanej w `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` folderu, podobnie jak przypadku strony ma każdy były odwiedzane przez po raz pierwszy w przeglądarce. W miejscu wstępnej kompilacji można przyspieszyć pierwsze żądanie, wprowadzone do nowo wdrożonym stron ASP.NET w witrynie, ponieważ eliminuje ona środowiska uruchomieniowego musi wykonać ten krok. Jednak wstępnej kompilacji w miejscu nie jest przydatne w przypadku większości hostowanej witryn sieci Web, ponieważ wymaga, że jesteś w stanie uruchamiania programów, z serwera sieci web wiersza polecenia. Ten poziom dostępu nie jest dozwolona w udostępnionych środowiskach hostingu.

> [!NOTE]
> Aby uzyskać więcej informacji o wstępnej kompilacji w miejscu, zapoznaj się [How to: Prekompilowanie witryny sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/ms227972.aspx) i [wstępnej kompilacji w programie ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Zamiast kompilowania stron w witrynie sieci Web do `Temporary ASP.NET Files` folderu, kompiluje wstępnej kompilacji dla wdrożenia stron do katalogu wybrane, w formacie, który można wdrożyć w środowisku produkcyjnym.

Istnieją dwa odmian wstępnej kompilacji do wdrożenia, którą omówimy w tym samouczku: wstępnej kompilacji za pomocą interfejsu użytkownika można aktualizować i wstępnej kompilacji za pomocą interfejsu użytkownika nie nadaje się do aktualizacji. Wstępnej kompilacji za pomocą interfejsu użytkownika można aktualizować pozostawia oznaczeniu deklaracyjnym w `.aspx`, `.ascx`, i `.master` plików, umożliwiając w ten sposób dla deweloperów wyświetlić i, jeśli to konieczne, należy zmodyfikować oznaczeniu deklaracyjnym na serwerze produkcyjnym. Generuje wstępnej kompilacji za pomocą interfejsu użytkownika nie można zaktualizować `.aspx` stron, które są nieważne żadnej zawartości i usuwa `.ascx` i `.master` plików, a tym samym ukrywanie oznaczeniu deklaracyjnym i uniemożliwiają zmianę z deweloperem środowiska produkcyjnego.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Prekompilowanie dla wdrożenia przy użyciu interfejsu użytkownika można aktualizować

Najlepszym sposobem zrozumienia wstępnej kompilacji wdrożenia jest aby zobaczyć przykład działania. Spróbujmy prekompilowanie WSP przeglądy książki do wdrożenia przy użyciu interfejsu użytkownika można aktualizować. Narzędzia kompilacji platformy ASP.NET można wywołać z menu kompilacji programu Visual Studio lub z wiersza polecenia. Tej części są rozpatrywane za pomocą narzędzia z poziomu programu Visual Studio; w sekcji "wstępnej kompilacji z wiersza polecenia" patrzy na uruchomione narzędzie kompilatora z wiersza polecenia.

Otwórz WSP przeglądu książki w programie Visual Studio, przejdź do menu kompilacji i wybierz opcję menu publikowanie witryny sieci Web. Spowoduje to uruchomienie okna dialogowego publikowania witryny sieci Web (zobacz **rys.1**), w którym można określić lokalizacji docelowej, czy interfejs użytkownika witryny wstępnie skompilowanym nadaje się do i inne opcje narzędzia kompilatora. Lokalizacji docelowej mogą być zdalnym serwerze sieci web lub serwer FTP, ale na razie wybierz folder na dysku twardym komputera. Ponieważ chcemy uruchomić przy użyciu interfejsu użytkownika można aktualizować, pozostaw pole wyboru "Zezwalaj na tej prekompilowanej witryny zezwalać na aktualizacje", zaznaczone, a następnie kliknij przycisk OK.

[![](precompiling-your-website-cs/_static/image2.png)](precompiling-your-website-cs/_static/image1.png)

**Rysunek 1**: narzędzia kompilacji platformy ASP.NET będzie Prekompilowanie witryny sieci Web do określona lokalizacja docelowa  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](precompiling-your-website-cs/_static/image3.png))

> [!NOTE]
> Opcja publikowania witryny sieci Web, w menu kompilacja nie jest dostępne w Visual Web Developer. Jeśli używasz programu Visual Web Developer, będą musieli używać wersji wiersza polecenia narzędzia kompilacji platformy ASP.NET, co zostało omówione w sekcji "wstępnej kompilacji z wiersza polecenia".


Po wstępnej kompilacji witryny sieci Web, przejdź do lokalizacji docelowej, która została wprowadzona w oknie dialogowym Publikowanie witryny sieci Web. Poświęć chwilę, aby porównać zwartość tego katalogu z zawartością witryny sieci Web. **Rysunek 2** pokazuje przeglądy książki folderu witryny sieci Web. Należy pamiętać, że zawiera ona `.aspx` i `.aspx.cs` plików. Ponadto należy pamiętać, że `Bin` katalogu zawiera tylko jeden plik `Elmah.dll`, dodanego w [poprzedni Samouczek](logging-error-details-with-elmah-cs.md)

[![](precompiling-your-website-cs/_static/image5.png)](precompiling-your-website-cs/_static/image4.png)

**Rysunek 2**: katalog projektu zawiera `.aspx` i `.aspx.cs` plików; `Bin` Folder zawiera po prostu `Elmah.dll`  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](precompiling-your-website-cs/_static/image6.png))

**Rysunek 3** widać folder lokalizacji docelowych, których zawartość zostały utworzone za pomocą narzędzia kompilacji platformy ASP.NET. Ten folder nie zawiera żadnych plików z kodem. Ponadto ten folder `Bin` katalogu obejmuje kilka zestawów i dwóch `.compiled` pliki oprócz `Elmah.dll` zestawu.

[![](precompiling-your-website-cs/_static/image8.png)](precompiling-your-website-cs/_static/image7.png)

**Rysunek 3**: Folder lokalizacji docelowych zawiera pliki do wdrożenia  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](precompiling-your-website-cs/_static/image9.png))

W odróżnieniu od kompilację typu explicit w WAPs wstępnej kompilacji procesu wdrażania nie tworzy jeden zestaw dla całej lokacji. Zamiast tego należy go partii ze sobą kilka stron do każdego zestawu. Również kompiluje `Global.asax` plików (jeśli istnieje) w ich własnych zestawach, a także wszystkie klasy w `App_Code` folderu. Pliki, które pełnią oznaczeniu deklaracyjnym dla platformy ASP.NET sieci web, stron, kontrolek użytkownika i stron wzorcowych (`.aspx`, `.ascx`, i `.master` pliki odpowiednio) są kopiowane w postaci-jest katalog lokalizacji docelowej. Podobnie `Web.config` plik jest kopiowany przez, wraz z plików statycznych, takich jak obrazy, klas CSS i pliki PDF. Opis bardziej formalne jak narzędzie kompilacji obsługuje różne typy plików, zobacz [plików obsługi podczas ASP.NET wstępnej kompilacji](https://msdn.microsoft.com/library/e22s60h9.aspx).

> [!NOTE]
> Można poinstruować narzędzia kompilacji, aby utworzyć jeden zestaw dla strony ASP.NET, formant użytkownika lub strony wzorcowej, zaznaczając pole wyboru "Używana stała nazewnictwa i zestawów pojedynczej strony" w oknie dialogowym Publikowanie witryny sieci Web. Po każdej stronie ASP.NET skompilowany w ich własnych zestawach umożliwia bardziej precyzyjną kontrolę nad wdrażaniem. Na przykład, jeśli utworzono pojedyncza strona sieci web platformy ASP.NET i potrzebne do wdrożenia tej zmiany, należy tylko wdrożeniem tej strony `.aspx` plików i skojarzony zestaw do środowiska produkcyjnego. Zapoznaj się z [instrukcje: generowanie stałe nazwy za pomocą narzędzia kompilacji platformy ASP.NET](https://msdn.microsoft.com/library/ms228040.aspx) Aby uzyskać więcej informacji.


Katalog lokalizacji docelowej zawiera także plik, który nie jest częścią projektu sieci web wstępnie skompilowanych, to znaczy `PrecompiledApp.config`. Ten plik informuje, środowisko uruchomieniowe programu ASP.NET, że aplikacja została wstępnie skompilowany i czy został prekompilowanych za pomocą interfejsu użytkownika można aktualizować lub południe-nadaje się do aktualizacji.

Na koniec, Poświęć chwilę, aby otworzyć jeden z `.aspx` plików w lokalizacji docelowej przy użyciu programu Visual Studio lub z wybranym edytorze tekstów. Po wstępnej kompilacji do wdrożenia przy użyciu interfejsu użytkownika można aktualizować, strony ASP.NET, w katalogu docelowym lokalizacji zawierają dokładnie ten sam kod znaczników jako odpowiadające im pliki w witrynie sieci Web.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Prekompilowanie dla wdrożenia przy użyciu interfejsu użytkownika nie nadaje się do aktualizacji

Narzędzia kompilatora platformy ASP.NET można również przeprowadzać prekompilowanie witryny dla wdrożenia przy użyciu nie nadaje się do aktualizacji interfejsu użytkownika. Prekompilowanie witryny nie można zaktualizować interfejs użytkownika działa podobnie do wstępnej kompilacji za pomocą nadaje się do aktualizacji interfejsu użytkownika, główną różnicą, że strony ASP.NET, kontrolki użytkownika i stron wzorcowych w katalogu docelowym są usuwane z ich kodu znaczników. Prekompilowanie witryny sieci Web do wdrożenia przy użyciu nie nadaje się do aktualizacji interfejsu użytkownika, wybierz opcję Publikuj witrynę sieci Web z menu kompilacji, ale Usuń zaznaczenie opcji "Zezwalaj na tej prekompilowanej witryny zezwalać na aktualizacje" (zobacz **rysunek 4**).

[![](precompiling-your-website-cs/_static/image11.png)](precompiling-your-website-cs/_static/image10.png)

**Rysunek 4**: Usuń zaznaczenie pola wyboru "Zezwalaj na tej prekompilowanej witryny zezwalać na aktualizacje" opcja do wstępnej kompilacji za pomocą można aktualizować bez interfejsu użytkownika  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](precompiling-your-website-cs/_static/image12.png))

**Rysunek 5** pokazuje folder lokalizacji docelowych po wstępnej kompilacji za pomocą interfejsu użytkownika nie nadaje się do aktualizacji.

[![](precompiling-your-website-cs/_static/image14.png)](precompiling-your-website-cs/_static/image13.png)

**Rysunek 5**: Folder docelowy lokalizacji wdrożenia za pomocą nie można zaktualizować interfejsu użytkownika  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](precompiling-your-website-cs/_static/image15.png))

Porównaj **rysunek 3** do **rysunek 5**. Gdy dwa foldery mogą wyglądała identycznie, należy pamiętać, brakuje w folderze nie nadaje się do aktualizacji interfejsu użytkownika strony wzorcowej `Site.master`. I otrzymało **rysunek 5** zawiera różne strony ASP.NET, czy wyświetlać zawartość tych plików, zobaczysz, że zostały one usunięte z ich oznaczeniu deklaracyjnym i zastąpiony tekst symbolu zastępczego: "to jest plik znacznika, generowany przez Narzędzie wstępnej kompilacji i nie należy usuwać! "

[![](precompiling-your-website-cs/_static/image17.png)](precompiling-your-website-cs/_static/image16.png)

**Rysunek 5**: oznaczeniu deklaracyjnym został usunięty ze strony ASP.NET

`Bin` Folderów w **rysunki 3** i **5** bardziej istotnie różnią się. Oprócz zestawów `Bin` folderu w **rysunek 5** obejmuje `.compiled` pliku dla każdej strony ASP.NET, formant użytkownika i strony wzorcowej.

Prekompilowanie witryny nie można zaktualizować interfejs użytkownika jest przydatne w sytuacji, gdy nie zawartość strony ASP.NET, aby zostać zmodyfikowane przez osoby lub firmy, który instaluje ani nie zarządza witryny sieci Web w środowisku produkcyjnym. Jeśli tworzysz aplikację sieci web platformy ASP.NET, która sprzedawać użytkowników do zainstalowania na serwerach sieci web, warto zapewnić ich przez bezpośrednią edycję nie należy modyfikować wygląd i działanie witryny `.aspx` stron odeślemy je. Przez prekompilowanie witryny internetowej za pomocą nie można zaktualizować interfejsu użytkownika, dostarczaj symbol zastępczy `.aspx` stron w ramach instalacji, zapobiegając w ten sposób klienci badanie i modyfikowanie ich zawartości.

### <a name="precompiling-from-the-command-line"></a>Prekompilowanie z wiersza polecenia

W tle, okno dialogowe publikowania witryny sieci Web programu Visual Studio wywołuje narzędzia kompilacji platformy ASP.NET (`aspnet_compiler.exe`) przeprowadzać prekompilowanie witryny sieci Web. Alternatywnie można wywołać to narzędzie z wiersza polecenia. W rzeczywistości Jeśli używasz Visual Web Developer należy uruchomić narzędzie kompilatora z wiersza polecenia jako menu kompilacja programu Visual Web Developer's nie ma opcji publikowania witryny sieci Web.

Aby korzystać z narzędzia kompilatora z wiersza polecenia, należy rozpocząć od rezygnację z użycia w wierszu polecenia i przejdź do katalogu struktury `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Następnie wprowadź następującą instrukcję, w wierszu polecenia:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Powyższe polecenie uruchamia narzędzie kompilatora platformy ASP.NET (`aspnet_compiler.exe`) i za pośrednictwem `-p` przełącznika, powoduje, że jego prekompilowanie witryny sieci Web na *fizycznych\_ścieżki\_do\_aplikacji*; Ta wartość będzie podobny do `C:\MySites\BookReviews`i powinny być rozdzielone znakami cudzysłowu.

`-v` Przełącznik określa katalog wirtualny witryny. Jeśli witryna jest zarejestrowana jako domyślna witryna sieci Web w metabazie IIS, wówczas można pominąć `-p` przełącznika i po prostu Określ wirtualnego katalogu aplikacji. Jeśli używasz `-p` Przełącz postępowania wartość `-v` przełącznika wskazuje katalog główny witryny sieci Web i jest używany do rozpoznawania odwołań do katalogu głównego aplikacji. Na przykład jeśli określona wartość `-v /MySite` odwołuje się w aplikacji, aby `~/path/file` zostanie rozpoznana jako `~/MySite/path/file`. Ponieważ witryna przeglądy książki znajduje się w katalogu głównym w mojej firmie hostingu w sieci web została wykorzystana przełącznika `-v /`.

`-f` Przełącznika, jeśli jest obecny, powoduje, że narzędzie kompilację, aby zastąpić *docelowej\_lokalizacji\_folderu* katalogu, jeśli już istnieje. Jeżeli pominięto `-f` przełącznika i folder lokalizacji docelowej już istnieje, narzędzie kompilacji zostanie zamknięta z powodu błędu: "Błąd ASPRUNTIME: katalog docelowy nie jest pusty. Usuń je ręcznie lub wybierz inną docelową."

`-u` Przełącznika, jeśli jest obecny, informuje o narzędzia do tworzenia interfejsu użytkownika można aktualizować. Pomiń ten przełącznik, aby uruchomić interfejs użytkownika nie można zaktualizować.

Na koniec *docelowej\_lokalizacji\_folderu* to ścieżka fizyczna do katalogu docelowego lokalizacji; ta wartość będzie podobny do `C:\MySites\Output\BookReviews`i powinny być rozdzielone znakami cudzysłowu.

## <a name="deploying-the-precompiled-website"></a>Wdrażanie wstępnie skompilowanym witryny sieci Web

W tym momencie Zaobserwowaliśmy jak prekompilowanie witryny sieci Web przy użyciu zarówno opcji interfejsu użytkownika można aktualizować i nie można zaktualizować za pomocą narzędzia kompilacji platformy ASP.NET. Jednak naszych przykładach dotychczasowych istnieje wstępnie skompilowany witryny sieci Web do folderu lokalnego, a nie w środowisku produkcyjnym. Dobra wiadomość jest wdrożenie wstępnie skompilowanych witryny sieci Web jest szybka i bezproblemowa i może odbywać się za pomocą programu Visual Studio lub innym mechanizmem pliku kopii, takie jak w kliencie FTP autonomicznych.

Okno dialogowe publikowania witryny sieci Web (najpierw pokazana w **rys.1**) ma opcja lokalizacji docelowej, która wskazuje, gdzie pliki wstępnie skompilowanym witryny sieci Web są kopiowane do. Ta lokalizacja może być zdalnym serwerze sieci web lub serwera FTP. Serwer zdalny do tego pola tekstowego wstępnie kompiluje i wdraża witryny sieci Web do określonego serwera w jednym kroku. Alternatywnie możesz prekompilowanie witryny sieci Web do folderu lokalnego i następnie ręcznie skopiuj zawartość tego folderu do środowiska produkcyjnego, za pośrednictwem protokołu FTP lub niektóre inne podejście.

Posiadanie wstępnie skompilowanych witryny sieci Web automatycznie wdrożoną za pomocą okno dialogowe publikowania witryny sieci Web programu Visual Studio jest przydatne w przypadku prostych witryn w przypadku, gdy nie istnieją różnice konfiguracji między środowiskami deweloperskim i produkcyjnym. Jednak jak zanotowane w [ *typowych konfiguracji różnice między deweloperskim i produkcyjnym* samouczek](common-configuration-differences-between-development-and-production-cs.md) nie jest niczym niezwykłym różnice istnieje. Na przykład przeglądów książki aplikacji sieci web używa innej bazy danych w środowisku produkcyjnym niż w środowisku programistycznym. Gdy program Visual Studio publikuje witryny sieci Web na serwerze zdalnym bezrefleksyjne kopiuje informacje dotyczące pliku konfiguracji w środowisku programistycznym.

Dla witryn o różnice konfiguracji między środowiskami deweloperskim i produkcyjnym ona najlepszym rozwiązaniem może być prekompilowanie witryny do katalogu lokalnego, skopiuj pliki konfiguracji specyficznej dla produkcji, a następnie skopiuj zawartość wstępnie skompilowanych danych wyjściowych Produkcja.

Przypomnienia informacji na temat kopiowania plików ze środowiska projektowego do środowiska produkcyjnego można znaleźć [ *Wdrażanie witryny sieci Web przy użyciu klienta FTP* ](deploying-your-site-using-an-ftp-client-cs.md) i [  *Wdrażanie usługi witryny sieci Web przy użyciu programu Visual Studio* ](determining-what-files-need-to-be-deployed-cs.md) samouczków.

## <a name="summary"></a>Podsumowanie

Program ASP.NET obsługuje dwa tryby kompilacji: automatyczne i jawne. Zgodnie z opisem w poprzednich samouczkach, projektów aplikacji sieci Web (WAPs) Użyj kompilację typu explicit, natomiast projektów witryny sieci Web (WSPs) domyślnie używają automatyczne kompilowanie. Jednak jest możliwe jawnie opracowanie WSP przed ich wdrożeniem za pomocą narzędzia kompilacji platformy ASP.NET.

Ten samouczek koncentruje się na narzędzia do kompilacji wstępnej kompilacji, do obsługi wdrożenia. Po wstępnej kompilacji do wdrożenia, narzędzia kompilacji tworzy folder lokalizacji docelowych, kompiluje kod źródłowy aplikacji sieci web określonej i kopiuje je skompilowanych zestawów i plików zawartości do lokalizacji folderu docelowego. Narzędzia kompilacji można skonfigurować do tworzenia interfejsu użytkownika można aktualizować lub nie nadaje się do aktualizacji. Prekompilowanie opcji interfejsu użytkownika nie można zaktualizować, usunięcie oznaczeniu deklaracyjnym plików zawartości. Mówiąc wstępnej kompilacji pozwala na wdrażanie aplikacji na podstawie projektu witryny sieci Web, bez uwzględniania żadnych plików kodu źródłowego i oznaczeniu deklaracyjnym usunięte, w razie potrzeby.

Wszystkiego najlepszego programowania!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Wstępnej kompilacji witryny sieci Web platformy ASP.NET](https://msdn.microsoft.com/library/ms228015.aspx)
- [Plik CodeBehind i kompilacja na platformie ASP.NET 2.0](https://msdn.microsoft.com/magazine/cc163675.aspx)
- [Wstępnej kompilacji w programie ASP.NET:](http://www.odetocode.com/Articles/417.aspx)
- [Opcje prekompilowanej witryny w programie ASP.NET:](http://www.dotnetperls.com/precompiled)

> [!div class="step-by-step"]
> [Poprzednie](logging-error-details-with-elmah-cs.md)
> [dalej](users-and-roles-on-the-production-website-cs.md)
