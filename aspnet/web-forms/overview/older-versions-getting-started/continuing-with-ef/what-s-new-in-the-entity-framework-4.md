---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
title: Co to jest nowa w programie Entity Framework 4.0 | Dokumentacja firmy Microsoft
author: tdykstra
description: Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez wprowadzenie do samouczka serii Entity Framework 4.0. I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 393df4a8-b1db-44c4-9db7-2b533ca887d0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/what-s-new-in-the-entity-framework-4
msc.type: authoredcontent
ms.openlocfilehash: 04444ce98fa60045cf617a6c518dd55677258148
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 04/06/2018
---
<a name="whats-new-in-the-entity-framework-40"></a>Co to jest nowa w programie Entity Framework 4.0
====================
Przez [Dykstra niestandardowy](https://github.com/tdykstra)

> Ten samouczek serii opiera się na aplikację sieci web Contoso University jest tworzony przez [wprowadzenie do korzystania z programu Entity Framework](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md) samouczka serii. Jeśli nie została ukończona wcześniejszych samouczki, jako punkt początkowy dla tego samouczka możesz [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a) będzie utworzony. Możesz również [pobrać aplikację](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa) tworzone przez zakończenie samouczka serii. Jeśli masz pytania dotyczące samouczków, możesz zamieścić je do [forum ASP.NET Entity Framework](https://forums.asp.net/1227.aspx).


W poprzednich instrukcji widać niektóre metody maksymalizacja wydajności aplikacji sieci web, który używa programu Entity Framework. W tym samouczku przegląda niektóre z najważniejszych nowych funkcji programu Entity Framework w wersji 4, a następnie łączy się z zasobami, które zapewniają pełniejsze wprowadzenie do wszystkich nowych funkcji. Funkcje wyróżniane w tym samouczku są następujące:

- Klucz obcy skojarzenia.
- Wykonywanie polecenia SQL zdefiniowane przez użytkownika.
- Programowanie pierwszego modelu.
- Obsługa POCO.

Ponadto krótko wprowadzi samouczka *pierwszy kod programowanie*, funkcja, która będzie dostępna w następnej wersji programu Entity Framework.

Aby uruchomić samouczek, uruchom program Visual Studio i Otwórz aplikację sieci web Contoso University pracy z poprzednich samouczka.

## <a name="foreign-key-associations"></a>Skojarzenia klucza obcego

Właściwości nawigacji uwzględnione w wersji 3.5 programu Entity Framework, ale go nie włączono właściwości klucza obcego do modelu danych. Na przykład `CourseID` i `StudentID` kolumn `StudentGrade` tabeli zostaną pominięte z `StudentGrade` jednostki.

[![Image01](what-s-new-in-the-entity-framework-4/_static/image2.png)](what-s-new-in-the-entity-framework-4/_static/image1.png)

Przyczyna tego podejścia został, mówiąc ściślej, kluczy obcych są szczegóły implementacji fizyczny i nie powinny znajdować się w modelu koncepcyjnym danych. Jednak to w praktyce, jest często łatwiejsze do pracy z jednostek kodu, gdy ma bezpośredni dostęp do kluczy obcych.

Na przykład klucze obce jak w modelu danych, można uprościć kod, należy rozważyć sposób trzeba było do kodu *DepartmentsAdd.aspx* strony bez nich. W `Department` jednostki, `Administrator` właściwość jest kluczem obcym, który odpowiada `PersonID` w `Person` jednostki. W celu określenia skojarzenia między nowy dział a jego administratora, wszystkie musiały czy ustawiono wartość `Administrator` właściwości w `ItemInserting` obsługi zdarzeń formantu z danymi:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample1.cs)]

Bez klucze obce w modelu danych, może obsługiwać `Inserting` zdarzeń formantu źródła danych, zamiast `ItemInserting` zdarzeń formantu z danymi, aby pobrać odwołanie do samej jednostki przed dodaniem jednostki do zestawu jednostek. Jeśli masz tego odwołania, możesz udostępnić skojarzenia przy użyciu kodu, tak jak w poniższych przykładach:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample2.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample3.cs)]

