---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
title: Prekompilowanie witryny sieci Web (VB) | Dokumentacja firmy Microsoft
author: rick-anderson
description: "Program Visual Studio oferuje deweloperom ASP.NET dwa typy projektów: projekty aplikacji sieci Web (WAPs) i projektów witryny sieci Web (WSPs). Jeden z podstawowych różnic betwe..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: c285dc6f-a1c6-46e6-ac03-3830947f57e3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/precompiling-your-website-vb
msc.type: authoredcontent
ms.openlocfilehash: e9f2e2d71815a2e8f17d3c505b48b69a23bceb1c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/10/2017
---
<a name="precompiling-your-website-vb"></a>Prekompilowanie witryny sieci Web (VB)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz kod](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_15_VB.zip) lub [pobierania plików PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial15_Precompiling_vb.pdf)

> Program Visual Studio oferuje deweloperom ASP.NET dwa typy projektów: projekty aplikacji sieci Web (WAPs) i projektów witryny sieci Web (WSPs). Jeden z podstawowych różnic między dwa typy projektów jest WAPs musi kodu jawnie skompilowany przed ich wdrożeniem, natomiast kod w WSP mogą być automatycznie kompilowane na serwerze sieci web. Istnieje możliwość wstępnej kompilacji WSP przed ich wdrożeniem. W tym samouczku Eksploruje zalet wstępnej kompilacji i pokazuje, jak wstępnej kompilacji witryny sieci Web z poziomu programu Visual Studio i z wiersza polecenia.


## <a name="introduction"></a>Wprowadzenie

Visual Studio oferuje deweloperom ASP.NET dwa typy inny projekt: projekty aplikacji sieci Web (WAP) i projektów witryny sieci Web (WSP). Jeden z podstawowych różnic między tymi typami projektów jest, że WAPs wymaga *kompilację typu explicit* stosowania WSPs *automatyczne kompilowanie*, domyślnie. Z WAPs, kompilacja kodu aplikacji sieci web w jednym zestawie, który jest tworzony w witrynie sieci Web `Bin` folderu. Wdrożenie pociąga za sobą kopiowania zawartości znacznika ( `.aspx.ascx`, i `.master` pliki) w projekcie, wraz z zestawu w `Bin` folderu; CodeBehind klasy same pliki nie muszą zostać wdrożone. Z drugiej strony możesz wdrożyć WSPs przez skopiowanie zarówno znaczników stron, jak i ich odpowiednich grup kodu powiązanego do środowiska produkcyjnego. Klasy związane z kodem są kompilowane na żądanie na serwerze sieci web.

> [!NOTE]
> Odwołaj się do sekcji "Kompilacji Versus automatyczne kompilację typu Explicit" [ *określania co pliki potrzebne do wdrożenia* samouczek](determining-what-files-need-to-be-deployed-vb.md) w tle więcej na temat różnic między projektu modele, jawne i automatyczne kompilacji i wpływ wdrożenia modelu kompilacji.


Opcja automatycznego kompilacji jest łatwy w użyciu. Brak nie krok kompilację typu explicit i tylko te pliki, które zostały zmodyfikowane muszą wdrożony kompilację typu explicit wymaga wdrażania strony zmienione znaczników i po prostu skompilowanego zestawu. Jednak wdrażania automatycznego ma dwie wady:

- Ponieważ stron muszą być automatycznie skompilowane, gdy są one najpierw odwiedzane, może istnieć krótki, ale zauważalnego opóźnienia zleconą strony ASP.NET po raz pierwszy po wdrożeniu.
- Automatyczne kompilowanie wymaga, aby obie deklaratywne znaczników i kod źródłowy znajdującego się na serwerze sieci web. Może to być nieatrakcyjnych opcję, jeśli planujesz sprzedaży aplikacji sieci web do klientów, którzy zostanie zainstalowany na serwerach sieci web.

Jeśli albo dwa powyżej niedociągnięć są podziałów transakcji, możesz Przełącz się do modelu WAP lub *prekompilowanie* WSP przed ich wdrożeniem. W tym samouczku sprawdza wstępnej kompilacji opcje najlepiej nadaje się do witryny sieci Web hostowanej i przeprowadzi Cię przez proces wstępnej kompilacji i wdrożenia prekompilowany witryny sieci Web.

