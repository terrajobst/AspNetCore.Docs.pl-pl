---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Źródła danych kontrolki | Dokumentacja firmy Microsoft
author: microsoft
description: Formant DataGrid w programie ASP.NET 1.x oznaczone dużą poprawy dostępu do danych w aplikacji sieci Web. Jednak nie tak przyjazną dla użytkownika jako mogło być...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: b1ac7fb62767d61c97fe00338bc0f5087f4863b5
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/30/2018
ms.locfileid: "28885898"
---
<a name="data-source-controls"></a>Kontrolki źródła danych
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Formant DataGrid w programie ASP.NET 1.x oznaczone dużą poprawy dostępu do danych w aplikacji sieci Web. Jednak nie tak przyjazną dla użytkownika jako mogło być. Wymaga ona nadal znaczną ilość kodu w celu uzyskania dużo przydatne funkcje z niego. Takie jest modelem w wszystkich kwot dostępu do danych w 1.x.


Formant DataGrid w programie ASP.NET 1.x oznaczone dużą poprawy dostępu do danych w aplikacji sieci Web. Jednak nie tak przyjazną dla użytkownika jako mogło być. Wymaga ona nadal znaczną ilość kodu w celu uzyskania dużo przydatne funkcje z niego. Takie jest modelem w wszystkich kwot dostępu do danych w 1.x.

Platforma ASP.NET 2.0 rozwiązuje ten problem z częściowo z kontrolki źródła danych. Kontrolki źródła danych w programie ASP.NET 2.0 zapewnia deweloperom deklaratywne model do pobierania danych, wyświetlanie danych i edytowanie danych. Celem kontrolki źródła danych jest zapewnienie spójności reprezentację danych do formantów powiązanych z danymi, niezależnie od tego źródła danych. Istotą kontrolki źródła danych w programie ASP.NET 2.0 jest klasa abstrakcyjna format źródła danych. Klasa format źródła danych udostępnia podstawowe implementacji interfejsu IDataSource i interfejsu IListSource ostatni z nich umożliwia przypisanie kontroli źródła danych jako źródła danych (za pomocą właściwości DataSourceId nowe kontrolki powiązane z danymi omówione później) i udostępniania danych w postaci listy. Każda lista danych z formantem źródła danych jest ujawniona jako obiekt DataSourceView. Dostęp do wystąpień DataSourceView są udostępniane przez interfejsu IDataSource. Na przykład metoda GetViewNames zwraca ICollection, umożliwiający wyliczenie DataSourceViews skojarzone z kontroli źródła danych, a metoda GetView umożliwia dostęp do określonego wystąpienia DataSourceView według nazwy.

Kontrolki źródła danych ma bez interfejsu użytkownika. Są one wykonywane zgodnie z serwera określa, czy obsługują one składni deklaratywnej i dzięki czemu mają dostęp do stanu strony w razie potrzeby. Kontrolki źródła danych nie są renderowane żadnych znaczników HTML do klienta.

> [!NOTE]
> Jak można zauważyć później, są również buforowanie korzyści uzyskać za pomocą kontrolki źródła danych.


## <a name="storing-connection-strings"></a>Przechowywanie parametrów połączenia

Przed uzyskujemy do spojrzenie na sposób konfigurowania kontrolki źródła danych, firma Microsoft obejmuje nową funkcję w programie ASP.NET 2.0 dotyczące parametrów połączenia. Platforma ASP.NET 2.0 wprowadzono nową sekcję w pliku konfiguracji, który można łatwo przechowywać parametry połączenia, które mogą być odczytywane dynamicznie w czasie wykonywania. &lt;ConnectionStrings&gt; sekcji można łatwo przechowywać parametry połączenia.

Poniższy fragment dodaje nowe parametry połączenia.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Tylko jako z &lt;appSettings&gt; sekcji &lt;connectionStrings&gt; poza zostanie wyświetlona sekcja &lt;system.web&gt; sekcji w pliku konfiguracji.


Aby użyć tego ciągu połączenia, używając następującej składni podczas ustawiania atrybutu ConnectionString kontrolki serwera.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt; sekcji może również być szyfrowana, dzięki czemu nie jest uwidaczniana poufne informacje. Zdolność zostanie omówiona w późniejszym modułu.

