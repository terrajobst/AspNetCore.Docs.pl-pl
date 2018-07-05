---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: Źródła danych kontrolki | Dokumentacja firmy Microsoft
author: microsoft
description: Formant DataGrid na platformie ASP.NET 1.x oznaczone doskonałe poprawę dostęp do danych w aplikacji sieci Web. Jednak to nie było jak przyjazna dla użytkownika jako mogło być...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: 225e23c0e7f4c2f4b8a54ee3c07e2614e9ba1038
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388456"
---
<a name="data-source-controls"></a>Kontrolki źródła danych
====================
przez [firmy Microsoft](https://github.com/microsoft)

> Formant DataGrid na platformie ASP.NET 1.x oznaczone doskonałe poprawę dostęp do danych w aplikacji sieci Web. Jednak to nie było jak przyjazna dla użytkownika jako mogło być. Wymaga ona nadal znaczną ilość kodu w celu uzyskania wiele przydatnych funkcji z niego. Przykład jest modelem w wszystkich przedsięwzięciach dostępu do danych w 1.x.


Formant DataGrid na platformie ASP.NET 1.x oznaczone doskonałe poprawę dostęp do danych w aplikacji sieci Web. Jednak to nie było jak przyjazna dla użytkownika jako mogło być. Wymaga ona nadal znaczną ilość kodu w celu uzyskania wiele przydatnych funkcji z niego. Przykład jest modelem w wszystkich przedsięwzięciach dostępu do danych w 1.x.

ASP.NET 2.0 rozwiązuje ten problem z częściowo z kontrolki źródła danych. Kontrolki źródła danych w programie ASP.NET 2.0 zapewnia deweloperom deklaratywny model do pobierania danych, wyświetlanie danych i edytowania danych. Kontrolki źródła danych ma na celu zapewnienia spójna reprezentacja danych do kontrolek powiązanych z danymi niezależnie od źródła tych danych. W ramach kontrolki źródła danych w programie ASP.NET 2.0 jest klasa abstrakcyjna format źródła danych. Klasa format źródła danych udostępnia podstawowej implementacji interfejsu IDataSource i interfejsu IListSource ostatni z nich umożliwia przypisanie do kontroli źródła danych jako źródła danych (za pośrednictwem nową właściwość DataSourceId kontrolki powiązane z danymi omówiono w dalszej części) i uwidocznić dane zapisane w postaci listy. Każda lista danych, z poziomu kontroli źródła danych jest udostępniany jako obiekt DataSourceView. Dostęp do wystąpienia DataSourceView jest zapewniana przez interfejsu IDataSource. Na przykład metoda GetViewNames zwraca interfejs ICollection, umożliwiający wyliczenie DataSourceViews skojarzone z kontrolą źródła danych, a metoda GetView umożliwia dostęp do konkretnego wystąpienia DataSourceView według nazwy.

Kontrolki źródła danych mają bez interfejsu użytkownika. Są implementowane jako serwera decyduje, czy obsługują one składni deklaratywnej i aby mieli oni dostęp do stanu strony w razie potrzeby. Kontrolki źródła danych nie są renderowane żadnych znaczników HTML do klienta.

> [!NOTE]
> Jak zobaczysz później, są również buforowania korzyści można uzyskać za pomocą kontrolki źródła danych.


## <a name="storing-connection-strings"></a>Przechowywanie parametrów połączenia

Przed przejściem do spojrzenie na sposób konfigurowania kontrolki źródła danych, firma Microsoft obejmuje nową funkcję w programie ASP.NET 2.0 dotyczących parametrów połączenia. Program ASP.NET 2.0 wprowadzono nową sekcję w pliku konfiguracji, który można łatwo przechowywać parametry połączenia, które mogą być odczytywane dynamicznie w czasie wykonywania. &lt;ConnectionStrings&gt; sekcji można łatwo przechowywać parametry połączenia.

Poniższy fragment kodu dodaje nowe parametry połączenia.

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> Podobnie jak &lt;appSettings&gt; sekcji &lt;connectionStrings&gt; poza pojawi się sekcja &lt;system.web&gt; sekcji w pliku konfiguracji.


Aby użyć tych parametrów połączenia, można użyć następującej składni, podczas ustawiania atrybutu ConnectionString formant serwera.

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt; sekcji może również być szyfrowana, tak, aby nie jest uwidaczniana, poufne informacje. Tej możliwości zostaną omówione w nowszym module.

## <a name="caching-data-sources"></a>Buforowanie źródła danych

Każdy format źródła danych zawiera cztery właściwości konfiguracji pamięci podręcznej; EnableCaching, CacheDuration, CacheExpirationPolicy i CacheKeyDependency.

## <a name="enablecaching"></a>EnableCaching

EnableCaching jest właściwość typu Boolean określającą, czy pamięć podręczna jest włączona dla kontroli źródła danych.

## <a name="cacheduration-property"></a>Właściwość CacheDuration

Właściwość CacheDuration Ustawia liczbę sekund pamięci podręcznej pozostaje ważny. Ustawienie tej właściwości na **0** powoduje, że pamięć podręczna pozostanie ważny aż do jawnie unieważnione.

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy Property

Właściwość CacheExpirationPolicy może być ustawiony na **bezwzględne** lub **ruchomej**. Ustawieniem dla niego bezwzględne oznacza, że maksymalną ilość czasu, które będą buforowane dane są liczby sekund określonej przez właściwość CacheDuration. Ustawiając wartość ruchomej, czas wygaśnięcia zostaje zresetowany, gdy każda operacja jest wykonywana.

## <a name="cachekeydependency-property"></a>Właściwość CacheKeyDependency

Jeśli wartość ciągu jest określona dla właściwości CacheKeyDependency, ASP.NET skonfiguruje zależność pamięci podręcznej na podstawie tego ciągu. Dzięki temu można jawnie unieważnienia pamięci podręcznej, po prostu zmienić lub usunąć CacheKeyDependency.

**Ważne**: Jeśli personifikacja jest włączona, a dostęp do źródła danych i/lub zawartości danych, które są oparte na tożsamości klienta, zalecane jest, że buforowanie można wyłączyć, ustawiając EnableCaching na wartość False. Jeśli w tym scenariuszu włączone jest buforowanie, użytkownik inny niż użytkownik, który pierwotnie żądaną danych generuje żądanie autoryzacji do źródła danych nie jest wymuszana. Dane będą po prostu udostępnianie z pamięci podręcznej.

## <a name="the-sqldatasource-control"></a>Kontrolki SqlDataSource

Kontrolki SqlDataSource umożliwia deweloperom dostęp do danych przechowywanych w relacyjnej bazie danych, która obsługuje ADO.NET. Służy dostawcy System.Data.SqlClient dostęp do bazy danych programu SQL Server, dostawca System.Data.OleDb, dostawca System.Data.Odbc lub dostawcę klient system.Data.Oracle dostęp do oprogramowania Oracle. W związku z tym SqlDataSource bez obaw nie służy wyłącznie do uzyskiwania dostępu do danych w bazie danych programu SQL Server.

Aby skorzystać z kontrolką SqlDataSource, możesz po prostu podać wartość dla właściwości ConnectionString i określić za pomocą polecenia SQL lub procedury składowanej. Kontrolki SqlDataSource zajmuje się praca z podstawową architekturę ADO.NET. Jego otwarcie połączenia, wysyła zapytanie do źródła danych lub wykonuje procedurę przechowywaną, zwraca dane, a następnie zamyka połączenie dla Ciebie.

> [!NOTE]
> Ponieważ klasa format źródła danych automatycznie zamyka połączenie dla Ciebie, jego powinno zmniejszyć liczbę wywołań klienta generowany przez wyciek połączenia z bazą danych.


Poniższy fragment kodu wiąże formant DropDownList z kontrolką SqlDataSource przy użyciu parametrów połączenia, który jest przechowywany w pliku konfiguracji, jak pokazano powyżej.

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

Jak pokazano powyżej, właściwości DataSourceMode z kontrolką SqlDataSource Określa tryb dla źródła danych. W powyższym przykładzie ustawiono właściwość DataSourceMode DataReader. W takim przypadku SqlDataSource zwróci obiekt IDataReader przy użyciu kursora tylko do przodu i tylko do odczytu. Określony typ obiektu, który jest zwracany jest kontrolowana przez dostawcę, który jest używany. W tym przypadku używam dostawcy System.Data.SqlClient określonych w &lt;connectionStrings&gt; sekcja pliku web.config. W związku z tym obiekt, który jest zwracany będzie typu SqlDataReader. Określając wartość elementu DataSourceMode zestawu danych, dane mogą być przechowywane w zestawie danych na serwerze. Ten tryb umożliwia dodawanie funkcji, takich jak sortowanie, stronicowanie itp. Jeśli mam była powiązanie danych SqlDataSource do kontrolki GridView I czy wybrano tryb zestawu danych. W przypadku kontrolki DropDownList, tryb DataReader jest odpowiednim wyborem.

> [!NOTE]
> Gdy buforowanie SqlDataSource lub AccessDataSource, właściwości DataSourceMode musi mieć wartość do zestawu danych. Wyjątek wystąpi po włączeniu buforowania elementu DataSourceMode DataReader.


## <a name="sqldatasource-properties"></a>Właściwości SqlDataSource

Poniżej przedstawiono niektóre właściwości kontrolki SqlDataSource.

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

Wartość logiczna określająca, czy polecenie select została anulowana, jeśli jeden z parametrów ma wartość null. Wartość true domyślnie.

### <a name="conflictdetection"></a>ConflictDetection

W sytuacji, gdy wielu użytkowników może być aktualizowanie źródła danych w tym samym czasie właściwość ConflictDetection określa zachowanie kontrolki SqlDataSource. Ta właściwość zwraca jedną z wartości wyliczenia ConflictOptions. Te wartości są **CompareAllValues** i **OverwriteChanges**. Jeśli ustawienie OverwriteChanges i nazwisko ostatniej osoby, które można zapisać danych do źródła danych spowoduje zastąpienie wszystkich zmian poprzednim. Jednak jeśli ustawiono właściwość ConflictDetection CompareAllValues, parametry są tworzone dla kolumny zwracane przez polecenia SelectCommand i parametry są również tworzone na potrzeby przechowywania oryginalnych wartości w każdym z tych kolumn, dzięki czemu SqlDataSource do Ustal, czy wartości zmieniły się od czasu polecenia SelectCommand został wykonany.

### <a name="deletecommand"></a>Element DeleteCommand

Ustawia lub pobiera parametry SQL używane podczas usuwania wierszy z bazy danych. Może to być zapytania SQL lub nazwa procedury składowanej.

### <a name="deletecommandtype"></a>DeleteCommandType

Ustawia lub pobiera typ polecenia Usuń to zapytanie SQL (tekst) lub procedury składowanej (procedura składowana).

### <a name="deleteparameters"></a>DeleteParameters

Zwraca parametry, które są używane przez element DeleteCommand obiektu SqlDataSourceView skojarzonego z kontrolką SqlDataSource.

### <a name="oldvaluesparameterformatstring"></a>Elementu OldValuesParameterFormatString

Ta właściwość jest używana do określania formatu oryginalnej wartości parametrów w przypadkach, w których właściwość ConflictDetection ustawiono CompareAllValues. Wartość domyślna to {0} co oznacza, że oryginalne wartości parametrów będzie mieć taką samą nazwę jak oryginalny parametru. Oznacza to, jeśli nazwa pola EmployeeID, oryginalnym parametru wartości będzie @EmployeeID.

### <a name="selectcommand"></a>SelectCommand

Ustawia lub pobiera parametry SQL, który służy do pobierania danych z bazy danych. Może to być zapytania SQL lub nazwa procedury składowanej.

### <a name="selectcommandtype"></a>SelectCommandType

Ustawia lub pobiera typ polecenia select, to zapytanie SQL (tekst) lub procedury składowanej (procedura składowana).

### <a name="selectparameters"></a>SelectParameters

Zwraca parametry, które są używane przez SelectCommand obiektu SqlDataSourceView skojarzonego z kontrolką SqlDataSource.

### <a name="sortparametername"></a>SortParameterName

Pobiera lub ustawia nazwę parametru procedury składowanej, który jest używany, gdy sortowanie danych pobrana przez kontrolę źródła danych. Prawidłowe tylko wtedy, gdy ustawiono SelectCommandType StoredProcedure.

### <a name="sqlcachedependency"></a>SqlCacheDependency

Rozdzielana średnikami lista ciąg określający baz danych i tabelami używanymi w zależności pamięci podręcznej programu SQL Server. (Zależności pamięci podręcznej SQL zostanie omówiony w nowszym module).

### <a name="updatecommand"></a>UpdateCommand

Ustawia lub pobiera parametry SQL, która jest używana podczas aktualizowania danych w bazie danych. Może to być zapytania SQL lub nazwa procedury składowanej.

### <a name="updatecommandtype"></a>UpdateCommandType

Ustawia lub pobiera typ polecenia update albo zapytanie SQL (tekst) lub procedury składowanej (procedura składowana).

### <a name="updateparameters"></a>UpdateParameters

Zwraca parametry, które są używane przez elementu UpdateCommand obiektu SqlDataSourceView skojarzonego z kontrolką SqlDataSource.

## <a name="the-accessdatasource-control"></a>Kontrolka AccessDataSource

Formant AccessDataSource pochodzi od klasy SqlDataSource i służy do tworzenia powiązań danych do bazy danych Microsoft Access. Właściwość ConnectionString kontrolki AccessDataSource jest właściwością tylko do odczytu. Zamiast korzystać z właściwości ConnectionString, właściwość pliku danych jest używana wskaż bazę danych programu Access, jak pokazano poniżej.

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource będzie zawsze równa ProviderName z podstawowej SqlDataSource System.Data.OleDb i nawiązuje połączenie z bazą danych przy użyciu dostawcy Microsoft.Jet.OLEDB.4.0 OLE DB. Nie można użyć formantu AccessDataSource połączyć się z chronionym hasłem bazy danych programu Access. Jeśli masz połączenie z bazą danych chroniony hasłem, należy używać kontrolki SqlDataSource.

> [!NOTE]
> Dostęp do baz danych przechowywanych w ramach witryny sieci Web, należy umieścić w aplikacji\_katalog danych. Program ASP.NET nie zezwala na pliki w tym katalogu do przeglądania. Należy przyznać uprawnienia do odczytu i zapisu do aplikacji konto procesu\_katalog danych w przypadku korzystania z baz danych programu Access.


## <a name="the-xmldatasource-control"></a>Kontrolki elementu XmlDataSource

Elementu XmlDataSource jest używana do wiązania danych dane XML do formantów powiązanych z danymi. Możesz powiązać pliku XML przy użyciu właściwości pliku danych lub można powiązać ciągu XML przy użyciu właściwości danych. Elementu XmlDataSource udostępnia atrybuty XML jako pola może być powiązana. W przypadkach, w których należy powiązać wartości, które nie są reprezentowane jako atrybuty należy używać transformacji XSL. Można również użyć wyrażenia XPath do filtrowania danych XML.

Należy wziąć pod uwagę następujący plik XML:

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

Należy zauważyć, że elementu XmlDataSource używa właściwość XPath *osób/osoby* w celu filtrowania tylko &lt;osoby&gt; węzłów. Metody DropDownList następnie tworzy powiązania danych przy użyciu właściwości DataTextField atrybutu nazwisko.

Podczas kontroli elementu XmlDataSource służy głównie do tworzenia powiązań danych z danymi XML tylko do odczytu, istnieje możliwość edytowania pliku danych XML. Należy pamiętać, że w takich przypadkach automatyczne wstawianie, aktualizowanie i usuwanie informacji w pliku XML nie jest wykonywane automatycznie jak w przypadku innych formantów źródła danych. Zamiast tego należy napisać kod, aby ręcznie edytować dane przy użyciu następujących metod kontroli elementu XmlDataSource.

### <a name="getxmldocument"></a>GetXmlDocument

Pobiera obiekt XmlDocument zawierający kod XML, pobierane przez elementu XmlDataSource.

### <a name="save"></a>Zapisanie

Zapisuje dokument XML w pamięci źródła danych.

Należy koniecznie należy pamiętać, że metody Zapisz działa tylko po spełnieniu następujących warunków:

1. Elementu XmlDataSource używa właściwości pliku danych do powiązania w pliku XML, zamiast właściwości danych do powiązania z danymi XML w pamięci.
2. Przekształcenie nie jest określony za pomocą właściwości transformacji lub TransformFile.

Należy zauważyć, że metody Zapisz może przynieść nieoczekiwane wyniki, gdy zostanie wywołana przez wielu użytkowników jednocześnie.

## <a name="the-objectdatasource-control"></a>Kontrolka ObjectDataSource

Kontrolki źródła danych, gdy Omówiliśmy już do tej pory, które są doskonałe możliwości aplikacji dwuwarstwowej, gdzie kontrola źródła danych komunikuje się bezpośrednio do magazynu danych. Jednak wiele rzeczywistych aplikacji są aplikacje wielowarstwowe gdzie kontrola źródła danych może być konieczne do komunikacji z obiektem biznesowym, który z kolei komunikuje się z warstwą danych. W takich sytuacjach kontrolki ObjectDataSource wypełnia dobrze rachunku. Kontrolki ObjectDataSource działa w połączeniu z obiektem źródłowym. Kontrolka ObjectDataSource spowoduje utworzenie wystąpienia obiektu źródłowego, wywołania określonej metody i usuwania wystąpienia obiektu w zakresie pojedynczego żądania, jeśli obiekt ma metody wystąpienia, zamiast metody statyczne (Shared w języku Visual Basic). W związku z tym obiekt musi być bezstanowe. Oznacza to obiektu, należy uzyskać i zwolnić wszystkie zasoby wymagane w zasięgu pojedynczego żądania. Możesz kontrolować sposób tworzenia obiektu źródłowego dzięki obsłudze zdarzeń ObjectCreating formantu ObjectDataSource. Można utworzyć wystąpienia obiektu źródłowego i następnie ustaw właściwości ObjectInstance klasy ObjectDataSourceEventArgs do tego wystąpienia. Kontrolka ObjectDataSource użyje wystąpienia, który jest tworzony w zdarzeniu ObjectCreating zamiast tworzenia wystąpienia na jego własnej.

Jeśli obiektu źródła kontrolki ObjectDataSource udostępnia publiczne metody statyczne (Shared w języku Visual Basic), które można wywołać w celu pobrania i modyfikowania danych, kontrolka ObjectDataSource będzie bezpośrednio wywoływać tych metod. Jeśli kontrolka ObjectDataSource, należy utworzyć wystąpienie obiektu źródłowego, przed wywołaniem metody, obiekt musi zawierać konstruktor publiczny, który nie przyjmuje żadnych parametrów. Kontrolka ObjectDataSource będzie wywoływać ten konstruktor, podczas tworzenia nowego wystąpienia obiektu źródłowego.

Jeśli obiekt źródłowy nie zawiera on publicznego konstruktora bez parametrów, można utworzyć wystąpienia obiektu źródłowego, który będzie używany przez formantu ObjectDataSource w zdarzeniu ObjectCreating.

## <a name="specifying-object-methods"></a>Określenie metod obiektu

Obiektu źródła kontrolki ObjectDataSource może zawierać dowolną liczbę metod, które są używane do wybierz oraz wstawiania, aktualizowania lub usuwania danych. Te metody są wywoływane przez kontrolka ObjectDataSource, w oparciu o nazwę metody, określone za pomocą metody SelectMethod, operacji InsertMethod, operacji UpdateMethod albo DeleteMethod właściwości formantu ObjectDataSource. Obiekt źródłowy można również dołączyć opcjonalny metody SelectCount, który jest identyfikowany przez kontrolka ObjectDataSource, za pomocą właściwości SelectCountMethod, która zwraca liczbę całkowita liczba obiektów w źródle danych. Kontrolka ObjectDataSource będzie wywoływać metoda SelectCount po wywołaniu metody Select można pobrać całkowitej liczbie rekordów w źródle danych do użycia podczas stronicowania.

## <a name="lab-using-data-source-controls"></a>Laboratorium przy użyciu kontrolki źródła danych

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>Ćwiczenie 1 — wyświetlanie danych przy użyciu kontrolki SqlDataSource

Poniższym ćwiczeniu używa kontrolki SqlDataSource do łączenia z bazą danych Northwind. Przyjęto założenie, że masz dostęp do bazy danych Northwind w wystąpieniu programu SQL Server 2000.

1. Utwórz nową witrynę sieci Web platformy ASP.NET.
2. Dodaj nowy plik web.config.

    1. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie kliknij przycisk Dodaj nowy element.
    2. Wybierz plik konfiguracji sieci Web z listy szablonów, a następnie kliknij przycisk Dodaj.
3. Edytuj &lt;connectionStrings&gt; sekcji w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. Przełącz do widoku kodu, a następnie dodaj atrybut ConnectionString i atrybutu SelectCommand &lt;asp: SqlDataSource&gt; kontrolować w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. W widoku Projekt dodać nowej kontrolki GridView.
6. Z listy rozwijanej wybierz źródło danych w menu zadania GridView wybierz SqlDataSource1.
7. Kliknij prawym przyciskiem myszy na Default.aspx i wybierz polecenie Wyświetl w przeglądarce z menu. Kliknij przycisk Tak, po wyświetleniu monitu, aby zapisać.
8. Kontrolki GridView wyświetla dane z tabeli Produkty.

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>Ćwiczenie 2 - edytowania danych przy użyciu kontrolki SqlDataSource

Poniższym ćwiczeniu pokazano, jak utworzyć powiązania danych kontrolki DropDownList sterować za pomocą składni deklaratywnej i służy do edytowania danych prezentowanych w kontrolki DropDownList.

1. W widoku Projekt należy usunąć kontrolki GridView z Default.aspx. 

    **Ważne**: pozostaw kontrolki SqlDataSource na stronie.
2. Dodaj formant DropDownList do Default.aspx.
3. Przełącz do widoku źródła.
4. Dodaj atrybut DataSourceId, DataTextField i DataValueField, aby &lt;asp: DropDownList&gt; kontrolować w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. Zapisz Default.aspx i wyświetlić je w przeglądarce. Należy pamiętać, że metody DropDownList zawiera wszystkie produkty z bazy danych Northwind.
6. Zamknij przeglądarkę.
7. W widoku źródła Default.aspx należy dodać nowy formant pola tekstowego poniżej kontrolki DropDownList. Zmień wartość właściwości identyfikator pola tekstowego do txtProductName.
8. W obszarze formant pola tekstowego należy dodać nowy formant przycisku. Zmień właściwość ID przycisku btnUpdate i właściwość tekst **nazwę produktu aktualizacji**.
9. W widoku źródła Default.aspx Dodaj właściwość elementu UpdateCommand i dwa nowe UpdateParameters tagu SqlDataSource w następujący sposób: 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > Należy pamiętać, że istnieją dwa parametry (ProductName i identyfikator produktu), dodane w tym kodzie aktualizacji. Te parametry są zamapowane na właściwości Text txtProductName pole tekstowe i właściwości SelectedValue ddlProducts DropDownList.
10. Przełącz do widoku projektu, a następnie kliknij dwukrotnie formant przycisku aby dodać program obsługi zdarzeń.
11. Dodaj następujący kod do btnUpdate\_kliknij kodu: 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. Kliknij prawym przyciskiem myszy na Default.aspx i wybierz, aby wyświetlić go w przeglądarce. Po wyświetleniu monitu, aby zapisać wszystkie zmiany, kliknij przycisk Tak.
13. Program ASP.NET 2.0 klasy częściowe umożliwiają kompilacji w czasie wykonywania. Nie jest niezbędne do tworzenia aplikacji, aby zobaczyć zmiany kodu zaczęły obowiązywać.
14. Wybierz produkt z metody DropDownList.
15. Wprowadź nową nazwę dla wybranego produktu w polu tekstowym, a następnie kliknij przycisk Aktualizuj.
16. Nazwa produktu jest aktualizowany w bazie danych.

## <a name="exercise-3-using-the-objectdatasource-control"></a>Ćwiczenie 3 za pomocą kontrolki ObjectDataSource

To ćwiczenie pokażemy, jak na potrzeby interakcji z bazą danych Northwind i obiektu źródłowego kontrolki ObjectDataSource.

1. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie kliknij pozycję Dodaj nowy element.
2. Wybierz formularz sieci Web z listy szablonów. Zmień nazwę na object.aspx, a następnie kliknij przycisk Dodaj.
3. Kliknij prawym przyciskiem myszy projekt w Eksploratorze rozwiązań, a następnie kliknij pozycję Dodaj nowy element.
4. Wybierz klasę z listy szablonów. Zmień nazwę klasy na NorthwindData.cs, a następnie kliknij przycisk Dodaj.
5. Kliknij przycisk Tak, po wyświetleniu monitu, aby dodać klasę do aplikacji\_katalogu z kodem.
6. Dodaj następujący kod do pliku NorthwindData.cs: 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Dodaj następujący kod do widoku źródła object.aspx: 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. Zapisz wszystkie pliki i przeglądać object.aspx.
9. Wyświetlanie szczegółów, edytowania pracowników, dodawanie pracowników i usuwając pracowników wchodzić w interakcje z interfejsem.