## <a name="an-overview-of-aspnet-code-generation-and-compilation"></a>Omówienie Generowanie i kompilacja kodu ASP.NET

Zanim przyjrzymy dostępne opcje wstępnej kompilacji umożliwia najpierw porozmawiać na temat generowania kodu i kompilacji, który występuje w przypadku żądania strony ASP.NET po raz pierwszy, ponieważ została utworzona lub ostatniej aktualizacji. Wiesz, stron ASP.NET składają się z dwóch części: deklaratywne znaczników w `.aspx` plików; i części kodu źródłowego, zwykle w pliku klasy oddzielne kodem (`.aspx.vb`). Czynności wykonywanych w czasie wykonywania, gdy zostanie zażądana strona ASP.NET zależy od aplikacji kompilacji modelu.

WAPs kod źródłowy stron muszą być jawnie skompilowane w jednym zestawie przed wdrożeniem. Podczas wdrażania tego zestawu i na różnych stronach znaczników są kopiowane do środowiska produkcyjnego. Gdy żądanie dociera do serwera sieci web dla strony platformy ASP.NET, środowisko uruchomieniowe tworzy wystąpienie klasy związane z kodem strony, a następnie wywołuje jego `ProcessRequest` metodę, która rozpoczyna się cyklu życia strony, a ostatecznie generuje strony zawartości, który jest zwracany do Obiekt żądający. Środowisko uruchomieniowe może współpracować z strony ASP.NET klasę kodu, ponieważ klasy związane z kodem już został skompilowany w zestawie przed ich wdrożeniem.

WSPs i automatyczne kompilacji jest nie krok kompilację typu explicit przed ich wdrożeniem. Zamiast tego wdrożenia obejmuje kopiowanie deklaratywne i zawartość źródłową kod do środowiska produkcyjnego. Gdy żądanie dociera do serwera sieci web dla strony platformy ASP.NET po raz pierwszy, ponieważ strona została utworzona lub ostatniej aktualizacji, środowisko uruchomieniowe należy najpierw skompilować klasę kodu w zestawie. Ta skompilowanego zestawu jest zapisywany w folderze `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files`, mimo że lokalizacji tego folderu można dostosować za pomocą `<pages>` element `Web.config`. Ponieważ ten zestaw jest zapisywany na dysku, nie musi być ponownie kompilowane w kolejnych żądań wysyłanych do tej samej strony.

> [!NOTE]
> Jak można oczekiwać występuje niewielkie opóźnienie podczas żądania strony po raz pierwszy (lub po raz pierwszy, ponieważ jest zostało zmienione) w lokacji, która używa automatyczne kompilowanie zajmuje chwilę dla serwera, aby skompilować kod strony i zapisać wynikowego zestawu do dysk.


Krótko mówiąc z kompilację typu explicit należy skompilować kod źródłowy witryny sieci Web przed wdrożeniem, zapisywanie środowiska uruchomieniowego z konieczności wykonania tego kroku. Automatyczne kompilowanie środowiska uruchomieniowego obsługuje kompilacji kodu źródłowego stron, ale kosztem nieznaczne inicjowania dla pierwszej wizyty na stronie od czasu utworzenia lub ostatniej aktualizacji.

Ale co deklaratywne część strony ASP.NET ( `.aspx` pliku)? Jest jasne, czy istnieje relacja między `.aspx` pliki i kod w ich klasy związane z kodem, jako formantów sieci Web zdefiniowany w znaczniku deklaratywne są dostępne w kodzie. Jest również oczywisty który zawartość `.aspx` pliki znacząco wpływa renderowanego kodu znaczników generowane przez stronę. W jaki sposób działa środowiska uruchomieniowego na tekst, HTML, i składni formantu sieci Web zdefiniowany w `.aspx` plik do wygenerowania żądanej strony do renderowania zawartości?