## <a name="caching-data-sources"></a>Buforowanie źródła danych

Format każdego źródła danych zawiera cztery właściwości konfiguracji pamięci podręcznej; EnableCaching, CacheDuration CacheExpirationPolicy i CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching jest właściwości typu Boolean określającą czy buforowanie jest włączone dla formantu źródła danych.

## <a name="cacheduration-property"></a>Właściwość CacheDuration

Właściwość CacheDuration Ustawia liczbę sekund, które pamięci podręcznej pozostaje ważny. Ustawienie tej właściwości na **0** powoduje, że pamięci podręcznej, aby nadal obowiązują do momentu jawnie unieważnione.

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy Property

Właściwość CacheExpirationPolicy może być ustawiona jako **bezwzględną** lub **ruchomej**. Ustawienie bezwzględnego oznacza, że liczby sekund określonej przez właściwość CacheDuration jest maksymalną ilość czasu, który dane będą buforowane. Przez ustawienie jej na ruchomej, czas wygaśnięcia zostaje zresetowany podczas każdej operacji.

## <a name="cachekeydependency-property"></a>Właściwość CacheKeyDependency

Jeśli określono wartość ciągu dla właściwości CacheKeyDependency, ASP.NET skonfiguruje zależność pamięci podręcznej oparte na ten ciąg. Dzięki temu można jawnie unieważnienie pamięci podręcznej przez wystarczy zmienić lub usunąć CacheKeyDependency.

**Ważne**: Jeśli personifikacja jest włączona i dostęp do źródła danych i/lub zawartości danych, które są oparte na tożsamości klienta, zaleca się, że buforowanie wyłączone przez ustawienia EnableCaching na wartość False. Jeśli w tym scenariuszu włączone jest buforowanie, użytkownika innego niż użytkownik, który pierwotnie żądany danych generuje żądanie autoryzacji do źródła danych nie jest wymuszana. Dane po prostu zostanie obsłużona z pamięci podręcznej.

## <a name="the-sqldatasource-control"></a>Formant SqlDataSource

Formant SqlDataSource umożliwia dewelopera uzyskują dostęp do danych przechowywanych w relacyjnej bazie danych, która obsługuje ADO.NET. Służy dla dostawcy System.Data.SqlClient dostęp do bazy danych programu SQL Server, dostawca System.Data.OleDb, dostawcy System.Data.Odbc lub dostawcy przestrzeni nazw System.Data.OracleClient można uzyskać dostępu do bazy danych Oracle. W związku z tym SqlDataSource oczywiście nie tylko służy do uzyskiwania dostępu do danych w bazie danych programu SQL Server.

Aby można było używać SqlDataSource, wystarczy podać wartość właściwości ConnectionString i określeniu polecenia SQL lub procedurę składowaną. Formant SqlDataSource odpowiada on za Praca z podstawowej architektury ADO.NET. Jego otwarcie połączenia, wysyła zapytanie do źródła danych lub wykonuje procedurę składowaną, zwraca dane, a następnie zamyka połączenia dla Ciebie.

> [!NOTE]
> Ponieważ klasa format źródła danych jest automatycznie zamykany połączenia dla Ciebie, go należy zmniejszyć liczbę wywołań klienta generowane przez przeciek połączenia z bazą danych.


Poniższy fragment kodu wiąże kontrolkę DropDownList formantu SqlDataSource przy użyciu parametrów połączenia, który jest przechowywany w pliku konfiguracji, jak pokazano powyżej.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Jak pokazano powyżej, właściwości DataSourceMode elementu SqlDataSource Określa tryb dla źródła danych. W powyższym przykładzie właściwość DataSourceMode ma wartość elementu DataReader. W takim przypadku SqlDataSource zwróci obiektu IDataReader przy użyciu kursora tylko do przodu i tylko do odczytu. Określony typ obiektu, który jest zwracany jest kontrolowana przez dostawcę, który jest używany. W takim przypadku używam dla dostawcy System.Data.SqlClient określonych w &lt;connectionStrings&gt; sekcji w pliku web.config. W związku z tym będzie zwracany obiekt typu SqlDataReader. Określając wartość DataSet elementu DataSourceMode dane mogą być przechowywane w zestawie danych na serwerze. Ten tryb umożliwia dodanie funkcji, takich jak sortowania, stronicowania, itd. Jeśli był I powiązania danych SqlDataSource do formantu widoku GridView I czy wybrano tryb zestawu danych. Jednak w przypadku DropDownList, tryb DataReader jest poprawny wybór.

