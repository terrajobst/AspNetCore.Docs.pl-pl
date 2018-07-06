---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: Tworzenie warstwy dostępu do danych (C#) | Dokumentacja firmy Microsoft
author: rick-anderson
description: W tym samouczku utworzymy zacząć od samego początku wymuszał i tworzenie danych warstwy dostępu (DAL), przy użyciu typizowanych zestawów danych, aby uzyskać dostęp do informacji w bazie danych.
ms.author: aspnetcontent
ms.date: 04/05/2010
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 556771994fd80773ec1e86bcbab0f60f2bb821e4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838757"
---
<a name="creating-a-data-access-layer-c"></a>Tworzenie warstwy dostępu do danych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe) lub [Pobierz plik PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> W tym samouczku utworzymy zacząć od samego początku wymuszał i tworzenie danych warstwy dostępu (DAL), przy użyciu typizowanych zestawów danych, aby uzyskać dostęp do informacji w bazie danych.


## <a name="introduction"></a>Wprowadzenie

Jako projektantom sieci web nasze życie koncentrują się wokół pracy z danymi. Możemy utworzyć bazy danych do przechowywania danych, kod, aby pobrać i zmodyfikować i stron sieci web do zbierania i podsumowaniu. To jest pierwszy samouczek w długich serii, która będzie Poznaj techniki implementacji tych typowych wzorców w programie ASP.NET 2.0. Zaczniemy od utworzenia [architektury oprogramowania](http://en.wikipedia.org/wiki/Software_architecture) składa się z danych programu Access warstwy (DAL) przy użyciu wpisanych zestawów danych, warstwy logiki biznesowej (LOGIKI) który wymusza stosowanie niestandardowych reguł biznesowych, a warstwa prezentacji składa się z ASP.NET strony, która Udostępniaj wspólne układu strony. Po sekcji tego podwaliny pod Projekt wdrożenia wewnętrznej bazy danych, zmienimy do raportowania, przedstawiający sposób wyświetlania, podsumować, zbieranie i sprawdzanie poprawności danych z aplikacji sieci web. Te samouczki są dostosowane do być zwięzłe i zapewnić instrukcje krok po kroku z dużą ilością zrzuty ekranu wizualnie przeprowadzi Cię przez proces. Każdy samouczek jest dostępny w C# i Visual Basic wersji i obejmuje pobieranie kompletny kod używany. (Tego pierwszego samouczka dotyczącego jest bardzo długi, ale pozostałe są prezentowane w znacznie więcej fragmentów o wysokim strawności).

Te samouczki będziemy używać programu Microsoft SQL Server 2005 Express Edition wersję bazy danych Northwind, umieszczone w **aplikacji\_danych** katalogu. Oprócz pliku bazy danych **aplikacji\_danych** folder zawiera także skrypty SQL do tworzenia baz danych, w przypadku, gdy chcesz użyć wersji innej bazy danych. Skrypty te mogą być również [pobrać bezpośrednio z witryny Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), jeśli chcesz użyć. Jeśli używasz innej wersji programu SQL Server, bazy danych Northwind, należy zaktualizować **NORTHWNDConnectionString** ustawienia aplikacji w witrynie **Web.config** pliku. Aplikacja sieci web został skompilowany przy użyciu programu Visual Studio 2005 Professional Edition jako projekt witryny sieci Web opartych na systemie plików. Jednak wszystkie samouczki będzie działać równie dobrze z bezpłatnej wersji usługi Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
W tym samouczku utworzymy zacząć od samego początku wymuszał i tworzenie danych warstwy dostępu (DAL), Tworzenie warstwy logiki biznesowej (LOGIKI) w drugim samouczku, a nad układu strony i nawigacja w trzecim. Samouczki, po trzecie będzie bazują na podstawę głosów pierwsze trzy. Mamy wiele pokrycia w tym pierwszym samouczku, dlatego środowisko Visual Studio i zaczynajmy!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Krok 1: Tworzenie projektu sieci Web i połączenie z bazą danych

Zanim będziemy mogli utworzyć nasze warstwy dostępu do danych (DAL), najpierw należy utworzyć witrynę sieci web i skonfigurować w naszej bazie danych. Rozpocznij od utworzenia nowego pliku w oparciu o system ASP.NET witryny sieci web. Aby to zrobić, przejdź do menu Plik, a następnie wybierz nową witrynę sieci Web w celu wyświetlania okna dialogowego nową witrynę sieci Web. Wybierz szablon witryny sieci Web platformy ASP.NET, zmień wartość na liście rozwijanej lokalizacji w systemie plików, wybierz folder do umieszczenia w witrynie sieci web i ustaw języka C#.


[![Tworzenie nowego pliku w oparciu o System witryny sieci Web](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Rysunek 1**: tworzenie witryny sieci Web New File System-Based ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image3.png))


Spowoduje to utworzenie nowej witryny sieci web za pomocą **Default.aspx** strony ASP.NET i **aplikacji\_danych** folderu.

Witryny sieci web utworzone następnym krokiem jest dodać odwołanie do bazy danych w Eksploratorze serwera Visual Studio. Dodając bazę danych do Eksploratora serwera można dodać tabel, procedur składowanych, widoków i itd. wszystko z poziomu programu Visual Studio. Można również wyświetlić dane w tabeli lub ręcznie lub graficznie tworzyć własne zapytania za pomocą konstruktora zapytań. Ponadto gdy będziemy tworzyć wpisanych zestawów danych dla warstwy DAL musimy punktu Visual Studio do bazy danych, z którego powinien być konstruowany wpisanych zestawów danych. Gdy firma Microsoft może zapewnić informacje o połączeniu w danym momencie, Visual Studio spowoduje automatyczne wypełnienie listy rozwijanej, baz danych już jest zarejestrowany w Eksploratorze serwera.

Kroki umożliwiające dodawanie bazy danych Northwind do Eksploratora serwera zależą od tego, czy chcesz użyć bazy danych programu SQL Server 2005 Express Edition w **aplikacji\_danych** folderze lub w przypadku programu Microsoft SQL Server 2000 lub 2005 Konfigurowanie serwera bazy danych, którą chcesz użyć.

## <a name="using-a-database-in-theappdatafolder"></a>Korzystanie z bazy danych w theApp\_DataFolder

Jeśli nie masz programu SQL Server 2000 lub serwer bazy danych 2005, aby nawiązać połączenie, lub po prostu chcesz uniknąć konieczności dodawania bazy danych do serwera bazy danych, można użyć wersji programu SQL Server 2005 Express Edition bazy danych Northwind, która znajduje się w pobranych websit e's **aplikacji\_danych** folder (**NORTHWND. MDF**).

Bazy danych są umieszczane w **aplikacji\_danych** folderu jest automatycznie dodawane do Eksploratora serwera. Przy założeniu, że program SQL Server 2005 Express Edition, na maszynie zainstalowano powinny być widoczne węzła o nazwie NORTHWND. MDF w Eksploratorze serwera, które można rozwijać i zapoznaj się z jego tabele, widoki, procedury składowanej i tak dalej (patrz rysunek 2).

**Aplikacji\_danych** folderu może też przechowywać Microsoft Access **.mdb** pliki, które podobnie jak ich odpowiedniki programu SQL Server, są automatycznie dodawane do Eksploratora serwera. Jeśli nie chcesz używać żadnych opcji programu SQL Server, zawsze możesz [Pobierz program Microsoft Access wersję pliku bazy danych Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) i upuść na **aplikacji\_danych** katalogu. Zwróć uwagę, jednak baz danych programu Access spoza jako wyposażonym w programie SQL Server i nie są przeznaczone do użytku w scenariuszach witryny sieci web. Ponadto kilka samouczków ponad 35 będzie korzystać z niektórych funkcji poziomu bazy danych, które nie są obsługiwane przez funkcję dostępu.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Łączenie z bazą danych na serwerze bazy danych programu Microsoft SQL Server 2000 lub 2005

Alternatywnie możesz nawiązać bazy danych Northwind zainstalowane na serwerze bazy danych. Jeśli serwer bazy danych nie ma jeszcze zainstalowane bazy danych Northwind, użytkownik musi go najpierw dodać do serwera bazy danych, uruchamiając skrypt instalacyjny zawarte w tym samouczku pobierania lub przez [pobieranie wersji programu SQL Server 2000 bazy danych Northwind i skrypt instalacji](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) bezpośrednio z witryny sieci web firmy Microsoft.

Po utworzeniu bazy danych, zainstalowane, przejdź do Eksploratora serwera w programie Visual Studio, kliknij prawym przyciskiem myszy węzła połączenia danych i wybierz polecenie Dodaj połączenie. Jeśli nie widzisz Eksploratora serwera, przejdź do widoku / Server Explorer lub trafień Ctrl + Alt + S. Okno dialogowe Dodawanie połączenia, w którym można określić serwer, aby nawiązać połączenie, spowoduje to wyświetlenie informacji o uwierzytelnianiu i nazwę bazy danych. Po pomyślnym skonfigurowanych informacji dotyczących połączenia bazy danych i kliknięciu przycisku OK, baza danych zostanie dodany jako węzeł poniżej tego węzła połączenia danych. Można rozwinąć węzeł bazy danych, aby eksplorować tabele, widoki, procedury składowane i tak dalej.


![Dodawanie połączenia do bazy danych Northwind serwer bazy danych](creating-a-data-access-layer-cs/_static/image4.png)

**Rysunek 2**: Dodawanie połączenia do bazy danych Northwind serwer bazy danych


## <a name="step-2-creating-the-data-access-layer"></a>Krok 2. Tworzenie warstwy dostępu do danych

Podczas pracy z jedną z opcji danych jest osadzone logikę specyficzną dla danych bezpośrednio do warstwy prezentacji (aplikacja sieci web składają stron ASP.NET Warstwa prezentacji). Może to potrwać postaci pisania kodu ADO.NET w części kodu strony ASP.NET lub przy użyciu kontrolki SqlDataSource z fragment kodu znaczników. W obu przypadkach to podejście ściśle couples logiką dostępu do danych z warstwą prezentacji. Zalecanym podejściem jest jednak oddzielić logiką dostępu do danych z warstwy prezentacji. Ta warstwa oddzielne nazywa się warstwy dostępu do danych, warstwy DAL w skrócie i jest zwykle implementowany jako oddzielny projekt biblioteki klas. Zalety tej architektury warstwowej są dobrze udokumentowane (zobacz sekcję "Dalsze odczytów" na końcu niniejszego samouczka, aby informacji na temat korzyści te) i jest rozwiązaniem podejmujemy w tej serii.

Cały kod, który zależy od bazowego źródła danych, takich jak tworzenie połączenia z bazą danych, wydawanie **wybierz**, **Wstaw**, **aktualizacji**, i  **Usuń** polecenia i tak dalej powinien znajdować się w warstwy DAL. Warstwa prezentacji nie powinna zawierać żadnych odwołań do takiego kodu dostępu do danych, ale zamiast tego należy wykonywać wywołania do warstwy DAL wszystkie dane żądań. Warstwy dostępu do danych zwykle zawierają metod dostępu do danych bazowych bazy danych. Bazy danych Northwind, na przykład ma **produktów** i **kategorie** tabel, służące do rejestrowania produktów, sprzedaż i kategorie, do których należą. W naszym DAL mamy metod, takich jak:

- **GetCategories(),** co spowoduje zwrócenie informacji na temat wszystkich kategorii
- **GetProducts()**, co spowoduje zwrócenie informacji na temat wszystkich produktów
- **GetProductsByCategoryID (*categoryID*)**, co spowoduje zwrócenie wszystkich produktów, które należą do określonej kategorii
- **GetProductByProductID (*productID*)**, co spowoduje zwrócenie informacji na temat określonego produktu

Te metody, gdy wywoływany, będzie połączenia z bazą danych, wystawiał odpowiednie zapytanie, a następnie zwracają wyniki. Ważne jest jak możemy zwrócić następujące wyniki. Metody te po prostu może zwrócić zestawu danych lub DataReader wprowadzona przez zapytanie bazy danych, ale najlepiej te wyniki powinny być zwracany za pomocą *silnie typizowanych obiektów*. Obiekt silnie typizowane jest jednym którego schemat sztywno jest zdefiniowany w czasie kompilacji, i na odwrót obiekt typowaniem luźnym jest jedną którego schemat nie jest znany aż do czasu.

Na przykład elementu DataReader i zestaw danych (domyślnie) to typowaniem luźnym obiekty, ponieważ jego schemat jest definiowany przez kolumny zwracane przez zapytanie bazy danych, używanych do wypełniania je. Dostęp do konkretnej kolumny z typowaniem luźnym DataTable, należy użyć składni, takich jak:  <strong><em>DataTable</em>. Wiersze [<em>indeksu</em>] ["<em>columnName</em>"]</strong>. Na obiekt DataTable nie będzie wpisując w tym przykładzie jest ujawnione przez fakt, że potrzebujemy dostęp do nazwy kolumny przy użyciu ciągu lub indeksem. Silnie typizowane DataTable, z drugiej strony, będzie miał każdego z jego kolumn implementowane jako właściwości, co w kodzie, który wygląda następująco:  <strong><em>DataTable</em>. Wiersze [<em>indeksu</em>]. *NazwaKolumny</strong>*.

Aby przywrócić silnie typizowanych obiektów, deweloperzy mogą tworzenie własnych obiektów niestandardowych biznesowych albo użyj wpisanych zestawów danych. Obiekt biznesowy jest implementowany przez dewelopera, ponieważ reprezentuje klasę, której właściwości zwykle odzwierciedla kolumny tabeli podstawowej bazy danych obiektu biznesowego. Wpisany zestaw danych jest klasą wygenerowane automatycznie przez program Visual Studio na podstawie schematu bazy danych i której członkami są silnie typizowaną zgodnie z tym schemacie. Wpisany zestaw danych sam składa się z klas, które rozszerzają klas ADO.NET DataSet, DataTable i DataRow. Oprócz silnie typizowane DataTable wpisanych zestawów danych teraz także TableAdapters, w której przedstawiono klasy z metodami wypełnianie zestawu danych DataTables zmiany i propagowanie zmian w ramach DataTable w bazie danych.

> [!NOTE]
> Aby uzyskać więcej informacji na temat zalet i wad przy użyciu wpisanych zestawów danych i obiektów niestandardowych biznesowych, zobacz [projektowanie składników warstwy danych i przekazywanie danych za pośrednictwem warstw](https://msdn.microsoft.com/library/ms978496.aspx).


Użyjemy silnie typizowane zestawy danych dla tych samouczków architektury. Rysunek 3 ilustruje przepływ pracy między różne warstwy aplikacji, która używa wpisanych zestawów danych.


[![Cały kod dostępu do danych jest były odpowiedzialne warstwy DAL](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Rysunek 3**: wszystkie kod dostępu do danych jest były odpowiedzialne z warstwą dal ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Tworzenie zestawu danych i tabeli karty

Aby rozpocząć tworzenie naszej DAL, Zaczniemy od Dodawanie wpisany zestaw danych do naszego projektu. W tym celu kliknij prawym przyciskiem myszy węzeł projektu w Eksploratorze rozwiązań i wybierz pozycję Dodaj nowy element. Wybierz opcję zestaw danych z listy szablonów, a następnie nadaj mu nazwę **Northwind.xsd**.


[![Wybierz dodać nowy zestaw danych do projektu](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Rysunek 4**: Wybierz dodać nowy zestaw danych do projektu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image10.png))


Po kliknięciu przycisku Dodaj, po wyświetleniu monitu, aby dodać zestaw danych do **aplikacji\_kodu** folderu, kliknij przycisk Tak. Projektant wpisany zestaw danych zostanie wyświetlony, a następnie uruchomi Kreator konfiguracji TableAdapter, umożliwiające dodanie Twojego pierwszego TableAdapter DataSet wpisane.

Wpisany zestaw danych jest zbiorem silnie typizowanych danych. składa się z silnie typizowane wystąpień elementu DataTable, z których każdy z kolei składa się z silnie typizowane wystąpień klasy DataRow. Utworzymy tabelę DataTable silnie typizowane dla każdej z tabel bazy danych, potrzebne nam pracować w tej serii samouczków. Zacznijmy od utworzenia elementu DataTable do **produktów** tabeli.

Należy pamiętać o tym, silnie typizowane DataTable nie dołączaj żadnych informacji o tym, jak uzyskać dostęp do danych z ich tabeli źródłowej bazy danych. Aby pobrać dane w celu wypełnienia tabeli DataTable, należy użyć klasy TableAdapter, która działa jako nasze warstwy dostępu do danych. Dla naszych **produktów** DataTable, TableAdapter będzie zawierać metod **GetProducts()**, **GetProductByCategoryID (*categoryID*)** i tak dalej, firma Microsoft będzie wywołania z warstwy prezentacji. Rola tabeli DataTable jest ma pełnić rolę silnie typizowanych obiektów, używany do przekazywania danych między warstwami.

Kreator konfiguracji TableAdapter rozpoczyna się od monit wybierz bazę danych, które chcesz pracować. Na liście rozwijanej przedstawiono tych baz danych w Eksploratorze serwera. Jeśli bazy danych Northwind nie zostały dodane do Eksploratora serwera, możesz kliknąć przycisk nowe połączenie, w tej chwili, aby to zrobić.


[![Z listy rozwijanej wybierz opcję bazy danych Northwind](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Rysunek 5**: z listy rozwijanej wybierz opcję bazy danych Northwind ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image13.png))


Po wybraniu bazy danych, a następnie klikając przycisk Dalej, użytkownik zostanie zapytany, czy chcesz zapisać parametry połączenia w **Web.config** pliku. Zapisując parametry połączenia można będzie uniknąć go twardych kodowane w klasach TableAdapter, która upraszcza czynności, jeśli informacje o parametrach połączenia zmieni się w przyszłości. Jeśli wybierzesz opcję Zapisz parametry połączenia w pliku konfiguracji jest umieszczany w **&lt;connectionStrings&gt;** sekcję, co może być [opcjonalnie zaszyfrowanych](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) na lepsze zabezpieczeń lub zmodyfikowany później przy użyciu nowej strony właściwości w wersji 2.0 programu ASP.NET w narzędziu administracyjnym usług IIS graficznego interfejsu użytkownika, co jest bardziej idealnym rozwiązaniem dla administratorów.


[![Zapisz parametry połączenia do pliku Web.config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Rysunek 6**: Zapisz parametry połączenia do **Web.config** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image16.png))


Następnie należy zdefiniować schemat dla pierwszego elementu DataTable silnie typizowane i Udostępnij pierwszą metodę naszych TableAdapter do użycia podczas wypełniania zestawu danych silnie typizowane. Następujące dwa kroki są wykonywane jednocześnie, tworząc kwerendę, która zwraca kolumny z tabeli, że chcemy znajduje odzwierciedlenie w naszym DataTable. Po zakończeniu pracy z kreatorem przedstawimy Nazwa metody do tego zapytania. Gdy który zostało zrobione, ta metoda może być wywoływany z naszych warstwy prezentacji. Metoda będzie wykonaj zapytanie zdefiniowane i wypełnij DataTable silnie typizowane.

Aby rozpocząć pracę, definiując kwerendy SQL musi najpierw wskazaliśmy jak chcemy TableAdapter do wysyłania kwerendy. Możemy użyć instrukcji SQL zapytań ad-hoc, Utwórz nową procedurę składowaną lub użyj istniejącą procedurę składowaną. Dla tych samouczkach użyjemy instrukcji SQL zapytań ad-hoc. Zapoznaj się [NieTak Brian](http://briannoyes.net/)użytkownika artykuł [Tworzenie warstwy dostępu do danych, za pomocą Projektanta obiektów DataSet 2005 Visual Studio](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) na przykład za pomocą procedur składowanych.


[![Wykonywanie zapytań o dane za pomocą instrukcji SQL zapytań Ad-Hoc](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Rysunek 7**: wykonywanie zapytań o dane za pomocą instrukcji SQL zapytań Ad-Hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image19.png))


W tym momencie możemy można wpisać w kwerendzie SQL ręcznie. Podczas tworzenia pierwszej metody w metodzie TableAdapter ma zazwyczaj kwerenda zwraca te kolumny, które muszą być wyrażone w odpowiedni DataTable. Możemy to zrobić, tworząc zapytania zwracającego wszystkie kolumny i wszystkie wiersze z **produktów** tabeli:


[![Wprowadź zapytanie SQL do kontrolki Textbox](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Rysunek 8**: SQL zapytań w polu tekstowym wprowadź ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image22.png))


Alternatywnie użyj konstruktora zapytań i graficznie konstruowania zapytania, jak pokazano na rysunku 9.


[![Utwórz zapytanie w formie graficznej, za pomocą edytora zapytań](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Rysunek 9**: tworzenie kwerend graficznie, za pomocą edytora zapytań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image25.png))


Po utworzeniu zapytania, ale przed przejściem do następnego ekranu kliknij przycisk Opcje zaawansowane. W projektach witryny sieci Web "Generuj Insert, Update i Delete instrukcji" jest jedyną zaawansowana opcja jest zaznaczona domyślnie; Po uruchomieniu tego kreatora z biblioteki klas lub projektu Windows również zostanie wybrana opcja "Użyj optymistycznej współbieżności". Zaznaczaj opcji "Użyj optymistycznej współbieżności" teraz. W samouczkach przyszłych zajmiemy się optymistycznej współbieżności.


[![Wybierz tylko generowanie Insert, Update i Delete instrukcje opcji](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Na rysunku nr 10**: Wybierz tylko generowanie Insert, Update i Delete instrukcje opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image28.png))


Po zweryfikowaniu opcje zaawansowane, kliknij przycisk Dalej, aby przejść do końcowej ekranu. W tym miejscu są możemy proszony o wybranie metod, aby dodać do TableAdapter. Istnieją dwa wzorce umożliwiający wypełnianie danych:

- **Wypełnij DataTable** za pomocą tego podejścia, tworzona jest metoda, która przyjmuje jako parametr DataTable i wypełnia ją na podstawie wyników zapytania. Klasa ADO.NET DataAdapter, na przykład implementuje tego wzorca z jego **Fill()** metody.
- **Zwraca DataTable** w przypadku tej metody metoda tworzy i wypełnia DataTable dla Ciebie i zwraca go jako metody zwracają wartość.

Może mieć TableAdapter, zaimplementuj jeden lub oba te wzorce. Można również zmienić nazwę metody podane tutaj. Pozostawmy oba pola wyboru zaznaczone, mimo że firma Microsoft będzie używać tylko ten drugi wzorzec w tych samouczkach. Ponadto Zmieńmy nazwy zamiast ogólnych **GetData** metody **GetProducts**.

Jeśli zaznaczone, tworzy końcowej pola wyboru "GenerateDBDirectMethods," **Insert()**, **Update()**, i **Delete()** metody TableAdapter. Jeśli zaznaczaj tej opcji wszystkie aktualizacje należy wykonać za pomocą TableAdapter jedynej **Update()** metody, która przyjmuje wpisany zestaw danych, DataTable, pojedynczego elementu DataRow lub tablicę wierszy danych. (Jeśli masz unchecked "Generuj Insert, Update i Delete instrukcje" w opcji Zaawansowane właściwości na rysunku 9 to pole wyboru ustawienia odniesie żadnego skutku.) Teraz należy pozostawić zaznaczone pole wyboru.


[![Zmień nazwę metody z GetData GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Rysunek 11**: Zmień nazwę metody **GetData** do **GetProducts** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image31.png))


Ukończ pracę kreatora, klikając przycisk Zakończ. Po zamknięciu kreatora możemy są zwracane do Projektanta obiektów DataSet, w której przedstawiono DataTable, którą właśnie utworzyliśmy. Można wyświetlić listę kolumn w **produktów** DataTable (**ProductID**, **ProductName**i tak dalej), a także metody  **ProductsTableAdapter** (**Fill()** i **GetProducts()**).


[![DataTable produktów i ProductsTableAdapter zostały dodane do wpisana zestawu danych](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Rysunek 12**: **produktów** DataTable i **ProductsTableAdapter** zostały dodane do zestawu danych wpisane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image34.png))


W tym momencie mamy wpisany zestaw danych z pojedynczej tabeli DataTable (**Northwind.Products**) i silnie typizowaną klasę DataAdapter (**NorthwindTableAdapters.ProductsTableAdapter**) przy użyciu  **GetProducts()** metody. Te obiekty mogą służyć dostęp do listy wszystkich produktów z kodu, takich jak:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Ten kod nie wymagała nam napisać jeden bit kodu specyficznego dla dostępu do danych. Firma Microsoft nie ma do tworzenia wystąpień dowolnego klas ADO.NET, firma Microsoft nie będzie musiał odwoływać się do żadnych parametrów połączenia zapytania SQL lub procedur składowanych. Zamiast tego TableAdapter zawiera kod dostępu do danych niskiego poziomu dla nas.

Każdy obiekt używany w tym przykładzie jest również silnie typizowaną, dzięki czemu Visual Studio, aby zapewnić funkcji IntelliSense i sprawdzanie typów w czasie kompilacji. A najlepsze cechy wszystkich DataTable, które są zwracane przez TableAdapter może być powiązana z formantów sieci Web, takich jak kontrolki GridView, DetailsView, DropDownList, CheckBoxList i kilka innych danych w programie ASP.NET. W poniższym przykładzie pokazano powiązania tabeli DataTable zwróconej przez **GetProducts()** metody w kontrolce GridView po prostu krępowane trzech wierszy kodu w **strony\_obciążenia** programu obsługi zdarzeń.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![Lista produktów są wyświetlane w widoku GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Rysunek 13**: Lista produktów jest wyświetlany w kontrolce GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image37.png))


W tym przykładzie wymagane, możemy napisać trzech wierszach kodu strony ASP.NET **strony\_obciążenia** program obsługi zdarzeń, w przyszłości zajmiemy się jak za pomocą kontrolki ObjectDataSource deklaratywne odbierać dane z samouczków WARSTWA DAL. Za pomocą kontrolki ObjectDataSource firma Microsoft nie będziesz mieć do pisania kodu i otrzyma obsługi stronicowania i sortowania.

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Krok 3: Dodawanie sparametryzowane metody służące do warstwy dostępu do danych

W tym momencie nasze **ProductsTableAdapter** klasa ma jednak jedną metodę **GetProducts()**, która zwraca wszystkich produktów w bazie danych. Możliwość pracy w przypadku pozostałych produktów jest zdecydowanie użyteczne, istnieją czasy, kiedy chcemy będzie można pobrać informacji na temat określonego produktu lub wszystkie produkty, które należą do określonej kategorii. Aby dodać funkcjonalność do naszych warstwy dostępu do danych możemy dodać sparametryzowane metody do TableAdapter.

Dodajmy **GetProductsByCategoryID (*categoryID*)** metody. Aby dodać nową metodę do warstwy DAL, wróć do Projektanta obiektów DataSet, kliknij prawym przyciskiem myszy **ProductsTableAdapter** sekcji, a następnie wybierz polecenie Dodaj zapytanie.


![Kliknij prawym przyciskiem myszy na obiekt TableAdapter i wybierz polecenie Dodaj zapytanie](creating-a-data-access-layer-cs/_static/image38.png)

**Rysunek 14**: kliknij prawym przyciskiem myszy na obiekt TableAdapter i wybierz polecenie Dodaj zapytanie


Firma Microsoft są najpierw pojawi się monit dotyczący tego, czy ma dostęp do bazy danych za pomocą instrukcji SQL zapytań ad-hoc lub nowej lub istniejącej procedury składowanej. Wybierzmy użyć instrukcji SQL zapytań ad-hoc ponownie. Są następnie proszeni jakiego typu zapytania SQL prosimy o poświęcenie do użycia. Ponieważ chcemy zwrócić wszystkie produkty, które należą do określonej kategorii, chcemy napisać **wybierz** instrukcji, która zwraca wiersze.


[![Utwórz instrukcję SELECT, która zwraca wiersze](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Rysunek 15**: możliwość tworzenia **wybierz** instrukcji która zwraca wiersze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image41.png))


Następnym krokiem jest określenie zapytania SQL używany do uzyskania dostępu do danych. Ponieważ chcemy zwracać tylko te produkty, które należą do określonej kategorii, czy mogę używać tego samego <strong>wybierz</strong> instrukcji z <strong>GetProducts()</strong>, Dodaj następujący kod, ale <strong>gdzie</strong> Klauzula: <strong>gdzie CategoryID = @CategoryID</strong> . <strong>@CategoryID</strong> Parametr wskazuje kreatora TableAdapter, że metoda tworzymy będą wymagały parametr wejściowy odpowiedniego typu (to znaczy, integer dopuszczającego wartość null).


[![Wprowadź zapytanie, aby zwracać tylko produktów w określonej kategorii](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Rysunek 16**: Wprowadź kwerendę tylko zwrócić produktów w określonej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image44.png))


W ostatnim kroku możemy wybrać który wzorce do użycia, a także dostosować nazwy metody generowane dostępu do danych. Deseń wypełnienia zmienimy nazwę aby <strong>FillByCategoryID</strong> i dotyczące zwracania DataTable zwracają wzorca ( <strong>uzyskać*X</strong>*  metod), użyjemy  <strong>GetProductsByCategoryID</strong>.


[![Wybierz nazwy dla metody TableAdapter](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Rysunek 17**: Wybierz nazwy dla metody TableAdapter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image47.png))


Po ukończeniu kreatora, Projektant obiektów DataSet zawiera nowe metody TableAdapter.


![Produkty mogą teraz można wykonywać zapytania według kategorii](creating-a-data-access-layer-cs/_static/image48.png)

**Rysunek 18**: produktów można teraz można wykonywać zapytania według kategorii


Poświęć chwilę, aby dodać **GetProductByProductID (*productID*)** metody przy użyciu tej samej techniki.

Można przetestować te zapytania parametrycznego bezpośrednio w Projektancie obiektów DataSet. Kliknij prawym przyciskiem myszy na metodzie, w metodzie TableAdapter, a następnie wybierz Podgląd danych. Następnie wprowadź wartości dla parametrów i kliknij przycisk (wersja zapoznawcza).


[![Te należące produktów do kategorii Beverages są wyświetlane.](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Rysunek 19**: są wyświetlane te produkty należące do kategorii Beverages ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image51.png))


Za pomocą **GetProductsByCategoryID (*categoryID*)** metody w naszym DAL, teraz można utworzyć strony ASP.NET, która wyświetla tylko te produkty w określonej kategorii. W poniższym przykładzie pokazano wszystkie produkty, które należą do kategorii Beverages, które mają **CategoryID** 1.

Beverages.asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.CS

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Tych produktów w należące do tej kategorii są wyświetlane.](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Rysunek 20**: te produkty należące do tej kategorii są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Krok 4: Wstawianie, aktualizowanie i usuwanie danych

Istnieją dwa wzorce często używane do wstawiania, aktualizowania i usuwania danych. Pierwszy wzorzec będzie wywoływany wzorzec bezpośrednie bazy danych obejmuje tworzenie metod, które po wywołaniu, problem **Wstaw**, **aktualizacji**, lub **Usuń** polecenia Baza danych, która operuje na rekord pojedynczej bazy danych. Takie metody zwykle są przekazywane w serii wartości skalarnych (liczby całkowite, ciągi, wartości logicznych, Data/Godzina i tak dalej), które odpowiadają wartościom do wstawiania, aktualizacji lub usuwania. Na przykład, w ramach tego wzorca dla **produktów** tabeli metodę delete zajmie się parametru liczby całkowitej wskazujący **ProductID** rekordu, aby usunąć, gdy metoda Wstaw zajęłoby ciąg dla **ProductName**, dziesiętny dla **UnitPrice**, liczbę całkowitą dla **UnitsOnStock**i tak dalej.


[![Każdy Insert, Update i Usuń żądania, które są wysyłane do bazy danych natychmiast](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Rysunek 21**: każdy Insert, Update i Usuń żądania, które są wysyłane do bazy danych od razu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image57.png))


Inne wzorzec, który mogę odnosić się do w partii aktualizacji wzorzec, jest aktualizacji całego zestawu danych, DataTable lub Kolekcja wierszy danych w jednego wywołania metody. W ramach tego wzorca Deweloper usuwa, wstawia modyfikuje wierszy danych w elemencie DataTable i następnie przekazuje do metody aktualizacji tych wierszy danych ani objekt DataTable. Ta metoda następnie wylicza dotyczy to również utworzeń przekazanej, określa, czy są już został zmodyfikowany, dodane lub usunięty (za pośrednictwem elementu DataRow [właściwość RowState](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) wartość) i wystawia żądanie odpowiednią bazę danych dla każdego rekordu.


[![Wszystkie zmiany są synchronizowane z bazą danych, gdy wywoływana jest metoda aktualizacji](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Rysunek 22**: wszystkie zmiany są synchronizowane z bazą danych, gdy wywoływana jest metoda aktualizacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter domyślnie używa wzorca aktualizacji usługi batch, ale także obsługuje bezpośredni wzorcu bazy danych. Ponieważ wybrano opcję "Generuj Insert, Update i Delete instrukcji" we właściwościach Zaawansowane przy tworzeniu naszych TableAdapter **ProductsTableAdapter** zawiera **Update()** metody który implementuje wzorzec aktualizacji usługi batch. W szczególności zawiera TableAdapter **Update()** metody, które mogą być przekazywane wpisane DataSet, DataTable silnie typizowane lub jeden lub więcej wierszy danych. Jeśli pozostawiono zaznaczone pole wyboru "GenerateDBDirectMethods", gdy najpierw tworząc TableAdapter wzorzec bezpośrednie bazy danych będą wprowadzone za pośrednictwem **Insert()**, **Update()**, i **Delete()**  metody.

Obu wzorców modyfikacji danych użyj TableAdapter **element InsertCommand**, **elementu UpdateCommand**, i **elementu DeleteCommand** właściwości do wysyłania ich **INSERT** , **Aktualizacji**, i **Usuń** poleceń w bazie danych. Możesz sprawdzić i modyfikować **element InsertCommand**, **elementu UpdateCommand**, i **elementu DeleteCommand** właściwości, klikając na obiekt TableAdapter w Projektancie obiektów DataSet, a następnie przechodząc w oknie właściwości. (Pamiętaj, że wybrano TableAdapter oraz że **ProductsTableAdapter** obiekt jest zaznaczony na liście rozwijanej w oknie dialogowym właściwości.)


[![TableAdapter ma element InsertCommand, elementu UpdateCommand i właściwości elementu DeleteCommand](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Ilustracja 23**: Obiekt TableAdapter ma **element InsertCommand**, **elementu UpdateCommand**, i **elementu DeleteCommand** właściwości ([kliknij, aby wyświetlić w pełnym rozmiarze obraz](creating-a-data-access-layer-cs/_static/image63.png))


Aby przejrzeć lub zmodyfikować dowolne z tych właściwości polecenia bazy danych, kliknij pozycję **CommandText** właściwości podrzędnej, co umożliwi wyświetlenie konstruktora zapytań.


[![Konfigurowanie INSERT, UPDATE i DELETE instrukcji w Konstruktorze zapytań](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Rysunek 24**: Konfigurowanie **Wstaw**, **aktualizacji**, i **Usuń** instrukcji w Konstruktorze zapytań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image66.png))


Poniższy przykład kodu pokazuje, jak użyć wzorca aktualizacji wsadowych dwukrotnie ceny wszystkich produktów, które nie są przerywane i mają 25 jednostek w magazynie lub mniej:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

Poniższy kod ilustruje sposób użyć wzorca bezpośrednie DB programowo usunąć określonego produktu, a następnie zaktualizować co, a następnie dodaj nowy:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Tworzenie niestandardowego wstawiania, aktualizowania i usuwania metod

**Insert()**, **Update()**, i **Delete()** metody tworzone przez metodę bezpośrednie bazy danych może być nieco uciążliwe, szczególnie w przypadku tabel z wieloma kolumnami. Wyszukiwanie w poprzednim przykładzie kodu bez funkcji IntelliSense w pomocy, nie jest szczególnie jasne co **produktów** mapuje kolumną tabeli każdego parametru wejściowego **Update()** i **Insert()**  metody. Mogą wystąpić sytuacje, kiedy ma być uruchamiany tylko zaktualizować pojedynczej kolumny lub dwóch, lub ma dostosowany **Insert()** metodę, która będzie, zwróć wartość rekordu nowo wstawionej **tożsamości** (automatycznego przyrostu) pole.

Aby utworzyć niestandardową metodę, wróć do Projektanta obiektów DataSet. Kliknij prawym przyciskiem myszy na obiekt TableAdapter i wybierz polecenie Dodaj zapytanie powrotu do kreatora TableAdapter. Na drugim ekranie firma Microsoft może wskazywać typ zapytanie w celu utworzenia. Utwórzmy metodę, która dodaje nowy produkt, a następnie zwraca wartość nowo dodanego rekordu **ProductID**. W związku z tym, wybrać opcję utworzenia **Wstaw** zapytania.


[![Utwórz metodę, aby dodać nowy wiersz do tabeli Produkty](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Rysunek 25**: Utwórz metodę, aby dodać nowy wiersz do **produktów** tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image69.png))


Na następnym ekranie **element InsertCommand**firmy **CommandText** pojawia się. Rozszerzaj tego zapytania, dodając **wybierz zakres\_IDENTITY()** na końcu zapytania, co spowoduje zwrócenie ostatnią wartość tożsamości wstawiony **tożsamości** kolumny w tym samym zakresie. (Zobacz [dokumentacji technicznej](https://msdn.microsoft.com/library/ms190315.aspx) Aby uzyskać więcej informacji na temat **zakres\_IDENTITY()** i dlaczego warto [Użyj zakresu\_IDENTITY() proceduralny @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Upewnij się, że końcowy **Wstaw** instrukcji za pomocą średnika przed dodaniem **wybierz** instrukcji.


[![Zapytanie, aby zwrócić wartość SCOPE_IDENTITY() rozszerzyć](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Rysunek 26**: rozszerzyć zapytania do zwrócenia **zakres\_IDENTITY()** wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image72.png))


Na koniec, nadaj nazwę nowej metody **InsertProduct**.


[![Ustaw nową nazwę metody InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Rysunek 27**: Ustaw nową nazwę metody **InsertProduct** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image75.png))


Po powrocie do Projektanta obiektów DataSet, zobaczysz **ProductsTableAdapter** zawiera nową metodę **InsertProduct**. Jeśli ta nowa metoda nie ma parametrów dla każdej kolumny w **produktów** tabeli, prawdopodobnie, że nie można zakończyć **Wstaw** instrukcji za pomocą średnika. Konfigurowanie **InsertProduct** metody i upewnij się, że rozdzielający je średnikiem **Wstaw** i **wybierz** instrukcji.

Domyślnie Wstaw metody problem niebędącą zapytaniem metod, co oznacza, że zwracają one liczbę wierszy dotyczy. Jednakże, chcemy **InsertProduct** metodę, aby zwrócić wartości zwracanej przez zapytanie, nie liczbę uwzględnionych wierszy. Aby to osiągnąć, należy dostosować **InsertProduct** metody **tryb ExecuteMode** właściwości **skalarną**.


[![Zmień tryb ExecuteMode właściwość skalarną](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Rysunek 28**: zmiana **tryb ExecuteMode** właściwości **skalarną** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image78.png))


Poniższy kod przedstawia nowy **InsertProduct** metody w akcji:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Krok 5: Kończenie warstwy dostępu do danych

Należy pamiętać, że **ProductsTableAdapters** klasy zwraca **CategoryID** i **IDDostawcy** wartości z kolekcji **produktów** tabeli, ale nie obejmuje **CategoryName** kolumny z **kategorie** tabeli lub **CompanyName** kolumny z **dostawców**tabeli, mimo że prawdopodobnie są kolumn, firma Microsoft ma być wyświetlana, gdy są wyświetlane informacje o produkcie. Firma Microsoft może rozszerzyć początkowe metody TableAdapter, **GetProducts()**, aby uwzględnić obie **CategoryName** i **CompanyName** wartości kolumny, które zostaną zaktualizowane silnie typizowane DataTable, aby uwzględnić te nowe kolumny.

Może to spowodować problem, jednak jako metody TableAdapter do wstawiania, aktualizowania, i usuwanie danych opierają się poza tym metodę początkowej. Na szczęście metody generowane automatycznie dla Wstawianie, aktualizowanie i usuwanie, nie są wpływ podzapytań w **wybierz** klauzuli. Zwracając szczególną uwagę na naszych zapytania, aby dodać **kategorie** i **dostawców** jako podzapytań, zamiast **Dołącz** s, firma Microsoft będzie uniknąć konieczności przerabiać tych metod do modyfikowania danych. Kliknij prawym przyciskiem myszy **GetProducts()** method in Class metoda **ProductsTableAdapter** i wybierz opcję Konfiguruj. Następnie Dostosuj **wybierz** klauzuli tak, aby wyglądał jak:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![Aktualizacja instrukcji SELECT w metodzie GetProducts()](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Rysunek 29**: aktualizacja **wybierz** poufności informacji dotyczące **GetProducts()** — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image81.png))


Po zaktualizowaniu **GetProducts()** metodę, aby użyć tego nowego zapytania DataTable będzie zawierać dwie nowe kolumny: **CategoryName** i **NazwaDostawcy**.


![DataTable produktów ma dwie nowe kolumny](creating-a-data-access-layer-cs/_static/image82.png)

**30 rysunek**: **produktów** DataTable ma dwie nowe kolumny


Poświęć chwilę, aby zaktualizować **wybierz** w klauzuli **GetProductsByCategoryID (*categoryID*)** również metody.

Jeśli zaktualizujesz **GetProducts()** **wybierz** przy użyciu **Dołącz** składni Projektanta obiektów DataSet nie będzie można automatycznie wygenerować metod wstawiania, aktualizowania i usuwania dane bazy danych przy użyciu wzorca bezpośrednie bazy danych. Zamiast tego należy ręcznie utworzyć je podobnie jak postępowanie z **InsertProduct** metoda we wcześniejszej części tego samouczka. Ponadto ręcznie musisz podać **element InsertCommand**, **elementu UpdateCommand**, i **elementu DeleteCommand** wartości właściwości, jeśli chcesz używać usługi batch, aktualizowanie wzorca.

## <a name="adding-the-remaining-tableadapters"></a>Dodawanie pozostałych TableAdapters

Pory tylko Opisaliśmy Praca z pojedynczy obiekt TableAdapter dla tabeli pojedynczej bazy danych. Jednak bazy danych Northwind zawiera kilka powiązanych tabel, które musisz pracować w naszej aplikacji sieci web. Wpisany zestaw danych może zawierać wiele powiązanych DataTables. W związku z tym aby ukończyć nasze warstwy DAL musimy dodać DataTable dla tabel, który będzie używany w tych samouczkach. Aby dodać nowy obiekt TableAdapter wpisana zestawu danych, Otwórz Projektanta obiektów DataSet, kliknij prawym przyciskiem myszy w Projektancie i wybierz pozycję Dodaj / TableAdapter. Spowoduje to utworzenie nowego elementu DataTable i TableAdapter i przeprowadzą Cię przez kreatora, który zbadaliśmy we wcześniejszej części tego samouczka.

Potrwać kilka minut, aby utworzyć następujące TableAdapters i metod za pomocą następujących zapytań. Należy pamiętać, że zapytania w **ProductsTableAdapter** obejmują podzapytania do pobrania nazwy kategorii i dostawcy każdego produktu. Ponadto jeśli wykonywano, dodano już **ProductsTableAdapter** klasy **GetProducts()** i **GetProductsByCategoryID (*categoryID* )** metody.

- **ProductsTableAdapter**

  - **GetProducts**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
  - **GetProductsByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
  - **GetProductsBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
  - **GetProductByProductID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

  - **GetCategories**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
  - **GetCategoryByCategoryID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

  - **GetSuppliers**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
  - **GetSuppliersByCountry**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
  - **GetSupplierBySupplierID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

  - **GetEmployees**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
  - **GetEmployeesByManager**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
  - **GetEmployeeByEmployeeID**: 

      [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![Projektant obiektów DataSet, po dodaniu cztery TableAdapters](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Rysunek 31**: DataSet Designer po czterech TableAdapters zostały dodane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Dodawanie niestandardowego kodu do warstwy DAL

TableAdapters i dodane do obiektu DataSet wpisane DataTable są wyrażane jako plik definicji schematu XML (**Northwind.xsd**). Te informacje schematu można wyświetlić, klikając prawym przyciskiem myszy **Northwind.xsd** plików w Eksploratorze rozwiązań i wybierając polecenie Pokaż kod.


[![Plik (XSD) definicji schematu XML dla Northwinds Typizowany zestaw danych](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Ilustracja 32**: plik definicji schematu XML (XSD) dla zestawu danych wpisane Northwinds ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image88.png))


Informacje o schemacie jest tłumaczona na kod C# lub Visual Basic w czasie projektowania, podczas kompilowania lub w czasie wykonywania (w razie potrzeby), w tym momencie możesz przejrzeć go za pomocą debugera. Aby wyświetlić ten kod wygenerowany automatycznie przejdź do widoku klas i testowania odzyskiwania po awarii w dół do klasy TableAdapter lub wpisana zestawu danych. Jeśli nie widzisz widoku klasy na ekranie, przejdź do menu widoku i wybierz go z tego miejsca lub naciśnij klawisze Ctrl + Shift + C. W widoku klasy widać, właściwości, metod i zdarzeń klasy wpisany zestaw danych i TableAdapter. Aby wyświetlić kod dla konkretnej metody, kliknij dwukrotnie nazwę metody w widoku klas lub kliknij prawym przyciskiem myszy na nim, a następnie wybierz Przejdź do definicji.


![Sprawdź kod wygenerowany automatycznie po wybraniu polecenia przejdź do definicji z widoku klasy](creating-a-data-access-layer-cs/_static/image89.png)

**Ilustracja 33**: Sprawdź automatycznie wygenerowany kod, wybierając Przejdź do definicji z widoku klasy


Automatycznie wygenerowany kod może być wygaszacz doskonały moment, kod jest często bardzo ogólny i musi być dostosowane do spełnianie unikatowych potrzeb aplikacji. Ryzyko rozszerzanie automatycznie wygenerowany kod jest jednak, że narzędzia, który wygenerował kod zdecydować, że nadszedł czas na "wygenerować ponownie", a następnie zastąpić dostosowania. Za pomocą nowej koncepcji częściowej klasy .NET 2.0 jest łatwe do podział klasy na wiele plików. Pozwala to nam dodać własne metody, właściwości i zdarzenia do automatycznie generowanej klasy bez konieczności martwienia się o programie Visual Studio, zastępując naszych dostosowania.

Aby zademonstrować sposobu dostosowywania warstwy DAL, dodamy **GetProducts()** metody **SuppliersRow** klasy. **SuppliersRow** klasa reprezentuje pojedynczy rekord w **dostawców** Tabela; każdy dostawca może dostawca zero do wielu produktów, dzięki czemu **GetProducts()** zwróci te produkty określonego dostawcy. Aby wykonać to utworzenie nowego pliku klasy w **aplikacji\_kodu** folder o nazwie **SuppliersRow.cs** i Dodaj następujący kod:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Tej klasy częściowej instruuje kompilator, że w przypadku tworzenia **Northwind.SuppliersRow** klasy, aby uwzględnić **GetProducts()** metoda po prostu zdefiniowana. Jeśli skompilowanie projektu, a następnie powróć do widoku klas zobaczysz **GetProducts()** teraz wymienione jako metoda **Northwind.SuppliersRow**.


![Metoda GetProducts() jest teraz częścią klasy Northwind.SuppliersRow](creating-a-data-access-layer-cs/_static/image90.png)

**Ilustracja 34**: **GetProducts()** metody jest teraz częścią **Northwind.SuppliersRow** klasy


**GetProducts()** metoda teraz może służyć do wyliczenia zestaw produktów dla określonego dostawcy, co ilustruje poniższy kod:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Dane te mogą także wyświetlane w dowolnej ASP. Dane firmy NET kontrolki sieci Web. Następująca strona używa kontrolki widoku siatki z dwóch pól:

- Wyświetla nazwę każdego z dostawców elementu BoundField i
- TemplateField, który zawiera formant BulletedList, który jest powiązany z wyników zwróconych przez **GetProducts()** metody dla każdego dostawcy.

Zajmiemy się sposób wyświetlania tych raportów wzorzec / szczegół w przyszłości samouczki. Teraz, w tym przykładzie zaprojektowano w celu zilustrowania przy użyciu metody niestandardowe dodane do **Northwind.SuppliersRow** klasy.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![Nazwa firmy dostawcy jest wyświetlana w kolumnie po lewej stronie, a ich produktów w prawo](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Ilustracja 35**: Nazwa firmy dostawcy jest wyświetlana w kolumnie po lewej stronie, a ich produktów w prawo ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Podsumowanie

Podczas tworzenia aplikacji sieci web, tworzenia warstwy DAL powinien być jednym z w ramach pierwszych kroków występuje przed rozpoczęciem tworzenia warstwą prezentacji. Za pomocą programu Visual Studio tworzenie DAL, w oparciu o wpisanych zestawów danych jest zadanie, które można wykonywać w 10 – 15 minut bez konieczności pisania nawet wiersza kodu. Samouczki, przenoszenie do przodu zostanie bazują na tym warstwy DAL. W [następnego samouczka](creating-a-business-logic-layer-cs.md) utworzymy zdefiniować liczbę reguł biznesowych i zobacz, jak wdrożyć je w oddzielne warstwy logiki biznesowej.

Wszystkiego najlepszego programowania!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji na tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Tworzenie warstwy, za pomocą silnie Typizowane TableAdapters i DataTable w programie VS 2005 i programu ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Projektowanie składników warstwy danych i przekazywanie danych za pomocą warstw](https://msdn.microsoft.com/library/ms978496.aspx)
- [Tworzenie warstwy dostępu do danych, za pomocą Projektanta obiektów DataSet 2005 Visual Studio](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Szyfrowanie informacji o konfiguracji w programie ASP.NET 2.0 aplikacji](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter — Przegląd](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Praca z Typizowany zestaw danych](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Przy użyciu dostępu do silnie Typizowanych danych w programie Visual Studio 2005 i platformy ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Jak rozszerzyć metody TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Pobieranie danych skalarną z procedury składowanej](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenie wideo na tematy zawarte w tym samouczku

- [Warstwy dostępu do danych w aplikacjach ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Jak ręcznie powiązanie zestawu danych z elementem Datagrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Jak pracować z zestawami danych i filtrami aplikacji ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor siedem ASP/ASP.NET książek i założycielem [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracował nad przy użyciu technologii Microsoft Web od 1998 r. Scott działa jako niezależny Konsultant, trainer i składnika zapisywania. Jego najnowszą książkę Stephena [ *Sams uczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). ADAM można z Tobą skontaktować w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blogu, który znajduje się w temacie [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

W tej serii samouczków został zrecenzowany przez wielu recenzentów pomocne. Wiodące osób dokonujących przeglądu, w tym samouczku zostały Piotr zielony, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez i Carlos Santos. Zainteresowani zapoznaniem Moje kolejnych artykułów MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [Next](creating-a-business-logic-layer-cs.md)