Nie chcę zbyt odwróciły na szczegóły implementacji niskiego poziomu, które różnią się między WAPs i WSPs, ale mówiąc środowiska uruchomieniowego automatycznie generuje plik klasy, który zawiera różne formantów sieci Web jako chronione elementy członkowskie i metody. Ten plik wygenerowany zaimplementowano jako *częściowej klasy* do odpowiedniej klasy związane z kodem. ([Klasy częściowe](http://www.dotnet-guide.com/partialclasses.html) umożliwienia zawartość jednej klasy było ich rozmieszczenie wielu plików.) W związku z tym kodem — klasa jest zdefiniowana w dwóch miejscach: w `.aspx.vb` plik, a w tej klasie generowanych automatycznie utworzone przez środowisko uruchomieniowe. Ta klasa generowanych automatycznie są przechowywane w `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` folderu.

Ważne odebranie tutaj jest to, że dla strony platformy ASP.NET mają być świadczone przez środowisko uruchomieniowe zarówno jego deklaratywne i fragmenty kodu źródłowego musi zostać skompilowany w zestawie. WAPs kod źródłowy jest jawnie skompilowany w zestawie przed ich wdrożeniem, ale nadal deklaratywne znaczników muszą być konwertowane na kod i kompilowane w czasie wykonywania na serwerze sieci web. WSPs przy użyciu automatycznego kompilacji zarówno kod źródłowy, jak i deklaratywne znacznika wymaga zbierane przez serwer sieci web.

Istnieje możliwość użycia z modelem WSP kompilację typu explicit. Fragment kodu źródłowego, jak można jawnie kompilacji, z modelu WAP. Co więcej będzie można kompilować deklaratywne znaczników.

## <a name="precompilation-options"></a>Opcje wstępnej kompilacji

.NET Framework jest dostarczany z [narzędzia kompilacji platformy ASP.NET (`aspnet_compiler.exe`)](https://msdn.microsoft.com/en-us/library/ms229863.aspx) , umożliwia kompilowanie kodu źródłowego (i nawet zawartości) aplikacja ASP.NET, utworzony za pomocą modelu WSP. To narzędzie został zwolniony z .NET Framework w wersji 2.0 i znajduje się w `%WINDIR%\Microsoft.NET\Framework\v2.0.50727` folder; można użyć w wierszu polecenia lub uruchamiany z poziomu programu Visual Studio za pomocą opcji publikowania witryny sieci Web menu Kompiluj.

To narzędzie kompilacji zawiera dwie różne formy kompilacji: w miejscu wstępnej kompilacji i wstępnej kompilacji dla wdrożenia. Za pomocą wstępnej kompilacji w miejscu będzie uruchomić `aspnet_compiler.exe` narzędzie z wiersza polecenia i określ ścieżkę do katalogu wirtualnego lub ścieżka fizyczna witryny sieci Web, która znajduje się na komputerze. Narzędzia kompilacji następnie kompiluje każdej strony ASP.NET w projekcie, przechowywanie wersji skompilowanej `%WINDIR%\Microsoft.NET\Framework\v2.0.50727\Temporary ASP.NET Files` folderu tak, jeśli strony ma każdego odwiedzone po raz pierwszy w przeglądarce. W miejscu wstępnej kompilacji można przyspieszyć pierwsze żądanie, wprowadzone do nowo wdrożonym stron ASP.NET w witrynie, ponieważ jego eliminuje środowiska uruchomieniowego musi wykonać ten krok. Jednak w miejscu wstępnej kompilacji nie jest przydatne w przypadku większości witryn sieci Web hostowanej, ponieważ wymaga ona, że jesteś w stanie uruchamianie programów z serwera sieci web wiersza polecenia. Ten poziom dostępu nie jest dozwolona w udostępnionych środowiskach hostingu.

> [!NOTE]
> Aby uzyskać więcej informacji o wstępnej kompilacji w miejscu, zapoznaj się z [jak: wstępnej kompilacji witryny sieci Web ASP.NET](https://msdn.microsoft.com/en-us/library/ms227972.aspx) i [wstępnej kompilacji w programie ASP.NET 2.0](http://www.odetocode.com/Articles/417.aspx).


Zamiast kompilowania stron w witrynie sieci Web do `Temporary ASP.NET Files` folderu, kompiluje wstępnej kompilacji dla wdrożenia stron do katalogu wybrane i w formacie, który można wdrożyć w środowisku produkcyjnym.

Istnieją dwa odmian wstępnej kompilacji dla wdrożenia, które firma Microsoft Eksplorowanie w tym samouczku: wstępnej kompilacji przy użyciu interfejsu użytkownika można aktualizować i wstępnej kompilacji z interfejsem użytkownika nie można aktualizować. Personalizacja przy użyciu interfejsu użytkownika można aktualizować pozostawia deklaratywne znaczników w `.aspx`, `.ascx`, i `.master` plików, umożliwiając dewelopera wyświetlić i, w razie potrzeby można zmodyfikować deklaratywne znaczników na serwerze produkcyjnym. Generuje wstępnej kompilacji z interfejsem użytkownika nie można aktualizować `.aspx` stron, które są nieważne żadnej zawartości i usuwa `.ascx` i `.master` plików, a tym samym ukrywanie deklaratywne znaczników i uniemożliwiają zmianę od dewelopera środowiska produkcyjnego.

### <a name="precompiling-for-deployment-with-an-updatable-user-interface"></a>Prekompilowanie dla wdrożenia przy użyciu interfejsu użytkownika można aktualizować

Najlepszy sposób, aby zrozumieć wstępnej kompilacji dla wdrożenia jest na przykład w akcji. Załóżmy prekompilowanie WSP przeglądami książki dla wdrożenia przy użyciu interfejsu użytkownika można aktualizować. Narzędzia kompilacji platformy ASP.NET można wywołać z menu kompilacji programu Visual Studio lub z wiersza polecenia. W tej sekcji sprawdza, czy narzędzie z poziomu programu Visual Studio; w sekcji "wstępnej kompilacji z wiersza polecenia" analizuje uruchomione narzędzie kompilatora z wiersza polecenia.

Otwórz WSP przeglądu książki w programie Visual Studio, przejdź do menu Kompiluj i wybierz opcję menu publikowanie witryny sieci Web. Otworzy się okno dialogowe publikowanie witryny sieci Web (zobacz **rysunek 1**), w którym można określić lokalizacji docelowej, czy interfejs użytkownika wstępnie skompilowanej witryny jest aktualizowalny i inne opcje narzędzia kompilatora. Lokalizacja docelowa może być zdalnym serwerze sieci web lub serwera FTP, ale teraz wybrać folder na dysku twardym komputera. Ponieważ chcemy prekompilowanie lokacji przy użyciu interfejsu użytkownika można aktualizować, pozostaw pole wyboru "Zezwalaj tej witryny wstępnie skompilowanej aktualizacja była możliwa" zaznaczone, a następnie kliknij przycisk OK.

[![](precompiling-your-website-vb/_static/image2.png)](precompiling-your-website-vb/_static/image1.png)

**Rysunek 1**: ASP.NET Compilation Tool będzie wstępnej kompilacji witryny sieci Web do określona lokalizacja docelowa  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](precompiling-your-website-vb/_static/image3.png))

> [!NOTE]
> Opcja publikowania witryny sieci Web, w menu kompilacji nie jest dostępne w programie Visual Web Developer. Jeśli używasz programu Visual Web Developer, musisz użyć wiersza polecenia wersji narzędzia kompilacji platformy ASP.NET, ta jest omówiona w sekcji "wstępnej kompilacji z wiersza polecenia".


Po wstępnej kompilacji witryny sieci Web, przejdź do lokalizacji docelowej, wprowadzone w oknie dialogowym Publikowanie witryny sieci Web. Poświęć chwilę, aby porównać zawartość tego katalogu z zawartością witryny sieci Web. **Rysunek 2** pokazuje przeglądami książki folderu witryny sieci Web. Należy pamiętać, że zawiera on zarówno `.aspx` i `.aspx.cs` plików. Ponadto należy pamiętać, że `Bin` katalogu zawiera tylko jeden plik `Elmah.dll`, dodanego w [poprzedniego samouczka](logging-error-details-with-elmah-vb.md)

[![](precompiling-your-website-vb/_static/image5.png)](precompiling-your-website-vb/_static/image4.png)

**Rysunek 2**: katalog projektu zawiera `.aspx` i `.aspx.cs` pliki; `Bin` Folder zawiera po prostu`Elmah.dll`  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](precompiling-your-website-vb/_static/image6.png))