> [!NOTE]
> Gdy buforowanie SqlDataSource lub AccessDataSource właściwości DataSourceMode musi mieć ustawioną zestawu danych. Wystąpił wyjątek zostanie przeprowadzona, jeśli włączone jest buforowanie z elementu DataSourceMode DataReader.


## <a name="sqldatasource-properties"></a>SqlDataSource właściwości

Poniżej przedstawiono niektóre właściwości formantu SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Wartość logiczna określająca, czy wybierz polecenie zostało anulowane, jeśli jeden z parametrów ma wartość null. Wartość true domyślnie.

### <a name="conflictdetection"></a>Wartość ConflictDetection

W sytuacji, gdy wielu użytkowników może być aktualizowanie źródła danych w tym samym czasie właściwość wartość ConflictDetection określa zachowanie SqlDataSource formantu. Ta właściwość zwraca jedną z wartości wyliczenia ConflictOptions. Te wartości są **CompareAllValues** i **OverwriteChanges**. Jeśli ustawiono OverwriteChanges, ostatniej osoby, można zapisać danych do źródła danych spowoduje zastąpienie wcześniejszych zmian. Jednak jeśli właściwość wartość ConflictDetection CompareAllValues tworzone dla kolumny zwracane przez polecenia SelectCommand parametry i parametry są również utworzony w celu przechowywania oryginalnych wartości w każdym z tych kolumn, dzięki czemu SqlDataSource do Ustal, czy wartości zostały zmienione, ponieważ wykonano polecenia SelectCommand.

### <a name="deletecommand"></a>Element DeleteCommand

Ustawia lub pobiera ciąg SQL używany podczas usuwania wierszy z bazy danych. Może być to zapytanie SQL lub nazwa procedury składowanej.

### <a name="deletecommandtype"></a>DeleteCommandType

Ustawia lub pobiera typ polecenia usuwania, albo kwerendy SQL (tekst) lub procedury składowanej (StoredProcedure).

### <a name="deleteparameters"></a>DeleteParameters

Zwraca parametry, które są używane przez element DeleteCommand obiektu SqlDataSourceView skojarzony z formantem SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>Elementu OldValuesParameterFormatString

Ta właściwość jest używana do określania oryginalnej wartości parametrów w przypadkach, w których wartość właściwości wartość ConflictDetection CompareAllValues. Wartość domyślna to {0}, co oznacza, że oryginalne wartości parametrów będzie mieć taką samą nazwę jak oryginalny parametru. Innymi słowy, jeśli nazwa pola jest identyfikator pracownika, pierwotny parametr wartość będzie @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Ustawia lub pobiera ciągu SQL, który służy do pobierania danych z bazy danych. Może być to zapytanie SQL lub nazwa procedury składowanej.

### <a name="selectcommandtype"></a>SelectCommandType

Ustawia lub pobiera typ polecenia wyboru albo kwerendy SQL (tekst) lub procedury składowanej (StoredProcedure).

### <a name="selectparameters"></a>SelectParameters

Zwraca parametry, które są używane przez polecenia SelectCommand obiektu SqlDataSourceView skojarzony z formantem SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Pobiera lub ustawia nazwę parametru procedury składowanej, który jest używany podczas sortowania danych pobrane przez formant źródła danych. Prawidłowy tylko wtedy, gdy SelectCommandType ma ustawioną wartość StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Ciąg określający baz danych i tabel używanych w zależności bufora SQL Server rozdzielany średnikami. (Zależności buforu SQL zostanie omówiony nowsze modułu).

### <a name="updatecommand"></a>UpdateCommand

Ustawia lub pobiera ciąg SQL, który jest używany podczas aktualizowania danych w bazie danych. Może być to zapytanie SQL lub nazwa procedury składowanej.