Jak widać w zespole Entity Framework [wpis w blogu na skojarzenia klucza obcego](https://blogs.msdn.com/b/efdesign/archive/2009/03/16/foreign-keys-in-the-entity-framework.aspx), istnieją innych przypadkach, gdy różnica w złożoność kodu jest znacznie większa. Na potrzeby tych, którzy wolą współdziałać z szczegóły implementacji w modelu koncepcyjnego danych ze względu na kod prostsze, Entity Framework teraz istnieje możliwość, w tym klucze obce w modelu danych.

W terminologii programu Entity Framework, jeśli zawiera klucze obce w modelu danych używasz *skojarzenia klucza obcego*, i jeśli wykluczone kluczy obcych używasz *skojarzenia niezależne*.

## <a name="executing-user-defined-sql-commands"></a>Wykonywanie polecenia SQL zdefiniowane przez użytkownika

We wcześniejszych wersjach programu Entity Framework nie było łatwy sposób tworzyć własne polecenia SQL na bieżąco i uruchom je. Entity Framework dynamicznie wygenerowana poleceń SQL albo musiał utworzyć procedury składowanej i zaimportować go jako funkcję. Dodaje w wersji 4 `ExecuteStoreQuery` i `ExecuteStoreCommand` metody `ObjectContext` klasy, która ułatwić przekazać każde zapytanie bezpośrednio do bazy danych.

Załóżmy, że pracowników firmy Contoso administracyjnych chcesz mieć możliwość wykonania zbiorcze zmiany w bazie danych bez konieczności przechodzenia przez proces tworzenia procedury składowanej i importowania go do modelu danych. Jest ich pierwsze żądanie strony, które umożliwi zmianę liczby środków dla wszystkich kursów w bazie danych. Na stronie sieci web chcą można było wprowadzić numer na potrzeby pomnożyć wartość każdego `Course` wiersza `Credits` kolumny.

Tworzenie nowej strony, która używa *Site.Master* strony wzorcowej i nadaj mu nazwę *UpdateCredits.aspx*. Następnie dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample4.aspx)]

Tworzy tego znacznika `TextBox` kontrolki, w którym użytkownik może wprowadzić wartość współczynnika `Button` sterowania kliknij, aby wykonać polecenie, a `Label` kontroli dla wskazującą liczbę uwzględnionych wierszy.

Otwórz *UpdateCredits.aspx.cs*i dodaj następującą `using` instrukcji i obsługi dla przycisku `Click` zdarzeń:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample5.cs)]

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample6.cs)]

Ten kod wykonuje SQL `Update` polecenia przy użyciu wartości w polu tekstowym i używa etykiety, aby wyświetlić liczbę uwzględnionych wierszy. Przed uruchomieniem strony uruchomić *Courses.aspx* stronę, aby wyświetlić obraz "przed" niektórych danych.

[![Image02](what-s-new-in-the-entity-framework-4/_static/image4.png)](what-s-new-in-the-entity-framework-4/_static/image3.png)

Uruchom *UpdateCredits.aspx*wprowadź "10" jako mnożnik, a następnie kliknij przycisk **Execute**.

[![Image03](what-s-new-in-the-entity-framework-4/_static/image6.png)](what-s-new-in-the-entity-framework-4/_static/image5.png)

Uruchom *Courses.aspx* stronę ponownie, aby zobaczyć zmienione dane.

[![Image04](what-s-new-in-the-entity-framework-4/_static/image8.png)](what-s-new-in-the-entity-framework-4/_static/image7.png)

(Aby ustalić środki na korzystanie z powrotem do ich oryginalnych wartości w *UpdateCredits.aspx.cs* zmienić `Credits * {0}` do `Credits / {0}` i uruchom ponownie strony wprowadzenia 10 jako dzielnik.)