**Rysunek 3** zawiera folder lokalizacji docelowych których zawartość zostały utworzone przez narzędzie kompilacji programu ASP.NET. Ten folder nie zawiera żadnych plików z kodem. Ponadto ten folder `Bin` katalogu obejmuje kilka zestawów i dwa `.compiled` pliki oprócz `Elmah.dll` zestawu.

[![](precompiling-your-website-vb/_static/image8.png)](precompiling-your-website-vb/_static/image7.png)

**Rysunek 3**: Folder lokalizacji docelowych zawiera pliki do wdrożenia  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](precompiling-your-website-vb/_static/image9.png))

W odróżnieniu od kompilację typu explicit w WAPs wstępnej kompilacji dla procesu wdrożenia nie powoduje utworzenia jednego zestawu dla całej lokacji. Zamiast tego należy go partii ze sobą kilka stron do każdego zestawu. Kompiluje również `Global.asax` plik (jeśli istnieje) do własnego zestawu, a także wszystkie klasy w `App_Code` folderu. Plików zawierających deklaratywne znaczników dla platformy ASP.NET sieci web stron, kontrolek użytkownika i stron wzorcowych (`.aspx`, `.ascx`, i `.master` pliki odpowiednio) są kopiowane jako-polega na lokalizację katalogu docelowego. Podobnie `Web.config` skopiować pliku nad, wraz z plików statycznych, takich jak obrazy, klas CSS i plików PDF. Opis bardziej formalnych jak narzędzia kompilacji obsługuje różne typy plików, zapoznaj się [plików obsługi podczas ASP.NET wstępnej kompilacji](https://msdn.microsoft.com/en-us/library/e22s60h9.aspx).

> [!NOTE]
> Możesz wydać narzędzie kompilacji, aby utworzyć jednego zestawu, na stronie platformy ASP.NET, kontrolki użytkownika lub strony wzorcowej, zaznaczając pole wyboru "Używane stałej nazewnictwa i zestawów jednej strony" w oknie dialogowym Publikowanie witryny sieci Web. Po każdej stronie ASP.NET skompilowany do własnych zestawach umożliwia bardziej precyzyjną kontrolę nad wdrożenia. Na przykład, jeśli zaktualizowane z pojedynczą stroną sieci web ASP.NET i potrzebne do wdrożenia tej zmiany, należy tylko wdrażania tej strony `.aspx` plików i skojarzony zestaw do środowiska produkcyjnego. Zapoznaj się [jak: generowanie stałe nazwy za pomocą narzędzia kompilacji ASP.NET](https://msdn.microsoft.com/en-us/library/ms228040.aspx) Aby uzyskać więcej informacji.


Katalog docelowy lokalizacji zawiera także plik, który nie jest częścią projektu sieci web wstępnie skompilowanym, czyli `PrecompiledApp.config`. Ten plik poinformuje środowiska uruchomieniowego ASP.NET, że aplikacja została wstępnie skompilowana i czy została ona wstępnie skompilowana z nadaje się do aktualizacji lub południe aktualizowalnych interfejsu użytkownika.

Na koniec Poświęć chwilę, aby otworzyć jedną z `.aspx` plików w lokalizacji docelowej przy użyciu programu Visual Studio lub w edytorze tekstów wyboru. Po wstępnej kompilacji dla wdrożenia przy użyciu interfejsu użytkownika można aktualizować, stron ASP.NET w katalogu docelowym lokalizacji zawiera dokładnie tego samego znaczników jako odpowiadające im pliki w witrynie internetowej.

### <a name="precompiling-for-deployment-with-a-non-updatable-user-interface"></a>Prekompilowanie wdrożenia za pomocą interfejsu użytkownika nie można aktualizować

Narzędzie kompilatora programu ASP.NET można również prekompilowanie lokacji dla wdrożenia przy użyciu — można aktualizować interfejsu użytkownika. Prekompilowanie lokacji przy użyciu interfejsu użytkownika nie można aktualizować działa podobne jak w przypadku wstępnej kompilacji z aktualizowalnych interfejsu użytkownika Najważniejsza różnica, że stron ASP.NET, kontrolek użytkownika i stron wzorcowych w katalogu docelowym są usuwane z ich znaczników. Wstępnej kompilacji witryny sieci Web do wdrożenia z — można aktualizować interfejsu użytkownika, wybierz opcję publikowania witryny sieci Web, w menu kompilacji, ale Usuń zaznaczenie opcji "Zezwalaj tej witryny wstępnie skompilowanej aktualizacja była możliwa" (zobacz **4 rysunek**).

[![](precompiling-your-website-vb/_static/image11.png)](precompiling-your-website-vb/_static/image10.png)

**Rysunek 4**: Usuń zaznaczenie pola wyboru "Zezwalaj tej witryny wstępnie skompilowanej aktualizacja była możliwa" opcja do wstępnej kompilacji z można aktualizować bez interfejsu użytkownika  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](precompiling-your-website-vb/_static/image12.png))