### <a name="updatecommandtype"></a>UpdateCommandType

Ustawia lub pobiera typ polecenia update albo kwerendy SQL (tekst) lub procedury składowanej (StoredProcedure).

### <a name="updateparameters"></a>UpdateParameters

Zwraca parametry, które są używane przez UpdateCommand obiektu SqlDataSourceView skojarzony z formantem SqlDataSource.

## <a name="the-accessdatasource-control"></a>Formant AccessDataSource

Formant AccessDataSource pochodzi od klasy SqlDataSource i jest używany do tworzenia powiązań danych z bazą danych programu Microsoft Access. Właściwość ConnectionString formantu AccessDataSource jest właściwością tylko do odczytu. Zamiast używać właściwości ConnectionString, właściwość DataFile jest używana do wskaż bazę danych programu Access, jak pokazano poniżej.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource zawsze będzie ustawiał ProviderName z podstawowej SqlDataSource System.Data.OleDb i nawiązuje połączenie z bazą danych przy użyciu dostawcy Microsoft.Jet.OLEDB.4.0 OLE DB. Nie można użyć formantu AccessDataSource do nawiązania połączenia chroniony hasłem dostępu do bazy danych. Jeśli masz połączenie z bazą danych chroniony hasłem, należy używać formantu SqlDataSource.

> [!NOTE]
> Dostęp do baz danych przechowywanych w obrębie witryny sieci Web powinna zostać umieszczona w aplikacji\_katalog danych. Program ASP.NET nie zezwala na pliki w tym katalogu do przeglądania. Należy przyznać uprawnienia odczytu i zapisu do aplikacji konto procesu\_katalog danych w przypadku korzystania z bazy danych programu Access.


## <a name="the-xmldatasource-control"></a>Formantu XmlDataSource

Formantu XmlDataSource służy do danych — wiązanie danych XML z formantów powiązanych z danymi. Można powiązać z pliku XML, używając właściwości pliku danych lub można powiązać z ciągu XML przy użyciu właściwości danych. Formantu XmlDataSource przedstawia atrybutów XML za pomocą pól powiązania. W przypadkach, w których należy powiązać z wartości, które nie są reprezentowane jako atrybuty będą musieli używać określono transformatę XSL. Umożliwia także wyrażenia XPath do filtrowania danych XML.

Należy wziąć pod uwagę następujące pliku XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Zwróć uwagę, że formantu XmlDataSource używa właściwość XPath *osób, osoba* Aby odfiltrować tylko &lt;osoby&gt; węzłów. Lista DropDownList następnie wiązania danych, który z atrybutem LastName, za pomocą właściwości DataTextField.

Formantu XmlDataSource jest głównie używane do tworzenia powiązań danych tylko do odczytu danych XML, istnieje możliwość edytowania pliku danych XML. Należy pamiętać, że w takich przypadkach automatyczne wstawianie, aktualizowanie i usuwanie informacji w pliku XML nie jest wykonywane automatycznie tak jak dla innych formantów źródła danych. Zamiast tego należy napisać kod, aby ręcznie edytować dane przy użyciu następujących metod formantu XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Pobiera obiekt XmlDocument zawierający kod XML, pobierane przez formantu XmlDataSource.

### <a name="save"></a>Zapisanie

Zapisuje element XmlDocument w pamięci źródła danych.

Ważne jest, aby należy pamiętać, że metody Zapisz działa tylko po spełnieniu następujących warunków:

1. Formantu XmlDataSource używa właściwość DataFile do powiązania do pliku XML zamiast właściwości danych można powiązać z danymi XML w pamięci.
2. Przekształcenie nie jest określona za pomocą właściwości Transform lub TransformFile.

Należy pamiętać, że metody Zapisz może spowodować nieoczekiwane wyniki wywołanego przez wielu użytkowników jednocześnie.

## <a name="the-objectdatasource-control"></a>Kontrolki ObjectDataSource

