---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: "Tworzenie Warstwa dostępu do danych (C#) | Dokumentacja firmy Microsoft"
author: rick-anderson
description: "W tym samouczku możemy rozpocząć od początku i tworzenie danych warstwy dostępu (DAL), za pomocą typizowane zbiory danych, dostęp do informacji w bazie danych."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: 927b2490b5c539a79bb9939b88942499b23cc464
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/24/2018
---
<a name="creating-a-data-access-layer-c"></a>Tworzenie Warstwa dostępu do danych (C#)
====================
przez [Bento Scott](https://twitter.com/ScottOnWriting)

[Pobierz przykładową aplikację](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe) lub [pobierania plików PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> W tym samouczku możemy rozpocząć od początku i tworzenie danych warstwy dostępu (DAL), za pomocą typizowane zbiory danych, dostęp do informacji w bazie danych.


## <a name="introduction"></a>Wprowadzenie

Jako projektantom sieci web nasze życie obracać wokół Praca z danymi. Utworzymy baz danych do przechowywania danych, kod, aby pobrać i zmodyfikować i stron sieci web do zbierania i podsumować. To jest pierwszy samouczek w długich serii, która będzie Eksploruj techniki stosowania tych wspólnych wzorców w programie ASP.NET 2.0. Zaczniemy tworzenie [architektura oprogramowania](http://en.wikipedia.org/wiki/Software_architecture) składa się z danych Access warstwy (DAL) przy użyciu wpisanych zestawów danych, warstwy logiki biznesowej (BLL) który wymusza niestandardowych reguł biznesowych i stron złożony z ASP.NET warstwy prezentacji Udostępnianie wspólnych układ strony. Gdy ten przygotowuje wewnętrznej bazy danych został utworzony, firma Microsoft będzie przenieść do raportowania, przedstawiający sposób wyświetlania, podsumować, zbieranie i sprawdzanie poprawności danych z aplikacji sieci web. Te samouczki są dostosowane do zwięzły i zawierają instrukcje krok po kroku z dużą ilością zrzuty ekranu wizualnie prowadzą użytkownika przez proces. Każdego samouczka jest dostępna w C# i Visual Basic wersji i obejmuje pobieranie pełny kod używany. (W tym pierwszym samouczku jest bardzo długi, ale pozostałe są prezentowane w większą o wysokim strawności fragmentów).

Te samouczki będzie używana wersja programu Microsoft SQL Server 2005 Express Edition, bazy danych Northwind, umieszczone w **aplikacji\_danych** katalogu. Oprócz pliku bazy danych **aplikacji\_danych** folder zawiera także skrypty SQL do tworzenia baz danych, w przypadku, gdy chcesz użyć wersji innej bazy danych. Skrypty te mogą być również [pobrać bezpośrednio z witryny Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), jeśli chcesz użyć. Jeśli używasz innej wersji programu SQL Server z bazą danych Northwind, musisz zaktualizować **NORTHWNDConnectionString** ustawienie w aplikacji **Web.config** pliku. Aplikacja sieci web został utworzony przy użyciu programu Visual Studio 2005 Professional Edition jako projekt witryny sieci Web opartej na systemie plików. Jednak wszystkie samouczków będzie działać równie oraz z bezpłatną wersją programu Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
W tym samouczku możemy rozpocząć od początku i tworzenie danych warstwy dostępu (DAL), następuje Tworzenie warstwy logiki biznesowej (BLL) w drugim samouczek i działa na układ strony i nawigacja w trzecim polu. Pierwsze trzy głosów samouczki po trzeci zostanie utworzona na podstawę. Mamy wiele opisano w tym pierwszym samouczku, dlatego Uruchom Visual Studio i do dzieła!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Krok 1: Tworzenie projektu sieci Web i łączenia z bazą danych

Zanim można utworzyć naszych warstwy dostępu do danych (DAL), najpierw należy utworzyć witrynę sieci web i skonfiguruj naszej bazie danych. Rozpocznij od utworzenia nowego pliku oparte na systemie witryna sieci web ASP.NET. W tym celu przejdź do menu Plik i wybierz nową witrynę sieci Web w celu wyświetlania okna dialogowego nowej witryny sieci Web. Wybierz szablon witryny sieci Web ASP.NET, wartość na liście rozwijanej lokalizacji do systemu plików, wybierz folder, który można umieścić witrynę sieci web i ustawioną języka C#.


[![Tworzenie nowego pliku oparte na systemie witryny sieci Web](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Rysunek 1**: tworzenie witryny sieci Web New File System-Based ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image3.png))


Spowoduje to utworzenie nowej witryny sieci web z **Default.aspx** strony ASP.NET i **aplikacji\_danych** folderu.

Z witryną sieci web utworzony następnym krokiem jest dodać odwołanie do bazy danych w Eksploratorze serwera Visual Studio. Dodając bazy danych do Eksploratora serwera można dodać tabele, procedur składowanych, widoków i itd. wszystko z poziomu programu Visual Studio. Można także wyświetlić dane w tabeli lub tworzenie własnych zapytań ręcznie lub w postaci graficznej za pośrednictwem konstruktora zapytań. Ponadto firma Microsoft kompilowania wpisanych zestawów danych dla warstwy DAL musimy punktu Visual Studio do bazy danych, z którego można skonstruować wpisanych zestawów danych. Gdy oferujemy tego połączenia w danym momencie Visual Studio automatycznie wypełni listy rozwijanej w zarejestrowani w Eksploratorze serwera baz danych.

Procedura dodawania do Eksploratora serwera bazy danych Northwind zależą od tego, czy chcesz użyć bazy danych programu SQL Server 2005 Express Edition w **aplikacji\_danych** folderu lub w przypadku programu Microsoft SQL Server 2000 lub 2005 Konfigurowanie serwera bazy danych chcesz użyć.

## <a name="using-a-database-in-theappdatafolder"></a>Korzystanie z bazy danych w theApp\_DataFolder

Jeśli nie masz programu SQL Server 2000 lub 2005 serwer bazy danych, aby nawiązać połączenie, lub aby uniknąć konieczności dodać bazy danych na serwerze bazy danych, można użyć wersji programu SQL Server 2005 Express Edition znajduje się w pobranych websit bazy danych Northwind e's **aplikacji\_danych** folder (**NORTHWND. MDF**).

Bazy danych są umieszczane w **aplikacji\_danych** folder jest automatycznie dodawany do Eksploratora serwera. Przy założeniu, że masz programu SQL Server 2005 Express Edition, na komputerze jest zainstalowany powinna zostać wyświetlona węzła o nazwie NORTHWND. MDF w Eksploratorze serwera, które można zwijać i Eksploruj jego tabele, widoki, procedury składowanej i tak dalej (patrz rysunek 2).

**Aplikacji\_danych** folderu może również zawierać programu Microsoft Access **.mdb** pliki, które, takich jak ich odpowiedniki programu SQL Server, są automatycznie dodawane do Eksploratora serwera. Jeśli nie chcesz użyć innych opcji programu SQL Server, możesz zawsze [pobrać wersję programu Microsoft Access pliku bazy danych Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) i upuść na **aplikacji\_danych** katalogu. Należy pamiętać, że, jednak baz danych programu Access spoza jako bogate jako serwer SQL i nie są przeznaczone do użycia w scenariuszach witryny sieci web. Ponadto kilka samouczków 35 będzie korzystać z niektórych funkcji poziom bazy danych, które nie są obsługiwane przez funkcję dostępu.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Łączenie z bazą danych na serwerze bazy danych programu Microsoft SQL Server 2000 lub 2005

Alternatywnie można podłączyć do bazy danych Northwind zainstalowane na serwerze bazy danych. Jeśli serwer bazy danych nie ma bazy danych Northwind zainstalowany, należy należy go najpierw dodać do serwera bazy danych, uruchamiając skrypt instalacji zawarte w tym samouczku pobierania lub przez [pobieranie wersji programu SQL Server 2000 Northwind skrypt instalacji](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) bezpośrednio z witryny sieci web firmy Microsoft.

Po utworzeniu bazy danych zainstalowany, przejdź do Eksploratora serwera w programie Visual Studio, kliknij prawym przyciskiem myszy węzeł połączenia danych, a następnie wybierz pozycję Dodaj połączenie. Jeśli nie widzisz Eksploratora serwera, przejdź do widoku / Eksploratora serwera lub trafień Ctrl + Alt + S. Okno dialogowe Dodawanie połączenia, w którym można określić serwera do nawiązania połączenia, zostaną wyświetlone informacje dotyczące uwierzytelniania i nazwę bazy danych. Po pomyślnie skonfigurować informacje o połączeniu i kliknięciu przycisku OK, bazy danych zostanie dodany jako węzeł poniżej tego węzła połączeń danych. Można rozwinąć węzła bazy danych, aby eksplorować tabele, widoki, procedury składowane i tak dalej.


![Dodaj połączenie z bazą danych Northwind serwer bazy danych](creating-a-data-access-layer-cs/_static/image4.png)

**Rysunek 2**: Dodaj połączenie z bazą danych Northwind serwer bazy danych


## <a name="step-2-creating-the-data-access-layer"></a>Krok 2: Tworzenie Warstwa dostępu do danych

Podczas pracy z jedną opcję danych jest osadzone dane dotyczące logiki bezpośrednio do warstwy prezentacji (aplikacja sieci web uzupełnić stron ASP.NET warstwę prezentacji). Może to potrwać postaci pisanie kodu ADO.NET w części kodu strony ASP.NET lub za pomocą formantu SqlDataSource z części znaczników. W obu przypadkach takie podejście ściśle couples logika dostępu do danych z warstwą prezentacji. Zalecane podejście jest jednak do oddzielania logika dostępu do danych z warstwy prezentacji. Ta warstwa oddzielne jest określany jako warstwa dostępu do danych DAL skrócie i jest zwykle implementowany jako osobne projektu biblioteki klas. Zalety architektury warstwowej są dobrze udokumentowane (zobacz sekcję "Dalsze odczytów" na końcu tego samouczka, aby uzyskać informacje dotyczące tych korzyści) i jest to rozwiązanie teraz nastąpi przekierowanie w tej serii.

Całego kodu, która jest specyficzna dla źródła danych, takich jak tworzenie połączenia z bazą danych, wystawiania **wybierz**, **Wstaw**, **aktualizacji**, i  **Usuń** polecenia i tak dalej powinien znajdować się w warstwy DAL. Warstwa prezentacji nie powinna zawierać żadnych odwołań do tych danych kodu dostępu, ale zamiast tego należy wykonywać wywołania do warstwy DAL wszystkie dane żądań. Warstwy dostępu do danych zwykle zawierają metod dostępu do danych bazy danych. Bazy danych Northwind, na przykład ma **produktów** i **kategorii** tabele służące do rejestrowania produktów, sprzedaż i kategorie, do których należą. W naszym DAL mamy metody, takie jak:

- **GetCategories(),** zwraca informacje o wszystkich kategorii
- **GetProducts()**, która zwraca informacje o wszystkich produktów
- **GetProductsByCategoryID (*categoryID*)**, która będzie zwracać wszystkie produkty, które należą do określonej kategorii
- **GetProductByProductID (*productID*)**, która zwraca informacje o określonego produktu

Te metody, gdy została wywołana, połączenia z bazą danych, odpowiednie zapytanie i zwracają wyniki. Ważne jest sposób zostanie zwrócona te wyniki. Te metody po prostu może zwrócić zestawu danych lub DataReader wypełnione przez zapytanie bazy danych, ale najlepiej te wyniki powinny być zwracane za pomocą *silnie typizowanych obiektów*. Obiekt jednoznacznie jest jednym którego schemat sztywno jest zdefiniowany w czasie kompilacji, i na odwrót obiektu typowaniem luźnym jest jedną którego schematu nie jest znany do środowiska wykonawczego.

Na przykład elementu DataReader i zestawie danych (domyślnie) są typowaniem luźnym obiektów, ponieważ ich schematu jest definiowana za pomocą kolumny zwracane przez zapytanie bazy danych używanych do wypełniania je. Aby uzyskać dostęp do określonej kolumny z typowaniem luźnym DataTable, należy użyć składni, takich jak: ***DataTable*. Wiersze [*indeksu*] ["*columnName *"]**. Wystawiony tabeli DataTable utracić wpisywanie w tym przykładzie jest fakt, że musimy dostęp do nazwy kolumn za pomocą ciągu lub indeksem. Jednoznacznie elementu DataTable z drugiej strony, będzie miał każdego z jego kolumn zaimplementowane jako właściwości, co w kodzie, który wygląda jak: ***DataTable*. Wiersze [*indeksu*].* Element columnName***.

Aby przywrócić silnie typizowanych obiektów, deweloperzy można utworzyć własne obiektów niestandardowych biznesowych albo użyj wpisanych zestawów danych. Obiekt biznesowy jest implementowany przez dewelopera jako reprezentuje klasę, którego właściwości zazwyczaj odzwierciedla kolumny tabeli podstawowej bazy danych obiektu biznesowego. Zestaw danych wpisany jest klasą wygenerowane automatycznie przez program Visual Studio na podstawie schematu bazy danych i której członkami są jednoznacznie zgodnie z tym schemacie. Wpisane zestawu danych sam składa się z klasy, stanowiące rozszerzenie klasy zestawu danych ADO.NET, DataTable i DataRow. Oprócz jednoznacznie DataTables wpisanych zestawów danych teraz także TableAdapters, które są klas z metody wypełnianie zestawu danych DataTables i propagowanie zmian w DataTables w bazie danych.

> [!NOTE]
> Aby uzyskać więcej informacji na zalety i wady używania wpisanych zestawów danych i obiektów niestandardowych biznesowych, zapoznaj się [projektowania składników warstwy danych i przekazywanie danych za pośrednictwem warstw](https://msdn.microsoft.com/library/ms978496.aspx).


Te samouczki architektury użyjemy silnie typizowane zestawy danych. Rysunek 3 przedstawia przepływ pracy między różne warstwy aplikacji, która używa wpisanych zestawów danych.


[![Wszystkie dane kod dostępu jest były odpowiedzialne warstwy DAL](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Rysunek 3**: wszystkie dane kod dostępu jest były odpowiedzialne warstwy DAL ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Tworzenie Typizowanego zestaw danych i tabeli karty

Aby rozpocząć tworzenie naszych DAL, Rozpoczniemy przez dodanie zestawu danych wpisany do naszej projektu. W tym celu kliknij prawym przyciskiem myszy węzeł projektu w Eksploratorze rozwiązań i wybierz polecenie Dodaj nowy element. Wybierz opcję zestaw danych z listy szablonów i nadaj mu nazwę **Northwind.xsd**.


[![Wybierz dodać nowy zestaw danych do projektu](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Rysunek 4**: dodać nowy zestaw danych do projektu Your ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image10.png))


Po kliknięciu przycisku Dodaj, po wyświetleniu monitu o Dodaj zestaw danych do **aplikacji\_kod** folderu, kliknij przycisk Tak. Następnie zostanie wyświetlona projektanta dla zestawu danych typu i zostanie uruchomiony Kreator konfiguracji TableAdapter, umożliwiające dodanie Twojego pierwszego TableAdapter wpisane DataSet.

Zestaw danych wpisane służy jako kolekcję silnie typizowanych danych; składa się z jednoznacznie wystąpień elementu DataTable, z których każdy z kolei składa się z jednoznacznie wystąpienia elementu DataRow. Dla każdej z tabel bazy danych, wymagające w tej serii samouczków utworzymy jednoznacznie DataTable. Zacznijmy od tworzenie DataTable dla **produktów** tabeli.

Należy pamiętać, że jednoznacznie DataTables nie zawierają żadnych informacji o tym, jak uzyskać dostęp do danych z ich tabeli źródłowej bazy danych. Aby pobrać dane do wypełnienia DataTable, używamy klasy TableAdapter, która działa jako naszych Warstwa dostępu do danych. Dla naszych **produktów** DataTable, TableAdapter będzie zawierać metody **GetProducts()**, **GetProductByCategoryID (*categoryID*)**i tak dalej, które firma Microsoft będzie wywoływać z warstwy prezentacji. Rola obiektu DataTable jest służenie jako silnie typizowanych obiektów, używany do przekazywania danych między warstwami.

TableAdapter Kreator konfiguracji rozpoczyna się od którym należy wybrać bazę danych do pracy z. Na liście rozwijanej pokazano tych baz danych w Eksploratorze serwera. Jeśli baza danych Northwind nie zostały dodane do Eksploratora serwera, kliknięcie przycisku nowe połączenie w tej chwili, aby to zrobić.


[![Wybierz z listy rozwijanej bazy danych Northwind](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Rysunek 5**: Wybierz z listy rozwijanej bazy danych Northwind ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image13.png))


Po wybraniu bazy danych, a następnie klikając przycisk Dalej, użytkownik zostanie zapytany, czy chcesz zapisać parametry połączenia w **Web.config** pliku. Zapisz parametry połączenia można będzie uniknąć go twardych kodowanych w klasach TableAdapter, co upraszcza czynności, jeśli informacje o parametrach połączenia zmieni się w przyszłości. Jeśli wybierzesz opcję Zapisz parametry połączenia w pliku konfiguracji jest umieszczany w  **&lt;connectionStrings&gt;**  sekcję, co może być [opcjonalnie zaszyfrowanych](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) na lepsze zabezpieczeń lub zmodyfikowane później za pomocą nowego ASP.NET 2.0 stronie właściwości w ramach narzędzia administracyjnego usług IIS graficznego interfejsu użytkownika, które jest bardziej odpowiedni dla administratorów.


[![Zapisz parametry połączenia w pliku Web.config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Rysunek 6**: Zapisz parametry połączenia do **Web.config** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image16.png))


Następnie musimy zdefiniować schematu dla pierwszego elementu DataTable jednoznacznie i podać metodę pierwszy dla naszych TableAdapter do użycia podczas wypełniania jednoznacznie zestawu danych. Następujące dwa kroki są jednocześnie osiągnąć, tworząc kwerendę, która zwraca kolumny z tabeli, firma Microsoft ma w naszym elementu DataTable. Na koniec kreatora przedstawimy nazwę metody w tym zapytaniu. Po, który jest zostały wykonane, ta metoda może być wywoływany z naszych warstwy prezentacji. Metoda będzie wykonać zdefiniowane zapytania i wypełniania elementu DataTable jednoznacznie.

Aby rozpocząć kwerendy SQL możemy najpierw wskazują, jak chcemy TableAdapter wydania zapytania. Możemy użyć instrukcji SQL ad-hoc, Utwórz nową procedurę składowaną lub użyj istniejącą procedurę składowaną. Te samouczki użyjemy ad hoc instrukcji SQL. Zapoznaj się [NieTak Brianowi](http://briannoyes.net/)do artykułu, [kompilacji Warstwa dostępu do danych przy użyciu projektanta programu Visual Studio 2005 zestawu danych](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) przykład korzystanie z procedur składowanych.


[![Zapytanie danych za pomocą instrukcji SQL Ad Hoc](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Rysunek 7**: zapytania na danych przy użyciu instrukcji SQL Ad-Hoc ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image19.png))


Na tym etapie firma Microsoft można wpisać w zapytaniu SQL ręcznie. Podczas tworzenia pierwsza metoda w TableAdapter zwykle mają zapytanie zwraca tych kolumn, które muszą być wyrażone w odpowiednich DataTable. Firma Microsoft można to zrobić, tworząc kwerendę, która zwraca wszystkie kolumny oraz wszystkich wierszy z **produktów** tabeli:


[![Wprowadź kwerendę SQL w polu tekstowym](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Rysunek 8**: Wprowadź SQL zapytań do pola tekstowego ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image22.png))


Alternatywnie użyj konstruktora zapytań i graficznie utworzyć kwerendy, jak pokazano na rysunku 9.


[![Utwórz kwerendę graficznie, za pomocą edytora zapytań](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Rysunek 9**: Utwórz zapytanie graficznie, za pomocą edytora zapytań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image25.png))


Po utworzeniu kwerendy, ale przed przeniesieniem na następnym ekranie kliknij przycisk Opcje zaawansowane. W projektach witryny sieci Web "instrukcje generowania Insert, Update i Delete" jest jedyną zaawansowanych opcji Domyślnie; Po uruchomieniu tego kreatora z biblioteki klas lub projekt dla systemu Windows również należy wybrać opcję "Użyj optymistycznej współbieżności". Pozostaw opcję "Użyj optymistycznej współbieżności" unchecked teraz. W przyszłości samouczki zajmiemy się optymistycznej współbieżności.


[![Wybierz tylko generowanie Insert, Update i Delete instrukcje opcji](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Na rysunku nr 10**: Wybierz tylko generowanie Insert, Update i Delete instrukcje opcji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image28.png))


Po zweryfikowaniu zaawansowane opcje, kliknij przycisk Dalej, aby przejść do ekranu końcowego. W tym miejscu możemy jest proszony o wybranie metod do dodania do TableAdapter. Istnieją dwa wzorce wypełniania danych:

- **Wypełnij DataTable** z tej metody metody jest tworzony, który przyjmuje jako parametr DataTable i wypełnia ją na podstawie wyników zapytania. Element DataAdapter ADO.NET klasy, na przykład implementuje tego wzorca z jego **Fill()** metody.
- **Zwraca element DataTable** z tej metody metoda tworzy i wypełnienia tabeli DataTable dla Ciebie i zwraca go jako wartość zwracana z metody.

Może mieć TableAdapter zaimplementować jedną lub obie te wzorce. Można również zmienić metody dostarczone w tym miejscu. Załóżmy pozostaw oba pola wyboru zaznaczone, mimo że tylko będzie używana ostatnie wzorca w całym te samouczki. Ponadto Zmieńmy nazwy zamiast ogólnego **GetData** metodę **GetProducts**.

Jeśli zaznaczone, tworzy końcowej pola wyboru "GenerateDBDirectMethods," **operacji Insert()**, **Update()**, i **Delete()** metody TableAdapter. Jeśli zaznaczaj tej opcji wyboru, wszystkie aktualizacje należy wykonać za pomocą TableAdapter jedyny **Update()** metodę, która przyjmuje wpisane zestawu danych, DataTable, pojedynczy element DataRow lub tablicę elementów DataRows. (Jeśli udało Ci się zaznaczenie opcji "Generuj Insert, Update i Delete instrukcje" dostępnej z zaawansowanych właściwości na rysunku nr 9 tego pola wyboru ustawienia nie odniesie żadnego skutku.) Załóżmy Pozostaw zaznaczenie tego pola wyboru.


[![Zmień nazwę metody GetData do GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Rysunek 11**: Zmień nazwę metody **GetData** do **GetProducts** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image31.png))


Ukończ pracę kreatora, klikając przycisk Zakończ. Po zamknięciu kreatora możemy nastąpi powrót do Projektanta obiektów DataSet, który pokazuje DataTable właśnie utworzyliśmy. Można zapoznać się z listą kolumn w **produktów** DataTable (**ProductID**, **ProductName**i tak dalej), a także metody  **ProductsTableAdapter** (**Fill()** i **GetProducts()**).


[![Produkty DataTable i ProductsTableAdapter zostały dodane do zestawu danych typu](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Rysunek 12**: **produktów** DataTable i **ProductsTableAdapter** zostały dodane do zestawu danych typu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image34.png))


W tym momencie mamy wpisane zestawu danych z jednego elementu DataTable (**Northwind.Products**) i klasę element DataAdapter jednoznacznie (**NorthwindTableAdapters.ProductsTableAdapter**) z  **GetProducts()** metody. Te obiekty umożliwia dostęp do listy wszystkich produktów z kodu, takich jak:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Ten kod nie wymagał firmie Microsoft w celu zapisania jeden bit kodu dostępu do określonych danych. Firma Microsoft nie miał można utworzyć wystąpienia klasy żadnych ADO.NET, firma Microsoft nie mają dotyczyć wszelkie parametry połączenia, zapytania SQL lub procedur składowanych. Zamiast tego TableAdapter zawiera kod dostępu niskiego poziomu danych firmie Microsoft.

Każdy obiekt w tym przykładzie jest również jednoznacznie, dzięki czemu Visual Studio, aby zapewnić IntelliSense i sprawdzanie typu kompilacji. I najlepiej wszystkie DataTables zwrócony przez element TableAdapter może być powiązana z formantów sieci Web, takich jak widoku GridView, widoku DetailsView DropDownList, CheckBoxList i kilka innych danych w programie ASP.NET. Poniższy przykład przedstawia powiązanie z tabeli DataTable zwróconej przez **GetProducts()** metody do widoku GridView właśnie krępowane trzy wiersza kodu w **strony\_obciążenia** obsługi zdarzeń.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![Lista produktów są wyświetlane w widoku GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Rysunek 13**: Lista produktów są wyświetlane w widoku GridView ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image37.png))


Podczas gdy w tym przykładzie wymagane możemy zapisać trzy wiersze kodu strony ASP.NET **strony\_obciążenia** obsługi zdarzeń, w przyszłości samouczki zajmiemy się jak używać ObjectDataSource można deklaratywnie pobrać danych z WARSTWA DAL. Z elementu ObjectDataSource firma Microsoft nie będziesz mieć do pisania kodu i otrzyma obsługi stronicowania i sortowania!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Krok 3: Dodawanie sparametryzowana metody Warstwa dostępu do danych

W tym momencie nasze **ProductsTableAdapter** klasa ma ale jedną metodę **GetProducts()**, który zwraca wszystkie produkty w bazie danych. Możliwość pracy z wszystkich produktów przydaje się ostatecznie, jednak powodują razy, gdy chcemy, aby pobrać informacje o określonym produktem lub wszystkie produkty, które należą do określonej kategorii. Dodawanie takich funkcji do naszej Warstwa dostępu do danych możemy dodać sparametryzowane metod do TableAdapter.

Dodajmy **GetProductsByCategoryID (*categoryID*)** metody. Aby dodać nową metodę do warstwy DAL powrót do Projektanta obiektów DataSet, kliknij prawym przyciskiem myszy w **ProductsTableAdapter** sekcji, a następnie wybierz pozycję Dodaj zapytanie.


![Kliknij prawym przyciskiem myszy na obiekt TableAdapter i wybierz polecenie Dodaj zapytania](creating-a-data-access-layer-cs/_static/image38.png)

**Rysunek 14**: kliknij prawym przyciskiem myszy na obiekt TableAdapter i wybierz polecenie Dodaj zapytania


Firma Microsoft są najpierw pojawi się monit dotyczący czy chcemy dostęp do bazy danych za pomocą instrukcji SQL ad hoc lub nowego lub istniejącego procedury składowanej. Ta funkcja pozwala wybrać ponownie instrukcję SQL ad hoc. Następnie zażądano jakiego typu zapytania SQL chcemy użyć. Ponieważ chcemy zwrócić wszystkie produkty, które należą do określonej kategorii, jeśli chcesz zapisać **wybierz** instrukcji, która zwraca wiersze.


[![Wybierz utworzyć instrukcję SELECT, która zwraca wiersze](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Rysunek 15**: Wybierz utworzyć **wybierz** instrukcji która zwraca wiersze ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image41.png))


Następnym krokiem jest określenie zapytania SQL używane do uzyskiwania dostępu do danych. Ponieważ chcemy zwracać tylko te produkty, które należą do określonej kategorii I używać tego samego **wybierz** instrukcji z **GetProducts()**, ale Dodaj następujące **gdzie** Klauzula: **gdzie CategoryID = @CategoryID** . **@CategoryID**  Parametr wskazuje TableAdapter Kreator metody tworzymy wymaga parametru wejściowego typu odpowiadającego (to znaczy, integer wartości null).


[![Wprowadź kwerendę, aby zwracał tylko produktów w określonej kategorii](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Rysunek 16**: Wprowadź kwerendę tylko zwrócić produktów w określonej kategorii ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image44.png))


W ostatnim kroku, firma Microsoft może wybrać którego wzorce do użycia, jak również dostosować nazwy metody wygenerowany dostępu do danych. Deseń wypełnienia teraz Zmień nazwę, aby **FillByCategoryID** i zwrotu DataTable zwracać wzorca ( **uzyskać * X*** metody), można użyć **GetProductsByCategoryID**.


[![Wybierz nazwy dla metody TableAdapter](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Rysunek 17**: Wybierz nazwy dla metody TableAdapter ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image47.png))


Po zakończeniu pracy kreatora, Projektant obiektów DataSet zawiera nowych metod TableAdapter.


![Produkty można teraz można wykonać zapytania według kategorii](creating-a-data-access-layer-cs/_static/image48.png)

**Rysunek 18**: produkty można teraz można wykonać zapytania według kategorii


Poświęć chwilę, aby dodać **GetProductByProductID (*productID*)** metodę przy użyciu tę samą metodę.

Można przetestować te zapytania parametrycznego bezpośrednio w Projektancie obiektów DataSet. Kliknij prawym przyciskiem myszy na metody TableAdapter, a następnie wybierz Podgląd danych. Następnie wprowadź wartości dla parametrów i kliknij polecenie Podgląd.


[![Te produkty należące do należące do tej kategorii są wyświetlane.](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Rysunek 19**: tych produktów należących do należące do tej kategorii są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image51.png))


Z **GetProductsByCategoryID (*categoryID*)** metody w naszym DAL, można teraz utworzyć strony ASP.NET, który wyświetla tylko tych produktów w określonej kategorii. Poniższy przykład przedstawia wszystkie produkty w należące do tej kategorii, które mają **CategoryID** 1.

Beverages.asp

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Te produkty w należące do tej kategorii są wyświetlane.](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Rysunek 20**: tych produktów należące do tej kategorii są wyświetlane ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Krok 4: Wstawianie, aktualizowanie i usuwanie danych

Istnieją dwa wzorce najczęściej używanych Wstawianie, aktualizowanie i usuwanie danych. Pierwszy wzorzec, którego będzie można wywołać wzorzec bezpośredniego bazy danych, obejmuje tworzenie metody, które, gdy została wywołana, problem **Wstaw**, **aktualizacji**, lub **usunąć** polecenie Baza danych działa w rekordzie pojedynczej bazy danych. Takie metody zwykle są przekazywane w szereg wartości skalarnych (liczby całkowite, ciągi, wartości logiczne, dat i godzin i tak dalej), które odpowiadają wartościom do wstawiania, aktualizowania lub usuwania. Na przykład z tego wzorca dla **produktów** tabeli metody delete zajmie się do parametru liczba całkowita wskazująca **ProductID** rekordu, aby usunąć, gdy metoda Wstaw zajmie się ciąg dla **ProductName**, decimal dla **UnitPrice**, for z liczbami całkowitymi **UnitsOnStock**i tak dalej.


[![Każdy Insert, Update i usunąć żądania są wysyłane do bazy danych natychmiast](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Rysunek 21**: każdy Insert, Update i usunąć żądania są wysyłane do bazy danych od razu ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image57.png))


Inne wzorzec, który będzie można odwołać się do w partii aktualizacji wzorzec, jest aktualizacja cały zestaw danych, DataTable lub kolekcji wierszy danych w wywołaniu jedną metodę. Z tego wzorca dewelopera usuwa, wstawia, modyfikuje wierszy danych w DataTable i następnie przekazuje do metody aktualizacji tych elementów DataRows lub elementu DataTable. Ta metoda następnie wylicza wierszy danych, przekazano, określa, czy są już został zmodyfikowany, dodane lub usunięty (za pośrednictwem DataRow [RowState właściwości](https://msdn.microsoft.com/library/system.data.datarow.rowstate.aspx) wartość) i wystawia żądanie odpowiednie bazy danych dla każdego rekordu.


[![Wszystkie zmiany są synchronizowane z bazy danych, gdy wywoływana jest metoda aktualizacji](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Rysunek 22**: wszystkie zmiany są synchronizowane z bazy danych, gdy wywoływana jest metoda aktualizacji ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter korzysta ze wzorca aktualizacji wsadowych domyślnie, ale obsługuje również wzorzec bezpośredniego bazy danych. Ponieważ wybrano opcję "instrukcje generowania Insert, Update i Delete" z właściwości zaawansowane podczas tworzenia naszych TableAdapter **ProductsTableAdapter** zawiera **Update()** metody która implementuje wzorzec aktualizacji wsadowych. W szczególności zawiera TableAdapter **Update()** metody mogą być przekazywane wpisane zestawu danych, jednoznacznie DataTable lub jeden lub więcej wierszy danych. Jeśli zaznaczone pole wyboru "GenerateDBDirectMethods", gdy najpierw tworząc TableAdapter wzorzec bezpośredniego DB będą wprowadzone za pośrednictwem **operacji Insert()**, **Update()**, i **Delete()**  metody.

Oba wzorce modyfikacji danych użyj TableAdapter **InsertCommand**, **UpdateCommand**, i **elementu DeleteCommand** właściwości do wystawiania ich **Wstaw** , **Aktualizacji**, i **usunąć** polecenia w bazie danych. Możesz sprawdzić i modyfikować **InsertCommand**, **UpdateCommand**, i **elementu DeleteCommand** właściwości, klikając w TableAdapter w Projektancie obiektów DataSet, a następnie przechodzi do w oknie właściwości. (Upewnij się, że wybrano TableAdapter oraz że **ProductsTableAdapter** obiekt jest zaznaczony na liście rozwijanej w oknie właściwości.)


[![TableAdapter ma InsertCommand UpdateCommand i właściwości elementu DeleteCommand](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Rysunek 23**: TableAdapter ma **InsertCommand**, **UpdateCommand**, i **elementu DeleteCommand** właściwości ([kliknij, aby wyświetlić obrazu w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image63.png))


Aby sprawdzić lub zmodyfikuj dowolny z tych właściwości polecenia bazy danych, kliknięcie **CommandText** podwłaściwości, który pojawi się konstruktora zapytań.


[![Skonfiguruj INSERT, UPDATE i DELETE instrukcje w konstruktora zapytań](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Rysunek 24**: Konfigurowanie **Wstaw**, **aktualizacji**, i **usunąć** instrukcje w konstruktora zapytań ([kliknij, aby wyświetlić obraz w pełnym rozmiarze ](creating-a-data-access-layer-cs/_static/image66.png))


Poniższy przykład kodu pokazuje, jak na potrzeby dwukrotnie cena wszystkich produktów, które nie są przerywane, które mają 25 jednostki w magazynie lub mniej wzorzec aktualizacji partii:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

Poniższy kod ilustruje sposób Użyj wzorca bezpośredniego DB programistycznie usunąć określonego produktu, a następnie zaktualizować jednego, a następnie dodaj nową:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Tworzenie niestandardowych wstawiania, aktualizowania i usuwania metod

**Operacji Insert()**, **Update()**, i **Delete()** utworzone przez metodę bezpośredniego bazy danych mogą być nieco obciążeniem, szczególnie w przypadku tabel zawierających wiele kolumn. Wyszukiwanie w poprzednim przykładzie kodu bez IntelliSense w pomocy nie jest szczególnie jasne co **produktów** kolumny tabeli mapy do każdego parametru wejściowego **Update()** i **operacji Insert()**  metody. Czasami, gdy tylko chcemy aktualizowanie kolumny jednego lub dwóch lub mają dostosowany **operacji Insert()** metodę, która będzie prawdopodobnie, zwróć wartość rekordu nowo wstawionej **tożsamości** (automatycznego przyrostu) pole.

Aby utworzyć niestandardowe metody, wróć do Projektanta obiektów DataSet. Kliknij prawym przyciskiem myszy na obiekt TableAdapter i wybierz polecenie Dodaj zapytanie powrotu do kreatora TableAdapter. Na ekranie drugi firma Microsoft może wskazywać typ zapytanie w celu utworzenia. Umożliwia tworzenie metody, która dodaje nowe produktu, a następnie zwraca wartość nowo dodanego rekordu **ProductID**. W związku z tym zdecydować się na tworzenie **Wstaw** zapytania.


[![Tworzenie metody, aby dodać nowy wiersz do tabeli Produkty](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Rysunek 25**: Tworzenie metody, aby dodać nowy wiersz do **produktów** tabeli ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image69.png))


Na następnym ekranie **InsertCommand**w **CommandText** pojawi się. Rozszerzyć przez dodanie tego zapytania **wybierz zakres\_IDENTITY()** na końcu zapytania, które zwraca ostatnią wartość tożsamości wstawione do **tożsamości** kolumny w tym samym zakresie. (Zobacz [dokumentacji technicznej](https://msdn.microsoft.com/library/ms190315.aspx) uzyskać więcej informacji o **zakres\_IDENTITY()** i dlaczego warto [Użyj zakresu\_IDENTITY() miejsce z @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Upewnij się, że kończyć **Wstaw** instrukcji średnikiem przed dodaniem **wybierz** instrukcji.


[![Rozszerzyć zapytanie zwraca wartość SCOPE_IDENTITY()](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Rysunek 26**: rozszerzyć zapytanie do powrotu **zakres\_IDENTITY()** wartość ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image72.png))


Ponadto nazwa nowej metody **InsertProduct**.


[![Ustaw nazwę nowej metody InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Rysunek 27**: Ustaw nazwę nowej metody **InsertProduct** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image75.png))


Po powrocie do Projektanta obiektów DataSet zobaczysz, że **ProductsTableAdapter** zawiera nową metodę **InsertProduct**. Jeśli ta nowa metoda nie ma parametru dla każdej kolumny w **produktów** tabeli, prawdopodobnie pamiętasz zakończenie **Wstaw** instrukcji średnikiem. Skonfiguruj **InsertProduct** — metoda i upewnić się średnikiem oddzielającego **Wstaw** i **wybierz** instrukcje.

Domyślnie Wstaw metody problem-query metod, co oznacza, że zwracają liczba wierszy, których dotyczy. Jednak chcemy **InsertProduct** metody w celu zwrócenia wartości zwróconych przez zapytanie, nie liczbę uwzględnionych wierszy. Aby to zrobić, należy dostosować **InsertProduct** metody **tryb ExecuteMode** właściwości **skalarną**.


[![Zmień tryb ExecuteMode właściwość skalarną](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Rysunek 28**: zmiana **tryb ExecuteMode** właściwości **skalarną** ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image78.png))


Poniższy kod przedstawia nowy **InsertProduct** metody akcji:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Krok 5: Kończenie Warstwa dostępu do danych

Należy pamiętać, że **ProductsTableAdapters** klasy zwraca **CategoryID** i **IDDostawcy** wartości z **produktów** tabeli, ale nie zawiera **CategoryName** kolumny z **kategorii** tabeli lub **NazwaFirmy** kolumny z **dostawców**tabeli, chociaż są one prawdopodobnie kolumn chcemy, aby wyświetlić podczas wyświetlania informacji o produkcie. Firma Microsoft może rozszerzyć początkowe metody TableAdapter, **GetProducts()**, aby uwzględnić zarówno **CategoryName** i **NazwaFirmy** wartości kolumn, które spowoduje zaktualizowanie jednoznacznie DataTable, aby uwzględnić te nowe kolumny.

Może stanowić problem, jednak jako element TableAdapter metod wstawiania, aktualizowania, i usuwanie danych są oparte na tej metody początkowej. Na szczęście metod generowane automatycznie dla Wstawianie, aktualizowanie i usuwanie nie są wpływ podzapytań w **wybierz** klauzuli. Przez zwracając szczególną uwagę na naszych zapytania, aby dodać **kategorii** i **dostawców** jako podzapytań, zamiast **JOIN** s, firma Microsoft będzie uniknąć zmian tych metod modyfikowania danych. Kliknij prawym przyciskiem myszy **GetProducts()** metody w **ProductsTableAdapter** i wybierz opcję Konfiguruj. Następnie Dostosuj **wybierz** klauzuli wyglądającą jak:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![Aktualizacja instrukcji SELECT w przypadku metody GetProducts()](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Rysunek 29**: aktualizacja **wybierz** instrukcji dla **GetProducts()** — metoda ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image81.png))


Po zaktualizowaniu **GetProducts()** metody przy użyciu tej nowej kwerendy DataTable obejmuje dwie nowe kolumny: **CategoryName** i **NazwaDostawcy**.


![Element DataTable produktów ma dwie nowe kolumny](creating-a-data-access-layer-cs/_static/image82.png)

**Rysunek 30**: **produktów** DataTable ma dwie nowe kolumny


Poświęć chwilę, aby zaktualizować **wybierz** w klauzuli **GetProductsByCategoryID (*categoryID*)** również metody.

Po zaktualizowaniu **GetProducts()** **wybierz** przy użyciu **JOIN** składni Projektanta obiektów DataSet nie będzie można automatycznie wygenerować metod wstawiania, aktualizowania i usuwania dane bazy danych przy użyciu wzorca bezpośredniego bazy danych. Zamiast tego należy ręcznie utworzyć je podobnie jak robiliśmy z **InsertProduct** metody we wcześniejszej części tego samouczka. Ponadto ręcznie musisz podać **InsertCommand**, **UpdateCommand**, i **elementu DeleteCommand** właściwość wartości, jeśli chcesz użyć partii aktualizowania wzorca.

## <a name="adding-the-remaining-tableadapters"></a>Dodawanie pozostałych TableAdapters

Dotychczas zostały tylko analizujemy Praca z pojedynczy obiekt TableAdapter dla tabeli pojedynczej bazy danych. Niemniej jednak bazy danych Northwind zawiera kilka powiązane tabele, które trzeba będzie współpracować w naszej aplikacji sieci web. Wpisane zestawu danych może zawierać wielu powiązanych DataTables. W związku z tym do ukończenia naszego DAL musimy dodać DataTables dla tych tabel, który będzie używany w tych samouczkach. Aby dodać nowy obiekt TableAdapter wpisane zestawu danych, Otwórz Projektanta obiektów DataSet, kliknij prawym przyciskiem myszy w Projektancie i wybierz polecenie Dodaj / TableAdapter. Spowoduje to utworzenie nowego elementu DataTable i TableAdapter i przeprowadzi użytkownika przez proces kreatora, który mamy się zbadana we wcześniejszej części tego samouczka.

Potrwać kilka minut, aby utworzyć następujące pliki TableAdapters i metody, za pomocą następujących zapytań. Należy pamiętać, że zapytania w **ProductsTableAdapter** obejmują podzapytania do pobrania nazwy kategorii i dostawcy każdego produktu. Ponadto, jeśli możesz już zostały następujące, dodaniu **ProductsTableAdapter** klasy **GetProducts()** i **GetProductsByCategoryID (*categoryID* )** metody.

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


[![Projektant obiektów DataSet, po dodaniu czterech TableAdapters](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Rysunek 31**: zestaw danych projektanta po cztery TableAdapters dodano ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Dodawanie niestandardowego kodu do warstwy DAL

Pliki TableAdapters i dodane do zestawu danych typu DataTables są wyrażane jako plik definicji schematu XML (**Northwind.xsd**). Informacje o schemacie można wyświetlić, klikając prawym przyciskiem myszy **Northwind.xsd** plików w Eksploratorze rozwiązań i wybierając widoku kodu.


[![Plik definicji (XSD) schematu XML dla Northwinds wpisane zestawu danych](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Rysunek 32**: plik definicji schematu XML (XSD) dla zestawu danych typu Northwinds ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image88.png))


Informacje o schemacie jest przetłumaczyć kodu C# lub Visual Basic w czasie projektowania, gdy skompilowano lub w czasie wykonywania (w razie potrzeby), w którym można przejrzeć go z debugera. Aby wyświetlić ten kod wygenerowany automatycznie przejdź do widoku klasy i przechodzenie do szczegółów w dół do klasy TableAdapter lub wpisany zestawu danych. Jeśli nie widzisz widoku klasy na ekranie, przejdź do menu Widok i wybierz go z niej lub kliknij przycisk Ctrl + Shift + C. Z widoku klasy widać, właściwości, metod i zdarzeń klasy DataSet wpisany i TableAdapter. Aby wyświetlić kod dla konkretnej metody, kliknij dwukrotnie nazwę metody w widoku klas lub kliknij prawym przyciskiem myszy na nim, a następnie wybierz Przejdź do definicji.


![Sprawdź automatycznie wygenerowany kod po wybraniu polecenia przejdź do definicji w widoku klas](creating-a-data-access-layer-cs/_static/image89.png)

**Rysunek 33**: Sprawdź automatycznie wygenerowany kod po wybraniu polecenia przejdź do definicji w widoku klas


Automatycznie wygenerowany kod, mogą być wygaszacz doskonały moment, kod jest często bardzo ogólnych i musi być dostosowane do potrzeb unikatowy aplikacji. Ryzyko rozszerzanie automatycznie wygenerowany kod jest jednak, że narzędzia, który wygenerował kod zdecydować, że czas na "Generuj" i zastąpić dostosowania. Z nowe pojęcie częściowej klasy .NET 2.0 jest łatwe do podział klas na wiele plików. Pozwala na dodawanie własnej metody, właściwości i zdarzeń do automatycznie generowanej klasy bez martwiąc się o Visual Studio zastępowanie naszych dostosowania.

Aby zademonstrować sposób dostosowywania warstwy DAL, Dodajmy **GetProducts()** metodę **SuppliersRow** klasy. **SuppliersRow** klasa reprezentuje jeden rekord w **dostawców** tabeli; każdy dostawca może dostawcy zero do wielu produktów, więc **GetProducts()** zwróci tych produkty określonego dostawcy. Do wykonania to utworzenie nowego pliku klasy w **aplikacji\_kod** folder o nazwie **SuppliersRow.cs** i Dodaj następujący kod:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Tej klasy częściowej instruuje kompilator, że w przypadku tworzenia **Northwind.SuppliersRow** klasy, aby uwzględnić **GetProducts()** metoda po prostu zdefiniowana. Jeśli skompilowanie projektu, a następnie wróć do widoku klasy zostanie wyświetlona **GetProducts()** teraz wyświetlany jako metoda **Northwind.SuppliersRow**.


![Metoda GetProducts() jest teraz częścią klasy Northwind.SuppliersRow](creating-a-data-access-layer-cs/_static/image90.png)

**Rysunek 34**: **GetProducts()** metody jest teraz częścią **Northwind.SuppliersRow** — klasa


**GetProducts()** metody można teraz używać wyliczyć zestawu produktów dla określonego dostawcy, jak przedstawiono na poniższym kodem:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Dane te mogą także wyświetlane w żadnym z ASP. Formanty danych w sieci Web. Następująca strona używa kontrolki GridView z dwóch pól:

- Pole BoundField, który wyświetla nazwę każdego z dostawców i
- TemplateField, który zawiera listy BulletedList formant, który jest powiązany z wyników zwróconych przez **GetProducts()** metody dla każdego dostawcy.

Zajmiemy się sposób wyświetlania tych raportów główny szczegółowy w przyszłości samouczki. Teraz, w tym przykładzie zaprojektowano w celu zilustrowania przy użyciu metody niestandardowe dodane do **Northwind.SuppliersRow** klasy.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![Nazwa firmy dostawcy jest podany w kolumnie po lewej, ich produktów w prawo](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Rysunek 35**: Nazwa firmy dostawcy jest podany w kolumnie po lewej, ich produktów w prawo ([kliknij, aby wyświetlić obraz w pełnym rozmiarze](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Podsumowanie

Podczas tworzenia aplikacji sieci web Tworzenie warstwy DAL powinien być jednym z etapów pierwszego wystąpienia przed rozpoczęciem tworzenia warstwą prezentacji. Program Visual Studio jest utworzenie DAL, w oparciu o wpisanych zestawów danych zadania, które można wykonywać w 10 – 15 minut bez pisania wiersz kodu. Samouczki następnych będzie zależą od tej warstwy DAL. W [następny samouczek](creating-a-business-logic-layer-cs.md) firma Microsoft będzie określić liczbę reguł biznesowych i zobacz, jak ich wdrażania w oddzielnych warstwy logiki biznesowej.

Programowanie przyjemność!

## <a name="further-reading"></a>Dalsze informacje

Więcej informacji dotyczących tematów omówionych w tym samouczku można znaleźć w następujących zasobach:

- [Tworzenie warstwy DAL przy użyciu silnie Typizowanej TableAdapters i DataTables VS 2005 i ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Projektowanie składników warstwy danych i przekazywania danych za pomocą warstw](https://msdn.microsoft.com/library/ms978496.aspx)
- [Tworzenie warstwy dostępu do danych przy użyciu projektanta programu Visual Studio 2005 zestawu danych](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Szyfrowanie informacji o konfiguracji w programie ASP.NET 2.0 aplikacji](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [TableAdapter — Przegląd](https://msdn.microsoft.com/library/bz9tthwx.aspx)
- [Praca z Typizowanego obiektu DataSet](https://msdn.microsoft.com/library/esbykkzb.aspx)
- [Dostępu do danych jednoznacznie w Visual Studio 2005 i ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Jak rozszerzyć metody TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Pobieranie danych skalarnych z procedury składowanej](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Szkolenie wideo na tematy zawarte w tym samouczku

- [Warstwy dostępu do danych w aplikacjach ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Jak ręcznie powiązać zestawu danych z elementu Datagrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Jak pracować z zestawami danych i filtrów aplikacji ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Informacje o autorze

[Scott Bento](http://www.4guysfromrolla.com/ScottMitchell.shtml), autora siedmiu książek ASP/ASP.NET i twórcę z [4GuysFromRolla.com](http://www.4guysfromrolla.com), pracuje z technologii Microsoft Web od 1998. Scott działa jako niezależnego konsultanta trainer i składnika zapisywania. Jest jego najnowszej książki [ *Sams nauczyć się ASP.NET 2.0 w ciągu 24 godzin*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Piotr można uzyskać pod adresem [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) lub za pośrednictwem jego blog, który znajduje się w temacie [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Specjalne podziękowania dla

Ten samouczek serii zostało sprawdzone przez wiele recenzentów przydatne. Prowadzić osób dokonujących przeglądu, w tym samouczku zostały Piotr zielony, Hilton Giesenow firmy Dennis Patterson, Liz Shulok, Gomez abela i Santos Artur. Zainteresowani recenzowania Moje nadchodzących artykuły MSDN? Jeśli tak, Porzuć mnie linii w [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Next](creating-a-business-logic-layer-cs.md)