**Rysunek 5** zawiera folder lokalizacji docelowych po wstępnej kompilacji z interfejsem użytkownika nie można aktualizować.

[![](precompiling-your-website-vb/_static/image14.png)](precompiling-your-website-vb/_static/image13.png)

**Rysunek 5**: Lokalizacja folderu docelowego dla wdrożenia przy użyciu — można aktualizować interfejsu użytkownika  
 ([Kliknij, aby wyświetlić obraz w pełnym rozmiarze](precompiling-your-website-vb/_static/image15.png))

Porównaj **rysunek 3** do **rysunek 5**. Gdy dwa foldery może wyglądać identyczne, należy pamiętać, że nie można aktualizować folderu interfejsu użytkownika nie ma strony wzorcowej `Site.master`. I podczas **rysunek 5** zawiera różne strony ASP.NET, jeśli przeglądasz zawartość tych plików, zobaczysz, że zostały one pozbawione ich deklaratywne znaczników i zastąpione tekst zastępczy: "jest to plik znacznika generowany przez Narzędzie wstępnej kompilacji i nie należy go usuwać! "

[![](precompiling-your-website-vb/_static/image17.png)](precompiling-your-website-vb/_static/image16.png)

**Rysunek 5**: deklaratywne znaczników został usunięty z stron ASP.NET