Kontrolki źródła danych, których firma Microsoft ma do tego punktu są doskonałe możliwości dwuwarstwowej aplikacji, gdzie kontroli źródła danych komunikuje się bezpośrednio z magazynem danych. Jednak wiele aplikacji rzeczywistych są aplikacje wielowarstwowe gdzie kontroli źródła danych może być konieczne przekazują obiekt biznesowy, który z kolei komunikuje się z warstwą danych. W takich przypadkach ObjectDataSource wypełnia BOM dobrze. Element ObjectDataSource działa w połączeniu z obiektem źródłowym. Kontrolki ObjectDataSource spowoduje utworzenie wystąpienia obiektu źródłowego, wywołaj metodę określonej i usunięcia wystąpienia obiektu w zasięgu pojedynczego żądania, jeśli obiekt wystąpienia metody zamiast metody statyczne (Shared w języku Visual Basic). Obiekt musi mieć zatem bezstanowe. To, że obiekt należy uzyskać i zwolnić wszystkie wymagane zasoby w zasięgu pojedynczego żądania. Można kontrolować sposób tworzenia obiektu źródłowego dzięki obsłudze zdarzeń ObjectCreating kontrolki ObjectDataSource. Można utworzyć wystąpienia obiektu źródłowego, a następnie ustaw właściwość właściwości ObjectInstance klasy ObjectDataSourceEventArgs do tego wystąpienia. Kontrolki ObjectDataSource użyje wystąpienia, które jest tworzony w przypadku ObjectCreating zamiast tworzenia wystąpienia samodzielnie.

Jeśli do obiektu źródłowego kontrolki ObjectDataSource udostępnia publiczne metody statyczne (Shared w języku Visual Basic), które mogą być wywoływane, aby pobrać i zmodyfikować danych, kontrolki ObjectDataSource wywoła bezpośrednio tych metod. Jeśli kontrolki ObjectDataSource musi utworzyć wystąpienie obiektu źródłowego, aby można było wykonywać wywołania metody, obiekt musi zawierać konstruktora publicznego, który nie przyjmuje żadnych parametrów. Kontrolki ObjectDataSource będzie wywoływać konstruktora, gdy tworzy nowe wystąpienie obiektu źródłowego.

Jeśli obiekt źródłowy nie zawiera publicznego konstruktora bez parametrów, można utworzyć wystąpienia obiektu źródłowego, który będzie używany przez element ObjectDataSource formant w zdarzeniu ObjectCreating.

## <a name="specifying-object-methods"></a>Określanie metody obiektów

Do obiektu źródłowego kontrolki ObjectDataSource może zawierać dowolną liczbę metod, które są używane do wybierz, wstawiania, aktualizowania lub usuwania danych. Te metody są wywoływane przez kontrolki ObjectDataSource na podstawie nazwy metody, określonej za pomocą właściwości kontrolki ObjectDataSource albo SelectMethod, InsertMethod, UpdateMethod i DeleteMethod. Obiekt źródłowy może również zawierać opcjonalny metody SelectCount, który jest identyfikowany przez kontrolki ObjectDataSource przy użyciu właściwości SelectCountMethod, która zwraca liczbę całkowitą liczbę obiektów w źródle danych. Kontrolki ObjectDataSource wywoła metodę SelectCount po wywołaniu metody Select można pobrać całkowita liczba rekordów w źródle danych do użycia podczas stronicowania.

## <a name="lab-using-data-source-controls"></a>Laboratorium przy użyciu kontrolki źródła danych

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Ćwiczenie 1 - wyświetlanie danych z formantem SqlDataSource

Poniższym ćwiczeniu używa kontrolki SqlDataSource do nawiązania połączenia z bazą danych Northwind. Przyjęto założenie, że masz dostęp do bazy danych Northwind w wystąpieniu programu SQL Server 2000.

1. Utwórz nową witrynę sieci Web programu ASP.NET.
2. Dodaj nowy plik web.config.

    1. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i kliknij przycisk Dodaj nowy element.
    2. Wybierz plik konfiguracji sieci Web z listy szablonów, a następnie kliknij przycisk Dodaj.