Aby uzyskać więcej informacji na temat wykonywanie zapytań, które definiują w kodzie, zobacz [porady: bezpośrednio wykonania polecenia względem źródła danych](https://msdn.microsoft.com/library/ee358769.aspx).

## <a name="model-first-development"></a>Pierwszy model programowania

W te wskazówki najpierw utworzyć bazy danych, a następnie wygenerować model danych, na podstawie struktury bazy danych. W programu Entity Framework 4 można zamiast tego uruchomić z modelem danych i Generuj bazę danych, na podstawie struktury modelu danych. Jeśli tworzysz aplikację, dla którego baza danych już nie istnieje, podejście pierwszy model umożliwia tworzenie jednostki i relacje odpowiednich koncepcyjnie dla aplikacji, podczas nie martwiąc się o szczegóły implementacji fizycznych . (To jest wartość true tylko za pośrednictwem początkowych etapów rozwoju, jednak. Po pewnym czasie baza danych zostanie utworzone i będzie mieć danych produkcyjnych, w którym i odtworzenie ją z modelu nie będzie już praktyczne; w tym momencie będziesz mieć do podejście pierwszej bazy danych.)

W tej części samouczka możesz utworzyć model danych proste i wygenerować bazę danych z niego.

W **Eksploratora rozwiązań**, kliknij prawym przyciskiem myszy *DAL* i wybierz polecenie **Dodaj nowy element**. W **Dodaj nowy element** okna dialogowego, w obszarze **zainstalowane szablony** wybierz **danych** , a następnie wybierz **modelu danych jednostki ADO.NET** szablonu . Nazwa nowego pliku *AlumniAssociationModel.edmx* i kliknij przycisk **Dodaj**.

[![Image06](what-s-new-in-the-entity-framework-4/_static/image10.png)](what-s-new-in-the-entity-framework-4/_static/image9.png)

Spowoduje to uruchomienie Kreatora modelu danych jednostki. W **wybierz zawartość modelu** krok, wybierz opcję **pusty Model** , a następnie kliknij przycisk **Zakończ**.

[![Image07](what-s-new-in-the-entity-framework-4/_static/image12.png)](what-s-new-in-the-entity-framework-4/_static/image11.png)

**Projektant modelu danych jednostki** otwiera z powierzchnią pustego projektu. Przeciągnij **jednostki** elementu z **przybornika** na powierzchnię projektu.

[![Image08](what-s-new-in-the-entity-framework-4/_static/image14.png)](what-s-new-in-the-entity-framework-4/_static/image13.png)

Zmień nazwę jednostki z `Entity1` do `Alumnus`, zmień `Id` nazwę właściwości, aby `AlumnusId`i Dodaj nową właściwość skalarną o nazwie `Name`. Aby dodać nowe właściwości naciśnięciu klawisza Enter po zmianie nazwy `Id` kolumn, lub kliknij obiekt prawym przyciskiem myszy i wybierz polecenie **Dodaj właściwość skalarną**. Domyślny typ dla nowych właściwości jest `String`, który działa poprawnie dla tej prezentacji prostego, ale oczywiście można zmienić takie informacje jak typ danych w **właściwości** okna.

Utwórz inną jednostkę taki sam sposób i nadaj mu nazwę `Donation`. Zmień `Id` właściwości `DonationId` i Dodaj właściwość skalarna o nazwie `DateAndAmount`.

[![Image09](what-s-new-in-the-entity-framework-4/_static/image16.png)](what-s-new-in-the-entity-framework-4/_static/image15.png)

Aby dodać skojarzenia między tymi dwoma obiektami, kliknij prawym przyciskiem myszy `Alumnus` jednostki, wybierz opcję **Dodaj**, a następnie wybierz **skojarzenia**.

[![Image10](what-s-new-in-the-entity-framework-4/_static/image18.png)](what-s-new-in-the-entity-framework-4/_static/image17.png)

Wartości domyślne w **Dodawanie skojarzenia** okno dialogowe są, co ma (jeden do wielu, zawierać właściwości nawigacji, zawierają kluczy obcych), więc wystarczy kliknąć **OK**.

[![Image11](what-s-new-in-the-entity-framework-4/_static/image20.png)](what-s-new-in-the-entity-framework-4/_static/image19.png)

Projektant dodaje linia asocjacji i właściwości klucza obcego.

[![Image12](what-s-new-in-the-entity-framework-4/_static/image22.png)](what-s-new-in-the-entity-framework-4/_static/image21.png)

Teraz możesz przystąpić do tworzenia bazy danych. Kliknij prawym przyciskiem myszy projekt powierzchni i wybierz pozycję **generowania bazy danych z modelu**.

[![Image13](what-s-new-in-the-entity-framework-4/_static/image24.png)](what-s-new-in-the-entity-framework-4/_static/image23.png)

Spowoduje to uruchomienie Kreatora generowania bazy danych. (Jeśli wyświetlane ostrzeżenia, które wskazują, że jednostek nie są zamapowane, możesz zignorować te obecnie.)

W **wybierz połączenie danych** kroku, kliknij przycisk **nowe połączenie**.

[![Image14](what-s-new-in-the-entity-framework-4/_static/image26.png)](what-s-new-in-the-entity-framework-4/_static/image25.png)

W **właściwości połączenia** okna dialogowego polu Wybierz lokalne wystąpienie programu SQL Server Express i nazwa bazy danych `AlumniAsssociation`.

[![Image15](what-s-new-in-the-entity-framework-4/_static/image28.png)](what-s-new-in-the-entity-framework-4/_static/image27.png)

Kliknij przycisk **tak** gdy pojawi się monit, aby utworzyć bazę danych. Gdy **wybierz połączenie danych** kroku zostanie wyświetlony ponownie, kliknij przycisk **dalej**.

W **Podsumowanie i ustawienia** kroku, kliknij przycisk **Zakończ**.

[![Image18](what-s-new-in-the-entity-framework-4/_static/image30.png)](what-s-new-in-the-entity-framework-4/_static/image29.png)

A *SQL* plików za pomocą poleceń (DDL) języka definicji danych zostało utworzone, ale polecenia nie zostało jeszcze uruchomione.

[![Image20](what-s-new-in-the-entity-framework-4/_static/image32.png)](what-s-new-in-the-entity-framework-4/_static/image31.png)

Za pomocą narzędzia, takie jak **programu SQL Server Management Studio** uruchomić skrypt i utworzyć tabele, jak mogą została utworzona podczas tworzenia `School` bazy danych dla [pierwszy samouczek wprowadzenie serii samouczka ](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md). (Jeśli nie można pobrać bazy danych.)

Można teraz używać `AlumniAssociation` modelu danych w sieci web pages taki sam sposób, w obecnie używasz `School` modelu. Aby wypróbować tę możliwość, należy dodać niektóre dane do tabel i utworzyć stronę sieci web, który wyświetla dane.

Przy użyciu **Eksploratora serwera**, Dodaj następujące wiersze do `Alumnus` i `Donation` tabel.

[![Image21](what-s-new-in-the-entity-framework-4/_static/image34.png)](what-s-new-in-the-entity-framework-4/_static/image33.png)

Tworzenie nowej strony sieci web o nazwie *Alumni.aspx* używającą *Site.Master* strony wzorcowej. Dodaj następujący kod do `Content` formantu o nazwie `Content2`:

[!code-aspx[Main](what-s-new-in-the-entity-framework-4/samples/sample7.aspx)]

Ten kod znaczników tworzy zagnieżdżonych `GridView` kontroluje, zewnętrzna, do wyświetlenia absolwentów nazwy i wewnętrznej, do wyświetlania dat pobrania i ilości.

Otwórz *Alumni.aspx.cs*. Dodaj `using` instrukcja dla danych dostęp warstwy i obsługę zewnętrznego `GridView` formantu `RowDataBound` zdarzeń:

[!code-csharp[Main](what-s-new-in-the-entity-framework-4/samples/sample8.cs)]

Ten kod databinds wewnętrzny `GridView` kontrolować za pomocą `Donations` właściwość nawigacji bieżącego wiersza `Alumnus` jednostki.

Uruchom strony.

[![Image22](what-s-new-in-the-entity-framework-4/_static/image36.png)](what-s-new-in-the-entity-framework-4/_static/image35.png)

(Uwaga: Ta strona jest dołączony do projektu do pobrania, ale aby działała, musisz utworzyć bazy danych w lokalnym wystąpieniu programu SQL Server Express; bazy danych nie jest uwzględniana jako *.mdf* w pliku *aplikacji\_ Dane* folderu.)

Aby uzyskać więcej informacji dotyczących używania funkcji pierwszego modelu programu Entity Framework, zobacz [pierwszego modelu w Entity Framework 4](https://msdn.microsoft.com/data/ff830362.aspx).

## <a name="poco-support"></a>Obsługa obiektów POCO

Korzystając z metody opartej na domenie projektu, projektowanie klas danych, które reprezentują danych i zachowania, które jest odpowiednie do domeny biznesowych. Te klasy powinny być niezależne od wszelkich określonych technologii używany do przechowywania (utrwalić) danych; innymi słowy, należy je *ignorujących trwałość*. Nieznajomości trwałości może również ułatwić klasy testu jednostkowego ponieważ jednostkowy projekt testowy mogą używać niezależnie od technologii trwałości jest najbardziej odpowiednim testowania. Wcześniejszych wersji programu Entity Framework oferowane ograniczoną obsługę nieznajomości trwałości, ponieważ dziedziczenie z klas jednostek `EntityObject` klasy i w związku z tym dołączony wiele funkcji specyficznych dla Entity Framework.

Entity Framework 4 wprowadzono możliwość używania klas jednostek, które nie dziedziczy z `EntityObject` klasy i w związku z tym są ignorujących trwałość. W kontekście programu Entity Framework, klas takich jak to są zwykle nazywane *obiektów CLR starego zwykłego* (POCO lub POCOs). Klasy POCO może zapisywać ręcznie lub może automatycznie generować je na podstawie istniejącego modelu danych, za pomocą szablonów Toolkit transformacji szablonu tekstowego (T4) podane przez program Entity Framework.

Aby uzyskać więcej informacji o używaniu POCOs w programie Entity Framework Zobacz następujące zasoby:

- [Praca z jednostki POCO](https://msdn.microsoft.com/library/dd456853.aspx). To jest dokument MSDN, który znajduje się przegląd POCOs, wraz z łączami do inne dokumenty, które zawierają bardziej szczegółowe informacje.
- [Wskazówki: Obiektów POCO szablonu Entity Framework](https://blogs.msdn.com/b/adonet/archive/2010/01/25/walkthrough-poco-template-for-the-entity-framework.aspx) to w blogu z programu Entity Framework zespół deweloperów, wraz z łączami do innych blogach o POCOs.

## <a name="code-first-development"></a>Programowanie pierwszy kodu

Obsługa POCO w Entity Framework 4 wymaga nadal tworzenia modelu danych, a następnie połączyć z klasami jednostki do modelu danych. Kolejnej wersji programu Entity Framework obejmuje funkcję *pierwszy kod programowanie*. Ta funkcja umożliwia za pomocą programu Entity Framework własnych klas POCO bez konieczności używania Projektant modelu danych lub plik XML modelu danych. (W związku z tym ta opcja również została wywołana *tylko kod*; *pierwszy kod* i *tylko kod* odnoszą się do tej samej funkcji programu Entity Framework.)

Aby uzyskać więcej informacji o korzystaniu z podejścia pierwszy kod do rozwoju, zobacz następujące zasoby:

- [Pierwszy kod programowanie z programu Entity Framework 4](https://weblogs.asp.net/scottgu/archive/2010/07/16/code-first-development-with-entity-framework-4.aspx). Jest to w blogu przez Scott Guthrie introducing programowanie pierwszy kod.
- [Posty na blogu zespołu programu Entity Framework programowanie — oznakowany CodeOnly](https://blogs.msdn.com/b/efdesign/archive/tags/codeonly/)
- [Blog zespołu Entity Framework programowanie — zapisuje oznakowanych Code First](https://blogs.msdn.com/b/efdesign/archive/tags/code+first/)
- [Samouczek magazynu utworów muzycznych MVC — część 4: modeli i dostęp do danych](../../../../mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4.md)
- [Wprowadzenie do platformy MVC 3 - część 4: Entity Framework pierwszy kod programowanie](../../../../mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-a-model.md)

Ponadto nowe samouczek pierwszego kodu MVC, który tworzy podobny do aplikacji Contoso University aplikacji jest zaprojektowana do opublikowania w źródła 2011 w [https://asp.net/entity-framework/tutorials](../../../../entity-framework.md)

## <a name="more-information"></a>Więcej informacji

Na tym kończy się przegląd, aby what's new in programu Entity Framework i ten proces jest kontynuowany z serii samouczka programu Entity Framework. Aby uzyskać więcej informacji na temat nowych funkcji programu Entity Framework 4, które nie są zawarte w tym miejscu zobacz następujące zasoby:

- [What's New in ADO.NET](https://msdn.microsoft.com/library/ex6y04yf.aspx) MSDN temat nowych funkcji programu Entity Framework w wersji 4.
- [Informuje o wersji programu Entity Framework 4](https://blogs.msdn.com/b/efdesign/archive/2010/04/12/announcing-the-release-of-entity-framework-4.aspx) zespół deweloperów program Entity Framework wpis w blogu nowe funkcje w wersji 4.

> [!div class="step-by-step"]
> [Poprzednie](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