`Bin` Folderów w **3 rysunki** i **5** różnią się znacznie więcej. Oprócz zestawów `Bin` folderu w **rysunek 5** obejmuje `.compiled` plik dla każdej strony ASP.NET, kontrolki użytkownika i strony wzorcowej.

Prekompilowanie lokacji przy użyciu — można aktualizować interfejsu użytkownika jest przydatne w sytuacjach, gdy nie ma zawartości strony ASP.NET ma być zmodyfikowana przez osoby lub firmy, który instaluje ani nie zarządza nimi witryny sieci Web w środowisku produkcyjnym. W przypadku tworzenia aplikacji sieci web ASP.NET, która je sprzedać użytkowników do zainstalowania na serwerach sieci web, można zapewnić ich bezpośrednio edytując nie należy modyfikować wyglądu i działania witryny `.aspx` stron je wysłać. Przez wstępnej kompilacji witryny sieci Web przy użyciu — można aktualizować interfejsu użytkownika, wysyłki symbol zastępczy `.aspx` stron w ramach instalacji, zapobiegając w ten sposób klienci z badania lub modyfikowania ich zawartości.

### <a name="precompiling-from-the-command-line"></a>Wstępnej kompilacji z wiersza polecenia

W tle, okno dialogowe publikowanie witryny sieci Web programu Visual Studio wywołuje narzędzia kompilacji platformy ASP.NET (`aspnet_compiler.exe`) wstępnej kompilacji witryny sieci Web. Alternatywnie można wywołać to narzędzie z wiersza polecenia. W rzeczywistości Jeśli używasz programu Visual Web Developer następnie należy uruchomić narzędzie kompilatora wiersza polecenia, zgodnie z menu kompilacji programu Visual Web Developer's nie ma opcji publikowania witryny sieci Web.