3. Edytuj &lt;connectionStrings&gt; sekcji w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Przełącz do widoku kodu i dodać atrybut ConnectionString i SelectCommand atrybutu &lt;asp: SqlDataSource&gt; kontroli w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. W widoku Projekt należy dodać nowe kontrolki widoku siatki.
6. Z listy rozwijanej wybierz źródło danych w menu zadania w widoku GridView wybierz SqlDataSource1.
7. Kliknij prawym przyciskiem Default.aspx i z menu wybierz polecenie Wyświetl w przeglądarce. Kliknij przycisk Tak, gdy zostanie wyświetlony monit o zapisanie.
8. Widoku GridView wyświetla dane z tabeli Produkty.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Ćwiczenie 2 - edycji danych z formantem SqlDataSource

Poniższym ćwiczeniu pokazano, jak do tworzenia powiązań danych DropDownList kontrolować za pomocą składni deklaratywnej i pozwala na edycję dane podane w formancie DropDownList.

1. W widoku Projekt należy usunąć formant widoku GridView z Default.aspx. 

    **Ważne**: pozostaw SqlDataSource kontrolki na stronie.
2. Dodawanie formantu DropDownList do Default.aspx.
3. Przejdź do widoku źródłowego.
4. Dodaj atrybut DataSourceId, DataTextField i DataValueField do &lt;asp: Lista DropDownList&gt; kontroli w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Zapisz plik Default.aspx i wyświetlany w przeglądarce. Należy pamiętać, że lista DropDownList zawiera wszystkie produkty z bazy danych Northwind.
6. Zamknij przeglądarkę.
7. W widoku źródła Default.aspx dodać nową kontrolkę TextBox poniżej formantu DropDownList. Zmień właściwości Identyfikatora pola tekstowego txtProductName.
8. W obszarze formantu TextBox dodać nową kontrolkę przycisku. Zmień wartość właściwości identyfikator przycisku btnUpdate, a właściwość tekst do **nazwę produktu aktualizacji**.
9. W widoku źródła Default.aspx Dodaj właściwość UpdateCommand i dwa nowe UpdateParameters do tagu w źródle SqlDataSource w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Należy pamiętać, że istnieją dwa aktualizacji parametrów (ProductName i ProductID) dodanych w tym kodzie. Te parametry są zamapowane na właściwości Text txtProductName pole tekstowe i właściwości SelectedValue ddlProducts DropDownList.
10. Przełącz do widoku projektu, a następnie kliknij dwukrotnie ikonę do formantu przycisku Dodaj program obsługi zdarzeń.
11. Dodaj następujący kod do btnUpdate\_kliknij kodu: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Kliknij prawym przyciskiem Default.aspx i wybierz, aby wyświetlić ją w przeglądarce. Po wyświetleniu monitu, aby zapisać wszystkie zmiany, kliknij przycisk Tak.
13. Platforma ASP.NET 2.0 klasy częściowe umożliwiają kompilacji w czasie wykonywania. Nie jest konieczne utworzyć aplikację, aby zobaczyć wprowadzone zmiany kodu.
14. Wybierz produkt z DropDownList.
15. Wprowadź nową nazwę dla produktu wybrane w polu tekstowym, a następnie kliknij przycisk Aktualizuj.
16. Nazwa produktu jest aktualizowana w bazie danych.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Ćwiczenie 3 za pomocą kontrolki ObjectDataSource

Tego ćwiczenia pokazują, jak używać kontrolki ObjectDataSource i obiektu źródłowego do interakcji z bazy danych Northwind.

1. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i kliknij pozycję Dodaj nowy element.
2. Wybierz z listy szablonów formularza sieci Web. Zmień nazwę na object.aspx, a następnie kliknij przycisk Dodaj.
3. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań i kliknij pozycję Dodaj nowy element.
4. Wybierz klasę na liście szablonów. Zmień nazwę klasy NorthwindData.cs, a następnie kliknij przycisk Dodaj.
5. Kliknij przycisk Tak, po wyświetleniu monitu, aby dodać klasę do aplikacji\_katalogu z kodem.
6. Dodaj następujący kod do pliku NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Dodaj następujący kod do widoku źródła object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Zapisz wszystkie pliki i Przeglądaj object.aspx.
9. Współdziałać z interfejsem przez wyświetlanie szczegółów, edytowanie pracowników, dodawanie pracowników i usuwanie pracowników.