Aby użyć narzędzia kompilatora wiersza polecenia, uruchom przez usunięcie do wiersza polecenia i przechodząc do katalogu framework `%WINDIR%\Microsoft.NET\Framework\v2.0.50727`. Następnie wprowadź następującą instrukcję, w wierszu polecenia:

`aspnet_compiler -p "physical_path_to_app" -v / -f -u "target_location_folder"`

Powyższe polecenie uruchamia narzędzie kompilatora programu ASP.NET (`aspnet_compiler.exe`) i za pośrednictwem `-p` przełącznika, powoduje, że jej wstępnej kompilacji witryny sieci Web, począwszy od *fizycznych\_ścieżki\_do\_aplikacji*; Ta wartość będzie wyglądać mniej więcej tak `C:\MySites\BookReviews`i powinien być rozdzielane znakami cudzysłowu.

`-v` Przełącznik określa katalog wirtualny witryny. Jeśli witryna jest zarejestrowana jako domyślnej witryny sieci Web w metabazie usług IIS, można pominąć `-p` przełącznika i określa wirtualny katalog aplikacji. Jeśli używasz `-p` przełącznika postępowania wartość `-v` przełącznika wskazuje katalog główny witryny sieci Web i jest używany do rozpoznawania odwołań do katalogu głównego aplikacji. Na przykład jeśli określono wartość `-v /MySite` odwołuje się w aplikacji, aby `~/path/file` zostanie rozpoznana jako `~/MySite/path/file`. Ponieważ witryna przeglądami książki znajduje się w katalogu głównym w mojej firmy hostingu w sieci web I użyto przełącznika `-v /`.

`-f` Przełącznik, jeśli jest obecny, powoduje, że narzędzie kompilacji, aby zastąpić *docelowej\_lokalizacji\_folderu* katalogu, jeśli już istnieje. W przypadku pominięcia `-f` przełącznika i folder lokalizacji docelowej już istnieje, narzędzie kompilacji zostanie zamknięta z powodu błędu: "Błąd ASPRUNTIME: katalog docelowy nie jest pusty. Usuń go ręcznie lub wybierz inny element docelowy."

`-u` Przełącznika, jeśli jest obecny, informuje narzędzie do tworzenia interfejsu użytkownika można aktualizować. Pomiń ten przełącznik wstępnej kompilacji witryny za pomocą interfejsu użytkownika nie można aktualizować.

Ponadto *docelowej\_lokalizacji\_folderu* jest ścieżkę fizyczną do katalogu docelowego lokalizacji; wartość ta będzie wyglądać mniej więcej tak `C:\MySites\Output\BookReviews`i powinien być rozdzielane znakami cudzysłowu.

## <a name="deploying-the-precompiled-website"></a>Wdrażanie wstępnie skompilowanym witryny sieci Web

W tym momencie zaobserwowano wstępnej kompilacji witryny sieci Web przy użyciu obu opcji interfejsu użytkownika można aktualizować i nie można aktualizować przy użyciu narzędzia kompilacji platformy ASP.NET. Jednak nasze przykłady dotychczasowych ma wstępnie skompilowana witryny sieci Web do folderu lokalnego, a nie do środowiska produkcyjnego. Dobre wieści jest wdrażanie wstępnie skompilowanym witryny sieci Web jest ona błyskawicznie i może odbywać się za pomocą programu Visual Studio lub innych plików kopii mechanizmu, takich jak z klienta autonomicznego FTP.

Okno dialogowe publikowanie witryny sieci Web (pokazywany jako pierwszy w **rysunek 1**) ma opcja lokalizacji docelowej, która wskazuje, gdzie pliki prekompilowanego witryny sieci Web są kopiowane do. Ta lokalizacja może być zdalnym serwerze sieci web lub serwera FTP. Wprowadzanie serwera zdalnego, w tym polu tekstowym precompiles i wdraża witryny sieci Web do określonego serwera w jednym kroku. Alternatywnie możesz wstępnej kompilacji witryny sieci Web do folderu lokalnego i następnie ręcznie skopiuj zawartość tego folderu do środowiska produkcyjnego, za pomocą protokołu FTP lub pewne inne podejścia.

O wstępnie skompilowanym witryny sieci Web automatycznie wdrożone przez okno dialogowe publikowanie witryny sieci Web programu Visual Studio jest przydatne w przypadku prostych witryn w przypadku, gdy nie ma żadnych różnic konfiguracji środowiska projektowania i produkcji. Jednakże, jakie zostały zanotowane w [ *typowych konfiguracji różnice między rozwoju i produkcji* samouczek](common-configuration-differences-between-development-and-production-vb.md) nie jest często różnice te mogą znajdować się. Na przykład aplikacja sieci web przeglądami książki używa innej bazy danych w środowisku produkcyjnym niż w środowisku programistycznym. Gdy program Visual Studio publikuje witryny sieci Web na serwerze zdalnym ślepo kopiuje informacje pliku konfiguracji w środowisku programistycznym.

Dla witryn o konfiguracji różnice między środowisk projektowania i produkcji może być najlepszym rozwiązaniem prekompilowanie lokacji do katalogu lokalnego, skopiuj pliki konfiguracyjne produkcji, a następnie skopiuj zawartość wstępnie skompilowanych danych wyjściowych do produkcji.

Odświeżacz kopiowania plików ze środowiska projektowego do środowiska produkcyjnego można znaleźć w temacie [ *Wdrażanie witryny sieci Web przy użyciu klienta FTP* ](deploying-your-site-using-an-ftp-client-vb.md) i [  *Wdrażanie Your witryny sieci Web przy użyciu programu Visual Studio* ](determining-what-files-need-to-be-deployed-vb.md) samouczki.

## <a name="summary"></a>Podsumowanie

Program ASP.NET obsługuje dwa tryby kompilacji: jawne i automatyczne. Zgodnie z opisem w poprzedniej samouczki, projekty aplikacji sieci Web (WAPs) Użyj jawnego kompilacji projektów witryny sieci Web (WSPs) używane automatyczne kompilowanie domyślnie. Jednak jest możliwe jawnie opracowanie WSP przed ich wdrożeniem za pomocą narzędzia kompilacji platformy ASP.NET.

Ten samouczek koncentruje się na wstępnej kompilacji narzędzia kompilacji do obsługi wdrożenia. Po wstępnej kompilacji dla wdrożenia, narzędzia kompilacji tworzy folder lokalizacji docelowych, kompiluje kod źródłowy aplikacji sieci web określonego i kopiuje je skompilowane zestawy i pliki zawartości w lokalizacji folderu docelowego. Narzędzia kompilacji można skonfigurować do tworzenia interfejsu użytkownika można aktualizować lub nie można aktualizować. Po wstępnej kompilacji z opcją interfejsu użytkownika nie można aktualizować, deklaratywne znaczników do plików zawartości zostaną usunięte. Mówiąc wstępnej kompilacji umożliwia wdrażanie aplikacji opartych na projekt witryny sieci Web bez łącznie z plikami kodu źródłowego i deklaratywne znaczników usunięte, w razie potrzeby.

Programowanie przyjemność!

### <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Wstępnej kompilacji witryny sieci Web ASP.NET](https://msdn.microsoft.com/en-us/library/ms228015.aspx)
- [Plik CodeBehind i kompilacji w programie ASP.NET 2.0](https://msdn.microsoft.com/en-us/magazine/cc163675.aspx)
- [Wstępnej kompilacji w programie ASP.NET](http://www.odetocode.com/Articles/417.aspx)
- [Opcje wstępnie skompilowanej witryny w programie ASP.NET](http://www.dotnetperls.com/precompiled)

>[!div class="step-by-step"]
[Poprzednie](logging-error-details-with-elmah-vb.md)
[dalej](users-and-roles-on-the-production-website-vb.md)
